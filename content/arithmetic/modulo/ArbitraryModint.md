---
title: "任意Modint"
---

## 説明

| 演算                   | 方法                          | 計算量                      |
| :--------------------  | :---------------------------- | :------------------------- |
| 足し算   $a + b$       | 足してN以上になったらN引く    | $O(1)$            |
| 引き算   $a - b$       | 引いて0未満になったらN足す    | $O(1)$            |
| 掛け算   $a \times b$  | 掛けてNで割った余り           | $O(1)$            |
| 割り算   $a \div b$    | 拡張ユークリッドの互除法      | $O(\log^2 N)$   |
| 累乗     $a ^ e$       | 繰り返し二乗法                | $O(\log N)$       |
| 平方根   $\sqrt{a}$    | Tonelli Shanksのアルゴリズム  | $O(\log^2 N)$   |
| 累乗根   $\sqrt[e]{a}$ | Tonelli Shanksのアルゴリズム     | $O(\min(N^{1/4},\sqrt{e})\log{e}\log^2{N})$     |
| 対数     $\log_e{a}$   | 離散対数問題            | $O(\sqrt{N})$     |

(参考: [整数論テクニック集のpdf](https://kirika-comp.hatenablog.com/entry/2018/03/12/210446) など)

## 実装

```cpp
#include <cstdint>
#include <istream>
#include <vector>

using ll = long long;

class runtime_modint {
  using u64 = std::uint_fast64_t;

  static u64 &mod() {
    static u64 mod_ = 0;
    return mod_;
  }

public:
  u64 a;

  runtime_modint(const u64 x = 0) : a(x % get_mod()) {}
  u64 &value() noexcept { return a; }
  const u64 &value() const noexcept { return a; }
  inline constexpr operator ll() const noexcept { return a; }
  runtime_modint operator+(const runtime_modint rhs) const {
    return runtime_modint(*this) += rhs;
  }
  runtime_modint operator-(const runtime_modint rhs) const {
    return runtime_modint(*this) -= rhs;
  }
  runtime_modint operator*(const runtime_modint rhs) const {
    return runtime_modint(*this) *= rhs;
  }
  runtime_modint operator/(const runtime_modint rhs) const {
    return runtime_modint(*this) /= rhs;
  }
  runtime_modint &operator+=(const runtime_modint rhs) {
    a += rhs.a;
    if (a >= get_mod()) {
      a -= get_mod();
    }
    return *this;
  }
  runtime_modint &operator-=(const runtime_modint rhs) {
    if (a < rhs.a) {
      a += get_mod();
    }
    a -= rhs.a;
    return *this;
  }
  runtime_modint &operator*=(const runtime_modint rhs) {
    a = a * rhs.a % get_mod();
    return *this;
  }
  runtime_modint &operator/=(runtime_modint rhs) {
    u64 exp = get_mod() - 2;
    while (exp) {
      if (exp % 2) {
        *this *= rhs;
      }
      rhs *= rhs;
      exp /= 2;
    }
    return *this;
  }

  template <class T> inline constexpr runtime_modint &operator+=(T x) noexcept {
    return operator+=(runtime_modint(x));
  }
  template <class T> inline constexpr runtime_modint &operator+(T x) noexcept {
    return runtime_modint(*this) += x;
  }
  template <class T> inline constexpr runtime_modint &operator-=(T x) noexcept {
    return operator-=(runtime_modint(x));
  }
  template <class T> inline constexpr runtime_modint &operator-(T x) noexcept {
    return runtime_modint(*this) -= x;
  }
  template <class T> inline constexpr runtime_modint &operator*=(T x) noexcept {
    return operator*=(runtime_modint(x));
  }
  template <class T> inline constexpr runtime_modint &operator*(T x) noexcept {
    return runtime_modint(*this) *= x;
  }
  template <class T> inline constexpr runtime_modint &operator/=(T x) noexcept {
    return operator/=(runtime_modint(x));
  }
  template <class T> inline constexpr runtime_modint &operator/(T x) noexcept {
    return runtime_modint(*this) /= x;
  }

  static void set_mod(const u64 x) { mod() = x; }
  static u64 get_mod() { return mod(); }
};

ll n, m;
bool used[100000];
std::vector<std::pair<ll, runtime_modint>> G[100000];

runtime_modint dfs(ll x) {
  used[x] = true;
  bool end = true;
  runtime_modint res = 1;
  for (auto &p : G[x]) {
    if (used[p.first] == true)
      continue;
    end = false;
    if (p.second != 0) {
      res *= p.second;
    } else {
      p.second = dfs(p.first);
      // cout << x MM p.fi MM p.se << endl;
      res *= p.second;
    }
  }
  used[x] = false;
  return res + 1;
}
```

## 使用例


## 関連項目
- [Modint](Modint.md)