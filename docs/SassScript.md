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
プロンプトが表示されたら、実行して結果を出力する規定されたSassスクリプト構文をどれでも入力します

    $ sass -i
    >> "Hello, Sassy World!"
    "Hello, Sassy World!"
    >> 1px + 1px + 1px
    3px
    >> #777 + #777
    #eeeeee
    >> #777 + #888
    white

### Variables: `$` {#variables_}

The most straightforward way to use SassScript
is to use variables.
Variables begin with dollar signs,
and are set like CSS properties:

    $width: 5em;

You can then refer to them in properties:

    #main {
      width: $width;
    }

Variables are only available within the level of nested selectors
where they're defined.
If they're defined outside of any nested selectors,
they're available everywhere.

Variables used to use the prefix character `!`;
this still works, but it's deprecated and prints a warning.
`$` is the recommended syntax.

Variables also used to be defined with `=` rather than `:`;
this still works, but it's deprecated and prints a warning.
`:` is the recommended syntax.

### Data Types

SassScript supports six main data types:

* numbers (e.g. `1.2`, `13`, `10px`)
* strings of text, with and without quotes (e.g. `"foo"`, `'bar'`, `baz`)
* colors (e.g. `blue`, `#04a3f9`, `rgba(255, 0, 0, 0.5)`)
* booleans (e.g. `true`, `false`)
* nulls (e.g. `null`)
* lists of values, separated by spaces or commas (e.g. `1.5em 1em 0 2em`, `Helvetica, Arial, sans-serif`)

SassScript also supports all other types of CSS property value,
such as Unicode ranges and `!important` declarations.
However, it has no special handling for these types.
They're treated just like unquoted strings.

#### Strings {#sass-script-strings}

CSS specifies two kinds of strings: those with quotes,
such as `"Lucida Grande"` or `'http://sass-lang.com'`,
and those without quotes, such as `sans-serif` or `bold`.
SassScript recognizes both kinds,
and in general if one kind of string is used in the Sass document,
that kind of string will be used in the resulting CSS.

There is one exception to this, though:
when using [`#{}` interpolation](#interpolation_),
quoted strings are unquoted.
This makes it easier to use e.g. selector names in [mixins](#mixins).
For example:

    @mixin firefox-message($selector) {
      body.firefox #{$selector}:before {
        content: "Hi, Firefox users!";
      }
    }

    @include firefox-message(".header");

is compiled to:

    body.firefox .header:before {
      content: "Hi, Firefox users!"; }

It's also worth noting that when using the [deprecated `=` property syntax](#sassscript),
all strings are interpreted as unquoted,
regardless of whether or not they're written with quotes.

#### Lists

Lists are how Sass represents the values of CSS declarations
like `margin: 10px 15px 0 0` or `font-face: Helvetica, Arial, sans-serif`.
Lists are just a series of other values, separated by either spaces or commas.
In fact, individual values count as lists, too: they're just lists with one item.

On their own, lists don't do much,
but the [Sass list functions](Sass/Script/Functions.html#list-functions)
make them useful.
The {Sass::Script::Functions#nth nth function} can access items in a list,
the {Sass::Script::Functions#join join function} can join multiple lists together,
and the {Sass::Script::Functions#append append function} can add items to lists.
The [`@each` rule](#each-directive) can also add styles for each item in a list.

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

When lists are turned into plain CSS, Sass doesn't add any parentheses,
since CSS doesn't understand them.
That means that `(1px 2px) (5px 6px)` and `1px 2px 5px 6px`
will look the same when they become CSS.
However, they aren't the same when they're Sass:
the first is a list containing two lists,
while the second is a list containing four numbers.

Lists can also have no items in them at all.
These lists are represented as `()`.
They can't be output directly to CSS;
if you try to do e.g. `font-family: ()`, Sass will raise an error.
If a list contains empty lists or null values,
as in `1px 2px () 3px` or `1px 2px null 3px`,
the empty lists and null values will be removed
before the containing list is turned into CSS.

### Operations

All types support equality operations (`==` and `!=`).
In addition, each type has its own operations
that it has special support for.

#### Number Operations

SassScript supports the standard arithmetic operations on numbers
(`+`, `-`, `*`, `/`, `%`),
and will automatically convert between units if it can:

    p {
      width: 1in + 8pt;
    }

is compiled to:

    p {
      width: 1.111in; }

Relational operators
(`<`, `>`, `<=`, `>=`)
are also supported for numbers,
and equality operators
(`==`, `!=`)
are supported for all types.

##### Division and `/`
{#division-and-slash}

CSS allows `/` to appear in property values
as a way of separating numbers.
Since SassScript is an extension of the CSS property syntax,
it must support this, while also allowing `/` to be used for division.
This means that by default, if two numbers are separated by `/` in SassScript,
then they will appear that way in the resulting CSS.

However, there are three situations where the `/` will be interpreted as division.
These cover the vast majority of cases where division is actually used.
They are:

1. If the value, or any part of it, is stored in a variable.
2. If the value is surrounded by parentheses.
3. If the value is used as part of another arithmetic expression.

For example:

    p {
      font: 10px/8px;             // Plain CSS, no division
      $width: 1000px;
      width: $width/2;            // Uses a variable, does division
      height: (500px/2);          // Uses parentheses, does division
      margin-left: 5px + 8px/2px; // Uses +, does division
    }

is compiled to:

    p {
      font: 10px/8px;
      width: 500px;
      height: 250px;
      margin-left: 9px; }

If you want to use variables along with a plain CSS `/`,
you can use `#{}` to insert them.
For example:

    p {
      $font-size: 12px;
      $line-height: 30px;
      font: #{$font-size}/#{$line-height};
    }

is compiled to:

    p {
      font: 12px/30px; }

#### Color Operations

All arithmetic operations are supported for color values,
where they work piecewise.
This means that the operation is performed
on the red, green, and blue components in turn.
For example:

    p {
      color: #010203 + #040506;
    }

computes `01 + 04 = 05`, `02 + 05 = 07`, and `03 + 06 = 09`,
and is compiled to:

    p {
      color: #050709; }

Often it's more useful to use {Sass::Script::Functions color functions}
than to try to use color arithmetic to achieve the same effect.

Arithmetic operations also work between numbers and colors,
also piecewise.
For example:

    p {
      color: #010203 * 2;
    }

computes `01 * 2 = 02`, `02 * 2 = 04`, and `03 * 2 = 06`,
and is compiled to:

    p {
      color: #020406; }

Note that colors with an alpha channel
(those created with the {Sass::Script::Functions#rgba rgba}
or {Sass::Script::Functions#hsla hsla} functions)
must have the same alpha value in order for color arithmetic
to be done with them.
The arithmetic doesn't affect the alpha value.
For example:

    p {
      color: rgba(255, 0, 0, 0.75) + rgba(0, 255, 0, 0.75);
    }

is compiled to:

    p {
      color: rgba(255, 255, 0, 0.75); }

The alpha channel of a color can be adjusted using the
{Sass::Script::Functions#opacify opacify} and
{Sass::Script::Functions#transparentize transparentize} functions.
For example:

    $translucent-red: rgba(255, 0, 0, 0.5);
    p {
      color: opacify($translucent-red, 0.3);
      background-color: transparentize($translucent-red, 0.25);
    }

is compiled to:

    p {
      color: rgba(255, 0, 0, 0.9);
      background-color: rgba(255, 0, 0, 0.25); }

IE filters require all colors include the alpha layer, and be in
the strict format of #AABBCCDD. You can more easily convert the
color using the {Sass::Script::Functions#ie_hex_str ie_hex_str}
function.
For example:

    $translucent-red: rgba(255, 0, 0, 0.5);
    $green: #00ff00;
    div {
      filter: progid:DXImageTransform.Microsoft.gradient(enabled='false', startColorstr='#{ie-hex-str($green)}', endColorstr='#{ie-hex-str($translucent-red)}');
    }

is compiled to:

    div {
      filter: progid:DXImageTransform.Microsoft.gradient(enabled='false', startColorstr=#FF00FF00, endColorstr=#80FF0000);
    }

#### String Operations

The `+` operation can be used to concatenate strings:

    p {
      cursor: e + -resize;
    }

is compiled to:

    p {
      cursor: e-resize; }

Note that if a quoted string is added to an unquoted string
(that is, the quoted string is to the left of the `+`),
the result is a quoted string.
Likewise, if an unquoted string is added to a quoted string
(the unquoted string is to the left of the `+`),
the result is an unquoted string.
For example:

    p:before {
      content: "Foo " + Bar;
      font-family: sans- + "serif";
    }

is compiled to:

    p:before {
      content: "Foo Bar";
      font-family: sans-serif; }

By default, if two values are placed next to one another,
they are concatenated with a space:

    p {
      margin: 3px + 4px auto;
    }

is compiled to:

    p {
      margin: 7px auto; }

Within a string of text, #{} style interpolation can be used to
place dynamic values within the string:

    p:before {
      content: "I ate #{5 + 10} pies!";
    }

is compiled to:

    p:before {
      content: "I ate 15 pies!"; }

Null values are treated as empty strings for string interpolation:

    $value: null;
    p:before {
      content: "I ate #{$value} pies!";
    }

is compiled to:

    p:before {
      content: "I ate  pies!"; }

#### Boolean Operations

SassScript supports `and`, `or`, and `not` operators
for boolean values.

#### List Operations

Lists don't support any special operations.
Instead, they're manipulated using the
[list functions](Sass/Script/Functions.html#list-functions).

### Parentheses

Parentheses can be used to affect the order of operations:

    p {
      width: 1em + (2em * 3);
    }

is compiled to:

    p {
      width: 7em; }

### Functions

SassScript defines some useful functions
that are called using the normal CSS function syntax:

    p {
      color: hsl(0, 100%, 50%);
    }

is compiled to:

    p {
      color: #ff0000; }

#### Keyword Arguments

Sass functions can also be called using explicit keyword arguments.
The above example can also be written as:

    p {
      color: hsl($hue: 0, $saturation: 100%, $lightness: 50%);
    }

While this is less concise, it can make the stylesheet easier to read.
It also allows functions to present more flexible interfaces,
providing many arguments without becoming difficult to call.

Named arguments can be passed in any order, and arguments with default values can be omitted.
Since the named arguments are variable names, underscores and dashes can be used interchangeably.

See {Sass::Script::Functions} for a full listing of Sass functions and their argument names,
as well as instructions on defining your own in Ruby.

### Interpolation: `#{}` {#interpolation_}

You can also use SassScript variables in selectors
and property names using #{} interpolation syntax:

    $name: foo;
    $attr: border;
    p.#{$name} {
      #{$attr}-color: blue;
    }

is compiled to:

    p.foo {
      border-color: blue; }

It's also possible to use `#{}` to put SassScript into property values.
In most cases this isn't any better than using a variable,
but using `#{}` does mean that any operations near it
will be treated as plain CSS.
For example:

    p {
      $font-size: 12px;
      $line-height: 30px;
      font: #{$font-size}/#{$line-height};
    }

is compiled to:

    p {
      font: 12px/30px; }

### Variable Defaults: `!default`

You can assign to variables if they aren't already assigned
by adding the `!default` flag to the end of the value.
This means that if the variable has already been assigned to,
it won't be re-assigned,
but if it doesn't have a value yet, it will be given one.

For example:

    $content: "First content";
    $content: "Second content?" !default;
    $new_content: "First time reference" !default;

    #main {
      content: $content;
      new-content: $new_content;
    }

is compiled to:

    #main {
      content: "First content";
      new-content: "First time reference"; }

Variables with `null` values are treated as unassigned by !default:

    $content: null;
    $content: "Non-null content" !default;

    #main {
      content: $content;
    }

is compiled to:

    #main {
      content: "Non-null content"; }
