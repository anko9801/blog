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
	- Rust Wasm
	- コードスプリッティングの手法がまだ確立されてないからでかいマルチページなウェブサイトつくると死ぬ


### Ecosystem
- Webpack
- Rollup
- Roam
- Babel
- SWC
- ESLint
	- [estools/esquery: ECMAScript AST query library. (github.com)](https://github.com/estools/esquery)

### Production Server
Vercel
- Next.js
