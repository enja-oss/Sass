+  元文書: [sass/doc-src/SASS_REFERENCE.md at f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2 · nex3/sass · GitHub](https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#sassscript-sassscript "sass/doc-src/SASS_REFERENCE.md at f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2 · nex3/sass · GitHub")

## SassScript [原文](http://sass-lang.com/docs/yardoc/file.SASS_REFERENCE.html#sassscript)

## Sassスクリプト [原文](http://sass-lang.com/docs/yardoc/file.SASS_REFERENCE.html#sassscript)


In addition to the plain CSS property syntax,
Sass supports a small set of extensions called SassScript.
SassScript allows properties to use
variables, arithmetic, and extra functions.
SassScript can be used in any property value.

プレーンなCSSプロパティの構文に加えることで、
SassはSassスクリプトと呼ぶ小型の機能拡張群を扱うことができます。
Sassスクリプトは、変数や計算そして追加機能をプロパティで利用できるようにしています。
Sassスクリプトはあらゆるプロパティの値で利用することができます。

SassScript can also be used to generate selectors and property names,
which is useful when writing [mixins](#mixins).
This is done via [interpolation](#interpolation_).

Sassスクリプトは、セレクタやプロパティ名を生成するときにも利用でき、
それらは[ミックスイン](#mixins)を記述するときに有用です。
これは[補間](#interpolation_)によって行われます。

### Interactive Shell

### 対話式シェル

You can easily experiment with SassScript using the interactive shell.
To launch the shell run the sass command-line with the `-i` option. At the
prompt, enter any legal SassScript expression to have it evaluated
and the result printed out for you:

対話式シェルを利用することで簡単にSassスクリプトの実験ができます。
シェルを起動させるにはコマンドラインで `-i` をつけてsassを実行します。
プロンプトが表示されたら、実行して結果を出力する規定されたSassスクリプト構文をどれでも入力します：

```command-line
$ sass -i
>> "Hello, Sassy World!"
"Hello, Sassy World!"
>> 1px + 1px + 1px
3px
>> #777 + #777
#eeeeee
>> #777 + #888
white
```

### Variables: `$` {#variables_}

### 変数: `$` {#variables_}

The most straightforward way to use SassScript
is to use variables.
Variables begin with dollar signs,
and are set like CSS properties:

Sassスクリプトのもっとも簡単な利用法は変数を使うことです。
変数はドルマークから始まり、CSSプロパティのようにセットされます：

```sass
$width: 5em;
```

You can then refer to them in properties:

それらはプロパティの中で参照できます：

```sass
#main {
  width: $width;
}
```

Variables are only available within the level of nested selectors
where they're defined.
If they're defined outside of any nested selectors,
they're available everywhere.

変数は、定義を行ったネストされたセレクタのレベルの内側でのみ有効になります。
すべてのネストされたセレクタの外側で定義されれば、その場所でも有効となります。

Variables used to use the prefix character `!`;
this still works, but it's deprecated and prints a warning.
`$` is the recommended syntax.

変数は接頭辞として`!`が利用されました；
これは今も動きますが、非推奨で警告が出力されます。
`$`が推奨された記述です。

Variables also used to be defined with `=` rather than `:`;
this still works, but it's deprecated and prints a warning.
`:` is the recommended syntax.

変数は`:`よりもむしろ`=`が定義時に利用されました；
これは今も動きますが、非推奨で警告が出力されます。
`:`が推奨された記述です。


### Data Types

### データタイプ

SassScript supports six main data types:

Sassスクリプトは6つの主なデータータイプをサポートしています：

* numbers (e.g. `1.2`, `13`, `10px`)
* strings of text, with and without quotes (e.g. `"foo"`, `'bar'`, `baz`)
* colors (e.g. `blue`, `#04a3f9`, `rgba(255, 0, 0, 0.5)`)
* booleans (e.g. `true`, `false`)
* nulls (e.g. `null`)
* lists of values, separated by spaces or commas (e.g. `1.5em 1em 0 2em`, `Helvetica, Arial, sans-serif`)

* 数値 (例 `1.2`, `13`, `10px`)
* 文字列、クオートはありなしどちらでも(例 `"foo"`, `'bar'`, `baz`)
* 色 (例 `blue`, `#04a3f9`, `rgba(255, 0, 0, 0.5)`)
* ブール値 (例 `true`, `false`)
* null値 (例 `null`)
* 値のリスト、スペースかカンマで区切られます (例 `1.5em 1em 0 2em`, `Helvetica, Arial, sans-serif`)

SassScript also supports all other types of CSS property value,
such as Unicode ranges and `!important` declarations.
However, it has no special handling for these types.
They're treated just like unquoted strings.

Sassスクリプトは、その他にもユニコードの範囲内の文字や
`!important`宣言といった全てのCSSのプロパティの値のタイプをサポートします。
どういう形であれ、これらに対して特別な対応は必要ありません。
これらはちゃんとクオートされていない文字として扱われます。

#### Strings {#sass-script-strings}

#### 文字列 {#sass-script-strings}

CSS specifies two kinds of strings: those with quotes,
such as `"Lucida Grande"` or `'http://sass-lang.com'`,
and those without quotes, such as `sans-serif` or `bold`.
SassScript recognizes both kinds,
and in general if one kind of string is used in the Sass document,
that kind of string will be used in the resulting CSS.

CSSでは2通りの文字列が規定されています：
`"Lucida Grande"`や`'http://sass-lang.com'`のようにクオートがついているものと、
`sans-serif`や`bold`のようにクオートがつかないものです。
Sassスクリプトではどちらも認識します、
また一般的にはSassの文書の中で片方のタイプの文字列が利用された場合には、
出力したCSSでもそのタイプの文字列が適用されます。

There is one exception to this, though:
when using [`#{}` interpolation](#interpolation_),
quoted strings are unquoted.
This makes it easier to use e.g. selector names in [mixins](#mixins).
For example:

とはいえ、一つ例外があります：
[`#{}` 補間](#interpolation_)を利用した場合には、
クオートされた文字列はクオートが取り除かれます。
これは[ミックスイン](#mixins)でのセレクタ名などを扱うのを容易にします。
例えば：

    @mixin firefox-message($selector) {
      body.firefox #{$selector}:before {
        content: "Hi, Firefox users!";
      }
    }

    @include firefox-message(".header");

is compiled to:

は以下にコンパイルされます：

    body.firefox .header:before {
      content: "Hi, Firefox users!"; }

It's also worth noting that when using the [deprecated `=` property syntax](#sassscript),
all strings are interpreted as unquoted,
regardless of whether or not they're written with quotes.

これは[非推奨である `=` のプロパティ構文](#sassscript)を利用した場合にも注意する必要があり、
全ての文字列はクオートが記述されているかどうかにかかわらず、
クオートされていないものとして扱われます。

#### Lists

#### リスト

Lists are how Sass represents the values of CSS declarations
like `margin: 10px 15px 0 0` or `font-face: Helvetica, Arial, sans-serif`.
Lists are just a series of other values, separated by either spaces or commas.
In fact, individual values count as lists, too: they're just lists with one item.

リストは`margin: 10px 15px 0 0`や`font-face: Helvetica, Arial, sans-serif`のようなCSSの定義の値を表す方法です。
リストは、他とスペースかカンマどちらかで区切られた値のセットそのものです。
実際には、ここの値もリストとしてカウントします：それらはひとつの要素しかもたないリストとなります。

On their own, lists don't do much,
but the [Sass list functions](Sass/Script/Functions.html#list-functions)
make them useful.
The {Sass::Script::Functions#nth nth function} can access items in a list,
the {Sass::Script::Functions#join join function} can join multiple lists together,
and the {Sass::Script::Functions#append append function} can add items to lists.
The [`@each` rule](#each-directive) can also add styles for each item in a list.

リスト自体はそれほど何かをするというわけではありません、
しかし[Sassのリスト機能](Sass/Script/Functions.html#list-functions)で、実用的になります。
{Sass::Script::Functions#nth nth function}はリストの項目にアクセスできますし、
{Sass::Script::Functions#join join function}は複数のリストを一つに結合できます、
また、{Sass::Script::Functions#append append function}はリストに項目を追加できます。
[`@each` ルール](#each-directive)はリストのそれぞれの項目にスタイルの追加もできます。

In addition to containing simple values, lists can contain other lists.
For example, `1px 2px, 5px 6px` is a two-item list
containing the list `1px 2px` and the list `5px 6px`.
If the inner lists have the same separator as the outer list,
you'll need to use parentheses to make it clear
where the inner lists start and stop.
For example, `(1px 2px) (5px 6px)` is also a two-item list
containing the list `1px 2px` and the list `5px 6px`.
The difference is that the outer list is space-separated,
where before it was comma-separated.

リストはシンプルな値を内包していることに加えて、他のリストを含めることもできます。
例えば、`1px 2px, 5px 6px`は、
`1px 2px`のリストと`5px 6px`のリストを内包した、2つの項目のリストとなります。
もし、内部のリストと外側のリストが同じ区切り文字だった場合は、
内部のリストの最初と最後にカッコをつけて明示する必要があります。
例えば、`(1px 2px) (5px 6px)`は、
`1px 2px`のリストと`5px 6px`のリストを内包した、2つの項目のリストとなります。
この違いは、外側のリストが先ほどカンマで区切られていたリストが、
スペースで区切られていることです。

When lists are turned into plain CSS, Sass doesn't add any parentheses,
since CSS doesn't understand them.
That means that `(1px 2px) (5px 6px)` and `1px 2px 5px 6px`
will look the same when they become CSS.
However, they aren't the same when they're Sass:
the first is a list containing two lists,
while the second is a list containing four numbers.

リストがCSSに置き換えられるときには、
CSSでは解釈されないので、Sassはカッコを入れることはありません。
それは、`(1px 2px) (5px 6px)`と`1px 2px 5px 6px`は、
CSSになったときには同じものに見えることを意味します。
しかしながら、それらはSassにおいては同じというわけではありません：
前者は2つのリストを内包したリストであり、
一方後者は4つの数字を内包したリストです。

Lists can also have no items in them at all.
These lists are represented as `()`.
They can't be output directly to CSS;
if you try to do e.g. `font-family: ()`, Sass will raise an error.
If a list contains empty lists or null values,
as in `1px 2px () 3px` or `1px 2px null 3px`,
the empty lists and null values will be removed
before the containing list is turned into CSS.

リストはひとつも値を持たないということもできます。
そのようなリストは`()`で表されます。
それらは直接CSSに出力されません；
もし、例えば  `font-family: ()`を試した場合、Sassはエラーを発生させます。
もしリストが、`1px 2px () 3px`または`1px 2px null 3px`といった
空のリストやヌル値を内包する場合、
空のリストやヌル値は、内包しているリストが
CSSに置き換えられる前に排除されます。

### Operations

### 演算

All types support equality operations (`==` and `!=`).
In addition, each type has its own operations
that it has special support for.

どの変数型も等値演算(`==` and `!=`)をサポートしています。
さらに、それぞれに独自にサポートしている演算があります。

#### Number Operations

#### 数値演算

SassScript supports the standard arithmetic operations on numbers
(`+`, `-`, `*`, `/`, `%`),
and will automatically convert between units if it can:

Sassスクリプトは数値型では基本的な四則演算をサポートしています、
(`+`, `-`, `*`, `/`, `%`)
さらに演算が可能であれば異なる単位のものでも自動的に変換します：

    p {
      width: 1in + 8pt;
    }

is compiled to:

は以下のようにコンパイルされます：

    p {
      width: 1.111in; }

Relational operators
(`<`, `>`, `<=`, `>=`)
are also supported for numbers,
and equality operators
(`==`, `!=`)
are supported for all types.

関係演算子
(`<`, `>`, `<=`, `>=`)
も数値型でサポートされますし、
等値演算
(`==`, `!=`)
も全ての変数型でサポートされます。

##### Division and `/`
{#division-and-slash}

##### 割り算と `/`
{#division-and-slash}

CSS allows `/` to appear in property values
as a way of separating numbers.
Since SassScript is an extension of the CSS property syntax,
it must support this, while also allowing `/` to be used for division.
This means that by default, if two numbers are separated by `/` in SassScript,
then they will appear that way in the resulting CSS.

CSSはプロパティの値で数値の区切りとして`/`が利用されています。
SassスクリプトはCSSのプロパティ構文の拡張なので、
これに対応する必要がありますが、一方で割り算で`/`が利用できるようにもしています。
というわけでデフォルトでは、Sassスクリプトで2つの数字が`/`で区切られていた場合、
結果としてCSSのやり方で出力されることを意味します。

However, there are three situations where the `/` will be interpreted as division.
These cover the vast majority of cases where division is actually used.
They are:

しかしながら、3つの条件下においては`/`は割り算として解釈されます。
その条件は割り算が実際に使用される大部分のケースを対象としています。
それは：

1. If the value, or any part of it, is stored in a variable.
2. If the value is surrounded by parentheses.
3. If the value is used as part of another arithmetic expression.

1. 値、もしくはその一部が変数に保持されているとき
2. 値がカッコで囲まれているとき
3. 値が四則演算の一部に存在しているとき

For example:

例えば：

    p {
      font: 10px/8px;             // Plain CSS, no division
      $width: 1000px;
      width: $width/2;            // Uses a variable, does division
      height: (500px/2);          // Uses parentheses, does division
      margin-left: 5px + 8px/2px; // Uses +, does division
    }

is compiled to:

は以下のようにコンパイルされます：

    p {
      font: 10px/8px;
      width: 500px;
      height: 250px;
      margin-left: 9px; }

If you want to use variables along with a plain CSS `/`,
you can use `#{}` to insert them.
For example:

もし、CSSの`/`と一緒に変数を利用したい場合は、
挿入するときに`#{}`を利用できます。
例えば：

    p {
      $font-size: 12px;
      $line-height: 30px;
      font: #{$font-size}/#{$line-height};
    }

is compiled to:

は、以下のようにコンパイルされます：

    p {
      font: 12px/30px; }

#### Color Operations

#### 色の演算

All arithmetic operations are supported for color values,
where they work piecewise.
This means that the operation is performed
on the red, green, and blue components in turn.
For example:

全ての四則演算は切り分けて動作することで、
色の値も対象にしています。
これは、順番に赤、緑、青の要素ごとに
計算するという意味です。
例えば：

    p {
      color: #010203 + #040506;
    }

computes `01 + 04 = 05`, `02 + 05 = 07`, and `03 + 06 = 09`,
and is compiled to:

は`01 + 04 = 05`、`02 + 05 = 07`そして`03 + 06 = 09`と計算し、
以下のようにコンパイルします：

    p {
      color: #050709; }

Often it's more useful to use {Sass::Script::Functions color functions}
than to try to use color arithmetic to achieve the same effect.

度々、{Sass::Script::Functions color functions}を利用するほうが、
色演算で同じ効果を出そうとするよりも有益なことがあります。

Arithmetic operations also work between numbers and colors,
also piecewise.
For example:

四則演算は色の値と数値の間でも動作します、
こちらも切り分けます。
例えば：

    p {
      color: #010203 * 2;
    }

computes `01 * 2 = 02`, `02 * 2 = 04`, and `03 * 2 = 06`,
and is compiled to:

は`01 * 2 = 02`、`02 * 2 = 04`そして`03 * 2 = 06`と計算し、
以下のようにコンパイルします：

    p {
      color: #020406; }

Note that colors with an alpha channel
(those created with the {Sass::Script::Functions#rgba rgba}
or {Sass::Script::Functions#hsla hsla} functions)
must have the same alpha value in order for color arithmetic
to be done with them.
The arithmetic doesn't affect the alpha value.
For example:

注意点としては、( {Sass::Script::Functions#rgba rgba}
または{Sass::Script::Functions#hsla hsla}機能で生成された)アルファチャンネル
を含む色の値で四則演算を行う場合、アルファの値は同じである必要があります。
演算はアルファの値には作用しません。
例えば：

    p {
      color: rgba(255, 0, 0, 0.75) + rgba(0, 255, 0, 0.75);
    }

is compiled to:

は以下のようにコンパイルされます：

    p {
      color: rgba(255, 255, 0, 0.75); }

The alpha channel of a color can be adjusted using the
{Sass::Script::Functions#opacify opacify} and
{Sass::Script::Functions#transparentize transparentize} functions.
For example:

アルファチャンネルは{Sass::Script::Functions#opacify opacify}と
{Sass::Script::Functions#transparentize transparentize}の機能を利用して
調整されます。
例えば：

    $translucent-red: rgba(255, 0, 0, 0.5);
    p {
      color: opacify($translucent-red, 0.3);
      background-color: transparentize($translucent-red, 0.25);
    }

is compiled to:

は以下のようにコンパイルされます：

    p {
      color: rgba(255, 0, 0, 0.9);
      background-color: rgba(255, 0, 0, 0.25); }

IE filters require all colors include the alpha layer, and be in
the strict format of #AABBCCDD. You can more easily convert the
color using the {Sass::Script::Functions#ie_hex_str ie_hex_str}
function.
For example:

IEのフィルターは全色がアルファレイヤーに含まれる必要があります、
そして#AABBCCDDの厳密な書式になります。
{Sass::Script::Functions#ie_hex_str ie_hex_str}機能を利用することで
より簡易に色の変換を行えます。
例えば：

    $translucent-red: rgba(255, 0, 0, 0.5);
    $green: #00ff00;
    div {
      filter: progid:DXImageTransform.Microsoft.gradient(enabled='false', startColorstr='#{ie-hex-str($green)}', endColorstr='#{ie-hex-str($translucent-red)}');
    }

is compiled to:

は以下のようにコンパイルされます：

    div {
      filter: progid:DXImageTransform.Microsoft.gradient(enabled='false', startColorstr=#FF00FF00, endColorstr=#80FF0000);
    }

#### String Operations

#### 文字列演算

The `+` operation can be used to concatenate strings:

`+` の演算は文字列の連結に利用できます:

    p {
      cursor: e + -resize;
    }

is compiled to:

は以下のようにコンパイルされます：

    p {
      cursor: e-resize; }

Note that if a quoted string is added to an unquoted string
(that is, the quoted string is to the left of the `+`),
the result is a quoted string.
Likewise, if an unquoted string is added to a quoted string
(the unquoted string is to the left of the `+`),
the result is an unquoted string.
For example:

注意点としては、もしクオートされた文字列にクオートされていない文字列が追加された場合、
（`+`の左側がクオートされた文字列）
演算の結果はクオートされた文字列になります。
同様に、もしクオートされていない文字列にクオートされた文字列が追加された場合、
（`+`の左側がクオートされていない文字列）
演算の結果はクオートされていない文字列になります。
例えば：

    p:before {
      content: "Foo " + Bar;
      font-family: sans- + "serif";
    }

is compiled to:

は以下のようにコンパイルされます：

    p:before {
      content: "Foo Bar";
      font-family: sans-serif; }

By default, if two values are placed next to one another,
they are concatenated with a space:

デフォルトでは、2つの値が他の値に続いている時、
スペースで連結されます：

    p {
      margin: 3px + 4px auto;
    }

is compiled to:

は以下のようにコンパイルされます：

    p {
      margin: 7px auto; }

Within a string of text, #{} style interpolation can be used to
place dynamic values within the string:

テキストの文字列の中では、#{}スタイルの補間は
文字列の中で直接値として利用できます：

    p:before {
      content: "I ate #{5 + 10} pies!";
    }

is compiled to:

は以下のようにコンパイルされます：

    p:before {
      content: "I ate 15 pies!"; }

Null values are treated as empty strings for string interpolation:

ヌル値は文字列補間で空文字として扱われます：

    $value: null;
    p:before {
      content: "I ate #{$value} pies!";
    }

is compiled to:

は以下のようにコンパイルされます：

    p:before {
      content: "I ate  pies!"; }

#### Boolean Operations

#### ブール値演算

SassScript supports `and`, `or`, and `not` operators
for boolean values.

Sassスクリプトは、`and` `or` `not` 演算がブール値で利用できます。

#### List Operations

#### リスト演算

Lists don't support any special operations.
Instead, they're manipulated using the
[list functions](Sass/Script/Functions.html#list-functions).

リストは特殊な演算をサポートしていません。
そのかわりに、[リスト機能](Sass/Script/Functions.html#list-functions)
を利用することで操作できます。

### Parentheses

### カッコ

Parentheses can be used to affect the order of operations:

カッコは演算の順序を操作することができます：

    p {
      width: 1em + (2em * 3);
    }

is compiled to:

以下のようにコンパイルされます：

    p {
      width: 7em; }

### Functions

### 関数

SassScript defines some useful functions
that are called using the normal CSS function syntax:

Sassスクリプトは通常のCSSの記法で利用できる
有用な関数をいくつか定義しています。

    p {
      color: hsl(0, 100%, 50%);
    }

is compiled to:

は以下のようにコンパイルされます：

    p {
      color: #ff0000; }

#### Keyword Arguments

#### キーワード引数

Sass functions can also be called using explicit keyword arguments.
The above example can also be written as:

Sassの関数は明示的にキーワード引数を使うことでも呼び出せます。
先ほどの例はこのようにも記述できます：

    p {
      color: hsl($hue: 0, $saturation: 100%, $lightness: 50%);
    }

While this is less concise, it can make the stylesheet easier to read.
It also allows functions to present more flexible interfaces,
providing many arguments without becoming difficult to call.

これは簡潔ではない一方で、スタイルシートを読みやすくしてくれます。
関数はより呼び出しが難しくなるようなこともなく多くの引数を扱える、
柔軟なインターフェースも提供しています。

Named arguments can be passed in any order, and arguments with default values can be omitted.
Since the named arguments are variable names, underscores and dashes can be used interchangeably.

名前付きの引数は任意の順番で渡すことができ、デフォルトの値の引数を省略することができます。
名前付き引数は変数名なので、アンダースコアとダッシュ（ハイフン）を交互に使用できます。

See {Sass::Script::Functions} for a full listing of Sass functions and their argument names,
as well as instructions on defining your own in Ruby.

にSass関数とその名前付き引数の全てのリストがありますので、
自分でRuby内で定義した命令と同様に、{Sass::Script::Functions}を把握してください。

{Sass::Script::Functions}

### Interpolation: `#{}` {#interpolation_}

### 補間: `#{}` {#interpolation_}

You can also use SassScript variables in selectors
and property names using #{} interpolation syntax:

Sassスクリプトの変数は、
セレクタやプロパティ名でも#{}の補間記法で利用できます：

    $name: foo;
    $attr: border;
    p.#{$name} {
      #{$attr}-color: blue;
    }

is compiled to:

は以下のようにコンパイルされます：

    p.foo {
      border-color: blue; }

It's also possible to use `#{}` to put SassScript into property values.
In most cases this isn't any better than using a variable,
but using `#{}` does mean that any operations near it
will be treated as plain CSS.
For example:

#{}を使うことで、プロパティの値に対してSassスクリプトを適用することも可能になります。
ほとんどの場合、変数を利用するよりも有効ということはないですが、
`#{}`を利用することは、
どんな演算も通常のCSSと同様に扱われるということになります。
例えば：

    p {
      $font-size: 12px;
      $line-height: 30px;
      font: #{$font-size}/#{$line-height};
    }

is compiled to:

はいかのようにコンパイルされます：

    p {
      font: 12px/30px; }

### Variable Defaults: `!default`

### 変数のデフォルト: `!default`

You can assign to variables if they aren't already assigned
by adding the `!default` flag to the end of the value.
This means that if the variable has already been assigned to,
it won't be re-assigned,
but if it doesn't have a value yet, it will be given one.

変数が未定義であれば、
`!default`フラグを値の末尾に付属した割り当てを行えます。
これは、変数がすでに割り当てられていた場合、
再度割り当てられないことを意味しまします、
しかし、まだ値を保持していない場合は、割り当てが行われます。

For example:

例えば：

    $content: "First content";
    $content: "Second content?" !default;
    $new_content: "First time reference" !default;

    #main {
      content: $content;
      new-content: $new_content;
    }

is compiled to:

は以下のようにコンパイルされます：

    #main {
      content: "First content";
      new-content: "First time reference"; }

Variables with `null` values are treated as unassigned by !default:

変数がヌル値をもつ場合は !defaultによって未定義として扱われます：

    $content: null;
    $content: "Non-null content" !default;

    #main {
      content: $content;
    }

is compiled to:

は以下のようにコンパイルされます：

    #main {
      content: "Non-null content"; }
