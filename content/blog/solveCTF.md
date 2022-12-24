
## SECCONCTF 2022
### [Crypto] pqpq
**Proposition**
fully byte padded RSA
$$
\begin{aligned}
n &= pqr, e = 2\cdot 65537 \\
c_1 &= p^e - q^e, c_2 = (p-q)^e, c_m = m^e
\end{aligned}
$$

**writeup**
1. $\gcd(c_1 + c_2, n) = p$ より $p, qr$ に分解.
2. $\gcd(c_2 - c_1, n) = q$ より $q, r$ に分解.
3. $e/2$ の逆元を取って $m^2$ を取得.
4. $p, q, r$ それぞれの剰余を取って Tonelli Shanks して $m \bmod p$ を取得.
5. 中国剰余定理から $m$ を復元.


### [Crypto] BBB
**Proposition**
$f(x) = x^2 + ax + b$ として $b, s_i$ を指定し、 $e_i = f^{n}(s_i) \quad (n\sim 100)$ かつ $e_i > 10$ となるような $e_i$ に設定される。
平文長が暗号 1/20 程度。

**writeup**
1. $e = 11$ の不動点が作られるような $b$ を指定する。
2. 回しまくる。
3. 成功した中から中国剰余定理で平文長を覆う程度まで合わせれば $m^{11}$ が取れる。
4. 整数上で $e$ 乗根を取る。

### [Crypto] Witches symmetric exam
**Proposition**
AES-GCMとAES-OFBを通す。選択的暗号文攻撃ができるので任意の暗号化と復号化をしよう。

**writeup**
1. AES-OFB の iv から復号する。