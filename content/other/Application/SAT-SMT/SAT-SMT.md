---
title: "SAT SMT"
---

## SAT
SAT (SATisfiability Problem)
SATを解くには指数時間掛かると信じられている. 指数時間の中でも高速化していく技術を学ぶ.

$((a\land\lnot b\land\lnot c)\lor(b\land c\land\lnot d))\land(\lnot b\lor\lnot c) \to (a,b,c,d)=(t,f,f,t)$

### 単純な探索

まずは SAT に関する全探索を考える. 以下の方法は DPLL (Davis Putnam Logemann Loveland) アルゴリズムと呼ばれている.
リテラルが1つしかない節を単位節と呼ぶ.

1. 単位節があればその変数の値は確定する.
2. それがなければ変数のどれか1つを深さ優先探索する.
3. コンフリクトしたなら失敗, コンフリクトなしに変数を全て割り当てられたら成功とする.

このとき深さ優先探索での深さが $d$ のときをレベル $d$ と呼ぶ.

$$
\begin{aligned}
\phi &= (a ∨ ¬b ∨ d) ∧ (a ∨ ¬b ∨ e) \\
&∧ (¬b ∨ ¬d ∨ ¬e) \\
&∧ (a ∨ b ∨ c ∨ d) ∧ (a ∨ b ∨ c ∨ ¬d) \\
&∧ (a ∨ b ∨ ¬c ∨ e) ∧ (a ∨ b ∨ ¬c ∨ ¬e)
\end{aligned}
$$
ミュンヘン工科大学の資料より


### 節学習

CDCL (Constrait-Driven Clause Learning)
DPLL に加え, コンフリクトしたときにその探索状態だと失敗することを条件に入れる.

| $t$ | $L_t$       | level | reason                                       |
| --- | ----------- | ----- | -------------------------------------------- |
| 0   | $\lnot x_1$ | 0     | $\lbrace\lnot x_1\rbrace$                    |
| 1   | $x_2$       | 1     | $\Lambda$                                    |
| 2   | $\lnot x_5$ | 1     | $\lbrace \lnot x_2,\lnot x_5\rbrace$         |
| 3   | $\lnot x_3$ | 2     | $\Lambda$                                    |
| 4   | $x_4$       | 2     | $\lbrace x_1,x_3,x_4\rbrace$                 |
| 5   | $x_6$       | 2     | $\lbrace x_1,\lnot x_2,\lnot x_4,x_6\rbrace$ |
| 6   | $\lnot x_6$ | 2     | $\lbrace x_3,\lnot x_4,x_5,\lnot x_6\rbrace$ |
ミュンヘン工科大学の資料より

ここで $t$ が 0 から 3 までの条件 $\lnot x_1\land x_2\land\lnot x_5\land\lnot x_3$ のときコンフリクトすることが分かる. するとその否定 $x_1\lor\lnot x_2\lor x_5\lor x_3 = \lbrace x_1,\lnot x_2,x_5,x_3\rbrace$ は必ず成立しなければならない為, 新たに条件として入れることができる. このときなぜ $\lnot x_4$ を入れてはいけないのかよくわからない. 資料には $x_6$ に依存しているからと書かれているが $\lbrace x_1,x_3,x_4\rbrace$ は $x_1, x_3$ によって決定されるので関係なさそう(入れた方が条件が弱くなるのは確かにそうだけれども, もし全てfalseでも $\lbrace x_1,x_3,x_4\rbrace$ とコンフリクトするので大丈夫そう). たぶん節内のリテラルを出来る限り少なくして探索時に有用に扱いたいんだと思う.
そして条件が多くなればなるほど探索をしなくて済むので高速化出来る.

### 実装
効率的なCDCLの実装方法を学ぶ.

## SMT
SMT (Satisfiability Modulo Theories)
EUF (Equality Logic With Uninterpreted Functions)

FOL (First-Order Logic)
HOL (Higher-Order Logic)

### Bit Vectors

### DPLL(T)


SMT ソルバ全般
- [SAT/SMTソルバの仕組み](https://www.slideshare.net/sakai/satsmt)
- [ミュンヘン工科大学の夏学期の自動推論に関する授業](https://www21.in.tum.de/teaching/sar/SS20/)

SAT/SMTソルバのサーベイ論文
- [SATソルバ・SMTソルバの技術と応用](https://www.jstage.jst.go.jp/article/jssst/27/3/27_3_3_24/_pdf)
- [A Survey of Satisfiability Modulo Theory](https://arxiv.org/abs/1606.04786)

SMTソルバの入力形式 SMT-LIBv2
- [The SMT-LIBv2 Language and Tools: A Tutorial](http://smtlib.github.io/jSMTLIB/SMTLIBTutorial.pdf)
p20. SMT-LIBv2 の token が表になって並んでおり、どのような正規表現でマッチさせられるか掲載している
- [SMT-LIB The Satisfiability Modulo Theories Library](http://smtlib.cs.uiowa.edu/)
SMT ソルバに与える入力の形式 SMT-LIB v2 についてまとまっている Web サイト
- [SMT-LIB-benchmarks / QF_UF · GitLab (uiowa.edu)](https://clc-gitlab.cs.uiowa.edu:2443/SMT-LIB-benchmarks/QF_UF)
QF_UF のベンチマーク用入力が大量に用意されている

形式手法
- モデル検査
- 定理証明支援系