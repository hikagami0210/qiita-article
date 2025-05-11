---
title: ReactNativeでWebView内の画像をbase64に変換して取得する方法
tags:
  - reactnative
private: false
updated_at: '2025-04-07T00:00:05+09:00'
id: 46cbe2fea4bddb49d16b
organization_url_name: null
slide: false
ignorePublish: false
---
## はじめに
今回は、ReactNativeでWebView内の画像をbase64に変換して取得する方法について紹介していきます。


### 結論のコード

```ts
// 画像URLからblobを取得し、Base64に変換する関数
function convertImageToBase64(url, callback) {
  var xhr = new XMLHttpRequest();
  xhr.onload = function() {
    var reader = new FileReader();
    reader.onloadend = function() {
      callback(reader.result);
    };
    reader.readAsDataURL(xhr.response);
  };
  xhr.onerror = function(err) {
    callback("error fetching image");
  };
  xhr.open('GET', url);
  xhr.responseType = 'blob';
  xhr.send();
}
```

こちらを`injectedJavaScript`に挿入して使用する感じになります。

本来は`fetch`やcanvasを使用すべきだと思いますが、なぜかそれだと弾かれてしまったので最終的に行き着いたのが今回のコードです。

