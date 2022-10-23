---
title: "Nginx (リバースプロキシ)チューニング"
---


### サーバー分割
**DB分ける**
**リクエストのロードバランス**
ロードバランサー
セッションパーシステンス

worker_processesにそれぞれ均等にworker_connectionsが分配される訳ではないので余裕をもって設定すべき
https://nrok81.hatenablog.com/entry/2014/11/12/191240

```conf
worker_processes  auto;  # コア数と同じ数まで増やすと良いかも

# nginx worker の設定
worker_rlimit_nofile  4096;
events {
    worker_connections  1024;  # 128より大きくするなら、 5_os にしたがって max connection 数を増やす必要あり（デフォルトで`net.core.somaxconn` が 128 くらいで制限かけられてるので）。さらに大きくするなら worker_rlimit_nofile も大きくする（file descriptor数の制限を緩める)
    # multi_accept on;         # error が出るリスクあり。defaultはoff。
    # accept_mutex_delay 100ms;
}

http {
    log_format main '$remote_addr - $remote_user [$time_local] "$request" $status $body_bytes_sent "$http_referer" "$http_user_agent" $request_time';   # kataribe 用の log format
    access_log  /var/log/nginx/access.log  main;   # これはしばらく on にして、最後に off にすると良さそう。
    # access_log  off;

    # 基本設定
    sendfile    on;
    tcp_nopush  on;
    tcp_nodelay on;
    types_hash_max_size 2048;
    server_tokens    off;
    open_file_cache max=100 inactive=65s; # file descriptor のキャッシュ。入れた方が良い。 20s->65s

    # proxy buffer の設定。白金動物園が設定してた。
    proxy_buffers 100 32k;
    proxy_buffer_size 8k;

    # mime.type の設定
    include       /etc/nginx/mime.types;

    # Keepalive 設定
    # ベンチマークとの相性次第ではkeepalive off;にしたほうがいい
    # keepalive off;
    # ベンチは1分しか回らない
    keepalive_timeout 65;
    keepalive_requests 500;

    # Proxy cache 設定。使いどころがあれば。1mでkey8,000個。1gまでcache。
    proxy_cache_path /var/cache/nginx/cache levels=1:2 keys_zone=zone1:1m max_size=1g inactive=1h;
    proxy_temp_path  /var/cache/nginx/tmp;
    # オリジンから来るCache-Controlを無視する必要があるなら。。。
    #proxy_ignore_headers Cache-Control;


    # unix domain socket 設定1
    upstream app {
        server unix:/run/unicorn.sock;  # systemd を使ってると `/tmp` 以下が使えない。appのディレクトリに`tmp`ディレクトリ作って配置する方がpermissionでハマらずに済んで良いかも。
    }

    # 複数serverへ proxy
    upstream app {
        server 192.100.0.1:5000 weight=2;  # weight をつけるとproxyする量を変更可能。defaultは1。多いほどたくさんrequestを振り分ける。
        server 192.100.0.2:5000;
        server 192.100.0.3:5000;
        # keepalive 60;  # app server への connection を keepalive する。app が対応できるならした方が良い。
    }

    server {
        # reverse proxy の 設定
        location / {
            proxy_pass http://localhost:3000;
            # proxy_http_version 1.1;          # app server との connection を keepalive するなら追加
            # proxy_set_header Connection "";  # app server との connection を keepalive するなら追加
        }

        # Unix domain socket の設定2。設定1と組み合わせて。
        location / {
            proxy_pass http://app;
        }

        # 簡易静的ファイルをNginx配信
        location / {
            root /home/user/app/public/;
            try_files $uri $uri/ @app;
        }
        location @app {
            proxy_pass http://app;
        }

        # For Server Sent Event
        location /api/stream/rooms {
            # "magic trio" making EventSource working through Nginx
            proxy_http_version 1.1;
            proxy_set_header Connection '';
            chunked_transfer_encoding off;
            # These are not an official way
            # proxy_buffering off;
            # proxy_cache off;
            proxy_pass http://localhost:8080;
        }

        # For websocket
        location /wsapp/ {
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            proxy_pass http://wsbackend;
        }

        # Proxy cache
        location /cached/ {
            proxy_cache zone1;
            # proxy_set_header X-Real-IP $remote_addr;
            # proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            # proxy_set_header Host $http_host;
            proxy_pass http://localhost:9292/;
            # デフォルトでは 200, 301, 302 だけキャッシュされる。proxy_cache_valid で増やせる。
            # proxy_cache_valid 200 301 302 3s;
            # cookie を key に含めることもできる。デフォルトは $scheme$proxy_host$request_uri;
            # proxy_cache_key $proxy_host$request_uri$cookie_jessionid;
            # レスポンスヘッダにキャッシュヒットしたかどうかを含める
            add_header X-Nginx-Cache $upstream_cache_status;
        }

        # static file の配信用の root
        root /home/isucon/webapp/public/;

        location ~ .*\.(htm|html|css|js|jpg|png|gif|ico) {
            expires 24h;
            add_header Cache-Control public;

            open_file_cache max=100;  # file descriptor などを cache

            gzip on;  # cpu 使うのでメリット・デメリット見極める必要あり。gzip_static 使えるなら事前にgzip圧縮した上でそちらを使う。
            gzip_types text/html text/css application/javascript application/json font/woff font/ttf image/gif image/png image/jpeg image/svg+xml image/x-icon application/octet-stream;
            gzip_disable "msie6";
            gzip_static on;  # nginx configure時に --with-http_gzip_static_module 必要
        }
    }
}
```

```
user www-data;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;
worker_rlimit_nofile 100000;

error_log  /var/log/nginx/error.log error;

http {
    default_type  application/octet-stream;

    client_max_body_size 10m;

    # TLS configuration
    ssl_protocols TLSv1.2;
    ssl_prefer_server_ciphers on;
    ssl_ciphers 'ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384';

	server {
	    listen 443 ssl http2;
	    server_name isucon9.catatsuy.org;

	    ssl_certificate /etc/nginx/ssl/fullchain.pem;
	    ssl_certificate_key /etc/nginx/ssl/privkey.pem;

		root /home/isucon/isucari/webapp/public;
		location /static/ {
			add_header Cache-Control "public max-age=86400";
		}
		location /upload/ {
			add_header Cache-Control "public max-age=86400";
		}

	    location / {
		proxy_pass http://app;
		proxy_set_header Host $host;
	    }
	    location /login {
		proxy_pass http://login_app;
		proxy_set_header Host $host;
	    }
	}
}
```

### 初期設定
https://wiki.trap.jp/user/to-hutohu/memo/ISUCON%E3%83%81%E3%83%BC%E3%83%88%E3%82%B7%E3%83%BC%E3%83%88
#### OS
設定したら`sudo sysctl -p {{filename}}`で反映する
実行中のプロセスには反映されないからプロセスの再起動が必要

備考: `net.ipv4.ip_, net.ipv4.ip_local_portrange, net.ipv4.tcp, net.ipv4.icmp_*`はipv6にも適用される

#### nginx
`$ sudo cp *.conf *.conf.bk`
`/etc/nginx/nginx.conf`
`/etc/nginx/sites-enabled/*.conf` (ここでは`http`ブロック内しか変更できない)
`/etc/nginx/sites-available/*.conf`に書いて↑ではエイリアスを貼るのが正解
https://wiki.trap.jp/user/to-hutohu/memo/ISUCON%E3%83%81%E3%83%BC%E3%83%88%E3%82%B7%E3%83%BC%E3%83%88#head10

nginxとアプリケーションの間をunix domain socket
`Unix domain socket`は後回し
https://qiita.com/ihsiek/items/11106ce7a13e09b61547#web%E3%82%B5%E3%83%BC%E3%83%90
`index.html`を配信してるところをnginxで返すようにする
```
    # index.htmlの配信
    location ~ ^/(?:login|register|timeline|categories|sell|items|buy/complete|transactions|users)(?!.*.(?:json|png)) {
        try_files $uri /index.html;
    }
```

```
server {
    listen       80 default_server;
    server_name  _;

    location / {
         root /home/isucon/torb/webapp/static/;
         try_files $uri $uri/ @app;
    }

    location @app {
        proxy_set_header Host $host;
        proxy_pass   http://127.0.0.1:8080;
    }
}
```
```
    proxy_buffer_size 128k;
    proxy_buffers 32 128k;
    proxy_busy_buffers_size 128k;
```

h2cの有効化(意味なさそうなのでhttpsを使ってhttp2を使うべき)
```
server {
    listen 80 http2;
```

#####
設定
```
worker_processes auto;
worker_rlimit_nofile 4096;
events {
    worker_connections 1024; # net.core.somaxconnとworker_rlimit_nofileも大きくする
    #multi_accept on; # error が出るリスクあり。defaultはoff
    #accept_mutex_delay 100ms;
}
```

nginx の worker_connections は worker 当たりの同時接続数だと思ってたけどどうも違うっぽい
https://nrok81.hatenablog.com/entry/2014/11/12/191240

##### ログ設定 httpディレクティブの中
```
log_format main '$remote_addr - $remote_user [$time_local] "$request" $status $body_bytes_sent "$http_referer" "$http_user_agent" $request_time';
access_log /tmp/access.log main;
#access_log off;
```

##### 基本設定 httpディレクティブの中
```
sendfile on;
tcp_nopush on;
tcp_nodelay on;
types_hash_max_size 2048;
server_tokens off;
open_file_cache max=100 inactive=20s; # file descriptorのキャッシュ

# proxy bufferの設定
proxy_buffers 100 32k;
proxy_buffer_size 8k;

# keepaliveの設定
#keepalive off; # ベンチマークとの相性次第ではoffにしたほうがいい
keepalive_timeout 120;
keepalive_requests 1048576;

# proxy cacheの設定
proxy_cache_path /var/cache/nginx/cache levels=1:2 keys_zone=zone1:1m max_size=1g inactive=1h;
proxy_temp_path  /var/cache/nginx/tmp;
#proxy_ignore_headers Cache-Control; # upstreamから来るCache-Controlを無視する必要があるなら
```

##### upstream設定 httpディレクティブの中
```
upstream app {
    server 127.0.0.1:8000;

    keepalive 256;
}
```

##### reverse proxy設定 serverディレクティブの中
```
location /initialize {
    proxy_http_version 1.1;
    proxy_set_header Connection "";

    proxy_read_timeout 120;

    #proxy_cache zone1;

    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-Forwarded-Host $host;
    proxy_set_header X-Forwarded-Server $host;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

    proxy_pass http://app;
}
```

##### 静的配信設定 serverディレクティブの中
```
# static file の配信用の root
root /home/isucon/webapp/public/;

location ~ .*\.(htm|html|css|js|eot|svg|ttf|woff|woff2|gif|jpg|png|ico) {
    expires 24h;

    gzip off;
    #gzip on; # CPUを使うのでメリット・デメリット見極める必要あり。gzip_staticが使えるなら事前にgzip圧縮した上でそちらを使う
    #gzip_types *;
    #gzip_disable "msie6";
    #gzip_static on;
}
```

##### gzip
- できればgzip_staticを使う
    - 圧縮コマンド `find ./* | xargs -I {}  sh -c 'gzip -9 -v -N -c {} > {}.gz'`
        - 元のファイルを残して圧縮する
***
