---
title: FlutterでOpacityウィジェットを使わずに半透明にさせる方法
tags:
  - Flutter
private: false
updated_at: '2023-10-21T15:02:31+09:00'
id: defbad2ea063a4a543cf
organization_url_name: null
slide: false
ignorePublish: false
---
# やりたいこと
Flutterでは、OpacityWidgetを使うことでウィジェットを透明にすることができます。
しかし以下のような状態で、赤色のContainerのみを半透明にしたい場合は別の方法を使わないといけません。

![スクリーンショット 2023-10-21 14.47.47.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/59e56178-d0a6-42e0-7416-da8b1459fffc.png)
↑青い部分まで透明化してしまっている

# 解決法
Colorプロパティの.withOpacityを使う

```dart
Container(
    height : 100,
    width : 100,
              
    color: Colors.red.withOpacity(0.5),
                      ^^^^^^^^^^^^^^^^

    alignment: Alignment.center,
    child : Container(
        height : 50,
        width : 50,
        color : Colors.blueAccent,
    ), 
),
```

こうすることで以下のように.withOpacityを使ったウィジェットのみ、半透明にさせることができます。

![スクリーンショット 2023-10-21 14.59.37.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/9c9e364c-5d8f-5f42-2923-d445f0abec91.png)

お役に立てば幸いです。
