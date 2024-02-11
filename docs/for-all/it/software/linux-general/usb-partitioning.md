# USBメモリのパーティショニング

> [!NOTE]
> タイトルには「USBメモリの」と記述していますが、
> リムーバブルメディア向けの1パーティション環境を作る事を前提としている為であり、
> これ以外の環境でもご利用いただけます。

ストレージデバイスにはパーティションを作成する必要がある。
このパーティションは、MBRやGPTといったパーティションテーブルにより、
作成できる数が異なる。

上下関係は下記の通り

- 生のストレージ（セクタ・ブロックの集合）
    - パーティションテーブル (1つのみ、MBR/ or GPT)
        - パーティション (MBRの場合は4つまで[^1])
        - パーティション
        - LVM パーティション（論理的なボリューム、中に仮想的なパーティションを作れる）
            - ボリュームグループ
                - ボリューム（パーティションとして利用できる）
                - ボリューム
        - 拡張パーティション（MBRのみ、4パーティション上限を突破できる）
            - 論理パーティション
            - 論理パーティション

この記事では、USBメモリのパーティションを設計・作成する方法を解説する。

## パーティションテーブルの作成

MBRとGPTの2種類のパーティションテーブルがある為、適切な物を選ぶ必要がある。
Windows Vista/Windows Server 2008以降のOSでは、問題なくGPTを利用できる[^2]が、
それ以前のOSや他のデバイスで利用する場合はMBRを選択する。

> [!NOTE]
> Windows Vista/Windows Server 2008以降のOSでは、MBRでもGPTでも利用できるが、
> Windowsの起動はx64版のみ可能である[^2]。

> [!TIP]
> Windows XP x64もGPTをサポートしている[^2]が、レアである。

### MBRの制限

MBRは、4つまでのパーティションしか作成できない[^1]。
また、MBRでは2TiB以上のストレージを扱う事ができない[^1]。

### `fdisk`を使う

fdiskは、Linuxで利用できる、TUIベースのパーティショニングツールである。

```sh
sudo fdisk /dev/sdX
```

`/dev/sdX`は、パーティションを作成するデバイスを指定する。
詳細は、
[ファイルシステムのマウント（一般的にUSBメモリの使い方）](mount-fs.md)を参照。

> [!NOTE]
> `fdisk`はWSL環境では利用できない。

#### パーティションテーブルの確認

画面で`l`を入力すると、現在のパーティションテーブルが表示される。
この段階で、必ず正しいディスクが選択されているかを確認する。

#### MBRの作成

`o`コマンドでMBRテーブルを作成する。

#### GPTの作成

`g`コマンドでGPTテーブルを作成する。

### `diskpart`を使う

Windowsのコマンドラインツールである`diskpart`を使う事もできる。
`diskpart`は管理者権限が要求されるコマンドである。

```cmd
diskpart
```

`diskpart`を起動した後、以下のコマンドを実行する。
なお、`#`以降はコメントである。実際に入力しない事。
また、`DISKPART> `部分も実際の入力は不要である。

> [!CAUTION]
> ディスク内のすべてのデータが削除される。
> 必ず注意して実行する事。

```diskpart
DISKPART> list disk # ディスクの一覧を表示

  Disk ###  Status         Size     Free     Dyn  Gpt
  --------  -------------  -------  -------  ---  ---
  Disk 0    Online          931 GB  1024 KB        *
  Disk 1    Online          465 GB  1024 KB        *
  Disk 2    Online          476 GB  1024 KB        *
  Disk 3    Online         7385 MB  6152 MB        *

DISKPART> # 認識されているディスク一覧が表示される。容量やディスク番号を確認し、ディスク番号をメモする。
DISKPART> # この環境は3台のストレージと1本のUSBメモリ（8GB）が接続されている。
DISKPART> select disk 3 # メモしたディスクを選択
DISKPART> list partition

  Partition ###  Type              Size     Offset
  -------------  ----------------  -------  -------
  Partition 1    Primary            240 KB    32 KB
  Partition 2    System               8 MB   272 KB
  Partition 3    OEM               1224 MB     8 MB
  Partition 4    Primary            300 KB  1232 MB

DISKPART> # パーティションの一覧が表示される。本当にこのディスクでいいか確認する最後のチャンス。
DISKPART> clean # ディスクを初期化する。データは全て消えるので注意する。
DISKPART> # もし他人にディスクを渡す場合、`clean all`を実行する。実行時間が長いので、寝る前にでも実行すると良い。
DISKPART> convert gpt # GPTに変換する。

DiskPart successfully converted the selected disk to GPT format.

DISKPART> convert mbr # MBRに変換する。

DiskPart successfully converted the selected disk to MBR format.

DISKPART> exit
```

GPTに変換する前段階でディスクがMBRでない場合、
下記のエラーが出る。

```text
The disk you specified is not MBR formatted.
Please select an empty MBR disk to convert.
```

この場合、一度`mbr`に変換する事で、`gpt`に変換する事ができる。
また、既にGPTである場合も、このエラーが出力される。

## パーティションの設計

USBメモリの場合、1パーティションでディスクのすべての容量を利用する使い方で十分である。

もしWindowsで利用したい場合、NTFSやFAT32、exFATなどのファイルシステムを利用する事ができる。

もしLinuxで利用したい場合、ext4などのファイルシステムを利用する事ができる。

もしWindowsとLinuxの両方で利用したい場合、FAT32が最も確実であるが、NTFSなどのファイルシステムも利用できる場合がある。
また、Windows側にExt2Fsdなどのソフトウェアをインストールすることで、
Windows側でext4を利用する事も可能である。

もしLinuxとWSLで利用する場合、ext4などのファイルシステムを利用する事ができる。

> [!IMPORTANT]
> FAT32は、ファイルサイズの制限がある為、大容量のファイルを扱う場合は注意する事。
> また、FAT32は安全性が低く、Linux的な考え方であるCase sensitiveも存在しない。
>
> もし環境がNTFSに対応しているのであれば、NTFSを利用する事を推奨する。

本記事では、`fdisk`と`mkfs`を利用する方法では`ext4`を利用し、
`diskpart`を利用する方法では`NTFS`を利用する事を前提としている。

## パーティションの作成

パーティションを実際に作成する。

この記事では、先の[#パーティションの設計](#パーティションの設計)でも述べた通り、
1ディスク全体を覆うパーティションを1つのみ作成する。

### `fdisk`と`mkfs`を使う

先ほどの続きから続行する。

#### `fdisk`を使ったパーティションの作成

`n`コマンドでパーティションを作成する。

多くの場合、設定はすべてデフォルトで問題ない。

#### パーティション情報の保存

`w`コマンドでパーティションテーブルを保存する。

#### `mkfs`を使ったファイルシステムの作成

```sh
sudo mkfs.ext4 /dev/sdX1
```

`/dev/sdX1`は、作成したパーティションを指定する。

### `diskpart`を使ったパーティションの作成

Diskpartを立ち上げる。

```cmd
diskpart
```

下記の様に実行する。
なお、既にパーティションテーブルの作成された空のディスクが`select`された状態である事を前提とする。

```diskpart
DISKPART> create partition primary # 容量をフルで使ったプライマリパーティションを作成する。
DISKPART> # 本来は`select partition 1`を実行するが、`create partition`句実行後に、自動的に選択されているので不要。
DISKPART> list partition # 作成されているか念のため確認

  Partition ###  Type              Size     Offset
  -------------  ----------------  -------  -------
* Partition 1    Primary           7384 MB  1024 KB

DISKPART> # もし`*`になっていない場合、`select partition 1`を実行する。
DISKPART> format fs=ntfs quick
DISKPART> # もし他人に譲渡する場合、`quick`の部分は削除する
```

もしディスクをマイコンピュータから見つけられない場合、
`assign`を実行する事で、ドライブレターを割り当てる事ができる。

```diskpart
DISKPART> select disk 3
DISKPART> select partition 1
DISKPART> assign
```

## 外部リンク

- [パーティショニング - ArchWiki]
- [Windows and GPT FAQ - Windows 10 hardware dev | Microsoft Learn]
- [fdisk - ArchWiki](https://wiki.archlinux.jp/index.php/Fdisk)
- [diskpart | Microsoft Learn](https://learn.microsoft.com/ja-jp/windows-server/administration/windows-commands/diskpart)

[^1]: [パーティショニング - ArchWiki]
[^2]: [Windows and GPT FAQ - Windows 10 hardware dev | Microsoft Learn]

[パーティショニング - ArchWiki]: https://wiki.archlinux.jp/index.php/%E3%83%91%E3%83%BC%E3%83%86%E3%82%A3%E3%82%B7%E3%83%A7%E3%83%8B%E3%83%B3%E3%82%B0
[Windows and GPT FAQ - Windows 10 hardware dev | Microsoft Learn]: https://learn.microsoft.com/en-us/previous-versions/windows/hardware/design/dn640535(v=vs.85)