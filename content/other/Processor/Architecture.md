
## x86-64アーキテクチャ
### レジスタ
64bit汎用レジスタ
| 64bitレジスタ | 32bitレジスタ | 16bitレジスタ | 8bitレジスタ |
|--|--|--|--|
| RSP | ESP |  SP | SPL |
| RBP | EBP |  BP | BPL |
| RAX | EAX |  AX |  AL |
| RBX | EBX |  BX |  BL |
| RDI | EDI |  DI | DIL |
| RSI | ESI |  SI | SIL |
| RDX | EDX |  DX |  DL |
| RCX | ECX |  CX |  CL |
| R8  | R8D | R8W | R8B |
| R9  | R9D | R9W | R9B |
| R10 | R10D | R10W | R10B |
| R11 | R11D | R11W | R11B |
| R12 | R12D | R12W | R12B |
| R13 | R13D | R13W | R13B |
| R14 | R14D | R14W | R14B |
| R15 | R15D | R15W | R15B |

フラグレジスタ
| 64bitレジスタ | 32bitレジスタ | 16bitレジスタ |
| ------------- | ------------- | ------------- |
| RFLAGS        | EFLAGS        | FLAGS         |

| bit | ニーモニック | 説明 | 用途 | 1となる条件 |
|--|--|--|--|--|
| 11 | OF |        Overflow Flag | 符号付き整数演算の結果の最上位bitが本来と異なる |
| 10 | DF |       Direction Flag | 文字列操作のデータポインタが減る向き |
|  7 | SF |            Sign Flag | 算術演算の結果が負 |
|  6 | ZF |            Zero Flag | 算術演算の結果が0 |
|  4 | AF | Auxiliary Carry Flag |  |
|  2 | PF |          Parity Flag | 演算の結果の最下位byte |
|  0 | CF |           Carry Flag | 加算/減算命令で最上位bitで繰り上がり/繰り下がりが発生 |

### メモリモデル
### アドレッシング
### 命令セット
## X86アセンブリ言語
### AT&T記法とIntel記法

## System V ABI(Application Binary Interface)