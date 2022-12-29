## カーネルパラメータ チューニング

[【Linux】カーネルパラメータのパフォーマンスチューニングについて](https://ac-as.net/kernel-parameter-performance-tuning/)

### ファイルディスクリプタの上限

`ulimit -l 10000`

sshだとulimitできない
https://yohei-a.hatenablog.jp/entry/20090310/1236706236

http://ramblog.blog129.fc2.com/blog-category-4.html

```
# /etc/systemd/system/*.service
[Service]
LimitNOFILE=65535
```
### カーネルパラメータ

上ほど優先順位高い(同じ名前のfileをoverrideする)
`/etc/sysctl.d/*.conf`
`/run/sysctl.d/*.conf`
`/usr/lib/sysctl.d/*.conf`
すべての設定ファイルは、どのディレクトリにあるかに関わらず、ファイル名の並び順で辞書順にソートされます。複数のファイルが同じオプションを指定している場合は、辞書的に最新の名前を持つファイルのエントリが優先されます。ファイルの順序を簡単にするために、すべてのファイル名の前に 2 桁の数字とダッシュを付けることをお勧めします。

```
# /etc/sysctl.d/100-isucon.conf
# maxconnection を増やす
net.core.somaxconn = 32768                  # 32768 (2^15) くらいまで大きくしても良いかも。
net.ipv4.ip_local_port_range = 10000 60999  # port の範囲を広げる

# tcp connection の再利用を有効化
net.ipv4.tcp_tw_reuse = 1

# tcp connection が FIN-WAIT2 から TIME_WAIT に状態が変化するまでの時間
net.ipv4.tcp_fin_timeout = 10               # デフォルト 60。CPU 負荷を減らせるが、短すぎると危ういかも？

# TIME_WAIT状態にある tcp connection 数の上限
net.ipv4.tcp_max_tw_buckets = 2000000       # デフォルトは 32768(=2^15) くらい

# パケット受信時にキューにつなぐことのできるパケットの最大数
net.core.netdev_max_backlog = 8192          # デフォルトは 1000 くらい

# 新規コネクション開始時のSYNパケットを受信した際の処理待ちキューの上限値
net.ipv4.tcp_max_syn_backlog = 1024         # デフォルトは 128 くらい

# window size scalingの有効化(ネットワークの帯域幅とメモリ使用量のトレードオフ)
net.ipv4.tcp_window_scaling = 1             # デフォルトで1になっているはず
# すべての種類のsocketに基本設定されているbufferのsize デフォルトは 212992(=13*2^14) くらい
net.core.rmem_default = 253952
net.core.wmem_default = 253952
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216
# TCP socket buffer sizeの変更 デフォルトは 212992(=13*2^14) くらい
net.ipv4.tcp_rmem = 253952 253952 16777216  # min / default / max
net.ipv4.tcp_wmem = 253952 253952 16777216  # min / default / max
# TCP用に使用できる合計メモリサイズを指定
net.ipv4.tcp_mem = 185688 247584 371376     # min / pressure / max; pressureの値を超えるとTCP memory pressureの状態になり、以降ソケットは指定されたmin値のメモリバッファのサイズを持つようになる

# カーネルレベルでのファイルディスクリプタ上限数変更
# プロセス単位のチューニングをやったけど、こっちもやっておく
fs.file-max=65535

# 3-way-handshakeの簡略化
# 相手側のサーバーがONにしていないとデータが2回送られてオーバーヘッドになるので一回やってみてスコアが上がらなかったら切る
net.ipv4.tcp_fastopen = 3

# 輻輳制御アルゴリズム TCP BBR の有効化
# `uname -r`が4.9以上で`sysctl net.ipv4.tcp_available_congestion_control`にbbrが含まれている場合　
# net.ipv4.tcp_congestion_control = bbr # 輻輳制御アルゴリズムをbbrに
# net.core.default_qdisc = fq # キューイングアルゴリズムをfqに
```

https://html5experts.jp/jxck/3529/
https://ac-as.net/kernel-parameter-performance-tuning/
φ(.. )ﾒﾓｼﾃｵｺｳ /proc/sys/net/ipv4/tcp_fastopenに設定する内容とか実装
https://kernhack.hatenablog.com/entry/2013/05/25/115634

