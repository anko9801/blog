---
title: "NTRU 暗号"
---

NTRU (N-th Degree Truncated Polynomial Ring)

$f: \mathbb{Z}_p[x]/(x^n-1) \to \mathbb{Z}_q[x]/(x^n-1)$

$$
F = \sum_{i=0}^nF_ix^i
$$

平文 $M\in\mathbb{Z}_p[x]/I$ とする, 乱数 $R\in\mathcal{L}(d_R, d_R)$ を
$$
C = pHR + M \pmod q \pmod I
$$
