+  元文書: [sass/doc-src/SASS_REFERENCE.md at f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2 · nex3/sass · GitHub](https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#output-style "sass/doc-src/SASS_REFERENCE.md at f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2 · nex3/sass · GitHub")

## 出力形式　[原文](http://sass-lang.com/docs/yardoc/file.SASS_REFERENCE.html#output_style)

Although the default CSS style that Sass outputs is very nice
and reflects the structure of the document,
tastes and needs vary and so Sass supports several other styles.

Sass がデフォルトで出力する CSS の形式は非常に良くできていて、
文書構造を反映したものになっていますが、好みや必要性は様々ですので、
Sass ではいくつかの出力形式をサポートしています。

Sass allows you to choose between four different output styles
by setting the [`:style` option](#style-option)
or using the `--style` command-line flag.

Sass では、四つの出力形式を選べます。[`:style` オプション](#style-option)、
または、コマンドライン引数で `--style` を使ってください。

### `:nested`

Nested style is the default Sass style,
because it reflects the structure of the CSS styles
and the HTML document they're styling.
Each property has its own line,
but the indentation isn't constant.
Each rule is indented based on how deeply it's nested.
For example:

ネスト形式（Nested style）は、Sass のデフォルト形式になっています。
というのも、この形式は CSS のスタイルや
HTML文書の構造を反映したものになっているからです。
どのプロパティも一行で表記されますが、インデントは一定ではありません。
どのルールも、どれだけネストされているかにしたがってインデントされます。
例えば:

    #main {
      color: #fff;
      background-color: #000; }
      #main p {
        width: 10em; }

    .huge {
      font-size: 10em;
      font-weight: bold;
      text-decoration: underline; }

Nested style is very useful when looking at large CSS files:
it allows you to easily grasp the structure of the file
without actually reading anything.

ネスト形式は、巨大な CSSファイルの中身を見るときに非常に役立ちます。
実際に読みこむことなく簡単にファイルの構造がつかめます。

### `:expanded`

Expanded is a more typical human-made CSS style,
with each property and rule taking up one line.
Properties are indented within the rules,
but the rules aren't indented in any special way.
For example:

展開形式（Expanded style）は、人間が書く典型的な CSS の形式に近いものです。
プロパティもルールも一行で表記されます。
プロパティはルール内でインデントされますが、
ルール自体はとくにインデントされません。
例えば:

    #main {
      color: #fff;
      background-color: #000;
    }
    #main p {
      width: 10em;
    }

    .huge {
      font-size: 10em;
      font-weight: bold;
      text-decoration: underline;
    }

### `:compact`

Compact style takes up less space than Nested or Expanded.
It also draws the focus more to the selectors than to their properties.
Each CSS rule takes up only one line,
with every property defined on that line.
Nested rules are placed next to each other with no newline,
while separate groups of rules have newlines between them.
For example:

コンパクト形式（Compact style）は、
ネスト形式や展開形式よりも少ない記述量で出力します。
プロパティよりもセレクタに着目しやすい形式でもあります。
どの CSSルールも一行になり、その一行の中にすべてのプロパティが定義されます。
ネストされたルールは、隣り合った行に空行を含まず配置されます。
一方で、別々のグループに属するルールは空行で分けられます。
例えば：

    #main { color: #fff; background-color: #000; }
    #main p { width: 10em; }

    .huge { font-size: 10em; font-weight: bold; text-decoration: underline; }

### `:compressed`

Compressed style takes up the minimum amount of space possible,
having no whitespace except that necessary to separate selectors
and a newline at the end of the file.
It also includes some other minor compressions,
such as choosing the smallest representation for colors.
It's not meant to be human-readable.
For example:

圧縮形式（Compressed style）は、可能な限り、最小の記述量で出力します。
セレクタを分けたり、ファイルの最後に改行を含めるといった必要なもの以外は、
空白文字も含みません。
さらに、色の指定などを最小の記述に書き換えるといったちょっとした圧縮も行います。
人間が読むためのものではありません。
例えば:

    #main{color:#fff;background-color:#000}#main p{width:10em}.huge{font-size:10em;font-weight:bold;text-decoration:underline}

