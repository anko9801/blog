---
title: "glibc malloc"
---

## 説明

環境
64bit

glibc のデータ領域に main_arena

binには沢山の種類がある
- tcache
- fastbins
- unsorted bins
- small bins
- large bins

ヒープ領域

| bins   | fd | bk |
|:------:|:---|:---|
| tcache | next | tcache_key |
| fastbins | next | null |
| unsorted bins | &main_arena.top |  |
| small bins |  |  |
| large bins |  |  |

libc 2.29 tcache_key
tcacheでdouble freeはできない

## 参考
[malloc(3)のメモリ管理構造](https://www.valinux.co.jp/technologylibrary/document/linux/malloc0001/)
[MallocInternals - glibc wiki (sourceware.org)](https://sourceware.org/glibc/wiki/MallocInternals)
