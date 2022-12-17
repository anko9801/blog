---
title: "ディープラーニング"
---

## モデル
- GPT
- BERT
	- [Tiiiger/bert_score: BERT score for text generation (github.com)](https://github.com/Tiiiger/bert_score)
- Transformer
	- [作って理解する Transformer / Attention - Qiita](https://qiita.com/halhorn/items/c91497522be27bde17ce)

## AI セキュリティ
- バックドア
	- 特定の入力データ(トリガー)を意図したクラスに誤分類させる
	- トリガー以外の入力データは正しく分類されるので検知は困難
	- Mitigation
		- 信頼できないドメインから入手した事前学習モデルを使用しない
		- 汚染されていないデータで再学習する
- 敵対的サンプル
	- 誤分類を誘発させる
	- AIと人間両方を騙す攻撃
		- ex. スパムメールフィルタを騙した上で人間も騙してクリックさせる
	- AIを騙し人間には理解させる攻撃
		- ex. 誹謗中傷コメント (バ力(ちから))
	- Mitigation
		- 敵対的学習: ぼかしやノイズなどを加えたサンプルを用いる
		- 蒸留: 巨大なネットワークをなるべく精度を落とさずに小さなネットワークにする手法 (Why?)
		- アンサンブル・メソッド: 複数のAIを組み合わせて学習することで頑健性を上げる(Why?)
		- Autoencoderによる検出: データを圧縮して復元するモデルを用いて敵対的サンプルを検出する (How?)
- モデル/データ抽出攻撃
	- メンバーシップ推論 ある画像が学習データ(メンバーシップ)に含まれているかは過剰に反応するかしないか
	- Mitigation
		- クエリアクセスに対して信頼スコアなどの不必要な情報を応答しない
		- 過学習を抑制する
- モデルの脆弱性
	- TensorFlowのLambdaレイヤーで任意コード実行ができるので事前学習モデルに悪意あるコードを埋め込める。
	- Mitigation
		- 信頼できないドメインから入手した事前学習モデルを使用しない
		- サンドボックス内での実行

[AIディフェンス研究所 (jpsec.ai)](https://jpsec.ai/)

## 自作深層学習フレームワーク