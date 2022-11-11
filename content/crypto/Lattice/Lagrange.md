---
title: "Lagrange 基底簡約 (Gauss 基底簡約)"
---

## 説明

Lagrange 基底簡約アルゴリズムは2次元格子上の SVP を解く効率的なアルゴリズムである.

**Definition 1** (Lagrange 簡約)
任意の $q \in \mathbb{Z}$ に対して $\|\mathbf{b}_1\| \leq \|\mathbf{b}_2\| \leq \|\mathbf{b}_1 + q\mathbf{b}_2\|$ を満たすとき, 2次元格子の基底 $\{\mathbf{b}_1, \mathbf{b}_2\}$ は Lagrange 簡約されているという.

**Theorem 2** (Lagrange 簡約によって逐次最小基底)
2次元格子の基底 $\{\mathbf{b}_1, \mathbf{b}_2\}$ について次の2つは同値である.
1. 基底 $\{\mathbf{b}_1, \mathbf{b}_2\}$ は Lagrange 簡約されている.
2. 基底 $\{\mathbf{b}_1, \mathbf{b}_2\}$ は格子 $L$ の逐次最小基底である.

**Proof.**
($\implies$)
任意の非零ベクトル $\mathbf{v} = x _ 1\mathbf{b} _ 1 + x _ 2\mathbf{b} _ 2 \in L$ について $x _ 2 = 0$ のとき $\|\mathbf{v}\| = |x _ 1|\|\mathbf{b} _ 1\| \geq \|\mathbf{b} _ 1\|$, $x _ 2 \neq 0$ のとき $\|\mathbf{v}\| \geq \|\mathbf{b} _ 2\| \geq \|\mathbf{b} _ 1\|$ を示す.

$$
\begin{aligned}
\|\mathbf{v}\| &= \|x _ 1\mathbf{b} _ 1 + x _ 2\mathbf{b} _ 2\| \\
&= \|r\mathbf{b} _ 1 + x _ 2(\mathbf{b} _ 2 + q\mathbf{b} _ 1)\| \\
&\geq x _ 2\|\mathbf{b} _ 2 + q\mathbf{b} _ 1\| - r\|\mathbf{b} _ 1\| \\
&= (x _ 2 - r)\|\mathbf{b} _ 2 + q\mathbf{b} _ 1\| + r(\|\mathbf{b} _ 2 + q\mathbf{b} _ 1\| - \|\mathbf{b} _ 1\|) \\
&\geq \|\mathbf{b} _ 2 + q\mathbf{b} _ 1\| \\
&\geq \|\mathbf{b} _ 2\| \geq \|\mathbf{b} _ 1\|
\end{aligned}
$$

Euclidの互除法を2次元格子に対して行うことで Lagrange 簡約されているような基底を生み出せる。

## 計算量

$O(\log{N})$

## 実装

```python
def lagrange(b1, b2):
    if b1.norm() > b2.norm():
        b1, b2 = b2, b1
    while b1.norm() < b2.norm():
        q = - round(b1.dot_product(b2) / b1.norm() ^ 2)
        b1, b2 = b2 + q * b1, b1
    return b2, b1
```

## 使用例

## 参考
