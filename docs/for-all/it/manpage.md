# man page

Linuxにおいて、コマンドやファイルの使い方を調べるためのシステムである。

使用用途は下記の通り

```bash
man <コマンド名|ファイル名など>
man <セクション番号> <コマンド名|ファイル名など>
```

例えば、`sudo(8)`を調べる場合は下記のようになる。

```bash
man sudo
man 8 sudo
```

多くの場合、セクション番号は省略できるが、
コマンド名とファイル名が同じなど、
複数のセクションがある場合、見たいセクションを明示的に指定する必要がある場合がある。

## 外部リンク

- [FreeBSD Manual Page](https://man.freebsd.org/cgi/man.cgi)（オンラインで一部のmanページを確認可能）
- [manページ - Wikipedia](https://ja.wikipedia.org/wiki/Man%E3%83%9A%E3%83%BC%E3%82%B8#%E5%A4%96%E9%83%A8%E3%83%AA%E3%83%B3%E3%82%AF)
