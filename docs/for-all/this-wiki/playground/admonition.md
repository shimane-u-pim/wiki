# PIm WikiのAdmonition

Pim WikiではGitHub上でのプレビューと大きな破綻が起きないように、
同じ書式でAdmonitionを呼び出せるように設定されています。

これは、MkDocsのプラグインである[mkdocs-callouts](https://github.com/sondregronas/mkdocs-callouts)が、
GitHubでも使われている書式のAdmonitionをPyMdownで使えるものに変換し、
[PyMdown Extensions](https://facelessuser.github.io/pymdown-extensions/)でHTMLに変換できるようにされているからです。

## Admonitionの書式

```md
\> [!NOTE]
\> これはNoteです。
```

の様に書くことで、Admonitionを呼び出すことができます。

※ 上記サンプルでは、`\`を入れていますが、実際には入れる必要はありません。

## Admonitionの種類とGitHubへの追従

`NOTE`のほかに、`TIP`, `IMPORTANT`, `WARNING`, `CAUTION`があります。

これ以外にも、理論上はテーマパッケージである[MkDocs Materialの提供するAdmonitionsタイプ](https://squidfunk.github.io/mkdocs-material/reference/admonitions/#supported-types)が利用できますが、
GitHub上でのプレビューができないため、Pim Wikiでの利用は禁止されています（MUST NOT）。

今後GitHub側が新しいタイプのAdmonitionをサポートした場合、
Pim Wikiはそれに追従し、追加を行います。
また、書式が変わった場合もできる限り追従を行います。

GitHubへの追従はGitHub Community上の投稿（[[Markdown] An option to highlight a "Note" and "Warning" using blockquote (Beta) #16925](https://github.com/orgs/community/discussions/16925)）を参照します。

また、初期のGitHubで利用できるAdmonition書式（2022/05/10に追加されたもの）は対応していません。

## すべての書式

> [!NOTE]
> Notes

> [!TIP]
> Tips

> [!IMPORTANT]
> Important things

> [!WARNING]
> Warnings

> [!CAUTION]
> Cautions
