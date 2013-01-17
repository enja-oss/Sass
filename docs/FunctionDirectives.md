+  元文書: [https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#function-directives-function_directives](https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#function-directives-function_directives)

## ファンクション命令型 {#function_directives}

It is possible to define your own functions in sass and use them in any
value or script context. For example:

Sassでは自分でファンクションを定義し、どのような値、スクリプトのコンテキストの中でも使うことができます。


    $grid-width: 40px;
    $gutter-width: 10px;

    @function grid-width($n) {
      @return $n * $grid-width + ($n - 1) * $gutter-width;
    }

    #sidebar { width: grid-width(5); }

このようになります:

    #sidebar {
      width: 240px; }

As you can see functions can access any globally defined variables as well as
accept arguments just like a mixin. A function may have several statements
contained within it, and you must call `@return` to set the return value of
the function.

見ての通り、ファンクションはどんなグローバル変数にもアクセスでき、またミックスインと同様に引数を受け付けることができます。
ファンクションはいくつかの命令その中に含むかもしれませんが、
`@return`で指定したファンクションの返り値を呼び出さないといけません。

As with mixins, you can call Sass-defined functions using keyword arguments.
In the above example we could have called the function like this:

ミックスインと同様に、キーワード引数を使うSassファンクションを呼び出すことができます。
前述の例は、次のように呼び出すことができた:

    #sidebar { width: grid-width($n: 5); }

It is recommended that you prefix your functions to avoid naming conflicts
and so that readers of your stylesheets know they are not part of Sass or CSS. For example, if you work for ACME Corp, you might have named the function above `-acme-grid-width`.

名前の衝突を避けるため、またあなたのスタイルシートを読む人が、SassかCSSかの見分けがつくように、自作のファンクションには接頭辞をつけることを推奨します。
例えば、もしあなたがACMEコーポレーションで働いてるとしたら、ファンクションに`-acme-grid-width`と名前をつけると良いかもしれません。

User-defined functions also support [variable arguments](#variable_arguments)
in the same way as mixins.

またユーザー定義のファンクションは、ミックスインと同様に[可変長引数](#variable_arguments)をサポートしています。