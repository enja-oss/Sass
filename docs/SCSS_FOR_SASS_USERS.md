+  元文書: [https://github.com/nex3/sass/blob/stable/doc-src/SCSS_FOR_SASS_USERS.md](https://github.com/nex3/sass/blob/stable/doc-src/SCSS_FOR_SASS_USERS.md)

# SassユーザーのためのSCSS導入

Sass3は完全にCSS3と互換性のあるSCSSと呼ばれる新しい構文を導入しました。一方で、Sass構文のサポートも今後も変わらず続けていきます。文法的に妥当なCSS3のすべてのスタイルシートはSCSSにおいてもすべて文法的に妥当ということを意味しています。加えて、SCSSは多くのCSSハックと[IEの古い`filter`構文](http://msdn.microsoft.com/en-us/library/ms533754%28VS.85%29.aspx)のようなベンダー特有の構文も解釈します。

SCSSはCSSを拡張したものなので、CSS上で動作するものはすべてSCSS上でも動作します。これは、SassユーザーにとってSCCSを理解するためには、Sass拡張がどのように機能しているか理解するだけで良いということを意味しています。これらの多く、つまり、変数、親参照、ディレクティブのようなものは同じように機能します。SassとSCSSの違いというのは、改行とインデントの代わりにセミコンロンとブラケットを必要とするかしないかだけです。

例えば、次のようにSassで書かれたシンプルな規則があります。

    #sidebar
      width: 30%
      background-color: #faa

これはブラケットとセミコロンを追加するだけでSCSSに変換できます。

    #sidebar {
      width: 30%;
      background-color: #faa;
    }

さらに、SCSSは空白文字を完全に無視します。つまり上記のコードは以下のようにも記述することができます。

    #sidebar {width: 30%; background-color: #faa}

多少、複雑な違いがいくつかありますが、後述を参照してください。SCSSは{file:SASS_CHANGELOG.md#3-0-0-syntax-changes Sass3におけるすべての構文変更}を取り扱っているので、次に進む前に確認しておいてください。

## ネストセレクタ

セレクタを入れ子にするために、新しいルールセットを既存のルールセット内に定義します。

    #sidebar {
      a { text-decoration: none; }
    }

もちろん、空白文字は意味をなさないですし、末尾のセミコロンはオプションなので次のように記述することも可能です。

    #sidebar { a { text-decoration: none } }

## ネストプロパティ

プロパティを入れ子にするには、今あるプロパティのコロンの後に新しいプロパティセットを記述します。

    #footer {
      border: {
        width: 1px;
        color: #ccc;
        style: solid;
      }
    }

これは以下のようにコンパイルされます。

    #footer {
      border-width: 1px;
      border-color: #cccccc;
      border-style: solid; }

## ミックスイン

ミックスインは`@mixin`ディレクティブを用いて宣言します。

    @mixin rounded($amount) {
      -moz-border-radius: $amount;
      -webkit-border-radius: $amount;
      border-radius: $amount;
    }

ミックスインは`@include`ディレクティブを用いて使用します。

    .box {
      border: 3px solid #777;
      @include rounded(0.5em);
    }

この構文はSass構文でも利用できますが、古い`=`と`+`の構文も引き続き利用できます。

Sass構文上で使用される`=` と `+`文字と比べれば、いくぶんか冗長な書き方かもしれません。これはSCSSフォーマットは簡潔さよりもCSSとの互換性を重視して設計されているためです。また、CSSディレクティブがすでに存在しているのにも関わらず、新しい構文を作ってしまうのは、不必要な構文を追加することを意味し、未来のバージョンのCSSとの前方互換性を持てなくなる可能性があります。

## コメント

Sass同様に、SCSSはコンパイルしたCSS上でも保持されるコメントと保持されないコメント両方共にサポートしています。しかしながら、SCSSのコメントはより柔軟です。標準の複数行CSSコメント`/* */`をサポートし、それはコンパイルされたCSS上でも保持されます。このコメントはあなたが自由にフォーマットを決めることができ、Sassはそのフォーマットをうまく出力します。

SCSSもSass同様に、`//`コメントを使用でき、そのコメントは出力されません。Sassとの違いはSCCSでは`//`コメントは最初の行だけでなくコメントアウトしたいすべての行に`//`を記述しなければなりません。

例：

    /* このコメントは
     * 複数行で長いコメントです。
     * CSSコメント構文で記述されているので、
     * コンパイルされたCSS上でも存在します。 */
    body { color: black; }

    // これらのコメントは長い一行コメントなだけです。
    // 単数行コメント構文で記述されているので、
    // コンパイルされたCSS上には出力されません。
    a { color: green; }

これはこのようにコンパイルされます。

    /* このコメントは
     * 複数行で長いコメントです。
     * CSSコメント構文で記述されているので、
     * コンパイルされたCSS上でも存在します。 */
    body {
      color: black; }

    a {
      color: green; }

## `@import`

インポートするために引用符で囲うことを除いて、SCCS関数における`@import`ディレクティブはまさにSassにおけるそれと同じものです。
例、Sassの場合：

    @import themes/dark
    @import font.sass

SCSSの場合：

    @import "themes/dark";
    @import "font.sass";