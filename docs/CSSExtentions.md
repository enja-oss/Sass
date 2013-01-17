+  元文書: [https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#css-extensions](https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#css-extensions)

## CSS拡張

### ネストルール

Sass allows CSS rules to be nested within one another.
The inner rule then only applies within the outer rule's selector.
Sassは相互にネストされたCSSルールを許容しています。
内側のルールは、外側のセレクタのルールの中でのみ適用されます。

例:

    #main p {
      color: #00ff00;
      width: 97%;

      .redbox {
        background-color: #ff0000;
        color: #000000;
      }
    }

コンパイル結果:

    #main p {
      color: #00ff00;
      width: 97%; }
      #main p .redbox {
        background-color: #ff0000;
        color: #000000; }

This helps avoid repetition of parent selectors,
and makes complex CSS layouts with lots of nested selectors much simpler.
これは親セレクタの重複を避け、そしてたくさんのネストされたセレクタで作られた複雑なCSS設計をもっとシンプルにすることができます。

例:

    #main {
      width: 97%;

      p, div {
        font-size: 2em;
        a { font-weight: bold; }
      }

      pre { font-size: 3em; }
    }

コンパイル結果:

    #main {
      width: 97%; }
      #main p, #main div {
        font-size: 2em; }
        #main p a, #main div a {
          font-weight: bold; }
      #main pre {
        font-size: 3em; }

### Referencing Parent Selectors: `&`
### 親セレクタの参照: `&`

Sometimes it's useful to use a nested rule's parent selector
in other ways than the default.
時々、デフォルトの方法とは異なる方法で
ネストルールの親セレクタを参照すると便利です。

For instance, you might want to have special styles
for when that selector is hovered over
or for when the body element has a certain class.
例えば、マウスオーバーされたセレクタや、body要素に特定のクラスを持たせた時に、特別なスタイルを適用したいことがあるでしょう。

In these cases, you can explicitly specify where the parent selector
should be inserted using the `&` character.
そのような場合は、親セレクタに`&`を挿入することによって、明示的に指定することができます。

例:

    a {
      font-weight: bold;
      text-decoration: none;
      &:hover { text-decoration: underline; }
      body.firefox & { font-weight: normal; }
    }

コンパイル結果:

    a {
      font-weight: bold;
      text-decoration: none; }
      a:hover {
        text-decoration: underline; }
      body.firefox a {
        font-weight: normal; }

`&` will be replaced with the parent selector as it appears in the CSS.
CSS上で`&`は親セレクタに置き換えられます。

This means that if you have a deeply nested rule,
the parent selector will be fully resolved
before the `&` is replaced.
これはもしネストが深いルールで書かれた場合でも、`&`が親セレクタに置き換えられる前に、深くなったルールを完全に解消してくれるということです。

例:

    #main {
      color: black;
      a {
        font-weight: bold;
        &:hover { color: red; }
      }
    }

コンパイル結果:

    #main {
      color: black; }
      #main a {
        font-weight: bold; }
        #main a:hover {
          color: red; }

### Nested Properties
### プロパティのネスト

CSS has quite a few properties that are in "namespaces;"
for instance, `font-family`, `font-size`, and `font-weight`
are all in the `font` namespace.
CSSには相当数の"名前空間"を持つプロパティがある。
例えば、`font-family`、 `font-size`、 そして `font-weight` は、すべて`font`名前空間のプロパティです。

In CSS, if you want to set a bunch of properties in the same namespace,
you have to type it out each time.
CSS上で、もし同じ名前空間のプロパティの集まりをセットしたい場合、何度もその名前空間をタイプしなければいけません。

Sass provides a shortcut for this:
just write the namespace once,
then nest each of the sub-properties within it.
Sassはそのような場合のためのショートカット機能を備えている:
名前空間を一度書き、その中でサブプロパティをネストして書くだけです。


例:

    .funky {
      font: {
        family: fantasy;
        size: 30em;
        weight: bold;
      }
    }



    .funky {
      font-family: fantasy;
      font-size: 30em;
      font-weight: bold; }

The property namespace itself can also have a value.
名前空間プロパティ自身も値を持つことができます。

例:

    .funky {
      font: 2px/3px {
        family: fantasy;
        size: 30em;
        weight: bold;
      }
    }

コンパイル結果:

    .funky {
      font: 2px/3px;
        font-family: fantasy;
        font-size: 30em;
        font-weight: bold; }

### プレースホルダセレクタ: `%foo`

Sass supports a special type of selector called a "placeholder selector".
Sassは"プレースホルダセレクタ"と呼ばれる特殊なセレクタをサポートしています。

These look like class and id selectors, except the `#` or `.` is replaced by `%`.
これらはクラスセレクタ、IDセレクタの`.` または `#` を除き、`%`に置き換えた見た目になっています。

They're meant to be used with the [`@extend` directive](#extend);
for more information see [`@extend`-Only Selectors](#placeholders).
プレースホルダセレクタは[`@extend`型](#extend)と一緒に使わなければいけない;
詳しい情報は[`@extend`-Only Selectors](#placeholders)を参照してください。

On their own, without any use of `@extend`, rulesets that use placeholder selectors
will not be rendered to CSS.
`@extend`を使わずにそのままプレースホルダセレクタを使ったルールセット自身は、CSSとしてレンダリングされません。

