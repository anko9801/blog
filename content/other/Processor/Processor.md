---
title: "プロセッサ"
---

## プロセッサの種類
- CPU
	- LGA
	- BGA
	- QFP
- GPU
	- iGPU(internal) CPU内にあるグラボ
	- dGPU(discrete) NVMe/SATA/PCIeなどで接続する外付けのグラボ
	- eGPU(external) Thunderbolt 3/4などで接続する外付けのグラボ
- TPU 行列積演算
- FPU

未分類
- [揚げて炙ってわかるコンピュータのしくみ | 秋田 純一 |本 | 通販 | Amazon](https://www.amazon.co.jp/%E6%8F%9A%E3%81%92%E3%81%A6%E7%82%99%E3%81%A3%E3%81%A6%E3%82%8F%E3%81%8B%E3%82%8B%E3%82%B3%E3%83%B3%E3%83%94%E3%83%A5%E3%83%BC%E3%82%BF%E3%81%AE%E3%81%97%E3%81%8F%E3%81%BF-%E7%A7%8B%E7%94%B0-%E7%B4%94%E4%B8%80/dp/4297116014)
- Qualcomm Snapdragon 6 Gen 1
- 富岳
	- OS IHK/McKernel
- Read/Write
- big.LITTLE processing

## CPU機能
- [ARM immediate value encoding (mcdiarmid.org)](https://alisdair.mcdiarmid.org/arm-immediate-value-encoding/)
- RTDSC 命令
	- [How to Benchmark Code Execution Times on Intel IA-32 and IA-64 Instruction Set Architectures White Paper](https://www.intel.com/content/dam/www/public/us/en/documents/white-papers/ia-32-ia-64-benchmark-code-execution-paper.pdf)
- ABI
- System V AMD64
- [x86_64-abi-0.99.pdf (linuxbase.org)](https://refspecs.linuxbase.org/elf/x86_64-abi-0.99.pdf)

## アーキテクチャ
命令セットアーキテクチャ (ISA; Instruction Set Architecture)
- CISC
	- x86
		- IA-32
		- x64 (AMD64)
	- IA-64
- RISC
	- ARM
	- RISC-V
	- PowerPC
	- MIPS

マイクロアーキテクチャ

## 回路
全加算器
Deep Learning を用いた回路設計も行われている.
- [Designing Arithmetic Circuits with Deep Reinforcement Learning | NVIDIA Technical Blog](https://developer.nvidia.com/blog/designing-arithmetic-circuits-with-deep-reinforcement-learning/)

## 応用
- GPGPU

## 脆弱性
Intel の CPU の脆弱性を見つけたら [Project Circuit Breaker](https://www.projectcircuitbreaker.com/) に報告するとよい

- PACMAN
	- Apple M1 CPUのポインター
	- [PACMAN (pacmanattack.com)](https://pacmanattack.com/)
- Spectre
	- Branch Target Buffer: BPB
	- 危険性
	- Mitigation
		- Spectre-v1 SWAPGS
			- kernel parameter `nospectre_v1`
		- Spectre-v2 Retpoline
			- LFENCE/JMP
			- kernel parameter `nospectre_v2`
		- Spectre-v3 KPTI
			- kernel parameter
				- `pti=off`
				- `nopti`
				- `noibrs`
				- `noibpb`
				- `nospec_store_bypass_disable`
	- [KB4073119: Windows client guidance for IT Pros to protect against silicon-based microarchitectural and speculative execution side-channel vulnerabilities (microsoft.com)](https://support.microsoft.com/en-us/topic/kb4073119-windows-client-guidance-for-it-pros-to-protect-against-silicon-based-microarchitectural-and-speculative-execution-side-channel-vulnerabilities-35820a8a-ae13-1299-88cc-357f104f5b11)
- MDS/Zombieload
	- https://www.kernel.org/doc/html/latest/admin-guide/hw-vuln/mds.html
- TSX Asynchronous Abort
	- https://www.kernel.org/doc/html/latest/admin-guide/hw-vuln/tsx_async_abort.html
- iTLB multihit
