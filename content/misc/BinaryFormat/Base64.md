---
title: "Base64"
---

## 説明
10 進数は `0-9`, 16 進数は `0-9A-F` と拡張されてて, それじゃ文字を沢山使えば 64 進数も書けるよね～って発想を規格にしたものが Base64 です。実際には Base64 変換前のデータは数字ではなくバイナリが指定されます。使用目的は特に Web 周辺技術において URL や Cookie など文字列しか受け付けない環境でバイナリを書きたいときに Base64 を使えば楽に文字に落とし込んで渡せるので嬉しいよね～という感じです。

## 仕様

1.  元データを6ビットずつに分割する。(6ビットに満たない分は後ろに0を追加して6ビットにする)
2.  各6ビットの値を変換表を使って4文字ずつ変換する。
3.  4文字に足りない分は `=` 記号を後ろに追加する。

| ビット列 | Base64 | ビット列 | Base64 | ビット列 | Base64 | ビット列 | Base64 |
| -------- |:------:| -------- |:------:| -------- |:------:| -------- |:------:|
| `000000` |  `A`   | `010000` |  `Q`   | `100000` |  `g`   | `110000` |  `w`   |
| `000001` |  `B`   | `010001` |  `R`   | `100001` |  `h`   | `110001` |  `x`   |
| `000010` |  `C`   | `010010` |  `S`   | `100010` |  `i`   | `110010` |  `y`   |
| `000011` |  `D`   | `010011` |  `T`   | `100011` |  `j`   | `110011` |  `z`   |
| `000100` |  `E`   | `010100` |  `U`   | `100100` |  `k`   | `110100` |  `0`   |
| `000101` |  `F`   | `010101` |  `V`   | `100101` |  `l`   | `110101` |  `1`   |
| `000110` |  `G`   | `010110` |  `W`   | `100110` |  `m`   | `110110` |  `2`   |
| `000111` |  `H`   | `010111` |  `X`   | `100111` |  `n`   | `110111` |  `3`   |
| `001000` |  `I`   | `011000` |  `Y`   | `101000` |  `o`   | `111000` |  `4`   |
| `001001` |  `J`   | `011001` |  `Z`   | `101001` |  `p`   | `111001` |  `5`   |
| `001010` |  `K`   | `011010` |  `a`   | `101010` |  `q`   | `111010` |  `6`   |
| `001011` |  `L`   | `011011` |  `b`   | `101011` |  `r`   | `111011` |  `7`   |
| `001100` |  `M`   | `011100` |  `c`   | `101100` |  `s`   | `111100` |  `8`   |
| `001101` |  `N`   | `011101` |  `d`   | `101101` |  `t`   | `111101` |  `9`   |
| `001110` |  `O`   | `011110` |  `e`   | `101110` |  `u`   | `111110` |  `+`   |
| `001111` |  `P`   | `011111` |  `f`   | `101111` |  `v`   | `111111` |  `/`   |

Wikipediaより

## 使い方

```shell
$ echo -n "あいうえおかきくけこ" | base64
$ echo -n "44GC44GE44GG44GI44GK44GL44GN44GP44GR44GT" | base64 -d
```

## 脆弱性
Base64 の仕様上同じデータを表す複数の Base64 が存在する。

// TODO 具体例

例えば, あるトークンを Base64 として完全一致したときに何らかの処理している場合は上のように同じデータとなるような Base64 を渡すことでバイパスされてしまいます. こういうときは変換後の文字列に対して完全一致すれば確実です.