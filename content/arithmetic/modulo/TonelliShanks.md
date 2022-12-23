---
title: "Tonelli Shanks"
---
$$
\newcommand{\ZZ}{\mathbb{Z}}
\newcommand{\ind}{\mathop{\mathrm{ind}}}
$$

## 概説
$p$ を素数として任意の $a\in\ZZ/p\ZZ$ に対し $x^2 = a$ となる $x$ を求めることができるアルゴリズムである。

まずアイデアを一通り説明しておくと、フェルマーの小定理から $x^2 = a = a^p$ であり、やりたいことは $x = a^{p/2}$ である。しかし $p$ は奇数なので整数累乗ではなく不可能。そこでまずは次のように分解することで整数累乗ができる代数 $\ZZ/q\ZZ$ から $x' = a^{(q+1)/2}$ を計算する。

$$
(\ZZ/p\ZZ)^\times \cong \ZZ/(p-1)\ZZ \cong \ZZ/q\ZZ \times \ZZ/2^Q\ZZ
$$

そうすると $x$ と $x'$ について $\ZZ/q\ZZ$ が合うので $\ZZ/2^Q\ZZ$ も合わせれば完璧である。こちらは $\ZZ/2^Q\ZZ$ での差分を $Q$ 桁の2進数と見なして、下から順に $1$ を $0$ に変換することで誤差を無くし $x$ が求まるといった感じである。

## 詳説

まずは分かりやすいように新しい記号を導入する。

**Definition**
$\mathrm{ind}: (\ZZ/p\ZZ)^\times\to \ZZ/q\ZZ \times \ZZ/2^Q\ZZ$
$\mathop{\mathrm{ind}}a = (a_1, a_2)$

$p = 13$ のとき
|  $2^Q$ \ $q$  | 0   | 1   | 2   |
| --- | --- | --- | --- |
| 0   | 0   | 4   | 8   |
| 1   | 9   | 1   | 5   |
| 2   | 6   | 10  | 2   |
| 3   | 3   | 7   | 11  |

**Proposition**
1. $\mathop{\mathrm{ind}}1 = (0,0)$
2. $\mathop{\mathrm{ind}}ab = \mathop{\mathrm{ind}}a + \mathop{\mathrm{ind}}b = (a_1, a_2) + (b_1, b_2) = (a_1 + b_1, a_2 + b_2)$
3. $\mathop{\mathrm{ind}}a^e = e(a_1,a_2) = (ea_1, ea_2)$

**Proposition**
1. $\mathop{\mathrm{ind}}a^{q} = (0, qa_2)$
2. $\mathop{\mathrm{ind}}a^{2^Q} = (2^Qa_1, 0)$
3. $\mathop{\mathrm{ind}}a^{q2^Q} = \mathop{\mathrm{ind}}a^{p} = \mathop{\mathrm{ind}}1 = (0, 0)$

目標は $x^2 = a$ となる $x$ から $\mathop{\mathrm{ind}}x^2 = (a_1, a_2)$ となる $x$ を見つけることに変化した。
ここで $x' = a^{(q+1)/2}$ とおくと

$$
\begin{aligned}
\mathop{\mathrm{ind}}x' &= \frac{q+1}{2}(a_1, a_2) \\
\mathop{\mathrm{ind}}x'^2 &= (a_1, (q+1)a_2)
\end{aligned}
$$

となり第一要素は一致するようになった。
第二要素の誤差 $E = x'^2a^{-1}$ は $\mathop{\mathrm{ind}}E = (0, w)$ と表される。ここでまず $w$ を $Q$ 桁の2進数として見る。2乗と左シフトを対応させられる。

$a^{1/2}$ は難しい $a^2$ は簡単
右シフトは難しいが左シフトは簡単

非平方剰余な数 $b$ ($b^{(p-1)/2} = 1$ を満たす $b$ ) を取り、$b' = b^q$ を用いて $\mathop{\mathrm{ind}}b^q = (0, b')$ とする。


```
w        = 1010110
w2^(Q-1) = 1000000 or 0000000
b'       = ??????1
b'2^1    = ?????10
```
