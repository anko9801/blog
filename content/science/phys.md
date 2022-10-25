---
title: "物理学"
---

# 物理学

## ベクトル解析

[ベクトル解析ノート](https://drive.google.com/file/d/1C-eszj1VSI98aenIdvabiADAPzYHqdXA/view?usp=sharing)

## 電磁気学

## 解析力学

波動方程式の離散化

$$
\begin{aligned}
\frac{\partial^2u}{\partial t^2} &= v^2\left(\frac{\partial^2u}{\partial x^2} + \frac{\partial^2u}{\partial y^2}\right) \\
\frac{u_{i,j}^{n+1}-2u_{i,j}^n+u_{i,j}^{n-1}}{\Delta t^2} &= v^2\left(\frac{u_{i,j}^{n+1}-2u_{i,j}^n+u_{i,j}^{n-1}}{\Delta x^2} + \frac{u_{i,j}^{n+1}-2u_{i,j}^n+u_{i,j}^{n-1}}{\Delta y^2}\right) \\
u_{i,j}^{n+1} &= 2u_{i,j}^n - u_{i,j}^{n-1} + \Delta t^2v^2\left(\frac{u_{i,j}^{n+1}-2u_{i,j}^n+u_{i,j}^{n-1}}{\Delta x^2} + \frac{u_{i,j}^{n+1}-2u_{i,j}^n+u_{i,j}^{n-1}}{\Delta y^2}\right)
\end{aligned}
$$

## 量子力学
