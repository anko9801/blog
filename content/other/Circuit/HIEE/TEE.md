---
title: "TEE"
---

Trusted Execution Environment: TEE とはプロセッサ上に隔離された実行環境を用意することでセキュリティを高める技術です。
- Normal Mode
- Secure Mode

- OSやアプリケーションの改竄を検知
- 公開鍵証明書による端末識別，認証
- ストレージデータの安全な暗号化
- ほぼほぼ暗号化処理をするため, 秘密鍵をそこにppm

## Intel SGX
- Remote Attestation
- ForeShadow
- [IAIK/AEPIC (github.com)](https://github.com/IAIK/AEPIC)
[Intel SGX - Remote Attestation概説 - Qiita](https://qiita.com/Cliffford/items/095b1df450583b4803f2)

## ARM TrustZone
- [Boomerang](https://github.com/ucsb-seclab/boomerang/)
- [Abusing Trust: Mobile Kernel Subversion via TrustZone Rootkits](https://security.inso.tuwien.ac.at/pdfs/woot22-preprint.pdf)
- [SoK: Understanding the Prevailing Security Vulnerabilities in TrustZone-assisted TEE Systems | IEEE Conference Publication | IEEE Xplore](https://ieeexplore.ieee.org/document/9152801)
- [Project Zero: Trust Issues: Exploiting TrustZone TEEs (googleprojectzero.blogspot.com)](https://googleprojectzero.blogspot.com/2017/07/trust-issues-exploiting-trustzone-tees.html)
Project ZeroはSpectre見つけた
- [ARM TrustZone エクスプロイト入門 - Speaker Deck](https://speakerdeck.com/rkx1209/arm-trustzone-ekusupuroitoru-men)

### Android KeyStore
Android の SoC には ARM TrustZone があり、その上で KeyStore を動かしている.
Android の Full Disk Encryption を TEE を介さずに Decrypt.  FBIでもできなかった
- [Bits, Please!: Extracting Qualcomm's KeyMaster Keys - Breaking Android Full Disk Encryption (bits-please.blogspot.com)](http://bits-please.blogspot.com/2016/06/extracting-qualcomms-keymaster-keys.html)

## RISC-V Keystone
- BOOM
- Speculative Attack

## OP-TEE
- [OP-TEE/optee_os: Trusted side of the TEE (github.com)](https://github.com/OP-TEE/optee_os)
- [OP-TEE Documentation — OP-TEE documentation documentation (optee.readthedocs.io)](https://optee.readthedocs.io/en/latest/)

須崎先生がよく知ってる

