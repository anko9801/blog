---
title: "数学の花"
---
## 二項演算の定義
集合$S\neq \phi$ を考える。$\forall a,b\in S$ に対して$\exists c \in S$ を対応させる法則つまり直積$S\times S$から$S$への写像を2項演算(law of composition)と呼ぶ.　記号的には
$$
\begin{align}
S\times S &\to S\\
(a,b)&\mapsto a\cdot b=c
\end{align}
$$
と表現される. 
## 群の定義
集合$G\neq \phi$について, 2項演算
$$
\begin{align}
G\times G &\to G\\
(a,b)&\mapsto a\cdot b=c
\end{align}
$$
が与えれていて, 次の3つの条件を満たすとき, $G$を群と呼ぶ.

G1. **結合法則 (associative law)** $\forall a,b,c \in G$について
$$
\begin{align}  \\
a\cdot (b\cdot c)=(a\cdot b)\cdot c
\end{align}
$$
G2. **単位元の存在** ある$e\in G$が存在して, $\forall a\in G$について
$$
e\cdot a=a\cdot e=a
$$
この$e$をGの**単位元(unit element)** という.
G3. **逆元の存在** $\forall a\in G$についてある$b\in G$が存在して
$$
\begin{align}
a\cdot b=b\cdot a=e
\end{align}
$$
となる. この$b$を$a$の**逆元(inverse element)** とよび,$a^{-1}$と表記する.
## 様々な概念
群以外の概念について
- $(G,\cdot)$が半群: G1つまり結合法則が成立するもの
- $(G,\cdot)$がモノイド: G1とG2つまり結合法則が成立し単位元が存在するもの. 
- 群
- $(G,\cdot)$が可換群, アーベル群: 群に**交換法則**を追加したもの. つまり, $\forall a,b\in G$について
$$
\begin{align}
a\cdot b=b\cdot a
\end{align}
$$
が成立

--- 
ここからは二項演算が$(G,\cdot)$,$(G,+)$の二つになる.　

- $(G,+,\cdot)$が環: $(G,+)$が可換群$(G,\cdot)$がモノイド
- $(G,+,\cdot)$が可換環: $(G,+,\cdot)$が環であり, かつ$(G,\cdot)$について交換法則が成立する
- $(G,+,\cdot)$が斜体: $(G,+,\cdot)$が環であり, かつ$(G,+)$の単位元$0$以外の元が**単元**である, つまり$(G,\cdot)$について逆元をもつ
- $(G,+,\cdot)$が体: $(G,+,\cdot)$が可換環かつ斜体
## 群の性質
>**Props 1.1** 群$(G,\cdot)$に対して単位元はただ一つ存在

証明: $e,e'\in G$を$G$の単位元としたとき($e$を群の定義から保証された)
 