---
title: "回路"
---

回路全然わからんから完全に理解するまでの軌跡を書く.
高校に遡ると私はコンピュータクラブに所属していて, 先輩に誘われ, 水中ロボコンにプログラミング担当として参加した. しかしプログラミングの方が好きで1ミリも回路なんか知らずにやってたので何もわからず, 優勝して終わった. 高学年になってくると低レイヤーが好きになっていき. 次第に回路にも興味が出るようになったが当時手に入る文献は簡単なものしかなく Arduino やラズパイでちょっと触るくらいで基板などには手が届かなかった. そして受験を経て大学に入り, 東工大のロ技研というロボットを製作するサークルに入った. そこでは念願の回路設計班に志望し入れた. しかし, 自分の専門が物理学系で完全にアウェイな環境で回路がよくわからずみんなに先越され, どう学べばいいのかわからず幽霊になってた.
1年後, 友人が自作 LSI をしていて自分も作りたくなったり, 自分が主催しているセミナーの講師が回路に詳しいということで再びデジタル回路に興味を持ち, その一環としてロ技研での活動を再開することにした. 

理解したい一心でやる.

## 1日目 前提知識

まず高校で習った回路を思い出してみる.

- 回路とは電気が電源の+から動かしたい電子部品や導線を通って-に戻ってくる1周の輪.
- 電流は電荷の流れ. 電源が電位を上げ, 電流を流し, 電子部品が電位を下げる.
- 任意のループについて, 電位変化は0となる.
- 任意の節点について, 電流は保存する.
- 任意の区間について, 電圧, 電流, 抵抗にはオームの法則 $V=IR$ が成り立つ.
- 使うエネルギーは電力 $P = VI$ で分かる. 電力が大きすぎると発熱し, 壊れる危険がある.
- コンデンサに電圧を掛けると電荷 $Q = CV$ が集まる.

こんな感じかな. あとは交流回路もやったけど知識として少ないから後でまとめる.

## 2日目 交流回路の基礎

大学で回路の授業を取ったのでそれを復習する.

交流回路は電圧が波の形をしている. 位相は三角関数より複素数の方が扱いやすいのでそれを使う. ちなみにその虚部は実際に観測されない. そのままだと電流と虚数単位の記号が被さるから電流 $i$ は虚数単位 $j$ と書く.
- 実数表示だと $v = V_m\sin(\omega t + \phi)$
- 複素表示だと $v=V_me^{j(\omega t + \phi)}$

| 電子部品       | 関係式                                 | 直流回路 | 交流回路                   |
| -------------- | -------------------------------------- | -------- | -------------------------- |
| 抵抗 $R$       | $v = Ri$                               | $v = Ri$ | $v = Ri$                   |
| コイル $L$     | $v = L\frac{\mathrm{d}i}{\mathrm{d}t}$ | $v=0$    | $v = j\omega Li$           |
| コンデンサ $C$ | $i = C\frac{\mathrm{d}v}{\mathrm{d}t}$ | $i=0$    | $v = \frac{1}{j\omega C}i$ |

|                    | 関係式       | 実部               | 虚部             |
| ------------------ | ------------ | ------------------ | ---------------- |
| インピーダンス $Z=V/I$ | $Z = R + jX$ | 抵抗 $R$           | リアクタンス $X$ |
| アドミタンス $Y=I/V$   | $Y = G + jB$ | コンダクタンス $G$ | サセプタンス $B$ |

と書いたけど多分使うのはインピーダンスくらいだと思う. インピーダンスと言えば交流回路の抵抗と思えば良さそう.

### RLC 回路
RLC回路とは抵抗 $R$ コイル $L$ コンデンサ $C$ を直列や並列に色々繋いだとき共振という良い性質が出てくる回路.
例えば最も簡単な例としてそれらを直列に繋いだとき, そのインピーダンスを計算すると

$$
\begin{aligned}
Z &= R + j\left(\omega L - \frac{1}{\omega C}\right) \\
|Z| &= \sqrt{R^2 + \left(\omega L - \frac{1}{\omega C}\right)^2}
\end{aligned}
$$

この式を見ると, ある周波数 $\omega = 1/\sqrt{LC}$ で共振といって電流が最大となる. その他の周波数では少ない. 言葉では説明できないや, 後でグラフを置いとこ.

| 固定                                 | 可変                          |
| ------------------------------------ | ----------------------------- |
| 抵抗                                 | ボリューム抵抗                |
| コイル                               | 変成器 (トランス)             |
| 電解コンデンサ, セラミックコンデンサ | バリコン (可変容量コンデンサ) |

この回路はラジオに使われている.

### インダクタンス

### 回路方程式
重ね合わせの理 // TODO 図
- 複数の電源をもつ回路において, それぞれ独立な電源として計算した電流, 電圧を重ね合わせたものと等しい.
- 電圧源: 電圧 = 0 $\implies$ 短絡
- 電流源: 電流 = 0 $\implies$ 解放

**Thm. 相反定理**
$$
\begin{pmatrix}
V_1 & V_2 & \cdots & V_n
\end{pmatrix}
\begin{pmatrix}
I_1 \\ I_2 \\ \vdots \\ I_n
\end{pmatrix}
=
\begin{pmatrix}
V_1' & V_2' & \cdots & V_n'
\end{pmatrix}
\begin{pmatrix}
I_1' \\ I_2' \\ \vdots \\ I_n'
\end{pmatrix}
$$
**Proof.**
**Thm. 補償定理**
**Thm. 鳳・テブナンの定理**
**Thm. Nortonの定理**
**Thm. 最大電力伝送定理**

三相交流

なるほど～大体理解した.

### 電子回路工作

とりま良い資料と言われたのでこれを全部読む.
- [回路講習会2017の資料 | Molina's Web Site](https://molina.jp/blog/%E5%9B%9E%E8%B7%AF%E8%AC%9B%E7%BF%92%E4%BC%9A%E3%81%AE%E8%B3%87%E6%96%99/)
	- ダンチで分かりやすくて面白かった.

やること
1. KiCad をインストールする.
2. 新しい project を作る.
3. スケッチを書く.
	- データシートを読む
	- A: 電子部品の追加
	- W: Wire
	- M: Move
	- G: 配線も一緒に動かす
	- E: Edit
	- V: Value
4. PCB ファイルを書く.

### 回路

- LED + 抵抗
- モーター + FET

ノイズ対策まとめ
- 入力端子に何も繋がないと壊れる
- ICの電源には必ずパスコン(バイパスコンデンサ)を付ける.
	- 0.1uFのセラミックコンデンサ
	- 電力補助
- パターンが短く、周波数が高い(1MHz)とそれ自体がコンデンサ化する
- 回路以外の部分をグランド化することで静電シールドとなる
- グランドは太く短く
折り返し(反射)
- 終端抵抗

水晶振動子
こるピッツ発振回路

### メカ
ギア
- そのままだと回転数 高 トルク小なので
- ギアを噛ませて減速 トルク大して使うのが一般的

PWM制御 (Pulse Width Modulation)
- 周波数の高いパルス波を断続的に流して相対的に電流量を減らす.
- Duty比: パルス波のHighの割合

### モーター
**モーターの種類**

DCモーター
- FA-130RA
	- 模型用モーター
- ロボサイトギアモーターRA25
サーボモーター
- モーターと処理回路が入っていて指定した角度になるようにしてくれる
ステッピングモーター
- パルス波1つにつき1段階回転する

**モーターコントローラー**
- 何もしないと1方向しか回らない
- Hブリッジ回路を組むと, 正転, 逆転, ブレーキができる.
- ブレーキ: 逆起電力
- MOS-FET を4つ

[趣味の電子回路工作　目次 (piclist.com)](http://www.piclist.com/images/www/hobby_elec/menu.htm)

## 3日目
真空管
400V掛けて電子が飛び出て
プレート網目でキャッチ.
+の電荷をためるとそこに引きつく
高周波だと散乱しがちなのでローパスフィルタになりがち
100mAが限界
10W 100mA * 100

周波数特性から抜け出す

コンデンサを取り払う -> オペアンプ
交流 400V, 1000Vにオフセット200V, 500Vを付けてた. 逆流を防ぐ為にコンデンサを使っていた.
基準電位から
DC デカップリング

2極管

I2Cは高周波だけど, オープンコレクタ回路なので終端抵抗付けてはだめ
VGAのケーブルはディスプレイ表示はI2C
CMOSのオープンコレクタはノイズに強い

オープンコレクタはトランジスタでNOT取って沢山ORを取ったもの

SPIはオープンコレクタではない
チップセレクト

PlatformIO


## 参考
- [回路講習会2017の資料 | Molina's Web Site](https://molina.jp/blog/%E5%9B%9E%E8%B7%AF%E8%AC%9B%E7%BF%92%E4%BC%9A%E3%81%AE%E8%B3%87%E6%96%99/)
- [The Electronic Lives Manufacturing - presented by ChaN (elm-chan.org)](http://elm-chan.org/index_j.html)
- [My Tube Amp Manual (op316.com)](http://www.op316.com/tubes/tips/tips0.htm)
