---
title: "レイトレーシング, パストレーシング"
---

## 説明

レイトレーシング(Ray Tracing)とはCGを作成する技術の一つ。
視点からレイ(Ray)と呼ばれる仮想の光線を飛ばし, 反射・屈折を繰り返して, 光源に当たればそこから逆算して画素を表示する仕組みです.

ガンマ補正 $(R^{2.2}, G^{2.2}, B^{2.2})$
PPM画像という数値をASCII, 区切り文字を空白で画素を表現する規格を用いると楽

ベクトル空間を定義して法線ベクトル

## 物理ベースレンダリング理論
ある波長 $\lambda$ の光のエネルギー
$$e_\lambda=\frac{hc}{\lambda}$$
スペクトル放射エネルギー
$$
\begin{aligned}
Q_\lambda&=ne_\lambda \\
Q&=\int_0^\infty Q_\lambda d\lambda
\end{aligned}
$$
スペクトル放射束
$$
\begin{aligned}
\Phi_\lambda&=\frac{dQ_\lambda}{dt} \\
\Phi&=\frac{dQ}{dt}
\end{aligned}
$$
放射輝度
$$L(x,\vec\omega)=\frac{d^2\Phi}{\cos\theta dAd\vec\omega}$$

RGBそれぞれのスペクトル放射束を画像にすればよい。
現実は可視光領域全てのスペクトルなのでこれは新井金治である。
可視光の全波長考慮したレンダリングをフルスペクトラムレンダリングと呼び、プリズムの光の分散などで使われます。

空気が真空なら光が進行しても放射輝度の値は変わらない。
現実は空気中の微粒子との衝突で光の吸収、散乱などの現象が起きる。
これを再現したレンダリングをボリュームレンダリングと呼び、煙や雲などで使われます。

### 双方向反射率分布関数 (BRDF)
反射・屈折を表現する関数 BRDF
$$
f(x,\vec\omega_i,\vec\omega_r) = \frac{dL_r(x,\vec\omega_r)}{L_i(x,\vec\omega_i)\cos\theta d\vec\omega_i}
$$
BRDF を用いると半球全体 $\Omega$ からある点にきた光が、方向 $\vec\omega_r$ に反射される放射輝度の値 $L_r(x,\vec\omega_r)$ を計算することが出来る。
$$
L_r(x,\vec\omega_r) = \int_\Omega f(x,\vec\omega_i,\vec\omega_r)L_i(x,\vec\omega_i)\cos\theta d\vec\omega_i
$$
これを $L_r(x,\vec\omega_r) = TL_i(x,\omega_i)$ と略す事にする。
- 物体表面のある点 $x$ から方向 $\vec\omega_r$ へ出射する放射輝度 $L_o(x,\vec\omega_r)$
- 点 $x$ において物体が発光し、$\vec\omega_r$ の方向へ出射する放射輝度 $L_e(x,\vec\omega_r)$
- 点 $x$ から $\vec\omega_r$ に反射される放射輝度 $L_r(x,\vec\omega_r)$
として次のレンダリング方程式が成り立つ。
$$
L_o(x,\vec\omega_r) = L_e(x,\vec\omega_r) + L_r(x,\vec\omega_r)
$$
また, 点 $x$ から $\vec\omega$ の方向へ進んだとき, 衝突する点を返す関数 $t(x,\vec\omega)$ を用いて $L_i(x,\vec\omega_i) = L_o(t(x,\vec\omega_i),\vec\omega_i)$ と表せられる。するとレンダリング方程式は以下の無限級数で表現できる。
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
&= L_e(x_0,\vec\omega_0)+\sum_{k=1}^\infty\int_\Omega\ldots\int_\Omega f(x_1,\vec\omega_1,\vec\omega_0)\ldots f(x_k,\vec\omega_k,\vec\omega_{k-1})L_e(x_{k},\vec\omega_{k})\cos\theta_1\ldots\cos\theta_k d\vec\omega_1\ldots d\vec\omega_k \\
&\approx L_e(x_0,\vec\omega_0)+\sum_{k=1}^\infty \frac{1}{N}\sum_{s=1}^N \frac{f(x_1,\vec\omega_1^s,\vec\omega_0)\ldots f(x_k,\vec\omega_k^s,\vec\omega_{k-1})L_e(x_{k},\vec\omega_{k})\cos\theta_1\ldots\cos\theta_k}{p_1(\vec\omega_1)\ldots p_k(\vec\omega_k)} \\
\end{aligned}
$$
サンプルには重点的サンプリングを行う。確率密度関数の累積分布関数の逆関数を用いてサンプルを生成することで

級数の計算ではロシアンルーレットを用いる。次項を計算する確率 $p$ を用いて次のように計算する。
$$
\begin{aligned}
S_n &= \sum_{k=0}^n x_k \\
R_n &= S_\infty - S_n \\
\hat R_n &= x_n + \begin{dcases}
\frac{\hat R_{n+1}}{p} & (u\leq p) \\
0 & (u>p)
\end{dcases} \\
\hat S_n &\approx x_0 + \frac{x_1}{p} + \frac{x_2}{p^2} + \ldots + \frac{x_n}{p^n} \\
\end{aligned}
$$
この期待値を計算すると $S_\infty$ となる。
$$
E[\hat S] = x_0 + p\frac{x_1}{p} + p^2\frac{x_2}{p^2} + \ldots + p^n\frac{\hat R_n}{p^n} = S_\infty
$$

## モデル
### 拡散反射モデル
紙のような乱反射する物体を考える。このときBRDFは次のようになる。
$$
f_{Diffuse}(x, \vec\omega_i, \vec\omega_o) = \frac{\rho}{\pi}
$$
この確率密度関数 $p(\vec\omega)$ はBRDF$\times\cos\theta$ に比例する為, 定数 $k$ を用いて $p(\vec\omega) = k\cos\theta$ となる。確率の条件より $k$ を決定することが出来る。
$$
\begin{aligned}
\int_\Omega p(\vec\omega)d\vec\omega &= k\int_0^{2\pi}\int_0^{\frac{\pi}{2}}\cos\theta\sin\theta d\theta d\phi = k\pi = 1 \\
p(\vec\omega) &= \frac{\cos\theta}{\pi}
\end{aligned}
$$
確率密度関数について変数を変えると
$$
\begin{aligned}
p(\theta,\phi) &= p(\vec\omega)\sin\theta = \frac{1}{\pi}\sin\theta\cos\theta \\
p(\theta) &= \int_0^{2\pi}p(\theta, \phi)d\phi = \sin{2\theta} \\
p(\phi\mid\theta) &= \frac{p(\theta,\phi)}{p(\theta)} = \frac{1}{2\pi}
\end{aligned}
$$
この累積分布関数の逆関数は
$$
\begin{aligned}
P(\theta) &= \int_0^\theta p(t)dt = \frac{1-\cos{2\theta}}{2} & P_\theta^{-1}(u) &= \frac{1}{2}\cos^{-1}(1-2u) \\
P(\phi\mid\theta) &= \int_0^\phi p(\phi\mid\theta)dt = \frac{\phi}{2\pi} & P_\phi^{-1}(v) &= 2\pi v
\end{aligned}
$$
となるので $\theta, \phi$ のサンプルは乱数 $u, v$ を用いて
$$
\begin{dcases}
\theta = \frac{1}{2}\cos^{-1}(1-2u) \\
\phi = 2\pi v
\end{dcases}
$$
と計算すればよい。これを方向ベクトルとするには
$$
\begin{dcases}
x = \sin\theta\cos\phi \\
y = \cos\theta \\
z = \sin\theta\sin\phi
\end{dcases}
$$
とする。

### 完全鏡面反射モデル
反射についての関係式は次のようになる。
$$
\vec\omega_i = -\vec\omega_o + 2(\vec\omega_o\cdot\vec n)\vec n
$$
これよりBRDFは次のようになる。$\vec\omega_r$ を $\vec\omega_o$ の反射方向とする。
$$
f_{Specular}(x,\vec\omega_i,\vec\omega_o) = \frac{\delta(\vec\omega_i-\vec\omega_r)}{\cos\theta}
$$
サンプルは反射方向を返せばよい。

### ガラスモデル
ガラスに入射した光は反射と屈折をする。反射は上記の式を用いればよい。屈折方向 $\vec\omega_r$ は次のように計算する。入射側屈折率 $n_1$ 出射側屈折率 $n_2$ と入射光線が法線となす角度 $\theta$ として
$$
\begin{aligned}
\vec\omega_r &= \frac{n_1}{n_2}(-\vec\omega_i+\cos\theta\vec n)-\sqrt{1-\alpha^2}\vec n \\
\alpha &= \frac{n_1}{n_2}\sin\theta
\end{aligned}
$$
このBRDFは次のようになる。
$$
f_{Refract}(x,\vec\omega_i,\vec\omega_o) = (1-f)\frac{\delta(\vec\omega_i-\vec\omega_r)}{\cos\theta}
$$
また屈折時には輝度も変化する。
$$
L_r = \left(\frac{n_1}{n_2}\right)^2L
$$
また、反射/屈折の割合はフレネルの式(Fresnel Equation) で表現される。それは複雑な式であるので通常それを近似したSchlickの近似式が使われる。
$$
\begin{aligned}
F_r(\theta) &= F_o + (1 - F_o)(1 - \cos\theta)^5 \\
F_o &= \left(\frac{n_1-n_2}{n_1+n_2}\right)^2
\end{aligned}
$$
これについてロシアンルーレットを用いれば

### 光モデル
発光については全ての方向に同じ強さの放射輝度の光を返せばよい。
空についても同様に地平線に近ければ白く、天井に近くなれば青いように返せばよい。
また IBL (Image Based Lighting) を用いれば輝度を保持する HDR 画像を用いて周囲を画像で表現できる。[sIBL Archive (hdrlabs.com)](http://www.hdrlabs.com/sibl/archive.html) を使うとよい。

## その他
### Next Event Estimation (NEE)
光源が少ない場所ではノイズが多く出やすい。そこで光源上の点をサンプリングしてその方向にレイを飛ばすことで光源が小さい場合でも寄与を加算しやすくできる。

### Bounding Volume Hierarchy (BVH)
複数の物体全体を囲むバウンディングボリュームを再帰的に作り、それぞれのレイヤーで衝突判定を行うことで衝突の探索を $O(n)$ から $O(\log n)$ に改善させる方法。

### 双方向パストレーシング
レイを飛ばすだけではなく、光源からシーンに向けても光を追跡する方法。

### メトロポリス光輸送

### フォトンマッピング

## 参考
Photorealism yumcyawiz