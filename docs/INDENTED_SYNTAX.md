+  元文書: [https://github.com/nex3/sass/blob/stable/doc-src/INDENTED_SYNTAX.md](https://github.com/nex3/sass/blob/stable/doc-src/INDENTED_SYNTAX.md)

# Sass インデント構文

* 目次
{:toc}

Sassのインデント構文（単に”Sass”として知られる）は、CSSベースのSCSS構文に代わるものとして、より簡潔に、より美しく記述できるように設計されています。ただ、CSSとの互換性はありません。スタイルブロックを区切るために`{`と`}` を使用する代わりに、インデントを使用し、プロパティはセミコロンで区切る代わりに改行を使用します。これにより冗長な表現を避けることができ、確実に文字量を減らすことができます。

プロパティ宣言やセレクタのようなSassにおける各ステートメントは、一行ごとに記述しなければなりません。さらに`{`と`}`内のステートメントは改行しインデントを一つ深くしなければなりません。
例えばこのCSSは、

    #main {
      color: blue;
      font-size: 0.3em;
    }

Sassだとこのようになります。

    #main
      color: blue
      font-size: 0.3em

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

Sassだとこのようになります。

    #main
      color: blue
      font-size: 0.3em

      a
        font:
          weight: bold
          family: serif
        &:hover
          background-color: #eee

## Sass構文の違い

セミコロンを改行に、ブレースをインデントにすれば、ほとんどのCSSとSCSSはSass構文として問題なく機能します。しかし、いくつかのケースにおいて差異や微妙な点がいくつかありますので、それは後述します。

## プロパティ構文

インデント構文は2種類のプロパティ宣言に対応しています。最初のひとつはCSSとセミコロンを除いて全く同じです。もうひとつはコロンを使用しますが、それはプロパティ名の前に記述する方法です。
例：

    #main
      :color blue
      :font-size 0.3em

デフォルトで両方とも使用できますが、{file:SASS_REFERENCE.md#property_syntax-option `:property_syntax`オプション}はどちらかひとつのプロパティ構文しか指定できません。

### 複数行セレクタ

インデント構文では通常、ひとつのセレクタはひとつの行に記述しますが、例外があります。コンマを記述すればその後で改行できます。
例：

    .users #userTab,
    .posts #postTab
      width: 100px
      height: 30px

### コメント

インデント構文のすべての文法同様にコメントもまた行単位です。これはSCSS構文上では同じように機能しないことを意味しています。インデント構文のコメントは行全体を必要とし、インデントされたすべてのテキストを含みます。

SCSS構文同様に、インデント構文もまた2種類のコメントをサポートしています。`/*`で始まるコメントはコンパイルされたCSS上でも出力されますが、SCSSと違うのは`*/`を必要としないことです。`//`で始まるコメントはコンパイルされると完全に削除されます。
例：

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

このようにコンパイルされます。

    /* このコメントはコンパイルされたCSS上に出力されます。
     * この行はインデントされているので、
     * コメントの一部です。 */
    body {
      color: black; }

    a {
      color: green; }

### `@import`

Sass構文の`@import`ディレクティブは引用符を必要としませんが、使用しても構いません。
SCSSの例：

    @import "themes/dark";
    @import "font.sass";

Sassだとこのように書きます。

    @import themes/dark
    @import font.sass

### ミックスインディレクティブ

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

以下と同じ意味です。

    @mixin large-text
      font:
        family: Arial
        size: 20px
        weight: bold
      color: #ff0000

    h1
      @include large-text

## 廃止予定の構文

インデント構文は結構な歴史があるため、バージョンの変遷で変わった構文が存在します。しかし、いくつかの古い構文は今も機能します。ここではそのような構文をまとめています。

**新しくSassファイルを作成する際に、これらの構文は非推奨です。** もし使用すればWarningが出力されるようになっていますし、新バージョンでは削除される可能性があります。

### プロパティと変数のための`=`

変数を定義するとき、またはSassScriptの値にプロパティを定義するとき、`:`ではなく`=`が使用されていました。これは`:`を使用するよりも意味的に合っていませんし、詳細は{file:SASS_CHANGELOG.md#3-0-0-sass-script-context 変更ログ}を参照してください。

### デフォルトの変数のための`||=`

変数のデフォルト値を定義するときに、`:`ではなく`||=`が使用されていました。また、`!default`フラグは使用されていませんでした。変数の値は`=`と同じ意味を持っています。詳細は{file:SASS_CHANGELOG.md#3-0-0-sass-script-context 変更ログ}を参照してください。

### 変数のための接頭辞`!`

`!`は`$`の代わりに変数の接頭辞として使用されていました。これによる機能的差異は生じません。ただ純粋に美的な変更です。