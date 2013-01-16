+  元文書: [https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#comments---and--comments](https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#comments---and--comments)

## コメント:: `/* */` と `//` {#comments}

Sass supports standard multiline CSS comments with `/* */`,
as well as single-line comments with `//`.
The multiline comments are preserved in the CSS output where possible,
while the single-line comments are removed.
Sassは標準の`/* */`を用いた複数行のCSSコメント`/* */`も、`//`を用いた単一行のコメントもサポートしています。
複数行コメントは出力されたCSSの中で可能なかぎり維持しますが、一方で単一行コメントは除かれます。

例:

    /* This comment is
     * several lines long.
     * since it uses the CSS comment syntax,
     * it will appear in the CSS output. */
    /* このコメントは
     * 数行に渡ります。
     * これはCSSコメントの構文として使われるので、
     * 出力されるCSSに出現します。 */
    body { color: black; }

    // These comments are only one line long each.
    // They won't appear in the CSS output,
    // since they use the single-line comment syntax.
    // これらのコメントは各一行だけです。
    // これは単一行コメントの構文として使われるので、
    // 出力されるCSSに出現しません。
    a { color: green; }

コンパイル結果:

    /* This comment is
     * several lines long.
     * since it uses the CSS comment syntax,
     * it will appear in the CSS output. */
    /* このコメントは
     * 数行に渡ります。
     * これはCSSコメントの構文として使われるので、
     * 出力されるCSSに出現します。 */
    body {
      color: black; }

    a {
      color: green; }

When the first letter of a comment is `!`, the comment will be interpolated
and always rendered into css output even in compressed output modes. This is useful for adding Copyright notices to your generated CSS.
コメントの一文字目を`!`にした時は、compressedモードでCSSが出力する場合でもそのコメントは差し込まれ、常にレンダリングされます。これは生成したCSS内にコピーライト表示を追加したい場合に役立ちます。
