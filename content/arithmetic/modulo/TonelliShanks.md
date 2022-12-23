---
title: "Tonelli Shanks"
---
$$
\newcommand{\ZZ}{\mathbb{Z}}
\newcommand{\ind}{\mathop{\mathrm{ind}}}
$$

## 説明
$x^2 = a \pmod p$ の $x$ を求める.
フェルマーの小定理から $x^2 = a = a^p$ なのでやりたいことは $x = a^{p/2}$ である。しかし $p$ は奇数なので整数累乗ではなく不可能。まずは分子が偶数となるように次のように分解して $x' = a^{(q+1)/2}$ を計算する。

$$
(\ZZ/p\ZZ)^\times \cong \ZZ/(p-1)\ZZ \cong \ZZ/q\ZZ \times \ZZ/2^Q\ZZ
$$

ここで分かりやすいように新しい記号を導入する。

**Definition**
$\mathrm{ind}: (\ZZ/p\ZZ)^\times\to \ZZ/q\ZZ \times \ZZ/2^Q\ZZ$
$\mathop{\mathrm{ind}}a = (a_1, a_2)$

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
第二要素の誤差を $\mathop{\mathrm{ind}}(x'^2a^{-1}) = (0, w)$ と表される。ここでまず $w$ を2進数として見ると左シフトを

$a^{1/2}$ は難しい $a^2$ は簡単
右シフトは難しいが左シフトは簡単

$$
\begin{aligned}
x &\mapsto x^{(p-1)/2} \\
(\ZZ/p\ZZ)^\times &\to \lbrace 1, -1\rbrace \\
\mathrm{数} &\to \mathrm{平方剰余かどうか}
\end{aligned}
$$

```
a               = 
E = x^2/a       = 1010100
E^(2^(Q-shift)) = 1000000 or 0000000
b               = ??????1
b^(2^shift)     = ????100
```
