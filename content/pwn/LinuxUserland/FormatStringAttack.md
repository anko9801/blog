---
title: "Format String Attack"
---

## Format String Bug
書式を printf の第一引数に入れることで読み込んだり書き込める.
%p

## 対策
`printf` の第一引数を攻撃者に渡さない.
