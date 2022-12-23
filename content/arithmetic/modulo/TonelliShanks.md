$\newcommand{\ZZ}{\mathbb{Z}}$
---
"title": "Tonelli Shanks"
---


## 説明
$x^2 = a \pmod p$ の $x$ を求める.
$x^2 = a = a^p$ なのでやりたいことは $x = a^{p/2}$ である。
$(\ZZ/p\ZZ)^\times \cong \ZZ/(p-1)\ZZ \cong \ZZ/q\ZZ \times \ZZ/2^Q\ZZ$
右シフトは難しいが左シフトは簡単
$(a_1, a_2) = \log{a}$
$\frac{q+1}{2}(a_1, a_2) = \log x$
$(0, w) = 2\log x - \log a$
$a^{(q + 1)/2}:(a_1, a_2) \mapsto (0, w)$

$f(x) = x^{(p-1)/2}$
