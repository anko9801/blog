---
title: "ツールチェーン"
---

# toolchain

ツールチェーンとはソースコードを実行する一連の処理に必要なソフトウェアのこと。要はどんな開発にも必要なツールセットである。

![[toolchain.svg]]

## コンパイラ

![[compiler.svg]]

- glibc
- [musl libc](https://musl.libc.org/)

- [コンパイル中にコンパイルする「コンパイル時Cコンパイラ」をつくった話 - kw-udonの日記 (hatenablog.com)](https://kw-udon.hatenablog.com/entry/2016/12/03/201722)
- [keiichiw/constexpr-8cc: Compile-time C Compiler implemented as C++14 constant expressions (github.com)](https://github.com/keiichiw/constexpr-8cc) 
- [コンパイル時Brainfuckコンパイラ ――C++14 constexpr の進歩と限界―― - ボレロ村上 - ENiyGmaA Code (hateblo.jp)](https://boleros.hateblo.jp/entry/2014/12/24/065155)
- [go/compile.go at master · golang/go (github.com)](https://github.com/golang/go/blob/master/src/cmd/compile/internal/ssa/compile.go#L331)

## disassembler

## decompiler

バイナリを入れてLLVM IR に変換後, 最適化する
[facebookincubator/BOLT: Binary Optimization and Layout Tool - A linux command-line utility used for optimizing performance of binaries (github.com)](https://github.com/facebookincubator/BOLT)
