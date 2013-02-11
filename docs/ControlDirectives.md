+  元文書: [https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#control-directives](https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#control-directives)

## コントロールディレクティブ

SassScriptは、ある条件の時だけスタイルを含ませるとか、同じスタイルをバリエーションを変えて含ませる等のような基本的なコントロールディレクティブをサポートしています。

**コントロールディレクティブは高度な機能で、日々行うような作業の過程で使うことは推奨しません。**
主に[ミックスイン](#mixins)、特に[Compass](http://compass-style.org)などのライブラリ内で使われるミックスインなど、柔軟性が要求される箇所で使われます。

### `@if`

`@if` ディレクティブは、SassScript式を受け取り、式が `false` や `null` 以外を返す場合、その中のスタイルを使います。

    p {
      @if 1 + 1 == 2 { border: 1px solid;  }
      @if 5 < 3      { border: 2px dotted; }
      @if null       { border: 3px double; }
    }

これは以下のように出力されます。

    p {
      border: 1px solid; }

`@if` 文は、複数の `@else if` 文と一つの `@else` 文を続けて記述できます。
`@if` 文が正しくない場合、 `@else if` 文が順に試され、文が正しいかそうでないかを `@else` に到達するまで続けます。

例えば、

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

これは以下のように出力されます。

    p {
      color: green; }

### `@for`

`@for` ディレクティブは、スタイルのセットを繰り返し出力します。
個々の反復処理の中で、カウンター変数は出力を調整するために使用されます。
構文には、 `@for $var from <start> through <end>` と `@for $var from <start> to <end>` の2つの形式があります。
`through` と `to` というキーワードの違いに注意して下さい。
`$var` は、 `$i` のように任意の変数名にすることができ、 `<start>` と `<end>` は数字を返すSassScript式です。

`@for` 文は、指定された範囲内で連続した数値を `$var` にセットして、 `$var` の値を使ってネスト内のスタイルを都度出力します。
`from ... through` の場合、値の取る範囲は `<start>` と `<end>` の値を含みますが、
`from ... to` の場合、値の取る範囲に `<end>` が*含まれません*。
`for ... through` を使った文は以下のようになります。

    @for $i from 1 through 3 {
      .item-#{$i} { width: 2em * $i; }
    }

これは以下のように出力されます。

    .item-1 {
      width: 2em; }
    .item-2 {
      width: 4em; }
    .item-3 {
      width: 6em; }

### `@each` {#each-directive}

`@each` ルールは、 `@each $var in <list>` の形式を取ります。
`$var` は、 `$length` や `$name` のような任意の変数名にすることができ、
`<list>` は、リストを返すSassScript式です。

`@each` ルールは、リスト内の各項目を `$var` にセットし、
その `$var` の値を使って、@each ルール文内のスタイルを出力します
例えば、

    @each $animal in puma, sea-slug, egret, salamander {
      .#{$animal}-icon {
        background-image: url('/images/#{$animal}.png');
      }
    }

これは以下のように出力されます。

    .puma-icon {
      background-image: url('/images/puma.png'); }
    .sea-slug-icon {
      background-image: url('/images/sea-slug.png'); }
    .egret-icon {
      background-image: url('/images/egret.png'); }
    .salamander-icon {
      background-image: url('/images/salamander.png'); }

### `@while`

`@while` ディレクティブは、SassScript式を受け取り、文が `false` と評価されるまで、文内のスタイルを繰り返し出力します。
これは必要になることはめったにありませんが、 `@for` 文で可能なことよりももっと複雑なループを実現するために使えます。
例えば、

    $i: 6;
    @while $i > 0 {
      .item-#{$i} { width: 2em * $i; }
      $i: $i - 2;
    }

これは以下のように出力されます。

    .item-6 {
      width: 12em; }

    .item-4 {
      width: 8em; }

    .item-2 {
      width: 4em; }
