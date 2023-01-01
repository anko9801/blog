---
title: "フロントエンド開発"
---

### Language
- HTML
- CSS
	- PostCSS では Sass の多くの機能が実現できる。
	- CSSプリプロセッサ
		- [Sass: Syntactically Awesome Style Sheets (sass-lang.com)](https://sass-lang.com/)
		- Less
	- CSSの書き方のルール
		- BEM
		- OOCSS
	- CSS in JS
		- [styled-components](https://styled-components.com/)
			- [styled-componentsの仕組みについての覚え書き | Wantedly Engineer Blog](https://en-jp.wantedly.com/companies/wantedly/post_articles/406209)
		- [vanilla-extract — Zero-runtime Stylesheets-in-TypeScript.](https://vanilla-extract.style/)
		- Twind
	- CSS ファイルの import
		- CSS Modules
	- CSS Framework
		- [Tailwind CSS - Rapidly build modern websites without ever leaving your HTML.](https://tailwindcss.com/)
			- [Tailwind考 - uhyo/blog](https://blog.uhy.ooo/entry/2022-10-01/tailwind/)
		- Windi CSS
	- [Atomic CSS](https://antfu.me/posts/reimagine-atomic-css)
		- UnoCSS
	- React UI
		- [MUI: The React component library you always wanted](https://mui.com/)
- JavaScript / TypeScript
- JSX / TSX

## JavaScript
TypeScript

### Runtime
- Node.js
- [Deno](https://deno.land/)
- [Bun](https://bun.sh/)
- Wasm
	- [WAPM - WebAssembly Package Manager](https://wapm.io/)

### Package Manager
package.json
- [npm](https://www.npmjs.com/)
- [Yarn](https://yarnpkg.com/)
- [pnpm](https://pnpm.io/)
	- Fast, disk space efficient package manager
- [Bun](https://bun.sh/)

### UI Framework
SSR/prerender(SSG)
- Vue
	- reactivity <-> VDOMの変換が挟まってるせいでreactivityの性能を発揮しきれない
	- SFC のカスタムディレクティブがDSLっぽい
	- TypeScript のサポート不足
	- NuxtJS
- React
	- コンポーネント
	- hooks
	- イベントシステムの独自実装
	- 更新されないことが分かりにくい
	- あんまり関係ないけど ReactNative: Android, iOSなどのスマホ用ビルドができる。他のフレームワークでもできそう。
	- Next.js
	- Storybook
	- データフェッチライブラリ SWR, tRPC
- Preact
	- 軽量
	- DOM APIを利用する
	- Reactと互換性がある
	- [Preactで始める軽量コンポーネント指向開発 | 第1回 Preactの特徴 (codegrid.net)](https://www.codegrid.net/articles/2020-preact-1/)
- Svelte
	- コードベースが大きくなった時にノーランタイムの弊害としてバンドルサイズが加速的に大きくなる
	- コンパイル時の変換が独自なのでバンドル速度のボトルネックになりうる
	- [仮想DOMは純粋なオーバーヘッド(Virtual DOM is pure overhead) (svelte.jp)](https://svelte.jp/blog/virtual-dom-is-pure-overhead)
- SolidJS
	- コンパイル時にJSXを特殊な形式に変換するのでesbuildやswcの組み込みのJSX変換を利用できなくて、大きなコードベースになったときにそこがバンドル速度のボトルネックになりうる
- Yew
	- Rust Wasm
	- コードスプリッティングの手法がまだ確立されてないからでかいマルチページなウェブサイトつくると死ぬ

[React ユーザーが Vue を選ばない理由 (zenn.dev)](https://zenn.dev/sa2knight/articles/why_react_folks_dont_choose_vue)

### Ecosystem
- Webpack
- Rollup
- Roam
- Babel
- SWC
- Vite
- ESLint
	- [estools/esquery: ECMAScript AST query library. (github.com)](https://github.com/estools/esquery)

### Production Server
Vercel
- Next.js


### API
- Fetch Upload Streaming
