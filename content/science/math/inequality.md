---
title: "不等式"
---

## Minkowski の不等式
三角不等式の一般化. 迂回するより直線的に進む方が短い.

$$
\|x + y\| _ p \leq \|x\| _ p + \|y\| _ p
$$

**proof.**

## ヘルダーの不等式
コーシーシュワルツの不等式の一般化.

$$
\prod_{i=1}^m\left(\sum_{j=1}^n a_{ij}\right)^{w_i} \geq \sum_{j=1}^n\left(\prod_{i=1}^m a_{ij}^{w_i}\right)
$$

## Muirhead の不等式
$[a]\preceq[b]$ のとき

$$
\sum_{sym}\prod_{i=1}^n x_i^{a_i} \geq \sum_{sym}\prod_{i=1}^n x_i^{b_i}
$$

等号成立条件
- $a=b$, $x_1 = \cdots = x_n$

## Schur の不等式
$x, y, z \geq 0$ のとき $x^r(x-y)(x-z) + y^r(y-z)(y-x) + z^r(z-x)(z-y) \geq 0$

等号成立条件
- $(x, y, z) = (a, a, a), (0, a, a)$

## Jensen の不等式
$\forall\lambda_i \geq 0$ $\sum_i\lambda_i = 1$ に対して $f(x)$ が凸関数

$$
f(\lambda_1x_1+\cdots+\lambda_nx_n) \leq \lambda_1f(x_1)+\cdots+\lambda_nf(x_n)
$$

$f(x)$ が凹関数

$$
f(\lambda_1x_1+\cdots+\lambda_nx_n) \geq \lambda_1f(x_1)+\cdots+\lambda_nf(x_n)
$$

## Karamata の不等式
$f(x)$ が凸関数 $[a] \preceq [b]$

$$
f(a_1)+\cdots+f(a_n) \leq f(b_1)+\cdots+f(b_n)
$$

## Weighted AM-GM
相加相乗平均の重み付きにおける一般化. 相加平均 (AM; Arithmetic Mean) は相乗平均 (GM; Geometric Mean) より大きいという定理.

$$
\sum_{i=1}^n w_ia_i \geq \prod_{i=1}^n a_i^{w_i}
$$

**proof.**
$f:x\mapsto \ln x$ は凸関数より Jensen の不等式から

$$
\ln\left(\sum_i\lambda_ia_i\right) \geq \sum_i\lambda_i\ln a_i = \ln\left(\prod_ia_i^{\lambda_i}\right)
$$

となる.
