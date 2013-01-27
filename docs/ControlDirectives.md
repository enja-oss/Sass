+  元文書: [https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#control-directives](https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#control-directives)

## 制御構文

SassScript supports basic control directives
for including styles only under some conditions
or including the same style several times with variations.

SassScriptは、ある条件の時だけスタイルを含ませるとか、同じスタイルをバリエーションを変えて含ませる等のような基本的な制御構文をサポートしています。

**Note that control directives are an advanced feature,
and are not recommended in the course of day-to-day styling**.
They exist mainly for use in [mixins](#mixins),
particularly those that are part of libraries like [Compass](http://compass-style.org),
and so require substantial flexibility.

**制御構文は高度な機能で、スタイルの日々の作業を行なっていく過程で使うことは推奨していません。**
[ミックスイン](#mixins)や、[Compass](http://compass-style.org)のようなライブラリの一部や、柔軟性が要求される部分で主に使われます。

### `@if`

The `@if` directive takes a SassScript expression
and uses the styles nested beneath it if the expression returns
anything other than `false` or `null`:

`@if` 構文は、SassScript式を受け取り、式が `false` や `null` 以外を返す場合、ネストされた中のスタイルを使います。:

    p {
      @if 1 + 1 == 2 { border: 1px solid;  }
      @if 5 < 3      { border: 2px dotted; }
      @if null       { border: 3px double; }
    }

is compiled to:

は以下のように出力されます。:

    p {
      border: 1px solid; }

The `@if` statement can be followed by several `@else if` statements
and one `@else` statement.
If the `@if` statement fails,
the `@else if` statements are tried in order
until one succeeds or the `@else` is reached.
For example:

`@if` 文は、複数の `@else if` 文と一つの `@else` 文を続けることができます。
`@if` 文が正しくない場合、 `@else if` 文から正しいものがあるか試し、 `@else` に到達します。
例:

    $type: monster;
    p {
      @if $type == ocean {
        color: blue;
      } @else if $type == matador {
        color: red;
      } @else if $type == monster {
        color: green;
      } @else {
        color: black;
      }
    }

is compiled to:

は以下のように出力されます。:

    p {
      color: green; }

### `@for`

The `@for` directive repeatedly outputs a set of styles. For each repetition, a
counter variable is used to adjust the output. The directive has two forms:
`@for $var from <start> through <end>` and `@for $var from <start> to <end>`.
Note the difference in the keywords `through` and `to`. `$var` can be any
variable name, like `$i`; `<start>` and `<end>` are SassScript expressions that
should return integers.

`@for` 構文は、スタイルのセットを繰り返し出力します。
個々の反復処理の中で、カウンター変数は出力を調整するために使用されます。
構文には、 `@for $var from <start> through <end>` と `@for $var from <start> to <end>` の2つの形式があります。
`through` と `to` というキーワードの違いに注意して下さい。
`$var` は、 `$i` のように任意の変数名にすることができ、 `<start>` と `<end>` は数字を返すSassScript式です。

The `@for` statement sets `$var` to each successive number in the specified
range and each time outputs the nested styles using that value of `$var`. For
the form `from ... through`, the range *includes* the values of `<start>` and
`<end>`, but the form `from ... to` runs up to *but not including* the value of
`<end>`. Using the `through` syntax,

`@for` 文は、指定された範囲内で連続した番号を `$var` にセットして、 `$var` の値を使ってネスト内のスタイルを都度出力します。
`from ... through` の形式から、 `<start>` の値と `<end>` の値の間に*含まれる値*の時だけ実行されますが、
が `from ... to` の形式では、 `<end>` の値が*含まれなくなる*まで実行されます。
`through` 構文を使って下さい。

    @for $i from 1 through 3 {
      .item-#{$i} { width: 2em * $i; }
    }

is compiled to:

は以下のように出力されます。:

    .item-1 {
      width: 2em; }
    .item-2 {
      width: 4em; }
    .item-3 {
      width: 6em; }

### `@each` {#each-directive}

The `@each` rule has the form `@each $var in <list>`.
`$var` can be any variable name, like `$length` or `$name`,
and `<list>` is a SassScript expression that returns a list.

`@each` ルールは、 `@each $var in <list>` の形式を取ります。
`$var` は、 `$length` や `$name` のような任意の変数名にすることができ、
`<list>` は、リストを返すSassScript式です。

The `@each` rule sets `$var` to each item in the list,
then outputs the styles it contains using that value of `$var`.
For example:

`@each` ルールは、リスト内の各項目を `$var` にセットし、
`$var` の値を使ったものが含まれているスタイルを出力します。
例:

    @each $animal in puma, sea-slug, egret, salamander {
      .#{$animal}-icon {
        background-image: url('/images/#{$animal}.png');
      }
    }

is compiled to:

は以下のように出力されます。:

    .puma-icon {
      background-image: url('/images/puma.png'); }
    .sea-slug-icon {
      background-image: url('/images/sea-slug.png'); }
    .egret-icon {
      background-image: url('/images/egret.png'); }
    .salamander-icon {
      background-image: url('/images/salamander.png'); }

### `@while`

The `@while` directive takes a SassScript expression
and repeatedly outputs the nested styles
until the statement evaluates to `false`.
This can be used to achieve more complex looping
than the `@for` statement is capable of,
although this is rarely necessary.
For example:

`@while` 構文は、SassScript式を受け取り、文が `false` と評価されるまでネストされたスタイルを繰り返し出力します。
これは必要になることはめったにありませんが、 `@for` 文で可能なことよりももっと複雑なループを実現するために使えます。
例:

    $i: 6;
    @while $i > 0 {
      .item-#{$i} { width: 2em * $i; }
      $i: $i - 2;
    }

is compiled to:

は以下のように出力されます。:

    .item-6 {
      width: 12em; }

    .item-4 {
      width: 8em; }

    .item-2 {
      width: 4em; }
