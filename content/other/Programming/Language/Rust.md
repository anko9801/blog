


スタックには Sized な要素のみしか入らない。

## 配列
- `[要素1, ..., 要素n]`
- `[要素; 要素数]` (`Copy` トレイトを実装している場合)
- 個別に初期化したい場合
	- `unsafe` 内で `std::mem::MaybeUninit` で配列を確保し、初期化して `std::mem::transmute` で型を変換して取り出す。`array-macro` crate がある。
	- 可変変数として `Default::default` で初期化し、後で値を入れる。
 