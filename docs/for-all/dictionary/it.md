# 情報系辞書

## 一覧

### `man`

コマンド名 ([Wikipedia:ja](https://ja.wikipedia.org/wiki/Man%E3%83%9A%E3%83%BC%E3%82%B8), [man]: `man(1)`)

### `PATH`

[環境変数](#環境変数)の1つ。

Windows、Linux、macOSなどのOSで利用される。

コマンドライン等において、`PATH`に記述されているディレクトリの中から実行ファイルを探し出し、実行する。

例えば、`C:\Windows\System32\;C:\Windows\`という`PATH`が設定されている場合、
`cmd.exe`を実行しようとすると、`C:\Windows\System32\`を検索しする。
もし見つからなければ`C:\Windows\`を検索する。
それでも見つからなければコマンド無しのエラーとなる。

複数のディレクトリを記述することが一般的で、
ディレクトリの区切り文字はOSによって異なる。
Windowsであれば`;`を使い、LinuxやmacOSであれば`:`を使う。

「`PATH`を通す」はディレクトリを`PATH`に追加することを指す。
「`PATH`が通っていない」とは、`PATH`に記述されているディレクトリに目的のディレクトリが存在しない事を差す。

### `sudo`

コマンド名 ([公式サイト](https://www.sudo.ws/), [Wikipedia:ja](https://ja.wikipedia.org/wiki/Sudo), [man]: `sudo(8)`)

管理者やその他のユーザーとしてコマンドの実行を行うためのコマンド。

### `sudoers`

ファイル名 ([man]: `sudoers(5)`)

[sudo](#sudo)コマンドの設定ファイル。
特定ユーザが特定のコマンドを実行する際の権限などを設定できる。

### 環境変数

[Wikipedia:ja](https://ja.wikipedia.org/wiki/%E7%92%B0%E5%A2%83%E5%A4%89%E6%95%B0)

情報共有を行うための機能の1つで、Windows/Linux/macOSなどのOSで利用される。

最も有名な環境変数に[`PATH`](#path)がある。
また、Linuxでは言語設定も`LANG`などの環境変数で行う。

多くの環境設定がここに含まれており、
Windowsでは`WINDIR`にWindowsのインストールディレクトリが設定されていたりする。

Linuxディストリ間やUnixベースのOS間で共通の環境変数が多いが、
Windowsとの互換性は低い。

## 外部リンク

なし

[man]: ../it/manpage.md
