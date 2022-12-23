---
title: "Tonelli Shanks"
---
$\newcommand{\ZZ}{\mathbb{Z}}$

## 説明
$x^2 = a \pmod p$ の $x$ を求める.
$x^2 = a = a^p$ なのでやりたいことは $x = a^{p/2}$ である。

$$
(\ZZ/p\ZZ)^\times \cong \ZZ/(p-1)\ZZ \cong \ZZ/q\ZZ \times \ZZ/2^Q\ZZ
$$

**Definition**
$\mathrm{ind}: (\ZZ/p\ZZ)^\times\to \ZZ/q\ZZ \times \ZZ/2^Q\ZZ$
$\mathrm{ind}: a \mapsto (a_1, a_2)$

**Proposition**
$1\mapsto(0,0)$
$a^e\mapsto e(a_1,a_2) = (ea_1, ea_2)$
$a^{q+1}\mapsto(a_1, (q+1)a_2)$
$a^{(q+1)/2}\mapsto \frac{q+1}{2}(a_1, a_2)$

$(0, w) = 2\mathop{\mathrm{ind} x - \mathrm{ind}a$

$x^2 = a$

$a^{1/2}$ は難しい $a^2$ は簡単
右シフトは難しいが左シフトは簡単

$a^{(q + 1)/2}:(a_1, a_2) \mapsto (0, w)$

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
