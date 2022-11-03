---
title: "TEE"
---

Trusted Execution Environment: TEE とはプロセッサ上に隔離された実行環境を用意することでセキュリティを高める技術です。
より広義にHIEE: Hardware-assisted Isolated Execution Environment
- Normal Mode
- Secure Mode

TEE の機能
- OSやアプリケーションの改竄を検知
- 公開鍵証明書による端末識別，認証
- ストレージデータの安全な暗号化
- ほぼほぼ暗号化処理をするため, 秘密鍵をそこにppm

## Intel SGX
Enclave では Ring 3 でしか動作しない, つまり syscall が使えない

![[Pasted image 20220927024448.png]]

- [Intel SGX入門 - SGX基礎知識編 - Qiita](https://qiita.com/Cliffford/items/2f155f40a1c3eec288cf)

### Remote Attestation
ECDH
- [Intel SGX - Remote Attestation概説 - Qiita](https://qiita.com/Cliffford/items/095b1df450583b4803f2)

### Foreshadow
CVE-2018-3615 - L1 Terminal Fault: SGX
CVE-2018-3620 - L1 Terminal Fault: OS/SMM、CVE-2018-3646 - L1 Terminal Fault: VMM
L1データキャッシュに存在するデータならなんでも読み取ることが可能

### AEPIC Leak
最初のアーキテクチャ由来のCPUのバグ. インテル製10~12世代のCPUの脆弱性を利用して, プロセッサ本体から機密情報を漏洩させる。APIC MMIOでの未定義範囲のアクセスによりキャッシュ階層から古いデータを参照できる。APIC MMIOのアクセスには管理者権限が必要であるから安全であるが, Intel SGXのような管理者権限を持つ攻撃者からデータを守るようなシステムはリスクとなる。
未初期化メモリの読み取りのようなもの
- [元論文](https://aepicleak.com/aepicleak.pdf)
- https://github.com/IAIK/AEPIC

### Graphene-SGX
- [Graphene-SGX](https://www.usenix.org/system/files/conference/atc17/atc17-tsai.pdf)

## ARM TrustZone
Cortex-A シリーズの拡張機能
ノーマルワールドとセキュアワールドそれぞれに特権/非特権モード
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

### Apple iOS Secure Enclave

## RISC-V Keystone
- BOOM
- Speculative Attack

## OP-TEE
- [OP-TEE/optee_os: Trusted side of the TEE (github.com)](https://github.com/OP-TEE/optee_os)
- [OP-TEE Documentation — OP-TEE documentation documentation (optee.readthedocs.io)](https://optee.readthedocs.io/en/latest/)

須崎先生がよく知ってる

