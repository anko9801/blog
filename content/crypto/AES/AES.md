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

## PKCS #7 パディング
最適化の為にパディングの最も左のバイトが判定の基準となる。逆にそれ以外は参照しない。

```
????????01
??????0202
????030303
...
0f...0f0f0f
??...??????1010...1010
```

padding文字数padding101202023030303::150f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0 (16)10101010101010101010101010101010

### AES-CBC

AES-NI

