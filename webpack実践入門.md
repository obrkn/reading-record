# webpack実践入門 第２版
## webpackの利点
* 読み込み順
* 依存関係
* その他（名前空間、最適化等）
## 基本
* モジュール・・・まとめるファイル（元ファイル）
* エントリーポイント・・・最初に解析するファイル
* バンドル・・・まとめられたファイル(生成したファイル)
* ビルド・・・バンドルを出力するまでの一連の処理
* ローダー・・・js以外のファイルをバンドルできるように変換
* プラグイン・・・バンドル時の処理を拡張
```sh:zsh
$ npm init -y # 初期化
$ npm install --save-dev webpack@5.30.0 webpack-cli@4.6.0
$ npm install --save jquery@3.6.0
$ npm run webpack # ビルド
```
## package.json
* scripts・・・エイリアス。初見の人のマニュアルになるので設定する。
* devDependencies・・・開発だけ使うライブラリ
* dependencies・・・共通で使うライブラリ

## webpack.config.js
module.exports = {
  mode: developmentはソースマップ生成・再ビルド時短、productionはファイル圧縮・モジュール最適化
  entry: エントリーポイント（複数指定可能）
  output: 出力先
  module: rules: 対象ファイルと利用するローダー（下から順に実行）
}
## ローダー
* babel-loader・・・ES6 → ES5
* scss-loader、css-loader、style-loader・・・CSS関係
* PostCSS・・・CSSのベンダープレフィックス（--web-...）付与、CSSの圧縮

## プラグイン
* webpack-merge・・・webpack.config.jsを共通部分、開発、本番に分けてmergeできる
* mini-css-extract-plugin・・・CSSを個別に出力する。jsと分けてキャッシュを効かせる。
* webpack-bundle-analyzer・・・モジュールのファイルサイズを確認

## その他
* watch: true・・・ watchモードを有効に、変更があったら再ビルド
* DataURL 画像をjsへ変換
* Asset Modules 画像サイズごとjsへ変換するか
* clean: true・・・古いファイルは削除
* devtool: 'eval-cheap-module-source-map'　・・・ソースマップ。コンソール画面からバンドル前のコードを確認できる。

## webpack-dev-server
変更を監視してブラウザをリロード
ビルド結果をメモリ上に保持し、ファイルを出力しない
```sh:zsh
$ npm webpack serve
```
webpack.confg.jsのdevServerで設定

## 最適化
* Tree Shaking・・・lodash-es。デッドコード（importしていないファイル）はバンドルしない → 個別に指定してimportする
* SplitChunksPlugin・・・複数のエントリーポイントが参照する共通のチャンク（モジュール）を共通ファイルにバンドル。キャッシュも効く。

