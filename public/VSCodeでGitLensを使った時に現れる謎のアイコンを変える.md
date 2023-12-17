---
title: 'VSCodeでGitLensを使った時に現れる謎のアイコンを変える'
tags:
  - VSCode
  - GitLens
private: false
updated_at: '2023-11-29T14:40:26+09:00'
id: 9faf2b6de7ba7d04e3fe
organization_url_name: null
slide: false
ignorePublish: false
---
## はじめに
VScodeで`GitLens`を使っている方は多いと思いますが、その際表示される謎のアイコンを変更する方法がなかなか見つからなかったので書き遺しておきます。

謎のアイコン↓
![スクリーンショット 2023-11-28 19.20.33.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/494a8324-9abc-b95d-6e7d-f8cdf452aaf7.png)
![スクリーンショット 2023-11-28 19.20.45.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/30a355d9-9e43-ad92-cb57-e35ef8aa1d48.png)


## 早めに結論
Githubに登録しているメールアドレスで、[Gravatar](https://ja.gravatar.com/)というサービスにアカウント登録することでそのアカウントに登録したアイコン画像が表示されます。

Gravatarを知らなかったので、chatGPTに聞いてみました。

chatGPT ：Gravatar（Globally Recognized Avatar）は、ウェブサイトやオンラインフォーラムなどで利用される、ユーザーを表すプロフィール画像の仕組みです。Gravatarサービスに登録したユーザーは、メールアドレスと関連付けられた画像をアップロードできます。その後、同じメールアドレスを使用して他のウェブサイトやオンラインプラットフォームに登録する際に、Gravatarをサポートしている場合、そのプラットフォームでユーザーのプロフィール画像としてGravatarが表示されます。

主な特徴と利点は以下の通りです：

1. **グローバルに利用可能**: Gravatarは世界中の多くのウェブサイトやサービスで利用されており、一度登録した画像が複数のサイトで再利用されるため、ユーザーは同じプロフィール画像を保持できます。

2. **一貫性と認識性**: 同じメールアドレスを使用すると、異なるサイトで一貫して同じプロフィール画像が表示され、ユーザーの認識が容易になります。

3. **個別設定**: Gravatarを利用するユーザーは、Gravatarのウェブサイトから自分のプロフィール画像をアップロードおよび設定できます。

4. **カスタマイズ可能なサイズ**: ウェブサイトやアプリケーションは、Gravatarから取得するプロフィール画像のサイズを指定できます。

Gravatarはオープンソースで、ユーザーのプロフィール画像を提供する際には、ユーザーのメールアドレスをハッシュ化して利用することでプライバシーを保護しています。一般的に、Gravatarを利用するには、ユーザーがGravatarのウェブサイトでアカウントを作成して、メールアドレスと画像を登録する必要があります。

ということで、とりあえず登録していても損はなさそうです。
ここまでお読みいただきありがとうございました !

## クリップ

https://github.com/gitkraken/vscode-gitlens/issues/305

https://ja.gravatar.com/
