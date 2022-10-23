---
title: "レイトレーシング"
---

## 説明

レイトレーシング(Ray Tracing)とはCGを作成する技術の一つ。
視点からレイ(Ray)と呼ばれる仮想の光線を飛ばし, 反射・屈折を繰り返して, 光源に当たればそこから逆算して画素を表示する仕組みです.

PPM画像という数値をASCII, 区切り文字を空白で画素を表現する規格を用いると楽

ベクトル空間を定義して法線ベクトル

## 物理ベースレンダリング理論
ある波長 $\lambda$ の光のエネルギー $e_\lambda=\frac{hc}{\lambda}$
スペクトル放射エネルギー $Q_\lambda=ne_\lambda$ $Q=\int_0^\infty Q_\lambda d\lambda$
スペクトル放射束 $\Phi_\lambda=\frac{dQ_\lambda}{dt}$ $\Phi=\frac{dQ}{dt}$
放射輝度 $L(x,\vec\omega)=\frac{d^2\Phi}{\cos\theta dAd\vec\omega}$

RGBそれぞれのスペクトル放射束を画像にすればよい。
現実は可視光領域全てのスペクトルなのでこれは新井金治である。
可視光の全波長考慮したレンダリングをフルスペクトラムレンダリングと呼び、プリズムの光の分散などで使われます。

空気が真空なら光が進行しても放射輝度の値は変わらない。
現実は空気中の微粒子との衝突で光の吸収、散乱などの現象が起きる。
これを再現したレンダリングをボリュームレンダリングと呼び、煙や雲などで使われます。

### 双方向反射率分布関数 (BRDF)
反射・屈折を表現する関数
$$
f(x,\vec\omega_i,\vec\omega_r) = \frac{dL_r(x,\vec\omega_r)}{L_i(x,\vec\omega_i)\cos\theta d\vec\omega_i}
$$

$$
L_r(x,\vec\omega_r) = \int_\Omega f(x,\vec\omega_i,\vec\omega_r)L_i(x,\vec\omega_i)\cos\theta d\vec\omega_i
$$

レンダリング方程式
$$
L_o(x,\vec\omega_r) = L_e(x,\vec\omega_r) + L_r(x,\vec\omega_r)
$$

$L_r(x,\vec\omega_r) = TL_i(x,\omega_i)$
$L_i(x,\vec\omega_i) = L_o(t(x,\vec\omega_i),\vec\omega_i)$

$$
\begin{aligned}
L_o(x_0,\vec\omega_0) &= L_e(x_0,\vec\omega_0) + L_r(x_0,\vec\omega_0) \\
&= L_e(x_0,\vec\omega_0)+TL_i(x_0,\vec\omega_1) \\
&= L_e(x_0,\vec\omega_0)+TL_o(t(x_0,\vec\omega_1),\vec\omega_1) \\
&= L_e(x_0,\vec\omega_0)+T(L_e(x_1,\vec\omega_1)+TL_o(x_2,\vec\omega_2)) \\
&= L_e(x_0,\vec\omega_0)+T(L_e(x_1,\vec\omega_1)+T(L_e(x_2,\vec\omega_2)+T(L_e(x_3,\vec\omega_3)+\ldots))) \\
&= L_e(x_0,\vec\omega_0)+\sum_{k=1}^\infty T^kL_e(x_k,\vec\omega_k) \\
\end{aligned}
$$

モンテカルロ法を用いることで積分を数値的解くことが出来る。
$$
\begin{aligned}
L_o(x_0,\vec\omega_0) &= L_e(x_0,\vec\omega_0)+\sum_{k=1}^\infty T^kL_e(x_k,\vec\omega_k) \\
&= L_e(x_0,\vec\omega_0)+\sum_{k=1}^\infty \int_\Omega f(x,\vec\omega_k,\vec\omega_r)\ldots\left(\int_\Omega f(x,\vec\omega_k,\vec\omega_{k-1})L_e(x_k,\vec\omega_k)\cos\theta_k d\vec\omega_k\right)\ldots \cos\theta_1 d\vec\omega_1 \\
&= L_e(x_0,\vec\omega_0)+\sum_{k=1}^\infty\int_\Omega f(x_1,\vec\omega_1,\vec\omega_0)\ldots f(x_k,\vec\omega_k,\vec\omega_{k-1})L_e(x_{k},\vec\omega_{k})\cos\theta_1\ldots\cos\theta_k d\vec\omega_1\ldots d\vec\omega_k \\
&\approx L_e(x_0,\vec\omega_0)+\sum_{k=1}^\infty \frac{1}{N}\sum_{s=1}^N \frac{f(x_1,\vec\omega_1^s,\vec\omega_0)\ldots f(x_k,\vec\omega_k^s,\vec\omega_{k-1})L_e(x_{k},\vec\omega_{k})\cos\theta_1\ldots\cos\theta_k}{p_1(\vec\omega_1)\ldots p_k(\vec\omega_k)} \\
\end{aligned}
$$

サンプルには重点的サンプリングを行う。

