+  元文書: [sass/doc-src/SASS_REFERENCE.md at f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2 · nex3/sass · GitHub](https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#using-sass "sass/doc-src/SASS_REFERENCE.md at f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2 · nex3/sass · GitHub")

## Using Sass

## Sassを使う

Sass can be used in three ways:
as a command-line tool,
as a standalone Ruby module,
and as a plugin for any Rack-enabled framework,
including Ruby on Rails and Merb.
The first step for all of these is to install the Sass gem:

Sassは3つの方法で使用することができます: コマンドラインツールとして、スタンドアロンなRubyのモジュールとして、そしてRuby on RailsやMerbや含む、Rackが有効な全てのフレームワークのプラグインとして。それらの全てにおける最初のステップはSass gemをインストールすることです。

    gem install sass

If you're using Windows,
you may need to [install Ruby](http://rubyinstaller.org/download.html) first.

もしWindowsを使用している場合は、最初に[Rubyのインストール](http://rubyinstaller.org/download.html)が必要な場合があります。

To run Sass from the command line, just use

Sassをコマンドラインで実行するには、このように使います。

    sass input.scss output.css

You can also tell Sass to watch the file and update the CSS
every time the Sass file changes:

また、ファイルを監視することをSassに伝えると、CSSを更新するたびにSassファイルを変更することができます:

    sass --watch input.scss:output.css

If you have a directory with many Sass files,
you can also tell Sass to watch the entire directory:

もし、多くのSassファイルがあるディレクトリがある場合、ディレクトリ全体を監視することをSassに伝えることができます。

    sass --watch app/sass:public/stylesheets

Use `sass --help` for full documentation.

全てのドキュメントを見るには`sass --help`を使います。

Using Sass in Ruby code is very simple.
After installing the Sass gem,
you can use it by running `require "sass"`
and using {Sass::Engine} like so:

Rubyのコード内でSassを使うことはとてもシンプルです。Sass gemをインストールした後、`require "sass"`を実行することで使うことができ、{Sass::Engine}をこのように使います。

    engine = Sass::Engine.new("#main {background-color: #0000ff}", :syntax => :scss)
    engine.render #=> "#main { background-color: #0000ff; }\n"

### Rack/Rails/Merb Plugin

### Rack/Rails/Merg プラグイン

To enable Sass in Rails versions before Rails 3,
add the following line to `environment.rb`:

SassをバージョンがRails 3より前のRailsで有効にするためには、`environment.rb`に次の行を追加します。

    config.gem "sass"

For Rails 3, instead add the following line to the Gemfile:

Rails 3では、代わりに次の行をGemfileに追加します。

    gem "sass"

To enable Sass in Merb,
add the following line to `config/dependencies.rb`:

MerbでSassを有効にするには`config/dependencies.rb`に次の行を追加します。

    dependency "merb-haml"

To enable Sass in a Rack application,
add

RackアプリケーションでSassを有効にするには、追加します

    require 'sass/plugin/rack'
    use Sass::Plugin::Rack

to `config.ru`.

`config.ru`に。

Sass stylesheets don't work the same as views.
They don't contain dynamic content,
so the CSS only needs to be generated when the Sass file has been updated.
By default, `.sass` and `.scss` files are placed in public/stylesheets/sass
(this can be customized with the [`:template_location`](#template_location-option) option).
Then, whenever necessary, they're compiled into corresponding CSS files in public/stylesheets.
For instance, public/stylesheets/sass/main.scss would be compiled to public/stylesheets/main.css.

Sassのスタイルシートはviewsと同じようには動作しません。それらには、動的なコンテンツが含まれていないので、CSSはSassファイルが更新された場合に生成される必要があるだけです。デフォルトでは、`.sass`と`.scss`ファイルはpublic/stylesheets/sassに置かれます(これは[`:template_location`](#template_location-option)でカスタマイズ可能です)。そうすると、必要に応じて、public/stylesheets内に対応するCSSファイルがコンパイルされます。例えば、public/stylesheets/sass/main.scssはpublic/stylesheets/main.cssにコンパイルされます。

### Caching

### キャッシュする

By default, Sass caches compiled templates and [partials](#partials).
This dramatically speeds up re-compilation of large collections of Sass files,
and works best if the Sass templates are split up into separate files
that are all [`@import`](#import)ed into one large file.

デフォルトでは、Sassはコンパイルされたテンプレートと[パーシャル](#partials)をキャッシュします。これは大きいSassファイルの集合の再コンパイルを劇的に高速化し、Sassのテンプレートが一つの大きいファイルの中で全て[`@import`](#import)される、別々に分割されるファイルの場合に最も効果的に動作します。

Without a framework, Sass puts the cached templates in the `.sass-cache` directory.
In Rails and Merb, they go in `tmp/sass-cache`.
The directory can be customized with the [`:cache_location`](#cache_location-option) option.
If you don't want Sass to use caching at all,
set the [`:cache`](#cache-option) option to `false`.

フレームワークがなければ、Sassは`.sass-cache`ディレクトリ内にキャッシュされたテンプレートを置きます。RailsやMerbでは、`tmp/sass-cache`になります。
そのディレクトリは[`:cache_location`](#cache_location-option)でカスタマイズすることができます。もしSassの全てのキャッシュを必要としない場合は[`:cache`](#cache-option)オプションを`false`に設定します。
