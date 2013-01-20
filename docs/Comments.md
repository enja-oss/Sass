+  元文書: [https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#comments---and--comments](https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#comments---and--comments)

## コメント:: `/* */` と `//` {#comments}

SassはCSS標準の`/* */`を使った複数行コメントも、`//`を使った単一行のコメントもサポートしています。
複数行コメントは可能であれば、出力されたCSSに保存されますが一方、単一行コメントは削除されます。

例:

```scss
/* このコメントは
 * 数行に渡ります。
 * CSSコメントの構文を使っているので、
 * 出力されたCSSに表示されます。 */
body { color: black; }

// これらのコメントは各一行だけです。
// 単一行コメントの構文を使っているので、
// 出力されるCSSには表示されません。
a { color: green; }
```

コンパイル結果:

```css
/* このコメントは
 * 数行に渡ります。
 * これはCSSコメントの構文として使われるので、
 * 出力されるCSSに出現します。 */
body {
  color: black; }

a {
  color: green; }
```

コメントの一文字目を`!`にした時は、compressedモードでCSSが出力する場合でもそのコメントは差し込まれ、常にレンダリングされます。これは生成したCSS内にコピーライト表示を追加したい場合に役立ちます。
