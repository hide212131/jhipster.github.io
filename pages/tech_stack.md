---
layout: default
title: 技術スタック
permalink: /tech-stack/
redirect_from:
  - /tech_stack.html
sitemap:
    priority: 0.8
    lastmod: 2021-03-08T12:00:00-00:00
---

# <i class="fa fa-stack-overflow"></i> 技術スタック

## クライアント側の技術スタック

Single Web Page Applicationです。

* [Angular](https://angular.io/)または[React](https://reactjs.org/)または[Vue](https://vuejs.org/)
* [Twitter Bootstrap](http://getbootstrap.com/)によるレスポンシブWebデザイン
* [HTML5 ボイラープレート](http://html5boilerplate.com/)
* モダンなブラウザ（Chrome、Firefox、Microsoft Edge...）に対応
* 完全な国際化対応
* CSSデザインのための[Sass](https://www.npmjs.com/package/node-sass) サポート（オプション）
* Spring WebsocketによるWebSocketのサポート（オプション）

これらを以下の素晴らしい開発ワークフローで行えます。

* [NPM](https://www.npmjs.com/get-npm)による新しいJavaScriptライブラリのインストール
* [Webpack](https://webpack.js.org/)によるビルド、最適化、ライブリロード
* [Jest](https://facebook.github.io/jest/)、[Protractor](http://www.protractortest.org)を用いたテスト

Single Web page Applicationがニーズと合わない場合はどうすればいいのでしょうか。

* サーバーサイドでWebページを生成するテンプレートエンジン[Thymeleaf](http://www.thymeleaf.org/)をサポート

## サーバー側の技術スタック

完全な[Springアプリケーション](http://spring.io/)です。

<<<<<<< HEAD
* アプリケーションの構成は、[Spring Boot](http://projects.spring.io/spring-boot/)を使用
* アプリケーションのビルド、テスト、実行のための [Maven](http://maven.apache.org/) または [Gradle](http://www.gradle.org/) の構成
* [「開発」と「プロダクション」のプロファイル]({{ site.url }}/profiles/)（Maven用とGradle用の両方）
* [Spring Security](http://docs.spring.io/spring-security/site/index.html)
* [Spring MVC REST](http://spring.io/guides/gs/rest-service/) + [Jackson](https://github.com/FasterXML/jackson)
* Spring WebsocketによるWebSocketのサポート（オプション）
* [Spring Data JPA](http://projects.spring.io/spring-data-jpa/) + Beanバリデーション
* [Liquibase](http://www.liquibase.org/)によるデータベースの更新
* データベースに検索機能を持たせたい場合は、[Elasticsearch](https://github.com/elastic/elasticsearch)をサポート
* JPAの代わりにドキュメント指向のNoSQLデータベースを使いたい場合は、[MongoDB](http://www.mongodb.org)と[Couchbase](https://www.couchbase.com)をサポート
* JPAの代わりにカラム指向のNoSQLデータベースを使用したい場合は、[Cassandra](http://cassandra.apache.org/)をサポート
* Pub/Subのメッセージングシステムを使用する場合は、[Kafka](http://kafka.apache.org/)をサポート
=======
*   [Spring Boot](http://projects.spring.io/spring-boot/) for application configuration
*   [Maven](http://maven.apache.org/) or [Gradle](http://www.gradle.org/) configuration for building, testing and running the application
*   ["development" and "production" profiles]({{ site.url }}/profiles/) (both for Maven and Gradle)
*   [Spring Security](http://docs.spring.io/spring-security/site/index.html)
*   [Spring MVC REST](http://spring.io/guides/gs/rest-service/) + [Jackson](https://github.com/FasterXML/jackson)
*   Optional WebSocket support with Spring Websocket
*   [Spring Data JPA](http://projects.spring.io/spring-data-jpa/) + Bean Validation
*   Database updates with [Liquibase](http://www.liquibase.org/)
*   [Elasticsearch](https://github.com/elastic/elasticsearch) support if you want to have search capabilities on top of your database
*   [MongoDB](http://www.mongodb.org) and [Couchbase](https://www.couchbase.com) support if you'd rather use a document-oriented NoSQL database instead of JPA
*   [Cassandra](http://cassandra.apache.org/) support if you'd rather use a column-oriented NoSQL database instead of JPA
*   [Kafka](http://kafka.apache.org/) and [Pulsar](http://pulsar.apache.org/) support if you want to use a publish-subscribe messaging system
>>>>>>> upstream/main

## マイクロサービスのための技術スタック

マイクロサービスはオプションで、完全にサポートされています。

* [Spring Cloud Gateway](https://github.com/spring-cloud/spring-cloud-gateway)を利用したHTTPルーティング
* [Netflix Eureka](https://github.com/Netflix/eureka)、[HashiCorp Consul](https://www.consul.io/)を利用したサービスディスカバリ

## プロダクションへ進む準備

* [Metrics](http://metrics.dropwizard.io/)と[ELKスタック](https://www.elastic.co/products)によるモニタリング
* [ehcache](http://ehcache.org/)（ローカルキャッシュ）、 [Caffeine](https://github.com/ben-manes/caffeine)（ローカルキャッシュ）、 [Hazelcast](http://www.hazelcast.com/), [Infinispan](http://infinispan.org/), [Memcached](https://memcached.org/) または [Redis](https://redis.io/) によるキャッシング
* 静的リソースの最適化（gzipフィルタ、HTTPキャッシュヘッダ）
* [Logback](http://logback.qos.ch/)による実行時にも設定可能なログ管理
* [HikariCP](https://github.com/brettwooldridge/HikariCP)によるコネクションプーリングで最適なパフォーマンスを実現
* 標準的なWARファイル、または実行可能なJARファイルをビルド
* DockerおよびDocker Composeの完全サポート
* AWS、Cloud Foundry、GCP、Heroku、Kubernetes、OpenShift、Azure、Dockerなど、すべての主要なクラウドプロバイダーをサポート
