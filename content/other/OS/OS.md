---
title: "OS"
---

## カーネルの種類

- Unikernel
	- [Unikernelについての現状調査 - Fixstars Tech Blog /proc/cpuinfo](https://proc-cpuinfo.fixstars.com/2020/03/unikernel/)
	- [microsoft/monza: Research unikernel for virtualized services (github.com)](https://github.com/microsoft/monza)
- Microkernel
	- seL4 Microkernel (Secure Embedded L4)
		- [オープンソースRTOS「seL4」の紆余曲折からマイクロカーネルの進化を俯瞰する：リアルタイムOS列伝（19）（1/3 ページ） - MONOist (itmedia.co.jp)](https://monoist.itmedia.co.jp/mn/articles/2201/31/news053.html)

## システムコール

- BIOS; Basic Input/Output System
	- ハードウェアの初期化
	- OS や bootloader へのサービスの提供
	- セキュアブート
		- ブートコードを改ざんしてシステムを乗っ取るようなタイプのウイルスなどの活動を防ぐためのもの
		- BIOSがデジタル署名を生成し, 信頼済み証明書と検証する.
		- TPM
- UEFI; Unified Extensible Firmware Interface
- PXE(Preboot eXecution Environment) Boot

## 割り込みの仕組みを知りたい


## io_uring

mem_info

## ロック
- `std::sync::RwLock::{write(), try_read()}` を併用した場合には「書き込みロックを最優先」という挙動は必ずしも期待できない (LinuxではNG)
- Pthread の規約が挙動に自由度をもたせており、Linuxにおけるデフォルト実装では **writer starvation** が発生する
- Rustにおいて writer starvation を回避しつつ readers-writer lock を使うには [`parking_lot::RwLock`](https://docs.rs/parking_lot/latest/parking_lot/type.RwLock.html) を使うと良い


## eBPF
- [verifier.c - kernel/bpf/verifier.c - Linux source code (v5.18.14) - Bootlin](https://elixir.bootlin.com/linux/v5.18.14/source/kernel/bpf/verifier.c#L10186)


## 自作OS

## 参考
[Linux source code (v5.17.9) - Bootlin](https://elixir.bootlin.com/linux/v5.17.9/source)
[The Linux Kernel documentation — The Linux Kernel documentation](https://docs.kernel.org/)