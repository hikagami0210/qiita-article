---
title: Next.jsでImageが表示されない現象の解決
tags:
  - Next.js
private: false
updated_at: '2023-10-20T10:55:38+09:00'
id: 6a61920c35e77fe56544
organization_url_name: null
slide: false
ignorePublish: false
---

Next.jsで画像を表示するのに結構手間取ったので備忘録として書いておきます

# 目次
1.[エラー](#エラーメッセージ)
2.[原因と解決法](#原因と解決法)

## エラー

画像を以下のように表示しようとしていたが、表示されなかった。

```Next.js

<Image src='assets/icons/icon.png' alt='logo' width={100} height={100} />

```

## 原因と解決法

### 原因その１ : publicフォルダに入れていなかった
Next.jsで静的ファイルを扱うときはプロジェクト直下にpublicフォルダを作ってそこにいれなきゃならんらしいです。

```Next.js

<Image src='public/icons/icon.png' alt='logo' width={100} height={100} />

```

しかしそれでも表示されない...

### 原因その２ : パスの指定の仕方が間違っていた

Next.jsではpublicフォルダの中身はルートディレクトリとして扱われる?ので、publicフォルダの中身を指定するときは以下のように書く必要があるみたいです。

```Next.js
// <Image src='public/icons/icon.png' alt='logo' width={100} height={100} />
<Image src='/icon.png' alt='logo' width={100} height={100} />
```

ここまでやって無事表示されました。

### 解決法

imageファイルなどの静的ファイルを扱うときは、

1. publicフォルダに入れる
2. パスは`public/icons/icon.png`のような階層構造であっても`/icon.png`と書く


お役に立てば幸いです。
