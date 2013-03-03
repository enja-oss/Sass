+  元文書: [sass/doc-src/FAQ.md at f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2 · nex3/sass · GitHub](https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/FAQ.md "sass/doc-src/FAQ.md at f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2 · nex3/sass · GitHub")

# よくある質問　[原文](http://sass-lang.com/docs/yardoc/file.FAQ.html)

* 目次
{:toc}

## 自分のコントローラの変数をSassファイルで使えますか？

{#q-ruby-code}

できません。Sassのファイルはビューではありません。
一度、Sassファイルが静的なCSSファイルとしてコンパイルされれば、変更されて再コンパイルされるまではそのままです。
毎回誰かがスタイルシートをリクエストするたびに、この全行程を実行したくないだけでなく、
その部分に多くのロジックを詰めるのは、ブラウザ毎の処理方法によらず良いアイデアではありません。

ある種の動的なCSSが本当に必要なら、データベースや他の設定を参照できるRubyを使った
自作の{Sass::Script::Functions Sass関数}を定義することもできます。
*この実行時には、デフォルトでSassファイルは一度しかコンパイルせずに、静的に提供されることに注意してください。*

本当に、本当にリクエスト毎にSassのコンパイルが必要な場合、初めにキャッシュ設定が十分であることを確認してください。
そうしてから、コードを生成するために{Sass::Engine}を使い、
{file:SASS_REFERENCE.md#custom-option `:custom` オプション}を
自作のSass関数からの{Sass::Script::Functions::EvaluationContext#optionsにアクセスできる}データに渡すために使用できます。

# まだ質問に全部答えてもらってないんですけど？

ごめんなさい！ [Sass](http://sass-lang.com/docs/yardoc/file.SASS_REFERENCE.html)
レファレンスを読んでみてください。
そこで答えが見つからない場合は、irc.freenode.netの `#sass` で気軽に質問してみてるか、
[メーリングリスト](http://groups.google.com/group/sass-lang)でメールも送ってください。