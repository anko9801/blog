---
title: "SSSA Attack"
---

## 説明
$$
\newcommand{\calO}{\mathcal{O}}
\newcommand{\FF}{\mathbb{F}}
\newcommand{\tE}{\tilde{E}}
\newcommand{\ZZ}{\mathbb{Z}}
\newcommand{\QQ}{\mathbb{Q}}
\DeclareMathOperator{\Ker}{Ker}
\DeclareMathOperator{\ord}{ord}
$$
## Definition
- $\FF_p$
    - 素数$p$に対して$p$個の元からなる有限体
    - 素数$p$の剰余群
    - $\mathbb{Z}/p\mathbb{Z}$
- $\mathbb{Q}_p$
    - p進数体
    - $27=2.5+2.25 + 27.125 + O(5^4)$ 射影極限による導入
    - $27 = 2 + 5^2 + O(5^3)$ p進展開による導入
    - $\mathbb{Q}$のp進付値による完備化をする導入
    - $n\times 100 = n.10^{-2}$
- $\mathbb{Z}_p$
    - p進整数環
    - $Q_p$の整数部分
    - 論文にも注釈があるが$\mathbb{Z}/n\mathbb{Z}$の略記$\mathbb{Z}_n$とは別物
- $\mathbb{Z}\text{-algebra}$に対する$\mathbb{P}^n(R)$
    - $\mathbb{Z}$から$R$への凖同型がある
    - 雪江代数学2巻 Def. 1.3.13 (p.15)の定義
      + $k, A$を環とする.　$k$から$A$への準同型があるとき, $A$を$k$代数, あるいは$k$上の代数という.
- $\calO=(0 : 1 : 0)$で楕円曲線の群演算に関する単位元
- 持ち上げ
    - 準同型の移動
- Rが体のとき$E(R)$は群であることは受け入れる
- $\mathscr{L}$は同型写像

$\mathbb{Q}_p$, $\mathbb{Z}_p$ はそれぞれp進体、p進整数環とする。

次の同型写像 $\lambda_E$ を用いて楕円曲線 $\tilde{E}(\mathbb{F}_p)$ 上が $\mathbb{F}_p$ 上の DLP、つまり $\bmod p$ の割り算に帰結する。

$$
\lambda_E \colon \tilde{E}(\mathbb{F}_p) \overset{u}{\to} E(\mathbb{Q}_p)\xrightarrow{p倍}\ker\pi \xrightarrow{\mathscr{L}} p\mathbb{Z}_p\xrightarrow{\bmod p^2}p\mathbb{Z}_p/p^2 \mathbb{Z}_p\simeq \mathbb{F}_p
$$



## 参考

[Fermat Quotient と Anomalous 楕円曲線の離散対数の多項式時間解法アルゴリズムについて(代数的整数論とその周辺)](https://repository.kulib.kyoto-u.ac.jp/dspace/bitstream/2433/61761/1/1026-15.pdf)
