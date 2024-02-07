# Docker上におけるMattermost運用のすべて

> [!NOTE]
> この記事は執筆途中のものです。

前提:

- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)
- [Ubuntu](https://ubuntu.com/) 22.04 LTS

## Ubuntuについて

Ubuntuは最も簡単にLinuxを使えるディストリビューションである。

Ubuntuは多くの企業や個人が使っており、情報が多く、開発元のCanonical社が情報を多く出しているため、
データが多く存在する。
情報量が多いのは何かと都合が良いので、今回のホストマシンとして利用している。

上級者向けTips: UbuntuはDebianの派生形であり、Debianの情報もある程度転用場合が多い。

### Ubuntuのバージョニングについて

Ubuntuのバージョンは`yy.mm.vv`で表記される。`yy`と`mm`はリリースされた年と日となり、
基本は各年の4月と10月にリリースされる。

`vv`部はマイナーバージョンである。
当該のリリース内での変更が一定数蓄積された際にバージョンアップする。22年5月に代わっても変更先が22年4月リリースのものであれば、`22.04.1`となる。

## Dockerについて

Dockerはアプリケーションをコンテナと呼ばれるものに閉じ込めて実行する。
その際、Linuxの根幹部分であるLinuxカーネルはDockerの親機と共有する。

Dockerでは後述するExposeやMountをしない限り、ホストの環境を汚さない（変更しない）ため、
簡単に環境を破棄できる。
コンテナ内で火災が起きたときは、コンテナごと海に不法投棄すればよいのと同じように、
コンピュータでも電子の藻屑にすることができる（トカゲのしっぽ切りを想像してもらいたい）

Dockerではコンテナのテンプレートをイメージとして提供する。
イメージは[Docker Hub](https://hub.docker.com/)や[ghcr.io](https://github.com/features/packages)などのコンテナレジストリで提供される。

多くの場合、コンテナイメージをそのまま起動することで、テンプレートの提供するアプリケーションをすぐに使えるようになるため、
簡単に環境の構築が可能であり、再現が簡単であるため、環境を簡単に捨てられる。

### DockerのExpose, Host Mount

Dockerはコンテナであり、このコンテナ内から外にはアクセスできない。
（現実のコンテナにソフトが閉じ込められているイメージを持つとよい）

外部からの入力を許可するにはポートのExpose（コマンドで言うと`-p 80:80`などの引数をrunコマンドに追加することでで指定する）を利用する。
標準では、親機でNAPTを行う。この説明は別の資料で行うが、親機のIPアドレスの指定したポートを転送すると思ってもらいたい。

Docker上のデータを保持したい場合、手法は様々あるが最も簡単なのはHost Mountである。
親機の特定のディレクトリもしくはファイルを、Dockerコンテナ内の任意の場所にそのまま配置し、内容を同期する。
これにより、データをホストに退避させることが可能である。

## MattermostのDockerイメージ

MattermostはDockerでの運用が可能である。
Dockerで運用する場合、Mattermostの更新はDockerイメージの更新で行うため、
非常に手軽に更新ができる。

> [!NOTE]
> この後にインストールなどの詳細な手順が続くが、
> 必ず[公式ドキュメント](https://docs.mattermost.com/install/install-docker.html)を参照し、常に最新の情報を確認すること。
> 本記事において紹介する手法は、必ずしも最新版に適合せず、また、非推奨の項目が含まれている場合がある。

MattermostのDockerイメージは[公式のDocker Hub](https://hub.docker.com/u/mattermost/)で提供されている。

無料版であるTeamエディションは、
基本的に[mattermost/mattermost-team-edition](https://hub.docker.com/r/mattermost/mattermost-team-edition)イメージを使うことになる。

Dockerのタグ（バージョン情報）はMattermostのリリースバージョンに合わせられている。

### 想定されるExpose

Mattermostは下記のポートを利用する

- 8065: Mattermostサーバー (必須、リバースプロキシ等で80/tcpや443/tcp+tlsに転送することが多い)
- 8075: [Performance Metrics](https://docs.mattermost.com/scale/performance-monitoring.html) (プロファイルやメモリヒープ等を確認したい場合のみ。詳細はドキュメントを参照すること)
- 8443/udp: 通話機能 (純正のCallsを利用する場合のみ必要)
- 8443/tcp: 通話機能 (純正のCallsを利用する場合のみ必要)
- 8045/tcp: 通話機能 (純正のCallsを利用する場合のみ必要)
- 3478/udp: 通話機能 (純正のCallsを利用する場合のみ必要)

通話機能に関する詳細は、公式ドキュメントの[Calls self-hosted deployment](https://docs.mattermost.com/configure/calls-deployment.html)を参考にすること。

利用ポートとその用途は、公式ドキュメントの[Architecture overview](https://docs.mattermost.com/getting-started/architecture-overview.html)を参考にすること。
また、このドキュメントには、Mattermostが必要な通信ポートの一覧が記載されている。
必要最低限のネットワーク上で構築したい場合は、このドキュメントを参照すること。

### 想定されるHost Mount

下記のディレクトリをホストマシンにマウントすることが想定される。

- /mattermost/config: Mattermostの設定ファイル（必須）
- /mattermost/data: Mattermostのデータ（S3だと不要？要検証）
- /mattermost/logs: Mattermostのログ（任意、Dockerのログ出力から取ることが可能）
- /mattermost/plugins: Mattermostのプラグイン（強く推奨）
- /mattermost/client/plugins: Mattermostのクライアントプラグイン（強く推奨）
- /mattermost/bleve-indexes: Mattermostの[Bleve検索](https://docs.mattermost.com/deploy/bleve-search.html)インデックス（推奨）
