---
title: "グラフ"
---

## 説明
グラフ (Graph) とは頂点の集合 $V$ と辺の集合 $E$ の組 $(V, E)$ です. 頂点 (Node) は点と見なせるオブジェクトの概念で, 辺 (Edge) は頂点 $u$ から頂点 $v$ へ向かう組 $(u, v)$ です. 辺には様々な属性を付け加えることがあります. 最短経路問題ではコストという属性を付けて辺の移動に重みを付け加えます.

多相型 `T` は整数または小数の型であることを仮定してます.
- `Edge(int from, int to, T cost = 1)`
	- 頂点 `from` から頂点 `to` へコスト `cost` の辺を生成します.
- `Graph(int n)`
	- 頂点が `n` 個のグラフを作ります.
- `void add_directed_edge(int from, int to, T cost = 1)`
	- 有向辺を張ります.
- `void add_edge(int from, int to, T cost = 1)`
	- 無向辺を張ります.

## 実装

```cpp
template <typename T = int>
struct Edge {
  int from, to;
  T cost;

  Edge() = default;
  Edge(int from, int to, T cost = 1) : from(from), to(to), cost(cost) {}

  operator int() const { return to; }
};

template <typename T = int>
struct Graph {
  std::vector<std::vector<Edge<T>>> g;

  Graph() = default;
  Graph(int n) : g(n) {}

  void add_directed_edge(int from, int to, T cost = 1) {
    g[from].emplace_back(from, to, cost);
  }
  void add_edge(int from, int to, T cost = 1) {
    g[from].emplace_back(from, to, cost);
    g[to].emplace_back(to, from, cost);
  }

  inline std::vector<Edge<T>> &operator[](const int k) {
    return g[k];
  }
};
```

## 参考