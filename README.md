
# 🧩 sass-responsive-util

[![npm version](https://img.shields.io/npm/v/sass-responsive-util.svg)](https://www.npmjs.com/package/sass-responsive-util)
[![license](https://img.shields.io/github/license/minori-003/sass-responsive-util)](./LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/minori-003/sass-responsive-util)](https://github.com/minori-003/sass-responsive-util)
![Sass Compatibility](https://img.shields.io/badge/Sass-%40use%20/%20%40forward-green)
[![Docs](https://img.shields.io/badge/docs-SassDoc-ff69b4.svg)](https://github.com/minori-003/sass-responsive-util)

> **sass-responsive-util** is a modern Sass utility library for responsive web design.  
> It provides DPI-aware conversions between `px`, `pt`, `rem`, `vw`, and `clamp()` to simplify scaling across devices and design tools.  
>
> **sass-responsive-util**は、レスポンシブWebデザインとデザインシステム構築を強力に支援するモダンなSassユーティリティ集です。  
> `px`・`pt`などの値を`rem`・`vw`・`clamp()`などへ変換し、**DPI差によるズレを解消**します。

---

## ✨ Features / 特徴

- **Modern module structure:** Built with `@use` and `@forward` for clean namespace-based imports.
- **DPI-aware `pt` conversion:** Perfect for Canva, Illustrator, and print design workflows.
- **Fluid Typography support:** `r-clamp()` simplifies creating responsive `clamp()` values.
- **Viewport-based scaling:** Generate `vw` values for both PC and mobile breakpoints.

> ⚠️ **Important:** This library **does not support LibSass / Node Sass**.  
> Please use **Dart Sass (>=1.56.0)**.

---

## ⚠️ Breaking Changes in v2.0 / v2.0での破壊的変更

> [!IMPORTANT]
> **`px-to-vw-pc()` の挙動が変更されました**
> - **v1.x**: デフォルトで上限付き（px値を上回らない）の値を返却していました。
> - **v2.0**: **上限なし**（純粋なvw変換値）を返却するようになりました。
> 
> 以前の「上限付き」の挙動（PCで一定以上の幅になった際に固定する挙動）を維持したい場合は、新しく導入された **`px-to-vw-pc-limited()`** を使用してください。

---

## 📦 Installation / インストール

### npm

```bash
npm install sass-responsive-util
```

or clone via Git
```bash
git clone https://github.com/minori-003/sass-responsive-util.git
```

## 📂 File Structure / ファイル構成

```pgsql
sass-responsive-util/
├ package.json
├ README.md
├ LICENSE
├ _index.scss              # Entry point
│
├ setting/
│   ├ _index.scss          # Entry point
│   └ _variables.scss      # Global variables
│
├ functions/
│   ├ _index.scss          # Entry point
│   ├ _unit-helpers.scss
│   ├ _px-conversions.scss
│   ├ _pt-conversions.scss
│   ├ _viewport-conversions.scss
│   ├ _local-conversions.scss
│   └ _fluid-type.scss
│
└ mixin/
   ├ _index.scss          # Entry point
   ├ _font-space-block.scss
   ├ _font-space-line.scss
   ├ _apply-r-clamp.scss
   ├ _font-size-r-clamp.scss
   └ _width-size-r-clamp.scss
```

## 📘 Usage / 使用方法

### Precautions / 注意事項
> **Note for Sass CLI users:** If you are compiling directly with the Sass CLI, you may need to add the `--load-path=node_modules` option to your command to help the compiler find the package.
> 
> **Sass CLIをお使いの方へ:** コマンドラインで直接コンパイルする場合、`--load-path=node_modules` オプションを追加して、コンパイラがパッケージを見つけられるようにする必要がある場合があります。
>
> ```bash
> npx sass <your-input-file>.scss <your-output-file>.css --load-path=node_modules
> ```
> **Note:** If you're using build tools like Vite, Webpack, or Gulp,
> you can set `includePaths: ["node_modules"]` in your Sass configuration instead.
>
> **補足:** Vite・Webpack・Gulpなどを使用している場合は、
> Sass設定で `includePaths: ["node_modules"]` を追加することで同様の動作になります。


### 1️⃣ Customize settings (recommended)

```scss
@use "sass-responsive-util/setting" with (
  $root-font-size: 10,
  $default-dpi: 72
);
@use "sass-responsive-util/functions" as fn;

.my-element {
  font-size: fn.px-to-rem(20); // => 2rem
}
```

### 2️⃣ Use with default settings

```scss
@use "sass-responsive-util/functions" as fn;
```

### 3️⃣ Import all modules at once

```scss
@use "sass-responsive-util" as sru;

.title {
  font-size: sru.px-to-rem(24);
}
```

## 💡 Example Functions / 使用例

| `px-to-rem()` | `px`を`rem`に変換 | `font-size: px-to-rem(24);` | `font-size: 1.5rem;` |
| `rem-to-px()` | `rem`を`px`に変換 | `font-size: rem-to-px(1.5rem);` | `font-size: 24px;` |
| `r-clamp()` | `px`に基づく`clamp()`生成 | `font-size: r-clamp(16, 32);` | `font-size: clamp(1rem, ..., 2rem);` |
| `px-to-vw-pc()` | PC用`vw`生成（上限なし） | `width: px-to-vw-pc(32);` | `width: 2.2222vw;` |
| `px-to-vw-pc-limited()` | 上限付きPC用`vw`生成 | `width: px-to-vw-pc-limited(1200);` | `width: min(1200px, 83.3333vw);` |
| `px-to-vw-sp()` | スマホ用`vw`を生成 | `width: px-to-vw-sp(300);` | `width: 80vw;` |
| `pt-to-px()` | `pt`を`px`に変換 | `margin-top: pt-to-px(12pt);` | `margin-top: 16px;` |
| `to-em()` | 相対サイズを`em`に変換 | `padding-top: to-em(24px, 16px);` | `padding-top: 1.5em;` |

## 🎨 Mixin Examples / Mixin使用例

```scss
@use "sass-responsive-util/mixin" as mixin;
.text {
  @include mixin.m-font-space-block(8px, 16px);
  // => line-height: 2;
}

.heading {
  @include mixin.m-font-space-line(8px, 16px);
  // =>  letter-spacing: 0.5em;
}

.title {
  @include mixin.font-size-r-clamp(16px, 32px);
  // => font-size: clamp(1rem, ..., 2rem);
}

.box {
  @include mixin.width-size-r-clamp(100px, 200px);
  // => width: clamp(6.25rem, ..., 12.5rem);
}
```

## ⚙️ Variables / 設定変数

| 変数名 | デフォルト値 | 説明 |
| --- | --- | --- |
| `$root-font-size` | `16` | `rem`変換の基準フォントサイズ |
| `$default-min-bp` | `375` | 最小ビューポート幅（px） |
| `$default-max-bp` | `1440` | 最大ビューポート幅（px） |
| `$default-dpi` | `96` | `pt`から`px`への変換比率（Web標準は96。デザイナーがCanvaやIllustrator（72dpi）を使用している場合は`72`に設定すると数値が一致します） |

### 例：設定変更

```scss
@use "sass-responsive-util/setting" with (
  $root-font-size: 10,
  $default-dpi: 72
);
@use "sass-responsive-util/functions" as fn;

.my-element {
  font-size: fn.px-to-rem(20); // => 2rem
}
```

## 📒 API Reference

| 関数名、mixin名 | 説明 |
| --- | --- |
| `px-to-rem($px, $baseFontSize: $root-font-size)` | pxをremに変換します。|
| `rem-to-px($rem, $baseFontSize: $root-font-size)` | remをpxに変換します。|
| `pt-to-px($pt)` | ptをpxに変換します。 |
| `pt-to-rem($pt, $baseFontSize: $root-font-size)` | ptをremに変換します。 |
| `rem-to-pt($rem, $baseFontSize: $root-font-size)` | remをptに変換します。 |
| `r-clamp($min, $max, ...)` | pxに基づくレスポンシブclamp()生成。最小/最大値は自動でソートされます。 |
| `r-clamp-pt($minPt, $maxPt, ...)` | ptに基づくレスポンシブclamp()生成。 |
| `r-clamp-rem($minRem, $maxRem, ...)` | remに基づくレスポンシブclamp()生成。 |
| `r-clamp-em($minEm, $maxEm, $minViewport, $maxViewport, $baseFontSize, $contextFontSize)` | emに基づくレスポンシブclamp()生成。 |
| `px-to-vw-sp($px, ...)` | スマホ幅基準のvw値を生成。 |
| `px-to-vw-pc($px, ...)` | PC幅基準のvw値を生成。 |
| `px-to-vw-pc-limited($px, ...)` | 上限付きPC用vw値を生成（px値を上回らない）。 |
| `pt-to-vw-sp($pt, ...)` | pt値をスマホ用のvw値に変換。 |
| `pt-to-vw-pc($pt, ...)` | pt値をPC用のvw値に変換。 |
| `pt-to-vw-pc-limited($pt, ...)` | pt値を上限付きPC用vw値に変換。 |
| `rem-to-vw-sp($rem, ...)` | rem値をスマホ用のvw値に変換。 |
| `rem-to-vw-pc($rem, ...)` | rem値をPC用のvw値に変換。 |
| `rem-to-vw-pc-limited($rem, ...)` | rem値を上限付きPC用vw値に変換。 |
| `to-em($target-size, $context-size)` | 相対サイズをemに変換。 |
| `to-percent($target-size, $context-size)` | 相対サイズを%に変換。 |
| `@mixin apply-r-clamp($property, $min, $max)` | 任意のプロパティにr-clampを適用。単位(px, pt, rem)を自動判別します。 |
| `@mixin font-size-r-clamp` | font-sizeにr-clampを適用するmixin |
| `@mixin width-size-r-clamp` | widthにr-clampを適用するmixin |
| `@mixin m-font-space-block` | 文字の上下につけたい余白からline-heightを算出する |
| `@mixin m-font-space-line` | 文字の横につけたい余白からletter-spacingを算出する |

## 🧠 Quick Example

```scss
@use "sass-responsive-util" as sru;

.title {
  font-size: sru.px-to-rem(24);
  margin-top: sru.pt-to-px(12pt);
  @include sru.m-font-space-block(8px, 16px);
}
```

## 🪪 License

Released under the [MIT License](./LICENSE).