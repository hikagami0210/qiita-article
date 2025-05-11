---
title: PostmanでLaravelのAPIを叩いた時、HTMLが返ってくる現象の解決法
tags:
  - API
  - Laravel
  - Postman
private: false
updated_at: '2023-12-30T15:53:06+09:00'
id: 9529dfe8ecdd6e5abc83
organization_url_name: null
slide: false
ignorePublish: false
---
# はじめに
PostmanでLaravelで構築されたapiのテストをしていた時、エラーになるはず(バリデーションに適合しない)リクエストを送っているのに、返ってきたボディはHTMLだしステータスコードも`200`だしで意味がわからなかったので調べました。

# 問題
こんなかんじ。一応FE側(React)ではErrorがちゃんと返ってきており、メッセージも存在している。
![スクリーンショット 2023-12-30 15.33.05.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/cace5f4b-b2f3-e18c-c547-8e170fda6aa9.png)

# 解決方法
リクエストのヘッダーの`Accept`を`application/json`に設定する。
![スクリーンショット 2023-12-30 15.42.17.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/12eb7a61-80cb-02db-94b3-0f8b730fa322.png)

デフォルトでは`*/*`となっていますが、これは「どういった形式で返してくれてもいい」という宣言らしいです。

これだとLaravelはHTMLで返してくるっぽいです。(HTMLが返ってきたことを"正常だ"とPostmanが判定してステータスコードが200になる?)

人間的には`json`で返してくれた方が嬉しいので、`application/json`、つまり`json`で返してくれと明示的に宣言することで`json`形式でレスポンスを返してくれます。

# おわりに
この辺りの知識が薄いので時間をつかってしまいました。
もっと勉強しなきゃなりませんね...

