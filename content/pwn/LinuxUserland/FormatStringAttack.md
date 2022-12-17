---
title: "Format String Attack"
---

## Format String Bug
書式を `printf` の第一引数に入れることで読み込んだり書き込める.

| Format                  | 説明                                                                                                         |
| ----------------------- | ------------------------------------------------------------------------------------------------------------ |
| `%x` `%6$x`             |                                                                                                              |
| `%42x`                  | 指定した数だけ文字を出力する                                                                                 |
| `%p` `%6$p`             | 読む                                                                                                         |
| `%s` `%6$s`             | その64bitアドレスから(null文字まで)読む                                                                      |
| `%n` `%6$n`             | その64bitアドレスへ書き込む                                                                                  |
| `%hhn` `%hn` `%n` `%ln` | それぞれ 1 2 4 8 バイト書き込む。%hhnは0x100で剰余をとってくれるので一周回してあげれば任意の値を書き込める。 |

## 対策
`printf` の第一引数を攻撃者に渡さない.