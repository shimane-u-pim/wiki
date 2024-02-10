# 情報系辞書

## A

### Advanced Packaging Tool

Debian系のディストリビューションで利用されるパッケージ管理システム。

詳細は、[APT（Debian系ディストリのパッケージ管理）](../it/software/debian/apt.md)を参照。

## B

### Bash

プロダクト名

現在最も標準的に使われているLinuxのシェル。

### BusyBox

プロダクト名 ([公式サイト](https://www.busybox.net/), [Wikipedia:ja](https://ja.wikipedia.org/wiki/BusyBox))

組み込みシステム向けの軽量なソフトウェア。

`ls`や`cp`等のコマンドを1つの実行ファイルにまとめたものとして実行できる。

## C

### C\#

プログラミング言語

Microsoftが開発したオブジェクト指向プログラミング言語。

## D

### DKIM

システム名の略

[DomainKeys Identified Mail](#domainkeys-identified-mail)の略。

### DMARC

システム名の略語

[Domain-based Message Authentication, Reporting and Conformance](#domain-based-message-authentication-reporting-and-conformance)の略。

### DomainKeys Identified Mail

システム名

メールを送信する際に、
メール送信者がメール本体にドメイン保持者とあらかじめ示し合わせた方式を利用した署名を行う事で、
メール送信者がドメイン保持者の意図通りにメールを扱っている事を照明するための仕組み。

DKIMと略される。

### Domain-based Message Authentication, Reporting and Conformance

システム名

メールの受信者が、メールの送信者が送信したメールをドメイン保持者がどのように扱ってほしいかを指定するための仕組み。
また、取扱いの結果をドメイン保持者に報告するための仕組み。

DMARCと略される。

## E

### Ed25519

実装の名前 ([Wikipedia:ja](https://ja.wikipedia.org/wiki/%E3%82%A8%E3%83%89%E3%83%AF%E3%83%BC%E3%82%BA%E6%9B%B2%E7%B7%9A%E3%83%87%E3%82%B8%E3%82%BF%E3%83%AB%E7%BD%B2%E5%90%8D%E3%82%A2%E3%83%AB%E3%82%B4%E3%83%AA%E3%82%BA%E3%83%A0#Ed25519))

エドワーズ曲線デジタル署名アルゴリズムの実装の1つ。

SSHの公開鍵認証等に利用されている。

## F

### foo

有名な[プレースホルダー](#プレースホルダー)

## G

### `GET`

HTTPメソッド名

## H

### hoge

有名な[プレースホルダー](#プレースホルダー)

## I

### IPアドレス

名詞

インターネット上で通信を行うためのホストの識別子。

インターネットプロトコル（IP）によって通信を行うためには、
IPアドレスが相互に必要である。

また、グローバルIPアドレスとプライベートIPアドレスがある。

## J

### Java

プログラミング言語名 ([公式サイト](https://www.java.com/), [Wikipedia:ja](https://ja.wikipedia.org/wiki/Java))

サン・マイクロシステムズが開発したオブジェクト指向プログラミング言語。

現在はOracleが開発を行っている。

JavaScriptとの関係は一切ない。

### JavaScript

プログラミング言語名 ([Wikipedia:ja](https://ja.wikipedia.org/wiki/JavaScript))

ウェブページ等でよく使われている言語。

Javaとの関係は一切ない。

### JavaScript Object Notation

データ形式 ([Wikipedia:ja](https://ja.wikipedia.org/wiki/JavaScript_Object_Notation))

JSONとも呼ばれる。文字列・数値・真偽値（Boolean）・NULLをKeyValue Pair・配列に入れることで表現する。
KeyValue Pair・配列のネストも可能である。

### JSON

データ形式の略語

[JavaScript Object Notation](#javascript-object-notation)の略。

## K

### KVM

略語

- [KVMコンソール](#kvmコンソール)
- [KVMスイッチ](#kvmスイッチ)
- [Kernel-based Virtual Machine](#kernel-based-virtual-machine)

### KVMコンソール

製品名

キーボード・マウス・ディスプレイの付いたユニット。
1Uサイズでラックに取り付けることができる物が主流。

### KVMスイッチ

キーボード、マウス、ディスプレイを複数のコンピュータで共有する（利用できるPCを切り替える）。
CPU切り替え機等と呼ばれる場合もある。

### Kernel-based Virtual Machine

プロダクト名 ([Wikipedia:ja](https://ja.wikipedia.org/wiki/Kernel-based_Virtual_Machine))

ハイパーバイザ（高性能なコンピュータの中に仮想的なコンピュータを作成し、動作させる技術）
をLinuxカーネルに実装したもの。

## L

### Linux

プロダクト名 ([公式サイト](https://kernel.org/), [Wikipedia:ja](https://ja.wikipedia.org/wiki/Linux))

## M

### Man Page

システム名 ([Wikipedia:ja](https://ja.wikipedia.org/wiki/Man%E3%83%9A%E3%83%BC%E3%82%B8))

## N

### NTP

プロトコル名 ([Wikipedia:ja](https://ja.wikipedia.org/wiki/Network_Time_Protocol))

ネットワーク上で時刻を同期させるためのプロトコル。
Network Time Protocolの略。

簡易版にSNTPがある。

## O

### OpenSSH

プロダクト名 ([公式サイト](https://www.openssh.com/), [Wikipedia:ja](https://ja.wikipedia.org/wiki/OpenSSH))

SSHの実装の1つ。
現状最も多く使われていると言われている。

## P

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

## Q

### QEMU

プロダクト名 ([公式サイト](https://www.qemu.org/), [Wikipedia:ja](https://ja.wikipedia.org/wiki/QEMU))

ハードウェアエミュレータ。
仮想的にコンピュータ・ハードウェアを作成し、OSなどを動作させることができる。

## R

### RDP

プロトコル名 ([Wikipedia:ja](https://ja.wikipedia.org/wiki/Remote_Desktop_Protocol))

Remote Desktop Protocolの略。

Windowsのリモートデスクトップ機能（`mstsc.exe`）で利用されるプロトコル。

## S

### SCP

プロトコル・コマンド名 ([Wikipedia:ja](https://ja.wikipedia.org/wiki/Secure_copy))

SSH上でファイルを転送するためのプロトコル、及びコマンド。

現状は非推奨。

### SFTP

プロトコル・コマンド名 ([Wikipedia:ja](https://ja.wikipedia.org/wiki/SSH_File_Transfer_Protocol))

SSH上でファイルを転送するためのプロトコル、及びコマンド。

### SSH

プロトコル名 ([Wikipedia:ja](https://ja.wikipedia.org/wiki/Secure_Shell))

Secure Shellの略。
Linuxにおいて、遠隔地のサーバを操作したり、ファイルを転送するためのプロトコル。

### `sudo`

コマンド名 ([公式サイト](https://www.sudo.ws/), [Wikipedia:ja](https://ja.wikipedia.org/wiki/Sudo), [man]: `sudo(8)`)

管理者やその他のユーザーとしてコマンドの実行を行うためのコマンド。

### `sudoers`

ファイル名 ([man]: `sudoers(5)`)

[sudo](#sudo)コマンドの設定ファイル。
特定ユーザが特定のコマンドを実行する際の権限などを設定できる。

## T

### TCP

プロトコル名 ([Wikipedia:ja](https://ja.wikipedia.org/wiki/Transmission_Control_Protocol))

Transmission Control Protocolの略。

信頼性のある通信をIPネットワーク上で行うためのプロトコル。

### TFTP

プロトコル名 ([Wikipedia:ja](https://ja.wikipedia.org/wiki/Trivial_File_Transfer_Protocol))

Trivial File Transfer Protocolの略。

軽量で簡易的なファイルの転送が可能。
ネットワーク機器等でファームウェアの更新・リカバリ、設定のバックアップ・レストアに利用される事が多い。

## U

### UDP

プロトコル名 ([Wikipedia:ja](https://ja.wikipedia.org/wiki/User_Datagram_Protocol))

User Datagram Protocolの略。

低信頼であるが、高速な通信をIPネットワーク上で行うためのプロトコル。

## V

### VNC

プロトコル名 ([Wikipedia:ja](https://ja.wikipedia.org/wiki/Virtual_Network_Computing))

Virtual Network Computingの略。

リモートデスクトップ機能を提供するプロトコル。
LinuxやmacOS等で主に利用されている。

## W

### Windows

プロダクト名 ([公式サイト](https://www.microsoft.com/ja-jp/windows), [Wikipedia:ja](https://ja.wikipedia.org/wiki/Microsoft_Windows))

Microsoftの開発しているOS。

## X

### X11

システム名 ([Wikipedia:ja](https://ja.wikipedia.org/wiki/X_Window_System))

X Window Systemの略。

Linux等のUnix系OSで利用されるウィンドウシステム。
GUIを提供する。

## Y

### YAML

データ形式 ([Wikipedia:ja](https://ja.wikipedia.org/wiki/YAML))

YAML Ain't Markup Languageの略。

JSONと互換性のあるデータ形式であるが、JSONより表現できる内容が多く、視認性も高い。

## Z

### ZIP

ファイル形式名 ([Wikipedia:ja](https://ja.wikipedia.org/wiki/ZIP_(%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%83%95%E3%82%A9%E3%83%BC%E3%83%9E%E3%83%83%E3%83%88)))

ファイルを圧縮するためのフォーマット。

## あ行

### アプリケーション

[Wikipedia:ja](https://ja.wikipedia.org/wiki/%E3%82%A2%E3%83%97%E3%83%AA%E3%82%B1%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3)

ソフトウェア・ソフト・アプリ等と同じ意味

## か行

### 環境変数

[Wikipedia:ja](https://ja.wikipedia.org/wiki/%E7%92%B0%E5%A2%83%E5%A4%89%E6%95%B0)

情報共有を行うための機能の1つで、Windows/Linux/macOSなどのOSで利用される。

最も有名な環境変数に[`PATH`](#path)がある。
また、Linuxでは言語設定も`LANG`などの環境変数で行う。

多くの環境設定がここに含まれており、
Windowsでは`WINDIR`にWindowsのインストールディレクトリが設定されていたりする。

Linuxディストリ間やUnixベースのOS間で共通の環境変数が多いが、
Windowsとの互換性は低い。

## さ行

### シェル

[Wikipedia:ja](https://ja.wikipedia.org/wiki/%E3%82%B7%E3%82%A7%E3%83%AB)

OSとユーザの間で機能するインターフェイス。

GUIのシェルとCUIのシェルがあるが、CUIのシェルのことを差す場合が多い。

Linuxでは`bash`が標準的なシェルとして使われている。
Windowsでは`cmd.exe`が標準的なシェルとして使われているが、
最近では`PowerShell`が標準的なシェルとして使われることが多い。

## た行

### ダイナミックDNS

DNSサービスの一種で、IPアドレスが変動する環境で、
常に同じドメイン名でアクセスできるようにするためのサービス。

IPアドレスを割り当てられているホスト、
もしくはそのホストのNAPT内ホストが定期的にダイナミックDNS事業者にIPアドレスを通知し、
AレコードもしくはAAAAレコードを更新する。

## な行

### ネットワーク

情報通信網

## は行

### ファイルシステム

[Wikipedia:ja](https://ja.wikipedia.org/wiki/%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%82%B7%E3%82%B9%E3%83%86%E3%83%A0)

一般的なUSBメモリやHDDは、
データをただ片っ端から書き込んでいるのではなく、
ファイルシステムを使い規則的にデータを扱っている。
これにより、ディレクトリ（フォルダ）やファイルの階層構造を持つことができたり、
ストレージの何処から何処までにデータがあるのかを特定できる。

### プレースホルダー

プログラムやコードの中で、意味がないことを示すために使われるもの。

## ま行

### マウント

[Wikipedia:ja](https://ja.wikipedia.org/wiki/Mount_(UNIX))

ファイルシステムをOSから使えるようにすること。
`mount`はコマンド名としても使われる。

「USBメモリをーして中の情報を閲覧する」

## や行

### ユーザ

コンピュータを利用する人間。

## ら行

### リモートデスクトップ

遠隔地からコンピュータを操作するための技術。

Windowsでは`mstsc.exe`が標準的なリモートデスクトップクライアントとして使われている。

## わ行

### ワイヤレス

無線通信

[man]: ../it/manpage.md
