
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
1. $\gcd(c_1 - c_2, n) = pq$ より $pq, r$ に分解.
2. $\gcd(c_1, c_2) = p - q$ より $p, q$ に分解.

### [Crypto] BBB

1. FLAG