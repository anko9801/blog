---
title: "OS"
---

PXE(Preboot eXecution Environment) Boot

## Unikernel
## Microkernel
## seL4 Microkernel
Secure Embedded L4 Microkernel

## システムコール

### Basic Input/Output System: BIOS

- ハードウェアの初期化
- OS や bootloader へのサービスの提供

### Unified Extensible Firmware Interface: UEFI

## 割り込みの仕組みを知りたい


## io_uring

mem_info

## ロック
- `std::sync::RwLock::{write(), try_read()}` を併用した場合には「書き込みロックを最優先」という挙動は必ずしも期待できない (LinuxではNG)
- Pthread の規約が挙動に自由度をもたせており、Linuxにおけるデフォルト実装では **writer starvation** が発生する
- Rustにおいて writer starvation を回避しつつ readers-writer lock を使うには [`parking_lot::RwLock`](https://docs.rs/parking_lot/latest/parking_lot/type.RwLock.html) を使うと良い


## eBPF
[verifier.c - kernel/bpf/verifier.c - Linux source code (v5.18.14) - Bootlin](https://elixir.bootlin.com/linux/v5.18.14/source/kernel/bpf/verifier.c#L10186)


## 参考
[Linux source code (v5.17.9) - Bootlin](https://elixir.bootlin.com/linux/v5.17.9/source)
[The Linux Kernel documentation — The Linux Kernel documentation](https://docs.kernel.org/)