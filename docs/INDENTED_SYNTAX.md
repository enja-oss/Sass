+  元文書: [https://github.com/nex3/sass/blob/stable/doc-src/INDENTED_SYNTAX.md](https://github.com/nex3/sass/blob/stable/doc-src/INDENTED_SYNTAX.md)

# Sass Indented Syntax

# Sass インデント構文

* Table of contents
{:toc}

* 目次
{:toc}

Sass's indented syntax (also known simply as "Sass")
is designed to provide a more concise
and, for some, more aesthetically appealing alternative
to the CSS-based SCSS syntax.
It's not compatible with CSS;
instead of using `{` and `}` to delimit blocks of styles,
it uses indentation,
and instead of using semicolons to separate statements it uses newlines.
This usually leads to substantially less text
when saying the same thing.

Sassのインデント構文（単に”Sass”として知られる）は、CSSベースのSCSS構文に代わるものとして、より簡潔に、より美しく記述できるように設計されています。ただ、CSSとの互換性はありません。スタイルブロックを区切るために`{`と`}` を使用する代わりに、インデントを使用し、プロパティはセミコロンで区切る代わりに改行を使用します。これにより冗長な表現を避けることができ、確実に文字量を減らすことができます。

Each statement in Sass, such as property declarations and selectors,
must be placed on its own line.
In addition, everything that would be within `{` and `}` after a statement
must be on a new line and indented one level deeper than that statement.
For example, this CSS:

プロパティ宣言やセレクタのようなSassにおける各ステートメントは、一行ごとに記述しなければなりません。さらに`{`と`}`内のステートメントは改行しインデントを一つ深くしなければなりません。
例えばこのCSSは、

    #main {
      color: blue;
      font-size: 0.3em;
    }

would be this Sass:
Sassだとこのようになる。

    #main
      color: blue
      font-size: 0.3em

Similarly, this SCSS:
同様にこのCSSは、

    #main {
      color: blue;
      font-size: 0.3em;

      a {
        font: {
          weight: bold;
          family: serif;
        }
        &:hover {
          background-color: #eee;
        }
      }
    }

would be this Sass:
Sassだとこのようになる。

    #main
      color: blue
      font-size: 0.3em

      a
        font:
          weight: bold
          family: serif
        &:hover
          background-color: #eee

## Sass Syntax Differences

## Sass構文の違い

In general, most CSS and SCSS syntax
works straightforwardly in Sass
by using newlines instead of semicolons
and indentation instead of braces.
However, there are some cases where there are differences or subtleties,
which are detailed below.

一般的に、セミコロンの代わりに改行、ブレスの代わりにインデントを使用すれば、ほとんどのCSSとSCSSはSass構文上でも問題なく機能します。しかし、いくつかのケースにおいて差異や微妙な点がいくつかありますので、それは後述します。

## Property Synax

## プロパティ構文

The indented syntax supports two ways of declaring CSS properties.
The first is just like CSS, except without the semicolon.
The second, however, places the colon *before* the property name.
For example:

インデント構文は2種類のプロパティ宣言に対応しています。最初のひとつはCSSとセミコロンを除いて全く同じです。もうひとつはコロンを使用しますが、それはプロパティ名の前に記述する方法です。
例：

    #main
      :color blue
      :font-size 0.3em

By default, both ways may be used.
However, the {file:SASS_REFERENCE.md#property_syntax-option `:property_syntax` option}
may be used to specify that only one property syntax is allowed.

デフォルトで両方とも使用出来ますが、{file:SASS_REFERENCE.md#property_syntax-option `:property_syntax`オプション}はどちらかひとつのプロパティ構文しか指定できません。

### Multiline Selectors

### 複数行セレクタ

Normally in the indented syntax, a single selector must take up a single line.
There is one exception, however:
selectors can contain newlines as long as they only appear after commas.
For example:

インデント構文では通常ひとつのセレクタはひとつの行で記述されますが、例外があります。コンマを記述すれば改行することができ複数のセレクタを記述することができます。
例：

    .users #userTab,
    .posts #postTab
      width: 100px
      height: 30px

### Comments

### コメント

Like everything else in the indented syntax,
comments are line-based.
This means that they don't work the same way as in SCSS.
They must take up an entire line,
and they also encompass all text nested beneath them.

インデント構文のすべての文法同様にコメントもまた行単位です。これはSCCS構文上では同じように機能しないことを意味しています。インデント構文のコメントは行全体を必要とし、インデントされたすべてのテキストを含みます。

Like SCSS, the indented syntax supports two kinds of comments.
Comments beginning with `/*` are preserved in the CSS output,
although unlike SCSS they don't require a closing `*/`.
Comments beginning with `//` are removed entirely.
For example:

SCSS構文同様に、インデント構文もまた2種類のコメントをサポートしています。`/*`で始まるコメントはコンパイルされたCSS上でも出力されますが、SCCSと違うのは`*/`を必要としないことです。`//`で始まるコメントはコンパイルされると完全に削除されます。
例：

    /* This comment will appear in the CSS output.
      This is nested beneath the comment,
      so it's part of it
    body
      color: black

    // This comment will not appear in the CSS output.
      This is nested beneath the comment as well,
      so it also won't appear
    a
      color: green

    /* このコメントはコンパイルされたCSS上に出力されます。
      この行はインデントされているので、
      コメントの一部です。
    body
      color: black

    // このコメントはコンパイルされたCSS上に出力されません。
      この行もまたインデントされているので、
      出力されません。
    a
      color: green


is compiled to:
このようにコンパイルされます。

    /* This comment will appear in the CSS output.
     * This is nested beneath the comment,
     * so it's part of it */
    body {
      color: black; }

    a {
      color: green; }

    /* このコメントはコンパイルされたCSS上に出力されます。
     * この行はインデントされているので、
     * コメントの一部です。 */
    body {
      color: black; }

    a {
      color: green; }

### `@import`

The `@import` directive in Sass does not require quotes, although they may be used.
For example, this SCSS:

Sass構文の`@import`ディレクティブは引用符を必要としませんが、使用しても構いません。
SCSSの例：

    @import "themes/dark";
    @import "font.sass";

would be this Sass:
Sassだとこのように書きます。

    @import themes/dark
    @import font.sass

### Mixin Directives

### ミックスインディレクティブ

Sass supports shorthands for the `@mixin` and `@include` directives.
Instead of writing `@mixin`, you can use the character `=`;
instead of writing `@include`, you can use the character `+`.
For example:

Sass構文は`@mixin`と`@include`ディレクティブのショートハンドをサポートしています。
`@mixin`と記述するかわりに、`=`と記述することができます。
`@include`と記述するかわりに、`+`と記述することができます。
例：

    =large-text
      font:
        family: Arial
        size: 20px
        weight: bold
      color: #ff0000

    h1
      +large-text

is the same as:
以下と同じ意味です。

    @mixin large-text
      font:
        family: Arial
        size: 20px
        weight: bold
      color: #ff0000

    h1
      @include large-text

## Deprecated Syntax

## 廃止予定の構文

Since the indented syntax has been around for a while,
previous versions have made some syntactic decisions
that have since been changed.
Some of the old syntax still works, though,
so it's documented here.

インデント構文が作られてから月日が流れているので、文法の変更を伴う旧バージョンが存在します。その古い文法は今なお機能しています。ドキュメントをこちらです。

**Note that this syntax is not recommended
for use in new Sass files**.
It will print a warning if it's used,
and it will be removed in a future version.

**新しくSassファイルを作成する際に、これらの構文は非推奨です。** もし使用すればWarningが出力されるようになっていますし、新バージョンでは削除される可能性があります。

### `=` for Properties and Variables

### プロパティと変数のための`=`

`=` used to be used instead of `:` when setting variables
and when setting properties to SassScript values.
It has slightly different semantics than `:`;
see {file:SASS_CHANGELOG.md#3-0-0-sass-script-context this changelog entry} for details.

変数を定義するとき、またはSassScriptの値にプロパティを定義するとき、`:`ではなく`=`が使用されていました。これは`:`を使用するよりも意味的に合っていませんし、詳細は{file:SASS_CHANGELOG.md#3-0-0-sass-script-context 変更ログ}を参照してください。

### `||=` for Default Variables

### デフォルトの変数のための`||=`

`||=` used to be used instead of `:` when setting the default value of a variable.
The `!default` flag was not used.
The variable value has the same semantics as `=`;
see {file:SASS_CHANGELOG.md#3-0-0-sass-script-context this changelog entry} for details.

変数のデフォルト値を定義するときに、`:`ではなく`||=`が使用されていました。また、`!default`フラグは使用されていませんでした。変数の値は`=`と同じ意味を持っています。詳細は{file:SASS_CHANGELOG.md#3-0-0-sass-script-context 変更ログ}を参照してください。

### `!` Prefix for Variables

### 変数のための接頭辞`!`

`!` used to be used as the variable prefix instead of `$`.
This had no difference in functionality;
it was a purely aesthetic change.

`!`は`$`の代わりに変数の接頭辞として使用されていました。これによる機能的差異は生じません。ただ純粋に美的な変更です。










