---
title: 🪴 あやめHex
enableToc: true
openToc: true
---

## これはなに？

CTF のライブラリ/知識集らしいです。 (現在は LYT に近い思想で書いてる。)

競プロはライブラリ化されたものがよく出回ってるけど、CTF の方はほとんど閉じた場所でしか共有されてないよな～と思ったのであんこ(将来的に traP の CTF 班)が CTF に関するライブラリや CTF に必要な知識をまとめたものです。編集したい方は大歓迎です！自動 push/pull してるので master に直 push でも良さです。

全く知らない人向けのワークショップを受けた後に、理解することが少し高いハードルとした記事を読み、全体像を把握できたら必要になったときに知識集を辿るような順番で思い描いています。

説明を読めば機能最小限の同じものを自作できること以上、研究的な部分は自分が面白いとか知識が整理されたなとか思えば載せます。
直交性を重視してます。

これらは CC0 ライセンスとします。自由にコピペして OK です！
TODO: ライセンス

## 競プロ
- [テンプレート](./Template.md)
- Python
- Rust

## データ構造
- [UnionFind](./DataStructure/UnionFind/UnionFind.md)
	- [ポテンシャル付きUnionFind](./DataStructure/UnionFind/PotentialUnionFind.md)
	- [永続UnionFind](./DataStructure/UnionFind/PersistUnionFind.md)
- [BIT (Binary-Indexed-Tree) / Fenwick Tree](./DataStructure/BIT/BIT.md)
- Sparse Table
- [セグメント木](./DataStructure/SegTree/SegTree.md)
	- [遅延セグメント木](./DataStructure/SegTree/LazySegTree.md)
	- [Segment Tree Beats](./DataStructure/SegTree/SegTreeBeats.md)
- 平方分割, パケット法
- 平衡二分探索木
	- 赤黒木 RBST
	- AVL木
	- Splay木
	- Treap
- Wavelet Matrix
- Wavelet Tree
- [slope trick](./DataStructure/SlopeTrick.md)
- コンテナ
	- List
	- Map
	- Set
	- Unorderd Map
	- Unordered Set

## グラフ
- [グラフ](./Graph/Graph.md)
	- [最短経路問題](./Graph/ShortestPath/ShortestPath.md)
		- [単一始点最短路 $O(E\log V)$ (Dijkstra)](./Graph/ShortestPath/Dijkstra.md)
			- k-最短路 $O(kE\log V)$ (Dijkstra)
		- [単一始点最短路 $O(EV)$ (Bellman-Ford)](./Graph/ShortestPath/BellmanFord.md)
			- 負閉路検出 $O(EV)$ (Bellman-Ford)
		- [全点対間最短路 $O(V^3)$ (Floyd Warshall)](./Graph/ShortestPath/FloydWarshall.md)
		- 全点対間最短路 $O((V + E)V\log V)$ (Johnson)
	- 閉路検出
	- 全域木
		- 最小全域森 $O(E\log V)$ (Prim)
		- 最小全域森 $O(E\log V)$ (Kruskal)
	- [フロー](./Graph/Flow/Flow.md)
		- [最大流 $O(V^2E)$ (Dinic)](./Graph/Flow/Dinic.md)
		- [最大流 $O(E\times\mathrm{maxFlow})$ (Ford Fulkerson)](./Graph/Flow/FordFulkerson.md)
		- 最小費用流
		- 燃やす埋める
	- [強連結成分分解](./Graph/SCC.md)
	- Dilworth の定理
	- 最大クリーク
	- 最大独立集合
	- Functional Graph
	- トポロジカルソート
	- 2-SAT
- ツリー
	- [木の直径](./graph/tree/diameter.md)
	- 木の重心
	- 重心分解
	- HL分解
	- オイラーツアー
	- 最小共通祖先

## 算数
- Bit Trick
- 数論
	- [拡張 Euclid の互除法](./Arithmetic/gcd.md)
	- [数論的関数/進数変換](./Arithmetic/ArithmeticalFunction.md)
	- 素数
		- [素因数分解](./Arithmetic/Primes/Factorize.md)
		- [高速素因数分解 (Pollard-$\rho$法/Millar-Rabin)](./Arithmetic/Primes/FastFactorize.md)
		- [素数列挙 (エラトステネスの篩)](./Arithmetic/Primes/Primes.md)
		- 素数判定
		- 素数テーブル
	- 剰余
		- [Modint](./Arithmetic/Modulo/Modint.md)
		- [任意Modint](./Arithmetic/Modulo/ArbitraryModint.md)
	- [中国剰余定理](./Arithmetic/CRT.md)
	- floor sum
	- 二分探索
	- 三分探索
- 有理数
- [行列](./Arithmetic/Matrix/Matrix.md)
- [高速フーリエ変換 (FFT)](./Arithmetic/FFT.md)
	- 任意mod畳み込み
	- [高速ゼータ変換/高速メビウス変換](./Arithmetic/Zeta.md)
	- Karatsuba 法
- 形式的冪級数 (FPS)
	- [数論変換 (NTT)](./Arithmetic/NTT.md)
	- 多項式GCD
	- 多項式補間
- スライド最小値

## 探索
- 深さ優先探索
- 幅優先探索
- 座標圧縮
- 枝刈り
- A*
- IDA*
- α-β 探索

## 幾何
- [幾何ライブラリ](./Geometry/Geometry.md)
- 凸包
- 偏角ソート

## 文字列
- Z algorithm
- Rabin-Karp 法
- 最長増加部分列
- ローリングハッシュ
- 接尾辞配列
- Boyer-Moore
- LL(1) parser
- Aho-Corasick

## 典型
- しゃくとり法
- 動的計画法
- 集合の整数表現
- 最小値の最大化
- 半分全列挙
- 行列累乗
- 数え上げ
	- 包除原理
	- 対称性
- Grundy 数

## ヒューリスティック
- [山登り法](./Heuristic/HillClimbing.md)
- [焼きなまし法](./Heuristic/SimulatedAnnealing.md)
- [ビームサーチ](./Heuristic/BeamSearch.md)
- [chokudai サーチ](./Heuristic/ChokudaiSearch.md)

## Pwn
使用言語はPythonまたはC言語です。

- Linux Userland
	- [テンプレート](./pwn/LinuxUserland/template.md)
	- [pwn を解く為に必要なステップ](./pwn/LinuxUserland/method.md)
	- [脆弱性とセキュリティ機構](./pwn/LinuxUserland/vulns&security.md)
	- Stack Exploit
		- [脆弱性とセキュリティ機構](./pwn/LinuxUserland/StackExploit/vulns&security.md)
		- [ret2xxx](./pwn/LinuxUserland/StackExploit/ret2xxx.md)
		- [ROP: Return Oriented Programming](./pwn/LinuxUserland/StackExploit/ROP.md)
	- [Format String Attack](./pwn/LinuxUserland/FormatStringAttack.md)
	- [GOT overwrite](./pwn/LinuxUserland/GOToverwrite.md)
	- [glibc](./pwn/LinuxUserland/glibc/glibc.md)
		- [glibc heap](./pwn/LinuxUserland/glibc/glibc_heap/glibc_heap.md)
			- [malloc_chunk](./pwn/LinuxUserland/glibc/glibc_heap/malloc_chunk.md)
			- [malloc_state](./pwn/LinuxUserland/glibc/glibc_heap/malloc_state.md)
			- [Bins and Chunks](./pwn/LinuxUserland/glibc/glibc_heap/BinsChunks.md)
			- [Internal Functions](./pwn/LinuxUserland/glibc/glibc_heap/InternalFunctions.md)
			- [Core Functions](./pwn/LinuxUserland/glibc/glibc_heap/CoreFunctions.md)
			- [Security Checks](./pwn/LinuxUserland/glibc/glibc_heap/SecurityChecks.md)
		- [`_IO_FILE`](./pwn/LinuxUserland/glibc/_IO_FILE/_IO_FILE.md)
	- [Heap Exploit](./pwn/LinuxUserland/HeapExploit/HeapExploit.md)
		- [脆弱性とセキュリティ機構](./pwn/LinuxUserland/HeapExploit/vulns&security.md)
		- First Fit
		- Double Free
		- Forging chunks
		- Unlink Exploit
		- Shrinking Free Chunks
		- [tcache poisoning](./pwn/LinuxUserland/HeapExploit/tcache_poisoning.md)
		- fastbin attack
		- overlapping chunks
		- mmap overlapping chunks
		- House of XXX
			- [House of Orange](./pwn/LinuxUserland/HeapExploit/HouseOfXXX/HouseOfOrange.md)
			- [House of Botcake](./pwn/LinuxUserland/HeapExploit/HouseOfXXX/HouseOfBotcake.md)
			- [House of Spirit](./pwn/LinuxUserland/HeapExploit/HouseOfXXX/HouseOfSpirit.md)
			- [House of Lore](./pwn/LinuxUserland/HeapExploit/HouseOfXXX/HouseOfLore.md)
			- House of Storm
			- House of Force
- Linux Kernel
	- [テンプレート](KernelExploit.md)
	- Kernel Code Reading
		- セキュリティ機構
		- カーネルモジュール
	- Heap Spray
	- Dirty Pipe
	- Race Condition
- Windows Userland
- Windows Kernel
- VM Escape
	- Container Escape
- [Automatic Exploit Generation](./pwn/AEG.md)

## Crypto
それぞれの暗号自体を取り扱うのではなく、Cryptoの背景にある理論を取り扱っていきます。使用言語はPythonまたはSageMathです。

- [SageMathチートシート](./Crypto/sagemath.md)
- 暗号構成基盤
	- 攻撃者から守る為に
	- [Diffie-Hellman 鍵交換](./Crypto/Cryptography/Diffie-Hellman.md)
	- [Fiat-Shamir 変換](./Crypto/Cryptography/Fiat-Shamir.md)
		- [Schnorr 署名](./Crypto/Cryptography/Schnorr.md)
		- [Frozen Heart](./Crypto/Cryptography/FrozenHeart.md)
	- Lamport 署名
	- [ゼロ知識証明](./Crypto/Cryptography/ZeroKnowledgeProof.md)
	- [Fujisaki-Okamoto Transformation](./Crypto/Cryptography/Fujisaki-Okamoto_Transformation.md)
	- [準同型暗号](./Crypto/Cryptography/homomorphism.md)
- [格子](./Crypto/Lattice/tour_of_Lattice.md)
	- [Gram-Schmidt](./Crypto/Lattice/GSO.md)
	- SVP (Shortest Vector Problem)
		- [Lagrange 基底簡約 (Gauss 基底簡約)](./Crypto/Lattice/Lagrange.md)
		- [サイズ基底簡約](./Crypto/Lattice/size_reduction.md)
		- [LLL 基底簡約](./Crypto/Lattice/LLL.md)
		- BKZ 基底簡約 / HKZ 基底簡約
		- Kannan’s embedding method
	- CVP (Closest Vector Problem)
		- Babai’s Algorithm
	- Merkle-Hellmanナップサック暗号
		- LO法
		- CLOS法
	- LWE (Learning with Errors) 暗号
		- LWE
			- BDD (Bounded Distance Decoding) Attack
			- SIS (Short Integer Solution) Attack
			- BKW Attack
			- Arora-Ge Attack
		- Ring-LWE
		- Module-LWE
			- CRYSTALS
		- LWR
	- [TFHE (Torus Fully Homomorphic Encryption)](./Crypto/Lattice/TFHE.md)
- 多項式
	- [Coppersmith Method](./Crypto/coppersmith.md)
	- グレブナー基底
	- 終結式
	- MQ 問題
	- Matsumoto-Imai 暗号 / HFE 暗号
	- NTRU 暗号
	- Rainbow 署名
	- UOV 署名 / QR-UOV 署名
- 数論
	- ディオファントス方程式
		- 二平方和
		- ペル方程式
	- [離散対数問題 (DLP)](./Crypto/DLP/DLP.md)
		- [Baby-step Giant-step](./Crypto/DLP/BSGS.md)
		- [Pollard's rho 法](./Crypto/DLP/Pollard_rho.md)
		- 指数計算法 (Index Calculus Algorithm)
		- 数体ふるい法
		- [Pohlig-Hellman](./Crypto/DLP/Pohlig_Hellman.md)
- [RSA暗号](./Crypto/RSA/RSA.md)
	- [Wiener's Attack](./Crypto/RSA/WienersAttack.md)
	- [Boneh-Durfee Attack](./Crypto/RSA/Boneh-DurfeeAttack.md)
	- [Common Modulus Attack](./Crypto/RSA/CommonModulusAttack.md)
	- [Håstad's Broadcast Attack](./Crypto/RSA/HåstadsBroadcastAttack.md)
	- [Small Common Private Exponent Attack](./Crypto/RSA/SmallCommonPrivateExponentAttack.md)
	- [適応的選択暗号文攻撃](./Crypto/RSA/RSA-CCA.md)
	- [LSB Decryption Oracle Attack](./Crypto/RSA/LSB-DecryptionOracleAttack.md)
	- [RSA-CRT Fault Attack](./Crypto/RSA/RSA-CRT-FaultAttack.md)
	- [Franklin-Reiter Related Message Attack](./Crypto/RSA/Franklin-ReiterRelatedMessageAttack.md)
	- [Partial Key Exposure Attack](./Crypto/RSA/PartialKeyExposureAttack.md)
	- [逆元が存在しないとき](./Crypto/RSA/NoInverse.md)
	- [ROCA Attack](./Crypto/RSA/ROCA.md)
- [楕円曲線暗号](./Crypto/ECC/ECC.md)
	- 楕円曲線
		- Millar のアルゴリズム
		- [Schoof のアルゴリズム](./Crypto/ECC/Schoof.md)
		- Tate pairing / Weil pairing
		- [ECFFT](./Crypto/ECC/ECFFT.md)
		- 超楕円曲線
	- 攻撃
		- [Pohlig-Hellman Attack](./Crypto/ECC/Pohlig-Hellman.md)
		- [MOV/FR Reduction](./Crypto/ECC/MOV-FR-Reduction.md)
		- [SSSA Attack](./Crypto/ECC/SSSA-Attack.md)
		- [Invalid Curve Attack](./Crypto/ECC/Invalid-Curve-Attack.md)
		- [GHS Attack](./Crypto/ECC/GHS-Attack.md)
		- Dual EC DRBG
		- [Attacks on SIKE](./Crypto/ECC/SIKE.md)
- [AES](./Crypto/AES/AES.md)
	- Padding Oracle Attack
	- BEAST Attack
	- Lucky Thirteen Attack
	- POODLE Attack
	- ghash
	- Integral Cryptanalysis
- [その他の暗号](./Crypto/Cryptography/other.md)
- [Hash](./Crypto/Hash/hash.md)
	- 誕生日攻撃
	- [Differencial cryptanalysis](./Crypto/Hash/DifferencialCryptoanalysis.md)
- 疑似乱数生成器 (PRNG)
	- Xorshift
	- [Mersenne twister](./Crypto/PRNG/MersenneTwister.md)
- ブロックチェーン
	- Flash Loan Attack
- [参考文献](./Crypto/Books.md)

## Web
Webに関してはよわよわなので読み込むと良いかもしれない資料リストを集めています。(これ読むといいよみたいなのがあったら教えてくださると助かります！)

- [Prototype Pollution](PrototypePollution.md)
- [CTFにおけるWebセキュリティ入門とまとめ](https://blog.hamayanhamayan.com/entry/2021/12/01/194114)
- 常設Web問
	- [Web Security Academy](https://portswigger.net/web-security/all-labs)
	- [KENRO](https://kenro.flatt.tech)
	- [wargame.kr](http://wargame.kr)
	- [XSS Game](https://xss-game.appspot.com)
	- [The Lord of the SQLI](https://los.rubiya.kr)
- [SQL Injection list](https://github.com/payloadbox/sql-injection-payload-list)

## Misc
CTF の3大分野以外をまとめます.

- [Forensics](./Misc/Forensics/Forensics.md)
	- [Windows](./Misc/Forensics/Windows.md)
- [OSINT](./Misc/OSINT.md)
- Reversing
	- [表層解析](./Reversing/SurfaceAnalysis.md)
	- 静的解析
		- Ghidra
	- 動的解析
		- Fuzzing
		- [シンボリック実行](./other/SAT-SMT/SymbolicExecution.md)
	- [マルウェア](./Reversing/Malware/Malware.md)
		- [デバッグ検知](./Reversing/Malware/AntiDebugging.md)
		- [仮想化検知](./Reversing/Malware/DetectVirtualization.md)
		- [Windowsの内部](./Reversing/Malware/WindowsInternal.md)
- バイナリフォーマット探検隊
	- [ASCII](./Misc/BinaryFormat/ASCII.md)
	- [Unicode](./Misc/BinaryFormat/Unicode.md)
	- [ELF](./Misc/BinaryFormat/ELF.md)
	- [FAT32](./Misc/BinaryFormat/FAT32.md)
	- ext4
	- [ZIP](./Misc/BinaryFormat/ZIP.md)
	- JPEG
- ピッキング
- Tamper Evident
- Social Engineering
- Car Hacking
- [Pyjail](./Misc/Misc/Pyjail.md)

## コンピュータ・アーキテクチャ
- [数学](./Science/Math/Math.md)
	- [命題論理](./Science/Math/PropositionalLogic.md)
	- [意味論](./Science/Math/Semantics.md)
- [物理](./Science/Physics/Physics.md)
	- 航空技術
- [回路](./other/Circuit/Circuit)
	- 回路素子
	- アナログ回路
		- 抵抗とコンデンサ
		- 昇圧チョッパ回路
		- マルチバイブレータ回路
		- 発振回路
			- コルピッツ発振回路
		- 電源
		- パワエレ
		- ノイズキャンセリング
		- センサー・アクチュエータ
			- モーター
			- Hブリッジ回路
			- PWM制御
			- ゲートドライバ
				- FETのオン抵抗, デッドタイム、昇圧回路
			- TA7291P, TB6643KQ, L298N
	- デジタル回路
		- 遅延回路 + シュミットトリガ
		- キーボード
		- AVLライタ
		- 論理回路
			- ラッチ
			- フリップフロップ
		- [マイコン](./other/Circuit/Digital/Microcomputer.md)
		- 通信
- [プロセッサ](./other/Processor/Processor.md)
	- [LSI](./other/Processor/LSI)
	- Spectre / Meltdown
	- HIEE; Hardware-assisted Isolated Execution Environments
		- [TEE; Trusted Execution Environment](./other/Processor/HIEE/TEE.md)
		- [TPM; Trusted Platform Module](./other/Processor/HIEE/TPM.md)
		- [DRM; Digital Rights Management](./other/Processor/HIEE/DRM.md)
		- DAA; Direct Anonymous Attestation
	- [rootkit](./other/Processor/rootkit.md)
- 論理
- マイクロアーキテクチャ
- アーキテクチャ
- 仮想化技術
	- [エミュレータ](./other/Virtualization/Emulator.md)
	- [コンテナ仮想化技術](./other/Virtualization/Container.md)
	- [ハイパーバイザの作り方](https://syuu1228.github.io/howto_implement_hypervisor/)
- [OS](./other/OS/OS.md)
	- [略語](./other/OS/abbreviation)
	- [スケジューラ](./other/OS/Scheduler)
	- BIOS/UEFI
	- ACPI
	- NIC
- サーバー
	- [RDBMS](RDBMS.md)
	- [RDBMS 最適化](rdbms-optimization.md)
	- [リバースプロキシ](reverse-proxy.md)
	- [リバースプロキシ最適化](reverse-proxy-optimization.md)
	- [フロントエンド最適化](frontend-optimization.md)
- [ネットワーク](./other/Network/Network.md)
	- [SDR](./other/Network/SDR.md)

## 量子アルゴリズム

- [各ゲートの紹介と量子計算の方法](./other/Quantum/Quantum.md)
- Shor のアルゴリズム
- 量子暗号通信
- 量子中継ネットワーク

## 雑学

- 生物学
- [プログラミング言語](./other/Programming/Programming.md)
	- [型推論](./other/Programming/Type.md)
	- [toolchain](./other/Programming/toolchain)
	- 未定義動作
	- シェル
	- 定理証明支援系
- [SAT/SMT](./other/SAT-SMT/SAT-SMT.md)
	- [シンボリック実行エンジン](./other/SAT-SMT/SymbolicExecution.md)
	- 自動定理証明支援系
- レンダリング
	- [レイトレーシング](./other/Rendering/RayTracing.md)
- ブロックチェーン
- [Deep Learning](DeepLearning.md)
- [【画像処理入門】アルゴリズム＆プログラミング](https://algorithm.joho.info/programming/image-processing/)
- 超解像
- [ツール](./other/Tools)

## 脆弱性集

- [CVEs for the Rust standard library](https://rustrepo.com/repo/Qwaz-rust-cve-rust-security-tools)
	- [Rustのunsound hole issue #25860を理解する](https://speakerdeck.com/moratorium08/rustfalseunsound-hole-issue-number-25860woli-jie-suru)
	- [str::repeat - stable wildcopy exploit](https://saaramar.github.io/str_repeat_exploit/)