# Wikiの記述環境の作成

このWikiは、誰でも編集できるWikiです。このWikiの書き方について説明します。

> [!NOTE]
> 本記事は不完全な物です。

## 記述環境の作成

- [GitHub](https://github.com/)のアカウントを作成してください。
- Python 3.10以上をインストールしてください。
- Gitをインストールしてください。

## wikiのフォーク

このリポジトリをフォークしてください。

https://github.com/shimane-u-pim/wiki
の画面右上にある[Forkボタン](https://github.com/shimane-u-pim/wiki/fork)からフォークできます。

## もし久しぶりに編集する場合

フォークの更新を行ってください。

> [!NOTE]
> もしあなたがリポジトリ名を変更している場合、以下のURLの`wiki`の部分を変更したリポジトリ名に変更してください。

https://github.com/<あなたのユーザ名>/wiki
の画面に`Sync Fork`ボタンがある場合、それを押し、`Update branch`を押してください。

もし`Update branch`ボタンがない場合、更新はありません。

## ローカル環境の作成

フォークしたリポジトリをローカルにクローンしてください。

```bash
git clone https://github.com/<あなたのユーザ名>/wiki.git pim-wiki
```

`pim-wiki`ディレクトリが作成されます。
これから先はこのディレクトリ内で作業します。

## ローカル環境の更新

ローカル環境を更新するためには、`pim-wiki`ディレクトリ内で、
以下のコマンドを実行してください。

```bash
git checkout master
git pull
```

## 作業環境の構築

この動作は`git clone`後の最初の1回のみ必要です。
`pim-wiki`ディレクトリ内で、
以下のコマンドを実行してください。

```cmd
python -m venv .venv
```

## 作業環境の有効化

この作業は毎回必要です。

`pim-wiki`ディレクトリ内で、
以下のコマンドを実行してください。

```cmd
.\.venv\Scripts\activate.bat
```

画面に`.venv`と出れば成功です。

もし、画面左に`PS `から始まる行がある場合、上記コマンドではなく、
以下のコマンドを実行してください。

```powershell
Set-ExecutePolicy Unrestricted -Scope Process
.\.venv\Scripts\Activate.ps1
```

画面に緑色の`(.venv)`が出れば成功です。

## 作業環境の更新

`git pull`を行った後は必ず以下のコマンドを実行してください。
最初の1回目でも必須です。

```cmd
pip install -r requirements.txt
```

## 変更用のブランチの作成

`pim-wiki`ディレクトリ内で、
以下のコマンドを実行してください。

```cmd
git checkout -b <あなたのブランチ名>
```

`<あなたのブランチ名>`は、あなたの作業内容に応じて適切な名前にしてください。

## ファイルの編集

適切にファイルの編集・追加・削除を行ってください。

書式については別の記事を参考にしてください。

## 変更点のステージング

`pim-wiki`ディレクトリ内で、
以下のコマンドを実行してください。

```cmd
git add -A
```

## 変更点のコミット

`pim-wiki`ディレクトリ内で、
以下のコマンドを実行してください。

```cmd
git commit -m "変更内容"
```

`"変更内容"`は、あなたの変更内容に応じて適切な内容にしてください。

> [!IMPORTANT]
> 最初のコミット前に、GitHubドキュメントの
> [Git でのユーザ名を設定する](https://docs.github.com/ja/get-started/getting-started-with-git/setting-your-username-in-git)と、
> [コミットメールアドレスを設定する](https://docs.github.com/ja/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences/setting-your-commit-email-address)
> を実施してください。

## 動作確認

`pim-wiki`ディレクトリ内で、
以下のコマンドを実行してください。

```cmd
mkdocs build
```

正常にコマンドが終了し、エラーが出力されなければ正常です。

```text
Aborted with 2 warnings in strict mode!
```

等が出力された場合、エラーが出ている部分を修正し、
[#変更点のステージング](#変更点のステージング)からやり直してください。

## 変更点のプッシュ

`pim-wiki`ディレクトリ内で、
以下のコマンドを実行してください。

```cmd
git push origin <あなたのブランチ名> -u
```

`<あなたのブランチ名>`は、[#変更用のブランチの作成](#変更用のブランチの作成)で作成したブランチ名です。

## プルリクエストの作成

GitHubのあなたのFork画面で、プルリクエストを作成してください。

後はこちら側で確認し、マージします。

## 作業用ブランチの終了

作業が終了したら、
`pim-wiki`ディレクトリで、
以下のコマンドを実行してください。

```cmd
git checkout master
```
