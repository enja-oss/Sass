+  元文書: [sass/doc-src/SASS_REFERENCE.md at f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2 · nex3/sass · GitHub](https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#sassscript-sassscript "sass/doc-src/SASS_REFERENCE.md at f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2 · nex3/sass · GitHub")

## SassScript [原文](http://sass-lang.com/docs/yardoc/file.SASS_REFERENCE.html#sassscript)

プレーンなCSSプロパティ構文に加えて、
SassはSassScriptと呼ぶ小型の機能拡張群を扱うことができます。
SassScriptは、変数や計算そして追加機能をプロパティで利用できるようにしています。
SassScriptはあらゆるプロパティの値で利用できます。

SassScriptは、セレクタやプロパティ名を生成するときにも利用でき、
それらは[ミックスイン](#mixins)を記述するときに有用です。
これは[補間](#interpolation_)によって行われます。

### 対話式シェル

SassScriptは対話式シェルを使って簡単に試せます。
シェルを起動させるにはコマンドラインで `-i` をつけてsassを実行します。
プロンプトが表示されたら、実行して結果を出力する規定されたSassScript構文をどれでも入力します。

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

### 変数: `$` {#variables_}

SassScriptのもっとも簡単な利用法は変数を使うことです。
変数はドル記号から始まり、CSSプロパティのようにセットされます。

```scss
$width: 5em;
```

それらはプロパティの中で参照できます。

```scss
#main {
  width: $width;
}
```

変数は、定義を行ったネストされたセレクタのレベルの内側でのみ有効になります。
すべてのネストされたセレクタの外側で定義されれば、全ての場所でも有効となります。

以前は変数の接頭辞として`!`が使われていました。
これは今も使えますが、非推奨のため警告が出力されます。
`$`が推奨された記述です。

以前、変数の定義には`:`よりもむしろ`=`が定義時に利用されました。
この記法は今も使えますが、非推奨のため警告が出力されます。
`:`が推奨された記述です。


### データタイプ

SassScriptは6つの主なデータタイプをサポートしています：

* 数値 (例 `1.2`, `13`, `10px`)
* 文字列、クオートはありなしどちらでも(例 `"foo"`, `'bar'`, `baz`)
* 色 (例 `blue`, `#04a3f9`, `rgba(255, 0, 0, 0.5)`)
* ブール値 (例 `true`, `false`)
* null値 (例 `null`)
* 値のリスト、スペースかカンマで区切られます (例 `1.5em 1em 0 2em`, `Helvetica, Arial, sans-serif`)

SassScriptは、その他にもユニコードの範囲内の文字列や
`!important`宣言といった全てのCSSのプロパティの値のタイプをサポートします。
どういう形であれ、これらに対して特別な対応は必要ありません。
これらはちゃんとクオートされていない文字列として扱われます。

#### 文字列 {#sass-script-strings}

CSSでは2通りの文字列が規定されています：
`"Lucida Grande"`や`'http://sass-lang.com'`のようにクオートがついているものと、
`sans-serif`や`bold`のようにクオートがつかないものです。
SassScriptではどちらも認識します、
また一般的にはSassの文書の中で片方のタイプの文字列が利用された場合には、
出力したCSSでもそのタイプの文字列が適用されます。

とはいえ、一つ例外があります：
[`#{}` 補間](#interpolation_)を利用した場合には、
クオートされた文字列からクオートが取り除かれます。
これは[ミックスイン](#mixins)でのセレクタ名などを扱うのを容易にします。

例えば

```scss
@mixin firefox-message($selector) {
  body.firefox #{$selector}:before {
    content: "Hi, Firefox users!";
  }
}

@include firefox-message(".header");
```

は以下にコンパイルされます：

```css
body.firefox .header:before {
  content: "Hi, Firefox users!"; }
```

これは[非推奨である `=` のプロパティ構文](#sassscript)を利用した場合にも注意する必要があり、
全ての文字列はクオートが記述されているかどうかにかかわらず、
クオートされていないものとして扱われます。

#### リスト

リストは`margin: 10px 15px 0 0`や`font-face: Helvetica, Arial, sans-serif`のようなCSSの定義の値を表す方法です。
リストは、他とスペースかカンマどちらかで区切られた値のセットそのものです。
さらには、個々の値もリストとして扱われます。それらはひとつの項目からなるリストとなります。

リスト自体は、特に何かをするわけではありません、
しかし[Sassのリスト関数](Sass/Script/Functions.html#list-functions)によって便利に使えます。
{Sass::Script::Functions#nth nth function}はリストの項目にアクセスできますし、
{Sass::Script::Functions#join join function}は複数のリストを一つに結合できます、
また、{Sass::Script::Functions#append append function}はリストに項目を追加できます。
[`@each` ルール](#each-directive)を使えば、リストの各項目にスタイルを追加できます。

リストにはシンプルな値だけでなく、他のリストを含められます。
例えば、`1px 2px, 5px 6px`は、
`1px 2px`のリストと`5px 6px`のリストからなる、2つの項目を持つリストとなります。
もし、内側のリストと外側のリストが同じ区切り文字を持つ場合は、
内側のリストの最初と最後にカッコをつけて明示する必要があります。
例えば、`(1px 2px) (5px 6px)`は、
`1px 2px`のリストと`5px 6px`のリストからなる、2つの項目を持つリストとなります。
違いは、外側のリストがカンマ区切りだった前の例に対し、
この例は外側のリストがスペース区切りということです。

リストがCSSに置き換わるときには、
CSSではカッコが解釈されないため、Sassはカッコを付加しません。
つまり、`(1px 2px) (5px 6px)`と`1px 2px 5px 6px`は、
出力されたCSSでは、同じように現れます。
しかし、Sass上では同じというわけではありません。
前者は2つのリストを内包したリストであり、
一方後者は4つの数字を内包したリストです。

リストはひとつも値を持たないということもできます。
そのようなリストは`()`で表されます。
それらは直接CSSに出力されません；
もし、例えば  `font-family: ()`を試した場合、Sassはエラーを発生させます。
もしリストが、`1px 2px () 3px`または`1px 2px null 3px`といった
空のリストやヌル値を内包する場合、
空のリストやヌル値は、内包しているリストが
CSSに置き換えられる前に削除されます。

### 演算

どの変数型も等値演算(`==` and `!=`)をサポートしています。
さらに、それぞれに独自にサポートしている演算があります。

#### 数値演算

SassScriptは数値型では基本的な四則演算をサポートしています、
(`+`, `-`, `*`, `/`, `%`)
さらに演算が可能であれば異なる単位のものでも自動的に変換します。

```scss
p {
  width: 1in + 8pt;
}
```

は以下のようにコンパイルされます。

```css
p {
  width: 1.111in; }
```

関係演算子
(`<`, `>`, `<=`, `>=`)
も数値型でサポートされますし、
等値演算
(`==`, `!=`)
も全ての変数型でサポートされます。

##### 割り算と `/`
{#division-and-slash}

CSSはプロパティの値で数値の区切りとして`/`が利用されています。
SassScriptはCSSのプロパティ構文の拡張なので、
これに対応する必要がありますが、一方で割り算で`/`が利用できるようにもしています。
というわけでデフォルトでは、SassScriptで2つの数字が`/`で区切られていた場合、
結果としてCSSのやり方で出力されることを意味します。

しかしながら、3つの条件下においては`/`は割り算として解釈されます。
その条件は割り算が実際に使用される大部分のケースを対象としています。
それは

1. 値、もしくはその一部が変数に保持されているとき
2. 値がカッコで囲まれているとき
3. 値が四則演算の一部に存在しているとき

例えば

```scss
p {
  font: 10px/8px;             // Plain CSS, no division
  $width: 1000px;
  width: $width/2;            // Uses a variable, does division
  height: (500px/2);          // Uses parentheses, does division
  margin-left: 5px + 8px/2px; // Uses +, does division
}
```

は以下のようにコンパイルされます。

```css
p {
  font: 10px/8px;
  width: 500px;
  height: 250px;
  margin-left: 9px; }
```

もし、CSSの`/`と一緒に変数を利用したい場合は、
挿入するときに`#{}`を利用します。

例えば

```scss
p {
  $font-size: 12px;
  $line-height: 30px;
  font: #{$font-size}/#{$line-height};
}
```

は、以下のようにコンパイルされます。

```css
p {
  font: 12px/30px; }
````

#### 色の演算

全ての四則演算は切り分けて動作することで、
色の値も対象にしています。
これは、順番に赤、緑、青の要素ごとに
計算するという意味です。

例えば

```scss
p {
  color: #010203 + #040506;
}
```

は`01 + 04 = 05`、`02 + 05 = 07`そして`03 + 06 = 09`と計算し、
以下のようにコンパイルします。

```css
p {
  color: #050709; }
```

しかし多くの場合、{Sass::Script::Functions color functions}のほうが、
色演算よりも便利です。

四則演算は色の値と数値の間でも動作します、
こちらも切り分けます。

例えば

```scss
p {
  color: #010203 * 2;
}
```

computes `01 * 2 = 02`, `02 * 2 = 04`, and `03 * 2 = 06`,
and is compiled to:

は`01 * 2 = 02`、`02 * 2 = 04`そして`03 * 2 = 06`と計算し、
以下のようにコンパイルします。

```css
p {
  color: #020406; }
```

注意点としては、( {Sass::Script::Functions#rgba rgba}
または{Sass::Script::Functions#hsla hsla}機能で生成された)アルファチャンネル
を含む色の値で四則演算を行う場合、アルファの値は同じである必要があります。
演算はアルファの値には作用しません。

例えば

```scss
p {
  color: rgba(255, 0, 0, 0.75) + rgba(0, 255, 0, 0.75);
}
```

は以下のようにコンパイルされます。

```scss
p {
  color: rgba(255, 255, 0, 0.75); }
```

アルファチャンネルは{Sass::Script::Functions#opacify opacify}と
{Sass::Script::Functions#transparentize transparentize}の機能を利用して
調整できます。

例えば

```scss
$translucent-red: rgba(255, 0, 0, 0.5);
p {
  color: opacify($translucent-red, 0.3);
  background-color: transparentize($translucent-red, 0.25);
}
```

は以下のようにコンパイルされます。

```css
p {
  color: rgba(255, 0, 0, 0.9);
  background-color: rgba(255, 0, 0, 0.25); }
```

IEのフィルターは全色がアルファレイヤーに含まれる必要があります、
そして#AABBCCDDの厳密な書式になります。
{Sass::Script::Functions#ie_hex_str ie_hex_str}機能を利用することで
より簡易に色の変換を行えます。

例えば

```scss
$translucent-red: rgba(255, 0, 0, 0.5);
$green: #00ff00;
div {
  filter: progid:DXImageTransform.Microsoft.gradient(enabled='false', startColorstr='#{ie-hex-str($green)}', endColorstr='#{ie-hex-str($translucent-red)}');
}
```

は以下のようにコンパイルされます。

```css
div {
  filter: progid:DXImageTransform.Microsoft.gradient(enabled='false', startColorstr=#FF00FF00, endColorstr=#80FF0000);
}
```

#### 文字列演算

`+` の演算は文字列の連結に利用できます:

```scss
p {
  cursor: e + -resize;
}
```

は以下のようにコンパイルされます。

```css
p {
  cursor: e-resize; }
```

注意点としては、もしクオートされた文字列にクオートされていない文字列が追加された場合
（`+`の左側がクオートされた文字列）、
演算の結果はクオートされた文字列になります。
同様に、もしクオートされていない文字列にクオートされた文字列が追加された場合
（`+`の左側がクオートされていない文字列）、
演算の結果はクオートされていない文字列になります。

例えば

```scss
p:before {
  content: "Foo " + Bar;
  font-family: sans- + "serif";
}
```

は以下のようにコンパイルされます。

```css
p:before {
  content: "Foo Bar";
  font-family: sans-serif; }
```

デフォルトでは、2つの値が連続している場合、それらはスペースで連結されます。

```scss
p {
  margin: 3px + 4px auto;
}
```

は以下のようにコンパイルされます。

```css
p {
  margin: 7px auto; }
```

テキストの文字列の中では、#{}スタイルの補間は
文字列の中で直接値として利用できます。

```scss
p:before {
  content: "I ate #{5 + 10} pies!";
}
```

は以下のようにコンパイルされます。

```css
p:before {
  content: "I ate 15 pies!"; }
```

ヌル値は文字列補間で空文字として扱われます。

```scss
$value: null;
p:before {
  content: "I ate #{$value} pies!";
}
```

は以下のようにコンパイルされます。

```css
p:before {
  content: "I ate  pies!"; }
```

#### ブール値演算

SassScriptは、`and` `or` `not` 演算子がブール値で利用できます。

#### リスト演算

リストは特殊な演算をサポートしていません。
そのかわりに、[リスト関数](Sass/Script/Functions.html#list-functions)
を利用することで操作できます。

### カッコ

カッコは演算の順序を操作することができます。

```scss
p {
  width: 1em + (2em * 3);
}
```

以下のようにコンパイルされます。

```css
p {
  width: 7em; }
```

### 関数

SassScriptではCSSの関数と同じ構文で利用できる、
いくつかの便利な関数を定義しています。

```scss
p {
  color: hsl(0, 100%, 50%);
}
```

は以下のようにコンパイルされます。

```css
p {
  color: #ff0000; }
```

#### キーワード引数

Sassの関数は明示的なキーワード引数を使うことでも呼び出せます。
先ほどの例はこのようにも記述できます。

```scss
p {
  color: hsl($hue: 0, $saturation: 100%, $lightness: 50%);
}
```

これは簡潔ではない一方で、スタイルシートを読みやすくしてくれます。
関数はより呼び出しが難しくなるようなこともなく多くの引数を扱える、
柔軟なインターフェースも提供しています。

名前付きの引数は任意の順番で渡すことができ、デフォルトの値の引数を省略することができます。
名前付き引数は変数名なので、アンダースコアとダッシュ（ハイフン）を交互に使用できます。

{Sass::Script::Functions} にすべてのSass関数とその引数名が紹介されています。
また、Rubyで独自の関数を定義する方法も書かれています。

### 補間: `#{}` {#interpolation_}

SassScriptの変数は、
セレクタやプロパティ名でも#{}の補間記法で利用できます。

```scss
$name: foo;
$attr: border;
p.#{$name} {
  #{$attr}-color: blue;
}
```

は以下のようにコンパイルされます。

```css
p.foo {
  border-color: blue; }
```

`#{}`を使うことで、プロパティの値に対してSassScriptを適用することも可能になります。
ほとんどの場合、変数よりも便利というわけではないですが、
`#{}`を利用することは、
どんな演算も通常のCSSと同様に扱われるということになります。

例えば

```scss
p {
  $font-size: 12px;
  $line-height: 30px;
  font: #{$font-size}/#{$line-height};
}
```

は以下のようにコンパイルされます。

```css
p {
  font: 12px/30px; }
```

### 変数の規定値: `!default`

変数が未定義であれば、
`!default`フラグを値の末尾に付属した割り当てを行えます。
これは、変数がすでに割り当てられていた場合、
再度割り当てられないことを意味しまします、
しかし、まだ値を保持していない場合は、割り当てが行われます。

例えば

```scss
$content: "First content";
$content: "Second content?" !default;
$new_content: "First time reference" !default;

#main {
  content: $content;
  new-content: $new_content;
}
```

は以下のようにコンパイルされます。

```css
#main {
  content: "First content";
  new-content: "First time reference"; }
```

変数がヌル値をもつ場合、値は未割り当てとして扱われ、!defaultの値が用いられます。

```scss
$content: null;
$content: "Non-null content" !default;

#main {
  content: $content;
}
```

は以下のようにコンパイルされます。

```css
#main {
  content: "Non-null content"; }
```
