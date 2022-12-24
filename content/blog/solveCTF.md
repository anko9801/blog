
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
$e' = f(e)$
$f(x) = $
