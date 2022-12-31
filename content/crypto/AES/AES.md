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

96bit の IV と 32bit の 0 とする。 $J_0 = IV\|0^{31}1$

$$
Y_i = E_k(IV\|i)\oplus X_i
$$
暗号文 $C = C_1\|\ldots\|C_n$ は次のように計算する。これを GCTR と呼ぶ。
$$
C_i = E_k(IV\|i) \oplus P_i \qquad (i = 1,\ldots,n)
$$
認証タグ $T$ はガロア体 $GF(2^{128})$ 上で計算する。
$$
GF(2^{128}) = \mathbb{F}_2[x]/(x^{128} + x^7 + x^2 + x + 1)
$$
$A, C$ は16バイトのゼロパディングしたもの, $\mathrm{len}(S)$ は $S$ の文字列長を8バイトで表したものとして次のように計算する。$A\|C\|\mathrm{len}(A)\|\mathrm{len}(C)$ から 16 バイトずつ切り出したものを前から順番に $X_i \quad (i\in[1,n])$ とすると $Y_0 = 0, H = E_k(0^{128})$ として
$$
Y_i = (X_i + Y_{i-1})\cdot H
$$
と計算する。これを GHASH と呼ぶ。最後に GCTR して上から平文のｎ

![[Pasted image 20221225174723.png]]

### AES-OFB

$$
C_i = E_k^i(\mathrm{iv})\oplus P_i
$$

### 解き方
Padding Oracle Attack を使って暗号/復号化関数 $E_k$ を作る。
すると鍵を考えなくてもいい感じになり、上の式を辿るだけで解けるようになる。


## 参考文献
- [暗号利用モード](https://ja.wikipedia.org/wiki/%E6%9A%97%E5%8F%B7%E5%88%A9%E7%94%A8%E3%83%A2%E3%83%BC%E3%83%89)
- [Recommendation for Block Cipher Modes of Operation: Galois/Counter Mode (GCM) and GMAC](https://nvlpubs.nist.gov/nistpubs/legacy/sp/nistspecialpublication800-38d.pdf)
