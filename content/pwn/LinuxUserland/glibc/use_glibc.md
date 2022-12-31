---
title: "glibc 関数リスト"
---

```cpp
// pathname`: パス名, `flags`: , `mode`: `chmod` コマンドと同じ
int open(const char *pathname, int flags, mode_t mode) // ファイルディスクリプタ (fd) or Error (-1)
int close(int fd)
```
