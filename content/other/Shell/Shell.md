---
title: "シェル"
---

Shell

シェル入門
- [雰囲気でシェルを使っている人のためのシェル入門 | κeenのHappy Hacκing Blog (keens.github.io)](https://keens.github.io/blog/2017/10/17/fun_ikideshieruwotsukatteiruninnotamenoshierunyuumon/)
- [bash tips - ももいろテクノロジー (hatenablog.com)](https://inaz2.hatenablog.com/entry/2014/12/14/013234)

`/dev/ttyS0`

Shell Shock (CVE-2014-6271)
- [bash の脆弱性 "Shell Shock" のめっちゃ細かい話 (CVE-2014-6271) - もろず blog (hatenablog.com)](https://moro-archive.hatenablog.com/entry/2014/09/27/200553)


コマンド
- sftp
- scp
- rsync
- shopt
- [gum を使ってシェルスクリプトの表示をカッコよくする (zenn.dev)](https://zenn.dev/kou_pg_0131/articles/gum-introduction)

## Makefile

## 冪等性 (idempotency)

## シェル芸

## 新しいシェルスクリプト
元からべき等性を担保しててパイプがモナドである型システムを持つシェルスクリプトあったら嬉しいのかな?
これ、べき等性を担保したコマンドを作るのとシェルスクリプトはレイヤーが違う.

逆にべき等性にしてほしくないこととは
- Aを追加するとき, Aが既にあるときエラーが出てほしい
- Aを削除するとき, Aが既にないときエラーが出てほしい
- Aを表示するとき, Aが一つもないときエラーが出てほしい

エラー型が同じになるようにするべき

$$
\begin{aligned}
\tau &:= (\tau\to\tau/\tau) \mid p\to\tau \mid \mathrm{result} \tau \tau \\
e &:= \mathrm{do} x e; e \mid \mathrm{return}\ e \mid (e\mid e) \mid e\ \mathrm{2\&>1} \mid c\ a \mid \ldots \\

\end{aligned}
$$

[Starship: Cross-Shell Prompt](https://starship.rs/ja-jp/)