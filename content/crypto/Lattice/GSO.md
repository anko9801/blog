---
title: "Gram-Schmidt 直交化"
---

## 説明

Gram-Schmidt 直交化 (GSO; Gram-Schmidt Orthonormalization) とは実 $m$ 次元ベクトル空間 $\mathbb{R}^m$ の任意の $\mathbb{R}$ ベクトル空間としての基底を直交基底に変換する方法である. イメージは [グラム・シュミットの正規直交化法のWikipedia](https://ja.wikipedia.org/wiki/%E3%82%B0%E3%83%A9%E3%83%A0%E3%83%BB%E3%82%B7%E3%83%A5%E3%83%9F%E3%83%83%E3%83%88%E3%81%AE%E6%AD%A3%E8%A6%8F%E7%9B%B4%E4%BA%A4%E5%8C%96%E6%B3%95) のgifがわかりやすいです。$\mathbf{b} _ n$ の直交化は $\mathbf{b} _ {1},\ldots, \mathbf{b} _ {n-1}$ すべてと直行するように元の高さのまま移動させる。

**Definition** (GSOベクトル) $n$ 次元格子 $L\subseteq \mathbb{R}^m$ の順序付き基底 $\{\mathbf{b} _ {1},\ldots, \mathbf{b} _ {n}\}$ に対するGSOベクトル $\mathbf{b} _ {1}^* ,\ldots, \mathbf{b} _ {n}^ *\in\mathbb{R}^m$ を次のように定義する. また $\mu _ {i,j}$ をGSO係数と呼ぶ.

$$
\begin{aligned}
&\begin{dcases}
\mathbf{b} _ 1^* := \mathbf{b} _ 1 \\
\mathbf{b} _ i^ * := \mathbf{b} _ i - \sum _ {j=1}^{i-1} \mu _ {i, j} \mathbf{b} _ j^ * & (2\leq i\leq n) \\
\end{dcases} \\
&\quad\mu _ {i, j} := \frac{\langle \mathbf{b} _ i, \mathbf{b}_j^ * \rangle}{\| \mathbf{b} _ j^ * \|^2} \qquad (1\leq j<i\leq n)
\end{aligned}
$$

行列を用いて表現すると次のように(要詳細) $B, B^* , U$ を定義したとき $B = UB^ *$ が成り立つと書ける.

$$
\begin{aligned}
B &=
\begin{pmatrix}
\mathbf{b} _ 1 \\
\vdots \\
\mathbf{b} _ n \\
\end{pmatrix}
& B^* &=
\begin{pmatrix}
\mathbf{b} _ 1^ * \\
\vdots \\
\mathbf{b} _ n^ * \\
\end{pmatrix}
& U &=
\begin{pmatrix}
1 & 0 & 0 & \cdots & 0 \\
\mu _ {2,1} & 1 & 0 & \cdots & 0 \\
\mu _ {3,1} & \mu_{3,2} & 1 & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
\mu _ {n,1} & \mu _ {n,2} & \mu _ {n,3} & \cdots & 1 \\
\end{pmatrix}
\end{aligned}
$$

**Theorem** (GSOベクトルの基本性質)
1. 任意の $1\leq i<j\leq n$ に対して $\langle\mathbf{b}_ i^* , \mathbf{b} _j^ *\rangle = 0$ が成り立つ.
2. 任意の $1\leq i\leq n$ に対して $\|\mathbf{b}_ i^ * \|\leq\|\mathbf{b} _i\|$ が成り立つ.
3. 任意の $1\leq i\leq n$ に対して $\langle\mathbf{b} _ 1^* ,\ldots,\mathbf{b} _ i^ *\rangle_{\mathbb{R}} = \langle\mathbf{b} _ 1,\ldots,\mathbf{b} _ i\rangle _ {\mathbb{R}}$ が成り立つ.
4. $\mathrm{vol}(L) = \prod _ {i=1}^n\|\mathbf{b} _ i^*\|$ が成り立つ.

**Proof.**
1. $j$ に関する数学的帰納法により示す. $j=1$ のときは証明すべきことはない. $1,\ldots,j$ について成立していると仮定する. $j+1$ のとき任意の $1\leq i<j+1$ に対して
$$
\begin{aligned}
\langle\mathbf{b} _ i^ * , \mathbf{b} _ {j+1}^ * \rangle &= \left\langle\mathbf{b} _ i^ * , \mathbf{b} _ {j+1} - \sum _ {k=1}^j\mu _ {j+1, k}\mathbf{b} _ {k}^ * \right\rangle \\
&= \langle\mathbf{b} _ i^ * , \mathbf{b} _ {j+1}\rangle - \mu _ {j+1,i}\langle\mathbf{b} _ i^ * , \mathbf{b} _ {i}^ * \rangle \\
&= \langle\mathbf{b} _ i^ * , \mathbf{b} _ {j+1}\rangle - \frac{\langle \mathbf{b} _ {j+1}, \mathbf{b} _ i^ * \rangle}{\| \mathbf{b} _ i^ * \|^2}\|\mathbf{b} _ i^ * \|^2 \\
&= 0
\end{aligned}
$$
が成り立つ. よって, 数学的帰納法より任意の $1\leq i<j\leq n$ に対して $\langle\mathbf{b} _ i^ * , \mathbf{b} _ j^ * \rangle = 0$ が成り立つ.

2. $i=1$ のとき $\mathbf{b} _ 1^* = \mathbf{b} _ 1$ より明らか. $i\geq 2$ のとき
$$
\|\mathbf{b}_ i\|^2 = \|\mathbf{b} _ i^ * \|^2 + \sum_{j=1}^{i-1}\mu _ {i,j}^2\|\mathbf{b}_j^ * \|^2\geq\|\mathbf{b} _ i^ * \|^2
$$
より成り立つ.

3. 任意の $1\leq k\leq i$ に対し, $\mathbf{b} _ k = \mathbf{b} _ k^* + \sum _ {j=1}^{k-1} \mu _ {k, j}\mathbf{b} _ j^ *$ より, $\mathbf{b} _ k\in\langle\mathbf{b} _ 1^ * ,\ldots,\mathbf{b} _ i^ * \rangle _ {\mathbb{R}}$ がわかる. よって $\langle\mathbf{b} _ 1,\ldots,\mathbf{b} _ i\rangle _ {\mathbb{R}}\subseteq\langle\mathbf{b} _ 1^ * ,\ldots,\mathbf{b} _ i^ * \rangle _ {\mathbb{R}}$ が成り立つ. 逆向きの包含関係は $i$ に関する数学的帰納法で示す. $i = 1$ のとき $\mathbf{b} _ 1^ * = \mathbf{b} _ 1$ より明らか. $i=k-1$ のとき成り立つと仮定すると $i=k$ のとき $\mathbf{b} _ k^ * = \mathbf{b} _ k - \sum _ {j=1}^{k-1} \mu _ {k, j}\mathbf{b} _ j^ *$ より $\mathbf{b} _ k^ * \in\langle\mathbf{b} _ 1,\ldots,\mathbf{b} _ i\rangle _ {\mathbb{R}}$ , よって任意の $i$ について示された. よって $\langle\mathbf{b} _ 1^ * ,\ldots,\mathbf{b} _ i^ * \rangle _ {\mathbb{R}}=\langle\mathbf{b} _ 1,\ldots,\mathbf{b} _ i\rangle _ {\mathbb{R}}$ である.

4. $B=UB^*$ と $\det(U) = 1$, GSOベクトルの直交性より
$$
\begin{aligned}
\mathrm{vol}(L)^2 &= \det(BB^\top) \\
&= \det(UB^ * (B^ * )^\top U^\top) \\
&= \det(B^ * (B^ * )^\top) \\
&= \prod _ {i=1}^n\|\mathbf{b} _ i^ * \|^2
\end{aligned}
$$

## 計算量
$O(N^3)$

## 実装

```python
def gram_schmidt():
```

## 使用例

## 参考
