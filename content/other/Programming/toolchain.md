---
title: "ツールチェーン"
---

# toolchain

ツールチェーンとはソースコードを実行する一連の処理に必要なソフトウェアのこと。要はどんな開発にも必要なツールセットである。

![[toolchain.svg]]

## コンパイラ

![[compiler.svg]]
- ソースコードの解析
	- 字句解析
		- 文字列をトークン列に変換。
	- プリプロセス
		- マクロやヘッダファイルの読み込みなどの前処理を行う。
	- 構文解析
		- トークン列を抽象構文木に変換。
	- 意味解析
		- 静的意味の解析。変数のスコープや型検査・型推論、制御の流れの検査を行う。
- 中間表現での最適化
	- クロージャ変換
		- 自由変数を束縛させながら関数を平坦化する
	- 中間コード生成
		- SSA形式の命令列に変換する
	- 最適化
		- 8つの最適化を回す
- コード生成
	- レジスタ割り当て
		- 変数をレジスタに割り当てる
	- コード生成
		- アセンブリやバイナリに変換する

### ソースコードの解析
eBNF


## 参考

- glibc
- [musl libc](https://musl.libc.org/)
- [コンパイル中にコンパイルする「コンパイル時Cコンパイラ」をつくった話 - kw-udonの日記 (hatenablog.com)](https://kw-udon.hatenablog.com/entry/2016/12/03/201722)
- [keiichiw/constexpr-8cc: Compile-time C Compiler implemented as C++14 constant expressions (github.com)](https://github.com/keiichiw/constexpr-8cc) 
- [コンパイル時Brainfuckコンパイラ ――C++14 constexpr の進歩と限界―― - ボレロ村上 - ENiyGmaA Code (hateblo.jp)](https://boleros.hateblo.jp/entry/2014/12/24/065155)
- [go/compile.go at master · golang/go (github.com)](https://github.com/golang/go/blob/master/src/cmd/compile/internal/ssa/compile.go#L331)

## disassembler

## decompiler

バイナリを入れてLLVM IR に変換後, 最適化する
- [facebookincubator/BOLT: Binary Optimization and Layout Tool - A linux command-line utility used for optimizing performance of binaries (github.com)](https://github.com/facebookincubator/BOLT)
