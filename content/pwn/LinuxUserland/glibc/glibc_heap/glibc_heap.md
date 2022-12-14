---
title: "glibc malloc"
---

## ヒープとは

## 動的メモリ

```cpp
// Dynamically allocate 10 bytes

char *buffer = (char *)malloc(10);

strcpy(buffer, "hello");

printf("%s\n", buffer); // prints "hello"

// Frees/unallocates the dynamic memory allocated earlier

free(buffer);
```


## glibc heap

### malloc chunk

```c
 struct malloc_chunk {
   INTERNAL_SIZE_T      mchunk_prev_size;  /* Size of previous chunk (if free).  */
   INTERNAL_SIZE_T      mchunk_size;       /* Size in bytes, including overhead. */
   struct malloc_chunk* fd;                /* double links -- used only if free. */
   struct malloc_chunk* bk;
   /* Only used for large blocks: pointer to next larger size.  */
   struct malloc_chunk* fd_nextsize; /* double links -- used only if free. */
   struct malloc_chunk* bk_nextsize;
 };
```

```
Allocated chunk
code:c
     chunk-> +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
             |             Size of previous chunk, if unallocated (P clear)  |
             +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
             |             Size of chunk, in bytes                     |A|M|P|
       mem-> +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
             |             User data starts here...                          .
             .                                                               .
             .             (malloc_usable_size() bytes)                      .
             .                                                               |
 nextchunk-> +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
             |             (size of chunk, but used for application data)    |
             +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
             |             Size of next chunk, in bytes                |A|0|1|
             +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
Notice how the data of an allocated chunk uses the first attribute (mchunk_prev_size) of the next chunk. mem is the pointer which is returned to the user.
Free chunk
code:c
     chunk-> +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
             |             Size of previous chunk, if unallocated (P clear)  |
             +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     `head:' |             Size of chunk, in bytes                     |A|0|P|
       mem-> +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
             |             Forward pointer to next chunk in list             |
             +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
             |             Back pointer to previous chunk in list            |
             +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
             |             Unused space (may be 0 bytes long)                .
             .                                                               .
             .                                                               |
 nextchunk-> +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     `foot:' |             Size of chunk, in bytes                           |
             +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
             |             Size of next chunk, in bytes                |A|0|0|
             +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

前回使用したサイズ以下の容量を確保することでfreeした後のデータを使える。
解放済みポインタ ( ダングリングポインタ :  dangling pointer ) を悪意のあるアドレスへ変えることで意図しない挙動にすることができる。

glibc のデータ領域に main_arena

### malloc_state

```cpp
struct malloc_state
{
  /* Serialize access.  */
  __libc_lock_define (, mutex);

  /* Flags (formerly in max_fast).  */
  int flags;

  /* Set if the fastbin chunks contain recently inserted free blocks.  */
  /* Note this is a bool but not all targets support atomics on booleans.  */
  int have_fastchunks;

  /* Fastbins */
  mfastbinptr fastbinsY[NFASTBINS];

  /* Base of the topmost chunk -- not otherwise kept in a bin */
  mchunkptr top;

  /* The remainder from the most recent split of a small request */
  mchunkptr last_remainder;

  /* Normal bins packed as described above */
  mchunkptr bins[NBINS * 2 - 2];

  /* Bitmap of bins */
  unsigned int binmap[BINMAPSIZE];

  /* Linked list */
  struct malloc_state *next;

  /* Linked list for free arenas.  Access to this field is serialized
     by free_list_lock in arena.c.  */
  struct malloc_state *next_free;

  /* Number of threads attached to this arena.  0 if the arena is on
     the free list.  Access to this field is serialized by
     free_list_lock in arena.c.  */
  INTERNAL_SIZE_T attached_threads;

  /* Memory allocated from the system in this arena.  */
  INTERNAL_SIZE_T system_mem;
  INTERNAL_SIZE_T max_system_mem;
};
```

binには沢山の種類がある
- tcache
- fastbins
- unsorted bins
- small bins
- large bins

ヒープ領域
```cpp
struct malloc_chunk{
  size_t prev_size; // Size of previous chunk (if free).
  size_t size;      // Size in bytes, including overhead. 下3bitはフラグ

  // userに渡されるアドレスはここ
  // double links -- used only if free.
  void *fd; // next
  void *bk; // key

  // for large size
  void *fd_nextsize;
  void *bk_nextsize;
}
```

| bins   | fd | bk |
|:------:|:---|:---|
| tcache | next | tcache_key |
| fastbins | next | null |
| unsorted bins | &main_arena.top |  |
| small bins |  |  |
| large bins |  |  |

tcache

```cpp
/* We overlay this structure on the user-data portion of a chunk when
   the chunk is stored in the per-thread cache.  */
typedef struct tcache_entry
{
  struct tcache_entry *next;
  /* This field exists to detect double frees.  */
  uintptr_t key;
} tcache_entry;

/* There is one of these for each thread, which contains the
   per-thread cache (hence "tcache_perthread_struct").  Keeping
   overall size low is mildly important.  Note that COUNTS and ENTRIES
   are redundant (we could have just counted the linked list each
   time), this is for performance reasons.  */
typedef struct tcache_perthread_struct
{
  uint16_t counts[TCACHE_MAX_BINS];
  tcache_entry *entries[TCACHE_MAX_BINS];
} tcache_perthread_struct;
```
本質は next つまり単方向連結リスト

#### mitigations
**counts**
初期値 0, free すると +1, malloc すると -1 される値
上限7個 これを超えると fastbin へ
連結リストとは独立にカウントされることで malloc の回数と free の回数の非整合性を検知する(counts は負の数となることはない).

ex)
B: 攻撃者が指定するchunk
```
entry:空 -> entry: A ->改ざん！ -> entry: A->B ->malloc-> entry: B
count:0 -> count: 1 -----------> count:1 -------------> count: 0

このmitigationのbypass例
entry:空 -> entry: A ... entry: A_1->A_2 ->改ざん！ -> entry: A_1->B ->malloc-> entry: B
count:0 -> count: 1 ... count: 2 ------------------> count:2 --------------------> count: 1
```
**key**
freeする度にentry.keyに&tcacheを代入する
free するときに entry.keyの位置に&tcache がある場合に限り
entryのリストを探索しdouble free を検知する.

mallocしてきた chunk A{.a = 0x12345678, .b = 0x90abcdef}
-> free ->
freeされた tcache chunk A{.next = (次のchunkのアドレス) .key= &tcache}

mallocしてきた chunk B{.a = 0x12345678, .b = &tcache} たまたまkeyの位置が&tcacheになってる！
-> free -> double free(擬陽性)

tcache.entry[0x10] -> A -> B -> C

```
struct malloc_chunk{
  size_t prev_size; // malloc中に必要！
  size_t size; // malloc中に必要！ 下3bitはフラグ

  // userに渡されるアドレスはここ

  void *fd; // next
  void *bk; // key

  // for large size
  void *fd_nextsize;
  void *bk_nextsize;
}
```

struct tcache_entryはこの構造体のfdの位置にoverlapしてtcache_perthread_structで管理してる
### 雑〜なexploit
- 余分にfreeをしてcountを増やした後、対象のfree済みchunkに対してkeyをごちゃごちゃにしnext改ざん
- これでentryの連結リストの中に攻撃者指定のアドレスが入るため、このままmallocしていけば任意アドレス書き込み可能


libc 2.29 tcache_key
tcacheでdouble freeはできない
wwwdsssssssssssssssssssssssssssasssssssad
```cpp
/* Caller must ensure that we know tc_idx is valid and there's room
   for more chunks.  */
static __always_inline void
tcache_put (mchunkptr chunk, size_t tc_idx)
{
  tcache_entry *e = (tcache_entry *) chunk2mem (chunk);

  /* Mark this chunk as "in the tcache" so the test in _int_free will
     detect a double free.  */
  e->key = tcache_key;

  e->next = PROTECT_PTR (&e->next, tcache->entries[tc_idx]);
  tcache->entries[tc_idx] = e;
  ++(tcache->counts[tc_idx]);
}
```

##

glibc-2.35
PROTECT_PTR
```cpp
/* Safe-Linking:
   Use randomness from ASLR (mmap_base) to protect single-linked lists
   of Fast-Bins and TCache.  That is, mask the "next" pointers of the
   lists' chunks, and also perform allocation alignment checks on them.
   This mechanism reduces the risk of pointer hijacking, as was done with
   Safe-Unlinking in the double-linked lists of Small-Bins.
   It assumes a minimum page size of 4096 bytes (12 bits).  Systems with
   larger pages provide less entropy, although the pointer mangling
   still works.  */
#define PROTECT_PTR(pos, ptr) \
  ((__typeof (ptr)) ((((size_t) pos) >> 12) ^ ((size_t) ptr)))
#define REVEAL_PTR(ptr)  PROTECT_PTR (&ptr, ptr)
```
`(pos >> 12) ^ ptr`
攻撃者側は最初heapのアドレスがわからない状態から始まる
ページは4k単位なので下位3nibble(12bit)は何度実行しても変わらない(randomizeされない)
だから3nibble分右にずらしてxorすると最大限の効果を得られる

ただfree済み tcacheを読めたとき
A{
.next = 0x7ffabcdef;
}
Aの上位9nibble分はとれる。

メモリの確保量が小さいとき、すなわちheapが1ページで収まっているとき
Aでとれた上位9nibble分の情報をそのまま使える。
-> nextに対して復元するようなXORを掛ければお終い

## 参考
- [malloc(3)のメモリ管理構造](https://www.valinux.co.jp/technologylibrary/document/linux/malloc0001/)
- [MallocInternals - glibc wiki (sourceware.org)](https://sourceware.org/glibc/wiki/MallocInternals)
- malloc動画 [The 67th Yokohama kernel reading party - YouTube](https://www.youtube.com/watch?v=0-vWT-t0UHg)
- [mallocの動作を追いかける(mmap編) - Qiita](https://qiita.com/kaityo256/items/9e78b507940b2292bf79)
- [Preface - heap-exploitation (dhavalkapil.com)](https://heap-exploitation.dhavalkapil.com/)
- [shellphish/how2heap: A repository for learning various heap exploitation techniques. (github.com)](https://github.com/shellphish/how2heap)
- [ヒープ系問題におけるstdout / stderrを利用したメモリリーク - CTFするぞ (hatenablog.com)](https://ptr-yudai.hatenablog.com/entry/2019/05/31/235444)