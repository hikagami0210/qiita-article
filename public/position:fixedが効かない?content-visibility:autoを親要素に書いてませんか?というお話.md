---
title: 'position:fixedが効かない?content-visibility:autoを親要素に書いてませんか?というお話'
tags:
  - HTML
  - CSS
private: false
updated_at: '2023-11-03T22:05:32+09:00'
id: 3e8bc522a2102c78f568
organization_url_name: null
slide: false
ignorePublish: false
---
表題の通り。
`transform`やら使ってないのになんで`position:fixed`が効かないんだ😡と思って調べてたら、`content-visibility:auto;`が原因でしたよという感じ。

## 実際に`position:fixed`が効かなくなる様子

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="vYbXbqB" data-user="hikagami0210" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/hikagami0210/pen/vYbXbqB">
  Untitled</a> by hikagami (<a href="https://codepen.io/hikagami0210">@hikagami0210</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

content-2に`content-visibility: auto;`をつけてます。
そうすると子であるfixed-contentは`position: fixed;`なのにcontent-2を基準にしてしまっていることがわかるかと思います。

`content-visibility: auto;`をコメントアウトするとしっかり全画面を覆ってくれます。


こうなる原因はいまいちよくわからなかったのですが、よくページの読み込みを早くするとメリットだけ書かれている`content-visibility: auto`もこういったデメリットがあるので私の様に時間を無駄にされぬようご注意ください。

もし共存させる方法がありましたら教えてください。
お読みいただきありがとうございました！

## 参考ページ
https://stackoverflow.com/questions/71331489/css-content-visibility-auto-inadvertently-hides-overflow
