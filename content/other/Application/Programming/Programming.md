---
title: "プログラミング言語"
---

# プログラミング言語入門

任意の実行はデータとアクションで構成されている。
それを人間にとってわかりやすく書きやすい形にするがプログラミング言語が必要とされる理由であり、データとアクションの関係性における多様なデザインがプログラミング言語の多様性の理由である。

データとアクションの関係性はロジックや階層構造によって表現される。
そして計算モデルに意味を与える意味論的な解釈を取り込みつつ解説する。

最初は抽象的で簡単なところから始まり、具体的で難しい問題に立ち向かう。
対象者
- 任意の言語を素早く習得したい方
- 基礎から理解して安心したい方
- 新しい言語を実装したい方

情報に踊らされずちゃんと軸を持つには論理で情報を作り出さないといけない。

## 第一章 型

TODO: なぜここで型を説明するのか
馴染み深いから？前提知識として持っていないと抽象論を展開できないから？あいまいな定義や同義な言葉が各言語にあるのでまとめて扱う為に一回必要

ここでは公理的に計算するところはつまらんので省いて、なるべく実践的、アイデアが得られる点を紹介する。何故かというと私は計算より抽象的なことの方が好きだからです。不足分については型理論の方で説明します。($\lambda, \pi, \mu$ 計算, 線形型, Boehm-Berarducci encodingなど)

### データの表現方法
- 直積型 $A_1\times A_2$
	- ex.) 構造体, メソッドのないクラス, 配列, Vectorなど
- 直和型 $A_1+A_2$
	- ex.) Rustのenum
- ユニオン型 $A_1\cup A_2$
	- 直和型とほぼ同じであるが、反例としてすべての型に null や undefined などがある TypeScript は直和型と同値ではない。
	- ex.) Cのunion
- 篩型 (refinement types) $\{x\in A\mid P(x)\}$
	- データに制約を持たせることができる。
	- 実装を篩型に分け与えてシンプルに出来る
	- ex.) Liquid Haskell

### ロジックの表現方法
- 関数型 $f\colon A_1\to A_2 \to\ldots\to A_n \to R$
	- カリー化
	- optional引数を考えるとカリー化は筋が良くない.
- クラス $(A, f_1,\ldots,f_n)\,\text{s.t.}\,f_i\colon A\to\ldots\to R$

### 型の順序
順序、数でいう不等号を型に与えます。

trait
- $(f)$ $f$ で生成されるtrait
- ex.) Rust: trait, Swift: protocol, extension


部分型
- 関数と型の集合の部分集合として定義できる
- $A_1 \subset A_1\cup A_2$
- $A_1 \subset A_1\times A_2$
- $(f_1, f_2) \subset (f_1)$

部分型の性質
- 共変性 $A_1 \subset A_2 \implies I[A_1] \subset I[A_2]$
- 反変性 $A_1 \subset A_2 \implies I[A_1] \supset I[A_2]$
	- 関数の引数
	- Goの継承とかが分かりやすいかな
	- $A_1\to R \supset A_2\to R$
	- `type X interface {F()}`
	- `type Y interface {F();G()}`
	- YがXのサブタイプ
- 双対性 共変かつ反変
- 非変性 共変でも反変でもない

### 型の構成と分類
- 依存型 $\Pi_{x\colon A}B(x)$
- 依存和型 $\sum_{x\colon A}B(x)$
- 型演算子
	- 多相カインドの抽象化
- 型クラス trait $\{A\in\mathcal{U}\mid f_1,\ldots,f_n \in A\}$
- Row Type

### 型の意味を捉える
- モナド/コモナド
	- 簡潔な状態遷移ができる

型レベル○○
- 型を用いてある代数と同値な型を定義すること
- 型レベル自然数
	- 例えば $A_0 = A$, $A_{n+1}=A_0\times \ldots\times A_n$ と定義すると加算、減算、乗算、順序などを埋め込むことができ、自然数と同型な代数となる。
- 型レベル文字列
	- TypeScript にはもともとある

## 第二章 内部実装
コード解析などで解決する問題はよくNP完全な問題であることが多い。しかし何がNP完全で何がそうではないか区別する技術を持つ人は少ないと思う。
また内部実装を理解すれば、よりよい言語というのが分かってくるだろう。

よりよい言語を知る為だけに必要な内部実装とはどこまでなのか？それには今までのプログラミング言語の歴史なしで考えることはできない。私は低レイヤを抽象化し, ロジックに注目できるように改善されてきたと考えている。その為, ゼロコスト抽象化ができるかどうかの境界からが説明を与えるのに妥当であろう。

### メモリ領域
ユーザーに渡されるメモリはローダによって以下のように分けられている。
- データ領域
- スタック領域
- ヒープ領域
- テキスト領域
- Thread Local Storage

### 関数
関数というかアセンブリ言語そのものを説明するかも。
- 関数
- generator
- 継続
	- Continuation-Passing Style: CPS変換
	- call/cc

### 多相性 (Polymorphism)
具体的に依存型を実装する多相性を紹介する。
- アドホック多相 (ad hoc polymorphism)
	- オーバーロード
- パラメータ多相 (parametric polymorphism)
	- 静的に呼び出された関数の引数の型を解析して自動で実装する。
- サブタイピング多相 (subtyping polymorphism)
	- クラスの継承 オーバーライド
	- vtable

**アドホック多相**
多重定義(オーバーロード)  
オーバーロード  
デフォルト引数  
オーバーライド  
マングリング  
Cの__main__これ辛い
型クラス(Haskell)、トレイト(Rust)、プロトコル(Swift)
プリプロセッサ

**パラメータ多相**
ジェネリクス(総称型)  
ジェネリクス(Java)、テンプレート(C++)、let多相(ML系言語)

これらの実装にはディスパッチを用いる
- 静的ディスパッチ
- 動的ディスパッチ

これ説明するならC++やRustだけではなくHaskellやScalaの内部実装を見ておきたい.

**サブタイピング多相**
- 公称的部分型 (nominal subtyping)
	- Java
- 構造的部分型 (structual subtyping)
	- OCaml, TypeScript

| 方法 |	記述法 | 複数指定 | 関数のオーバライド |
| -------- | -------- | -------- | ------- |
|通常の継承（extends）	|class B extends A {}	|✖	|適宜必要な関数のみ
|abstractを利用した継承（extends）|	class B extends A {}※Aは基本的にabstractクラス|	✖	|全関数必須
|Interfaceとは（implements）|	class B implements A {}	|○	|全関数必須|
|Mixins（with）|	class B with A {}※Aは基本的にmixinクラス|	○|	適宜必要な関数のみ

https://zenn.dev/iwaku/articles/2020-12-16-iwaku

### 最適化 (Optimization)
コンパイラが行う最適化処理は低レイヤに依存するものが多い. その点, アーキテクチャの抽象化とも言える. それよりコードの読みやすさと高速化には相反する場合がよくある.

何を最適化するのか?
最適化の方法

SSA形式に落とし込むとCFGと単純な同値関係になり、グラフ理論を持ち込んでより深い最適化を考えられる。

現代の主要なコンパイラの最適化は巨大となっているが, 最もクリティカルな8つの最適化さえ実装すれば最大80%の性能まで向上する。

- インライン展開
- ループ展開, ベクトル化
	- ループを展開する
	- 利点
		- 分岐が減る
		- 命令レベルの並列化を行える
	- 欠点
		- キャッシュミスの増加
		- ループ内に複雑な制御フローが含まれていると分岐予測が当たりづらくなる
- 共通部分式除去 (CSE; Common Subexpression Elimination)
- デッドコード除去 (DCE; Dead Code Elimination)
	- よく定数畳み込みをすると命令列が"死ぬ"ことがある. 実行時に1度も到達できないコードをデッドコードと呼び, それらは削除できる.
- コード移動
- 定数畳み込み, 定数伝播 (Constant Fold, Constant Propagation)
	- コンパイル時に定数の計算をする
	- 定数伝搬 constexpr, 定数畳み込み consteval
- Peephole最適化

資料
- [CompilerTalkFinal (venge.net)](http://venge.net/graydon/talks/CompilerTalk-2019.pdf)
- [A Catalogue of Optimizing Transformations (rice.edu)](https://www.clear.rice.edu/comp512/Lectures/Papers/1971-allen-catalog.pdf)

JIT (Just In Time Compiler)
- 実行時にコンパイルして最適化を行う.

GC (Gabage Collection)
- 参照カウントが0になったものを自動的に解放する

Lifetime
- mutable reference は Linear type を根拠にしている.
- ダングリングポインタを無くす
- 脆弱性の1つ, Use after free を用いることで様々な exploit ができる為, できる限り無くしたいのは嘘では
- RAII; Resource Acquisition Is Initialization
- smart pointer
- miracle pointer
- [Google Online Security Blog: Use-after-freedom: MiraclePtr (googleblog.com)](https://security.googleblog.com/2022/09/use-after-freedom-miracleptr.html)

### ライブラリ (Library)
下のことを気にしないで開発できる.
- 静的ライブラリ
- 動的ライブラリ
	- glibc


## 第三章 開発体験
思想が混じりやすい話題なので極力様々な意見を取り込むべき

開発体験が良い状態とは, 短期的にも長期的にも最短時間でコードを読み, 書くことができることとする. これには2つの軸で考える.

- わかりやすさ
- フェイルセーフ

これに対して以下のような問題を考えることが出来る.

- Abstraction leak
	- うまく抽象化したつもりでも、どこかに必ず漏れが出てきてしまう
- 幾何的, 編集距離
- バグ

これを解決するには次のような方法がある.

- 階層構造
	- import classの継承
- 情報隠蔽
	- カプセル化 private public
- 依存性注入
- 情報の粗密
- 具体的で正確で簡潔な文章
- 1度に1つの情報, 情報の独立性
- 一貫性
- エコシステム
- パターン
- Immutable

### オブジェクト指向
カプセル化 getter/setter
本来は、値引き判定のロジックをどのオブジェクトに配するかを決めるにあたって、どのような知識を隠蔽すべきか、あるいは裏返して言えば、どのような知識は開示して構わないかという点に思いをめぐらすべきでした。
解決策は、「データとロジックを一体に」という、どちらかというとゲームのルールのような具体的で単純なルールから視点を引き上げ、「情報隠蔽（＝知識隠蔽）」のような、より本質的な、目的志向的な設計原則に立ち帰って考えることです。
何を隠蔽して何を表に出すのかという設計判断
引数に関数を入れることもできる。このような特性を持つ言語を関数型言語と呼ぶ。
Visitorパターン

### 関数型言語
2階で十分なぜか

### Clean Architecture

### エコシステム
- 静的解析
	- LSP (補完, ハイライト, 定義ジャンプ, 型ヒントなど)
	- formatter, linter
	- コード生成
	- 脆弱性スキャン
	- コールグラフ
- 動的解析
	- デバッガ
	- プロファイラ
	- fuzzer
	- Concolic Execution
- テスト
	- CI/CD
- VSCode拡張機能
- パッケージマネージャ
- ランタイム
- FFI
- ライブラリ

## 第四章 Concurrency
ErlangVM

Concurrency
- coroutine
- future
- async/await 並行
	- 裏でnode.jsが並列をしているから
	- node.js が single thread は本当だが嘘
	- event loop 1process
	- Golang event loop 複数のprocess
- Actor
- atomic
	- compare and swap

async/await
async goroutine作ってchannel渡して
await channel待つ
channel の queue はeventloop のqueueと同じ感じ

async iterator
- Arc/Rc

意味論
- happens-before 実行順序
- data race free
- sequentially consistent atomics(素直なatomics)
	- Javaのvolatile, C++のdefault atomics, Goのsync/atomic, JavaScript
