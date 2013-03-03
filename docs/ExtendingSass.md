+  元文書: [sass/doc-src/SASS_REFERENCE.md at f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2 · nex3/sass · GitHub](https://github.com/nex3/sass/blob/f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2/doc-src/SASS_REFERENCE.md#extending-sass "sass/doc-src/SASS_REFERENCE.md at f2ff5d2d60a461f7b1ecfdb036c558ad6fa34fa2 · nex3/sass · GitHub")

## Sassを拡張する　[原文](http://sass-lang.com/docs/yardoc/file.SASS_REFERENCE.html#extending_sass)

Sassは独自の要望を持ったユーザーのためにいくつかの高度なカスタマイズを提供しています。
この機能を利用するためには、Rubyの深い理解が必要です。

### カスタムSass関数を定義する

ユーザはRuby APIを使って、独自のSass関数を定義できます。
詳しくは[ソースコードのドキュメント](Sass/Script/Functions.html#adding_custom_functions)
をご覧ください。

### キャッシュの保存場所

Sassは変更が無い限り再パースせずに再利用できるように、パース済ドキュメントをキャッシュします。
デフォルトで、Sassは[`:cache_location`](#cache_location-option)で指定されたファイルシステムの場所にキャッシュファイルを書き込みます。
キャッシュファイルをファイルシステム上に書き出せなかったり、
Ruby のプロセス間や異なるマシン間で共有したい場合は、
[`:cache_store`オプション](#cache_store-option)で設定した場所に、
キャッシュするように変更できます。
独自にキャッシュの保存場所を設定するための詳細については、
{Sass::CacheStores::Base ソースコードのドキュメント}をご覧ください。

### カスタムインポータ

Sassのインボータは`@import`で渡されたパスを管理し、これらのパスに対して適切なSassコードを探します。
デフォルトでは、このコードは{Sass::Importers::Filesystem ファイルシステム}から読み込まれますが、
データベースから読み込んだり、HTTP接続を利用したり、
Sassの想定と異なるファイル名スキームでコードを取得するようなインポータも追加できます。

各インポータはシングルロードパス(または、バックエンドの類似概念のどんなものでも)を管理します。
インポータは{file:SASS_REFERENCE.md#load_paths-option `:load_paths`配列}に通常のファイルシステムパスと一緒に配置されます。

`@import`を解決するときは、Sassはロードパスを調べて、パスの読み込みに成功したインポータを探します。
1度1つが見つかると、インポートされたファイルが使われます。

ユーザの自作インポータは、{Sass::Importers::Base}を継承していなければなりません。