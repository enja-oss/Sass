原文:[https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#syntax](https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#syntax)

## Syntax

## 構文

There are two syntaxes available for Sass.
The first, known as SCSS (Sassy CSS) and used throughout this reference,
is an extension of the syntax of CSS3.
This means that every valid CSS3 stylesheet
is a valid SCSS file with the same meaning.
In addition, SCSS understands most CSS hacks
and vendor-specific syntax, such as [IE's old `filter`
syntax](http://msdn.microsoft.com/en-us/library/ms533754%28VS.85%29.aspx).
This syntax is enhanced with the Sass features described below.
Files using this syntax have the `.scss` extension.

Sassでは2つの構文を使用することができます。
1つ目は、SCSS (Sassy CSS)として知られ、このリファレンス全体で使用しており、
CSS3の構文を拡張するものです。
これは、すべての正しいCSS3スタイルシートが、
同じ意味を持つ正しいSCSSファイルであることを意味します。
加えて、SCSSはほとんどのCSSハックや、
[IEの古い `filter` 構文](http://msdn.microsoft.com/en-us/library
/ms533754%28VS.85%29.aspx)のようなベンダ固有の構文を解釈します。
この構文は後で記述したSass機能で拡張されます。
この構文を使用するファイルの拡張子は `.scss` です。

The second and older syntax, known as the indented syntax (or sometimes
just "Sass"),
provides a more concise way of writing CSS.
It uses indentation rather than brackets to indicate nesting of selectors,
and newlines rather than semicolons to separate properties.
Some people find this to be easier to read and quicker to write than SCSS.
The indented syntax has all the same features,
although some of them have slightly different syntax;
this is described in {file:INDENTED_SYNTAX.md the indented syntax
reference}.
Files using this syntax have the `.sass` extension.

2つ目は古い構文で、インデント構文 (もしくは単に "Sass") として知られており、
CSSの記述をより簡単にする方法を提供します。
この構文では、セレクタのネストを示すために、カッコよりもインデントを、
プロパティの区切りに、セミコロンよりも改行を使用します。
一部の人々は、この構文がSCSSよりも簡単に読め、早くかけることに気づきました。
インデント構文はSCSSと全く同じ機能を持っていますが、
中には少し構文が違うものがあります。
この構文の違いについては、{file:INDENTED_SYNTAX.md インデックス構文のリファレンス}に書いています。

Either syntax can [import](#import) files written in the other.
Files can be automatically converted from one syntax to the other
using the `sass-convert` command line tool:

どちらの構文を使っていても、お互いのファイルを[インポート](#import)することができます。
ファイルは、`sass-convert` のコマンドラインツールを使うことで、ある構文からもう一方の構文へ自動で変換することができます。

    # Convert Sass to SCSS
    # SassからSCSSへの変換
    $ sass-convert style.sass style.scss

    # Convert SCSS to Sass
    # SCSSからSassへの変換
    $ sass-convert style.scss style.sass