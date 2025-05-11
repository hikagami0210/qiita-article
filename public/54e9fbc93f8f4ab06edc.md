---
title: react-hook-form + zod でnullableかつメールアドレスの形式チェックを行う方法
tags:
  - TypeScript
  - React
  - react-hook-form
  - zod
private: false
updated_at: '2024-01-09T23:24:04+09:00'
id: 54e9fbc93f8f4ab06edc
organization_url_name: null
slide: false
ignorePublish: false
---
# はじめに
Webサイトを作っている時、入力していない場合はバリデーションをかけず、一文字でも入力している場合はemailのバリデーションをかけるという流れで少し詰まったので備忘録として書き残しておきます。

# 問題
emailの入力フォームで
- 入力は任意(nullでもいい)
- 入力した場合はemailの形式であるかバリデーション
 という二つを両立させる方法がわからなかった
 
# 解決方法
<font color="red">**追記です**</font>

X(Twitter)でもっといい方法を教えていただきました！

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">The solution proposed in the referenced thread has always worked well for me.👌<br><br>z.string().email().optional().or(z.literal(&#39;&#39;))</p>&mdash; moshyfawn (@moshyfawn) <a href="https://twitter.com/moshyfawn/status/1744717567225405939?ref_src=twsrc%5Etfw">January 9, 2024</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

ということで、
```ts
z.string().email().optional().or(z.literal(''))
```
のみで今回タイトルにあるバリデーションは問題なく行えるようです！

そもそも正規表現を自前で書いた理由が、zodの`.email`では日本語の文字を許容してしまうという情報に基づいたものでしたが、バージョン3.22.0にて修正されていたようです🫨
実際に確認せずに古い情報を信じてコードを書くのはダメですね...

バージョン3.22.0以前であれば以下の対応で問題ないかと思われます。

`superRefine`を使用する。

https://zod.dev/?id=superrefine

```ts
const registerFormValidation = z
    .object({
        email: z.string().nullable(),
    })
    .superRefine(({ email }, ctx) => {
        if (email !== null && email.length > 0) {

            const regex =
                /^[a-zA-Z0-9_.+-]+@([a-zA-Z0-9][a-zA-Z0-9-]*[a-zA-Z0-9]*\.)+[a-zA-Z]{2,}$/;
            if (!regex.test(email)) {
                ctx.addIssue({
                    path: ["email"],
                    code: "custom",
                    message: "メールアドレスの形式が不正です",
                });
            }
        }
    });
```

まず、`email: z.string().nullable(),`で値がnullの時はバリデーションをとおるようにします。
そのあと、`superRefine`をemailの文字数が1以上の時正規表現を用いてテストします。
正規表現に当てはまらなかった場合、ctx.addIssueを登録してエラーメッセージを生成します。

# おわりに
もっといい方法があれば教えてください...!

# 参考サイト

https://zod.dev/

https://javadrive.jp/regex-basic/sample/index13.html

