---
title: "Rust"
---

こ言語

ライフタイムとLLVM

スタックには Sized な要素のみしか入らない。

## 配列
- `[要素1, ..., 要素n]`
- `[要素; 要素数]` (`Copy` トレイトを実装していて要素数が静的定数である場合)
- 個別に初期化する方法
	- `unsafe` 内で `std::mem::MaybeUninit` で配列を確保し、初期化して `std::mem::transmute` で型を変換して取り出す。このマクロは `array-macro` crate で定義されている。
	- 可変変数として `Default::default` で初期化し、後で値を入れる。
 