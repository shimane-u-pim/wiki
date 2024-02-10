# APT（Debian系ディストリのパッケージ管理）

> [!NOTE]
> この記事ではDebian系ディストリビューションにおけるAPT及びaptコマンドを紹介します。
> BusyBoxで利用できるaptコマンドは対象外です。

aptはAdvanced Packaging Toolの略であり、
Debian系ディストリビューションにおいてパッケージの管理を行うためのコマンドである。

`apt`コマンド、`apt-get`コマンド等で操作できる。

当初は`apt-get`コマンドのみが提供されており、
Debian buster（Debian 10）及びUbuntu Focal (20.04)から`apt`コマンドが導入された。

`apt`コマンドは`apt-get`コマンドの後発であるため、
進捗バーが実装されていたりと利便性が高いが、
人間向けに設定されており、これからの更新がある可能性もあるため[^1]、
スクリプトでの利用では`apt-get`コマンドの方を推奨されている。

## aptコマンドの基本的な使い方

このセクションでは一部の`apt`コマンドの利用方法について解説する。
すべての機能については、[man] `apt(8)`を。
`apt-get`コマンドについては、[man] `apt-get(8)`を参照すること。

> [!NOTE]
> このセクションでは、rootユーザでの操作を`sudo`コマンドを用いて行うものとします。
> 既にrootユーザでログインしている場合は、`sudo`コマンドは不要です。

> [!NOTE]
> このセクションではUbuntu 22.04を利用した際の挙動を解説します。
> Debianでは一部[PATH](../../../dictionary/it.md#path)が通っていない場合があります。

### パッケージ情報の更新

パッケージ情報の更新を行う。

aptでは`/etc/apt/sources.list`に記述されたリポジトリからパッケージ情報を取得する。

このパッケージ情報が古い場合、パッケージのインストールができない等の問題が発生する。

```shell
sudo apt update
```

### パッケージのインストール

パッケージのインストールはパッケージ名を指定して行う。

事前にパッケージ情報の更新を行っておく事が推奨される。

```shell
sudo apt install <package-name>
```

もし特定のバージョンのパッケージをインストールしたい場合は、
`=`を使ってバージョンを指定することができる。

```shell
sudo apt install <package-name>=<version>
```

### パッケージの削除

パッケージを削除したい場合、`remove`サブコマンドを利用する。

```shell
sudo apt remove <package-name>
```

パッケージの設定ファイルごと削除したい場合、`purge`サブコマンドを利用する。

```shell
sudo apt purge <package-name>
```

### パッケージのアップデート

パッケージを安全に更新する場合、
`upgrade`サブコマンドを利用する。

事前にパッケージ情報の更新を実施しない場合、
最新のパッケージ情報が取得されないため、
更新をaptが検出できない。

```shell
sudo apt upgrade -u
```

`-u`オプションを指定することで、
更新が必要なパッケージの一覧を表示することができる。

### パッケージの検索

Regexを用いてパッケージを検索する。

```shell
apt search <package-name>
```

### パッケージのリスト表示

すべてのインストールできるパッケージの一覧を表示する。

```shell
apt list
```

更新可能なパッケージのみを表示することもできる。

```shell
apt list --upgradable
```

もし現在インストールされているパッケージの一覧を表示したい場合、
`--installed`オプションを指定することで表示することができる。

```shell
apt list --installed
```

### パッケージの自動削除

他のパッケージのインストールに合わせて依存関係としてインストールされたが、
そのパッケージが削除されたなどの理由で不要になったパッケージを削除する。

```shell
sudo apt autoremove
```

もし、パッケージの削除段階でそのパッケージの設定ファイルも削除したい場合、
`autopurge`サブコマンドを利用することで設定ファイルも削除することができる。

```shell
sudo apt autopurge
```

## Pimの内部ルール

Pimでは、`apt`コマンドを利用してパッケージの管理を行う際に`-y`オプションの利用を禁止する。

`-y`オプションは、パッケージのインストールや削除を行う際に、
確認を行わずに実行するためのオプションである。

このオプションを使用することで、異常な依存関係の解決や、削除すべきでないパッケージの削除などが発生する可能性があるため、このオプションを利用した操作は禁止される。

インターネットで紹介されている手法では、`-y`オプションを利用していることがあるため、注意すること。

また、他のコマンドと同じように`-dy`などと他のオプションと共通の`-`を使い表記されている場合があるので、
`-y`オプションが含まれていないか確認すること。

また、`--yes`や、`--assume-yes`も`-y`と同義である[^2]。

## 外部リンク

- [Wikipedia:ja](https://ja.wikipedia.org/wiki/APT)
- [AptCLI - Debian Wiki](https://wiki.debian.org/AptCLI)
- [apt-get - Debian Wiki](https://wiki.debian.org/apt-get)
- [aptitude - Debian Wiki](https://wiki.debian.org/aptitude)
- [aptitude - Wikipedia:ja](https://ja.wikipedia.org/wiki/Aptitude)
- [DebianPackageManagement - Debian Wiki](https://wiki.debian.org/DebianPackageManagement)

[^1]: [man] `apt(8)` [SCRIPT USAGE AND DIFFERENCES FROM OTHER APT TOOLS](https://manpages.debian.org/bookworm/apt/apt.8.en.html#SCRIPT_USAGE_AND_DIFFERENCES_FROM_OTHER_APT_TOOLS)
[^2]: [man] `apt-get(8)` [OPTIONS](https://manpages.debian.org/bookworm/apt/apt-get.8.en.html#OPTIONS)

[man]: ../../manpage.md