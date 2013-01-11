+  元文書: [sass/doc-src/SASS_REFERENCE.md at f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2 · nex3/sass · GitHub](https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#extending-sass "sass/doc-src/SASS_REFERENCE.md at f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2 · nex3/sass · GitHub")

## Extending Sass

## Sass を拡張する　[原文](http://sass-lang.com/docs/yardoc/file.SASS_REFERENCE.html#extending_sass)

Sass provides a number of advanced customizations for users with unique requirements.
Using these features requires a strong understanding of Ruby.

Sass では、ユーザ独自の要望に合わせた高度なカスタマイズができます。
この機能を利用するためには、Ruby の深い理解が必要です。

### Defining Custom Sass Functions

### カスタムSass関数を定義する

Users can define their own Sass functions using the Ruby API.
For more information, see the [source documentation](Sass/Script/Functions.html#adding_custom_functions).

ユーザは Ruby API を使って、独自の Sass関数を定義できます。
詳しくは[ソースコードのドキュメント](Sass/Script/Functions.html#adding_custom_functions)
をご覧ください。

### Cache Stores

### キャッシュの保存場所

Sass caches parsed documents so that they can be reused without parsing them again
unless they have changed. By default, Sass will write these cache files to a location
on the filesystem indicated by [`:cache_location`](#cache_location-option). If you
cannot write to the filesystem or need to share cache across ruby processes or machines,
then you can define your own cache store and set the[`:cache_store`
option](#cache_store-option). For details on creating your own cache store, please
see the {Sass::CacheStores::Base source documentation}.

Sass は、変換済みドキュメントのキャッシュを行います。
変更がない限り、再利用時には再び変換せずに済むことになります。
デフォルトでは、Sass は、これらのキャッシュファイルを
[`:cache_location`](#cache_location-option) によって指示された
ファイルシステム上に書き出します。
キャッシュファイルをファイルシステム上に書き出せなかったり、
Ruby のプロセス間や異なるマシン間で共有したい場合は、
[`:cache_store` オプション](#cache_store-option)で設定した場所に、
キャッシュするように変更できます。
独自にキャッシュの保存場所を設定するための詳細については、
{Sass::CacheStores::Base ソースコードのドキュメント} をご覧ください。

### Custom Importers

### カスタムインポータ

Sass importers are in charge of taking paths passed to `@import` and finding the
appropriate Sass code for those paths. By default, this code is loaded from
the {Sass::Importers::Filesystem filesystem}, but importers could be added to load
from a database, over HTTP, or use a different file naming scheme than what Sass expects.

Sass のインポータは `@import` で渡されたパスを受け取り、これらのパスに対して適切な Sassコードを探します。
デフォルトでは、このコードは {Sass::Importers::Filesystem ファイルシステム}
から読み込まれますが、データベースから読み込んだり、HTTP接続を利用したり、
Sass の想定と異なるファイル名スキームでコードを取得するようなインポータも追加できます。

Each importer is in charge of a single load path (or whatever the corresponding notion
is for the backend). Importers can be placed in the {file:SASS_REFERENCE.md#load_paths-option
`:load_paths` array} alongside normal filesystem paths.

どのインポータも、シングル・ロード・パス（あるいはバックエンドを支持する類似概念のようなもの）の役割を持ちます。
インポータは {file:SASS_REFERENCE.md#load_paths-option
`:load_paths` 配列} に通常のファイルシステム用のパスと一緒に配置できます。

When resolving an `@import`, Sass will go through the load paths looking for an importer
that successfully imports the path. Once one is found, the imported file is used.

`@import` を解決するときは、Sass は読み込むパスを巡回し、
読み込みに成功したインポータを探します。
ひとつ見つかれば、それによってインポートされたファイルが使われます。
 
User-created importers must inherit from {Sass::Importers::Base}.

ユーザの自作インポータは、{Sass::Importers::Base} を継承していなければなりません。