---
title: Flutterで画像やウィジェットを左右反転、上下反転する方法
tags:
  - Flutter
private: false
updated_at: '2023-10-24T19:51:57+09:00'
id: 64f61e7fff3e232635c0
organization_url_name: null
slide: false
ignorePublish: false
---
画像(ImageWidget)を左右反転したかったのでググっていたのですがあまり日本語での情報がなかったので備忘録として書き遺しておきます


## ウィジェットを左右反転させる

`Transform.scale`を使います。
```dart
Column(
  children: [
    Image.network('https://picsum.photos/250?image=9'),
    SizedBox(height:15),
    Transform.scale(
      scaleX: -1,
      child: Image.network('https://picsum.photos/250?image=9'),
    ),
    Text("テスト----"),
    Transform.scale(
      scaleX: -1,
      child: Text("テスト----"),
    ),
  ],
),
```
![スクリーンショット 2023-10-24 19.42.20.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/1f610af8-5f56-e3d6-a1b5-9a474ba3fa0f.png)

また、

```dart
Transform.flip(
    flipX: true,
    child: Image.network('https://picsum.photos/250?image=9'),
),
```
のような書き方もできます。
動作は同じですがこっちのが直感的でいいですね...

## ウィジェットを上下反転させる

左右反転の時は`scaleX: -1,`を用いていましたが、`scaleY: -1,`に変更します。

```dart
Column(
  children: [
    Image.network('https://picsum.photos/250?image=9'),
    SizedBox(height:15),
    Transform.scale(
      scaleY: -1,
      child: Image.network('https://picsum.photos/250?image=9'),
    ),
    Text("テスト----"),
    Transform.scale(
      scaleY: -1,
      child: Text("テスト----"),
    ),
  ],
),
```
![スクリーンショット 2023-10-24 19.46.00.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/658fd08d-e853-fde4-66a6-dfabe2f09693.png)

お役に立てば幸いです。
