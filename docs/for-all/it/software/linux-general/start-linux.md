# Linuxを始めよう

　本資料では、Linuxの簡単な概要の解説及び、Gentoo Linuxのインストールについて解説します。

> [!IMPORTANT]
> 本資料には、かなり強めの思想が入っています。

## Linuxとは

　Linuxは世界で最も使われている**かもしれない**OS（正確には[カーネル]）です。
Linux Kernel本体は無料な上、大抵の関連システムは[GNU GPL License]という、
めんどくさい規約で保護されたLinuxに有料のシステムを追加することをしません[^1]。
そのため、大量のライセンス費用が必要になる組み込み等でも活用されていますし、
その古い歴史から[^2]サーバ業界でも昔からよく使われています。

### Windowsとの違い

　LinuxはOSではなくKernelです。そのため、Windows OSと比べることは出来ません。
一応WindowsにもKernelという概念は（もちろん）存在し、
NT Kernel等と呼ばれていますが (See: [Architecture of Windows NT](https://en.wikipedia.org/wiki/Architecture_of_Windows_NT))、
悲しいことに筆者にはその知識が無いため、ここで詳細を解説することはしません。唯一解説出来るのは、Linux KernelとNT Kernelは気が狂うレベルで大きく異なるということです。

　では、Linuxを搭載した有名なOSである[Debian]/[Ubuntu]とWindows 11を比べてみましょう。
これらは全く異なる設計を持っています。
特にDebianはそのソースコード（設計図）のすべてが公開されている[^3]のに対し、
Windows 11で公開されているのはほんの一握りです。
まあ、ソースコードに大量のFワードが入っている[^4]OSのソースコードなんて、
まともに公開できた物ではないでしょうが。

　また、Linuxは先ほどから説明しているようにあくまでもOSの中のカーネルにすぎない為、
それ以外の部分を自由に変更できます。
[Lubuntu], [KDE Neon], [Xubuntu]等の見た目がガラッと変わるものはもちろん、
[ブートローダ]の変更も可能です。
これらは、Windowsでは（特殊な改造で変更出来なくはないですが、公式的には）変更できないものです。

### Linuxディストリビューション

　Linux入門系の資料を見るとLinuxディストリビューションについての記述があるはずです。
きっと[Ubuntu]や[Debian]、[Linux Mint]、[CentOS]等を例として解説されているでしょう。

　それらの説明では、カスタムされた独自のLinux等という、
絶妙に合っている場合があるため否定しきれないなんとも言えない表記がされる場合がある。

　ディストリビューションとしてLinuxカーネル本体に変更を与える場合はもちろんある。
[Debianのカスタム](https://salsa.debian.org/kernel-team/linux/-/tree/debian/latest/debian/patches?ref_type=heads)や、[Gentooのカスタム](https://gitweb.gentoo.org/repo/gentoo.git/tree/sys-kernel/gentoo-kernel)等、
様々な変更が加えられている。

　しかし、ディストリビューションの異なる点としてよく上げられているのは、
上記のカーネルチューニングなどではなく、Linux以外の標準ソフトウェアである。
これを独自のLinuxカスタムとか、自作Linuxと言われているのはあまり気持ちのよいものではないが、
とりあえずこの定義にしたがって説明しよう。

　Linuxではカスタマイズ出来る点がかなりの数ある。
思いつくものを下記に箇条書きにしてみよう。

- [ブートローダ] Grub/LILO/SysLinux もしくは使わない[^5]など
- [Initシステム](https://wiki.gentoo.org/wiki/Init_system) 最近は[systemd]が主流
- ディスプレイサーバ [X系](https://wiki.archlinux.jp/index.php/Xorg)とか[Wayland](https://wiki.archlinux.jp/index.php/Wayland)とか
- [Window Manager]/[Desktop Environment] [Fluxbox]とか[KDE Plasma]とか
- ブラウザソフト: ChromiumとかFirefoxとか
- シェル: bashとかzshとか

　特に、WMやDEを変更したLinuxは見た目が大きく異なるため、
全く別のLinux OSだと思われている説がある。
これらはすべてLinuxカーネルの上で成り立っていて、
現代であればおそらくGrub2で起動し、
これまた現代であればWaylandが動いている。
ただWM/DEが違うだけで、下手したらカーネルがカスタムも同じだと言うのに、
異なるLinuxOSだと表現されるのには違和感を感じてしまう。

　そのため、本資料では、真のLinuxを知ってもらうため、
[Gentoo Linux]のインストールを行いながら、Linux OSとは何かをステップアップ的に学習してもらう。
本資料の開始時に思想強めの内容が含まれていると警告しているが、
ここまで耐えれた人なら大丈夫だろう。
この資料は早い話、「思想の強い不審者が（その人の考える）真のLinuxに関する認識を押し売りする」ものだと考えてもらいたい。

　なお、本資料は筆者の趣味嗜好で[Gentoo Linux]という（筆者が思うに）最も初心者向きで、
最高に頼もしいOSを使っていくが、
Linuxがインストールされているソフトウェアによって全く異なるように見えるという点を確認するだけなら、
[Debian]や[Ubuntu]でもよい。
特に[Debian]はinitシステムの変更が出来る[^6]という頼もしい子であるから、
少し難易度が高すぎると思ったらこれを使ってもよいかもしれない。

　また、[Arch Linux]というものもある。
私は個人的にi686マシンにLinuxを入れて使いたいというニーズがあるから、
このディストリビューションは使っていないが、
もし興味があるなら使ってみてもよいだろう。
Gentooより要求スペックが少なく、難易度も低いらしい。

　もしあなたがパソコンに詳しい人であれば、
[Linux From Scratch]をやってみてもよいだろう。
きっと楽しい。

## Gentoo Linuxを始める

　**公式がとてもわかりやすい[Gentoo AMD64 Handbook]を出しているのでそれを見てほしい。**

　と投げやりにしたくなるレベルでこの資料はとてもよく出来ているが、
さすがに酷な気もするので、こちらで解説を入れていく。

　また、中身を消してもよいPCがない場合は、
適当に[VMware Workstation Pro]あたりをインストールして使おう。
使い方は製品名でググれば、大量に出てくる。
もしかしたら他の部員が新しいセクションで軽く使い方を書いてくれるかもしれない。
この資料はこんな内容をしてはいるものの、一応新入部員育成のための資料ではあるから…

　もしBroadcomに個人情報を聞かれるのが嫌なのであれば、
[Oracle VirtualBox]や、Windows 11 Proを利用しているのであれば、[Hyper-V]を使うのもアリだろう。
これらの使い方もググれば出てくるはずなので、説明しない。

## 外部リンク

[^1]: [Red Hat Enterprise Linux](https://www.redhat.com/ja/technologies/linux-platforms/enterprise-linux)のような有償Linuxも一応存在します
[^2]: [UNIX](https://ja.wikipedia.org/wiki/UNIX)というものが大昔にありまして…
[^3]: [Salsa](https://salsa.debian.org/)とかで見れるはず
[^4]: [The Hidden Gems in the Windows Source Code - Enderman - YouTube](https://youtu.be/nnt5_qWX0eg)
[^5]: EFI Stub ([GentooWiki](https://wiki.gentoo.org/wiki/EFI_stub/ja), [ArchWiki](https://wiki.archlinux.jp/index.php/EFI_%E3%83%96%E3%83%BC%E3%83%88%E3%82%B9%E3%82%BF%E3%83%96))
[^6]: https://www.debian.org/releases/trixie/amd64/ch06s05.ja.html#id3446

[カーネル]: https://ja.wikipedia.org/wiki/%E3%82%AB%E3%83%BC%E3%83%8D%E3%83%AB
[GNU GPL License]: https://www.gnu.org/licenses/gpl.html
[Lubuntu]: https://lubuntu.me/
[KDE Neon]: https://neon.kde.org/
[Xubuntu]: https://xubuntu.org/
[ブートローダ]: https://e-words.jp/w/%E3%83%96%E3%83%BC%E3%83%88%E3%83%AD%E3%83%BC%E3%83%80.html
[Ubuntu]: https://ubuntu.com/
[Debian]: https://www.debian.org/
[Linux Mint]: https://www.linuxmint.com/
[CentOS]: https://www.centos.org/
[systemd]: https://systemd.io/
[Window Manager]: https://wiki.archlinux.jp/index.php/%E3%82%A6%E3%82%A3%E3%83%B3%E3%83%89%E3%82%A6%E3%83%9E%E3%83%8D%E3%83%BC%E3%82%B8%E3%83%A3
[Desktop Environment]: https://wiki.archlinux.jp/index.php/%E3%83%87%E3%82%B9%E3%82%AF%E3%83%88%E3%83%83%E3%83%97%E7%92%B0%E5%A2%83
[Fluxbox]: https://wiki.archlinux.jp/index.php/Fluxbox
[KDE Plasma]: https://wiki.archlinux.jp/index.php/KDE
[Gentoo Linux]: https://www.gentoo.org/
[Arch Linux]: https://archlinux.org/
[Linux From Scratch]: https://linuxfromscratch.org/
[Gentoo AMD64 Handbook]: https://wiki.gentoo.org/wiki/Handbook:AMD64
[VMware Workstation Pro]: https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion
[Oracle VirtualBox]: https://www.virtualbox.org/
[Hyper-V]: https://learn.microsoft.com/ja-jp/windows-server/virtualization/hyper-v/overview
