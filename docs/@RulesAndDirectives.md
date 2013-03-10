+  元文書: [https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#-rules-and-directives-directives](https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#-rules-and-directives-directives)

## `@`-Rules and Directives {#directives}

## `@`-ルールとディレクティブ {#directives}

Sass supports all CSS3 `@`-rules,
as well as some additional Sass-specific ones
known as "directives."
These have various effects in Sass, detailed below.
See also [control directives](#control_directives)
and [mixin directives](#mixins).

Sassは、ディレクティブとして知られているSass仕様の全てのCSS3の `@` ルールをサポートしています。
ディレクティブは、Sassでは様々な機能を持っていて、詳しくは下記のようになっています。
[コントロールディレクティブ](#control_directives)と[ミックスインディレクティブ](#mixins)を参照して下さい。

### `@import` {#import}

Sass extends the CSS `@import` rule
to allow it to import SCSS and Sass files.
All imported SCSS and Sass files will be merged together
into a single CSS output file.
In addition, any variables or [mixins](#mixins)
defined in imported files can be used in the main file.

Sassは、SCSSとSassのファイルをインポートできるようにするために、CSSの `@import` ルールを拡張しています。
インポートしたSCSSとSassの全てのファイルは、出力する一つのCSSファイルにマージされます。
さらに、インポートしたファイルで定義されている任意の変数や[ミックスイン](#mixins)はメインファイルで使うことができます。

Sass looks for other Sass files in the current directory,
and the Sass file directory under Rack, Rails, or Merb.
Additional search directories may be specified
using the [`:load_paths`](#load_paths-option) option,
or the `--load-path` option on the command line.

Sassは、現在のディレクトリ内や、Rack、Rails、Merbの管理下のディレクトリ内の他のSassファイルを探します。
[`:load_paths`](#load_paths-option)を使って指定したり、
コマンドラインで `--load-path` オプションで指定することで探索するディレクトリを追加することもできます。

`@import` takes a filename to import.
By default, it looks for a Sass file to import directly,
but there are a few circumstances under which it will compile to a CSS `@import` rule:

`@import` は、インポートするファイルのファイル名を受け取ります。
デフォルトで、直接インポートするSassファイルを探しますが、
CSSの `@import` ルールでコンパイルされるための条件がいくつかあります。

* If the file's extension is `.css`.
* If the filename begins with `http://`.
* If the filename is a `url()`.
* If the `@import` has any media queries.

* ファイルの拡張子が `.css` の場合。
* ファイル名が `http://` で始まる場合。
* ファイル名が `url()` の場合。
* `@import` にメディアクエリーがある場合。

If none of the above conditions are met
and the extension is `.scss` or `.sass`,
then the named Sass or SCSS file will be imported.
If there is no extension,
Sass will try to find a file with that name and the `.scss` or `.sass` extension
and import it.

上記のいずれの条件も満たさず、拡張子が `.scss` や `.sass` の場合、
そのファイル名のSassファイルやSCSSファイルがインポートされます。
拡張子がない場合、Sassはそのファイル名で `.scss` や `.sass` の拡張子のファイルを探して、インポートします。

For example,

例えば、

    @import "foo.scss";

or

もしくは、

    @import "foo";

would both import the file `foo.scss`,
whereas

どちらも `foo.scss` というファイルをインポートしようとします。
それに対して、

    @import "foo.css";
    @import "foo" screen;
    @import "http://foo.com/bar";
    @import url(foo);

would all compile to

これら全ては、以下のようにコンパイルされます。

    @import "foo.css";
    @import "foo" screen;
    @import "http://foo.com/bar";
    @import url(foo);

It's also possible to import multiple files in one `@import`. For example:

`@import` で一度に複数のファイルをインポートすることも可能です。
例えば、

    @import "rounded-corners", "text-shadow";

would import both the `rounded-corners` and the `text-shadow` files.

`rounded-corners` と `text-shadow` のファイルの両方をインポートしようとします。

Imports may contain `#{}` interpolation, but only with certain restrictions.
It's not possible to dynamically import a Sass file based on a variable;
interpolation is only for CSS imports.
As such, it only works with `url()` imports.
For example:

インポートの際に `#{}` というインターポレーションを含めることができますが、一定の制限があります。
変数を使ってSassファイルを動的にインポートすることはできません。
インターポレーションはCSSをインポートすることしかできません。
このように、インターポレーションは `url()` インポートと同時に使われた時だけ処理が行われます。
例えば、

    $family: unquote("Droid+Sans");
    @import url("http://fonts.googleapis.com/css?family=\#{$family}");

would compile to

これは以下のようにコンパイルされます。

    @import url("http://fonts.googleapis.com/css?family=Droid+Sans");

#### Partials {#partials}

#### パーシャル {#partials}

If you have a SCSS or Sass file that you want to import
but don't want to compile to a CSS file,
you can add an underscore to the beginning of the filename.
This will tell Sass not to compile it to a normal CSS file.
You can then import these files without using the underscore.

インポートしたいが、CSSファイルにコンパイルしたくないSCSSファイルやSassファイルがある場合、
ファイル名の最初にアンダースコアを付けます。
こうすることで、アンダースコアを付けたファイルをCSSファイルにコンパイルしないようにSassに指示します。
アンダースコアを付けずにこれらのファイルをインポートすることはできます。

For example, you might have `_colors.scss`.
Then no `_colors.css` file would be created,
and you can do

例えば、 `_colors.scss` があるとします。
それから `_colors.css` というファイルを作成したくない場合、以下のようにします。

    @import "colors";

and `_colors.scss` would be imported.

そして、 `_colors.scss` はインポートされます。

Note that you may not include a partial and a non-partial with the same name in
the same directory. For example, `_colors.scss` may not exist alongside
`colors.scss`.

同じディレクトリに同じ名前のパーシャルファイルとパーシャルファイルではないファイルを含ませたくない場合は注意が必要です。
例えば `_colors.scss` は、 `colors.scss` と同じディレクトリに一緒に存在しない場合があります。

#### Nested `@import` {#nested-import}

#### ネストされた `@import` {#nested-import}

Although most of the time it's most useful to just have `@import`s
at the top level of the document,
it is possible to include them within CSS rules and `@media` rules.
Like a base-level `@import`, this includes the contents of the `@import`ed file.
However, the imported rules will be nested in the same place as the original `@import`.

ドキュメントの最上位レベルに `@import` を定義することが一番使いやすい場合が多いですが、
CSSルールや `@media` ルール内に @import を含めることができます。
ベースレベルの `@import` のように、これは `@import` されたファイルの内容が含まれています。
しかし、インポートされたルールは、元の `@import` と同じ場所でネストされます。

For example, if `example.scss` contains

例えば、 `example.scss` に以下のものが定義されている場合、

    .example {
      color: red;
    }

then

それから

    #main {
      @import "example";
    }

would compile to

これは以下のようにコンパイルされます。

    #main .example {
      color: red;
    }

Directives that are only allowed at the base level of a document,
like `@mixin` or `@charset`, are not allowed in files that are `@import`ed
in a nested context.

`@mixin` や `@charset` のようなディレクティブは、ドキュメントのベースレベルでしか定義することができません。
これらのディレクティブは、ネストされたコンテキストで `@import` されたファイルを定義することができません。

It's not possible to nest `@import` within mixins or control directives.

ミックスインやコントロールディレクティブ内で `@import` をネストすることはできません。

### `@media` {#media}

`@media` directives in Sass behave just like they do in plain CSS,
with one extra capability: they can be nested in CSS rules.
If a `@media` directive appears within a CSS rule,
it will be bubbled up to the top level of the stylesheet,
putting all the selectors on the way inside the rule.
This makes it easy to add media-specific styles
without having to repeat selectors
or break the flow of the stylesheet.
For example:

Sassの `@media` ディレクティブは追加された機能がありますが、CSSと同じように振る舞います。
`@media` ディレクティブはCSSルールでネストさせることができます。
`@media` ディレクティブはCSSルールの中で定義されている場合、
スタイルシートの上位レベルに上げられ、そのルールの中に全てのセレクタを定義します。
これは、セレクタを繰り返したり、スタイルシートの定義順を壊すことなくmedia仕様のスタイルを追加することが簡単にできます。
例は以下のとおりです。

    .sidebar {
      width: 300px;
      @media screen and (orientation: landscape) {
        width: 500px;
      }
    }

is compiled to:

以下のようにコンパイルされます。

    .sidebar {
      width: 300px; }
      @media screen and (orientation: landscape) {
        .sidebar {
          width: 500px; } }

`@media` queries can also be nested within one another.
The queries will then be combined using the `and` operator.
For example:

`@media` クエリも別の `@media` クエリをネストすることが可能です。
クエリは `and` オペレーターを使って連結することができます。
例は以下のとおりです。

    @media screen {
      .sidebar {
        @media (orientation: landscape) {
          width: 500px;
        }
      }
    }

is compiled to:

以下のようにコンパイルされます。

    @media screen and (orientation: landscape) {
      .sidebar {
        width: 500px; } }

Finally, `@media` queries can contain SassScript expressions (including
variables, functions, and operators) in place of the feature names and feature
values. For example:

最後に、`@media` クエリは、特性の名前や特性の値の場所にSassScript式(変数、関数、オペレーターを含む)を入れることができます。
例は以下のとおりです。

    $media: screen;
    $feature: -webkit-min-device-pixel-ratio;
    $value: 1.5;

    @media #{$media} and ($feature: $value) {
      .sidebar {
        width: 500px;
      }
    }

is compiled to:

以下のようにコンパイルされます。

    @media screen and (-webkit-min-device-pixel-ratio: 1.5) {
      .sidebar {
        width: 500px; } }

### `@extend` {#extend}

There are often cases when designing a page
when one class should have all the styles of another class,
as well as its own specific styles.
The most common way of handling this is to use both the more general class
and the more specific class in the HTML.
For example, suppose we have a design for a normal error
and also for a serious error. We might write our markup like so:

ページのデザインをしている時に、あるクラスに他のクラスの全てのスタイルを固有のスタイルとして持たせたい場合が時々あります。
これを実装する最も一般的な方法は、より一般的なクラスを両方に使って、HTMLに固有のクラスを使うことです。
例えば、標準的なエラーと深刻なエラーをデザインすると仮定します。
以下のようにマークアップするとします。

    <div class="error seriousError">
      Oh no! You've been hacked!
    </div>

And our styles like so:

そして、スタイルは以下のようになります。

    .error {
      border: 1px #f00;
      background-color: #fdd;
    }
    .seriousError {
      border-width: 3px;
    }

Unfortunately, this means that we have to always remember
to use `.error` with `.seriousError`.
This is a maintenance burden, leads to tricky bugs,
and can bring non-semantic style concerns into the markup.

残念なことに、これは `.seriousError` と同時に `.error` を使うことを覚えておかなければいけません。
これはメンテナンス性が良くなくて、複雑なバグの原因となり、マークアップがセマンティックではなくなる危険があります。

The `@extend` directive avoids these problems
by telling Sass that one selector should inherit the styles of another selector.
For example:

`@extend` ディレクティブは、あるセレクタが他のセレクタのスタイルを継承するようにSassに指示することで、
これらの問題を避ける事ができます。
例えば、

    .error {
      border: 1px #f00;
      background-color: #fdd;
    }
    .seriousError {
      @extend .error;
      border-width: 3px;
    }

This means that all styles defined for `.error`
are also applied to `.seriousError`,
in addition to the styles specific to `.seriousError`.
In effect, everything with class `.seriousError` also has class `.error`.

これは、 `.error` に定義されている全てのスタイルは、 `.seriousError` にも適用させて、
それに加えて `.seriousError` に固有のスタイルを適用することを意味しています。
要するに、 `.seriousError` クラスに定義されてる全てのスタイルは `.error` クラスにも定義されています。

Other rules that use `.error` will work for `.seriousError` as well.
For example, if we have special styles for errors caused by hackers:

`.error` を使う他のルールは、 `.seriousError` と同じように処理されます。
例えば、ハッカーによって引き起こされたエラーに対して特別なスタイルがある場合、

    .error.intrusion {
      background-image: url("/image/hacked.png");
    }

Then `<div class="seriousError intrusion">`
will have the `hacked.png` background image as well.

それから、 `<div class="seriousError intrusion">` は同じように背景画像に `hacked.png` が定義されています。

#### How it Works

#### どのように動作するか

`@extend` works by inserting the extending selector (e.g. `.seriousError`)
anywhere in the stylesheet that the extended selector (.e.g `.error`) appears.
Thus the example above:

`@extend` は、拡張されたセレクタ(例えば、 `.error`)が表示されているスタイルシートのどこかに拡張するセレクタ(例えば `.seriousError`)を挿入することで処理されます。
よって、上述の例として、

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

is compiled to:

これは以下のようにコンパイルされます。

    .error, .seriousError {
      border: 1px #f00;
      background-color: #fdd; }

    .error.intrusion, .seriousError.intrusion {
      background-image: url("/image/hacked.png"); }

    .seriousError {
      border-width: 3px; }

When merging selectors, `@extend` is smart enough
to avoid unnecessary duplication,
so something like `.seriousError.seriousError` gets translated to `.seriousError`.
In addition, it won't produce selectors that can't match anything, like `#main#footer`.

セレクタをマージする時、 `@extend` は不必要な重複を避けるために賢く振る舞い、
`.seriousError.seriousError` のようなものは `.seriousError` に変換されます。
その上、 `#main#footer` のように何かが一致しないセレクタを生成しません。

#### Extending Complex Selectors

#### 複雑なセレクタの拡張

Class selectors aren't the only things that can be extended.
It's possible to extend any selector involving only a single element,
such as `.special.cool`, `a:hover`, or `a.user[href^="http://"]`.
For example:

クラスセレクタは拡張できるだけではありません。
`.special.cool` 、 `a:hover` 、 `a.user[href^="http://"]` のような一つの要素しか関与していない任意のセレクタを拡張することが可能です。
例は以下のとおりです。

    .hoverlink {
      @extend a:hover;
    }

Just like with classes, this means that all styles defined for `a:hover`
are also applied to `.hoverlink`.
For example:

クラスの時と同じように、これは `a:hover` に適用された全てのスタイルは `.hoverlink` にも適用されます。
例は以下のとおりです。

    .hoverlink {
      @extend a:hover;
    }
    a:hover {
      text-decoration: underline;
    }

is compiled to:

これは以下のようにコンパイルされます。

    a:hover, .hoverlink {
      text-decoration: underline; }

Just like with `.error.intrusion` above,
any rule that uses `a:hover` will also work for `.hoverlink`,
even if they have other selectors as well.
For example:

上述した `.error.intrusion` の時と同じように、 `a:hover` で使われているルールは、
他のセレクタと同じように `.hoverlink` にも適用されます。
例は以下のとおりです。

    .hoverlink {
      @extend a:hover;
    }
    .comment a.user:hover {
      font-weight: bold;
    }

is compiled to:

これは以下のようにコンパイルされます。

    .comment a.user:hover, .comment .user.hoverlink {
      font-weight: bold; }

#### Multiple Extends

#### 複数の拡張

A single selector can extend more than one selector.
This means that it inherits the styles of all the extended selectors.
For example:

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

is compiled to:

これは以下のようにコンパイルされます。

    .error, .seriousError {
      border: 1px #f00;
      background-color: #fdd; }

    .attention, .seriousError {
      font-size: 3em;
      background-color: #ff0; }

    .seriousError {
      border-width: 3px; }

In effect, everything with class `.seriousError`
also has class `.error` *and* class `.attention`.
Thus, the styles defined later in the document take precedence:
`.seriousError` has background color `#ff0` rather than `#fdd`,
since `.attention` is defined later than `.error`.

要するに、 `.seriousError` クラスは `.error` クラス *と* `.attention` クラスで定義されているものを全てを持っています。
よって、ドキュメントの後半で定義されたスタイルが優先され、
`.seriousError` の背景色は、 `.attention` が `.error` よりも後ろで定義されているので、 `#fdd` ではなく `#ff0` になります。

Multiple extends can also be written using a comma-separated list of selectors.
For example, `@extend .error, .attention`
is the same as `@extend .error; @extend.attention`.

複数の拡張は、コンマで分割されたセレクタのリストを使って記述することもできます。
例えば、 `@extend .error, .attention` は `@extend .error; @extend.attention` と同じです。

#### Chaining Extends

#### 拡張を連鎖させる

It's possible for one selector to extend another selector
that in turn extends a third.
For example:

あるセレクタが別のセレクタを順に拡張することが可能です。
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

Now everything with class `.seriousError` also has class `.error`,
and everything with class `.criticalError` has class `.seriousError`
*and* class `.error`.
It's compiled to:

`.seriousError` クラスは、 `.error` クラスの全てのスタイルも定義されていて、
`.criticalError` クラスは、 `.seriousError` クラス *と* 、 `.error` クラスのスタイルの全てが定義されています。
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

#### Selector Sequences

#### セレクタシーケンス

Selector sequences, such as `.foo .bar` or `.foo + .bar`, currently can't be extended.
However, it is possible for nested selectors themselves to use `@extend`.
For example:

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

is compiled to

以下のようにコンパイルされます。

    a, #fake-links .link {
      color: blue; }
      a:hover, #fake-links .link:hover {
        text-decoration: underline; }

##### Merging Selector Sequences

##### セレクタシーケンスのマージ

Sometimes a selector sequence extends another selector that appears in another sequence.
In this case, the two sequences need to be merged.
For example:

時々セレクタシーケンスは別のシーケンスで記述されている他のセレクタを拡張することがあります。
今回のケースでは、2つのシーケンスをマージする必要があるとします。
例は以下のとおりです。

    #admin .tabbar a {
      font-weight: bold;
    }
    #demo .overview .fakelink {
      @extend a;
    }

While it would technically be possible
to generate all selectors that could possibly match either sequence,
this would make the stylesheet far too large.
The simple example above, for instance, would require ten selectors.
Instead, Sass generates only selectors that are likely to be useful.

いずれかのシーケンスと一致する可能性がある全てのセレクタを生成することが技術的に可能ですが、
スタイルシートが大きくなりすぎます。
上の簡単な例では、例えば、10個のセレクタが必要です。
代わりに、Sassは使う可能性があるセレクタだけ出力します。

When the two sequences being merged have no selectors in common,
then two new selectors are generated:
one with the first sequence before the second,
and one with the second sequence before the first.
For example:

2つのシーケンスがマージされる時に共通するセレクタがない場合、
2つ目のシーケンスの前に1つ目のシーケンスが追加されたものと、
1つ目のシーケンスの前に2つ目のシーケンスが追加されものの
2つの新しいセレクタが生成されます。
例は以下のとおりです。

    #admin .tabbar a {
      font-weight: bold;
    }
    #demo .overview .fakelink {
      @extend a;
    }

is compiled to:

以下のようにコンパイルされます。

    #admin .tabbar a,
    #admin .tabbar #demo .overview .fakelink,
    #demo .overview #admin .tabbar .fakelink {
      font-weight: bold; }

If the two sequences do share some selectors,
then those selectors will be merged together
and only the differences (if any still exist) will alternate.
In this example, both sequences contain the id `#admin`,
so the resulting selectors will merge those two ids:

2つのシーケンスがいくつかのセレクタを共有している場合、
これらのセレクタは一緒にマージされ、異なる部分(いずれかがまだ存在する場合)だけ別に出力されます。
この例では、両方のシーケンスに `#admin` というIDを含んでいるので、出力されるセレクタは2つのIDをマージします。

    #admin .tabbar a {
      font-weight: bold;
    }
    #admin .overview .fakelink {
      @extend a;
    }

This is compiled to:

これは以下のようにコンパイルされます。

    #admin .tabbar a,
    #admin .tabbar .overview .fakelink,
    #admin .overview .tabbar .fakelink {
      font-weight: bold; }

#### `@extend`-Only Selectors {#placeholders}

#### セレクタだけ `@extend` する {#placeholders}

Sometimes you'll write styles for a class
that you only ever want to `@extend`,
and never want to use directly in your HTML.
This is especially true when writing a Sass library,
where you may provide styles for users to `@extend` if they need
and ignore if they don't.

`@extend` したいが、HTMLで直接使わないスタイルを一つのクラスに書くことが時々あります。
これは、必要な時は `@extend` して、必要ない時は無視するスタイルをユーザーに提供しているようなSassのライブラリを書いている時は特にそうです。

If you use normal classes for this, you end up creating a lot of extra CSS
when the stylesheets are generated, and run the risk of colliding with other classes
that are being used in the HTML.
That's why Sass supports "placeholder selectors" (for example, `%foo`).

このような標準的なクラスを使う場合、スタイルシートが生成される時にたくさんのCSSが生成することになり、
HTMLで既に使われている他のクラスと重複するリスクがあります。
Sassが「プレースホルダーセレクタ」(例、 `%foo`)をサポートするのはこの理由からです。

Placeholder selectors look like class and id selectors,
except the `#` or `.` is replaced by `%`.
They can be used anywhere a class or id could,
and on their own they prevent rulesets from being rendered to CSS.
For example:

プレースホルダーセレクタは、 `#` や `.` が `%` に置き換えられていること以外は、
クラスやIDセレクタのようなものです。
プレースホルダーセレクタは、クラスやIDのようにどこでも使うことができ、
プレースホルダーセレクタ自体のルールセットがCSSに出力されることはありません。
例は以下のとおりです。

    // This ruleset won't be rendered on its own.
    // このルールセット自体は出力されません。
    #context a%extreme {
      color: blue;
      font-weight: bold;
      font-size: 2em;
    }

However, placeholder selectors can be extended, just like classes and ids.
The extended selectors will be generated, but the base placeholder selector will not.
For example:

しかし、プレースホルダーセレクタは、クラスやIDのように拡張することができます。
拡張したセレクタは生成されますが、元にしたプレースホルダーセレクタは生成されません。
例は以下のとおりです。

    .notice {
      @extend %extreme;
    }

Is compiled to:

以下のようにコンパイルされます。

    #context a.notice {
      color: blue;
      font-weight: bold;
      font-size: 2em; }

#### The `!optional` Flag

#### `!optional` フラグ

Normally when you extend a selector, it's an error if that `@extend` doesn't
work. For example, if you write `a.important {@extend .notice}`, it's an error
if there are no selectors that contain `.notice`. It's also an error if the only
selector containing `.notice` is `h1.notice`, since `h1` conflicts with `a` and
so no new selector would be generated.

通常は、セレクタを拡張する時に `@extend` が正しく動作しない場合はエラーになります。
例えば、 `a.important {@extend .notice}` と記述した時に `.notice` を含むセレクタがない場合エラーになります。
`.notice` を含むセレクタが `h1.notice` だけの時も、 `h1` は `a` とコンフリクトを起こし、
新しいセレクタが生成されないのでエラーになります。

Sometimes, though, you want to allow an `@extend` not to produce any new
selectors. To do so, just add the `!optional` flag after the selector. For
example:

しかし、新しいセレクタを生成しない `@extend` を定義したい場合も時々あります。
このようにするには、セレクタの後に `!optional` フラグを追加するだけです。
例は以下のとおりです。

    a.important {
      @extend .notice !optional;
    }

#### `@extend` in Directives

#### ディレクティブの `@extend`

There are some restrictions on the use of `@extend` within directives such as
`@media`. Sass is unable to make CSS rules outside of the `@media` block apply
to selectors inside it without creating a huge amount of stylesheet bloat by
copying styles all over the place. This means that if you use `@extend` within
`@media` (or other CSS directives), you may only extend selectors that appear
within the same directive block.

`@media` のようなディレクティブ内で、 `@extend` を使うにはいくつかの制限があります。
Sassは、 `@media` ブロックの外側にCSSルールを作ることができません。
Sassは、あらゆる場所にスタイルをコピーすることで膨大な量のスタイルシートが作成されることがないように、
`@media` ブロックの外側のCSSルールに `@media` 内のセレクタを適用することができません。
これは、 `@media` 内(もしくは他のCSSディレクティブ内)で `@extend` を使う場合、
同じディレクティブブロック内で記述されているセレクタだけ拡張することができます。

For example, the following works fine:

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

But this is an error:

しかし、これはエラーになります。

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

Someday we hope to have `@extend` supported natively in the browser, which will
allow it to be used within `@media` and other directives.

いつか `@extend` が `@media` や他のディレクティブ内で使うことができるように、
ブラウザに標準でサポートされることを望んでいます。

### `@debug`

The `@debug` directive prints the value of a SassScript expression
to the standard error output stream.
It's useful for debugging Sass files
that have complicated SassScript going on.
For example:

`@debug` ディレクティブは、SassScript式の値を標準エラー出力に出力します。
SassScriptが複雑になってきた際のSassファイルのデバッグの際に便利です。
例は以下のとおりです。

    @debug 10em + 12em;

outputs:

これは以下のように出力されます。

    Line 1 DEBUG: 22em

### `@warn`

The `@warn` directive prints the value of a SassScript expression
to the standard error output stream.
It's useful for libraries that need to warn users of deprecations
or recovering from minor mixin usage mistakes.
There are two major distinctions between `@warn` and `@debug`:

`@warn` ディレクティブは、SassScript式の値を標準エラー出力に出力します。
非推奨の機能を使った場合にユーザーに警告を出したり、
マイナーなミックスインの間違った使い方をした場合に警告を出して正しく使ってもらえるようにする等の
ライブラリには便利な機能です。
`@warn` と `@debug` の主な違いは2つあります。

1. You can turn warnings off with the `--quiet` command-line option
   or the `:quiet` Sass option.
2. A stylesheet trace will be printed out along with the message
   so that the user being warned can see where their styles caused the warning.

1. `--quiet` コマンドラインオプションか `:quiet` Sassオプションで、警告を非表示にすることができます。
2. 警告の原因となったスタイルの場所を確認することができるように、
   スタイルシートの解析結果がメッセージと一緒に出力されます。

Usage Example:

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
