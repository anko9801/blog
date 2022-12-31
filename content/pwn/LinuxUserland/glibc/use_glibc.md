---
title: "glibc 関数リスト"
---

```cpp
// 標準入出力
int printf(const char *format, ...)
int fprintf(FILE *stream, const char *format, ...)
int dprintf(int fd, const char *format, ...)
int sprintf(char *str, const char *format, ...)
int snprintf(char *str, size_t size, const char *format, ...)

// ファイル入出力
int open(const char *pathname, int flags, mode_t mode) // ファイルディスクリプタ (fd) or Error (-1)
int close(int fd)
ssize_t read(int fd, const void *buf, size_t count)
ssize_t write(int fd, const void *buf, size_t count)
off_t lseek(int fd, off_t offset, int whence)
int unlink(const char *pathname)
int setgid(gid_t gid) // errno
int setuid(uid_t uid) // errno

// プロセス関連
pid_t fork(void)
int execve(const char *pathname, char *const argv[], char *const envp[])
void exit(int status)
```
