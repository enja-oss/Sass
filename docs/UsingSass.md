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

### Options

### オプション

Options can be set by setting the {Sass::Plugin::Configuration#options Sass::Plugin#options} hash
in `environment.rb` in Rails or `config.ru` in Rack...

オプションはRailsの`environment.rb`やRackの`config.ru`内で{Sass::Plugin::Configuration#options Sass::Plugin#options}ハッシュによって設定できます。

    Sass::Plugin.options[:style] = :compact

...or by setting the `Merb::Plugin.config[:sass]` hash in `init.rb` in Merb...

もしくは、Merbの`init.rb`で`Merb::Plugin.config[:sass]`を設定します。

    Merb::Plugin.config[:sass][:style] = :compact

...or by passing an options hash to {Sass::Engine#initialize}.
All relevant options are also available via flags
to the `sass` and `scss` command-line executables.
Available options are:

もしくは、{Sass::Engine#initialize}にオプションのハッシュを渡します。関連する全てのオプションは、`sass`か`scss`のコマンドラインの実行時にフラグを通して利用可能です。利用可能なオプションは次の通りです。

{#style-option} `:style`
: Sets the style of the CSS output.
  See [Output Style](#output_style).

{#style-option} `:style`
: CSSの出力スタイルを設定します。
  [Output Style](#output_style)を見てください。

{#syntax-option} `:syntax`
: The syntax of the input file, `:sass` for the indented syntax
  and `:scss` for the CSS-extension syntax.
  This is only useful when you're constructing {Sass::Engine} instances yourself;
  it's automatically set properly when using {Sass::Plugin}.
  Defaults to `:sass`.

{#syntax-option} `:syntax`
: 入力ファイルのシンタックスで、`:sass`はインデントのシンタックス、`:scss`はCSS拡張のシンタックスです。
  これは{Sass::Engine}を初期化する場合にのみ利用可能です。
  {Sass::Plugin}を使う場合はプロパティは自動的に設定されます。
  デフォルトは`:sass`です。

{#property_syntax-option} `:property_syntax`
: Forces indented-syntax documents to use one syntax for properties.
  If the correct syntax isn't used, an error is thrown.
  `:new` forces the use of a colon or equals sign
  after the property name.
  For example: `color: #0f3`
  or `width: $main_width`.
  `:old` forces the use of a colon
  before the property name.
  For example: `:color #0f3`
  or `:width $main_width`.
  By default, either syntax is valid.
  This has no effect on SCSS documents.

{#property_syntax-option} `:property_syntax`
: インデント形式の文書のプロパティの構文を強制します。
  正しい構文が使われていない場合にエラーを投げます。
  `:new`はプロパティ名の後にコロンか等号をを使うことを強制します。
  例えば、`color: #0f3`や`width: $main_width`などです。
  `:old`はプロパティ名の前にコロンを使うことを強制します。
  デフォルトでは両方の構文が有効です。
  これはSCSSの文書では効果がありません。

{#cache-option} `:cache`
: Whether parsed Sass files should be cached,
  allowing greater speed. Defaults to true.

{#cache-option} `:cache`
: 高速化のために、パースされたSassファイルをキャッシュするかどうか。
  デフォルトはtrueです。

{#read_cache-option} `:read_cache`
: If this is set and `:cache` is not,
  only read the Sass cache if it exists,
  don't write to it if it doesn't.

{#read_cache-option} `:read_cache`
: このオプションが設定されていて`:cache`が設定されていない場合、Sassのキャッシュが存在していた場合に読み込みだけおこない、キャッシュが存在していなくても書き込みはおこないません。

{#cache_store-option} `:cache_store`
: If this is set to an instance of a subclass of {Sass::CacheStores::Base},
  that cache store will be used to store and retrieve
  cached compilation results.
  Defaults to a {Sass::CacheStores::Filesystem} that is
  initialized using the [`:cache_location` option](#cache_location-option).

{#cache_store-option} `:cache_store`
: このオプションに{Sass::CacheStores::Base}のサブクラスのインスタンスが設定されると、キャッシュされたコンパイル結果の保存や検索にそのキャッシュストアが使われます。
  デフォルトは{Sass::CacheStores::Filesystem}で、[`:cache_location` option](#cache_location-option)を使って初期化されます。

{#never_update-option} `:never_update`
: Whether the CSS files should never be updated,
  even if the template file changes.
  Setting this to true may give small performance gains.
  It always defaults to false.
  Only has meaning within Rack, Ruby on Rails, or Merb.

{#never_update-option} `:never_update`
: テンプレートファイルを変更してもCSSファイルが更新されないかどうか。
  trueに設定するとパフォーマンスが少し向上する可能性があります。
  デフォルトは常にfalseです。
  Rack、Ruby on Rails、Merbの内部でのみ意味があります。

{#always_update-option} `:always_update`
: Whether the CSS files should be updated every
  time a controller is accessed,
  as opposed to only when the template has been modified.
  Defaults to false.
  Only has meaning within Rack, Ruby on Rails, or Merb.

{#always_update-option} `:always_update`
: テンプレートが変更された場合だけでなく、コントローラーにアクセスされるたびにCSSファイルを更新するかどうか。
  デフォルトはfalseです。
  Rack、Ruby on Rails、Merbの内部でのみ意味があります。

{#always_check-option} `:always_check`
: Whether a Sass template should be checked for updates every
  time a controller is accessed,
  as opposed to only when the server starts.
  If a Sass template has been updated,
  it will be recompiled and will overwrite the corresponding CSS file.
  Defaults to false in production mode, true otherwise.
  Only has meaning within Rack, Ruby on Rails, or Merb.

{#always_check-option} `:always_check`
: サーバーの起動時だけでなく、コントローラーにアクセスされるたびにSassテンプレートがチェックされるかどうか。
  Sassテンプレートが更新されていたら、再コンパイルして対応するCSSファイルに上書きします。
  デフォルトはproductionモードではfalseで、そうでなければtrueです。
  Rack、Ruby on Rails、Merbの内部でのみ意味があります。

{#poll-option} `:poll`
: When true, always use the polling backend for {Sass::Plugin::Compiler#watch}
  rather than the native filesystem backend.

{#poll-option} `:poll`
: trueの場合、{Sass::Plugin::Compiler#watch}でネイティブのファイルシステムのバックエンドでなくポーリングのバックエンドを常に利用します。

{#full_exception-option} `:full_exception`
: Whether an error in the Sass code
  should cause Sass to provide a detailed description
  within the generated CSS file.
  If set to true, the error will be displayed
  along with a line number and source snippet
  both as a comment in the CSS file
  and at the top of the page (in supported browsers).
  Otherwise, an exception will be raised in the Ruby code.
  Defaults to false in production mode, true otherwise.
  Only has meaning within Rack, Ruby on Rails, or Merb.

{#full_exception-option} `:full_exception`
: Sassのエラーの詳細な説明を生成されたCSSファイル内に提供するかどうか。
  trueが設定されると、エラーは行番号とソースの断片とともに、CSSファイル中のコメントとページの一番上（サポートされているブラウザの場合）の両方に表示されます（訳注: before擬似要素がサポートされているブラウザではエラーがブラウザ上に表示される）。
  デフォルトはproductionモードではfalseで、そうでなければtrueです。
  Rack、Ruby on Rails、Merbの内部でのみ意味があります。

{#template_location-option} `:template_location`
: A path to the root sass template directory for your application.
  If a hash, `:css_location` is ignored and this option designates
  a mapping between input and output directories.
  May also be given a list of 2-element lists, instead of a hash.
  Defaults to `css_location + "/sass"`.
  Only has meaning within Rack, Ruby on Rails, or Merb.
  Note that if multiple template locations are specified, all
  of them are placed in the import path, allowing you to import
  between them.
  **Note that due to the many possible formats it can take,
  this option should only be set directly, not accessed or modified.
  Use the {Sass::Plugin::Configuration#template_location_array Sass::Plugin#template_location_array},
  {Sass::Plugin::Configuration#add_template_location Sass::Plugin#add_template_location},
  and {Sass::Plugin::Configuration#remove_template_location Sass::Plugin#remove_template_location} methods instead**.

: アプリケーションにおけるsassテンプレートのルートディレクトリのパス。
  ハッシュならば、`:css_location`は無視され、このオプションは入力と出力ディレクトリのマッピングを指定します。
  ハッシュの代わりに2つの要素のリストを与えることもできます。
  デフォルトは`css_location + "/sass"`です。
  複数のテンプレートロケーションが指定された場合、それらの全てがインポートパスに配置され、それらの間でインポートが許可されことに注意してください。
  多くのフォーマットを取ることができるので、このオプションは直接指定するだけで、アクセスや変更されるべきでないことに注意してください。代わりに{Sass::Plugin::Configuration#template_location_array Sass::Plugin#template_location_array}、{Sass::Plugin::Configuration#add_template_location Sass::Plugin#add_template_location}、{Sass::Plugin::Configuration#remove_template_location Sass::Plugin#remove_template_location}を利用してください。**

{#css_location-option} `:css_location`
: The path where CSS output should be written to.
  This option is ignored when `:template_location` is a Hash.
  Defaults to `"./public/stylesheets"`.
  Only has meaning within Rack, Ruby on Rails, or Merb.

{#css_location-option} `:css_location`
: CSSの出力が書き込まれるパス。
  このオプションは`:template_location`がHashの場合無視されます。
  デフォルトは`"./public/stylesheets"`です。
  Rack、Ruby on Rails、Merbの内部でのみ意味があります。

{#cache_location-option} `:cache_location`
: The path where the cached `sassc` files should be written to.
  Defaults to `"./tmp/sass-cache"` in Rails and Merb,
  or `"./.sass-cache"` otherwise.
  If the [`:cache_store` option](#cache_location-option) is set,
  this is ignored.

{#cache_location-option} `:cache_location`
: キャッシュされた`sassc`ファイルが書き込まれるパス。
  RailsやMerbでのデフォルトは`"./tmp/sass-cache"`で、それ以外では`"./.sass-cache"`です。
  [`:cache_store` option](#cache_location-option)が設定されていれば、この設定は無視されます。

{#unix_newlines-option} `:unix_newlines`
: If true, use Unix-style newlines when writing files.
  Only has meaning on Windows, and only when Sass is writing the files
  (in Rack, Rails, or Merb, when using {Sass::Plugin} directly,
  or when using the command-line executable).

{#unix_newlines-option} `:unix_newlines`
: trueの場合、ファイルに書き込みしたときUnix-styleの改行を使います。
  Windowsで、Sassがファイルに書き込んでいる場合（Rack、Rails、Merbで{Sass::Plugin}を直接使うか、コマンドラインから実行する場合）のみ意味があります。

{#filename-option} `:filename`
: The filename of the file being rendered.
  This is used solely for reporting errors,
  and is automatically set when using Rack, Rails, or Merb.

{#filename-option} `:filename`
: レンダリングされるファイルのファイル名。
  エラーレポートだけで使われ、RackやRails、Merbでは自動的に設定されます。

{#line-option} `:line`
: The number of the first line of the Sass template.
  Used for reporting line numbers for errors.
  This is useful to set if the Sass template is embedded in a Ruby file.

{#line-option} `:line`
: Sassテンプレートの一行目の数。
  エラーの行数表示に使われる。
  RubyファイルにSassテンプレートが埋め込まれている場合に設定すると便利です。

{#load_paths-option} `:load_paths`
: An array of filesystem paths or importers which should be searched
  for Sass templates imported with the [`@import`](#import) directive.
  These may be strings, `Pathname` objects, or subclasses of {Sass::Importers::Base}.
  This defaults to the working directory and, in Rack, Rails, or Merb,
  whatever `:template_location` is.
  The load path is also informed by {Sass.load_paths}
  and the `SASS_PATH` environment variable.

{#load_paths-option} `:load_paths`
: [`@import`](#import)ディレクティブでインポートされるSassテンプレートの検索パスをファイルシステムのパスかインポーターの配列で指定します。
  要素には文字列か`Pathname`のオブジェクト、{Sass::Importers::Base}のサブクラスを指定することができます。
  デフォルトはワーキングディレクトリで、RackやRails、Merbでは`:template_location`です。
  ロードパスは`SASS_PATH`環境変数と{Sass.load_paths}からも設定することができます。

{#filesystem_importer-option} `:filesystem_importer`
: A {Sass::Importers::Base} subclass used to handle plain string load paths.
  This should import files from the filesystem.
  It should be a Class object inheriting from {Sass::Importers::Base}
  with a constructor that takes a single string argument (the load path).
  Defaults to {Sass::Importers::Filesystem}.

{#filesystem_importer-option} `:filesystem_importer`
: {Sass::Importers::Base}のサブクラスは文字列のロードパスを処理するために利用されます。
  これはファイルシステムからファイルをインポートします。
  コンストラクタで一つの文字列（ロードパス）を引数に取る{Sass::Importers::Base}を継承したクラスのオブジェクトを指定します。
  デフォルトは{Sass::Importers::Filesystem}です。

{#line_numbers-option} `:line_numbers`
: When set to true, causes the line number and file
  where a selector is defined to be emitted into the compiled CSS
  as a comment. Useful for debugging, especially when using imports
  and mixins.
  This option may also be called `:line_comments`.
  Automatically disabled when using the `:compressed` output style
  or the `:debug_info`/`:trace_selectors` options.

{#line_numbers-option} `:line_numbers`
: trueに設定すると、セレクターが定義された行番号とファイルをコンパイルされたCSSの中のコメントに出力します。
  デバッグの際、特にインポートとミックスインを使う場合に便利です。
  このオプションは`:line_comments`とも呼ばれます。
  `:compressed`出力スタイルの場合や`:debug_info`/`:trace_selectors`オプションを指定したときは自動的に無効になります。

{#trace_selectors-option} `:trace_selectors`
: When set to true, emit a full trace of imports and mixins before
  each selector. This can be helpful for in-browser debugging of
  stylesheet imports and mixin includes. This option supersedes
  the `:line_comments` option and is superseded by the
  `:debug_info` option. Automatically disabled when using the
  `:compressed` output style.

{#trace_selectors-option} `:trace_selectors`
: trueに設定すると、各セレクタの前にインポートとミックスインの全てのトレースを出力します。
  これはスタイルシートのインポートやミックスインのインクルードをブラウザでデバッグするときに役立ちます。
  このオプションは`:line_comments`オプションより優先され、`:debug_info`オプションはこのオプションより優先されます。
  `:compressed`出力形式の場合は自動的に無効になります。

{#debug_info-option} `:debug_info`
: When set to true, causes the line number and file
  where a selector is defined to be emitted into the compiled CSS
  in a format that can be understood by the browser.
  Useful in conjunction with [the FireSass Firebug extension](https://addons.mozilla.org/en-US/firefox/addon/103988)
  for displaying the Sass filename and line number.
  Automatically disabled when using the `:compressed` output style.

{#debug_info-option} `:debug_info`
: trueに設定すると、コンパイルされたCSSにセレクタが定義された行番号とファイルがブラウザが理解できる形式で出力されます。
  Sassのファイル名と行番号を表示するのに[Firebug拡張のFireSass](https://addons.mozilla.org/en-US/firefox/addon/103988)と併用すると便利です。
  `:compressed`出力形式の場合は自動的に無効になります。

{#custom-option} `:custom`
: An option that's available for individual applications to set
  to make data available to {Sass::Script::Functions custom Sass functions}.

{#custom-option} `:custom`
: 個々のアプリケーションが{Sass::Script::Functions Sassのカスタム関数で}利用するためのデータを設定するために利用可能なオプションです。

{#quiet-option} `:quiet`
: When set to true, causes warnings to be disabled.

{#quiet-option} `:quiet`
: trueに設定すると、警告が無効になります。
