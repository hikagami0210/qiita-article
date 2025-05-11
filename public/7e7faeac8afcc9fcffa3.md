---
title: JavaScriptのクロージャーについて理解しようとしてみた
tags:
  - JavaScript
private: false
updated_at: '2024-04-28T23:58:18+09:00'
id: 7e7faeac8afcc9fcffa3
organization_url_name: null
slide: false
ignorePublish: false
---
## はじめに
JavaScriptについて勉強する機会があり、その中でクロージャーなるもの(名前は知っていた)を講義してもらったのですがイマイチどう使えばいいものかわからなかったので今回調べてみました！

## クロージャーとは
`クロージャは、組み合わされた（囲まれた）関数と、その周囲の状態（レキシカル環境）への参照の組み合わせです`(参考:[MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Closures))

```js
function makeFunc() {
  const name = "Mozilla";
  function displayName() {
    console.log(name);
  }
  return displayName;
}

const myFunc = makeFunc();
myFunc(); // Mozilla
```
再度MDNからのコピーとなりますが、この時`myFunc（）`は`Mozilla`をログに表示します。
これは冷静に考えると、すこし理解し難い結果です。`const myFunc = makeFunc();`が完了した時点で`name`が破棄され、`name`は参照できなくなりエラーとなる方が直感的には理解できます。

しかしJSに限ってはそうではなく、`name`は`Mozilla`という値を保持し続けます。これは**レキシカルスコープ**という仕組みによるものです。

レキシカルスコープは 関数を定義した時点でスコープが決まります。
再度上のコードを見てみましょう。

```js
function makeFunc() {
  const name = "Mozilla";
  function displayName() {
    console.log(name);
  }
  return displayName;
}

const myFunc = makeFunc();
myFunc(); // Mozilla
```
関数を定義した時点でスコープが決まる。という約束に則れば、`displayName()`が`name`を参照できることが理解できるのではないでしょうか。

ここで最初の`クロージャは、組み合わされた（囲まれた）関数と、その周囲の状態（レキシカル環境）への参照の組み合わせです。`を読み返すと、これもまた納得できませんか?

ちなみに、この時クロージャーは`displayName()`です。`displayName()`がクロージャーになって`myFunc`に代入されているという認識で大丈夫なはずです。

### 実際どう役立つのか?を調べてみた
#### 変数を外部から変更されないようにできる
調べているとよく出てくる使い方の中に、変数を外部から変更されないようにできる、というものがあります。
オブジェクト指向言語のプライベートプロパティと同じようなことができます。

```js
function setToken(userToken) {
    var token = userToken;
    return function () {
        return token;
    };
}

const getToken = setToken("0123456789");

console.log(getToken()); // 0123456789
```

この例では、tokenは変更できない変数として保存できます。
しかし、これはそもそもconstで宣言すればいい話なので実用性は低いんでしょうか?🧐
グローバルな変数を増やさないという点での恩恵が大きいのかなと思います。

#### カウンターなど、外部から値を弄られない且つ、呼び出すたびに値を変化させる

```js
function counter() {
    let count = 0;
    return function () {
        count++;
        console.log(count);
    };
}

const myCounter = counter();

myCounter(); // 1
myCounter(); // 2
myCounter.count = 100;
myCounter(); // 3
```

この例では、呼び出すたびに値が1足され続ける処理を実現できます。
さらに、`count`の値は外からいじることはできません。

#### 動的な関数を作る

```js
function counter(initCount) {
    let count = initCount;
    return function () {
        count++;
        console.log(count);
    };
}

const myCounter10 = counter(10);
const myCounter100 = counter(100);

myCounter10(); // 11
myCounter10(); // 12
myCounter100(); // 101 
```

この例では、前項のカウンターに初期値を持つ機能を足した例です。
また、クロージャーは実行するたび別の関数を作るので(今回の例だと`count`が10の関数、100の関数)、別々のカウンターとして存在できます。

### クロージャーのデメリット
クロージャーのデメリットとしてはメモリリークの原因になってしまうことが挙げられます。
使用していない変数を持ち続けることになるので、さもありなんといったところですが...
あまりに巨大な変数をクロージャーで持ち続けたり、クロージャーを多用するのはやめた方が良いということですね。


### おわりに
最後までお読みいただきありがとうございました！
今までReactばっかり触っていたので、少し理解に苦しんでいましたがなんとなく理解ができました。
認識が間違っている箇所などあればコメントにお願いいたします🙇‍♂️
