---
title: "強連結成分分解"
---

## 説明
有向グラフにおいて、ある部分グラフが強連結であるとは部分グラフの任意の2点が互いに行き来可能であること。深さ優先探索の帰りがけ順(トポロジカルソート順)に逆グラフを探索したときそれらが通る点は強連結成分となる。

## 仕様
- `dag`
	- `g` の強連結成分を1つの頂点として見たグラフ. DAGとなる.
- `comp`
	- 各頂点に強連結成分は同じ値となるように値を割り当てる.
- `group`
	- 強連結成分の頂点のリスト.
- `void build()`
	- `dag`, `comp`, `group` を計算する.
	- $O(V + E)$

## 実装
```cpp
template <typename T = int>
struct SCC : Graph<T> {
public:
  using Graph<T>::Graph;
  using Graph<T>::g;
  Graph<T> dag;
  vector<int> comp;
  vector<vector<int>> group;

  void build() {
    // reversed graph
    rg = Graph<T>(g.size());
    for (size_t i = 0; i < g.size(); i++) {
      for (auto &e : g[i]) {
        rg.add_directed_edge(e.to, e.from, e.cost);
      }
    }
    // topological sort labeling
    used.assign(g.size(), false);
    for (size_t i = 0; i < g.size(); i++) dfs(i);
    reverse(order.begin(), order.end());
    // scc labeling
    int cnt = 0;
    comp.assign(g.size(), -1);
    for (auto idx : order) if (comp[idx] == -1) rdfs(idx, cnt), cnt++;
    // build dag
    dag = Graph<T>(cnt);
    for (size_t i = 0; i < g.size(); i++) {
      for (auto &e : g[i]) {
        int from = comp[e.from], to = comp[e.to];
        if (from == to) continue;
        dag.add_directed_edge(from, to, e.cost);
      }
    }
    // grouping scc
    group.resize(cnt);
    for (size_t i = 0; i < g.size(); i++) {
      group[comp[i]].emplace_back(i);
    }
  }

  int operator[](int k) const {
    return comp[k];
  }

private:
  Graph<T> rg;
  vector<int> order;
  vector<bool> used;

  void dfs(int idx) {
    if (used[idx]) return;
    used[idx] = true;
    for (auto &to : g[idx]) dfs(to);
    order.push_back(idx);
  }

  void rdfs(int idx, int cnt) {
    if (comp[idx] != -1) return;
    comp[idx] = cnt;
    for (auto &to : rg[idx]) rdfs(to, cnt);
  }
};
```

## 使用例

## 参考
- [強連結成分（SCC） | technical-note (hkawabata.github.io)](https://hkawabata.github.io/technical-note/note/Algorithm/graph/scc.html)
- [Strongly Connected Components(強連結成分分解) | Luzhiled’s Library (ei1333.github.io)](https://ei1333.github.io/library/graph/connected-components/strongly-connected-components.hpp)
