+  元文書: [https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#-rules-and-directives-directives](https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#-rules-and-directives-directives)

## `@`-ルールとディレクティブ {#directives}

Sassは、CSS3の全ての `@` ルールだけではなく "ディレクティブ" として知られるSass固有の追加機能もサポートしています。
詳しくは後述しますが、ディレクティブはSassの中で様々な効果を持っています。
[コントロールディレクティブ](#control_directives)と[ミックスインディレクティブ](#mixins)を参照して下さい。

### `@import` {#import}

Sassは、SCSSとSassのファイルをインポートできるようにするために、CSSの `@import` ルールを拡張しています。
インポートされた全てのSCSSとSassファイルは、一つのCSS出力ファイルに一緒にマージされます。
さらに、インポートされたファイルで定義されている任意の変数や[ミックスイン](#mixins)はメインファイルで使うことができます。

Sassは、現在のディレクトリ内とRack、RailsやMerbのSassファイルディレクトリ内の他のSassファイルを探します。
[`:load_paths`](#load_paths-option)を使って指定するか、
コマンドラインで `--load-path` オプションで指定することで探索するディレクトリを追加することもできます。

`@import` は、インポートするためのファイル名を取ります。
デフォルトで、直接インポートするSassファイルを探しますが、
CSSの `@import` ルールでコンパイルされるための条件がいくつかあります。

* ファイルの拡張子が `.css` の場合。
* ファイル名が `http://` で始まる場合。
* ファイル名が `url()` の場合。
* `@import` にメディアクエリーがある場合。

上記のいずれの条件も満たさず、拡張子が `.scss` や `.sass` の場合、
そのファイル名のSassファイルやSCSSファイルがインポートされます。
拡張子がない場合、Sassはそのファイル名で `.scss` や `.sass` の拡張子のファイルを探して、インポートしようとします。

例えば、

    @import "foo.scss";

もしくは、

    @import "foo";

どちらも `foo.scss` というファイルをインポートしようとします。
それに対して、

    @import "foo.css";
    @import "foo" screen;
    @import "http://foo.com/bar";
    @import url(foo);

これら全ては、以下のようにコンパイルされます。

    @import "foo.css";
    @import "foo" screen;
    @import "http://foo.com/bar";
    @import url(foo);

`@import` で一度に複数のファイルをインポートすることも可能です。
例えば、

    @import "rounded-corners", "text-shadow";

`rounded-corners` と `text-shadow` のファイルの両方をインポートしようとします。

インポートの際に `#{}` での挿入を含めることができますが、一定の制限があります。
変数を使ってSassファイルを動的にインポートすることはできません。
挿入はCSSをインポートすることしかできません。
このように、挿入は `url()` インポートと同時に使われた時だけ処理が行われます。
例えば、

    $family: unquote("Droid+Sans");
    @import url("http://fonts.googleapis.com/css?family=\#{$family}");

これは以下のようにコンパイルされます。

    @import url("http://fonts.googleapis.com/css?family=Droid+Sans");

#### パーシャル {#partials}

インポートしたいが、CSSファイルにコンパイルしたくないSCSSファイルやSassファイルがある場合、
ファイル名の最初にアンダースコアを付けます。
こうすることで、アンダースコアを付けたファイルをCSSファイルにコンパイルしないようにSassに指示します。
インポートする際は、アンダースコアを付けずにこれらのファイルをインポートします。

例えば、 `_colors.scss` があるとします。
それから `_colors.css` というファイルを作成したくない場合、以下のようにします。

    @import "colors";

そして、 `_colors.scss` はインポートされます。

同じ名前のパーシャルファイルとパーシャルファイルではないファイルを同じディレクトリにないように注意して下さい。
例えば `_colors.scss` は、 `colors.scss` と同じディレクトリに置かないようにして下さい。

#### ネストされた `@import` {#nested-import}

ドキュメントの最上位レベルに `@import` を定義することが一番使いやすい場合が多いですが、
CSSのルール内や `@media` ルール内に @import を含めることができます。
最上位レベルで `@import` した時のように、 `@import` されたファイルの内容を含んでいます。
しかし、インポートされたルールは、元の `@import` と同じ場所でネストされます。

例えば、 `example.scss` に以下のものが定義されている場合、

    .example {
      color: red;
    }

それから

    #main {
      @import "example";
    }

これは以下のようにコンパイルされます。

    #main .example {
      color: red;
    }

`@mixin` や `@charset` のようなディレクティブは、ドキュメントの最上位レベルでしか定義することができず、
これらのディレクティブは、ネストされたコンテキスト内でファイルを `@import` させて定義することができません。

ミックスインやコントロールディレクティブ内で `@import` をネストさせて使うことはできません。

### `@media` {#media}

Sassの `@media` ディレクティブはCSSと同じように振る舞いますが、一つだけ追加された機能があります。
`@media` ディレクティブはCSSのルール内でネストさせることができるという機能です。
`@media` ディレクティブはCSSのルール内で記述されている場合、
スタイルシートの最上位レベルに上げられ、そのルールの中に全てのセレクタを定義します。
このことにより、セレクタを繰り返したり、スタイルシートの定義順を壊すことなくmediaを定義するスタイルを追加することが簡単にできます。
例は以下のとおりです。

    .sidebar {
      width: 300px;
      @media screen and (orientation: landscape) {
        width: 500px;
      }
    }

以下のようにコンパイルされます。

    .sidebar {
      width: 300px; }
      @media screen and (orientation: landscape) {
        .sidebar {
          width: 500px; } }

`@media` クエリも別の `@media` クエリをネストすることが可能です。
クエリは `and` オペレーターを使って連結されます。
例は以下のとおりです。

    @media screen {
      .sidebar {
        @media (orientation: landscape) {
          width: 500px;
        }
      }
    }

以下のようにコンパイルされます。

    @media screen and (orientation: landscape) {
      .sidebar {
        width: 500px; } }

最後に、`@media` クエリには、特性の名前や特性の値の場所にSassScript式(変数、関数、オペレーターを含む)を入れることができます。
例は以下のとおりです。

    $media: screen;
    $feature: -webkit-min-device-pixel-ratio;
    $value: 1.5;

    @media #{$media} and ($feature: $value) {
      .sidebar {
        width: 500px;
      }
    }

以下のようにコンパイルされます。

    @media screen and (-webkit-min-device-pixel-ratio: 1.5) {
      .sidebar {
        width: 500px; } }

### `@extend` {#extend}

ページのデザインをしている時に、あるクラスに別のクラスの全てのスタイルを固有のスタイルとして持たせたい場合が時々あります。
これを実装する最も一般的な方法は、より一般的なクラスを両方に使って、HTMLにより固有なクラスを使うことです。
例えば、標準的なエラーと深刻なエラーをデザインすると仮定します。
以下のようにマークアップするとします。

    <div class="error seriousError">
      Oh no! You've been hacked!
    </div>

そして、スタイルは以下のようになります。

    .error {
      border: 1px #f00;
      background-color: #fdd;
    }
    .seriousError {
      border-width: 3px;
    }

残念なことに、これは `.seriousError` と `.error` を同時に使うことを覚えておかなければいけません。
これはメンテナンス性が良くなくて、複雑なバグの原因となり、マークアップがセマンティックではなくなる危険があります。

`@extend` ディレクティブは、あるセレクタが別のセレクタのスタイルを継承するようにSassに指示することで、
これらの問題を避ける事ができます。
例は以下のとおりです。

    .error {
      border: 1px #f00;
      background-color: #fdd;
    }
    .seriousError {
      @extend .error;
      border-width: 3px;
    }

これは、 `.error` に定義されている全てのスタイルは、 `.seriousError` にも適用されていて、
それに加えて `.seriousError` に固有のスタイルを適用することを意味しています。
要するに、 `.seriousError` クラスを持つ全ての要素は、 `.error` クラスのスタイルも指定されています。

`.error` を使う他のルールは、 `.seriousError` と同じように処理されます。
例えば、ハッカーによって引き起こされたエラーに対して特別なスタイルがある場合は以下のようになります。

    .error.intrusion {
      background-image: url("/image/hacked.png");
    }

そして、 `<div class="seriousError intrusion">` は同じように背景画像に `hacked.png` が定義されています。

#### どのように動作するか

`@extend` は、スタイルシートのどこかに記述されている拡張されるセレクタ(例、 `.error`)に、
拡張するセレクタ(例、 `.seriousError`)を挿入することで処理されます。
よって、上の例は以下のようになります。

    .error {
      border: 1px #f00;
      background-color: #fdd;
    }
    .error.intrusion {
      background-image: url("/image/hacked.png");
    }
    .seriousError {
      @extend .error;
      border-width: 3px;
    }

これは以下のようにコンパイルされます。

    .error, .seriousError {
      border: 1px #f00;
      background-color: #fdd; }

    .error.intrusion, .seriousError.intrusion {
      background-image: url("/image/hacked.png"); }

    .seriousError {
      border-width: 3px; }

セレクタをマージする時、 `@extend` は不必要な重複を避けるために賢く振る舞い、
`.seriousError.seriousError` のようなものは `.seriousError` に変換されます。
その上、 `#main#footer` のように何も一致しないセレクタを生成することはありません。

#### 複雑なセレクタの拡張

クラスセレクタは拡張できるだけではありません。
`.special.cool` 、 `a:hover` 、 `a.user[href^="http://"]` のような一つの要素しか関与していない任意のセレクタを拡張することが可能です。
例は以下のとおりです。

    .hoverlink {
      @extend a:hover;
    }

クラスの時と同じように、これは `a:hover` に適用された全てのスタイルは `.hoverlink` にも適用されます。
例は以下のとおりです。

    .hoverlink {
      @extend a:hover;
    }
    a:hover {
      text-decoration: underline;
    }

これは以下のようにコンパイルされます。

    a:hover, .hoverlink {
      text-decoration: underline; }

上述した `.error.intrusion` の時と同じように、 `a:hover` で使われているルールは、
他のセレクタと同じように `.hoverlink` にも適用されます。
例は以下のとおりです。

    .hoverlink {
      @extend a:hover;
    }
    .comment a.user:hover {
      font-weight: bold;
    }

これは以下のようにコンパイルされます。

    .comment a.user:hover, .comment .user.hoverlink {
      font-weight: bold; }

#### 複数の拡張

一つのセレクタは、一つ以上のセレクタを拡張することができます。
これは拡張したセレクタの全てのスタイルを継承するということです。
例は以下のとおりです。

    .error {
      border: 1px #f00;
      background-color: #fdd;
    }
    .attention {
      font-size: 3em;
      background-color: #ff0;
    }
    .seriousError {
      @extend .error;
      @extend .attention;
      border-width: 3px;
    }

これは以下のようにコンパイルされます。

    .error, .seriousError {
      border: 1px #f00;
      background-color: #fdd; }

    .attention, .seriousError {
      font-size: 3em;
      background-color: #ff0; }

    .seriousError {
      border-width: 3px; }

要するに、 `.seriousError` クラスを持つ全ての要素は、
`.error` クラス *と* `.attention` クラスで定義されているものを全てを持っています。
よって、ドキュメントの後半で定義されたスタイルが優先され、
`.seriousError` の背景色は、 `.attention` が `.error` よりも後ろで定義されているので、 `#fdd` ではなく `#ff0` になります。

複数の拡張は、コンマで分割されたセレクタのリストを使って記述することもできます。
例えば、 `@extend .error, .attention` は `@extend .error; @extend.attention` と同じです。

#### 拡張を連鎖させる

あるセレクタが別のセレクタを、順番に拡張していくことが可能です。
例は以下のとおりです。

    .error {
      border: 1px #f00;
      background-color: #fdd;
    }
    .seriousError {
      @extend .error;
      border-width: 3px;
    }
    .criticalError {
      @extend .seriousError;
      position: fixed;
      top: 10%;
      bottom: 10%;
      left: 10%;
      right: 10%;
    }

`.seriousError` クラスを持つ全ての要素は、 `.error` クラスのスタイルも定義されていて、
`.criticalError` クラスを持つ全ての要素は、 `.seriousError` クラス *と* 、 `.error` クラスのスタイルも定義されています。
以下のようにコンパイルされます。

    .error, .seriousError, .criticalError {
      border: 1px #f00;
      background-color: #fdd; }

    .seriousError, .criticalError {
      border-width: 3px; }

    .criticalError {
      position: fixed;
      top: 10%;
      bottom: 10%;
      left: 10%;
      right: 10%; }

#### セレクタシーケンス

`.foo .bar` や `.foo + .bar` のようなセレクタシーケンスは、現在拡張することができません。
しかし、ネストされたセレクタ自体に `@extend` を使うことはできます。
例は以下のとおりです。

    #fake-links .link {
      @extend a;
    }

    a {
      color: blue;
      &:hover {
        text-decoration: underline;
      }
    }

以下のようにコンパイルされます。

    a, #fake-links .link {
      color: blue; }
      a:hover, #fake-links .link:hover {
        text-decoration: underline; }

##### セレクタシーケンスのマージ

時々セレクタシーケンスは別のシーケンスで記述されている他のセレクタを拡張することがあります。
今回のケースでは、2つのシーケンスをマージする必要があるとします。
例は以下のとおりです。

    #admin .tabbar a {
      font-weight: bold;
    }
    #demo .overview .fakelink {
      @extend a;
    }

いずれかのシーケンスと一致する可能性がある全てのセレクタを生成することは技術的に可能ですが、スタイルシートが大きくなりすぎます。
上の簡単な例では、例えば、10個のセレクタを要求されます。
代わりに、Sassは使う可能性があるセレクタだけ出力します。

2つのシーケンスがマージされる時に共通するセレクタがない場合、2つの新しいセレクタが生成されます。
2つ目のシーケンスの前に1つ目のシーケンスが追加されたものと、
1つ目のシーケンスの前に2つ目のシーケンスが追加されものです。
例は以下のとおりです。

    #admin .tabbar a {
      font-weight: bold;
    }
    #demo .overview .fakelink {
      @extend a;
    }

以下のようにコンパイルされます。

    #admin .tabbar a,
    #admin .tabbar #demo .overview .fakelink,
    #demo .overview #admin .tabbar .fakelink {
      font-weight: bold; }

2つのシーケンスがいくつかのセレクタを共有している場合、
これらのセレクタは一緒にマージされ、異なる部分(いずれかがまだ存在する場合)だけ別に出力されます。
この例では、両方のシーケンスに `#admin` というIDを含んでいるので、出力されるセレクタは2つのIDをマージします。

    #admin .tabbar a {
      font-weight: bold;
    }
    #admin .overview .fakelink {
      @extend a;
    }

これは以下のようにコンパイルされます。

    #admin .tabbar a,
    #admin .tabbar .overview .fakelink,
    #admin .overview .tabbar .fakelink {
      font-weight: bold; }

#### セレクタのみの `@extend` {#placeholders}

`@extend` したいが、HTMLで直接使わないスタイルを一つのクラスに書こうとすることがあるでしょう。
これは、必要な時は `@extend` して、必要ない時は無視するスタイルをユーザーに提供しているようなSassのライブラリを書いている時は特にそうです。

このような標準的なクラスを使う場合、スタイルシートが生成される時にたくさんのCSSが生成することになり、
HTMLで既に使われている他のクラスと重複するリスクがあります。
このことが、Sassが「プレースホルダーセレクタ」(例、 `%foo`)をサポートする理由です。

プレースホルダーセレクタは、 `#` や `.` が `%` に置き換えられていること以外は、クラスやIDセレクタのようなものです。
プレースホルダーセレクタは、クラスやIDのようにどこでも使うことができ、
プレースホルダーセレクタ自体のルールセットがCSSに出力されることはありません。
例は以下のとおりです。

    // このルールセット自体は出力されません。
    #context a%extreme {
      color: blue;
      font-weight: bold;
      font-size: 2em;
    }

しかし、プレースホルダーセレクタは、クラスやIDのように拡張することができます。
拡張したセレクタは生成されますが、元にしたプレースホルダーセレクタは生成されません。
例は以下のとおりです。

    .notice {
      @extend %extreme;
    }

以下のようにコンパイルされます。

    #context a.notice {
      color: blue;
      font-weight: bold;
      font-size: 2em; }

#### `!optional` フラグ

通常は、セレクタを拡張する時に `@extend` が正しく動作しない場合はエラーになります。
例えば、 `a.important {@extend .notice}` と記述した時に `.notice` を含むセレクタがない場合エラーになります。
`.notice` を含むセレクタが `h1.notice` だけの時も、 `h1` は `a` とコンフリクトを起こし、
新しいセレクタが生成されないのでエラーになります。

しかし、新しいセレクタを生成しない `@extend` を定義したい場合も時々あります。
このようにするには、セレクタの後に `!optional` フラグを追加するだけです。
例は以下のとおりです。

    a.important {
      @extend .notice !optional;
    }

#### ディレクティブ内の `@extend`

`@media` のようなディレクティブ内で、 `@extend` を使うにはいくつかの制限があります。
Sassは、あらゆる場所にスタイルをコピーすることにより膨大な量のスタイルシートが作成されないように、
`@media` ブロックの外側のCSSルールに `@media` 内のセレクタを適用することができません。
これは、 `@media` 内(もしくは他のCSSディレクティブ内)で `@extend` を使う場合、
同じディレクティブブロック内で記述されているセレクタだけ拡張することができるということです。

例として、以下のコードは実行されます。

    @media print {
      .error {
        border: 1px #f00;
        background-color: #fdd;
      }
      .seriousError {
        @extend .error;
        border-width: 3px;
      }
    }

しかし、以下のコードはエラーになります。

    .error {
      border: 1px #f00;
      background-color: #fdd;
    }

    @media print {
      .seriousError {
        // INVALID EXTEND: .error is used outside of the "@media print" directive
        @extend .error;
        border-width: 3px;
      }
    }

いつか `@extend` が `@media` や他のディレクティブ内で使うことができるように、
ブラウザに標準でサポートされることを望んでいます。

### `@debug`

`@debug` ディレクティブは、SassScript式の値を標準エラー出力をストリームで出力します。
SassScriptが複雑になってきた際のSassファイルのデバッグの際に便利です。
例は以下のとおりです。

    @debug 10em + 12em;

これは以下のように出力されます。

    Line 1 DEBUG: 22em

### `@warn`

`@warn` ディレクティブは、SassScript式の値を標準エラー出力をストリームで出力します。
非推奨の機能を使った場合にユーザーに警告を出したり、
マイナーなミックスインの間違った使い方をした場合に警告を出して正しく使ってもらえるようにする等の
ライブラリには便利な機能です。
`@warn` と `@debug` の主な違いは2つあります。

1. `--quiet` コマンドラインオプションか `:quiet` Sassオプションで、警告を非表示にすることができます。
2. 警告の原因となったスタイルの場所を確認することができるように、
   スタイルシートの解析結果がメッセージと一緒に出力されます。

使い方の例は以下のとおりです。

    @mixin adjust-location($x, $y) {
      @if unitless($x) {
        @warn "Assuming #{$x} to be in pixels";
        $x: 1px * $x;
      }
      @if unitless($y) {
        @warn "Assuming #{$y} to be in pixels";
        $y: 1px * $y;
      }
      position: relative; left: $x; top: $y;
    }
