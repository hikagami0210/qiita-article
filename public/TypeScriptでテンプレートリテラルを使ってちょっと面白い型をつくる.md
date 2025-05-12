---
title: TypeScriptでテンプレートリテラルを使ってちょっと面白い型をつくる
tags:
  - TypeScript
private: false
updated_at: '2025-05-11T22:39:28+09:00'
id: 382f0a64f90b0df33eed
organization_url_name: null
slide: false
ignorePublish: false
---

今回は TypeScript でテンプレートリテラルを使ってちょっと面白い型をつくってみます。
実用性とか度外視で、テンプレートリテラルはこういうこともできるんだなあと思って思いついたものを紹介します。

### 4 桁の数字の型

```typescript
type Digit = '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9';
type FourDigits = `${Digit}${Digit}${Digit}${Digit}`;

const digits: FourDigits = '1234'; // OK
const digits2: FourDigits = '12345'; // NG
```

### WEB サイトの URL 型

```typescript
type UrlPrefix = 'http' | 'https';

type Url = `${UrlPrefix}://${string}`;

const url: Url = 'https://example.com/path/to/resource'; // OK
const url2: Url = 'http://example.com/path/to/resource'; // OK
const url3: Url = 'ftp://example.com/path/to/resource'; // NG
```

### セマンティックバージョン

```typescript
type Digit = '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9';

type Major = Digit;
type Minor = Digit;
type Patch = Digit;

type SemanticVersion = `${Major}.${Minor}.${Patch}`;

const version: SemanticVersion = '1.2.3'; // OK
const version2: SemanticVersion = '1.2.3.4'; // NG
```

### CSS 単位

```typescript
type CssUnit = `${number}px` | `${number}%` | `${number}em` | `${number}rem`;

const cssUnit: CssUnit = '100px'; // OK
const cssUnit2: CssUnit = '100%'; // OK
const cssUnit3: CssUnit = '100'; // NG
```

### トランプの札

```typescript
type Num = '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9' | '10';
type Suit = 'S' | 'H' | 'D' | 'C';
type CardNo = 'A' | 'K' | 'Q' | 'J' | `${Num}`;
type PlayingCard = `${Suit}${CardNo}`;

const card: PlayingCard = 'S10'; // OK
const card2: PlayingCard = 'S100'; // NG
```

## おわり

ほかに何か面白いもの知ってるよって方はぜひ教えてください！
