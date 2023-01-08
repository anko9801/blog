---
title: "Rust"
---

## 概要
Rust は安全で高速なソフトウェアを作れる言語と言われています。個人の感想としては最も書きやすい言語だと思っています。
安全性を支える技術はライフタイム、所有権など、高速を支える技術はゼロコスト抽象化、LLVMなどです。
主な応用先はCLIツール、WASMビルド、ネットワーク、組み込みデバイスなどです。

## 標準言語からの差分
スタックには Sized な要素のみしか入らない。

### 配列
スタックに入る同一な型の直積型です。つまり要素数は静的定数のみという制約があります。
- `[要素1, ..., 要素n]`
- `[要素; 要素数]` (`Copy` トレイトを実装している場合)
- 個別に初期化する方法
	- `unsafe` 内で `std::mem::MaybeUninit` で配列を確保し、初期化して `std::mem::transmute` で型を変換して取り出す。このマクロは `array-macro` crate で定義されている。
	- 可変変数として `Default::default` で初期化し、後で値を入れる。

### コンテナ
`std` で提供されているヒープに入る直積型の総称をコンテナと言っています。有名なコンテナを列挙すると
- [`std::vec::Vec<T>`](https://doc.rust-lang.org/std/vec/struct.Vec.html)
- [`std::collections::VecDeque<T>`](https://doc.rust-lang.org/std/collections/struct.VecDeque.html)
- [`std::collections::LinkedList<T>`](https://doc.rust-lang.org/std/collections/struct.LinkedList.html)
- [`std::collections::HashMap<T>`](https://doc.rust-lang.org/std/collections/struct.HashMap.html)
- [`std::collections::BTreeMap<T>`](https://doc.rust-lang.org/std/collections/struct.BTreeMap.html)
- [`std::collections::HashSet<T>`](https://doc.rust-lang.org/std/collections/struct.HashSet.html)
- [`std::collections::BTreeSet<T>`](https://doc.rust-lang.org/std/collections/struct.BTreeSet.html)
- [`std::collections::BinaryHeap<T>`](https://doc.rust-lang.org/std/collections/struct.BinaryHeap.html)

### グローバル変数
- 静的に定数として初期化できる `const` `static`
- `lazy_static`, `once_cell`
- Thread Local Storage を使う `thread_local`
