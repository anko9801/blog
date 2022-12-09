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
暗号文 $C$ 秘密鍵 $F$ によって $A$ を求め, $\mathbb{Z} _ p[x]/I$ における逆元 $F _ p^{-1}$ によって復号文 $M'$ を求める.
$$
\begin{aligned}
A &= CF\pmod q\pmod I \\
M' &= AF_p^{-1} \pmod p\pmod I
\end{aligned}
$$
