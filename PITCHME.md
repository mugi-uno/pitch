## babel&webpack入門

2017/08/12 Toyama.rb フロントエンド勉強会

---

Javascript

- ブラウザだけですぐ書けるよ
- 初心者向け
- 型がないから楽だよ

---

「ES6ってので書くのがいいらしい」

---

![Logo](assets/babel.png)

---

なんぞこれ・・・

---

「いろんなライブラリを使いたい」

---

「npmってのがあるのか、ふむふむ...」

---

「あれ、でもこれブラウザでどうやって使うんだ？」

---

![Logo](assets/webpack.png)

---

なんぞこれ・・・

---

- React？え、jQueryじゃだめなの？
- JSX？え、HTMLじゃないの？なにこれ
- Browserify？え、webpackじゃないの？
- Vue？Reactがいいんじゃないの？
- yarn？え、npmは？
- Flow？え、型無いんじゃなかったの？
- えっ？えっ？

---

Javascript

- ブラウザだけですぐ書けるよ
- 初心者向け
- 型がないから楽だよ

---

一つずつ学びましょう

---

今日は混乱しやすい2つについて

- babel
- webpack

---

# babel

---

〜基本〜

- ES6(ES7)で書かれたコードをES5に変換
- ECMAScriptの次世代機能を使えるようにする
- ブラウザは対象とする環境の１つでしかない

--- 

babelを使っているpackage.jsonの例

```
"dependencies": {
  "babel-core": "^6.25.0",
  "babel-loader": "7.x",
  "babel-plugin-syntax-dynamic-import": "^6.18.0",
  "babel-plugin-transform-class-properties": "^6.24.1",
  "babel-polyfill": "^6.23.0",
  "babel-preset-env": "^1.6.0",
  "babel-preset-stage-1": "^6.24.1",
  ...
```

---

babel多くね...?

---

ポイント

- モジュールが細かく分離

---

### babel-core

コア機能。  
後述するpresetやpluginに応じた  
**シンタックスの変換**のみを行う。

```js
(x, y) => x + y;
```
↓
```js
"use strict";

(function (x, y) {
  return x + y;
});
```

---

### babel-polyfill

シンタックスの変換でサポートできないような、  
グローバルライブラリや、新しいネイティブメソッドを使えるようにする。

```js
new Promise();
```

```js
Object.assign(x, y);
```

---

babel-coreを利用するモジュール例

---

### babel-cli

CLIからbabelによる変換を行うときに使う。

```
babel script.js --out-file script-compiled.js
```

---

### babel-register

node.jsの `require` に時にフックして変換を行う。

```
require("babel-register");

# → .es6, .es, .jsx, .js をrequireすると変換される
```

---

### babel-loader

webpackから読み込むときに変換を行う。

```js
module: {
  rules: [
    {
      test: /\.js$/,
      exclude: /(node_modules|bower_components)/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['env']
        }
      }
    }
  ]
}
```

---

## Presets/Pluginsについて

---

ES6(7)→ES5変換で考える必要があること

---

「どこまでの機能をサポートするのか？」

---

ECMAScriptの各仕様にはstageという概念がある

- 0: Strawman
- 1: Proposal
- 2: Draft
- 3: Candidate
- 4: Finished

0はアイデアレベル。4で時期ECMAScriptへの追加。

---

babelでどこまでのものを変換したいのか？

---

### Presets

変換ルールをひとまとめにしたもの。  
Presetsを使いたいstageに応じて選んで使う。

- babel-preset-stage-0
- babel-preset-stage-1
- babel-preset-stage-2
- babel-preset-stage-3
- babel-preset-env
- など

```
babel script.js --presets=stage-2
```

---

特殊な変換用Presetsも存在する。

- babel-preset-react
- babel-preset-flow
- など

---

### Plugins

- 変換ルールを個別に適用するためのもの
- babel-preset-stage-x はこれの集合体

---

「stage-3のObjectRestSpreadが使いたい」  
「けど、stage-3全部入れるのはな〜」

→ `babel-plugin-transform-object-rest-spread`

---

## 設定ファイル

---


`.babelrc`

---

変換時に `.babelrc` というファイルがあると自動的にロードされる。  
PresetsやPluginsの設定はここに書くことが多い。

```
{
  "plugins": ["transform-react-jsx"],
  "ignore": [
    "foo.js",
    "bar/**/*.js"
  ]
}
```

---

あとは色々試してみよう！

---

# webpack

---

〜基本〜

- モジュールバンドラ
- CommonJS/AMD形式の依存解決を可能とする

---

例 : 「Lodash」をブラウザで使いたい

```js
_.defaults({ 'a': 1 }, { 'a': 3, 'b': 2 });
// → { 'a': 1, 'b': 2 }
_.partition([1, 2, 3, 4], n => n % 2);
// → [[1, 3], [2, 4]]
```

---

単純な利用方法(not webpack)

```html
<script src="lodash.js"></script>
<script src="application.js"></script>
```

---

`application.js` だけを見てみると

```js
var hoge = _.defaults({ 'a': 1 }, { 'a': 3, 'b': 2 });
```

---

→ `_` ってどこから出てきたんだ？

---

node.jsの場合


```js
var _ = require('lodash');
var hoge = _.defaults({ 'a': 1 }, { 'a': 3, 'b': 2 });
```

---

ブラウザでも同じように書きたい！！！

---

![Logo](assets/webpack.png)

---

## webpack

`require`(CommonJS)での依存を解決  
↓  
ブラウザで実行可能なファイルとして出力する

---

application.js

```js
var _ = require('lodash');
var hoge = _.defaults({ 'a': 1 }, { 'a': 3, 'b': 2 });
```

html

```html
<script src="application.js"></script>
```

---

## webpackの基本

---

### 設定ファイル

---

#### webpack.config.js

実行時に存在していると自動的にロードされる。

```js
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
};
```

---

ここからの話  
↓  
webpack.config.js の設定について

---

### entry

webpackビルドの起点となるファイル

---

```js
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
};
```
@[4](→ './src/index.js' を起点としてビルドする)

---

```js
entry: {
  home: "./home.js",
  about: "./about.js",
  contact: "./contact.js"
}
```
複数指定することもできる

---

entry1ファイル = 1ファイル出力される

---

### output

entryのファイルを出力する際に、  
どういった形式で出力するかを指定する。

---

```js
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
};
```
@[5-8]
@[6](→ `bundle.js`という名前で)
@[7](→ `dist`配下に出力)


---

複数ファイル出力のケース

```js
{
  entry: {
    app: './src/app.js',
    search: './src/search.js'
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist'
  }
}
```


---

### loader

ファイルを読み込む際に何らかの変換を行う。

---

例 : 「ES6を使いたい！！」

---

![Logo](assets/babel.png)

---

#### babel-loader

```js
module: {
  rules: [
    {
      test: /\.js$/,
      exclude: /node_modules/,
      loader: 'babel-loader'
    }
  ]
}
```

拡張子`.js`のファイルをロードした場合、  
babelによる変換が行われる

--- 

その他loaderの例

---

#### css-loader

javascriptからcssを読み込めるようになる

```js
import css from 'file.css';
```

---

#### coffee-loader

CoffeeScriptを変換する

---

#### vue-loader

Vueの単一ファイルコンポーネントを変換する

```html
<template>
<div>
  {{text}}
</div>
</template>
<script>
export default {
  name: 'Sample’,
  props: ['text']
}
</script>
```

---

やりたいことに応じたloaderを選んで使っていけばok

---

### plugin

最終的な生成物に対して何らかの処理を行う

---

例 : minify(ファイル圧縮)したい！

---

### UglifyJsPlugin

```
const webpack = require('webpack');

module.exports = {
  /*
    entryとかoutputの設定とか
    ...
  */

  plugins:[
    new webpack.optimize.UglifyJsPlugin()
  ]
};
```
@[9-11](最終的な生成物が`UglifyJS`によって圧縮される)

---

その他pluginの例

---

### CommonsChunkPlugin

一部のモジュールだけを別ファイル化できる。  
Reactなどの共通モジュールだけで1ファイルとすることができる。

- 各outputファイルの軽量化
- キャッシュの効率化

---

### EnvironmentPlugin

`process.env.NODE_ENV` などをビルド時点で置き換えてくれる。  

- `development`向けにビルドされた場合のみデバッグログを出力したり

---

基本的なものはwebpackのドキュメントに載ってる

https://webpack.js.org/plugins/

---

ちなみに、loaderもpluginも作ろうと思えば作れる

> (私はまだ作ったこと無い...)

---



----
