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
- IV (Initialization Vector)
出力
- 暗号文 $C$
- 認証タグ $T$

バイトを揃える為にゼロパディングが必要となる。あるバイト長 n でゼロパディングするとはビット長 $8n$ の倍数となるように論理左シフトする操作である。
$J_0$ を次のように定義する。
$$
J_0 = \begin{cases}
IV\|0^{31}1 & (\text{IV is 96 bits})\\
GHASH_H(IV) & (\text{IV isn't 96 bits, IV is zero padded as 128-bit block size})
\end{cases}
$$
AES の暗号化と相性をよくする為に入力を 16 バイトごとに切り分ける。それぞれのブロックを $X_i$ とすると出力の列 $Y_i$ は次のような計算で求める。これを GCTR と呼ぶ。
$$
Y_i = E_k(J_0 + i) \oplus X_i \qquad (i = 1,\ldots,n)
$$
暗号文 $C = C_1\|\ldots\|C_n$ は GCTR を用いてこのように計算できる。
$$
C_i = E_k(IV\|i) \oplus P_i \qquad (i = 1,\ldots,n)
$$
これで暗号文を作ることはできた。次に認証タグを計算する。認証タグは正しく暗号化されているか検証するため、また認証情報を入れて正しい情報が送られてきているかを検証するタグである。
認証タグ $T$ はガロア体 $GF(2^{128})$ 上で計算する。
$$
GF(2^{128}) = \mathbb{F}_2[x]/(x^{128} + x^7 + x^2 + x + 1)
$$
$A, C$ は16バイトでゼロパディングしたもの, $\mathrm{len}$ は文字列長を8バイトで出力する関数である。$A\|C\|\mathrm{len}(A)\|\mathrm{len}(C)$ から 16 バイトずつ切り出したものを前から順番に $X_i \quad (i = 1,\ldots,n)$ とすると $Y_0 = 0, H = E_k(0^{128})$ として
$$
Y_i = (X_i + Y_{i-1})\cdot H
$$
と計算する。これを GHASH と呼ぶ。最後にもう一度

$$
Y_i = E_k(J_0 + i-1) \oplus X_i \qquad (i = 1,\ldots,n)
$$

上から平文の長さだけ取ってくると認証タグとなる。

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
