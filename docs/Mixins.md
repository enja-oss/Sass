+ 元文書: [https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#mixin-directives-mixins](https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#mixin-directives-mixins)

## Mixin Directives {#mixins}

## Mixinディレクティブ {#mixins}

Mixins allow you to define styles
that can be re-used throughout the stylesheet
without needing to resort to non-semantic classes like `.float-left`.
Mixins can also contain full CSS rules,
and anything else allowed elsewhere in a Sass document.
They can even take [arguments](#mixin-arguments)
which allows you to produce a wide variety of styles
with very few mixins.

Mixinはスタイルシート内で再利用可能なスタイルを定義する仕組みです。これによって `.float-left` といったセマンティックではないクラスに頼る必要がなくなります。Mixinはまた、CSSルールや、その他Sassで利用可能なものすべてを含められます。Mixinはさらに[引数](#mixin-arguments)も取ることができ、利用すればほんのすこしのmixinから多種多様なスタイルを生成できます。

### Defining a Mixin: `@mixin` {#defining_a_mixin}

### Mixinの定義: `@mixin` {#defining_a_mixin}

Mixins are defined with the `@mixin` directive.
It's followed by the name of the mixin
and optionally the [arguments](#mixin-arguments),
and a block containing the contents of the mixin.
For example, the `large-text` mixin is defined as follows:

Mixinは `@mixin` ディレクティブにより定義されます。`@mixin` ディレクティブの後には、mixinの名前、[引数](#mixin-arguments)(任意)、そしてmixinの内容を含むブロックが続きます。たとえば、`large-text` というmixinは次のように定義します。

    @mixin large-text {
      font: {
        family: Arial;
        size: 20px;
        weight: bold;
      }
      color: #ff0000;
    }

Mixins may also contain selectors,
possibly mixed with properties.
The selectors can even contain [parent references](#referencing_parent_selectors_).
For example:

Mixinにはセレクタも含められ、またセレクタとプロパティの混在も可能です。さらに、セレクタには[親参照](#referencing_parent_selectors_)も含められます。

    @mixin clearfix {
      display: inline-block;
      &:after {
        content: ".";
        display: block;
        height: 0;
        clear: both;
        visibility: hidden;
      }
      * html & { height: 1px }
    }

### Including a Mixin: `@include` {#including_a_mixin}

### Mixinの読み込み: `@include` {#including_a_mixin}

Mixins are included in the document
with the `@include` directive.
This takes the name of a mixin
and optionally [arguments to pass to it](#mixin-arguments),
and includes the styles defined by that mixin
into the current rule.
For example:

Mixinは `@include` ディレクティブから読み込みます。このディレクティブに続けてmixinの名前を、続けて指定可能な場合は[引数](#mixin-arguments)を記述すると、mixinが定義したスタイルが現在のルールに読み込まれます。たとえば、次のようなコードがあります。

    .page-title {
      @include large-text;
      padding: 4px;
      margin-top: 10px;
    }

is compiled to:

これはこのようにコンパイルされます。

    .page-title {
      font-family: Arial;
      font-size: 20px;
      font-weight: bold;
      color: #ff0000;
      padding: 4px;
      margin-top: 10px; }

Mixins may also be included outside of any rule
(that is, at the root of the document)
as long as they don't directly define any properties
or use any parent references.
For example:

Mixinが直下にプロパティを定義しておらず、また親を参照していない場合は、他のどのルールの外側にも読み込めます (つまり、文書のルートです)。たとえば、次のようなコードがあります。

    @mixin silly-links {
      a {
        color: blue;
        background-color: red;
      }
    }

    @include silly-links;

is compiled to:

これはこのようにコンパイルされます。

    a {
      color: blue;
      background-color: red; }

Mixin definitions can also include other mixins.
For example:

Mixinは他のmixinも読み込めます。たとえば、次のようなコードがあります。

    @mixin compound {
      @include highlighted-background;
      @include header-text;
    }

    @mixin highlighted-background { background-color: #fc0; }
    @mixin header-text { font-size: 20px; }

Mixins that only define descendent selectors, can be safely mixed
into the top most level of a document.

子孫セレクタのみを定義するmixinであれば、ドキュメントの最上位に問題なく含められます。

### Arguments {#mixin-arguments}

### Mixinの引数 {#mixin-arguments}

Mixins can take arguments SassScript values as arguments,
which are given when the mixin is included
and made available within the mixin as variables.

MixinはSassScriptの値を引数に取ることができます。引数はmixinを読み込む際に渡され、mixinの中では変数として扱われます。

When defining a mixin,
the arguments are written as variable names separated by commas,
all in parentheses after the name.
Then when including the mixin,
values can be passed in in the same manner.
For example:

引数はmixin名の後に括弧を続け、その中に変数名として記述します。複数の引数がある場合はカンマで区切り記述します。Mixinに引数を与えて
読み込むときは、定義したときと同じように記述します。たとえば、次のようなコードがあります。

    @mixin sexy-border($color, $width) {
      border: {
        color: $color;
        width: $width;
        style: dashed;
      }
    }

    p { @include sexy-border(blue, 1in); }

is compiled to:

これはこのようにコンパイルされます。

    p {
      border-color: blue;
      border-width: 1in;
      border-style: dashed; }

Mixins can also specify default values for their arguments
using the normal variable-setting syntax.
Then when the mixin is included,
if it doesn't pass in that argument,
the default value will be used instead.
For example:

Mixinの引数には規定値も指定できます。書き方は変数定義の構文と同じです。規定値はMixinの読み込み時、引数が与えられなかった場合に用いられます。たとえば、次のようなコードがあります。

    @mixin sexy-border($color, $width: 1in) {
      border: {
        color: $color;
        width: $width;
        style: dashed;
      }
    }
    p { @include sexy-border(blue); }
    h1 { @include sexy-border(blue, 2in); }

is compiled to:

これはこのようにコンパイルされます。

    p {
      border-color: blue;
      border-width: 1in;
      border-style: dashed; }

    h1 {
      border-color: blue;
      border-width: 2in;
      border-style: dashed; }

#### Keyword Arguments

#### キーワード引数

Mixins can also be included using explicit keyword arguments.
For instance, we the above example could be written as:

明示的なキーワード引数を利用しmixinを読み込むこともできます。たとえば、先ほどの例は次のようにも書けます。

    p { @include sexy-border($color: blue); }
    h1 { @include sexy-border($color: blue, $width: 2in); }

While this is less concise, it can make the stylesheet easier to read.
It also allows functions to present more flexible interfaces,
providing many arguments without becoming difficult to call.

あまり簡潔ではなくなりましたが、キーワードのおかげでスタイルシートが読みやすくなります。キーワード引数を使えば、mixinが多くの引数を取ったとしても呼び出しにくくならない、より柔軟なインターフェースとして表現できます。

Named arguments can be passed in any order, and arguments with default values can be omitted.
Since the named arguments are variable names, underscores and dashes can be used interchangeably.

キーワード引数はどんな順番で与えても構いません。また、規定値を持つ引数は省略できます。キーワード引数は変数名なので、アンダースコアとダッシュは区別されません。

#### Variable Arguments

Sometimes it makes sense for a mixin to take an unknown number of arguments. For
example, a mixin for creating box shadows might take any number of shadows as
arguments. For these situations, Sass supports "variable arguments," which are
arguments at the end of a mixin declaration that take all leftover arguments and
package them up as a [list](#lists). These arguments look just like normal
arguments, but are followed by `...`. For example:

引数の数を定めないmixinを定義したいこともあるでしょう。たとえば、`box-shadow` を生成するmixinであれば、シャドウの数に制限がないほうがよいでしょう。このようなケースに対応すべく、Sassでは「可変長引数」に対応しています。可変長引数はmixin定義の最後に置かれ、残りの引数すべてを[リスト](#lists)としてまとめます。可変長引数は普通の引数によく似ていますが、引数の最後に `...` とある点が異なります。たとえば、次のようなコードがあります。

    @mixin box-shadow($shadows...) {
      -moz-box-shadow: $shadows;
      -webkit-box-shadow: $shadows;
      box-shadow: $shadows;
    }

    .shadows {
      @include box-shadow(0px 4px 5px #666, 2px 6px 10px #999);
    }

is compiled to:

これはこのようにコンパイルされます。

    .shadowed {
      -moz-box-shadow: 0px 4px 5px #666, 2px 6px 10px #999;
      -webkit-box-shadow: 0px 4px 5px #666, 2px 6px 10px #999;
      box-shadow: 0px 4px 5px #666, 2px 6px 10px #999;
    }

Variable arguments can also be used when calling a mixin. Using the same syntax,
you can expand a list of values so that each value is passed as a separate
argument. For example:

可変長引数はmixinを読み込む際にも利用できます。構文は同じで、読み込む際に複数の値をまとめたリストを展開し、各値が別々の引数に渡されるようにできるのです。たとえば、次のようなコードです。

    @mixin colors($text, $background, $border) {
      color: $text;
      background-color: $background;
      border-color: $border;
    }

    $values: #ff0000, #00ff00, #0000ff;
    .primary {
      @include colors($values...);
    }

is compiled to:

これはこのようにコンパイルされます。

    .primary {
      color: #ff0000;
      background-color: #00ff00;
      border-color: #0000ff;
    }

You can use variable arguments to wrap a mixin and add additional styles without
changing the argument signature of the mixin. If you do so, even keyword
arguments will get passed through to the wrapped mixin. For example:

可変長引数を使うと、mixinと他のスタイルを引数の名前を変えずに囲めます。こうすると、キーワード引数さえも、その囲ったmixinに渡せるのです。たとえば、次のようなコードになります。

    @mixin wrapped-stylish-mixin($args...) {
      font-weight: bold;
      @include stylish-mixin($args...);
    }

    .stylish {
      // The $width argument will get passed on to "stylish-mixin" as a keyword
      @include wrapped-stylish-mixin(#00ff00, $width: 100px);
    }

    @mixin wrapped-stylish-mixin($args...) {
      font-weight: bold;
      @include stylish-mixin($args...);
    }

    .stylish {
      // $width 引数が "stylish-mixin" にキーワードとして渡される
      @include wrapped-stylish-mixin(#00ff00, $width: 100px);
    }

### Passing Content Blocks to a Mixin {#mixin-content}

### コンテンツブロックをmixinに渡す {#mixin-content}

It is possible to pass a block of styles to the mixin for placement within the styles included by
the mixin. The styles will appear at the location of any `@content` directives found within the mixin. This makes is possible to define abstractions relating to the construction of
selectors and directives.

Mixinにスタイルブロックを渡して、mixin定義内のスタイルの中に読み込むこともできます。渡されたスタイルブロックはmixin内の `@content` ディレクティブが記述された場所に読み込まれます。この仕組みによって、セレクタとディレクティブの構成を抽象化できます。

For example:

たとえば、次のようなコードがあります。

    @mixin apply-to-ie6-only {
      * html {
        @content;
      }
    }
    @include apply-to-ie6-only {
      #logo {
        background-image: url(/logo.gif);
      }
    }

Generates:

これは次のようなコードを生成します。

    * html #logo {
      background-image: url(/logo.gif);
    }

The same mixins can be done in the `.sass` shorthand syntax:

`.sass` ショートハンド構文では、次のように記述します。

    =apply-to-ie6-only
      * html
        @content

    +apply-to-ie6-only
      #logo
        background-image: url(/logo.gif)

**Note:** when the `@content` directive is specified more than once or in a loop, the style block will be duplicated with each invocation.

**注:** `@content` がひとつ以上、もしくはループ内に記述された場合、mixinに渡されたスタイルブロックはつど複製されます。

#### Variable Scope and Content Blocks

#### 変数のスコープとコンテンツブロック

The block of content passed to a mixin are evaluated in the scope where the block is defined,
not in the scope of the mixin. This means that variables local to the mixin **cannot** be used
within the passed style block and variables will resolve to the global value:

Mixinに渡されたコンテンツブロックは、mixinのスコープではなく、ブロックが定義された箇所のスコープで評価されます。どういうことかというと、mixinローカルの変数はmixinに渡されるスタイルブロック内で利用 **できない** のです。この時、スタイルブロック内の変数はグローバル値として解決されます。

    $color: white;
    @mixin colors($color: blue) {
      background-color: $color;
      @content;
      border-color: $color;
    }
    .colors {
      @include colors { color: $color; }
    }

Compiles to:

これはこのようにコンパイルされます。

    .colors {
      background-color: blue;
      color: white;
      border-color: blue;
    }

Additionally, this makes it clear that the variables and mixins that are used within the
passed block are related to the other styles around where the block is defined. For example:

この仕様によって、mixinに渡されたブロック内で使われる変数とmixinは、ブロックが定義された箇所のスタイルに関係することが明白になります。たとえば、次のようなコードの場合です。

    #sidebar {
      $sidebar-width: 300px;
      width: $sidebar-width;
      @include smartphone {
        width: $sidebar-width / 3;
      }
    }
