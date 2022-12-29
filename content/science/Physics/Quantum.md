---
title: "量子力学"
---

**Definition**
量子力学での複素ベクトル空間の状態ベクトルをケット(ket) と呼び $\ket{\alpha}$ と記す。ケットの演算
観測可能量は演算子左から作用して別のケットになる。

**Theorem** (ブラケットの表現)
$$
\begin{aligned}
\ket{\alpha} & = \begin{pmatrix}
\braket{a^{(1)}|\alpha} \\
\braket{a^{(2)}|\alpha} \\
\vdots
\end{pmatrix} \\
\bra{\gamma} & = \begin{pmatrix}
\braket{a^{(1)}|\gamma}^* & \braket{a^{(2)}|\gamma}^* & \cdots
\end{pmatrix} \\
\ket{\beta}\bra{\alpha} &= \begin{pmatrix}
\braket{a^{(1)}|\beta}\braket{a^{(1)}|\alpha}^* & \braket{a^{(1)}|\beta}\braket{a^{(2)}|\alpha}^* & \cdots \\
\braket{a^{(2)}|\beta}\braket{a^{(1)}|\alpha}^* & \braket{a^{(2)}|\beta}\braket{a^{(2)}|\alpha}^* & \cdots \\
\vdots & \vdots & \ddots
\end{pmatrix}
\end{aligned}
$$
**Proof.**

