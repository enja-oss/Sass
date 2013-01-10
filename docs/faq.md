+  元文書: [sass/doc-src/FAQ.md at f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2 · nex3/sass · GitHub](https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/FAQ.md "sass/doc-src/FAQ.md at f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2 · nex3/sass · GitHub")

# よくある質問　[原文](http://sass-lang.com/docs/yardoc/file.FAQ.html)

* 目次
{:toc}

## Can I use a variable from my controller in my Sass file?

## 自分のコントローラの変数を Sass ファイルで使えますか？

{#q-ruby-code}

No. Sass files aren't views.
They're compiled once into static CSS files,
then left along until they're changed and need to be compiled again.
Not only don't you want to be running a full request cycle
every time someone requests a stylesheet,
but it's not a great idea to put much logic in there anyway
due to how browsers handle them.

できません。Sass のファイルはビューではありません。
Sass のファイルは一度静的な CSS ファイルにコンパイルされ、変更されるまでそのままです。
また、変更があれば再度コンパイルされなければなりません。
誰かがスタイルシートをリクエストするたびに、この全工程を実行したくはないでしょうし、
ブラウザごとのスタイルシートの扱い方を考えれば、この部分にロジックを詰め込むのは
良いアイデアではありません。

If you really need some sort of dynamic CSS,
you can define your own {Sass::Script::Functions Sass functions} using Ruby
that can access the database or other configuration.
*Be aware when doing this that Sass files are by default only compiled once
and then served statically.*

もし、本当に動的な感じの CSS が必要なら、Ruby を使ってデータベースやその他の設定を参照するような
自作の {Sass::Script::Functions Sass関数} を作りましょう。
*これを行うとき、デフォルトでは Sass は一回しかコンパイルを行わず、以降は静的なままである
ということに注意してください。*

If you really, really need to compile Sass on each request,
first make sure you have adequate caching set up.
Then you can use {Sass::Engine} to render the code,
using the {file:SASS_REFERENCE.md#custom-option `:custom` option}
to pass in data that {Sass::Script::Functions::EvaluationContext#options can be accessed}
from your Sass functions.

本当に、本当に、リクエストのたびに Sass でコンパイルしたい場合ですが、まずキャッシュの設定を
十分に済ませていることを確認してください。
次に {Sass::Engine} で、コードを生成します。その際には、あなたの自作した Sass関数から
{Sass::Script::Functions::EvaluationContext#options アクセスできる} 
データを受け取れるように
{file:SASS_REFERENCE.md#custom-option `:custom` オプション}を指定します。

# You still haven't answered my question!

# まだ質問に全部答えてもらってないんですけど？

Sorry! Try looking at the [Sass](http://sass-lang.com/docs/yardoc/file.SASS_REFERENCE.html) reference,
If you can't find an answer there,
feel free to ask in `#sass` on irc.freenode.net
or send an email to the [mailing list](http://groups.google.com/group/sass-lang).

ごめんなさい！ [Sass](http://sass-lang.com/docs/yardoc/file.SASS_REFERENCE.html)
レファレンスを読んでみてください。
答えが見つからない場合は irc.freenode.net に `#sass` で気軽に質問してみてね。
[メーリングリスト](http://groups.google.com/group/sass-lang)にメールも送れます。