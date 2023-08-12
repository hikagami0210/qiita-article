---
title: '【プログラミング初心者向け】Macのターミナルを超便利にカスタマイズしてみよう '
tags:
  - Zsh
  - Mac
  - iTerm2
  - Terminal
private: false
updated_at: '2023-08-12T22:33:46+09:00'
id: 560bd0b2a413ef08ad46
organization_url_name: null
slide: false
---

## はじめに

Mac に搭載されているターミナルを使いやすくカスタマイズしてしまおうという趣旨の記事です。
できるだけ初心者の方でもわかりやすいよう書くつもりです。少し長いですが完成すれば使い心地は天と地の差だと思うので是非試してみてください!

## どんな事ができるようになるのか

・ コマンドの履歴を一覧表示して選択
・ `git commit -m ''`などのコマンドをスニペットとして保存、3 文字程度のコマンドで呼び出し
・ 常に現在のブランチや時間を表示
・ よく使うディレクトリをブックマーク。どこからでも楽に`cd`
・ 間違ったコマンドは赤字で表示。実行できるときは緑で表示

などなど、標準のターミナルよりはるかに使いやすくなります。

![zsh-custom-demo.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/db294079-9586-ae7e-48b4-0a2924c4c77f.gif)

## この記事を読んで zsh をカスタマイズする上での注意

:::note warn
筆者が記事を書くのに慣れておらず、また少し複雑な作業なので**読み飛ばしがないようご注意ください**。
複雑とは言っても、一つずつ丁寧に実行していけば問題なくカスタマイズは終わるはずです。
読むのがめんどくさくても、手順を間違えると余裕で機能しなくなるので注意です
:::

## 環境

macOS(M1 チップ搭載) ventura (バージョン 13.2.1)
zsh 5.8.1 (x86_64-apple-darwin22.0)

## 下準備

## 現在のシェルを確認

```zsh:Terminal
echo $SHELL
```

結果が`/bin/zsh`なら OK
それ以外なら

```zsh:Terminal
chsh -s /bin/zsh
```

でパスワード打ち込んでシェルを zsh に変更

## `.zshrc`をバックアップ

path などが万が一にも消えないように避難させておきます。
そもそも`.zshrc`がないよ!って人、現在 oh-my-zsh を使用している人は飛ばして OK です。

```zsh:Terminal
cp ~/.zshrc ~/.zshrc_backup
```

## oh-my-zsh などを使用している場合はアンインストール

今回 oh-my-zsh は使用しないのでアンインストールします。

```zsh:Terminal
uninstall_oh_my_zsh
```

実行時、`.zshrc`が自動でコピーされ`~/.zshrc.omz-uninstalled-{日時}`というファイルが生成されます。PATH などはここから新しい`.zshrc`に移しましょう！

<details><summary>余談</summary>

以前 oh-my-zsh インストール時に「.zshrc リセット!?なにしてくれてんだ!!」と憤っていましたが、しっかり　`~/.zshrc.pre-oh-my-zsh-{日時}`　にバックアップされるようです...。

</details>

## Homebrew をインストール

[`Homebrew`](https://brew.sh/)というパッケージマネージャーをインストールします。

```zsh:Terminal
brew -v
#Homebrew 4.0.28
```

バージョンが表示されれば飛ばして OK。表示されない方は

```zsh:Terminal
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

でインストールしてください。

<details><summary>M1チップのMacの場合</summary>

.zshrc にパスを通す必要があります。

```zsh:Terminal
vi ~/.zshrc
```

を実行し、PATH をどこかに書いておきましょう。

```zsh:.zshrc
export PATH=/opt/homebrew/bin:$PATH
```

vi の基本的な操作は、`i`キーで編集モードにし、上記のパスを`command + v`で貼り付け、`esc`キーで編集モードを抜け`:wq`で上書き保存し終了です。

</details>

## いよいよ構築！

ここから構築に入ります。

これ以降、`.zshrc`というファイルを編集しまくるのですが、vi で編集するのは不便なので VScode を使用している人はそちらで行いましょう。
未使用の方はこの機会にぜひインストールしましょう!

:::note warn
`.zshrc`って？って方は、下記を確認してから構築を始めてください !

**`.zshrc`ファイルとは zsh(ターミナル)の設定を記載するファイルです。**
ホームディレクトリに存在し、ファインダーでは`shift + command + .`で表示されます。
最初はファイルごと存在しないかもしれませんが、構築する時生成されるので大丈夫です。

また、以降ターミナルでコマンドを打ったり、`.zshrc`に追記したりとすこしややこしいです。
**コードブロック左上の表示でどこで実行（追記）するのかを必ず確認してください!**

左上が`iTerm`ならコマンドをターミナルで実行します。
![スクリーンショット 2023-07-10 21.38.11.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/0449487d-fdea-35ec-1a86-63ee0fdf6402.png)

`.zshrc`なら vi や VSCode を使用し`.zshrc`に追記します。
![スクリーンショット 2023-07-10 21.38.41.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/7513c477-8ffe-74bf-f5c4-651a23802c29.png)

<details><summary>VScodeで.zshrcを編集する方法(強く推奨)</summary>

VScode 未インストールの場合は`VSCode インストール mac`とでもググってインストールしましょう。
難しいことは特にないのですぐ終わるかと思います。

インストール済みの場合、VScode で`shift + command + P`でコマンドパレットを開き、`shell command`を入力します。
![スクリーンショット 2023-07-09 21.02.22.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/610becc3-feef-95f5-8c39-b55350253f9d.png)
`シェルコマンド : PATH内にcodeコマンドをインストールします`を選択し権限が求められたら許可。
成功のダイアログがでたら完了です。

```zsh:Terminal
code ~/.zshrc
```

で`.zshrc`を VSCode にて編集できるようになります!

</details>

:::

## 1. iTerm2 のインストール

iTerm2 というターミナルアプリをインストールします。もともとのターミナルより良い機能が揃ったアプリです!

https://iterm2.com/

1. 上のサイトを開き、サイト下部の Download ボタンでダウンロード
2. ファインダーで iTerm を開く
3. 開くとアプリケーションフォルダに移動するか質問されるので許可
4. ダイアログで色々質問されますが、全て許可して問題ありません。

起動したら`echo $SHELL`でシェルが zsh かも確認しておきましょう。

**以降のコマンド入力などの作業は全てこの iTerm で行います。**
**元々あったターミナルは以降使用しません。**

## 2. Zinit のインストール

[`Zinit`](https://github.com/zdharma-continuum/zinit)は、zsh のプラグインマネージャーです。起動も早くなるらしいです。

以下のコマンドを順に実行します。

```zsh:iTerm2
sh -c "$(curl -fsSL https://git.io/zinit-install)"
source ~/.zshrc
zinit self-update
```

![スクリーンショット 2023-07-08 17.40.38.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/e296da8c-193d-92a6-a49a-13945a5a55df.png)
このような表示がなされれば OK です。`y`を押して続行しましょう。

コマンドを入力できるようになったら次に進みます。

:::note
`source ~/.zshrc`は.zshrc を読み込むコマンドです。
**今後.zshrc をいじった後は実行することをおすすめします。**
:::

## 3. powerlevel10k をインストール

ターミナルを自分好みの見た目にカスタマイズできるプラグインです。

https://github.com/romkatv/powerlevel10k

`.zshrc`に以下を記載します。**ターミナルで実行するわけではないので注意！**

```zsh:.zshrc
zinit light romkatv/powerlevel10k
```

そして`source ~/.zshrc`を実行すると`powerlevel10k`の設定ウィザードが起動します。
![p10k.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/f500df55-6e91-3920-e545-54b72a4a3246.png)
推奨フォントをインストールするか質問されるので`y`で続行。

推奨フォントを使用しない場合、絵文字が表示されなかったり不都合が生じます。

フォントのインストールが終わると iTerm の再起動を求められるので再起動します。

**もし推奨フォントをインストールするか質問されなかった場合、以下の手順でフォントをインストールしてください。**

<details><summary>推奨フォントのインストール方法</summary>
1. Githubのページからフォントをダウンロード

[このページ](https://github.com/romkatv/powerlevel10k#fonts)にアクセスし、
![スクリーンショット 2023-07-09 22.36.57.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/bad015e5-ddb9-a924-f823-cc8e8e0a9b08.png)
を 4 つともダウンロード。

2\. フォントのインストール
Finder でダブルクリックして 4 つともインストールします。

3\. フォントの適用
iTerm で`command + ,`で設定を開き画像の場所でインストールしたフォントを適用させます。![スクリーンショット 2023-07-09 22.39.31.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/9d2b810c-7757-edee-03e1-c8608ee6d33e.png)

これでフォントの設定は終了です。

</details>

また、そもそもウィザードが表示されなかった場合は`p10k configure`をターミナルで実行してください。

再起動後、表示されるウィザードにしたがってテーマを設定していきます。

色々なデザインを提示されるので、各自で好きなデザインに設定してください。

例えば、3 番のデザインがいいなら`3`キーを押す、といった手順です。

::: note warn
選択肢に`(recommend)`と書かれた選択肢は原則それを選択してください。

例外的に`Instant Prompt Mode`のときのみ(2)を選択してください。

![スクリーンショット 2023-07-10 22.36.20.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/3d3c2cc6-670d-5540-e79f-009a475b87b8.png)

:::

私は最終的にこうなりました。
![スクリーンショット 2023-07-09 22.49.31.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/8ecaced9-0f26-724a-cac4-3d9ac93ef740.png)

やり直したい場合、`p10k configure`で再設定できます。

## VSCode のターミナルで表示がおかしい場合(アイコンが表示されないなど)

VSCode のターミナルで`powerlevel10k`の設定は反映されているけど、絵文字が表示されていない、などの場合、VSCode の`settings.json`を編集する必要があります。

VScode 上にて`shift + command + P`でコマンドパレットを開き、`open settings`と入力します。

`Preference: open Settings (JSON)`または
`Preference: open User settings(JSON)`をクリックします。

Json ファイルが開くので、下記の 2 行を追記しましょう。フォントサイズはお好みで変えてください。

```json:settings.json
"terminal.integrated.fontFamily": "MesloLGS NF",
"terminal.integrated.fontSize": 12,
```

<details><summary>例</summary>

```json:settings.json
{
  "git.autofetch": true,
  "terminal.integrated.inheritEnv": false,
  "terminal.integrated.fontFamily": "MesloLGS NF", <ここと
  "terminal.integrated.fontSize": 12,　 <ここ。
  "explorer.confirmDragAndDrop": false,
  "workbench.iconTheme": "vscode-icons",
}
```

</details>

## VSCode のターミナルが powerlevel10k で設定した見た目に変わっていない場合

`powerlevel10k`で見た目を変えたのにそれが反映されていない場合、まずは VSCode の再起動を試してください。(`command + Q`などで完全に終了させます)

それでも治らない場合、以下の解決法で反映されるかと思います。

<details><summary>解決法</summary>

1. VSCode で`shift + command + P`でコマンドパレットを表示
2. `Open User Settings`と入力し、`Preference: Open User Settings`を開きます
3. 下の画像のように、`terminal`で絞り込み、`ターミナル(Terminal)`タブから`Terminal > External: Osx Exec`を探し、`iTerm.app`に書き換えます。

![スクリーンショット 2023-07-10 22.08.22.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/b5ba9bdc-f19d-e2a4-abba-973ba86751d0.png)
これで変更が反映されます

</details>

## iTerm 起動時にエラーが出てしまう場合

iTerm 起動時に
`[WARNING]: Console output during zsh initialization detected.`というエラーがでてしまった時の対処法

<details><summary>エラー全文</summary>

```zsh:iTerm
[WARNING]: Console output during zsh initialization detected.

When using Powerlevel10k with instant prompt, console output during zsh
initialization may indicate issues.

You can:

  - Recommended: Change ~/.zshrc so that it does not perform console I/O
    after the instant prompt preamble. See the link below for details.

    * You will not see this error message again.
    * Zsh will start quickly and prompt will update smoothly.

  - Suppress this warning either by running p10k configure or by manually
    defining the following parameter:

      typeset -g POWERLEVEL9K_INSTANT_PROMPT=quiet

    * You will not see this error message again.
    * Zsh will start quickly but prompt will jump down after initialization.

  - Disable instant prompt either by running p10k configure or by manually
    defining the following parameter:

      typeset -g POWERLEVEL9K_INSTANT_PROMPT=off

    * You will not see this error message again.
    * Zsh will start slowly.

  - Do nothing.

    * You will see this error message every time you start zsh.
    * Zsh will start quickly but prompt will jump down after initialization.

For details, see:
https://github.com/romkatv/powerlevel10k/blob/master/README.md#instant-prompt

-- console output produced during zsh initialization follows --
```

</details>

<details><summary>解決法</summary>

Powerlevel10k の設定ファイル、`.p10k.zsh`を編集する必要があります。

```zsh:iTerm
code ~/.p10k.zsh
```

で`.p10k.zsh`を開き、`command + f`で検索バーに
`typeset -g POWERLEVEL9K_INSTANT_PROMPT=verbose`を入力します。

`typeset -g POWERLEVEL9K_INSTANT_PROMPT=verbose`を
`typeset -g POWERLEVEL9K_INSTANT_PROMPT=quiet`に変更し、保存します。

iTerm で`source ~/.zshrc`を打てばエラーはなくなるはずです!

</details>

## 4. 便利なオプションを追加

zsh では何かをインストールせずとも、`.zshrc`に追記するだけで ON にできるオプションがあります。
おすすめのオプションとその効果を書いたリストを以下に記載するので、`.zshrc`に追記してください。

```zsh:.zshrc

# -------------------------------------------------------
#      setting....
# -------------------------------------------------------

# コマンド履歴の管理ファイル
HISTFILE=~/.zsh_history
# メモリに保存する履歴のサイズ
export HISTSIZE=10000
# ファイルに保存する履歴のサイズ
export SAVEHIST=100000
# 補完で小文字でも大文字にマッチさせる
zstyle ':completion:*' matcher-list 'm:{a-z}={A-Z}'
# コマンドのスペルを訂正
setopt correct
# 補完候補を詰めて表示
setopt list_packed
## 他のzshと履歴を共有
setopt inc_append_history
setopt share_history
## パスを直接入力してもcdする
setopt AUTO_CD
# 補完候補をカーソルで選択
zstyle ':completion:*:default' menu select=1
# スラッシュを単語の区切りと見なす
autoload -Uz select-word-style
select-word-style bash
WORDCHARS='.-'
# ヒストリーに重複を表示しない
setopt hist_ignore_all_dups
# 重複するコマンドが保存されるとき、古い方を削除する。
setopt hist_save_no_dups
# コマンドのタイムスタンプをHISTFILEに記録する
setopt extended_history
# HISTFILEのサイズがHISTSIZEを超える場合、最初に重複を削除
setopt hist_expire_dups_first
# コマンドラインでも # 以降をコメントと見なす
setopt interactive_comments
# LSDで使用されるカラーを変更(ディレクトリのみ変更)
export LS_COLORS="di=36"
# cd抜きでもパスと認識されればcd
setopt AUTO_CD
```

一部働かないオプションがあるかもしれませんが、この記事全ての項目が終われば動くはずです。

## ５. 各種便利なプラグインのインストール

ここでは必須とも言える便利なプラグイン、見た目を整えるプラグインのインストールを行なっていきます。

まず５つのプラグインを紹介します。

インストールはあとでまとめて行うので簡単な説明と使い方を記載します。

詳しい使い方は公式の Github などで確認できます。

### [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)

過去に入力したコマンドから候補を引っ張って薄く表示してくれるプラグイン
![スクリーンショット 2023-07-08 16.19.34.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/f2a93401-b7a4-bd4c-fd6a-d4c0807bc5d1.png)

### [fast-syntax-highlighting](https://github.com/zdharma-continuum/fast-syntax-highlighting)

入力したコマンドに色付けしてくれるプラグイン
![スクリーンショット 2023-07-09 23.47.02.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/d0176bab-dadb-6c6b-3858-2f3aa29e324c.png)
間違っている場合赤字にしてくれる機能もあります。
![スクリーンショット 2023-07-08 16.19.53.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/f5ef3ff7-086c-be10-28d4-a8bb29b8c4b5.png)

### [zsh-completions](https://github.com/zsh-users/zsh-completions)

`Tab`キーで次に来るコマンドを補完してくれるプラグイン
![スクリーンショット 2023-07-10 1.08.07.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/486ffcec-aeec-b804-3019-b34a9bd85ca0.png)

### [cd-bookmark](https://qiita.com/mollifier/items/46b080f9a5ca9f29674e)

ディレクトリを任意の名前でブックマークし、簡単に`cd`できるようになるプラグイン

よく使用するディレクトリはブックマークしておきましょう。

```zsh:.zshrc
# -a オプションで今いるディレクトリをブックマークに追加する。(複数登録可)
% cd-bookmark -a name　<=任意の名前

# 'name' を指定すれば上で設定したディレクトリに戻れる
% cd-bookmark name

# オプション無しで登録済のブックマーク一覧を表示する
% cd-bookmark
name|{設定したディレクトリ}
```

### [LSDeluxe](https://github.com/lsd-rs/lsd)のインストール

`ls`コマンドを使用した時の表示をカラフルに、見やすくするプラグイン

参考画像(公式 Github より)
![参考](https://raw.githubusercontent.com/Peltoche/lsd/assets/screen_lsd.png)

### ここまでのプラグイン 5 つを一気にインストールします

`.zshrc`に追記

```zsh:.zshrc
autoload -Uz compinit
compinit

zinit light zdharma-continuum/fast-syntax-highlighting
zinit light zsh-users/zsh-autosuggestions
zinit light zsh-users/zsh-completions
zinit light mollifier/cd-bookmark
```

LSDeluxe は Homebrew でインストールします。

```zsh:iTerm2
brew install lsd
```

`.zshrc`に追記します。

```zsh:.zshrc
alias ls='lsd'
alias l='ls -l'
alias la='ls -a'
alias lla='ls -la'
alias lt='ls --tree'
```

これを追記することにより、`ls`コマンドを打った時`lsd`に自動で変換されます。

この`alias`、自分の好きなように色々設定できるのでよく打つコマンドには打ちやすいものを設定しておくと便利です！

ではその他のプラグインをインストールします。

### [peco](https://github.com/peco/peco)・[anyframe](https://qiita.com/mollifier/items/81b18c012d7841ab33c3) のインストール

シェルフィルタリングプラグイン、とそれを使いやすいようにするプラグイン

簡単に言うと履歴の確認、検索が便利になります。

peco のインストール

```zsh:iTerm2
brew install peco
```

anyframe のインストール

```zsh:.zshrc
zinit light mollifier/anyframe
```

以下を`.zshrc`に追記

```zsh:.zshrc
# -----------------------------
#  Anyframe bindKey
# -----------------------------

# # ディレクトリの移動履歴を表示
bindkey '^]' anyframe-widget-cdr
autoload -Uz chpwd_recent_dirs cdr add-zsh-hook
add-zsh-hook chpwd chpwd_recent_dirs

# # コマンドの実行履歴を表示
bindkey '^R' anyframe-widget-execute-history

# # Gitブランチを表示して切替え
bindkey '^x^b' anyframe-widget-checkout-git-branch
```

追記し終わったら`source ~/.zshrc`を実行し、

`control + R` でこのような画面が出てきたら成功です
![スクリーンショット 2023-07-09 14.38.14.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/0e619bdf-3c69-4059-756c-b4b653333514.png)
これは`bindkey '^R' anyframe-widget-execute-history`部分の機能で、コマンドの履歴が一覧表示されています。

各部の色がなんか気にいらないので変えましょう。(気にならない人は飛ばして OK)

```zsh:iTerm2

mkdir ~/.config/peco
touch config.json

```

で config ファイルを作成し、

`code ~/.config/peco/config.json`

などで`config.json`を開いて追記します。

```json:config.json
{
  "Style": {
    "Basic": ["on_default", "default"],
    "Selected": ["underline", "on_cyan", "black"],
    "SavedSelection": ["bold", "on_yellow", "white"],
    "Query": ["yellow", "bold"],
    "Matched": ["yellow"]
  }
}

```

![スクリーンショット 2023-07-09 14.43.25.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/5b6a629b-9ebb-419d-3ef0-03a78dffd6d8.png)

色はお好みで変えてみてください。[peco の style 設定一覧はこちら](https://github.com/peco/peco#styles)

### [Zeno.zsh](https://github.com/yuki-yano/zeno.zsh)のインストール

便利なスニペットを設定できるプラグインです。

前述の`alias`よりも使いやすく、細かい設定が可能です。

また履歴に展開後のコマンドが残るところも便利です。

まず前提パッケージのインストールをします。

```zsh:iTerm2
brew install deno
brew install fzf
```

問題なく終わったら`zeno.zsh`のインストール・設定をします。

以下を`.zshrc`に追記します。

```zsh:.zshrc
export ZENO_HOME=~/.config/zeno

zinit ice lucid depth"1" blockf
zinit light yuki-yano/zeno.zsh

if [[ -n $ZENO_LOADED ]]; then
  bindkey ' '  zeno-auto-snippet
  bindkey '^g' zeno-ghq-c
  bindkey '^x' zeno-insert-snippet
fi
```

次にスニペットを設定する`config.yml`ファイルを作成します。

```zsh:iTerm2

mkdir ~/.config/zeno
touch config.yml

```

で config ファイルを作成し、

`code ~/.config/zeno/config.yml`

などのコマンドで`config.yml`を開き以下を追記します。

```yml:config.yml

snippets:
  # 標準的なsnippet
  - name: git status
    keyword: gs
    snippet: git status --short --branch

  # {{}}で囲むことによって''内に自動でカーソルが移動
  - name: git commit message
    keyword: gcim
    snippet: git commit -m '{{commit_message}}'

```

保存して設定は完了です。

お好みでよく使うコマンドを設定しておくと便利です。

[**細かい使い方はこちら（zeno.zsh 作者様の記事）**](https://zenn.dev/yano/articles/zsh_plugin_from_deno#abbrev-snippet)

設定した keyword を入力し、`space`キーを入力することによって設定した snippet が展開されます。

`control + x`で設定したスニペット一覧を表示、またそこから選択して実行も可能です。

これでこの記事でのすべての設定は終わりです。お疲れ様でした!

::: note
下準備の段階で PATH などを避難させている方は`.zshrc`に戻しておきましょう。
:::

<details><summary>この記事における.zshrcの完成形</summary>

```zsh:.zshrc
if [[ ! -f $HOME/.local/share/zinit/zinit.git/zinit.zsh ]]; then
    print -P "%F{33} %F{220}Installing %F{33}ZDHARMA-CONTINUUM%F{220} Initiative Plugin Manager (%F{33}zdharma-continuum/zinit%F{220})…%f"
    command mkdir -p "$HOME/.local/share/zinit" && command chmod g-rwX "$HOME/.local/share/zinit"
    command git clone https://github.com/zdharma-continuum/zinit "$HOME/.local/share/zinit/zinit.git" && \
        print -P "%F{33} %F{34}Installation successful.%f%b" || \
        print -P "%F{160} The clone has failed.%f%b"
fi

source "$HOME/.local/share/zinit/zinit.git/zinit.zsh"
autoload -Uz _zinit
(( ${+_comps} )) && _comps[zinit]=_zinit

if [[ -r "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh" ]]; then
  source "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh"
fi

[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh

autoload -Uz compinit
compinit

# -----------------------------------------------------------------------------
#   Plugin list
# -----------------------------------------------------------------------------

zinit light romkatv/powerlevel10k
zinit light zdharma-continuum/fast-syntax-highlighting
zinit light zsh-users/zsh-autosuggestions
zinit light zsh-users/zsh-completions
zinit light mollifier/anyframe
zinit light mollifier/cd-bookmark
zinit ice lucid depth"1" blockf
zinit light yuki-yano/zeno.zsh

# ---------------------------------------------------------------------------
#        PATH ...
# ---------------------------------------------------------------------------

export PATH="$PATH:/usr/local/bin"
export PATH=/opt/homebrew/bin:$PATH
export ZENO_HOME=~/.config/zeno

# -----------------------------------------------------------------
#      setting....
# -----------------------------------------------------------------

# コマンド履歴の管理ファイル
HISTFILE=~/.zsh_history
# メモリに保存する履歴のサイズ
export HISTSIZE=10000
# ファイルに保存する履歴のサイズ
export SAVEHIST=100000
# 補完で小文字でも大文字にマッチさせる
zstyle ':completion:*' matcher-list 'm:{a-z}={A-Z}'
# コマンドのスペルを訂正
setopt correct
# 補完候補を詰めて表示
setopt list_packed
## 他のzshと履歴を共有
setopt inc_append_history
setopt share_history
## パスを直接入力してもcdする
setopt AUTO_CD
# 補完候補をカーソルで選択
zstyle ':completion:*:default' menu select=1
# スラッシュを単語の区切りと見なす
autoload -Uz select-word-style
select-word-style bash
WORDCHARS='.-'
# ヒストリーに重複を表示しない
setopt hist_ignore_all_dups
# 重複するコマンドが保存されるとき、古い方を削除する。
setopt hist_save_no_dups
# コマンドのタイムスタンプをHISTFILEに記録する
setopt extended_history
# HISTFILEのサイズがHISTSIZEを超える場合、最初に重複を削除
setopt hist_expire_dups_first
# コマンドラインでも # 以降をコメントと見なす
setopt interactive_comments
# LSDで使用されるカラーを変更(ディレクトリのみ変更)
export LS_COLORS="di=36"
# cd抜きでもパスと認識されればcd
setopt AUTO_CD

# ----------------------------
# alias
# ----------------------------
# 使わないaliasは消してもいい
# zeno-snippet > ~/.config/zeno/config.yml

alias g="git"
alias gb="git branch"
alias gsw="git switch"
alias gco="git checkout"

alias ls='lsd'
alias l='ls -l'
alias la='ls -a'
alias lla='ls -la'
alias lt='ls --tree'

alias tp='cd-bookmark'

alias dcb="docker-compose build"
alias dcu="docker-compose up -d"
alias dcd="docker-compose down"
alias dce="docker-compose exec"
alias dcps="docker-compose ps"

alias sz="source ~/.zshrc"


# -----------------------------
#  Anyframe bindkey
# -----------------------------
# color config > ~/.config/peco/config.json

# ディレクトリの移動履歴を表示
bindkey '^]' anyframe-widget-cdr
autoload -Uz chpwd_recent_dirs cdr add-zsh-hook
add-zsh-hook chpwd chpwd_recent_dirs

# コマンドの実行履歴を表示
bindkey '^R' anyframe-widget-execute-history

# Gitブランチを表示して切替え
bindkey '^x^b' anyframe-widget-checkout-git-branch


# -----------------------------
#  zeno setting
# -----------------------------
if [[ -n $ZENO_LOADED ]]; then
  bindkey ' '  zeno-auto-snippet
  bindkey '^x' zeno-insert-snippet
  bindkey '^g' zeno-ghq-cd
fi
```

</details>

## おわりに

ここまでいじるとかなり使いやすいものになったのではないかと思います。

自分でカスタマイズしていくと使うのも楽しくなるので、ぜひいろいろ調べてさらに自分好みにしていってください!

また、この記事にミスや改善点などありましたらコメントに書いてくださると助かります。

ここまで読んでくださりありがとうございました!

## 参考記事

https://zenn.dev/nagan/articles/8168ba0ca407ad#%E3%82%82%E3%81%A3%E3%81%A8%E3%82%AB%E3%82%B9%E3%82%BF%E3%83%9E%E3%82%A4%E3%82%BA%E3%81%97%E3%81%9F%E3%81%84

https://qiita.com/mollifier/items/46b080f9a5ca9f29674e

https://zenn.dev/ttskch/articles/fb5a700b37a504#oh-my-zsh%E3%83%97%E3%83%A9%E3%82%B0%E3%82%A4%E3%83%B3%E3%82%92%E3%81%84%E3%81%8F%E3%81%A4%E3%81%8B%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB

https://qiita.com/obake_fe/items/da8f861eed607436b91c

https://amateur-engineer-blog.com/lsd/

https://qiita.com/ryutaro-kimura/items/3d4b263a9ca84091880c

https://dev.classmethod.jp/articles/visual-studio-code-change-terminal/

抜けがありましたら申し訳ございません。
