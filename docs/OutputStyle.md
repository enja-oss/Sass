+  元文書: [sass/doc-src/SASS_REFERENCE.md at f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2 · nex3/sass · GitHub](https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#output-style "sass/doc-src/SASS_REFERENCE.md at f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2 · nex3/sass · GitHub")

## 出力形式　[原文](http://sass-lang.com/docs/yardoc/file.SASS_REFERENCE.html#output_style)

Sassがデフォルトで出力するCSSスタイルは非常に良いですし、
文書構造を反映してはいますが、好みやニーズは様々なので
Sassではいくつかの出力形式をサポートしています。

Sassでは[`:style`オプション](#style-option)を設定するか、
コマンドラインのフラグに`--style`を使うことで、
4つの異なった出力スタイルを選ぶことができます。

### `:nested`

ネストスタイルは、CSSスタイルやスタイリングされたHTMLドキュメントの構造を反映しているので、
Sassのデフォルトスタイルとなっています。
各プロパティは1行ですが、インデントは一定ではありません。
各ルールは、ネストがどれだけ深いかにしたがってインデントされます。

例:

    #main {
      color: #fff;
      background-color: #000; }
      #main p {
        width: 10em; }

    .huge {
      font-size: 10em;
      font-weight: bold;
      text-decoration: underline; }

ネストスタイルは、大きいCSSファイルを見る際にとても便利です：
ファイルの構造を実際には何も読まずに、簡単に把握できます。

### `:expanded`

エキスパンドスタイルは、各プロパティとルールが1行を占めていて、
さらに典型的な人が作ったCSSスタイル的です。
プロパティはルール内でインデントされますが、ルールは特別な方法ではインデントされません。

例:

    #main {
      color: #fff;
      background-color: #000;
    }
    #main p {
      width: 10em;
    }

    .huge {
      font-size: 10em;
      font-weight: bold;
      text-decoration: underline;
    }

### `:compact`

コンパクトスタイルはネストやエキスパンドよりもスペースを取りません。
また、プロパティよりもセレクタがより目を引く形式です。
各CSSルールは全てのプロパティを定義した1行だけになります。
ネストされたルールは、空行なしでその次の行に置かれる一方、
独立したルールのグループは、それぞれ空行を置かれます。

例:

    #main { color: #fff; background-color: #000; }
    #main p { width: 10em; }

    .huge { font-size: 10em; font-weight: bold; text-decoration: underline; }

### `:compressed`

コンプレスドスタイルは、セレクタを分けるために必要な空白やファイルの最後の空行を無くし、
可能な限りスペースを最小にします。
また、最小限の色の記述を選択するというような、地味な圧縮も含んでいます。
これは人の可読性のためという意図ではありません。

例:

    #main{color:#fff;background-color:#000}#main p{width:10em}.huge{font-size:10em;font-weight:bold;text-decoration:underline}
