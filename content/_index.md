---
title: ðª´ ãããHex
enableToc: true
openToc: true
---

## ããã¯ãªã«ï¼

CTF ã®ã©ã¤ãã©ãª/ç¥è­éãããã§ãã (ç¾å¨ã¯ LYT ã«è¿ãææ³ã§æ¸ãã¦ãã)

ç«¶ãã­ã¯ã©ã¤ãã©ãªåããããã®ãããåºåã£ã¦ããã©ãCTF ã®æ¹ã¯ã»ã¨ãã©éããå ´æã§ããå±æããã¦ãªãããªï½ã¨æã£ãã®ã§ããã(å°æ¥çã« traP ã® CTF ç­)ã CTF ã«é¢ããã©ã¤ãã©ãªã CTF ã«å¿è¦ãªç¥è­ãã¾ã¨ãããã®ã§ããç·¨éãããæ¹ã¯å¤§æ­è¿ã§ãï¼èªå push/pull ãã¦ãã®ã§ master ã«ç´ push ã§ãè¯ãã§ãã

å¨ãç¥ããªãäººåãã®ã¯ã¼ã¯ã·ã§ãããåããå¾ã«ãçè§£ãããã¨ãå°ãé«ããã¼ãã«ã¨ããè¨äºãèª­ã¿ãå¨ä½åãææ¡ã§ãããå¿è¦ã«ãªã£ãã¨ãã«ç¥è­éãè¾¿ããããªé çªã§æãæãã¦ãã¾ãã

èª¬æãèª­ãã°æ©è½æå°éã®åããã®ãèªä½ã§ãããã¨ä»¥ä¸ãç ç©¶çãªé¨åã¯èªåãé¢ç½ãã¨ãç¥è­ãæ´çããããªã¨ãæãã°è¼ãã¾ãã
ç´äº¤æ§ãéè¦ãã¦ã¾ãã

ãããã¯ CC0 ã©ã¤ã»ã³ã¹ã¨ãã¾ããèªç±ã«ã³ãããã¦ OK ã§ãï¼
TODO: ã©ã¤ã»ã³ã¹

## ç«¶ãã­
- [ãã³ãã¬ã¼ã](./Template.md)
- Python
- Rust

## ãã¼ã¿æ§é 
- [UnionFind](./DataStructure/UnionFind/UnionFind.md)
	- [ããã³ã·ã£ã«ä»ãUnionFind](./DataStructure/UnionFind/PotentialUnionFind.md)
	- [æ°¸ç¶UnionFind](./DataStructure/UnionFind/PersistUnionFind.md)
- [BIT (Binary-Indexed-Tree) / Fenwick Tree](./DataStructure/BIT/BIT.md)
- Sparse Table
- [ã»ã°ã¡ã³ãæ¨](./DataStructure/SegTree/SegTree.md)
	- [éå»¶ã»ã°ã¡ã³ãæ¨](./DataStructure/SegTree/LazySegTree.md)
	- [Segment Tree Beats](./DataStructure/SegTree/SegTreeBeats.md)
- å¹³æ¹åå², ãã±ããæ³
- å¹³è¡¡äºåæ¢ç´¢æ¨
	- èµ¤é»æ¨ RBST
	- AVLæ¨
	- Splayæ¨
	- Treap
- Wavelet Matrix
- Wavelet Tree
- [slope trick](./DataStructure/SlopeTrick.md)
- ã³ã³ãã
	- List
	- Map
	- Set
	- Unorderd Map
	- Unordered Set

## ã°ã©ã
- [ã°ã©ã](./Graph/Graph.md)
	- [æç­çµè·¯åé¡](./Graph/ShortestPath/ShortestPath.md)
		- [åä¸å§ç¹æç­è·¯ $O(E\log V)$ (Dijkstra)](./Graph/ShortestPath/Dijkstra.md)
			- k-æç­è·¯ $O(kE\log V)$ (Dijkstra)
		- [åä¸å§ç¹æç­è·¯ $O(EV)$ (Bellman-Ford)](./Graph/ShortestPath/BellmanFord.md)
			- è² éè·¯æ¤åº $O(EV)$ (Bellman-Ford)
		- [å¨ç¹å¯¾éæç­è·¯ $O(V^3)$ (Floyd Warshall)](./Graph/ShortestPath/FloydWarshall.md)
		- å¨ç¹å¯¾éæç­è·¯ $O((V + E)V\log V)$ (Johnson)
	- éè·¯æ¤åº
	- å¨åæ¨
		- æå°å¨åæ£® $O(E\log V)$ (Prim)
		- æå°å¨åæ£® $O(E\log V)$ (Kruskal)
	- [ãã­ã¼](./Graph/Flow/Flow.md)
		- [æå¤§æµ $O(V^2E)$ (Dinic)](./Graph/Flow/Dinic.md)
		- [æå¤§æµ $O(E\times\mathrm{maxFlow})$ (Ford Fulkerson)](./Graph/Flow/FordFulkerson.md)
		- æå°è²»ç¨æµ
		- çããåãã
	- [å¼·é£çµæååè§£](./Graph/SCC.md)
	- Dilworth ã®å®ç
	- æå¤§ã¯ãªã¼ã¯
	- æå¤§ç¬ç«éå
	- Functional Graph
	- ããã­ã¸ã«ã«ã½ã¼ã
	- 2-SAT
- ããªã¼
	- [æ¨ã®ç´å¾](./graph/tree/diameter.md)
	- æ¨ã®éå¿
	- éå¿åè§£
	- HLåè§£
	- ãªã¤ã©ã¼ãã¢ã¼
	- æå°å±éç¥å

## ç®æ°
- Bit Trick
- æ°è«
	- [æ¡å¼µ Euclid ã®äºé¤æ³](./Arithmetic/gcd.md)
	- [æ°è«çé¢æ°/é²æ°å¤æ](./Arithmetic/ArithmeticalFunction.md)
	- ç´ æ°
		- [ç´ å æ°åè§£](./Arithmetic/Primes/Factorize.md)
		- [é«éç´ å æ°åè§£ (Pollard-$\rho$æ³/Millar-Rabin)](./Arithmetic/Primes/FastFactorize.md)
		- [ç´ æ°åæ (ã¨ã©ãã¹ããã¹ã®ç¯©)](./Arithmetic/Primes/Primes.md)
		- ç´ æ°å¤å®
		- ç´ æ°ãã¼ãã«
	- å°ä½
		- [Modint](./Arithmetic/Modulo/Modint.md)
		- [ä»»æModint](./Arithmetic/Modulo/ArbitraryModint.md)
	- [ä¸­å½å°ä½å®ç](./Arithmetic/CRT.md)
	- floor sum
	- äºåæ¢ç´¢
	- ä¸åæ¢ç´¢
- æçæ°
- [è¡å](./Arithmetic/Matrix/Matrix.md)
- [é«éãã¼ãªã¨å¤æ (FFT)](./Arithmetic/FFT.md)
	- ä»»æmodç³ã¿è¾¼ã¿
	- [é«éã¼ã¼ã¿å¤æ/é«éã¡ãã¦ã¹å¤æ](./Arithmetic/Zeta.md)
	- Karatsuba æ³
- å½¢å¼çåªç´æ° (FPS)
	- [æ°è«å¤æ (NTT)](./Arithmetic/NTT.md)
	- å¤é å¼GCD
	- å¤é å¼è£é
- ã¹ã©ã¤ãæå°å¤

## æ¢ç´¢
- æ·±ãåªåæ¢ç´¢
- å¹åªåæ¢ç´¢
- åº§æ¨å§ç¸®
- æåã
- A*
- IDA*
- Î±-Î² æ¢ç´¢

## å¹¾ä½
- [å¹¾ä½ã©ã¤ãã©ãª](./Geometry/Geometry.md)
- å¸å
- åè§ã½ã¼ã

## æå­å
- Z algorithm
- Rabin-Karp æ³
- æé·å¢å é¨åå
- ã­ã¼ãªã³ã°ããã·ã¥
- æ¥å°¾è¾éå
- Boyer-Moore
- LL(1) parser
- Aho-Corasick

## å¸å
- ãããã¨ãæ³
- åçè¨ç»æ³
- éåã®æ´æ°è¡¨ç¾
- æå°å¤ã®æå¤§å
- ååå¨åæ
- è¡åç´¯ä¹
- æ°ãä¸ã
	- åé¤åç
	- å¯¾ç§°æ§
- Grundy æ°

## ãã¥ã¼ãªã¹ãã£ãã¯
- [å±±ç»ãæ³](./Heuristic/HillClimbing.md)
- [ç¼ããªã¾ãæ³](./Heuristic/SimulatedAnnealing.md)
- [ãã¼ã ãµã¼ã](./Heuristic/BeamSearch.md)
- [chokudai ãµã¼ã](./Heuristic/ChokudaiSearch.md)

## Pwn
ä½¿ç¨è¨èªã¯Pythonã¾ãã¯Cè¨èªã§ãã

- Linux Userland
	- [ãã³ãã¬ã¼ã](./pwn/LinuxUserland/template.md)
	- [pwn ãè§£ãçºã«å¿è¦ãªã¹ããã](./pwn/LinuxUserland/method.md)
	- [èå¼±æ§ã¨ã»ã­ã¥ãªãã£æ©æ§](./pwn/LinuxUserland/vulns&security.md)
	- Stack Exploit
		- [èå¼±æ§ã¨ã»ã­ã¥ãªãã£æ©æ§](./pwn/LinuxUserland/StackExploit/vulns&security.md)
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
		- [èå¼±æ§ã¨ã»ã­ã¥ãªãã£æ©æ§](./pwn/LinuxUserland/HeapExploit/vulns&security.md)
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
	- [ãã³ãã¬ã¼ã](KernelExploit.md)
	- Kernel Code Reading
		- ã»ã­ã¥ãªãã£æ©æ§
		- ã«ã¼ãã«ã¢ã¸ã¥ã¼ã«
	- Heap Spray
	- Dirty Pipe
	- Race Condition
- Windows Userland
- Windows Kernel
- VM Escape
	- Container Escape
- [Automatic Exploit Generation](./pwn/AEG.md)

## Crypto
ããããã®æå·èªä½ãåãæ±ãã®ã§ã¯ãªããCryptoã®èæ¯ã«ããçè«ãåãæ±ã£ã¦ããã¾ããä½¿ç¨è¨èªã¯Pythonã¾ãã¯SageMathã§ãã

- [SageMathãã¼ãã·ã¼ã](./Crypto/sagemath.md)
- æå·æ§æåºç¤
	- æ»æèããå®ãçºã«
	- [Diffie-Hellman éµäº¤æ](./Crypto/Cryptography/Diffie-Hellman.md)
	- [Fiat-Shamir å¤æ](./Crypto/Cryptography/Fiat-Shamir.md)
		- [Schnorr ç½²å](./Crypto/Cryptography/Schnorr.md)
		- [Frozen Heart](./Crypto/Cryptography/FrozenHeart.md)
	- Lamport ç½²å
	- [ã¼ã­ç¥è­è¨¼æ](./Crypto/Cryptography/ZeroKnowledgeProof.md)
	- [Fujisaki-Okamoto Transformation](./Crypto/Cryptography/Fujisaki-Okamoto_Transformation.md)
	- [æºååæå·](./Crypto/Cryptography/homomorphism.md)
- [æ ¼å­](./Crypto/Lattice/tour_of_Lattice.md)
	- [Gram-Schmidt](./Crypto/Lattice/GSO.md)
	- SVP (Shortest Vector Problem)
		- [Lagrange åºåºç°¡ç´ (Gauss åºåºç°¡ç´)](./Crypto/Lattice/Lagrange.md)
		- [ãµã¤ãºåºåºç°¡ç´](./Crypto/Lattice/size_reduction.md)
		- [LLL åºåºç°¡ç´](./Crypto/Lattice/LLL.md)
		- BKZ åºåºç°¡ç´ / HKZ åºåºç°¡ç´
		- Kannanâs embedding method
	- CVP (Closest Vector Problem)
		- Babaiâs Algorithm
	- Merkle-Hellmanããããµãã¯æå·
		- LOæ³
		- CLOSæ³
	- LWE (Learning with Errors) æå·
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
- å¤é å¼
	- [Coppersmith Method](./Crypto/coppersmith.md)
	- ã°ã¬ããã¼åºåº
	- çµçµå¼
	- MQ åé¡
	- Matsumoto-Imai æå· / HFE æå·
	- NTRU æå·
	- Rainbow ç½²å
	- UOV ç½²å / QR-UOV ç½²å
- æ°è«
	- ãã£ãªãã¡ã³ãã¹æ¹ç¨å¼
		- äºå¹³æ¹å
		- ãã«æ¹ç¨å¼
	- [é¢æ£å¯¾æ°åé¡ (DLP)](./Crypto/DLP/DLP.md)
		- [Baby-step Giant-step](./Crypto/DLP/BSGS.md)
		- [Pollard's rho æ³](./Crypto/DLP/Pollard_rho.md)
		- ææ°è¨ç®æ³ (Index Calculus Algorithm)
		- æ°ä½ãµããæ³
		- [Pohlig-Hellman](./Crypto/DLP/Pohlig_Hellman.md)
- [RSAæå·](./Crypto/RSA/RSA.md)
	- [Wiener's Attack](./Crypto/RSA/WienersAttack.md)
	- [Boneh-Durfee Attack](./Crypto/RSA/Boneh-DurfeeAttack.md)
	- [Common Modulus Attack](./Crypto/RSA/CommonModulusAttack.md)
	- [HÃ¥stad's Broadcast Attack](./Crypto/RSA/HÃ¥stadsBroadcastAttack.md)
	- [Small Common Private Exponent Attack](./Crypto/RSA/SmallCommonPrivateExponentAttack.md)
	- [é©å¿çé¸ææå·ææ»æ](./Crypto/RSA/RSA-CCA.md)
	- [LSB Decryption Oracle Attack](./Crypto/RSA/LSB-DecryptionOracleAttack.md)
	- [RSA-CRT Fault Attack](./Crypto/RSA/RSA-CRT-FaultAttack.md)
	- [Franklin-Reiter Related Message Attack](./Crypto/RSA/Franklin-ReiterRelatedMessageAttack.md)
	- [Partial Key Exposure Attack](./Crypto/RSA/PartialKeyExposureAttack.md)
	- [éåãå­å¨ããªãã¨ã](./Crypto/RSA/NoInverse.md)
	- [ROCA Attack](./Crypto/RSA/ROCA.md)
- [æ¥åæ²ç·æå·](./Crypto/ECC/ECC.md)
	- æ¥åæ²ç·
		- Millar ã®ã¢ã«ã´ãªãºã 
		- [Schoof ã®ã¢ã«ã´ãªãºã ](./Crypto/ECC/Schoof.md)
		- Tate pairing / Weil pairing
		- [ECFFT](./Crypto/ECC/ECFFT.md)
		- è¶æ¥åæ²ç·
	- æ»æ
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
- [ãã®ä»ã®æå·](./Crypto/Cryptography/other.md)
- [Hash](./Crypto/Hash/hash.md)
	- èªçæ¥æ»æ
	- [Differencial cryptanalysis](./Crypto/Hash/DifferencialCryptoanalysis.md)
- çä¼¼ä¹±æ°çæå¨ (PRNG)
	- Xorshift
	- [Mersenne twister](./Crypto/PRNG/MersenneTwister.md)
- ãã­ãã¯ãã§ã¼ã³
	- Flash Loan Attack
- [åèæç®](./Crypto/Books.md)

## Web
Webã«é¢ãã¦ã¯ãããããªã®ã§èª­ã¿è¾¼ãã¨è¯ããããããªãè³æãªã¹ããéãã¦ãã¾ãã(ããèª­ãã¨ãããã¿ãããªã®ããã£ããæãã¦ãã ããã¨å©ããã¾ãï¼)

- [Prototype Pollution](PrototypePollution.md)
- [CTFã«ãããWebã»ã­ã¥ãªãã£å¥éã¨ã¾ã¨ã](https://blog.hamayanhamayan.com/entry/2021/12/01/194114)
- å¸¸è¨­Webå
	- [Web Security Academy](https://portswigger.net/web-security/all-labs)
	- [KENRO](https://kenro.flatt.tech)
	- [wargame.kr](http://wargame.kr)
	- [XSS Game](https://xss-game.appspot.com)
	- [The Lord of the SQLI](https://los.rubiya.kr)
- [SQL Injection list](https://github.com/payloadbox/sql-injection-payload-list)

## Misc
CTF ã®3å¤§åéä»¥å¤ãã¾ã¨ãã¾ã.

- [Forensics](./Misc/Forensics/Forensics.md)
	- [Windows](./Misc/Forensics/Windows.md)
- [OSINT](./Misc/OSINT.md)
- Reversing
	- [è¡¨å±¤è§£æ](./Reversing/SurfaceAnalysis.md)
	- éçè§£æ
		- Ghidra
	- åçè§£æ
		- Fuzzing
		- [ã·ã³ããªãã¯å®è¡](./other/SAT-SMT/SymbolicExecution.md)
	- [ãã«ã¦ã§ã¢](./Reversing/Malware/Malware.md)
		- [ãããã°æ¤ç¥](./Reversing/Malware/AntiDebugging.md)
		- [ä»®æ³åæ¤ç¥](./Reversing/Malware/DetectVirtualization.md)
		- [Windowsã®åé¨](./Reversing/Malware/WindowsInternal.md)
- ãã¤ããªãã©ã¼ãããæ¢æ¤é
	- [ASCII](./Misc/BinaryFormat/ASCII.md)
	- [Unicode](./Misc/BinaryFormat/Unicode.md)
	- [ELF](./Misc/BinaryFormat/ELF.md)
	- [FAT32](./Misc/BinaryFormat/FAT32.md)
	- ext4
	- [ZIP](./Misc/BinaryFormat/ZIP.md)
	- JPEG
- ããã­ã³ã°
- Tamper Evident
- Social Engineering
- Car Hacking
- [Pyjail](./Misc/Misc/Pyjail.md)

## ã³ã³ãã¥ã¼ã¿ã»ã¢ã¼ã­ãã¯ãã£
- [æ°å­¦](./Science/Math/Math.md)
	- [å½é¡è«ç](./Science/Math/PropositionalLogic.md)
	- [æå³è«](./Science/Math/Semantics.md)
- [ç©ç](./Science/Physics/Physics.md)
	- èªç©ºæè¡
- [åè·¯](./other/Circuit/Circuit)
	- åè·¯ç´ å­
	- ã¢ãã­ã°åè·¯
		- æµæã¨ã³ã³ãã³ãµ
		- æå§ãã§ããåè·¯
		- ãã«ããã¤ãã¬ã¼ã¿åè·¯
		- çºæ¯åè·¯
			- ã³ã«ãããçºæ¯åè·¯
		- é»æº
		- ãã¯ã¨ã¬
		- ãã¤ãºã­ã£ã³ã»ãªã³ã°
		- ã»ã³ãµã¼ã»ã¢ã¯ãã¥ã¨ã¼ã¿
			- ã¢ã¼ã¿ã¼
			- Hããªãã¸åè·¯
			- PWMå¶å¾¡
			- ã²ã¼ããã©ã¤ã
				- FETã®ãªã³æµæ, ãããã¿ã¤ã ãæå§åè·¯
			- TA7291P, TB6643KQ, L298N
	- ãã¸ã¿ã«åè·¯
		- éå»¶åè·¯ + ã·ã¥ãããããªã¬
		- ã­ã¼ãã¼ã
		- AVLã©ã¤ã¿
		- è«çåè·¯
			- ã©ãã
			- ããªãããã­ãã
		- [ãã¤ã³ã³](./other/Circuit/Digital/Microcomputer.md)
		- éä¿¡
- [ãã­ã»ããµ](./other/Processor/Processor.md)
	- [LSI](./other/Processor/LSI)
	- Spectre / Meltdown
	- HIEE; Hardware-assisted Isolated Execution Environments
		- [TEE; Trusted Execution Environment](./other/Processor/HIEE/TEE.md)
		- [TPM; Trusted Platform Module](./other/Processor/HIEE/TPM.md)
		- [DRM; Digital Rights Management](./other/Processor/HIEE/DRM.md)
		- DAA; Direct Anonymous Attestation
	- [rootkit](./other/Processor/rootkit.md)
- è«ç
- ãã¤ã¯ã­ã¢ã¼ã­ãã¯ãã£
- ã¢ã¼ã­ãã¯ãã£
- ä»®æ³åæè¡
	- [ã¨ãã¥ã¬ã¼ã¿](./other/Virtualization/Emulator.md)
	- [ã³ã³ããä»®æ³åæè¡](./other/Virtualization/Container.md)
	- [ãã¤ãã¼ãã¤ã¶ã®ä½ãæ¹](https://syuu1228.github.io/howto_implement_hypervisor/)
- [OS](./other/OS/OS.md)
	- [ç¥èª](./other/OS/abbreviation)
	- [ã¹ã±ã¸ã¥ã¼ã©](./other/OS/Scheduler)
	- BIOS/UEFI
	- ACPI
	- NIC
- ãµã¼ãã¼
	- [RDBMS](RDBMS.md)
	- [RDBMS æé©å](rdbms-optimization.md)
	- [ãªãã¼ã¹ãã­ã­ã·](reverse-proxy.md)
	- [ãªãã¼ã¹ãã­ã­ã·æé©å](reverse-proxy-optimization.md)
	- [ãã­ã³ãã¨ã³ãæé©å](frontend-optimization.md)
- [ãããã¯ã¼ã¯](./other/Network/Network.md)
	- [SDR](./other/Network/SDR.md)

## éå­ã¢ã«ã´ãªãºã 

- [åã²ã¼ãã®ç´¹ä»ã¨éå­è¨ç®ã®æ¹æ³](./other/Quantum/Quantum.md)
- Shor ã®ã¢ã«ã´ãªãºã 
- éå­æå·éä¿¡
- éå­ä¸­ç¶ãããã¯ã¼ã¯

## éå­¦

- çç©å­¦
- [ãã­ã°ã©ãã³ã°è¨èª](./other/Programming/Programming.md)
	- [åæ¨è«](./other/Programming/Type.md)
	- [toolchain](./other/Programming/toolchain)
	- æªå®ç¾©åä½
	- ã·ã§ã«
	- å®çè¨¼ææ¯æ´ç³»
- [SAT/SMT](./other/SAT-SMT/SAT-SMT.md)
	- [ã·ã³ããªãã¯å®è¡ã¨ã³ã¸ã³](./other/SAT-SMT/SymbolicExecution.md)
	- èªåå®çè¨¼ææ¯æ´ç³»
- ã¬ã³ããªã³ã°
	- [ã¬ã¤ãã¬ã¼ã·ã³ã°](./other/Rendering/RayTracing.md)
- ãã­ãã¯ãã§ã¼ã³
- [Deep Learning](DeepLearning.md)
- [ãç»åå¦çå¥éãã¢ã«ã´ãªãºã ï¼ãã­ã°ã©ãã³ã°](https://algorithm.joho.info/programming/image-processing/)
- è¶è§£å
- [ãã¼ã«](./other/Tools)

## èå¼±æ§é

- [CVEs for the Rust standard library](https://rustrepo.com/repo/Qwaz-rust-cve-rust-security-tools)
	- [Rustã®unsound hole issue #25860ãçè§£ãã](https://speakerdeck.com/moratorium08/rustfalseunsound-hole-issue-number-25860woli-jie-suru)
	- [str::repeat - stable wildcopy exploit](https://saaramar.github.io/str_repeat_exploit/)