+  元文書: [https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#function-directives-function_directives](https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#function-directives-function_directives)

## Functionディレクティブ {#function_directives}

Sassでは自分でファンクションを定義し、どのような値、スクリプトのコンテキストの中でも使うことができます。例：

```scss
$grid-width: 40px;
$gutter-width: 10px;

@function grid-width($n) {
  @return $n * $grid-width + ($n - 1) * $gutter-width;
}

#sidebar { width: grid-width(5); }
```

このようになります:

```css
#sidebar {
  width: 240px; }
```

見ての通り、ファンクションはどんなグローバル変数にもアクセスでき、ミックスインと同じように引数を受け入れられます。
ファンクションはいくつかの命令を含んでおり、ファンクションの値を返すように設定するには、`@return`を呼び出す必要があります。

ミックスインの仕様と同様に、キーワード引数を使うSass定義ファンクションを呼び出すことができます。
上述の例ではこのようにファンクションを呼び出すこともできました。：

```scss
#sidebar { width: grid-width($n: 5); }
```

名前の衝突を避けるため、またあなたのスタイルシートを読む人が、SassかCSSかの見分けがつくように、自作のファンクションには接頭辞をつけることを推奨します。
例えば、もしあなたがACMEコーポレーションで働いてるとしたら、ファンクションに`-acme-grid-width`と名前をつけると良いかもしれません。

またユーザー定義のファンクションは、ミックスインと同様に[可変長引数](#variable_arguments)をサポートしています。