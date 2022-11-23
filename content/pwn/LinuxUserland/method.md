---
title: "pwn を解く為に必要なステップ"
---

脆弱性の種類と，その攻略方法

| 段階               | 例                                                                         |
| ------------------ | -------------------------------------------------------------------------- |
| Vulnerabilities    | Stack BOF, Heap BOF, Use After Free, Format String Bug, Race Condition ... |
| Exploit Techniques | ret2plt, ret2libc, ROP, GOT Overwrite, Stack pivot ...                     |
| Anti-Exploits      | NX, ASLR, RELRO, PIE, Stack Canary ...                                     |

flagを獲得する方法
1. シェルコードを実行
	1. Exec-shell系
	2. Exec-shell + バックコネクト系
	3. [shellcode - Qiita](https://qiita.com/yyamada_bigtree/items/97ea176484f5b05c195d)
2. `system("/bin/sh")`や`execve("/bin/sh", 0, 0)`を実行
	1. 必要に応じてdup2したりする
3. open("flag") -> read() -> write()
	1. サンドボックスでexec系が禁止されているケース，chrootされているケースなどで有効

## バイナリ解析
動的解析
- gdb
	- gdb-peda
	- pwndbg
	- rust-gdb
静的解析
- Ghidra
	- v10.1.5が安定版 (プラグインも豊富だと思う)
- IDA Pro (アイダプロ)
	- 専門家はデコンパイラの性能から Ghidra より IDA Pro の方を使いがち
- Binary Ninja
- WinDbg
	- `bp <address>` : ブレークポイント
	- `pr` : レジスタ情報とgdbでいうnexti
	- `pt` : retが来るまで進める
	- `pctr` : レジスタ情報とcallとretが来るまで進める
	- `dc <address reg>` : double word単位とascii文字でデータ表示
- Immunity Debugger
- radare2

デバッグ情報
- DWARF
- gdbの自動的にやることでカバレッジを取れる
- seccomp BPF

必要なコマンド
- ld: リンク先
- patchelf: 動的ライブラリの解決
	- [libcにデバッグシンボルを付ける方法と自動化 - Satoooonの物置 (hatenablog.com)](https://satoooon1024.hatenablog.com/entry/2022/06/12/libc%E3%81%AB%E3%83%87%E3%83%90%E3%83%83%E3%82%B0%E3%82%B7%E3%83%B3%E3%83%9C%E3%83%AB%E3%82%92%E4%BB%98%E3%81%91%E3%82%8B%E6%96%B9%E6%B3%95%E3%81%A8%E8%87%AA%E5%8B%95%E5%8C%96)
- objdump
- checksec.sh
- one_gadget
- ROPgadget
- ptrace
- strace

 ```shell
$ (cat out; cat) | ./a.out
$ nm -D ./a.out | grep " system"
$ strings -a -tx ./libc.so.6 | grep "sh$"
$ objdump -S -M intel ./libc.so.6 --disassemble=execve
$ ldd a.out
$ echo -en ""
$ grep -E ""
$ one_gadget ./libc.so.6
$ ROPgadget --binary ./a.out
```

## gccオプション

- No RELRO: RELRO無効
- Partial RELRO: RELRO有効かつ遅延バインド有効
- Full RELRO: RELRO有効かつ遅延バインド無効

| やること                   | オプション                                |
|:-------------------------- |:----------------------------------------- |
| SSP 無効                   | `-fno-stack-protector`                    |
| SSP 有効 (default)         | `-fstack-protector`                       |
| NX bit 無効                | `-z execstack`                            |
| NX bit 有効 (default)      | `-z`                                      |
| No RELRO                   | `-Wl,-z,norelro`                          |
| Partial RELRO              | `-Wl,-z,relro,-z lazy`                    |
| Full RELRO (default)       | `-Wl,-z,relro,-z,now`                     |
| ASLR 無効                  | `sudo sysctl kernel.randomize_va_space=0` |
| ASLR 有効 (default)        | `sudo sysctl kernel.randomize_va_space=2` |
| PIE 無効                   | `-fno-pie -no-pie`                        |
| PIE 有効 (default)         | `-fPIE -pie`                              |
| Exec-Shield 無効           | `sudo sysctl -w kernel.exec-shield=0`     |
| Exec-Shield 有効 (default) | `sudo sysctl -w kernel.exec-shield=1`     |
