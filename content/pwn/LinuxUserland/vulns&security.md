---
title: "Linux Userland のセキュリティ機構"
---

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

32bit環境ASLRに対してブルートフォース + NOP sled
https://inaz2.hatenablog.com/entry/2014/03/15/073837

防御機構
SSP :  Stack-Smashing Protection
関数呼び出し時にランダムな値(Canary)を配置し、関数から出る時に変化したか検証し、変わっていたら`__stack_chk_fail` 関数を呼び出し、強制終了させる。

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

