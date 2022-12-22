### Language
- HTML
- CSS
	- [Sass: Syntactically Awesome Style Sheets (sass-lang.com)](https://sass-lang.com/)
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

### Runtime
- Node.js
- [Deno](https://deno.land/)

### Package Manager
種類
- [npm](https://www.npmjs.com/)
- [Yarn](https://yarnpkg.com/)
- [pnpm](https://pnpm.io/)
	- Fast, disk space efficient package manager

package.json

### UI Framework
SSR
- Vue
	- reactivity <-> VDOMの変換が挟まってるせいでreactivityの性能を発揮しきれない
	- Nuxt
- React
	- コンポーネント
	- hooks
	- イベントシステムの独自実装
	- 更新されないことが分かりにくい
	- Next
	- ReactNative: Android, iOSなどの
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

### Ecosystem
- Webpack
- Rollup
- ESLint
	- [estools/esquery: ECMAScript AST query library. (github.com)](https://github.com/estools/esquery)


Wasm
- [WAPM - WebAssembly Package Manager](https://wapm.io/)
- コードスプリッティングの手法がまだ確立されてないからでかいマルチページなウェブサイトつくると死ぬ

