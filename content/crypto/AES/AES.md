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
入力
- 平文 $P$
- 認証データ (AAD; Additional Authenticated Data) $A$
- nonce と呼ばれる IV (Initialization Vector)
出力
- 暗号文 $C$
- 認証タグ $T$

IV は 96bit とする。
暗号文 $C_i$ は次のように計算する。
$$
C_i = E_k(\mathrm{iv}\|i) \oplus P_i \qquad (i = 1,\ldots,n)
$$
認証タグ $T$ はガロア体 $GF(2^{128})$ 上で計算する。
$$
GF(2^{128}) = \mathbb{F}_2[x]/(x^{128} + x^7 + x^2 + x + 1)
$$
$A, C$ は16バイトのゼロパディングしたもの, $\mathrm{len}(S)$ は $S$ の文字列長を8バイトで表したもの。 $A\|C\|\mathrm{len}(A)\|\mathrm{len}(C)$ を 16 バイトごとに切り分けたブロックを $B_i$ とすると$H = E_k(0^{128})$
$$
\begin{aligned}
X_1 & = 0 \\
X_{i+1} & = H\cdot(X_{i} + B_i) \\
T & = X_n + E_k(\mathrm{iv}\|0^{32})
\end{aligned}
$$
と書ける。

### AES-OFB

$$
C_i = E_k^i(\mathrm{iv})\oplus P_i
$$

### 解き方
Padding Oracle Attack を使って暗号/復号化関数 $E_k$ を作る。
すると鍵を考えなくてもいい感じになり、上の式を辿るだけで解けるようになる。
