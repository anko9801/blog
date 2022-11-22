---
title: "単一始点最短路 (Dijkstra)"
---

## 説明
有向グラフに負の辺が存在しないとき次の事がいえる。

「まだ最短距離が確定していない点の中で、始点からの距離が最小 $\iff$ 最短距離として確定」

## 仕様

- `ShortestPath<T>`
	- 始点から終点までの距離 `dist` 
	- 始点からの最短路のパスで終点の1つ前の点 `from`
- `ShortestPath<T> dijkstra(const Graph<T> &g, int s)`
	- グラフ `g` について始点 `s` からの最短経路を計算する.
	- 密グラフ 単純に探索 $O(V^2)$
	- 疎グラフ ヒープを用いると $O(E\log{V})$

## 実装

```cpp
template <typename T>
struct ShortestPath {
  vector<T> dist;
  vector<int> from;
};

template <typename T>
ShortestPath<T> dijkstra(const Graph<T> &g, int s) {
  const auto INF = numeric_limits<T>::max();
  vector<T> dist(g.size(), INF);
  dist[s] = 0;
  vector<int> from(g.size(), -1);
  pq<pair<T, int>> q;
  q.emplace(dist[s], s);

  while (!q.empty()) {
    auto [cost, idx] = q.top(); q.pop();
    if (dist[idx] < cost) continue;
    for (auto &e : g[idx]) {
      if (dist[e.to] > dist[idx] + e.cost) {
        dist[e.to] = dist[idx] + e.cost;
        from[e.to] = idx;
        q.emplace(dist[e.to], e.to);
      }
    }
  }
  return {dist, from};
}
```

## 使用例


## 参考
- [ダイクストラ法のよくあるミスと落し方 - あなたは嘘つきですかと聞かれたら「YES」と答えるブログ (hatenablog.com)](https://snuke.hatenablog.com/entry/2021/02/22/102734)
