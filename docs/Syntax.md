原文:[https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#syntax](https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#syntax)

## 構文

Sassでは2つの構文を使用できます。
1つ目は、SCSS (Sassy CSS)として知られ、このリファレンス全体で使用しており、
CSS3の構文を拡張するものです。
これは、すべての正しいCSS3スタイルシートが、
同じ意味を持つ正しいSCSSファイルであることを意味します。
加えて、SCSSはほとんどのCSSハックや、
[IEの古い `filter` 構文](http://msdn.microsoft.com/en-us/library
/ms533754%28VS.85%29.aspx)のようなベンダ固有の構文を解釈します。
この構文は後で紹介するSass機能で拡張されます。
この構文を使用するファイルの拡張子は `.scss` です。

2つ目は古い構文で、インデント構文 (もしくは単に "Sass") として知られており、
CSSの記述をより簡単にする方法を提供します。
この構文では、セレクタのネストを示すために、カッコよりもインデントを、
プロパティの区切りに、セミコロンではなく改行を使用します。
こちらのほうがSCSSよりも簡単に読めて、また速く書けるかもしれません。
インデント構文はSCSSと全く同じ機能を持っていますが、
中には少し構文が違うものがあります。
この構文の違いについては、{file:INDENTED_SYNTAX.md インデント構文のリファレンス}に書いています。

どちらの構文を使っていても、お互いのファイルを[インポート](#import)できます。
ファイルは、`sass-convert` のコマンドラインツールを使うことで、ある構文からもう一方の構文へ自動的に変換できます。

```
# SassからSCSSへの変換
$ sass-convert style.sass style.scss

# SCSSからSassへの変換
$ sass-convert style.scss style.sass
```