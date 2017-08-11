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

ポイント

- 目的に応じてモジュールが細かく分離

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

### babel-register

node.jsの `require` に時にフックして変換を行う。

```
require("babel-register");

# → .es6, .es, .jsx, .js をrequireすると変換される
```

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

Presetsを使いたいstageに応じて選んで使う。

- babel-preset-stage-0
- babel-preset-stage-1
- babel-preset-stage-2
- babel-preset-stage-3
- babel-preset-env

```
babel script.js --presets=stage-2
```

---

### Transform Plugins



---

オフィシャルページ  
`Ready to get started?` より引用

```
npm install --save-dev babel-cli babel-preset-env
```

---



---

### preset


---





---

# webpack

---


### entry

---

### output

---

### loader

--- 

### plugin

---

---

bbb
