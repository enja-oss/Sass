+  元文書: [https://github.com/nex3/sass/blob/stable/doc-src/SCSS_FOR_SASS_USERS.md](https://github.com/nex3/sass/blob/stable/doc-src/SCSS_FOR_SASS_USERS.md)

# Intro to SCSS for Sass Users

# SassユーザーのためのSCSS導入

Sass 3 introduces a new syntax known as SCSS
which is fully compatible with the syntax of CSS3,
while still supporting the full power of Sass.
This means that every valid CSS3 stylesheet
is a valid SCSS file with the same meaning.
In addition, SCSS understands most CSS hacks
and vendor-specific syntax, such as [IE's old `filter` syntax](http://msdn.microsoft.com/en-us/library/ms533754%28VS.85%29.aspx).

Sass3は完全にCSS3と互換性のあるSCSSと呼ばれる新しい構文を導入しました。一方で、Sass構文のサポートも今後も変わらず続けていきます。文法的に妥当なCSS3のすべてのスタイルシートはSCSSにおいてもすべて文法的に妥当ということを意味しています。加えて、SCSSは多くのCSSハックと[IEの古い`filter`構文](http://msdn.microsoft.com/en-us/library/ms533754%28VS.85%29.aspx)のようなベンダー特有の構文も解釈します。


Since SCSS is a CSS extension,
everything that works in CSS works in SCSS.
This means that for a Sass user to understand it,
they need only understand how the Sass extensions work.
Most of these, such as variables, parent references, and directives work the same;
the only difference is that SCSS requires semicolons
and brackets instead of newlines and indentation.
For example, a simple rule in Sass:

SCSSはCSSを拡張したものなので、CSS上で動作するものはすべてSCSS上でも動作します。これは、SassユーザーにとってSCCSを理解するためには、Sass拡張がどのように機能しているか理解するだけで良いということを意味しています。これらの多く、つまり、変数、親参照、ディレクティブのようなものは同じように機能します。SassとSCSSの違いというのは、改行とインデントの代わりにセミコンロンとブラケットを必要とするかしないかだけです。

例：Sassのシンプルな規則

    #sidebar
      width: 30%
      background-color: #faa

could be converted to SCSS just by adding brackets and semicolons:

ブラケットとセミコロンを追加するだけでSCSSに変換することが可能：

    #sidebar {
      width: 30%;
      background-color: #faa;
    }

In addition, SCSS is completely whitespace-insensitive.
That means the above could also be written as:

さらに、SCSSは空白文字を完全に無視します。つまり上記のコードは以下のようにも記述することができます。

    #sidebar {width: 30%; background-color: #faa}

There are some differences that are slightly more complicated.
These are detailed below.
Note, though, that SCSS uses all the
{file:SASS_CHANGELOG.md#3-0-0-syntax-changes syntax changes in Sass 3},
so make sure you understand those before going forward.

多少、複雑な違いがいくつかありますが、後述を参照してください。SCSSは{file:SASS_CHANGELOG.md#3-0-0-syntax-changes Sass3におけるすべての構文変更}を取り扱っているので、次に進む前に確認しておいてください。

## Nested Selectors

## ネストセレクタ

To nest selectors, simply define a new ruleset
inside an existing ruleset:

セレクタを入れ子にするために、新しいルールセットを既存のルールセット内に定義します。

    #sidebar {
      a { text-decoration: none; }
    }

Of course, white space is insignificant
and the last trailing semicolon is optional
so you can also do it like this:

もちろん、空白文字は意味をなさないですし、末尾のセミコロンはオプションなので次のように記述することも可能です。

    #sidebar { a { text-decoration: none } }

## Nested Properties

## ネストプロパティ

To nest properties,
simply create a new property set
after an existing property's colon:

プロパティを入れ子にするために、既存のプロパティコロンのアロに新しいプロパティセットを記述します。

    #footer {
      border: {
        width: 1px;
        color: #ccc;
        style: solid;
      }
    }

This compiles to:

これは以下のようにコンパイルされます。

    #footer {
      border-width: 1px;
      border-color: #cccccc;
      border-style: solid; }

## Mixins

## ミックスイン

A mixin is declared with the `@mixin` directive:

ミックスインは`@mixin`ディレクティブを用いて宣言します。

    @mixin rounded($amount) {
      -moz-border-radius: $amount;
      -webkit-border-radius: $amount;
      border-radius: $amount;
    }

A mixin is used with the `@include` directive:

ミックスインは`@include`ディレクティブを用いて使用します。

    .box {
      border: 3px solid #777;
      @include rounded(0.5em);
    }

This syntax is also available in the indented syntax,
although the old `=` and `+` syntax still works.

この構文はインデント構文上でも使用することが可能で、`=` と `+`の古い構文が機能します。

This is rather verbose compared to the `=` and `+` characters used in Sass syntax.
This is because the SCSS format is designed for CSS compatibility rather than conciseness,
and creating new syntax when the CSS directive syntax already exists
adds new syntax needlessly and
could create incompatibilities with future versions of CSS.

Sass構文上で使用される`=` と `+`文字と比べれば、いくぶんか冗長な書き方かもしれません。これはSCSSフォーマットは簡潔さよりもCSSとの互換性を重視して設計されているためです。また、CSSディレクティブがすでに存在しているのにも関わらず、新しい構文を作ってしまうのは、不必要な構文を追加することを意味し、未来のバージョンのCSSとの前方互換性を持てなくなる可能性があります。

## Comments

## コメント

Like Sass, SCSS supports both comments that are preserved in the CSS output
and comments that aren't.
However, SCSS's comments are significantly more flexible.
It supports standard multiline CSS comments with `/* */`,
which are preserved where possible in the output.
These comments can have whatever formatting you like;
Sass will do its best to format them nicely.

Sass同様に、SCSSはコンパイルしたCSS上でも保持されるコメントと保持されないコメント両方共にサポートしています。しかしながら、SCSSのコメントはいくぶんか複雑です。標準の複数行CSSコメント`/* */`をサポートし、それはコンパイルされたCSS上でも保持されます。このコメントはあなたが自由にフォーマットを決めることができ、Sassはそのフォーマットをうまく出力します。

SCSS also uses `//` for comments that are thrown away, like Sass.
Unlike Sass, though, `//` comments in SCSS may appear anywhere
and last only until the end of the line.

SCSSもSass同様に、`//`コメントを使用でき、そのコメントは出力されません。Sassとの違いはSCCSでは`//`コメントはどこでも記述可能で、最終行でもコメントアウトすることができます。

For example:

例：

    /* This comment is
     * several lines long.
     * since it uses the CSS comment syntax,
     * it will appear in the CSS output. */
    body { color: black; }

    // These comments are only one line long each.
    // They won't appear in the CSS output,
    // since they use the single-line comment syntax.
    a { color: green; }

    /* このコメントは
     * 複数行で長いコメントです。
     * CSSコメント構文で記述されているので、
     * コンパイルされたCSS上でも存在します。 */
    body { color: black; }

    // これらのコメントは長い一行コメントなだけです。
    // 単数行コメント構文で記述されているので、
    // コンパイルされたCSS上には出力されません。
    a { color: green; }

is compiled to:

これはこのようにコンパイルされます。

    /* This comment is
     * several lines long.
     * since it uses the CSS comment syntax,
     * it will appear in the CSS output. */
    body {
      color: black; }

    a {
      color: green; }

    /* このコメントは
     * 複数行で長いコメントです。
     * CSSコメント構文で記述されているので、
     * コンパイルされたCSS上でも存在します。 */
    body {
      color: black; }

    a {
      color: green; }

## `@import`

The `@import` directive in SCSS functions just like that in Sass,
except that it takes a quoted string to import.
For example, this Sass:

インポートするために引用符で囲うことを除いて、SCCS関数における`@import`ディレクティブはまさにSassにおけるそれと同じものです。
例、Sassの場合：

    @import themes/dark
    @import font.sass

SCSSの場合：

    @import "themes/dark";
    @import "font.sass";