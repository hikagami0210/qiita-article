---
title: Reactでimgのエラーが起きた時に代わりの画像を表示させる方法
tags:
  - HTML
  - 初心者
  - React
private: false
updated_at: '2024-03-10T22:52:48+09:00'
id: ee3a0d460ffd8d4a85fd
organization_url_name: null
slide: false
ignorePublish: false
---
# はじめに
ReactでWebサービスを開発する際、apiから持ってきた画像のurlを表示させることは多々あると思います。
そのとき何らかの原因で画像が表示できないと、なんとも言えない残念なUIになってしまいます。

そこで、imgタグに`onError`　というイベントハンドラーを使用することによって、画像が読み込めなかった時にデフォルトの画像を表示させるというようなことができます。

# 実際にやってみる

```tsx
import defaultImage from "src/assets/default_image.png";

const Image = ({ src, alt }: { src: string; alt: string }) => {
    return (
            <img
                src={src}
                alt={alt}
                onError={(e) => {
                    const target = e.target as HTMLImageElement;
                    target.src = defaultImage;
                }}
            />
    );
};
```
このように書くと、たとえ`src`の画像がエラーになった時でも、`defaultImage`を表示させることができます。

またこれを関数として切り出すと、もっと使いやすくなります。
例えば、アイコンやヘッダー、サムネイルなどの種類によって画像を分けたいときは以下のように書くことができます。

```tsx
const imageLoadErrorHandler = (
    e: React.SyntheticEvent<HTMLImageElement, Event>,
    imageType: "header" | "thumbnail" | "icon"
) => {
    const target = e.target as HTMLImageElement;

    switch (imageType) {
        case "header":
            target.src = defaultHeader;
            break;
        case "thumbnail":
            target.src = defaultThumbnail;
            break;
        case "icon":
            target.src = defaultIcon;
            break;
        default:
            break;
    }
};

// 使用するとき

const Image = ({ src, alt }: { src: string; alt: string }) => {
    return (
            <img
                src={src}
                alt={alt}
                onError={(e) => {
                    imageLoadErrorHandler(e,"icon")
                }}
            />
    );
};
```

# おわりに
最後までお読みいただきありがとうございました！
もっといい方法などありましたらコメントに書いていただけると嬉しいです!

## 参考リンクなど
https://developer.mozilla.org/ja/docs/Web/HTML/Element/img#%E7%94%BB%E5%83%8F%E8%AA%AD%E3%81%BF%E8%BE%BC%E3%81%BF%E3%82%A8%E3%83%A9%E3%83%BC

https://qiita.com/m3816/items/56b9b6b6f340265cbfab
