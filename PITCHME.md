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
- ECMAScriptの次世代機能をブラウザで使えるようにする

---

ポイント

- 目的に応じてモジュールが細かく分離

---

### babel-core

コア機能。  
後述するpresetやpluginによって  
**シンタックスの変換**のみを行う。

```
(x, y) => x + y;
```
↓
```
"use strict";

(function (x, y) {
  return x + y;
});
```

---

### babel-polyfill

シンタックスの変換でサポートできないような、  
グローバルライブラリや、新しいネイティブメソッドを使えるようにする。

```
Promise
```

```
Object.assign(x, y);
```

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
