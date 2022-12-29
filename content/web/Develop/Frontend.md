---
title: "フロントエンド開発"
---

### Language
- HTML
- CSS
	- [Sass: Syntactically Awesome Style Sheets (sass-lang.com)](https://sass-lang.com/)
		- PostCSS では Sass の多くの機能が実現できる。
	- CSS Modules
	- UnoCSS
	- [Tailwind CSS - Rapidly build modern websites without ever leaving your HTML.](https://tailwindcss.com/)
	- [MUI: The React component library you always wanted](https://mui.com/)
	- styled-components
	- Windi CSS
	- Twind
- JavaScript / TypeScript
- JSX / TSX
	- [vanilla-extract — Zero-runtime Stylesheets-in-TypeScript.](https://vanilla-extract.style/)

[Tailwind考 - uhyo/blog](https://blog.uhy.ooo/entry/2022-10-01/tailwind/)
[styled-componentsの仕組みについての覚え書き | Wantedly Engineer Blog](https://en-jp.wantedly.com/companies/wantedly/post_articles/406209)

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
	- NuxtJS
- React
	- コンポーネント
	- hooks
	- イベントシステムの独自実装
	- 更新されないことが分かりにくい
	- Next.js
	- ReactNative: Android, iOSなどのスマホ用ビルド
- Preact
	- 軽量
	- DOM APIを利用する
	- Reactと互換性がある
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
