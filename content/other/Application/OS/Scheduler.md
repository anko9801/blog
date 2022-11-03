
## タスク
ジョブ、プロセス、スレッド

## プロセススケジューラ
スケジュールする単位はタスク。タスクはプロセスやスレッドによって作業している仕事のこと。

### 初期スケジューラ (~v2.4)
実行可能タスクは一般にランキューと呼ばれるキューに繋がれています。
$O(n)$

1. ランキューから順方向に全走査してタイムスライスが最大のタスクにコンテキストスイッチ。
2. インターバルタイマーが割り込み、タイムスライスを減らす。
3. タイムスライスを使い果たすとランキュー末尾に挿入。
4. 1~3をすべての実行可能タスクのタイムスライスが0になるまで繰り返し、すべて使い果たしたらタイムスライスを初期化。このとき、スリープ状態のタスクには少しタイムスライスを追加。

nice 値が小さいほどタイムスライスが貰える。有効範囲は -19 ~ 20 で root でないとマイナス値を設定できない。 `nice()` システムコールによって変更可能。

#### プリエンプション機能 (preemption)
生成直後かスリープ起床時のタスクはタイムスライスを普段より多く与えてランキュー末尾に挿入。

#### 複数LCPU対応
複数のLCPUが1つのランキューを見る。排他制御が必要。各LCPUは前に取ったタスクを取りやすい。

#### リアルタイムポリシー
リアルタイムタスク
`sched_setscheduler()` システムコールや `chrt` コマンドなどによってリアルタイムタスクにできる。常に優先的に動作可能。ハートビート処理など、システムの負荷が高いときでも定期的に動かないとまずいため、リアルタイムタスクにする。それが動いていれば他のタスクは永久にスケジュールされない。
`SCHED_OTHER` ポリシー : デフォルトのポリシー
`SCHED_FIFO` ポリシー : タイムスライスなし
`SCHED_RR` ポリシー : タイムスライスあり

### $O(1)$ スケジューラ (v2.6.0~2.6.22)
v2.6系は $O(1)$ スケジューラを用いて管理していた。
ランキューは active キューと expired キューに分けられ、active キューの先頭のタスクを取り、タイムスライスを使い切ったら expired キューに挿入するのを繰り返す。
nice 値ごとにキュー分けるようになって、優先度が高いキューから active キューを空にさせます。
これで LCPU 当たり $40\times 2$ 本のキューがあることになります。

#### ロードバランサー
LCPU ごとにキューをもつようになったのでロードバランサーが必要になりました。
NUMAシステムの場合は 2 階層のバランス処理が動作します。

1. NUMAノード間のバランス
2. ノード間LCPU間のバランス

NUMAノードの中で負荷の偏りを検出し、負荷が高いノードの一番負荷の高い LCPU から負荷の低いノードの一番負荷の低い LCPU へタスクを移動させる。

階層が複雑になると物理CPU > ダイ > CCX > コア > スレッドのそれぞれでバランスする。

移動を許す LCPU を制限する CPU affinity がある。タスクが動作可能なLCPUの集合を決められる。`sched_setaffinity()`システムコールや`taskset`コマンドによって設定できる。

### 対話型タスクの優先動作
bash や X Window System などの、人間が直接やりとりする、応答性が重視されるタスク指す。

- 単位時間あたりにスリープしている率が高いプロセスを対話型タスクとみなす。
- 対話型タスクに次のような優遇措置をとる
	- タイムスライスが切れると expired キューではなく active キューに移す。
	- nice 値相当の優先度を上げる。タイムスライスは変化しない。
	- その一方、ずっと実行可能なタスクは優先度を下げる。


### $O(1)$スケジューラの問題点
- 対話型タスクによるハング
1. 対話型タスクは優先度が上がり、 active キューに再挿入される。
2. 他のタスクはそもそも動けないから優先度が上がらない。
3. 少数の高優先度タスクが長時間動き続ける。
- 実行可能タスクが多いと、なかなかCPU時間が回ってこない。典型的には、実行可能なタスク数 × 100ms 待たされる。
- タイムスライスの粒度が荒いため、fair share scheduling や CFS bandwidth controllerなどのような細かい制御ができない。
	- 粒度を小さくするにはインターバルタイマーの割り込み頻度を増やせばよい。
	- 割り込み回数が増えると割り込みハンドラが動作する時間が増えるため、システム全体スループットが下がる。

### Completely Fair Scheduler (v2.6.23~)
v2.6.23 から入った Completely Fair Scheduler: CFS
赤黒木上で葉ノードに個々のタスク、キーはタスクごとに存在するvruntimeという値。
検索コスト $O(\log n)$ に最適化(vruntime 最小のタスクをキャッシュなど)を加えてよい。

ワンショットの高精度タイマーを用いることでタイムスライスが切れるときだけ割り込むので割り込み負荷を減らし、CFSが実現できるようになった。
レイテンシターゲット 数ms～数十ms を各タスクに均等に割り当てる。
nice 値が1低いと1.25倍多くタイムスライスが与えられる。

スケジューラアルゴリズムがプラガブルになった。スケジューリングクラスとコールバック関数を用意すればOK。

fair group scheduling(v2.6.24): cgroup でグループごとにロードバランスする。メモリ制限したりもできる。
Realtime group scheduling(v2.6.25): リアルタイムタスクが暴走すると通常のタスクは一切動けないため、1割程度他のタスクに渡す。
CFS bandwidth controller(v3.2.2): 個々のユーザーにCPU時間の制限を与えることができる。マルチテナントシステムであるユーザーにリソースが占有されないようにあるcgroup 内のタスクがperiodと呼ばれる所定時間内にruntimeと呼ばれる時間のみ動けるように制限する。
deadline scheduling class(v3.14): リアルタイムタスクの多様性を持たせる。
Energy-aware scheduling(v5.0): スマートフォンなどで使われるbig.LITTLE processing考え方を採用したCPUの為のもの。
- 処理性能が高いが消費電力が大きいCPU
- 処理性能が低いが消費電力が小さいCPU
重いタスクは前者に、軽いタスクは後者に実行させる。重いタスクがなければ全射のコアの動作周波数を下げたり、電源を切ったりすることによって消費電力を少なくするのが狙い。