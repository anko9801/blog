---
title: "AES"
---

- AES-ECB (Electronic Codeblock)
- AES-CBC (Cipher Block Chaining)
- AES-OFB
- AES-CTR
- PCBC (Propagating Cipher Block Chaining)
- CFB (Cipher Feedback)
- AES-GCM (Galois/Counter Mode)

AES-NI

## PKCS #7 パディング
~~最適化の為にパディングの最も左のバイトが判定の基準となる。逆にそれ以外は参照しない。~~
全部見るらしいので次のようなイディオムを使うとよいかも
```
from pwn import xor
ciphertext = xor(ciphertext[pad:], long_to_bytes(i+1), long_to_bytes(i+2))
```

Padding Oracle Attackでは最後の1文字総当り時に全く同じとき例外として落とす必要がある。

```
????????01
??????0202
????030303
...
0f...0f0f0f
??...??????1010...1010
```

AES オンラインシミュレータほしいかも

### AES-CBC
### AES-GCM
`iv = nonce`

$$
\begin{aligned}
C_i &= E_k(\mathrm{iv}\|i) \oplus P_i \qquad (i = 1,\ldots,n) \\
X_i &= \begin{cases}
H\cdot E_k(0^{128}) & (i = 0) \\
H\cdot(X_{i-1}\oplus A_i) & (i = 1,\ldots,m) \\
H\cdot(X_{i-1}\oplus C_{i-m}) & (i = m+1,\ldots,m+n) \\
H\cdot(X_{i-1}\oplus \mathrm{len}(A)\| \mathrm{len}(C)) & (i = m+n+1) \\
\end{cases}
\end{aligned}
$$

### AES-OFB

$$
C_i = E_k^i(\mathrm{iv})\oplus P_i
$$
