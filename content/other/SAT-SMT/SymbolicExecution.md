---
title: "シンボリック実行エンジン"
---

## 説明

プログラム上で目的の地点に到達したいとき、それに当てはまるような入力値の条件を数学的に解くプログラム。条件分岐ごとに条件式を追加してそれをSMTソルバに解かせる。

Concolic Execution
SSA形式とCFGが対応する
Taint 解析

## 計算量

条件分岐数を $N$ として $O(2^N)$

## 実装

レジスタ, メモリに関する制約とSSA形式のIRを条件式としてSMTソルバで解く。
- [Rustでの実装 (mini_symbolic)](https://github.com/anko9801/mini_symbolic)

## 使用例

シンボリック実行エンジンのプロジェクト angr のサンプルコード。

```python
import angr
import logging

binary_path = './chall'

target = angr.Project(binary_path, main_opts={'base_addr': 0x10000})
logging.getLogger('angr').setLevel(logging.CRITICAL)
entry_state = target.factory.entry_state()
simulation = target.factory.simulation_manager(entry_state)
simulation.explore(find=0x000115b1, avoid=0x000115c4)
for sf in simulation.found:
    print(sf.posix.dumps(0))

solution - simulation.found[0].posix.dumps(0)
print(solution)
```

## 参考

- [バイナリ萌えの彼女がシンボリック実行に恋着してますが、制約に挑む幼気な表情が最高です！（１）](https://speakerdeck.com/katc/bainarimeng-efalsebi-nu-gasinboritukushi-xing-nilian-zhao-sitemasuga-zhi-yue-nitiao-muyou-qi-nabiao-qing-gazui-gao-desu-1)
- [Girls Meets Symbolic Execution: Assertion 2. Automated Exploit Generation](https://speakerdeck.com/katc/girls-meets-symbolic-execution-assertion-2-automated-exploit-generation)
- [実装して学ぶ Symbolic Backward Execution](https://speakerdeck.com/katc/shi-zhuang-sitexue-bu-symbolic-backward-execution-aceefce8-d25e-4db0-8ebb-d648bb2c41cd)
- [プログラム解析入門、もしくはC/C++を安全に書くのが難しすぎる話 - Google スライド](https://docs.google.com/presentation/d/1WHmCLeC5ZPiq2MBOQaZc-pNVWaJanx8eXAkViGl2zws/edit#slide=id.g135752f5899_0_673)
