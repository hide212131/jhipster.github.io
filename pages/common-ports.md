---
layout: default
title: 共通的なポート番号
permalink: /common-ports/
sitemap:
    priority: 0.7
    lastmod: 2021-03-08T12:00:00-00:00
---

# <i class="fa fa-plug"></i> 共通的なポート番号

JHipsterは多くのツールとサービスを構成し、それぞれが1つまたは複数のポートを使用する可能性があります。ここでは、各ポートの機能を理解し、ポートの競合が発生した場合に役立つドキュメントを示します。

JHipster[ポリシー 1]({{ site.url }}/policies/)に従って、問題が発生しない限り（ここで説明する必要があります）、各テクノロジーの標準ポートが使用されることに注意してください。

ここではポートを順に示していますが、最も一般的な質問はポート`8080`、`9000`、`9060`に関するものです。

<table class="table table-striped table-responsive">
  <tr>
    <th>ポート</th>
    <th>説明</th>
  </tr>
  <tr>
    <td>2181</td>
    <td>Zookeeper（Kafkaで使用）</td>
  </tr>
  <tr>
    <td>3000</td>
    <td>Grafana</td>
  </tr>
  <tr>
    <td>3306</td>
    <td>MySQLとMariaDB</td>
  </tr>
  <tr>
    <td>5000</td>
    <td>Logstash</td>
  </tr>
  <tr>
    <td>5432</td>
    <td>PostgreSQL</td>
  </tr>
  <tr>
    <td>5701</td>
    <td>Hazelcast</td>
  </tr>
  <tr>
    <td>6650</td>
    <td>Pulsar - Service port</td>
  </tr>
  <tr>
    <td>7742</td>
    <td>Swagger Editor</td>
  </tr>
  <tr>
    <td>8080</td>
    <td>JHipsterアプリケーションバックエンド開発ポート（Spring Bootサーバ）</td>
  </tr>
  <tr>
    <td>8081</td>
    <td>JHipsterマイクロサービスのデフォルトポート</td>
  </tr>
  <tr>
    <td>8091</td>
    <td>Couchbase - Web管理ポート</td>
  </tr>
  <tr>
    <td>8092</td>
    <td>Couchbase - APIポート</td>
  </tr>
  <tr>
    <td>8093</td>
    <td>Couchbase - REST/HTTPトラフィックのクエリサービスで使用</td>
  </tr>
  <tr>
    <td>8180</td>
    <td>Hazelcast管理センター</td>
  </tr>
  <tr>
    <td>8301</td>
    <td>Consul - serflan-tcpとserflan-udp</td>
  </tr>
  <tr>
    <td>8302</td>
    <td>Consul - serfwan-tcpとserfwan-udp</td>
  </tr>
  <tr>
    <td>8300</td>
    <td>Consul - サーバ</td>
  </tr>
  <tr>
    <td>8400</td>
    <td>Consul - RPC</td>
  </tr>
  <tr>
    <td>8500</td>
    <td>Consul - Web UIを持つHTTPポート</td>
  </tr>
  <tr>
    <td>8600</td>
    <td>Consul - DNS</td>
  </tr>
  <tr>
    <td>8761</td>
    <td>JHipsterレジストリ(Netflix Eureka)</td>
  </tr>
  <tr>
    <td>9000</td>
    <td>BrowserSyncを使用したJHipsterフロントエンド開発ポート</td>
  </tr>
  <tr>
    <td>9001</td>
    <td>SonarQube</td>
  </tr>
  <tr>
    <td>9042</td>
    <td>Cassandra - CQL</td>
  </tr>
  <tr>
    <td>9060</td>
    <td>Webpackホットリロードを使用したJHipsterフロントエンド開発ポート</td>
  </tr>
  <tr>
    <td>9090</td>
    <td>Prometheus</td>
  </tr>
  <tr>
    <td>9092</td>
    <td>Kafka</td>
  </tr>
  <tr>
    <td>9093</td>
    <td>Prometheusアラートマネージャ</td>
  </tr>
  <tr>
    <td>9160</td>
    <td>Cassandra - Thrift</td>
  </tr>
  <tr>
    <td>9200</td>
    <td>Elasticsearch - HTTP接続（REST API）</td>
  </tr>
  <tr>
    <td>9300</td>
    <td>Elasticsearch - トランスポート接続（ネイティブAPI）</td>
  </tr>
  <tr>
    <td>9411</td>
    <td>Zipkin</td>
  </tr>
  <tr>
    <td>11210</td>
    <td>Couchbase - 内部/外部バケットポート</td>
  </tr>
  <tr>
    <td>18080</td>
    <td>モノリス内で実行されるH2（組み込みデータベース）。デフォルトポートは通常9092ですが、Kafkaとの競合が発生するため、"1" + "Spring Bootポート"に固定されています。</td>
  </tr>
  <tr>
    <td>18081</td>
    <td>マイクロサービス内で実行されるH2（組み込みデータベース）。詳細については、上記の行を参照してください。</td>
  </tr>
  <tr>
    <td>27017</td>
    <td>MongoDB</td>
  </tr>
</table>
