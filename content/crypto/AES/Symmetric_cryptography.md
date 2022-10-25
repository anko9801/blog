最新の状況はこの発表を見ればすべてわかる
[Invited talk: Symmetric Cryptography for Long Term Security, by Maria Naya Plasencia - YouTube](https://www.youtube.com/watch?v=kcaPL2xxXiY)
軽量な
セキュリティ vs. 効率
応用に合わせた性質
- ZK-friendly
- FHE-friendly
- low latency
- easy-masking
- quantum safe

最近の大きな研究10選
- SHA-1 collision and Chosen-prefix collision
- Practical attacks on 15-year old operating mode and ISO standard OCB2
- Attacks on $\approx2^{40}$ on GEA-1(back door) and GEA-2
- Division property: Generalization of integrals, higher order diff.
	- Can be combined with cube attacks
	- Provides best known attacks on several stream ciphers
	- First full break of MISTY1, a widely implemented primitive
- Partial non-linear layers: Zorro, LowMC attacks, Malicious, Hades/Poseidon
- Stream cipher based: Kreyvium, FLIP
- Large fields: MiMC round $x^3$
![[Pasted image 20221024224251.png]]

Post-Quantum Cryptography
Asymmetric (e.g. RSA)
Shor's algorithm: Polynomial time
Solution: lattice-based, code-based,...

Symmetric (e.g. AES)
Grover's algorithm: $2^{K}\to 2^{K/2}$
鍵長を倍の長さにすることで同じセキュリティを担保できる。