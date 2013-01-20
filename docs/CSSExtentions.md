+  元文書: [https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#css-extensions](https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#css-extensions)

## CSS拡張

### ネストルール

Sassは相互にネストされたCSSルールを許容しています。
内側のルールは、外側のセレクタのルールの中でのみ適用されます。

例:

```scss
#main p {
  color: #00ff00;
  width: 97%;

  .redbox {
    background-color: #ff0000;
    color: #000000;
  }
}
```

コンパイル結果:

```css
#main p {
  color: #00ff00;
  width: 97%; }
  #main p .redbox {
    background-color: #ff0000;
    color: #000000; }
```

これは親セレクタの重複を避けることができ、たくさんのネストされたセレクタで作られた複雑なCSS設計をもっとシンプルにすることができます。

例:

```scss
#main {
  width: 97%;

  p, div {
    font-size: 2em;
    a { font-weight: bold; }
  }

  pre { font-size: 3em; }
}
```

コンパイル結果:

```css
#main {
  width: 97%; }
  #main p, #main div {
    font-size: 2em; }
    #main p a, #main div a {
      font-weight: bold; }
  #main pre {
    font-size: 3em; }
```

### 親セレクタの参照: `&`

デフォルトの方法以外の方法でネストルールの親セレクタを参照すると便利な場合があります。

例えば、マウスオーバーされたセレクタや、body要素に特定のクラスを持たせた時に、特別なスタイルを適用したい場合などです。

このような場合は、親セレクタとして`&`を挿入することで、明示的に指定することができます。

例:

```scss
a {
  font-weight: bold;
  text-decoration: none;
  &:hover { text-decoration: underline; }
  body.firefox & { font-weight: normal; }
}
```

コンパイル結果:

```css
a {
  font-weight: bold;
  text-decoration: none; }
  a:hover {
    text-decoration: underline; }
  body.firefox a {
    font-weight: normal; }
```

`&`はCSS内で親セレクタとして置き換えられます。

これはネストが深いルールで書かれていても、`&`が置き換えられる前に、親セレクタが完全に分析されているということを意味します。

例:

```scss
#main {
  color: black;
  a {
    font-weight: bold;
    &:hover { color: red; }
  }
}
```

コンパイル結果:

```css
#main {
  color: black; }
  #main a {
    font-weight: bold; }
    #main a:hover {
      color: red; }
```

### プロパティのネスト

CSSは"名前空間"に属しているプロパティをかなりの数持っています。
例えば、`font-family`、 `font-size`、 そして `font-weight` は、すべて`font`名前空間のプロパティです。

CSSでは、同じ名前空間のプロパティの集合を設定したい場合、その都度タイプしなければいけません。

Sassはこのような場合のためにショートカットを用意しています。:
たった1回だけ名前空間を書いて、その中でサブプロパティをネストしていくだけです。

例:

```scss
.funky {
  font: {
    family: fantasy;
    size: 30em;
    weight: bold;
  }
}
```

コンパイル結果:

```css
.funky {
  font-family: fantasy;
  font-size: 30em;
  font-weight: bold; }
```

名前空間のプロパティ自体も値を持つことができます。

例:

```scss
.funky {
  font: 2px/3px {
    family: fantasy;
    size: 30em;
    weight: bold;
  }
}
```

コンパイル結果:

```css
.funky {
  font: 2px/3px;
    font-family: fantasy;
    font-size: 30em;
    font-weight: bold; }
```

### プレースホルダセレクタ: `%foo`

Sassは"プレースホルダセレクタ"と呼ばれる特殊なセレクタをサポートしています。

これらは`#`や`.`を`%`に置き換えたことを除けば、クラスやIDセレクタと同じように見えます。

上記のことは[`@extend` ディレクティブ](#extend)と一緒に使わなければいけないという意味になります；
詳しい情報は[`@extend`-Only Selectors](#placeholders)を参照してください。

`@extend`を全く使わずにプレースホルダセレクタを使ったルールセット自体は、CSSとしてレンダリングされません。
