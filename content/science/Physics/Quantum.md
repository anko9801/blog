---
title: "量子力学"
---

**Definition** (ケットとその演算)
量子力学での複素ベクトル空間の状態ベクトルをケット(ket) と呼び $\ket{\alpha}$ と記す。ケット同士を足すと新しいケット $\ket{\gamma} = \ket{\alpha} + \ket{\beta}$、複素数 $c$ を掛けても新しいケット $c\ket{\alpha} = \ket{\alpha}c$ ができる。また運動量などの観測可能量はベクトル空間の演算子 $A$ によって表される。一般に運動量は左から作用して新しいケット $A\cdot(\ket{\alpha}) = A\ket{\alpha}$ になる。

一般に $A\ket{\alpha}$ は $\ket{\alpha}$ の定数倍ではない。しかし、固有ケットと呼ばれる特別なケット $\ket{}$ のとき固有値 $固有状態

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

