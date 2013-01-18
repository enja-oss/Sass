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
