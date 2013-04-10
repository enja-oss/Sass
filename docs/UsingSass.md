+  元文書: [sass/doc-src/SASS_REFERENCE.md at f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2 · nex3/sass · GitHub](https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#using-sass "sass/doc-src/SASS_REFERENCE.md at f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2 · nex3/sass · GitHub")

## Sassを使う

Sassは3つの方法で使用することができます: コマンドラインツール、スタンドアロンなRubyモジュール、そしてRuby on RailsやMerbを含む、Rackが使えるならどんなフレームワークでもプラグインとして使えます。以上全ての方法の最初のステップは、Sass gemをインストールすることです。

    gem install sass

Windowsを使用している場合は、最初に[Rubyのインストール](http://rubyinstaller.org/download.html)が必要になります。

Sassをコマンドラインで実行するには、このように使います。

    sass input.scss output.css

Sassにファイルの監視をさせSassファイルの変更するたびに、CSSファイルを更新させるようにすることもできます:

    sass --watch input.scss:output.css

ディレクトリにSassファイルが多い場合、Sassにディレクトリを丸ごと監視させることもできます。

    sass --watch app/sass:public/stylesheets

`sass --help`で全てのドキュメントが見れます。

Rubyのコード内でSassを使うのはとても簡単です。Sass gemをインストール後、`require "sass"`を実行し{Sass::Engine}を以下のようにして使います。

    engine = Sass::Engine.new("#main {background-color: #0000ff}", :syntax => :scss)
    engine.render #=> "#main { background-color: #0000ff; }\n"

### Rack/Rails/Merg プラグイン

Rails 3より前のバージョンのRailsでSassを有効にするためには、`environment.rb`に次の行を追加します。

    config.gem "sass"

Rails 3では、次の行をGemfileに代わりに追加します。

    gem "sass"

MerbでSassを有効にするには`config/dependencies.rb`に次の行を追加します。

    dependency "merb-haml"

RackアプリケーションでSassを有効にするには、config.ruに

    require 'sass/plugin/rack'
    use Sass::Plugin::Rack

と、追加します。

Sassのスタイルシートはviewsと同じようには動作しません。動的なコンテンツが含まれていないので、Sassファイルが更新された時にCSSが必要に応じて生成されます。デフォルトでは、`.sass`と`.scss`ファイルはpublic/stylesheets/sassに置かれています(これは[`:template_location`](#template_location-option)でカスタマイズ可能です)。そして、必要に応じて、public/stylesheets内に対応するCSSファイルがコンパイルされます。例えば、public/stylesheets/sass/main.scssはpublic/stylesheets/main.cssにコンパイルされるでしょう。

### キャッシュする

デフォルトでは、Sassはコンパイルされたテンプレートと[パーシャル](#partials)をキャッシュします。これはSassファイルの大きいコレクションの再コンパイルを劇的に高速化し、Sassのテンプレートが、全てのファイルが1つの大きいファイルの中で[`@import`](#import)されているような個別のファイルに分かれている場合に最も効果的に動作します。

フレームワークがなければ、Sassは`.sass-cache`ディレクトリ内にキャッシュされたテンプレートを置きます。RailsやMerbでは、`tmp/sass-cache`になります。ディレクトリは[`:cache_location`](#cache_location-option)でカスタマイズすることができます。Sassのキャッシュを全く必要としないなら、[`:cache`](#cache-option)オプションを`false`に設定してください。

### オプション

オプションはRailsの`environment.rb`やRackの`config.ru`にある{Sass::Plugin::Configuration#options Sass::Plugin#options}ハッシュによって設定できます。

    Sass::Plugin.options[:style] = :compact

もしくは、Merbの`init.rb`で`Merb::Plugin.config[:sass]`ハッシュを設定します。

    Merb::Plugin.config[:sass][:style] = :compact

もしくは、{Sass::Engine#initialize}にオプションのハッシュを渡します。関連する全てのオプションは、`sass`か`scss`のコマンドラインの実行時にフラグを通して利用可能です。利用可能なオプションは次の通りです。

{#style-option} `:style`
: CSSの出力スタイルを設定します。
  [Output Style](#output_style)を見てください。

{#syntax-option} `:syntax`
: 入力ファイルの構文で、`:sass`はインデントの構文、`:scss`はCSS拡張の構文です。
  これは{Sass::Engine}を初期化する場合にのみ利用可能です。
  {Sass::Plugin}を使う場合は、適切な構文が自動的に設定されます。
  デフォルトは`:sass`です。

{#property_syntax-option} `:property_syntax`
: インデント構文の文書で、プロパティの書式を1つに強制します。
  正しい構文が使われていない場合にエラーを投げます。
  `:new`はプロパティ名の後にコロンか等号をを使うことを強制します。
  例えば、`color: #0f3`や`width: $main_width`などです。
  `:old`はプロパティ名の前にコロンを使うことを強制します。
  デフォルトでは両方の構文が有効です。
  これはSCSSの文書では効果がありません。

{#cache-option} `:cache`
: 高速化のために、パースされたSassファイルをキャッシュするかどうか。
  デフォルトはtrueです。

{#read_cache-option} `:read_cache`
: このオプションが設定されていて`:cache`が設定されていない場合、Sassキャッシュの読み込みはそれが存在する場合にのみ行われます。キャッシュが存在していなくても、書き込みは行われません。

{#cache_store-option} `:cache_store`
: このオプションに{Sass::CacheStores::Base}のサブクラスのインスタンスが設定されると、キャッシュされたコンパイル結果の保存や検索にそのキャッシュストアが使われます。
  デフォルトは{Sass::CacheStores::Filesystem}で、[`:cache_location` option](#cache_location-option)を使って初期化されます。

{#never_update-option} `:never_update`
: テンプレートファイルを変更してもCSSファイルが更新されないかどうか。
  trueに設定するとパフォーマンスが少し向上する可能性があります。
  デフォルトは常にfalseです。
  Rack、Ruby on Rails、Merbの内部でのみ意味があります。

{#always_update-option} `:always_update`
: テンプレートが変更された場合だけでなく、コントローラーにアクセスされるたびにCSSファイルを更新するかどうか。
  デフォルトはfalseです。
  Rack、Ruby on Rails、Merbの内部でのみ意味があります。

{#always_check-option} `:always_check`
: サーバーの起動時だけでなく、コントローラーにアクセスされるたびにSassテンプレートがチェックされるかどうか。
  Sassテンプレートが更新されていたら、再コンパイルして対応するCSSファイルを上書きします。
  デフォルトはproductionモードではfalseで、そうでなければtrueです。
  Rack、Ruby on Rails、Merbの内部でのみ意味があります。

{#poll-option} `:poll`
: trueの場合、{Sass::Plugin::Compiler#watch}でネイティブのファイルシステムのバックエンドでなくポーリングのバックエンドを常に利用します。

{#full_exception-option} `:full_exception`
: Sassのエラーの詳細な説明を生成されたCSSファイル内に提供するかどうか。
  trueが設定されると、エラーは行番号とソースの断片とともに、CSSファイル中のコメントとページの一番上（サポートされているブラウザの場合）の両方に表示されます（訳注: before擬似要素とcontentプロパティがサポートされているブラウザではエラーがブラウザ上に表示される）。
  デフォルトはproductionモードではfalseで、そうでなければtrueです。
  Rack、Ruby on Rails、Merbの内部でのみ意味があります。

: アプリケーションにおけるsassテンプレートのルートディレクトリのパス。
  ハッシュならば、`:css_location`は無視され、このオプションは入力と出力ディレクトリのマッピングを指定します。
  ハッシュの代わりに2つの要素のリストを与えることもできます。
  デフォルトは`css_location + "/sass"`です。
  複数のテンプレートロケーションが指定された場合、それらの全てがインポートパスに配置され、それらの間でインポートが許可されことに注意してください。
  **多くのフォーマットを取ることができるので、このオプションは直接指定するだけで、アクセスや変更されるべきでないことに注意してください。代わりに{Sass::Plugin::Configuration#template_location_array Sass::Plugin#template_location_array}、{Sass::Plugin::Configuration#add_template_location Sass::Plugin#add_template_location}、{Sass::Plugin::Configuration#remove_template_location Sass::Plugin#remove_template_location}を利用してください。**

{#css_location-option} `:css_location`
: CSSの出力が書き込まれるパス。
  このオプションは`:template_location`がHashの場合無視されます。
  デフォルトは`"./public/stylesheets"`です。
  Rack、Ruby on Rails、Merbの内部でのみ意味があります。

{#cache_location-option} `:cache_location`
: キャッシュされた`sassc`ファイルが書き込まれるパス。
  RailsやMerbでのデフォルトは`"./tmp/sass-cache"`で、それ以外では`"./.sass-cache"`です。
  [`:cache_store` option](#cache_location-option)が設定されていれば、この設定は無視されます。

{#unix_newlines-option} `:unix_newlines`
: trueの場合、ファイル書き込み時にUnix-styleの改行を使います。
  このオプションはWindows環境で、かつSassがファイルに書き込んでいる場合（Rack、Rails、Merbで{Sass::Plugin}を直接使う場合、もしくはコマンドラインから実行する場合）にのみ有効です。

{#filename-option} `:filename`
: レンダリングされるファイルのファイル名。
  エラーレポートだけで使われ、RackやRails、Merbでは自動的に設定されます。

{#line-option} `:line`
: Sassテンプレートの一行目の数。
  エラーの行数表示に使われる。
  RubyファイルにSassテンプレートが埋め込まれている場合に設定すると便利です。

{#load_paths-option} `:load_paths`
: [`@import`](#import)ディレクティブでインポートされるSassテンプレートの検索パスをファイルシステムのパスかインポーターの配列で指定します。
  要素には文字列か`Pathname`のオブジェクト、{Sass::Importers::Base}のサブクラスを指定することができます。
  デフォルトはワーキングディレクトリで、RackやRails、Merbでは`:template_location`です。
  ロードパスは`SASS_PATH`環境変数と{Sass.load_paths}からも設定することができます。

{#filesystem_importer-option} `:filesystem_importer`
: {Sass::Importers::Base}のサブクラスは文字列のロードパスを処理するために利用されます。
  これはファイルシステムからファイルをインポートします。
  コンストラクタで一つの文字列（ロードパス）を引数に取る{Sass::Importers::Base}を継承したクラスのオブジェクトを指定します。
  デフォルトは{Sass::Importers::Filesystem}です。

{#line_numbers-option} `:line_numbers`
: trueに設定すると、セレクタが定義された行番号とファイル名を、コンパイルされたCSSのコメントに出力します。
  デバッグの際、特にインポートとミックスインを使う場合に便利です。
  このオプションは`:line_comments`とも呼ばれます。
  `:compressed`出力スタイルの場合や`:debug_info`/`:trace_selectors`オプションを指定したときは自動的に無効になります。

{#trace_selectors-option} `:trace_selectors`
: trueに設定すると、各セレクタの前にインポートとミックスインの全てのトレースを出力します。
  これはスタイルシートのインポートやミックスインのインクルードをブラウザでデバッグするときに役立ちます。
  このオプションは`:line_comments`オプションより優先され、`:debug_info`オプションはこのオプションより優先されます。
  `:compressed`出力形式の場合は自動的に無効になります。

{#debug_info-option} `:debug_info`
: trueに設定すると、セレクタが定義された行番号とファイル名が、コンパイルされたCSSの中にブラウザが理解できる形式で出力されます。
  [Firebug拡張のFireSass](https://addons.mozilla.org/en-US/firefox/addon/103988)と一緒に使うと、Sassのファイル名と行番号が表示されて便利です。
  `:compressed`出力形式の場合は自動的に無効になります。

{#custom-option} `:custom`
: このオプションは、個々のアプリケーションが{Sass::Script::Functions Sassのカスタム関数で}利用可能なデータを設定するために設けられています。

{#quiet-option} `:quiet`
: trueに設定すると、警告が無効になります。

### Syntax Selection

### 構文の選択

The Sass command-line tool will use the file extension to determine which
syntax you are using, but there's not always a filename. The `sass`
command-line program defaults to the indented syntax but you can pass the
`--scss` option to it if the input should be interpreted as SCSS syntax.
Alternatively, you can use the `scss` command-line program which is exactly
like the `sass` program but it defaults to assuming the syntax is SCSS.

Sassのコマンドラインツールは使用する構文を決定するためにファイルの拡張子を使用しますが、常にファイル名によって決定するわけではありません。
`sass`コマンドラインプログラムのデフォルトはインデントされた構文ですが、`--scss`オプションを指定することでSCSS構文として解釈されます。
あるいは、`sass`プログラムとそっくりだがデフォルトの構文がSCSSである`scss`コマンドラインプログラムを使います。

### Encodings

### エンコーディング

When running on Ruby 1.9 and later, Sass is aware of the character encoding of documents
and will handle them the same way that CSS would.
By default, Sass assumes that all stylesheets are encoded
using whatever coding system your operating system defaults to.
For many users this will be `UTF-8`, the de facto standard for the web.
For some users, though, it may be a more local encoding.

Ruby 1.9かそれ以降で実行する場合、Sassは文書の文字コードを知っており、CSSと同じように処理します。
デフォルトではSassは、全てのスタイルシートはオペレーティングシステムのデフォルトのコーディングシステムを使ってエンコードされることを前提とします。
多くのユーザーはWebの事実上の標準である`UTF-8`になるでしょう。
しかしながら、一部のユーザーはよりローカルなエンコーディングになるかもしれません。

If you want to use a different encoding for your stylesheet
than your operating system default,
you can use the `@charset` declaration just like in CSS.
Add `@charset "encoding-name";` at the beginning of the stylesheet
(before any whitespace or comments)
and Sass will interpret it as the given encoding.
Note that whatever encoding you use, it must be convertible to Unicode.

スタイルシートでオペレーティングシステムのデフォルトと異なるエンコーディングを利用したい場合は、CSSと同じ`@charset`宣言を使います。
`@charset "encoding-name";`をスタイルシートの先頭（空白やコメントより前）に追加することでSassは与えられたエンコーディングを解釈します。
どのようなエンコーディングを使ってもユニコードに変換可能でなければならないことに注意してください。

Sass will also respect any Unicode BOMs and non-ASCII-compatible Unicode encodings
[as specified by the CSS spec](http://www.w3.org/TR/CSS2/syndata.html#charset),
although this is *not* the recommended way
to specify the character set for a document.
Note that Sass does not support the obscure `UTF-32-2143`,
`UTF-32-3412`, `EBCDIC`, `IBM1026`, and `GSM 03.38` encodings,
since Ruby does not have support for them
and they're highly unlikely to ever be used in practice.

また、SassはCSSの仕様で指定された非ASCII互換のユニコードエンコーディングやユニコードのBOMを尊重しますが、文書の文字コード設定を指定する方法としては推奨*しません*。
Sassは`UTF-32-2143`、`UTF-32-3412`、`EBCDIC`、`IBM1026`、`GSM 03.38`などの無名なエンコーディングをサポートしていないことに注意してください。これらはRubyでもサポートされておらず、実際に利用される可能性も非常に低いからです。

#### Output Encoding

#### 出力のエンコーディング

In general, Sass will try to encode the output stylesheet
using the same encoding as the input stylesheet.
In order for it to do this, though, the input stylesheet must have a `@charset` declaration;
otherwise, Sass will default to encoding the output stylesheet as UTF-8.
In addition, it will add a `@charset` declaration to the output
if it's not plain ASCII.

通常、Sassは入力のスタイルシートと同じエンコーディングを使って出力のスタイルシートをエンコードすることを試みます。
しかしながら、それを適切に行うには入力のスタイルシートに`@charset`宣言がある必要があります。そうでなければ、Sassは出力のスタイルーシートのエンコーディングの初期値ををUTF-8に設定します。
さらに、プレーンなASCIIでなければ、`@charset`宣言を出力に付け加えます。

When other stylesheets with `@charset` declarations are `@import`ed,
Sass will convert them to the same encoding as the main stylesheet.

その他の`@charset`宣言があるスタイルシートが`@import`された場合、Sassはメインのスタイルシートのエンコーディングと同じものに変換します。

Note that Ruby 1.8 does not have good support for character encodings,
and so Sass behaves somewhat differently when running under it than under Ruby 1.9 and later.
In Ruby 1.8, Sass simply uses the first `@charset` declaration in the stylesheet
or any of the other stylesheets it `@import`s.

Ruby 1.8は文字コードをサポートしていないので、Ruby 1.9以降で実行したときと比べるとSassは少々異なる振る舞いをすることに注意してください。
Ruby 1.8では、Sassは単純にスタイルシートかそこから`@import`したスタイルシートの中の最初の`@charset`宣言を使います。
