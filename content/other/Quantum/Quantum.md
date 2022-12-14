---
title: "量子コンピュータ"
---

## 対象読者
- とにかく量子コンピュータを使ってみたい

## 1. 初めに

2019年10月23日、Googleの量子コンピューターが今までのコンピューターより速く計算出来るようになった！という事がありましたね。普段使っている暗号が解読され悪用されるかも！けど量子コンピューターとか量子アニーリング、名前は知ってるけど中で何やってるのかさっぱりわからん〜という人を見かけます。そこでここでは量子コンピューターを体感しつつ、仕組みを理解してこうと思います。量子回路を組む際に高校数学の知識は必要ですが、出来るだけ簡単に説明するのでそれ以上の量子力学などの知識は無くてもわかると思います。

## 2. 量子コンピューターってなに？

Wikipediaによると
>量子コンピュータ （りょうしコンピュータ、英語：quantum computer） は、量子力学的な重ね合わせを用いて並列性を実現するとされるコンピュータ。
量子コンピュータは「量子ビット」 (qubit; quantum bit、キュービット) により、重ね合わせ状態によって情報を扱う。
n量子ビットがあれば$2^n$の状態が同時に計算され、$2^n$個の重ね合わされた結果が得られる。しかし、重ね合わされた結果を観測してもランダムに選ばれた結果が1つ得られるだけで、古典コンピュータに対する高速性は得られない。(引用元:https://ja.wikipedia.org/wiki/量子コンピュータ)

量子コンピュータと対比して現在使われているコンピュータを古典コンピュータとします。

## 3. 量子コンピュータを使う事で嬉しいことは？

1994年にピーター・ショア（Peter Shor)は古典コンピュータでは解くことが難しい問題である素因数分解を量子計算によって容易な問題に落とすショーアのアルゴリズムというものを開発しました。それによって量子コンピュータの研究はとても盛んになりました。なぜなら暗号の中で今最もよく使われているRSA暗号は素因数分解を使って暗号化しているからです。それから色々な研究がなされ、機械学習を量子コンピュータで行ったり

## 4. 量子ビット

### ブラケット記法

量子力学において量子の状態の記述に必要なものがブラケット記法です。なんかめっちゃ難しそうと思うかもしれませんが、これはベクトルを言い換えただけと思って構わないです。左がケット・ベクトル、右がブラ・ベクトルと呼びます。この記事内ではケット・ベクトルだけを使いますのでブラとケットを区別しないで、あぁただのベクトルねと思って大丈夫です。

$$
\ket{\psi} = \begin{pmatrix} 
a_0 \\
a_1 \\
\vdots \\
a_n \\
\vdots
\end{pmatrix}
\bra{\psi}
$$

### 量子ビット
古典コンピュータで扱うビットは0と1という数値を扱います。これは電圧の高低によって表現します。それに対して量子コンピュータは数値ではなく固有の状態を持ちます。これは古典コンピュータのビットとはとてもかけ離れたものであるので量子ビット・qubitと区別します。そして古典コンピュータのビットと対応する状態を$\ket{0}$と$\ket{1}$とします。$\ket{0}$と$\ket{1}$はその状態なら操作をしなければその状態のままで、$\ket{0}$と$\ket{1}$以外の状態にはなりません。

2量子ビットの場合はそれぞれが独立な為それらの組み合わせで表現できます。これは直積を取るといい、2量子ビットの状態は$\ket{0}\otimes\ket{1}$と表します。もっと増えてn量子ビットの場合もこれで表現できますが、面倒くさいので
$$ \ket{011} = \ket{0}\otimes\ket{1}\otimes\ket{1} $$
と表すことにします。

### 重ね合わせの状態
量子ビットは$\ket{0}$と$\ket{1}$だけではなく重ね合わせの状態を取る事ができ、このように表します。($c_0$と$c_1$は複素数)

$$
\ket{\psi} = c_0\ket{0} + c_1\ket{1}\\
|c_0|^2 + |c_1|^2 = 1
$$

このように$\ket{0}$と$\ket{1}$にそれぞれ複素数を掛けたものとなります。現実世界に複素数？どうしてそうなるの？と言われても実験での事実と矛盾が起きないからとしか言いようがありません。

重ね合わせの状態の例としてこのようなものを考えます。

$$
\ket{\psi} = \frac{1}{\sqrt{2}}\ket{0}+\frac{1}{\sqrt{2}}\ket{1}
$$

$\ket{\psi}$と$\ket{1}$の2量子ビットの時は

$$
\ket{\psi} \otimes \ket{1} = \frac{1}{\sqrt{2}}\ket{01}+\frac{1}{\sqrt{2}}\ket{11}
$$

と表し、$\ket{\psi}$がn量子ビットあったら

$$
\begin{align}
\ket{\psi}^n &= \frac{1}{\sqrt{2^n}}(\ket{0}+\ket{1})^n \\
&= \frac{1}{\sqrt{2^n}}(\ket{00\cdots0}+\ket{00\cdots1}+\cdots+\ket{11\cdots1})
\end{align}
$$

となります。一般に2量子ビットの重ね合わせの状態は

$$
\begin{align}
&(c_0\ket{0} + c_1\ket{1}) \otimes (c_2\ket{0} + c_3\ket{1}) \\
&= c_0c_2\ket{00} + c_0c_3\ket{01} + c_1c_2\ket{10} + c_1c_3\ket{11}
\end{align}
$$

となります。このような重ね合わせの状態にし、その状態のまま様々な演算を行う事で並列処理を可能にし量子計算を行えます。

### どうやって作られているの？

※ここは速く実際に動かしてみたいという方は飛ばして構わないです
ここまでは表記的な部分について触れてきましたが、これはどのように生成されているのか気になります。

これは主に2つあります。

- 光子の偏光
- スピン

#### 光子の偏光

EPRパラドックス
アスペの実験
ベルの不等式

#### スピン

シュテルン・ゲルラッハの実験

## 5. 量子論理ゲート

古典コンピュータではANDやOR、NOT、XORなどの古典論理ゲートのようなものを使って計算します。対して量子コンピュータでは量子論理ゲートを用いて計算します。ユニタリ変換でありほとんどの演算がn対n対応
量子計算をするためには量子論理ゲートは必須のものとなります。

### 1. パウリゲート
パウリゲートは3種類あり、Xゲート、Yゲート、Zゲートがあります。XゲートはNOTゲート、Zゲートは位相のNOTゲートとみれます。X,Y,Zゲートは$\ket{0}$、$\ket{1}$どちらも2回作用すると元に戻ります。確かめてみてください。

$$
\begin{align}
&X:\ket{0} \rightarrow \ket{1}, \quad \ket{1} \rightarrow \ket{0} \\
&Y:\ket{0} \rightarrow i\ket{1}, \quad \ket{1} \rightarrow -i\ket{0} \\
&Z:\ket{0} \rightarrow \ket{0}, \quad \ket{1} \rightarrow -\ket{1}
\end{align}
$$

### 2. アダマールゲート
アダマールゲートは量子ビットの回転を表し、$H$と表記されます。

$$
\ket{0} \rightarrow \frac{1}{\sqrt{2}}(\ket{0} + \ket{1}) \\
\ket{1} \rightarrow \frac{1}{\sqrt{2}}(\ket{0} - \ket{1})
$$

アダマールゲートも2回作用すると元に戻るという性質があります。パッと見分かりませんがこのようにして分かります。これは$\ket{0}$の場合ですが$\ket{1}$も同様です。

$$
\begin{align}
H(H(\ket{0})) &= H(\frac{1}{\sqrt{2}}(\ket{0} + \ket{1})) \\
&= \frac{1}{\sqrt{2}}(\frac{1}{\sqrt{2}}(\ket{0} + \ket{1})) + \frac{1}{\sqrt{2}}(\frac{1}{\sqrt{2}}(\ket{0} - \ket{1})) \\
&= \frac{1}{2}\ket{0} + \frac{1}{2}\ket{1} + \frac{1}{2}\ket{0} - \frac{1}{2}\ket{1} \\
&= \ket{0}
\end{align}
$$

### ブロッホ球

### 3. 制御ユニタリゲート・古典制御ゲート

#### 制御ノットゲート CNOTゲート
CNOTゲートは1つの量子ビットの状態によって、もう1つの量子ビットにNOTゲートを作用させるゲートです。この状態を判断する量子ビットを制御ビット(control qubit)、作用させる量子ビットを標的ビット(target qubit)と呼びます。制御ビットが$\ket{1}$である場合のみ標的ビットに作用させます。左は制御ビット、右は標的ビットです。作用後の標的ビットは制御ビットと標的ビットの排他的論理和と同値と分かります。

$$
\begin{align}
&\ket{00} \rightarrow \ket{00} \\
&\ket{01} \rightarrow \ket{01} \\
&\ket{10} \rightarrow \ket{11} \\
&\ket{11} \rightarrow \ket{10}
\end{align}
$$

例えば、一般の2量子ビットがあり、それについてCNOTゲートを作用させると

$$
\begin{align}
&(c_0\ket{0} + c_1\ket{1}) \otimes (c_2\ket{0} + c_3\ket{1}) \\
&= c_0c_2\ket{00} + c_0c_3\ket{01} + c_1c_2\ket{10} + c_1c_3\ket{11} \\
&\rightarrow c_0c_2\ket{00} + c_0c_3\ket{01} + c_1c_2\ket{11} + c_1c_3\ket{10}
\end{align}
$$


#### トフォリゲート
トフォリゲートはCNOTゲートの3つ版で、2つの量子ビットの状態がどちらも$\ket{1}$のとき、もう1つの量子ビットにNOTゲートを通すゲートです。

$$
\begin{align}
&\ket{000} \rightarrow \ket{000} \\
&\ket{010} \rightarrow \ket{010} \\
&\ket{110} \rightarrow \ket{111}
\end{align}
$$

### 観測

最終的に量子論理ゲートによって作られた状態から確率的に0か1の値を取り出すゲートです。その確率は状態に依存します。例えば、$\ket{0}$,$\ket{1}$はそれぞれ絶対的に0,1となり、 $\frac{1}{\sqrt{2}}(\ket{0} + \ket{1})$,$\frac{1}{\sqrt{2}}(\ket{0} - \ket{1})$は50%の確率で0か1となります。このように状態が確定する事を量子力学では「波束の収束」と言います。波束の収束をするとその量子ビットは壊れ、ゲートを通しても変化は起こりません。

その他のゲートは[IBM Q Experience](https://quantum-computing.ibm.com/)や[OpenQASMのライブラリ](https://github.com/Qiskit/openqasm/blob/master/examples/generic/qelib1.inc)で感じとってください。

#### IBM Q Experience

IBM Q Experienceとはクラウド上で量子回路を構築し、シミュレーションや実際の量子コンピュータに計算させることが出来るIBMのシステムです。実際の量子コンピュータを使う時は5量子ビットだけという制限はありますが誰でも扱う事が出来ます！

次は実際に量子論理ゲートを使って簡単な量子計算を行ってみます。[IBM Q Experience](https://quantum-computing.ibm.com/)へ飛んでサインアップをすると使えるようになります。

#### スワップゲート

量子計算の簡単な例としてスワップゲートがあります。スワップゲートとは2量子ビット間において状態を交換するような演算です。

$$
\begin{align}
&\ket{00} \rightarrow \ket{00} \\
&\ket{01} \rightarrow \ket{10} \\
&\ket{10} \rightarrow \ket{01} \\
&\ket{11} \rightarrow \ket{11}
\end{align}
$$

このようになるものを作ります。具体的にはCNOTゲートを交互に3つ置くと作れます。

<img width="199" alt="スクリーンショット 2019-11-03 22.56.10.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/227781/d7b318fb-0d9d-4312-5a62-2764aa3a891b.png">

これをIBM Qに置きます。制御ビットと標的ビットの順番が逆の場合、ゲートをクリックすると、バツマークとえんぴつマークが出るのでえんぴつマークを押して、繋がっている線を入れ替えましょう。そして、スワップゲートの前後に$\ket{10}$(下から読みます)の状態を作る為にXゲート、値を取り出す為にそれぞれの量子ビットを観測します。観測する際どの古典ビットに埋め込むか指定する為に左のCircuit EditerからOpenQASMのコードを変更しましょう。するとこのようになります。

<img width="1440" alt="スクリーンショット 2019-11-09 19.47.16.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/227781/5491aeff-888b-08fd-414d-b07176756ffb.png">

これを右上のボタンで保存してRunしてみます...！しばらく待ち、Resultを見てみると...

<img width="1257" alt="スクリーンショット 2019-11-09 19.42.50.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/227781/b6fcb9b8-ad7c-ef86-512a-bb0e79d78fa1.png">

このように結果が出ます！100% $\ket{01}$という事で成功してますね！！！やった！！！

## エンタングルメント

量子計算の上で非常に重要なものとしてエンタングルメントというものがあります。説明の前にまずは下のように制御ビットは$\frac{1}{\sqrt{2}}(\ket{0} + \ket{1})$、標的ビットは$\ket{1}$のCNOTゲートを作用させます。

$$
\begin{align}
&\frac{1}{\sqrt{2}}(\ket{0} + \ket{1}) \otimes \ket{1} \\
\rightarrow &\frac{1}{\sqrt{2}}(\ket{01} + \ket{10})
\end{align}
$$

この時、CNOTゲートを作用する前までは量子ビットの積で表せられていましたが、作用した後では積で表す事が出来なくなっています。これはつまり、量子ビットの一方が決まった時、もう一方も自然と決まってしまう事になります。この場合だと、左量子ビットが$\ket{0}$と観測されるともう一方の量子ビットは$\ket{1}$となります。その逆もまた然り。

このようにエンタングルメントとは複数の量子ビットが存在し、それらが各々の量子ビットの状態の積で表せない状態の事を言います。日本語では量子もつれ、絡まった状態と言います。EPRペア

## 量子クローニングの不可能性

### 量子テレポーテーション

量子は通常

量子クローニングの不可能性をどうやってかいくぐっているのか

エンタングルメントの具体的な例として量子テレポーテーションというものがあります。量子テレポーテーションは二つの量子が量子情報の世界では情報の送り主をアリス、お届け先をボブと呼びます。

1. アリスはボブへ量子情報を送ります。
2. アリスは自分の量子を観測し、量子状態を知ります。
3. ボブはアリスが知った量子状態から同じ量子状態にする事が出来ます。

粒子が空間の別の場所に瞬間移動するものとは異なります。


<img width="450" alt="スクリーンショット 2019-11-03 23.05.39.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/227781/2bb5ec39-5442-92af-660f-d06decafe44b.png">

<img width="1259" alt="スクリーンショット 2019-11-09 21.35.02.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/227781/25c06381-512c-abe7-5d54-8f51b52f383b.png">


## ショアのアルゴリズム

素因数分解を古典・量子コンピュータを併用する事で今までよりも早く行えるようになるアルゴリズムです。

### 量子フーリエ変換

$$
\ket{j} \rightarrow \frac{1}{\sqrt{2^n}}\sum_{k=0}^{2^n-1}exp(\frac{i2\pi jk}{2^n})\ket{k}
$$

## 量子エラーコレクション
これは沢山研究されていて


まだまだ研究されているあります。


[Quantum Algorithm Zoo](https://quantumalgorithmzoo.org/)
