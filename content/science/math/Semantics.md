---
title: "意味論"
---

## 定義

統語論的方法だと推論が正しくないことを証明できない.
意味論的方法で正しい推論とは、前提が真であるならば、結論も必ず真である推論である.
推論が意味論的方法において正しいことを、その推論は意味論的に妥当であるという.

推論の正否を判定する二つの方法: 統語論的方法と意味論的方法は一致するかどうかを考える.
- 健全性: 統語論的に正しい $\implies$ 意味論的に正しい
- 完全性: 意味論的に正しい $\implies$ 統語論的に正しい

$\phi_1,\ldots,\phi_n \vdash \psi$
$\iff$ 前提 $\phi_1,\ldots,\phi_n$ から、結論 $\psi$ を導出する演繹が存在する.

$\phi_1,\ldots,\phi_n \vDash \psi$
$\iff$ 前提 $\phi_1,\ldots,\phi_n$ から結論 $\psi$ を得る推論が意味論的に妥当である.
$\iff$ その真理表において, すべての前提 $(\phi_1,\ldots,\phi_n)$ が真でありかつ結論 $\psi$ が偽である行が存在しない.

特に $\vdash \psi$ を定理, $\vDash \psi$ をトートロジーと呼ぶ.

## 真理値表
次の原理を満たして真理値の表を書く.
2値原理: 任意の記号文は真か偽のどちらか一方である。
関数的合成の原理: 任意の記号文の真理値は、それを構成する原子文の真理値のみに依存して一意に定まる。

//TODO

## 命題論理の健全性・完全性
以下は互いに同値である

1. $\phi_1,\ldots,\phi_n \vdash \psi$
2. $\vdash \phi_1\land\cdots\land\phi_n\to\psi$
3. $\vDash \phi_1\land\cdots\land\phi_n\to\psi$
4. $\phi_1,\ldots,\phi_n \vDash \psi$

**proof**
(1 $\implies$ 2)
1. Show $\vdash \phi_1\land\cdots\land\phi_n\to\psi$
2. $\phi_1\land\cdots\land\phi_n$ AssCD
3. $\phi_1,\ldots,\phi_n$ 2S
4. $\phi_1,\ldots,\phi_n \vdash \psi$ Pr
5. $\psi$
5CD

(1 $\impliedby$ 2)
1. Show $\phi_1,\ldots,\phi_n \vdash \psi$
2. $\phi_1,\ldots,\phi_n$ Pr
3. $\phi_1\land\cdots\land\phi_n$ 2Adj
4. $\vdash \phi_1\land\cdots\land\phi_n\to\psi$ Pr
5. $\psi$ 3,4MP
5DD

(2 $\implies$ 3)
1. 定理の演繹の行番号を演繹を構成する際に利用可能となる順に付け替える. <-ちょっとバグある.
2. 演繹の n 行目において利用可能な家庭すべてからなる連言を前件, n 行目の記号文を後件とする条件文を n 行目の導出式と定義する. ただし利用可能な仮定が存在しないとき, n 行目の記号分そのものを n 行目の導出式とする.
3. 任意の導出式はトートロジーであることを数学的帰納法により示す.
    1. 1行目の導出式はトートロジーである.
    2. n行目までのすべての導出式がトートロジーならば n+1 行目もトートロジーである.
4. 最後の導出式は定理そのものであり, それはトートロジーである.


(2 $\impliedby$ 3)

まず意味論的な真理値と統語論的な結合子を対応させる.

**Def.** 記号 $'$ を次のように定義する.

$$
x' = \begin{cases}
x & (x = 1) \\
\lnot x & (x = 0)
\end{cases}
$$

**Lemma.** 任意の記号文 $\eta$ について, それに含まれる原子文 $P_1,\ldots,P_n$ について $P_1',\ldots,P_n' \vdash \eta'$ となる.
**Proof.**
結合子が 0 個のとき, $P_1' \vdash \eta'$ より成り立つ.
結合子が n 個より小さいすべての記号文が成り立つと仮定し, 各結合子について n 個のときを考える.

$P_1',\ldots,P_n' \vdash \xi'$ が成り立つとき, $P_1',\ldots,P_n' \vdash (\lnot\xi)'$ について

$$
(\lnot\xi)' = \begin{cases}
\lnot\lnot\xi & (\xi = 1) \\
\lnot\xi & (\xi = 0)
\end{cases}
$$

よりこれは演繹により成り立つ.

$P_1',\ldots,P_i' \vdash \pi'$ かつ $Q_1',\ldots,Q_j' \vdash \theta'$ が成り立つとき, $P_1',\ldots,P_i',Q_1',\ldots,Q_j' \vdash (\pi\to\theta)'$ について

$$
(\pi\to\theta)' = \begin{cases}
\pi\to\theta & (\pi = 0 \lor \theta = 1) \\
\lnot(\pi\to\theta) & (\pi = 1 \land \theta = 0)
\end{cases}
$$

よりこれは演繹により成り立つ.

**Thm.** $\vDash \xi \implies \vdash\xi$
**Proof.**
Lemma から記号文 $\xi$ について $P_1',\ldots,P_n' \vdash \xi'$ が成り立つ. $\vDash \xi$ なので $P_1',\ldots,P_n' \vdash \xi$ であり, このとき $P_1',\ldots,P_{n-1}' \vdash P_n'\to\xi$ となる.
同様に $P_n'$ についてだけ真理値が異なる真理表を考えると Lemma から $P_1',\ldots,\lnot P_n' \vdash \xi$ であり, $P_1',\ldots,P_{n-1}' \vdash \lnot P_n'\to\xi$ が成り立つ.
2つのことから $P_1',\ldots,P_{n-1}' \vdash \xi$ が演繹できる. これを繰り返すことで $\vdash \xi$ が得られる.

(3 $\iff$ 4)
$\nvDash \phi_1\land\cdots\land\phi_n\to\psi$
$\iff$ $\phi_1\land\cdots\land\phi_n\to\psi$ がトートロジーではない
$\iff$ $\phi_1\land\cdots\land\phi_n$ が真で $\psi$ が偽である行がある.
$\iff$ $\phi_1,\ldots,\phi_n$ が真で $\psi$ が偽である行がある.
$\iff$ 前提 $\phi_1,\ldots,\phi_n$ から結論 $\psi$ を得る推論は意味論的に妥当ではない.
$\iff$ $\phi_1,\ldots,\phi_n \nvDash \psi$

対偶より

$\phi_1,\ldots,\phi_n \vDash \psi \iff \vDash \phi_1\land\cdots\land\phi_n\to\psi$

よってすべての同値性が証明された.


完全性・健全性が成り立つとき次のことが言える.
**Thm.** 無矛盾性
矛盾する２つの記号文 $\eta$ と $\lnot\eta$ が定理となることはない.
**Proof.**
$\eta$, $\lnot\eta$ が定理だと仮定すると $\eta\land\lnot\eta$ はトートロジーでないから矛盾.
