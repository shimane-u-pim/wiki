# PostgreSQLのバックアップ（`pg_dump`）

> [!NOTE]
> この記事は執筆途中です。

> [!NOTE]
> この記事は`pg_dump`のすべてを網羅した情報を記入する物ではなく、部内システムで利用されている用途のドキュメンテーションを目的とした内容になっています。

## 部内で使われている構文

```bash
pg_dump -U db_user -F p db_name > db_dump_file.sql
```

`-U`でデータベースの処理ユーザを指定し、`-F`でフォーマットをplain text (`p`に)指定しています。
`db_name`にはバックアップを取りたいデータベースを指定します。

この構文は実際にはDocker Composeを利用し実行されます。
例えば、Mattermostの場合、以下のような構文になります。

```bash
docker-compose exec -T postgres pg_dump -U mattermost_db_user -F p mattermost_db > db_dump_file.sql
```

`docker-compose`では、`exec`でコンテナ内でコマンドを実行できます。
Crontab等で実行できるようにするため、`-T`を指定し、tty（標準入力など）を利用しないようにしています。

Docker ComposeのExecコマンドの処理は下記の通り行われます。

1. Bashが実行する範囲を指定
    1. この場合は、`docker-compose exec -T postgres pg_dump -U mattermost_db_user -F p mattermost_db`をひとまず実行
2. `docker-compose exec`コマンドで識別できないオプション以降を抽出
    1. この場合は、`pg_dump`以降
    2. `docker-compose exec -T postgres`まではDocker Compose側で処理される
3. 抽出したコマンドを`postgres`コンテナで実行
    1. `docker-compose.yaml`の`service`で指定されたコンテナ名と紐づけられる
4. `pg_dump`コマンドが引数を処理し、バックアップを作成
    1. この場合は、`-U mattermost_db_user -F p mattermost_db`が引数となる
    2. [重要] 出力は標準出力（一般的なターミナル）に出力される
5. 出力をファイルにリダイレクト
    1. `>`で標準出力をファイルにリダイレクトする
    2. この場合は、`db_dump_file.sql`にリダイレクトされる
    3. `>`が1つのみなので、ファイルが既に存在した場合は上書きされる（前のバックアップが残らない原因になる）

DockerイメージのPostgreSQLの場合、`env`で指定したユーザ名を`-U`で指定することで、パスワード入力が必要なくなります。

### Cronを使った自動バックアップの仕組み

Cronを使った自動バックアップの仕組みは、上記の構文をCronに登録することで実現できます。

Pim内部では次のような構文を利用しています。

> [!IMPORTANT]
> ここで紹介されているcrontabは実環境を確認しておらず、机上の空論である可能性があります。

```crontab
32 12 * * * cd /path/do/docker/compose/dir; docker-compose exec -T postgres pg_dump -U mattermost_db_user -F p mattermost_db | tee /path/to/db/backup/dir/$(date -d"-0 days" +%Y%m%d).sql | head
```

ここで`tee`と`head`を使っています（これらはStep 1の段階でBashに後で実行になるため、docker内で実行されず、結果を元にホストで実行されます）

なぜ`tee`と`head`を使うかというと、`pg_dump`は標準出力に出力するため、そのままリダイレクトすると、結果を確認できません。

正常にバックアップが取れていることを確認するため、最初の10行を出力してメールでレポートを送らせ、エラーとなっていないことを確認します。
`pg_dump`でMattermostのDBを落とした場合、最初の10行にセキュアな情報は入らないため、このような手法を取れます。

## DB Connection StringからバックアップすべきDBとそのユーザを取得する

`pg_dump`を実行するためには、DBのユーザ名とDB名・パスワードが必要です。

Ubuntu等に直接インストールされたPostgreSQL等であれば、
`sudo -u postgres`でコマンドを始めることで、
`postgres`ユーザで特権的にアクセスするため、DB名のみが必要な状態になりますが、
特権的に接続できないDBで、接続情報が分からない・新たに払い出されない場合、
よく使われるConnection Stringから情報を取得する事ができます。

> [!TIP]
> `sudo -u postgres`を利用する場合、この構文は`docker-compose exec`と同じ意味を持ちます。
>
> `sudo -u postgres pg_dump -F p mattermost_db > db_dump.sql`を実行した場合、
> `pg_dump -F p mattermost_db`を`sudo`コマンドが`postgres`ユーザ（PostgreSQLに対しての事実上の特権ユーザ）として実行し、
> その結果を`> db_dump.sql`の部分で、ファイルにリダイレクトするような命令になります。

以下は、Mattermostで使われているConnection Stringです。

```url
postgres://mm_db_user:mm_db_password@db_host/db_name?sslmode=disable\u0026connect_timeout=10\u0026binary_parameters=yes
```

Connection Stringは多くの場合、`postgres://`から始まります。
これは、Connection Stringの考え方が、URLを同じであるからです。

認証情報は`HTTP`や`HTTPS`で`BASIC`を利用する際に指定する値と同じような指定をします。

`@`で認証情報とホスト名を分け、`:`でユーザ名とパスワードを分けます。
そのため、`postgres://`と`@`までの間がユーザ名とパスワードを保持しています。
`:`で別けるため、`mm_db_user`がユーザであり、`mm_db_password`がパスワードです。

ホスト名は`/`までの文字列になります。
この場合、`db_host`がホスト名になります。

> [!TIP]
> Docker Composeの場合、Service名をホスト名として利用できます。

`/`以降の文字列はDB名になります。
多くの場合、ここで終わりますが、その後、`?`から始まるクエリパラメータが続く場合もあります。
例の場合はクエリパラメータが含まれているため、`db_name`までをDB名として利用します。

よって、上記のConnection Stringからは、次の情報が取得できます。

- ユーザ名: `mm_db_user`
- パスワード: `mm_db_password`
- ホスト名: `db_host`
- DB名: `db_name`

## 外部リンク

- [PostgreSQL: Documentation: 16: pg_dump](https://www.postgresql.org/docs/16/app-pgdump.html)
- [pg_dump (PostgreSQL 15, 日本PostgreSQLユーザ会)](https://www.postgresql.jp/document/15/html/app-pgdump.html)
- [Install and configure PostgreSQL | Ubuntu](https://ubuntu.com/server/docs/databases-postgresql)
