---
title: FirebaseAnalyticsのDebugViewで実機のAndroid端末が表示されない時にやったこと　
tags:
  - Android
  - Flutter
  - FirebaseAnalytics
private: false
updated_at: '2023-12-17T17:35:12+09:00'
id: da9e652e5104d6d9929f
organization_url_name: null
slide: false
ignorePublish: false
---
## やりたかったこと
FirebaseAnalyticsのDebugViewでログをリアルタイムで取得したかったが、
デバッグに使用するデバイスのところにデバイスが表示されなかった。

## 直接の原因

__<font color="Red">広告ブロッカーをつけていた</font>__

私のAndroidではAdGuardを使用していたのですが、AdGuardをつけている状態だと`デバッグに使用するデバイス`に表示されないようです...(ログ自体も送信されない?)

## 他に注意すること
結局、広告ブロックが原因だったのですが、もうひとつミスってたことがあったので書き残しておきます。

### adbコマンドが間違っていないか
androidでログを送信する際には、以下のadbコマンドを打つ必要があります。

```
adb shell setprop debug.firebase.analytics.app パッケージ名
```

パッケージ名を間違えていてもエラーなどはでないため、注意が必要です。

adbコマンドを打った後、再起動する必要があるのも注意です。
