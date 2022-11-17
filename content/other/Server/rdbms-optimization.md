---
title: "バックエンド最適化"
---


Cache

Copy on Write
[An Overview of Query Optimization in Relational Systems 論文紹介 - Google スライド](https://docs.google.com/presentation/d/1ruGYLRLeagkfv1gQBlmh_di7AviaSx0MJih4oH24AsY/edit#slide=id.p)

O/R Mapper

# ISUCONメモ

https://github.com/FujishigeTemma/isucon9-qualify/blob/master/go/main.go
https://github.com/tohutohu/isucon9/blob/master/webapp/go/main.go

https://www.youtube.com/watch?v=0DhBLswwcRs

https://gist.github.com/941/8c64842b71995a2d448315e2594f62c2
https://gist.github.com/south37/d4a5a8158f49e067237c17d13ecab12a
https://isucon.net/archives/54822761.html

## 事前講習

工事中
:::spoiler
ssh接続
ssh鍵
githubで公開リポジトリに置くと失格
マニュアルを全員読む(情報をまとめることが得意な人がいたら)
初期ベンチ
ベンチの振れ幅、挙動を調べる
1コミット1ベンチ
コミットコメントにログとか載せると良き
キャッシュが必要以上に残るとフェイル
タイムアウトか間違ったキャッシュならタイムアウトを取る

変更するファイル
webapp/go
webapp/sql
各種設定ファイル
nginxやmysqlの設定などはgit内に移して元の場所にシンボリックリンクを貼る



### 開始直後

マニュアルを全員で読む
- 点数に関わる記述が重要情報
- キャッシュの許され方
    - 「N秒キャッシュしてもよい」と書いてある

不要なデーモンを止める
```
cat /proc/cpuinfo
free -h
systemctl list-units --type=service --state=running
```

ssh configの設定
ベンチ1回目
リポジトリ内にシンボリックリンクを置く
git init git remote git add . git commit -m "Initial commit"
お昼食べる
利用するサイトの動作確認
忘れて無ければ全ページのスクショを撮る
ラストのチェックのときにCSSとかをざっと見る
使いそうなファイル群を固めてローカルに持ってくる
tar xvcf webapp
scp user@ip:~/webapp.tar .
更新箇所のチェック
エンドポイントの数
更新があるエンドポイント
mysqlとnginxのログ設定
解析ツールのインストール
ベンチ2回目
秘伝のタレ
ベンチ3回目

```
INDEX idx_category_id_created_at (category_id,created_at),
INDEX idx_created_at (created_at),
INDEX idx_buyer_id (buyer_id),
INDEX mul_idx_seller_id_created_at (seller_id, created_at)
```
https://github.com/reyu0722/piscon2021
- ItemとかUserとかで取れる部分はキャッシュする -> 71500
- Item以外をメモリだけで管理するようにした (他にもいくつか) -> 87840
    - 超大変だった
:::

## 各種解析ツール 推測するな計測せよ

### パフォーマンスモニタリング
CPU占有率の高いプロセスを特定します
htop dstat
dstat -tlamp

### メトリクス
高級なパフォーマンスモニタリングツール
[netdata](https://github.com/netdata/netdata) NewRelic Splunk
https://dev.classmethod.jp/articles/netdata/

### アクセス解析
パスパラメータ/クエリパラメータ別のアクセス数を表示してくれます
主に見るべきはavg 次にcount
kataribe **[alp](https://github.com/tkuchiki/alp)**

### プロファイリング
**pprof** fgprof

### スロークエリ
mysqldumpslow **pt-query-digest**

### SQL全般
**[percona tool kit](https://www.s-style.co.jp/products/percona/toolkit)**
[mysqltuner](https://github.com/major/MySQLTuner-perl)

### Makefile
TODO: 秘伝のMakefileを作る
https://gist.github.com/azonti/dee0547cb561dfdec4a90e093a418bdc
https://github.com/FujishigeTemma/isucon10-final/blob/master/Makefile
https://github.com/tohutohu/isucon9/blob/master/Makefile
https://git.trap.jp/eiya/202008piscon/src/branch/master/go/Makefile

## アプリ

- クエリ最適化
- 変更の少ないデータのキャッシュ化
- やらなくてよい処理を省く
- 強いセキュリティを弱くして高速化(gorilla/sessions->自前実装 net/http->fasthttp bcrypt->SHA256など)
- APIの並列化
- FOR UPDATEアプリケーションで排他制御
- gollira/sessionはセキュアなセッションをcookieだけで実現しているので、暗号化のコストが結構かかる -> ランダムな値とmapを使った実装に差し替え
- jsonのエンコード/デコードをgo-json

### プロファイラの設定

#### pprof https://github.com/google/pprof
標準で入っているプロファイラです。デファクトスタンダードという感じ。
```go
import _ 'net/http/pprof'

func main() {
    go func() {
        http.ListenAndServe(":6060")
    }()

    // <code to profile>
}
```
プロファイリングした画像をdiscordやslackに届けるシェルです。
```shell
go tool pprof -png -output pprof.png http://localhost:6060/debug/pprof/profile?seconds=60
curl -X POST -F img=@pprof.png $(DISCORD_WEBHOOK_URL)
slackcat --channel general pprof.png
```

#### fgprof https://github.com/felixge/fgprof
一部適切な表示とならないことがある
```go
import (
    "net/http"
    _ "net/http/pprof"
    "github.com/felixge/fgprof"
)

func main() {
    http.DefaultServeMux.Handle("/debug/fgprof", fgprof.Handler())
    go func() {
        log.Println(http.ListenAndServe(":6060", nil))
    }()

    // <code to profile>
}
```

`http://localhost:6060/debug/fgprof`

## MySQL チューニング

### MySQL設定

別のサーバーからDBへアクセスできるようにする
`/etc/mysql/mysql.conf.d/mysqld.cnf`
`bind-address = 127.0.0.1`をコメントアウト
`bind-address = 0.0.0.0`

http://dsas.blog.klab.org/archives/50860867.html
- グローバルバッファ mysqld全体でそのバッファが1つだけ確保されるもの
- スレッドバッファ スレッド(コネクション)ごとに確保されるもの
チューニングの際にはグローバル/スレッドの違いを意識するようにしましょう。 なぜなら、スレッドバッファに多くのメモリを割り当てると、コネクションが増えたとたんにアッという間にメモリ不足になってしまうからです。

```
max_connections=1024
query_cache_type=ON
# innoDB全体で一つ生成されるグローバルバッファ(別鯖なら搭載メモリの80%)
innodb_buffer_pool_size = 1GB
# InnoDBの内部データなどを保持する足りないとエラーログが出るからその時増やす
#innodb_additional_mem_pool_size = 30MB ←これあるとダメ
# innoDBの更新ログを保持するメモリ
innodb_log_buffer_size = 16MB
# innodb_log_fileがいっぱいになると、メモリ上のinnodb_buffer_poolの中の更新された部分のデータを、ディスク上のInnoDBのデータファイルに書き出すしくみになっているから
innodb_log_file_size = 128MB
# ORDER BYやGROUP BYのときに使われるメモリ上の領域
innodb_sort_buffer_size = 4MB
read_rnd_buffer_size = 2MB #
key_buffer_size = 256MB
# 1に設定するとトランザクション単位でログを出力するが 2 を指定すると1秒間に1回ログを吐く。0だとログも1秒に1回。TODO違いをみる
innodb_flush_log_at_trx_commit = 0
innodb_flush_log_at_trx_commit = 2
# データファイル、ログファイルの読み書き方式を指定する(実験する価値はある)
innodb_flush_method = O_DIRECT
# 再起動試験対策
innodb_buffer_pool_dump_at_shutdown = ON
innodb_buffer_pool_load_at_startup = ON
```

```
$ mysql -u isucari -p
pass: isucari
$ sudo journalctl -u isucari.golang
sudo rm /var/log/mysql/mysql-slow.log
sudo systemctl restart mysql
sudo rm /var/log/nginx/access.log
sudo nginx -t
sudo systemctl reload nginx
cd ~/isucari/webapp/go
make
cd -
sudo systemctl restart isucari.golang
```

### スロークエリ

`/etc/mysql/mysql.conf.d/mysqld.cnf`
```bash
slow_query_log=1
slow_query_log_file=/var/log/mysql/mysql-slow.log
long_query_time=0 # 全てのクエリを書き込んで解析ツールに渡す
log_queries_not_using_indexes=1 # インデックスを使っていないクエリも出力する
```
`sudo mysqldumpslow -s t -t 10 /path/to/slow.log`
`pt-query-digest /path/to/slow.log`

複数のSQLクエリをIN、JOIN、LIMIT句、外部キーでまとめる(N+1問題など)
不要なカラムやクエリを削除する
転送量を減らす。取得するカラムを減らす、特にでかいもの(`VARCHAR(4096)`など)が入っているのは削るべき
https://qiita.com/ikenji/items/b868877492fee60d85ce

**INDEX**
B+ツリーのインデックスを適切に設定することで検索速度を高める。二分探索ができるものなら速くなる。
更に言えば上記のBツリーインデックスとハッシュインデックスが存在し、ハッシュインデックスの方が速いが、等価比較しかできない。MySQLは自動的にそれらの選択をしている。
これが多すぎるとINSERTでくそおもになるので注意

ここで役立つコマンド
`mysql -uユーザ名 -pパスワード DB名 -e'EXPLAIN ~~;'`

LIKEも最初の方がワイルドカードではなければ使える
- 速くなるもの WHERE, ORDER BY, JOIN句
- 効かないもの LIKE, OR, 演算, 関数処理, IS NULL, 否定形
- NOT NULLを出来る限りつける
- ORをUNION/UNION ALLに変換する
- ORDER BYでDESCとASCの混合->MySQL 8.0では[降順インデックス](https://dev.mysql.com/doc/refman/8.0/ja/descending-indexes.html)で適用できる
- [空間インデックス](https://dev.mysql.com/doc/refman/8.0/ja/creating-spatial-indexes.html) `SRID 0`を付ける https://matsuu.hatenablog.com/entry/2020/09/13/131145

プレフィックスインデックス
非常に長い文字列にインデックスを付ける必要がある場合、値全体ではなく最初の数文字にインデックスを付けることで、記憶域を節約し、パフォーマンスを改善することができます。
- プレフィックスインデックスでは、選択性も低下するため、十分な選択性が得られる長さを持ち、記憶域を節約するくらい短いプレフィックスを選択すること
- 欠点として、ORDER BY 句や GROUP BY 句を使用するクエリにプレフィックスインデックスを使用できない
- 適切なサイズは、下記クエリで選択性を計測し、収束する値を見ることで発見できる。選択性とは、インデックス付けされた値の数と、テーブル内の行の合計数の比率のこと

マルチインデックス
[クエリ文がINDEX作成時のカラム順に基づいていないと、INDEXが使えない](https://qiita.com/rm-rf-slant/items/8023500788352646b6c2)
- 等しい (=)、より大きい (>)、より小さい (<)、BETWEENなどの検索条件のWHERE句で使用されるか、結合に含まれる列を、先頭に配置する
- クエリによるカラム順序の制限がない場合、最も選択的な列をインデックスの先頭に配置する。カラム毎の選択性は下記クエリで計測する
- ちゃんと出来てないとEXPLAIN type=index:フルインデックススキャンと言われる
1種類の値に対し多くのデータが存在するようなカラムを先に置く
重複の多いものを先に置く
それに伴うようにクエリのカラムを先に置く

https://lukesilvia.hatenablog.com/entry/20080315/1205583930
Using filesort
レコードをソートして取り出す方法を決定するには、MySQL はパスを余分に実行しなくてはならないことを示す。 join type に従ってすべてのレコードをスキャンし、WHERE 条件に一致する全てのレコードに、ソートキー + 行ポインタを格納して、ソートは実行される。 その後キーがソートされる。 最後に、ソートされた順にレコードが取り出される。

Using temporary
クエリの解決に MySQL で結果を保持するテンポラリテーブルの作成が必要であることを示す。これは一般に、GROUP BY を実行したカラムセットと異なるカラムセットに対して ORDER BY を実行した場合に発生する。
UNIONとかも...？

https://qiita.com/katsukii/items/3409e3c3c96580d37c2b
https://nishinatoshiharu.com/overview-multicolumn-indexes/

インデックスオンリースキャン
SELECTするカラムが少ないときINDEXにそのカラムを置くとそれを取ってくるだけでよい

### クエリーキャッシュ Qcache
※注意 [MySQL 8.0以降には存在しない](https://yakst.com/ja/posts/4612)
メモリ上にクエリのバイト列とその結果を保存して再度同じ(大文字・小文字を区別する)クエリが来たらDBを探さずそれを返す。更新がかかるとキャッシュがフラッシュされる。
ディスクI/Oの多発の解決
INSERT,UPDATEが少なくSELECTが多いアプリに有効

[MySQL クエリーキャッシュ 【チューニング方法とかも】](https://qiita.com/ryurock/items/9f561e486bfba4221747)

***

SQLで画像を入れるとAXと呼ばれるやつが入るらしい
https://stackoverflow.com/questions/52426874/how-do-i-extract-microsoft-sql-varbinarymax-field-to-image-using-golang

`キーキャッシュのヒット率 = 100 - ( key_reads / key_read_requests × 100 )`
mysqlをmariadbに変える? (あまり必要はない)

```shell
$ curl -LsS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup | sudo bash
$ sudo apt install mariadb-server
$ #このあと/etc/mysql/mysql.conf.d/mysqld.cnfに書いてたやつを/etc/mysql/mariadb.conf.d/50-server.cnfに書く
```

~~proxysqlを利用する？~~ ~~mariadbなら~~そのままquery cache使えばよさそう

http://dsas.blog.klab.org/archives/50860867.html

### 再起試験対策
1台目サーバと2台目サーバの再起動タイミングがずれるとアプリからのDB接続に失敗
`/etc/systemd/system/isuumo.go.service`
```
[Service]
StartLimitBurst=999 # 失敗して再起動するのを何回行うか デフォルトは5
```

再起動試験用DB待ち
```go
package main

import (
	"database/sql"
	"log"
	"time"
)

// main関数内に置く
waitDB(db)
go pollDB(db)

func waitDB(db *sql.DB) {
	for {
		err := db.Ping()
		if err == nil {
			return
		}

		log.Printf("Failed to ping DB: %s", err)
		log.Println("Retrying...")
		time.Sleep(time.Second)
	}
}

func pollDB(db *sql.DB) {
	for {
		err := db.Ping()
		if err != nil {
			log.Printf("Failed to ping DB: %s", err)
		}

		time.Sleep(time.Second)
	}
}
```
***

#### golang
```golang
dbx.SetMaxIdleConns(1024) // デフォルトだと2
dbx.SetConnMaxLifetime(0) // 一応セット
dbx.SetConnMaxIdleTime(0) // 一応セット go1.15以上

// goroutineを生やしすぎてもタイムアウトする https://www.sambaiz.net/article/61/
// Keep-AliveするとTCPコネクションを使い回し、名前解決やコネクション(3 way handshake)を毎回行わなくてよくなる
http.DefaultTransport.(*http.Transport).MaxIdleConns = 0 // 無制限 デフォルトだと100
http.DefaultTransport.(*http.Transport).MaxIdleConnsPerHost = 1024 // 0にすると2になっちゃう
http.DefaultTransport.(*http.Transport).ForceAttemptHTTP2 = true // go1.13以上
http.DefaultClient.Timeout = 5 * time.Second // 問題の切り分け用
```

https://qiita.com/go_sagawa/items/11929cd0883608a6888d

- redis
```
sudo add-apt-repository ppa:chris-lea/redis-server
sudo apt update
sudo apt install redis
```
`sudo nano /etc/redis/redis.conf`
`bind 127.0.0.1 ::1`のコメントアウト
`protected-mode yes`→`protected-mode no`
`requirepass hogehoge`に
```
sudo systemctl unmask redis-server
sudo systemctl enable redis-server
sudo systemctl restart redis-server
```
`sudo echo never > /sys/kernel/mm/transparent_hugepage/enabled`
`sudo nano rc.local`→追記: `echo never > /sys/kernel/mm/transparent_hugepage/enabled`

### JSON
https://github.com/json-iterator/go を試してみる
```golang
var json = jsoniter.Config{
    EscapeHTML:                    false,
    ObjectFieldMustBeSimpleString: true,
}.Froze()
```
https://github.com/francoispqt/gojay
https://github.com/goccy/go-json
https://github.com/minio/simdjson-go
Marshal / Encoder: goccy/go-json > francoispqt/gojay
Unmarshal: francoispqt/gojay > goccy/go-json
Decoder: francoispqt/gojay >> goccy/go-json

- メモリ上にキャッシュ
    - `map`
        - 読み込み書き込みともに多く行う -> `sync.Map`
        - 読み込み多く行う -> `sync.RWMutex` + `map`
        - ロックが気になるならシャーディングをするとよい
            - https://github.com/orcaman/concurrent-map
            - https://github.com/FujishigeTemma/isucon9-qualify/compare/6eaa28f77cb8bac674c0b8cfbf9794d91999d026...49cede08c6fcf59afa29857559d146abb1e96165
            - 15～20個ずつになるくらいがよさそう？
    - sessionメモリに持つ
        - `gorilla/sessions`はコピーのために`encoding/gob`が無駄に呼び出される
- singleflight
    - `x/sync/singleflight`
    - `sync.Map` + `sync.Cond`
        - https://github.com/FujishigeTemma/isucon9-qualify/compare/2fb8ff382ce2f7b083ae9f343e5ae5d543bc5e65...6d3f1ab77bd377be00fdf6d5735ffe14b8f5afd6
    - `go-chi/httpcoala`
        - https://github.com/FujishigeTemma/isucon9-qualify/commit/495ae7ba4732b3bcb9c4a6795bea86dfac060c6c
- メモリ効率化
    - `sync.Pool`
        - https://github.com/FujishigeTemma/isucon9-qualify/commit/4e7a9be6cbee699dc11db96c2134836dfd207def
- 挿入後の結果をとらずに返す
    - timeはミリ秒を四捨五入すること`.Round`
        - デフォルトが秒までだけど、`DATETIME`のあとに数字がある場合は異なるので要確認
        - https://dev.mysql.com/doc/refman/5.6/ja/fractional-seconds.html
- session
    - 自前の`interface`の変換なしの実装
        - https://github.com/FujishigeTemma/isucon9-qualify/commit/64d095a6400bbde6df4ed62d6ad9bd12dbc8a964
- `pat.Param`
    - 自前の`interface`の変換なしの実装
        - https://github.com/FujishigeTemma/isucon9-qualify/compare/b87747c27f305927b40b86285b1642cbe52e1c55...68c236f9782f041bca64f185ae0a3fbec9122ee2
- misc
    - キーが100個程度までならmapよりarrayを線形探索したほうがよいらしい
    - sliceもmapもcapacityを指定する
    - `i, item := range items`をすると`item`にコピーが走るので`items[i]`を使う
    - 画像を返してる箇所はキャッシュのヘッダーを設定する
    - GOGC `400`～`1200`とか
        - 有効にしたときは**毎回再起動すること**

- [MySQL のパーティショニングで速くなる？ならない？問題、あらためて実験してみた](https://qiita.com/hmatsu47/items/354f979cde6ad91bcc6b)
- カバリングインデックス

http://akouryy.hatenablog.jp/entry/2020/09/13/130415

- https://dev.mysql.com/doc/refman/5.6/ja/optimizing-innodb-bulk-data-loading.html

#### MySQL8
- NOWAIT, SKIP LOCKED: https://qiita.com/hmatsu47/items/7675b026e65762d2445f
- HASH JOIN: https://qiita.com/hmatsu47/items/e473a3e566b910d61f5b
- Multi-valued index(jsonカラム用): https://qiita.com/hmatsu47/items/3e49a473bc36aeefc706
- GROUP BYをLATERALにする？: https://qiita.com/hmatsu47/items/040d65d118d0ecec6381


パーティショニングとは

## HTTP チューニング

https://qiita.com/yumin/items/5de33b068ead564ebcbf

[martini](https://github.com/go-martini/martini)
[gin](https://github.com/gin-gonic/gin)

fasthttp
https://medium.com/eureka-engineering/net-http%E3%82%88%E3%82%8A10%E5%80%8D%E9%80%9F%E3%81%84valyala-fasthttp%E3%81%8C%E9%9D%A2%E7%99%BD%E3%81%9D%E3%81%86%E3%81%AA%E3%81%AE%E3%81%A7%E8%AA%BF%E6%9F%BB%E3%81%97%E3%81%A6%E3%81%BF%E3%81%9F%E4%BB%B6-a608fe197f1d


### メモ
https://github.com/cs3238-tsuzu/sqlx-selector

http://nginx.org/en/docs/http/ngx_http_rewrite_module.html#if
https://stackoverflow.com/questions/8591600/nginx-proxy-pass-based-on-whether-request-method-is-post-put-or-delete

得点につながるエンドポイントの確認
大量アクセスかつ同じものが使える→singleflight

lockfree map