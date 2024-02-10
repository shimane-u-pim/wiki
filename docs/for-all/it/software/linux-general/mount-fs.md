# ファイルシステムのマウント（一般的にUSBメモリの使い方）

> [!IMPORTANT]
> 本記事ではUbuntu 22.04での操作を想定しています。
> 他のディストリビューション（特にBusyBox）の場合、コマンドやディレクトリ構造が異なる場合があります。

ファイルシステムについては、[dictionary/it#ファイルシステム](../../../dictionary/it.md#ファイルシステム)を参照。

マウントについては、[dictionary/it#マウント](../../../dictionary/it.md#マウント)を参照。

## 概要

USBメモリやHDDなどの外部ストレージを使う際には、OSから使えるようにする必要がある。これをマウントという。

マウントするにはそのデバイスに1つ以上のパーティションが存在し、
それぞれのパーティションに一般的なツリー型のディレクトリ・ファイル構造を持つファイルシステムが存在する必要がある。

もしそれが存在しない新品のUSBメモリであれば、ファイルシステムを作成する必要がある。
詳しくは、`mkfs`や`fdisk`で検索してほしい。将来的にこのWikiにも書くかもしれない。

## 認識しているパーティションの確認

多くの場合、HDDやUSBメモリは`/dev/sd`から始まるデバイス名で認識される。

`/dev/sda`から`/dev/sdz`までがよく見られる範囲であり、多くの場合、
`/dev/sda`にはLinuxがインストールされているディスク（物理）が割り当てられている。
ちなみに、27台目からは`aa`とExcelのような表記になる。

`/dev/sda1`等の様に数字が付いている場合、それはパーティション番号である。

多くの場合、USBや外付けHDD等には1つのパーティションのみが存在するため、`sdb`がUSBメモリなのであれば、
`/dev/sdb`と`/dev/sdb1`の2つしか見当たらないことが多い。

`lsblk`コマンドを使うと、デバイス名とそのデバイスが持つパーティションの情報を見ることができる。

```txt
$ lsblk
NAME                         MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda                            8:0    0    8G  0 disk
├─sda1                         8:1    0  487M  0 part /boot
├─sda2                         8:2    0    1K  0 part
└─sda5                         8:5    0  7.5G  0 part
  ├─test--alive01--vg-root   254:0    0  6.6G  0 lvm  /
  └─test--alive01--vg-swap_1 254:1    0  980M  0 lvm  [SWAP]
sr0                           11:0    1 1024M  0 rom
```

この例ではUSBメモリを挿入していないため、Linuxのインストール先である`sda`のみが出てくる。
なお、`sr0`はCD/DVDドライブである。

もし`sdb`がUSBメモリである確証が無いのであれば、一度USBメモリを抜いて見よう。
もし抜いても`sdb`が残っているのであれば、それはUSBメモリではない可能性がある。
必ず確認しよう。

## `mount`コマンドの使い方

`mount`コマンドでマウントをするには、
マウント先に空ディレクトリがあることが必要である。

事前にマウント先のディレクトリを作成しておく。

```shell
sudo mkdir /mnt/usb-memory
```

ここで、`usb-memory`部分に当たる名前は任意である。
`/mnt/`の部分は慣習的にマウント先のディレクトリとして使われる。
大規模なディストリビューションでは、自動的にUSBをマウントする際に、`/media/`以下にディレクトリが作成されることがある。

ディレクトリの作成が出来たら、実際にマウントを行う。

```shell
sudo mount /dev/sdb1 /mnt/usb-memory
```

このコマンドでは、`/dev/sdb1`を`/mnt/usb-memory`にマウントしている。

`/dev/sdb1`はUSBメモリのパーティションである。
`/mnt/usb-memory`はマウント先のディレクトリである。

このコマンドにおいて、ファイルシステムのタイプと読み書き権限については明示的に設定されていない事を注意すること。

もし、読み取り専用でマウントしたい場合は、`-r`オプションを使う。書き込み可能でマウントしたい場合は、`-w`オプションを使う。

ファイルシステムのタイプは、`-t`オプションを使って指定する。

```shell
sudo mount -t vfat /dev/sdb1 /mnt/usb-memory
```

対応しているファイルシステムの一覧は、`/proc/filesystems`に記述されているので、`cat`等で読もう。

## アンマウント

> [!IMPORTANT]
> Linuxではストレージデバイスを取り外す前に、
> 必ず`umount`コマンドを使ってアンマウントすること。

マウントしたデバイスをアンマウントするには、`umount`コマンドを使う。

```shell
sudo umount /mnt/usb-memory
```

## 外部リンク

- [Ubuntu Manpage: mount - mount a filesystem](https://manpages.ubuntu.com/manpages/jammy/en/man8/mount.8.html)
- [Ubuntu Manpage: umount - unmount filesystems](https://manpages.ubuntu.com/manpages/jammy/en/man8/umount.8.html)