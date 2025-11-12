# sass-responsive-util
[![npm version](https://img.shields.io/npm/v/sass-responsive-util.svg)](https://www.npmjs.com/package/sass-responsive-util)
[![license](https://img.shields.io/github/license/minori-003/sass-responsive-util)](./LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/minori-003/sass-responsive-util)](https://github.com/minori-003/sass-responsive-util)

> A modern Sass utility library for responsive web design, supporting px, pt, rem, vw, and clamp() conversions.

レスポンシブWebデザインとデザインシステム構築を強力にサポートする、モダンなSassモジュール群です。`px`, `pt`単位の値を、`rem`, `vw`, `em`, `clamp()`といったレスポンシブ対応のCSS単位へ簡単に変換できます。

## ✨ 特徴

- **モダンなSassモジュール構成:** `@use`および`@forward`ルールに基づいた、シンプルで使いやすいモジュール構造。
- **pt対応:** DTPやデザインツール（Canva, Figmaなど）で一般的な**ポイント（pt）**単位を`px`や`rem`に変換する関数を標準装備。
- **Fluid Type/Spacingの実現:** CSSの`clamp()`関数を`rem`基準で簡単に生成できる`r-clamp()`を提供。
- **VW変換:** PC/SPそれぞれの基準ビューポート幅を元にした`vw`変換関数を提供。

## 📥 インストールとセットアップ

### 📦 インストール

```bash
npm install sass-responsive-util
```

### Gitでクローンする場合

```bash
git clone https://github.com/minori-003/sass-responsive-util.git
```




## 📂 ファイル構成
<pre>
sass-responsive-util/
├─ _index.scss <- エントリポイント
│
└global/
   ├─ _index.scss <- エントリポイント
   │
   ├── setting/
   │       ├── _index.scss <- エントリポイント
   │       └── _variables.scss <- 変数定義
   │
   └── functions/
            ├── _index.scss <- エントリポイント
            ├── _unit-helpers.scss <- ヘルパー
            ├── _px-conversions.scss // px変換関連
            ├── _pt-conversions.scss // pt変換関連
            ├── _viewport-conversions.scss // vw変換関連
            ├── _local-conversions.scss // emや%変換関連
            └── _fluid-type.scss // Fluid Typography
</pre>

## 使用方法 (Usage)

プロジェクトのSCSSファイル内で、`_index.scss`を`@use`ディレクティブでインポートしてください。

### 基本的なインポート

ライブラリの**変数設定を変更したい場合**は、必ず他のモジュールよりも前に`setting`モジュールを`@use`し、`with`キーワードで変数を設定してください。

```scss
// 変数をカスタマイズする場合（推奨）
// 必ず他のsass-responsive-utilの@useよりも前に記述してください。
@use "sass-responsive-util/setting" with (
  $root-font-size: 10,
  $default-dpi: 72
);

// その後、functionsモジュールをインポートし、'fn'という名前空間で利用します
@use "sass-responsive-util/functions" as fn;
```

**変数をカスタマイズせず、デフォルト値のままで良い場合**、または**すべての公開モジュールをまとめて利用したい場合は**、以下の方法でインポートできます。

```scss
// 変数をカスタマイズしない場合、またはfunctionsモジュールのみを利用する場合
@use "sass-responsive-util/functions" as fn;

// すべての公開モジュールをまとめて利用する場合（設定、関数群など）
// 'sru'という名前空間で利用します。
@use "sass-responsive-util" as sru;
```

### 💡 サンプルコード

| 関数 | 目的 | SCSS例 |　CSS出力例 |
| --- | --- | --- | --- |
| `px-to-rem()` | `px`を`rem`に変換 | `font-size: px-to-rem(24);` | `font-size: 1.5rem;` |
| `r-clamp()` | `px`に基づき`clamp()`を生成 | `font-size: r-clamp(16, 32);` | `font-size: clamp(1rem, calc(0.85rem + 0.52vw), 2rem);` |
| `r-clamp-pt()` | `pt`に基づき`clamp()`を生成 | `font-size: r-clamp-pt(12pt, 24pt);` | `font-size: clamp(1rem, calc(0.85rem + 0.52vw), 2rem);` |
| `pt-to-px()` | `pt`を`px`に変換 | `margin-top: pt-to-px(12pt);` | `margin-top: 16px;` |
| `to-em()` | 相対サイズを`em`に変換 | `padding-top: to-em(24px, 16px);` | `padding-top: 1.5em`; |
| `px-to-vw-sp()` | スマホ用`vw`を生成 | `width: px-to-vw-sp(300);` | `width: 80vw;` |


### ⚙️ 変数（設定）

`_variables.scss`で定義されている以下の変数を変更することで、ライブラリ全体の動作を変更できます。
**これらの変数を変更するには、他の`sass-responsive-util`モジュールの`@use`より前に、`setting`モジュールを`with`キーワードを使ってインポートする必要があります。**

| 変数名 | デフォルト値 | 説明 |
| --- | --- | --- |
| `$root-font-size` | `16` | `rem`変換の基準となるルートのフォントサイズ。 |
| `$default-min-bp` | `375` | `r-clamp()`や`vw-sp`計算の最小ビューポート幅（px）。 |
| `$default-max-bp` | `1440` | `r-clamp()`や`vw-pc`計算の最大ビューポート幅（px）。 |
| `$default-dpi` | `96` | `pt`から`px`への変換レート（**1pt = $dpi / 72 px**）。デザインツールの設定に合わせて変更してください。 |

**変更例**
```scss
// `setting`モジュールをインポートし、`with`キーワードで変数を設定します。
@use "sass-responsive-util/setting" with (
  $root-font-size: 10,       // 基準フォントサイズを10pxに変更
  $default-dpi: 72          // pt換算基準を72dpiに変更
);

// その後、関数など他のモジュールを利用します
@use "sass-responsive-util/functions" as fn;

.my-element {
  font-size: fn.px-to-rem(20); // $root-font-size: 10px のため、2rem となる
}
```

### 📒 APIリファレンス
すべての関数は`_index.scss`を通して利用可能です。

+ `px-to-rem($px, $baseFontSize: $root-font-size)`: ピクセル値をremに変換します。

+ `pt-to-px($pt)`: ポイント値をピクセル単位（px）に変換します。

+ `pt-to-rem($pt, $baseFontSize: $root-font-size)`: ポイント値をremに変換します。

+ `r-clamp($min, $max, $minViewport, $maxViewport, $baseFontSize)`: `px`を基にしたレスポンシブな`clamp()`文字列を生成します。

+ `r-clamp-pt($minPt, $maxPt, $minViewport, $maxViewport, $baseFontSize)`: `pt`を基にしたレスポンシブな`clamp()`文字列を生成します。

+ `px-to-vw-sp($px, $minViewport: $default-min-bp)`: スマートフォン用のビューポート幅を基準とした`vw`値を返します。

+ `pt-to-vw-sp($pt, $minViewport: $default-min-bp)`: ポイント値をスマートフォン用の`vw`値に変換します。

+ `to-em($target-size, $context-size)`: ターゲットサイズをコンテキストサイズに基づいて`em`に変換します（`px`, `pt`に対応）。

+ `to-percent($target-size, $context-size)`: ターゲットサイズをコンテキストサイズに基づいて`%`に変換します（`px`, `pt`に対応）。

## 🪪 ライセンス

このプロジェクトは [MITライセンス](./LICENSE) の下で公開されています。
