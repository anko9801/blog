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

右シフトは難しいが左シフトは簡単

**Definition**
$\mathrm{ind}: (\ZZ/p\ZZ)^\times\to \ZZ/q\ZZ \times \ZZ/2^Q\ZZ$
$\mathrm{ind}: a \mapsto (a_1, a_2)$
**Theorem**


$(a_1, a_2) = \log{a}$
$\frac{q+1}{2}(a_1, a_2) = \log x$
$(0, w) = 2\log x - \log a$
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
