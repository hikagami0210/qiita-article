---
title: tailwind使ってみてハマったこと
tags:
  - tailwindcss
private: false
updated_at: '2024-05-05T23:39:20+09:00'
id: dfd39cac7c799bb3c3c8
organization_url_name: null
slide: false
ignorePublish: false
---
## はじめに
今回は表題の通りtailwindを使ってみてハマったところとその解決法について書いていきたいと思います。

## ハマったところ①　急にスタイルが当たらなくなる
原因と解決法 : コンポーネントのあるファイル/ディレクトリが`tailwind.config.ts`内の`content`に含まれていない

`./src/features`コンポーネントを作成した場合、追記する必要がある
```ts
const config: Config = {
  content: [
    "./src/pages/**/*.{js,ts,jsx,tsx,mdx}",
    "./src/components/**/*.{js,ts,jsx,tsx,mdx}",
    "./src/app/**/*.{js,ts,jsx,tsx,mdx}",
    "./src/features/**/*.{js,ts,jsx,tsx,mdx}", // ←---追記!
  ],

~~~~~~~~~~~ 省略 ~~~~~~~~~~~~~~
```

## ハマったところ② `prettier-plugin-tailwindcss`が効かない
`className`内のユーティリティクラスを並び替えてくれる公式が提供しているprettierプラグインです。

原因と解決法 : `.prettierrc`にプラグインを追記していなかった
```json:.prettierrc
{
    "tabWidth": 4,
    "plugins": ["prettier-plugin-tailwindcss"] 
}
```

こうすることで問題なくフォーマッターが働きました。

## ハマったところ③ js変数をユーティリティクラスに入れても動かない
以下のようなコードです。

```tsx
const width = "30px";
<div className={`w-[${width}]`}></div>;
```

原因と解決策 : tailwindの仕様的に無理

tailwind公式より引用
>Tailwind がクラス名を抽出する方法の最も重要な意味は、ソース ファイル内に完全に切れていない文字列として存在するクラスのみが検索されるということです。
文字列補間を使用する場合、または部分クラス名を連結する場合、Tailwind はそれらを見つけられないため、対応する CSS を生成しません。

なので、`完全に切れていない文字列として存在するクラス`を変数に入れていればいける
例 : `const width = "w-[30px]"`

### おわりに
お読みいただきありがとうございました。

tailwindは初めて触りましたが、慣れるまでは大変そうですね...
クラス名を考えないでよかったり、html-cssをファイルを行き来して照合する手間が省けたりするのはとても良いと感じています。

また、ハマりポイントがあれば随時追記していこうと考えています。
