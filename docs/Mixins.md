+ 元文書: [https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#mixin-directives-mixins](https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#mixin-directives-mixins)

## Mixinディレクティブ {#mixins}

ミックスイン (mixin) はスタイルシート内で再利用可能なスタイルを定義する仕組みです。これによって `.float-left` といったセマンティックではないクラスに頼る必要がなくなります。ミックスインはまた、すべてのCSSルールと、その他Sassで利用可能なものすべてを含められます。ミックスインはさらに[引数](#mixin-arguments)も取ることができ、利用すればほんのすこしのミックスインから多種多様なスタイルを生成できます。

### ミックスインの定義: `@mixin` {#defining_a_mixin}

ミックスインは `@mixin` ディレクティブにより定義されます。`@mixin` ディレクティブの後には、ミックスインの名前、[引数](#mixin-arguments)(任意)、そしてミックスインの内容を含むブロックが続きます。たとえば、`large-text` というミックスインは次のように定義します。

```scss
@mixin large-text {
  font: {
    family: Arial;
    size: 20px;
    weight: bold;
  }
  color: #ff0000;
}
```

ミックスインにはセレクタも含められ、またセレクタとプロパティの混在も可能です。さらに、セレクタには[親参照](#referencing_parent_selectors_)も含められます。

```scss
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
```

### ミックスインの読み込み: `@include` {#including_a_mixin}

ミックスインは `@include` ディレクティブから読み込みます。このディレクティブに続けてミックスインの名前を、続けて指定可能な場合は[引数](#mixin-arguments)を記述すると、ミックスインが定義したスタイルが現在のルールに読み込まれます。たとえば、次のようなコードがあります。

```scss
.page-title {
  @include large-text;
  padding: 4px;
  margin-top: 10px;
}
```

これはこのようにコンパイルされます。

```css
.page-title {
  font-family: Arial;
  font-size: 20px;
  font-weight: bold;
  color: #ff0000;
  padding: 4px;
  margin-top: 10px; }
```

ミックスインが直下にプロパティを定義しておらず、また親を参照していない場合は、他のどのルールの外側にも読み込めます (つまり、文書のルートです)。たとえば、次のようなコードがあります。

```scss
@mixin silly-links {
  a {
    color: blue;
    background-color: red;
  }
}

@include silly-links;
```

これはこのようにコンパイルされます。

```css
a {
  color: blue;
  background-color: red; }
```

ミックスインは他のミックスインも読み込めます。たとえば、次のようなコードがあります。

```scss
@mixin compound {
  @include highlighted-background;
  @include header-text;
}

@mixin highlighted-background { background-color: #fc0; }
@mixin header-text { font-size: 20px; }
```

子孫セレクタのみを定義するミックスインであれば、ドキュメントの最上位に問題なく含められます。

### 引数 {#mixin-arguments}

ミックスインはSassScriptの値を引数に取ることができます。引数はミックスインを読み込む際に渡され、ミックスインの中では変数として扱われます。

引数はミックスイン名の後に括弧を続け、その中に変数名として記述します。複数の引数がある場合はカンマで区切り記述します。ミックスインに引数を与えて
読み込むときは、定義したときと同じように記述します。たとえば、次のようなコードがあります。

```scss
@mixin sexy-border($color, $width) {
  border: {
    color: $color;
    width: $width;
    style: dashed;
  }
}

p { @include sexy-border(blue, 1in); }
```

これはこのようにコンパイルされます。

```css
p {
  border-color: blue;
  border-width: 1in;
  border-style: dashed; }
```

ミックスインの引数には規定値も指定できます。書き方は変数定義の構文と同じです。規定値はミックスインの読み込み時、引数が与えられなかった場合に用いられます。たとえば、次のようなコードがあります。

```scss
@mixin sexy-border($color, $width: 1in) {
  border: {
    color: $color;
    width: $width;
    style: dashed;
  }
}
p { @include sexy-border(blue); }
h1 { @include sexy-border(blue, 2in); }
```

これはこのようにコンパイルされます。

```css
p {
  border-color: blue;
  border-width: 1in;
  border-style: dashed; }

h1 {
  border-color: blue;
  border-width: 2in;
  border-style: dashed; }
```

#### キーワード引数

明示的なキーワード引数を利用しミックスインを読み込むこともできます。たとえば、先ほどの例は次のようにも書けます。

```scss
p { @include sexy-border($color: blue); }
h1 { @include sexy-border($color: blue, $width: 2in); }
```

あまり簡潔ではなくなりましたが、キーワードのおかげでスタイルシートが読みやすくなります。キーワード引数を使えば、ミックスインが多くの引数を取ったとしても呼び出しにくくならない、より柔軟なインターフェースとして表現できます。

キーワード引数はどんな順番で与えても構いません。また、規定値を持つ引数は省略できます。キーワード引数は変数名なので、アンダースコアとダッシュは区別されません。

#### 可変長引数

引数の数を定めないミックスインを定義したいこともあるでしょう。たとえば、ボックスシャドウを生成するミックスインであれば、シャドウの数に制限がないほうがよいでしょう。このようなケースに対応すべく、Sassでは「可変長引数」に対応しています。可変長引数はミックスイン定義の最後に置かれ、残りの引数すべてを[リスト](#lists)としてまとめます。可変長引数は普通の引数によく似ていますが、引数の最後に `...` とある点が異なります。たとえば、次のようなコードがあります。

```scss
@mixin box-shadow($shadows...) {
  -moz-box-shadow: $shadows;
  -webkit-box-shadow: $shadows;
  box-shadow: $shadows;
}

.shadows {
  @include box-shadow(0px 4px 5px #666, 2px 6px 10px #999);
}
```

これはこのようにコンパイルされます。

```css
.shadowed {
  -moz-box-shadow: 0px 4px 5px #666, 2px 6px 10px #999;
  -webkit-box-shadow: 0px 4px 5px #666, 2px 6px 10px #999;
  box-shadow: 0px 4px 5px #666, 2px 6px 10px #999;
}
```

可変長引数はミックスインを読み込む際にも利用できます。構文は同じで、読み込む際に複数の値をまとめたリストを展開し、各値が別々の引数に渡されるようにできるのです。たとえば、次のようなコードです。

```scss
@mixin colors($text, $background, $border) {
  color: $text;
  background-color: $background;
  border-color: $border;
}

$values: #ff0000, #00ff00, #0000ff;
.primary {
  @include colors($values...);
}
```

これはこのようにコンパイルされます。

```css
.primary {
  color: #ff0000;
  background-color: #00ff00;
  border-color: #0000ff;
}
```

可変長引数を使うと、ミックスインと他のスタイルを引数の名前を変えずに囲めます。こうすると、キーワード引数さえも、その囲ったミックスインに渡せるのです。たとえば、次のようなコードになります。

```scss
@mixin wrapped-stylish-mixin($args...) {
  font-weight: bold;
  @include stylish-mixin($args...);
}

.stylish {
  // $width 引数が "stylish-mixin" にキーワードとして渡される
  @include wrapped-stylish-mixin(#00ff00, $width: 100px);
}
```

### コンテンツブロックをミックスインに渡す {#mixin-content}

ミックスインにスタイルブロックを渡して、ミックスイン定義内のスタイルの中に読み込むこともできます。渡されたスタイルブロックはミックスイン内の `@content` ディレクティブが記述された場所に読み込まれます。この仕組みによって、セレクタとディレクティブの構成を抽象化できます。

たとえば、次のようなコードがあります。

```scss
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
```

これは次のようなコードを生成します。

```css
* html #logo {
  background-image: url(/logo.gif);
}
```

`.sass` ショートハンド構文では、次のように記述します。

```sass
=apply-to-ie6-only
  * html
    @content

+apply-to-ie6-only
  #logo
    background-image: url(/logo.gif)
```

**注:** `@content` がひとつ以上、もしくはループ内に記述された場合、ミックスインに渡されたスタイルブロックはつど複製されます。

#### 変数のスコープとコンテンツブロック

ミックスインに渡されたコンテンツブロックは、ミックスインのスコープではなく、ブロックが定義された箇所のスコープで評価されます。どういうことかというと、ミックスインローカルの変数はミックスインに渡されるスタイルブロック内で利用 **できない** のです。この時、スタイルブロック内の変数はグローバル値として解決されます。

```scss
$color: white;
@mixin colors($color: blue) {
  background-color: $color;
  @content;
  border-color: $color;
}
.colors {
  @include colors { color: $color; }
}
```

これはこのようにコンパイルされます。

```css
.colors {
  background-color: blue;
  color: white;
  border-color: blue;
}
```

この仕様によって、ミックスインに渡されたブロック内で使われる変数とミックスインは、ブロックが定義された箇所のスタイルに関係することが明白になります。たとえば、次のようなコードの場合です。

```scss
#sidebar {
  $sidebar-width: 300px;
  width: $sidebar-width;
  @include smartphone {
    width: $sidebar-width / 3;
  }
}
```
