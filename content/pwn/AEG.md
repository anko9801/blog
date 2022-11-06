---
title: "Automatic Exploit Generation"
---

## 説明
自動的にexploitを行うコードを生成する.
バグを見つけるパートとexploitを生成するパートがある.
バグを見つけるときはシンボリック実行エンジンを使う. ただ実行するだけだと実行時間がかなり長くなってしまう. これを効率的に行う為には前条件を用いるとよい.
前条件に使われるものは次のようなものがある.
- Known Length: BOF検知
- Known Prefix: HTTPヘッダ
- Concolic Execution

疑問
ライブラリの定理証明をしていった方が良いのでは

## 参考文献
[AEG: Automatic Exploit Generation (psu.edu)](https://www.cse.psu.edu/~trj1/cse544-f15/docs/aeg-current.pdf)