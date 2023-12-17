---
title: 型ファイルが邪魔じゃない。React開発者のための型安全CssModules環境構築
tags:
  - CSS
  - TypeScript
  - React
  - css-modules
private: false
updated_at: '2023-11-29T20:22:21+09:00'
id: 356ed165c40daba987e3
organization_url_name: null
slide: false
ignorePublish: false
---
# はじめに
Reactを用いた開発の際、CSSをどうするかは意見が分かれるところだと思います。
今回はそのなかでもCSS Modulesを用いて型安全な開発環境を構築する方法を紹介したいと思います。

## 前提環境
この記事を書いた人の環境は以下です。
React : 18.2.0
Typescript : 4.9.5
node : 18.17.1

Dockerで動かしていますが、使っていなくても大丈夫です。

## この記事で作れる環境
- 型ファイルを普段目につかない場所に置ける(自分の中では最重要)
- cssファイルを編集した瞬間型ファイルに反映
- 存在しないクラス名の時はエラーを出してくれる
![スクリーンショット 2023-11-27 22.21.15.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/90a6f6ba-a343-20c1-b9d0-fa0c97e89229.png)
- インポートしてあるCSSファイルのクラス名を早い段階でサジェストに出すことができる
![スクリーンショット 2023-11-27 22.22.12.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2778030/adf01076-f418-5b41-f80e-cba2672f08f9.png)

などです。
また、CSSModulesそのもののメリットとして以下のものがあります、
- 学習コストが低い
- 過去の`.css`ファイルをほぼそのままで使える
- クラス名を一意に生成することにより、クラス名の衝突が防げる(importしていないCSSファイルのクラスは適用されない)

しかし一概にcss-modulesがCSSを扱う上での最適解というわけではなく、特にReactにおいてはCSSの選択は意見が分かれるところだと思うのでこのぐらいにしておきます。では本題の環境構築の方法に進みます。

## 環境構築
Typescript入りのReactは動いている前提で話を進めます。

:::note warn
この作業をする時はブランチを切って進めていくことを強く勧めます。(環境を大きくいじる場合があるので)
:::

<details>
<summary>最終的なディレクトリ構造はこちらです</summary>

```text
.
├── dist // 型ファイルがここに生成されます
│   ├── index.css.d.ts
│   └── components
│       ├── card
│       │   └── card.css.d.ts
│       └── list
│           └── list.css.d.ts
├── src
│   ├── index.tsx
│   ├── index.module.css
│   └── components
│       ├── card
│       │   ├── card.tsx
│       │   └── card.module.css
│       └── list
│           ├── list.tsx
│           └── list.module.css
└── tsconfig.json
```
</details>

### 1. `typed-css-modules`をプロジェクトに追加
クラス名を型ファイルに書き出してくれる神パッケージです。
[こちらの手順](https://github.com/Quramy/typed-css-modules)に従ってインストールします。
```
npm install typed-css-modules
npm install -g typed-css-modules
```
順に実行します。

### 2.`tsconfig.json`を編集
必要ない行は省略してます。
```json:tsconfig.json
{
    "compilerOptions": {
        "baseUrl": ".",
        "paths": {
            "src/*": ["./dist/src/*", "./src/*"],
        }
    },
    "include": ["src","dist/**/*"],
}
```
<details>
<summary>全文</summary>

```json
{
    "compilerOptions": {
        "target": "es5",
        "baseUrl": ".",
        "lib": ["dom", "dom.iterable", "esnext"],
        "allowJs": true,
        "skipLibCheck": true,
        "esModuleInterop": true,
        "allowSyntheticDefaultImports": true,
        "strict": true,
        "forceConsistentCasingInFileNames": true,
        "noFallthroughCasesInSwitch": true,
        "module": "esnext",
        "moduleResolution": "node",
        "resolveJsonModule": true,
        "isolatedModules": true,
        "noEmit": true,
        "jsx": "react-jsx",
        "typeRoots": ["types"],
        "downlevelIteration": true,
        "paths": {
            "src/*": ["./dist/src/*", "./src/*"]
        }
    },
    "include": ["src", "types", "dist/**/*"],
    "exclude": ["node_modules"]
}

```
`tsconfig`未だによくわかってないので、意味不明な部分あれば教えてください🙇🏻‍♂️
</details>

肝は以下の部分で、
```json
"paths": {
    "src/*": ["./dist/src/*", "./src/*"],
}
```
`import styles from "src/myComponent.module.css";`とした時に、まず`"./dist/src/*"`を見に行ってくれます。型情報を参照しつつ、`import`エラーも出なくなります。このおかげで型ファイルを普段見えない場所に置いておくことができます。

### 3. `package.json`を編集
`package.json`の`scripts`の部分に追記します。
```json:package.json
"scripts": {
    "tcm": "npx tcm -p src/**/*.module.css -o dist -w"
}
```
説明します。これは先ほどインストールした`typed-css-modules`を実行するコマンドです。
`-p src/**/*.module.css`で監視するファイルを`src`以下の`.module.css`に限定します。
`-o dist`で型ファイルをアウトプットする先を指定しています。`dist`部分は好きに変えて問題ないですが、合わせて`tsconfig.json`も変更する必要があることに注意してください。
`-w`でcssファイルの変更を監視して、自動で型ファイルを変更に合わせて書き換えてくれます。

### 4.コマンドを実行

上の作業を終えた後`、npm run tcm`で実行すると、プロジェクトのルートディレクトリに`dist`フォルダが作成され、`src`以下に対応したディレクトリに型ファイルが生成されます。

ローカルで開発している方は新規ターミナルを立ち上げて上記のコマンドを打ってください。
Dockerを使用している方はコンテナの立ち上げに合わせて`npm run tcm`を実行させるか、
`start`の部分を`"start": "npx tcm -p src/**/*.module.css -o dist -w & react-scripts start",`のように記述しても大丈夫です。

### 5.実際につかってみる

任意のファイルで`module.css`ファイルを作って、インポートしてみます。
```react:src/MyComponent.tsx
import styles from "src/myComponent.module.css";

function MyComponent() {
    return <div className={styles.text_highlight_test}>UItest</div>;
}

export default MyComponent;
```
```css:src/myComponent.module.css
.text_h1_bold_test {
    font-weight: bold;
    font-size: 30px;
}

.text_h2_test {
    font-size: 25px;
}

.text_highlight_test {
    color: #0048ff;
    font-weight: bold;
}
```
[この記事で作れる環境](https://qiita.com/hikagami/private/356ed165c40daba987e3#%E3%81%93%E3%81%AE%E8%A8%98%E4%BA%8B%E3%81%A7%E4%BD%9C%E3%82%8C%E3%82%8B%E7%92%B0%E5%A2%83)で列記した通りになっていれば環境構築は終了です！
お疲れ様でした。

うまく動かない場合、chatGPTに聞くと解決できるかもしれません。(無責任)
コメントにエラー文などを書いていただけると手助けができるかもしれません。
特に`tsconfig`の`baseUrl`を変更した場合、既存のインポート文が使えなくなっている事があり、エラーになっている`import`文を書き直す必要があります。

## おわりに
最後まで読んでいただきありがとうございました!
css modulesを型安全に扱う上で型ファイルが邪魔すぎたので、それを解消できたのは大きいなと思っています。

もっと簡単に[この記事で作れる環境](https://qiita.com/hikagami/private/356ed165c40daba987e3#%E3%81%93%E3%81%AE%E8%A8%98%E4%BA%8B%E3%81%A7%E4%BD%9C%E3%82%8C%E3%82%8B%E7%92%B0%E5%A2%83)で列記した要素を満たせる方法がある場合教えていただけると嬉しいです!
また、本文中で不適切な部分がある場合、もっといいやり方がある場合も教えていただけると助かります🙇🏻‍♂️
