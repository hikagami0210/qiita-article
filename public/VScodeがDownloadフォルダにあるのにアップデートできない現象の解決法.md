---
title: VScodeがDownloadフォルダにあるのにアップデートできない現象の解決法
tags:
    - Mac
    - VSCode
private: false
updated_at: "2023-08-12T22:33:43+09:00"
id: d6861e8de5bf513c2c96
organization_url_name: null
slide: false
---

調べて出てくるのは大体`Visual Studio Code.app`が Download フォルダにないから、という理由ですが、ちゃんと Application フォルダにあったのにエラーが出ていたので解決法を探した。

## エラー全文

```:error
Cannot update while running on a read-only volume.
The application is on a read-only volume.
Please move the application and try again.
If you're on macOS Sierra or later,
you'll need to move the application out of the Downloads directory.
This might mean the application was put on quarantine by macOS.
See this link for more information.
```

## 解決法

エラー文にある [link](https://github.com/microsoft/vscode/issues/7426#issuecomment-425093469) 先のコマンドで解決。
ターミナル(iTerm)で以下のコマンドを実行し、VScode を再起動するとアップデートが問題なく行えました。

```:zsh
xattr -dr com.apple.quarantine /Applications/Visual\ Studio\ Code.app
```

`Visual\ Studio\ Code.app`の部分は自身の VScode の Application 名に合わせてください。
また、iTerm だと実行時パーミッションエラーが出たので、

システム設定 > プライバシーとセキュリティ > アプリケーション管理

で iTerm に権限を付与すると実行できました。

### おわりに

今回、Qiita CLI を使用して記事を書きましたが思ったより使いやすいのでおすすめです。
https://github.com/increments/qiita-cli
