
## Prometheus


## Grafana
ビューワー
サーバー上でgrafanaを立ててCaddyで外から見れるようにする
docker-composeに入れればいい
```
{{ grafana.host }} {
  reverse_proxy localhost:3000
}
```

## ログ可視化
ログ保存、集計しメトリクスにする
- #Loki

## ログ転送
- Promtail
- Fluentd
- Fluent Bit

## Alertmanager
Slackにアラートを飛ばす
docker-compose に入れればいい

## node_exporter

## Continuous profiling
phlare