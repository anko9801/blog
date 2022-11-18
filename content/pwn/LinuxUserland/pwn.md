脆弱性の種類と，その攻略方法
Vulnerabilities: Stack BOF, Heap BOF, Use After Free, Format String Bug, Race Condition ...
Exploit Techniques: ret2plt, ret2libc, ROP, GOT Overwrite, Stack pivot ...
Anti-Exploits: NX, ASLR, RELRO, PIE, Stack Canary ...

シェルを開く方法
1. シェルコードを実行
	1. Exec-shell系
	2. Exec-shell + バックコネクト系
2. `system("/bin/sh")`や`execve("/bin/sh", 0, 0)`を実行
	1. 必要に応じてdup2したりする
3. open("flag") -> read() -> write()
	1. 正確にはシェルを開いていない
	2. でもCTF的にはフラグが読めればいい
	3. サンドボックスでexec系が禁止されているケース，chrootされているケースなどで有効

## Vulnerabilities

#### Format String Bug
書式をprintfの第一引数に入れることで読み込んだり書き込める。

#### スタックオーバーフロー (Stack-based buffer overflow)
スタック上のオーバーフローを利用してリターンアドレスやローカル変数を書き換えられる。
- トリガー
メモリ書き換え (文字列入力、文字列入力の最後尾にヌル文字を挿入するなど)
- 防御機構
SSP canary
[https://inaz2.hatenablog.com/entry/2014/03/14/151011 単純なスタックバッファオーバーフロー攻撃をやってみる - ももいろテクノロジー]

#### Stack underflow
関数フレーム外までpopを行う。

Off-by-one error
Null-byte Overflow

## Exploit Techniques

　GOT overwrite
GOT ( Global Offset Table ) の行先を書き換える。
ROPでscanfして書き換えたりできる
- 防御機構
Full RELRO


 libcリーク :   libc leak
libc内のアドレスをputs, printf関数などで出力させる。
 ret2XX
リターンアドレスを書き換える。

 ROP :  Return-Oriented Programming
ret命令で終わる少ない命令列(Gadget)を組み合わせてret2hogeをする。呼び出し規約 #Pwn_よく使うglibcの関数まとめ がcdeclなどスタックを用いる場合、3つ以上の関数を呼ぶとき引数が関数アドレスと被らないように引数を削除する為のpop retガジェットを挟む。fastcallなどレジスタを用いる場合、レジスタに引数を渡すためにpop rdi retガジェット等を使う。
- 防御機構
Intel CET
[https://smallkirby.hatenablog.com/entry/2020/09/10/230629 【pwn 36.0】Intel CETが、みんなの恋人ROPを殺す - newbieからバイナリアンへ]
[https://inaz2.hatenablog.com/entry/2014/07/04/001851 x64でスタックバッファオーバーフローをやってみる - ももいろテクノロジー]

 ヒープ関連
資料
[https://www.youtube.com/watch?v=0-vWT-t0UHg]
[https://www.valinux.co.jp/technologylibrary/document/linux/malloc0001/ malloc(3)のメモリ管理構造 - VALINUX]
[https://qiita.com/kaityo256/items/9e78b507940b2292bf79 mallocの動作を追いかける(mmap編)]
[https://heap-exploitation.dhavalkapil.com heap-exploitation]
攻撃デモ [https://github.com/shellphish/how2heap how2heap]
[https://ptr-yudai.hatenablog.com/entry/2019/05/31/235444 ヒープ系問題におけるstdout / stderrを利用したメモリリーク - CTFするぞ]

tcache
fastbin
unsorted bin
mmap

code:c
 struct malloc_chunk {
   INTERNAL_SIZE_T      mchunk_prev_size;  /* Size of previous chunk (if free).  */
   INTERNAL_SIZE_T      mchunk_size;       /* Size in bytes, including overhead. */
   struct malloc_chunk* fd;                /* double links -- used only if free. */
   struct malloc_chunk* bk;
   /* Only used for large blocks: pointer to next larger size.  */
   struct malloc_chunk* fd_nextsize; /* double links -- used only if free. */
   struct malloc_chunk* bk_nextsize;
 };
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

 ヒープバッファオーバーフロー :  Heap-based buffer overflow
ヒープ上のバッファオーバーフローを利用して、関数の戻りアドレスや関数ポインタを書き換える
[https://inaz2.hatenablog.com/entry/2014/05/14/011448 ヒープオーバーフローによるGOT overwriteをやってみる - ももいろテクノロジー]
[https://inaz2.hatenablog.com/entry/2014/05/15/012621 ヒープオーバーフローによるC++ vtable overwriteをやってみる - ももいろテクノロジー]

 use-after-free
解放した領域を誤って使用してしまうUse After Freeを利用し、同じ領域に悪意のあるデータ構造を確保して利用させる事で、関数ポインタを書き換える。
[https://inaz2.hatenablog.com/entry/2014/06/18/215452 use-after-freeによるGOT overwriteをやってみる - ももいろテクノロジー]
[https://inaz2.hatenablog.com/entry/2014/06/18/220735 use-after-freeによるC++ vtable overwriteをやってみる - ももいろテクノロジー]

前回使用したサイズ以下の容量を確保することでfreeした後のデータを使える。
解放済みポインタ ( ダングリングポインタ :  dangling pointer ) を悪意のあるアドレスへ変えることで意図しない挙動にすることができる。

 C++ vtable overwrite
C++では仮想関数を実現する為に実行バイナリ中に型に対して呼び出す関数の対応表( vtable :  virtual method table )を持つ。このvtableの行先を書き換える。たぶん、C++だけではなく動的ポリモーフィズムを選択した言語なら出来そう。
動的ポリモーフィズム C++
静的ポリモーフィズム Rust
[https://inaz2.hatenablog.com/entry/2014/05/15/012621 ヒープオーバーフローによるC++ vtable overwriteをやってみる - ももいろテクノロジー]
[https://inaz2.hatenablog.com/entry/2014/06/18/220735 use-after-freeによるC++ vtable overwriteをやってみる - ももいろテクノロジー]
[https://ptr-yudai.hatenablog.com/entry/2019/02/12/000202 _IO_str_overflowを使ったvtable改竄検知の回避手法 - CTFするぞ]

 __free_hook 書き換え
__free_hookはfree時に呼び出す関数アドレスを指定できる。Full RELROでも書き換えられる。
[https://qiita.com/hanya1995/items/c29a89737bbd521e67f2 SECCON Beginners CTF 2020 Beginners Heap Writeup &初心者向け解説]

 tcache poisoning
[https://hackmd.io/@Xornet/H1hYUUR2I SECCON Beginners CTF 2019 - Babyheap]

 tcache double-free
glibc 2.28 以前の tcache では double free が検出されない
[https://smallkirby.hatenablog.com/entry/2020/12/15/042803 【pwn 41.0】realloc-baseのmemory corruptionの古い小ネタと最近のtcache周りの小話 - newbieからバイナリアンへ]

　chunk overlap

　fastbin attack


[https://ptr-yudai.hatenablog.com/entry/2019/05/31/235444 ヒープ系問題におけるstdout / stderrを利用したメモリリーク - CTFするぞ]


## Anti-Exploits

### DEP (Data Execution Prevention)
NX bit (No eXecute bit)  read write execute protection
セグメント毎に実行する権限を付与するかしないかを設定する。任意のコードを挿入し実行を誘う攻撃を防御できる。
メモリ領域のアクセス保護オプションを書き換えるには、Windowsの場合VirtualProtectEx関数、Linuxの場合mprotect(2)が使える。([https://inaz2.hatenablog.com/entry/2014/04/20/010545 Return-to-libcとmprotect(2)でDEPを回避してみる - ももいろテクノロジー] より)


### RELRO (RELocation Read-Only)
GOTは初回呼び出し時に行き先を書き込む ( 遅延バインド: lazy binding )。

[https://inaz2.hatenablog.com/entry/2014/04/30/173618 RELROとformat string attackによるリターンアドレス書き換え - ももいろテクノロジー]

### ASLR (Address Space Layout Randomization)
スタック領域・ヒープ領域や共有ライブラリが置かれるベースアドレスは一定の範囲の中でランダムに決められる。
これらのアドレスを知るには以下のコマンドを叩く。一回目と二回目で変わることが分かる。

```shell
 $ cat /proc/<process ID>/maps              // = gdbのi proc map = pwndbgのvmmap
```

防御機構
SSP :  Stack-Smashing Protection
関数呼び出し時にランダムな値(Canary)を配置し、関数から出る時に変化したか検証し、変わっていたら__stack_chk_fail関数を呼び出し、強制終了させる。

#### PIE (Position-Independent Executables)

実行ファイルそのものが置かれるベースアドレスをランダムに決められる。


1nibble brute force
 ASLR + PIEに対する対抗策
ベースアドレスがランダム化しても関数・データ間のオフセットは同じであるため、例えば次のようにしてベースアドレスを取得できる。
  スタックに積まれたリターンアドレスの値から、実行ファイルのベースアドレスが計算できる。
  一度呼び出されたライブラリ関数のGOTアドレスの値から、そのライブラリのベースアドレスが計算できる。
  スタックに積まれたsaved ebpの値から、スタック領域に置かれる他のデータのアドレスが計算できる。
  ヒープ領域に確保されたデータを指すポインタの値から、ヒープ領域のベースアドレスが計算できる。

 ASCII-armor - Exec-Shield
共有ライブラリのベースアドレスを0x00XXXXXXのように\x00を含めることでret2libcを難しくする機構。
対抗策
- ライブラリ内のsystem関数ではなくsystem@pltに飛ばす(ret2plt)。
- system@pltがなければstrcpy@pltやsnprintf@pltに飛ばして(ret2strcpy)、実行バイナリ中の1バイトで1つずつGOT overwriteし、シェルを起動する。
https://ja.wikipedia.org/wiki/Exec_Shield

 /etc/sysctl.conf

 KASLR :  Kernel ASLR
[https://blog.ishikawa.tech/entry/2019/12/17/161319 KASLRの実装と挙動の確認 - 人生は勉強ブログ]
 KADR :  Kernel Address Display Restriction
 KPTI :  Kernel  Page Table Isolation
Meltdown Spectreに対する防御機構として導入。
 SMAP :  Supervisor Mode Access Prevention, SMEP :  Supervisor Mode Execution Prevention
user-space memory mapping
[https://inaz2.hatenablog.com/entry/2015/03/27/021422 LinuxカーネルモジュールでStackjackingによるSMEP+SMAP+KADR回避をやってみる - ももいろテクノロジー]



#### gccオプションまとめ

Partial RELRO = RELRO有効かつ遅延バインド有効
Full RELRO = RELRO有効かつ遅延バインド無効

| やること | オプション |
|:---|:---|
| SSP 無効 | `-fno-stack-protector` |
| SSP 有効 | `-fstack-protector` |
| NX bit 無効 | `-z execstack` |
| NX bit 有効 | `-z` |
| No RELRO | `-Wl,-z,norelro` |
| Partial RELRO | `-Wl,-z,relro,-z lazy` |
| Full RELRO | `-Wl,-z,relro,-z,now` |
| ASLR 無効 | `sudo sysctl kernel.randomize_va_space=0` |
| ASLR 有効 | `sudo sysctl kernel.randomize_va_space=2` |
| PIE 無効 | `-fno-pie -no-pie` |
| PIE 有効 | `-fPIE -pie` |
| Exec-Shield 無効 | `sudo sysctl -w kernel.exec-shield=0` |
| Exec-Shield 有効 | `sudo sysctl -w kernel.exec-shield=1` |

checksec.sh

Master Canary
https://www.youtube.com/watch?v=UTC2iWxQ4qc


## 引数

### fastcall
シェルを呼び出す
execve("/bin/sh", null, null)
system("/bin/sh")
[https://qiita.com/yyamada_bigtree/items/97ea176484f5b05c195d シェルコード]

 ```
 $ (cat out; cat) | ./a.out
 $ nm -D ./a.out | grep " system"
 $ strings -a -tx ./libc.so.6 | grep "sh$"
 $ objdump -S -M intel ./libc.so.6 --disassemble=execve
 $ ldd a.out
 $ gcc -fno-stack-protector -fPIE bof.c
 $ echo -en ""
 $ grep -E ""
 $ one_gadget ./libc.so.6
 $ ROPgadget --binary ./a.out
```
32bit環境ASLRに対してブルートフォース + NOP sled
https://inaz2.hatenablog.com/entry/2014/03/15/073837

アセンブリを読む
次の関数を目標にして読むと読みやすい
引数はどこから入るのか、この引数はどう構成されているのか
gdb >>>> i files

wine
WinDbg
bp <address> : ブレークポイント
pr : レジスタ情報とgdbでいうnexti
pt : retが来るまで進める
pctr : レジスタ情報とcallとretが来るまで進める
dc <address reg> : double word単位とascii文字でデータ表示
