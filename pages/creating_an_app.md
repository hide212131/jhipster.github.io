---
layout: default
title: アプリケーションの構築
permalink: /creating-an-app/
redirect_from:
  - /creating_an_app.html
sitemap:
    priority: 0.7
    lastmod: 2018-03-18T18:20:00-00:00
---

# <i class="fa fa-rocket"></i> アプリケーションの構築

_**新しいJHipsterアプリケーションの作成については、[ビデオチュートリアル]({{ site.url }}/video-tutorial/)を参照してください。**_

1. [クイックスタート](#1)
2. [アプリケーション生成時の質問](#2)
3. [Blueprintを使う](#5)
4. [コマンドラインオプション](#3)
5. [ヒント](#4)

<h2 id="1">クイックスタート</h2>

最初に、アプリケーションを作成する空のディレクトリを作成します。

`mkdir myapplication`

ディレクトリに移動します。

`cd myapplication/`

アプリケーションを生成するには、次のように入力します。

`jhipster`

ジェネレータからの質問に答えて、ニーズに合ったアプリケーションを作成します。これらのオプションについては、[次のセクション](#2)で説明します。

アプリケーションが生成されると、Maven(Linux/Mac OS/Windows PowerShellの場合は`./mvnw`、Windows Cmdの場合は`mvnw`)またはGradle(Linux/Mac OS/Windows PowerShellの場合は`./gradlew`、Windows Cmdの場合は`gradlew`)を使用して起動できます。

**注意** Mavenを使用していて、`./mvnw`コマンドを最初に実行した後フロントエンドファイルを変更した場合は、`./mvnw -Pwebapp`を実行して最新のフロントエンドバージョンを表示する必要があります（Gradleはフロントエンドの変更を自動的に検出し、必要に応じてフロントエンドを再コンパイルします）。

アプリケーションは[http://localhost:8080](http://localhost:8080)にあります。

**重要** JavaScript/TypeScriptコードの「ライブリロード」を行いたい場合は、`npm start`または`yarn start`を実行する必要があります。詳細については、[開発でのJHipsterの使用]({{ site.url }}/development/)ページにアクセスしてください。

「ライブリロード」を使用している場合は、`./mvnw -P-webapp`または`./gradlew -x webapp`を使用してクライアント側のタスクを除外することで、サーバの起動を高速化できます。特にGradleの速度が向上します。

<h2 id="2">アプリケーション生成時の質問</h2>

_いくつかの質問は、以前に行った選択によって変わります。たとえば、SQLデータベースを選択しなかった場合は、Hibernateキャッシュを構成する必要はありません。_

### どの種類のアプリケーションを作成したいですか？（Which _type_ of application would you like to create?）

アプリケーションのタイプは、マイクロサービスアーキテクチャを使用するかどうかによって異なります。マイクロサービスの詳細な説明は[ここにあります]({{ site.url }}/microservices-architecture/)。不明な場合は、デフォルトの"Monolithic application"を使用してください。

次のいずれかを使用できます。

*   Monolithic application：これは古典的で、フリーサイズのアプリケーションです。使用と開発が容易であり、推奨されるデフォルトです。
*   Microservice application：マイクロサービスアーキテクチャでは、これはサービスの1つです。
*   Microservice gateway：マイクロサービスアーキテクチャでは、これはリクエストをルーティングして保護するエッジサーバです。

### アプリケーションのベース名は何ですか?（What is the base name of your application?）

アプリケーションの名前です。

### デフォルトのJavaパッケージ名は何ですか?（What is your default Java package name?）

Javaアプリケーションはこれをルート・パッケージとして使用します。この値はYeomanによって保存されるため、次にジェネレータを実行するときには、最後の値がデフォルトになります。
この値は、新しい値を指定することで上書きできます。

### JHipsterレジストリを使用して、アプリケーションの構成、監視、拡張を行いますか?（Do you want to use the JHipster Registry to configure, monitor and scale your application?）

[JHipster Registry]({{ site.url }}/jhipster-registry/)は、実行時にアプリケーションを管理するためのオープンソースツールです。

マイクロサービスアーキテクチャを使用する場合に必要です（これが、この質問がモノリスを生成するときにのみ尋ねられる理由です）。

### どの _種類_ の認証を使用しますか?（Which _type_ of authentication would you like to use?）

この質問に対する回答は、以前の回答によって異なります。たとえば、前述の[JHipster Registry]({{ site.url }}/JHipster-registry/)を選択した場合は、JWT認証のみを使用できます。

可能なすべてのオプションを以下に示します。

* JWT認証：[JSON Web Token(JWT)](https://jwt.io/)を使用します。これはデフォルトの選択肢であり、ほとんどの人が使用しています。
* OAuth 2.0/OIDC認証：アプリケーションの外部で認証を処理します。[Keycloak](https://www.keycloak.org/)または[Okta](https://developer.okta.com)などのOpenID Connectサーバを使用します。これはJWTよりも安全ですが、OpenID Connectサーバを設定する必要があるため、少し複雑です。デフォルトでは、JHipsterはOpenID Connectサーバからのユーザデータを同期するため、データベースが必要になることに注意してください。
* HTTPセッション認証：古典的なセッションベースの認証メカニズムで、[Spring Security](http://docs.spring.io/spring-security/site/index.html)で通常行われるものです。

詳細については、[アプリケーションのセキュリティ保護]({{ site.url }}/security/)ページを参照してください。

### どの _種類_ のデータベースを使用しますか？（Which _type_ of database would you like to use?）

次のいずれかを選択できます。

- SQLデータベース（H2, MySQL, MariaDB, PostgreSQL, MSSQL, Oracle）。Spring Data JPAでアクセスします。
- [MongoDB]({{ site.url }}/using-mongodb/)
- [Cassandra]({{ site.url }}/using-cassandra/)
- [Couchbase]({{ site.url }}/using-couchbase/)
- [Neo4j]({{ site.url }}/using-neo4j/)
- データベースなし（JWT認証で[マイクロサービス・アプリケーション]({{ site.url }}/microservices-architecture/)を使用する場合にのみ使用可能）

### どの _プロダクション_ データベースを使用しますか?（Which _production_ database would you like to use?）

これは、「プロダクション」のプロファイルで使用するデータベースです。構成するには、`src/main/resources/config/application-prod.yml`ファイルを変更してください。

Oracleを使用する場合は、[Oracleデータベースの使用]({{ site.url }}/using-oracle/)時の現在の制限に注意する必要があります。

### どの _開発_ データベースを使用しますか?（Which _development_ database would you like to use?）

これは、「開発」プロファイルで使用するデータベースです。次のいずれかを使用できます。

*   インメモリで実行するH2：これはJHipsterを使用する最も簡単な方法ですが、サーバーを再起動するとデータが失われます。
*   データをディスクに保存するH2：これは、アプリケーションの再起動時にデータが失われないため、インメモリで実行するよりも優れたオプションです。
*   プロダクション用に選択したのと同じデータベース：セットアップは少し複雑ですが、最終的にはプロダクションで使用するデータベースと同じデータベースで作業する方がよいでしょう。これは、[開発ガイド]({{ site.url }}/development/)で説明されているように、liquibase-hibernateを使用する最善の方法でもあります。

構成するには、`src/main/resources/config/application-dev.yml`ファイルを変更してください。

### Springのキャッシュ抽象化を使用しますか?（Do you want to use the Spring cache abstraction?）

Springのキャッシュ抽象化では、さまざまなキャッシュ実装を使用できます。[ehcache](http://ehcache.org/)（ローカルキャッシュ）、[Caffeine](https://github.com/ben-manes/caffeine)（ローカルキャッシュ）、[Hazelcast](http://www.hazelcast.com/)（分散キャッシュ）、[Infinispan](http://infinispan.org/)（分散キャッシュ）、[Memcached](https://memcached.org/)（その他の分散キャッシュ）、または[Redis](https://redis.io/)（単一サーバキャッシュとして構成）を使用できます。これはアプリケーションのパフォーマンスに非常にプラスの影響を与える可能性があるため、推奨されるオプションです。

### Hibernateの第2レベルのキャッシュを使用しますか?（Do you want to use Hibernate 2nd level cache?）

このオプションは、SQLデータベースの使用を選択し（JHipsterはSpring Data JPAを使用してアクセスするため）、前の質問でキャッシュプロバイダを選択した場合にのみ使用できます。

[Hibernate](http://hibernate.org/)は、JHipsterで使用されるJPAプロバイダであり、キャッシュプロバイダを使用してパフォーマンスを大幅に向上させることができます。そのため、このオプションを使用し、アプリケーションのニーズに応じてキャッシュ実装を調整することを強くお勧めします。

### MavenとGradleのどちらを使用しますか?（Would you like to use Maven or Gradle?）

生成されたJavaアプリケーションは、[Maven](http://maven.apache.org/)または[Gradle](http://www.gradle.org/)を使用して構築できます。Mavenはより安定しており、より成熟しています。Gradleはより柔軟で、拡張が容易で、話題が豊富です。

### 他にどのテクノロジーを使用しますか?（Which other technologies would you like to use?）

これは、1つ以上の他のテクノロジをアプリケーションに追加するための複数選択の回答です。使用可能なテクノロジは以下のとおりです。

#### swagger-codegenを使用したAPIファーストな開発（API first development using swagger-codegen）

このオプションを使用すると、[Swagger-Codegen](https://github.com/swagger-api/swagger-codegen)をビルドに統合することによって、アプリケーションに対して[APIファーストな開発]({{ site.url }}/doing-api-first-development)を実施できます。

#### ElasticSearchを使用した検索エンジン（Search engine using ElasticSearch）

[Elasticsearch](https://github.com/elastic/elasticsearch)は、Spring Data Elasticsearchを使用して構成されます。詳細については[Elasticsearchガイド]({{ site.url }}/using-elasticsearch/)を参照してください。

#### Hazelcastを使ったクラスタ化されたHTTPセッション（Clustered HTTP sessions using Hazelcast）

デフォルトでは、JHipsterは[Spring Security](http://docs.spring.io/spring-security/site/index.html)の認証および認可情報を格納するためにのみHTTPセッションを使用し、HTTPセッションにより多くのデータを格納することを選択できます。
HTTPセッションを使用すると、クラスタで実行している場合、特に「スティッキーセッション」でロードバランサを使用していない場合に問題が発生します。
クラスター内でセッションを複製する場合は、[Hazelcast](http://www.hazelcast.com/)を設定するためにこのオプションを選択してください。

#### WebSocket（Spring Websocket）の使用（WebSockets using Spring Websocket）

WebSocketは、Spring Websocketを使用して有効にできます。また、フレームワークを効率的に使用する方法を示す完全なサンプルも提供します。

#### Apache Kafkaを使用した非同期メッセージ（Asynchronous messages using Apache Kafka）

[Apache Kafka]({{ site.url }}/using-kafka/)をパブリッシュ/サブスクライブ・メッセージ・ブローカーとして使用します。

<<<<<<< HEAD
### クライアントにどの _フレームワーク_ を使用しますか?（Which _Framework_ would you like to use for the client?）
=======
#### Asynchronous messages using Apache Pulsar

Use [Apache Pulsar]({{ site.url }}/using-pulsar/) as a publish/subscribe message broker.

### Which _Framework_ would you like to use for the client?
>>>>>>> upstream/main

使用するクライアント側のフレームワークです。

次のいずれかを使用できます。

*   Angular
*   React
*   Vue

### Bootswatchのテーマを使用しますか?（Would you like to use a Bootswatch theme?）

使用するクライアントテーマです。

[Bootswatch](https://bootswatch.com/)の任意のテーマを使用するか、デフォルトのテーマをそのまま使用できます。

### CSSにSassスタイルシートプリプロセッサを使用しますか?（Would you like to use the Sass stylesheet preprocessor for your CSS?）

[Sass](https://sass-lang.com/)は、CSSの設計を単純化するための優れたソリューションです。効率的に使用するには、[Webpack](https://webpack.js.org)開発サーバを実行する必要があります。これは自動的に設定されます。

### 国際化のサポートを有効にしますか?（Would you like to enable internationalization support?）

デフォルトでは、JHipsterはクライアント側とサーバー側の両方で優れた国際化サポートを提供します。ただし、国際化は少しオーバーヘッドが追加され、管理が少し複雑になるため、この機能をインストールしないことを選択できます。

JHipsterはUIの国際化のみをカバーしていることに注意してください。データの国際化のためには、JPA/Hibernateレイヤーを自分でコーディングする必要があります。

### どのテストフレームワークを使用しますか?（Which testing frameworks would you like to use?）

デフォルトでは、JHipsterはJavaのユニット/統合テスト（SpringのJUnitサポートを使用）とJavaScriptのユニット・テスト（Jestを使用）を提供します。オプションとして、以下のサポートを追加できます。

*   Gatlingを使用したパフォーマンステスト
*   Cucumberを使用した受け入れテスト
*   Protractorを使用したAngularとの統合テスト

詳細については、[「テストの実行」ガイド]({{ site.url }}/running-tests/)を参照してください。

### Liquibaseの増分変更ログを使用しますか?（Would you like to use incremental Liquibase changelogs?）

JHipsterはオプションで増分変更ログを作成します。そのため、データベースを再作成したりLiquibaseのdiffを手動で生成したりする必要はありません。

JHipsterを有効にするには、いつでも`--incremental-changelog`オプションを使用してJHipsterを実行してください。

JHipsterを実行する場合、エンティティには次の2つの状態が含まれます。

*   ディスクにすでに保存されている古い状態
*   メモリー内にある新しい状態（jdlまたはプロンプトから生成）

それらの間で相違が生成され、変更ログが作成されます。

サポートされている機能は以下のとおりです。

*   フィールドの作成/削除
*   関係の作成/削除
*   JDLおよびプロンプト

タイプや制約などの属性の変更はサポートしていません。

以下とは競合します。

*   `--fork`オプションを指定すると、古い状態を上書きしてディスクに保存します。

### JHipster Marketplaceから他のジェネレータをインストールしますか?（Would you like to install other generators from the JHipster Marketplace?）

[JHipster Marketplace]({{ site.url }}/modules/marketplace/)では、サードパーティの開発者によって作成された追加モジュールをインストールして、プロジェクトに非公式機能を追加できます。

<h2 id="5">Blueprintの使用</h2>

JHipster 5は、Blueprintの概念を導入しています。Blueprintは、JHipsterからのテンプレートをオーバーライドするカスタムのクライアント／サーバ側テンプレートを提供できるJHipsterモジュールです。例えば、[KotlinのBlueprint](https://github.com/jhipster/jhipster-kotlin)は、Javaサーバ側コードの大部分をKotlinに置き換えます。

例えば、KotlinのBlueprintを使用するには、アプリを生成する際に以下のようにBlueprintの名前を渡します。

```bash
jhipster --blueprint kotlin
```

Blueprintの名前は`.yo-rc.json`に保存され、`entity`、`spring-controller`、`spring-service`などのサブジェネレータを実行する際に自動的に使用されます。

Blueprintに特定のサブジェネレータが実装されていない場合、そのサブジェネレータはスキップされ、同じサブジェネレータのJHipsterテンプレートが使用されます。

**注意：**アプリケーションは1つのBlueprintのみを使用できます。複数のBlueprintはまだサポートされていません。

<h2 id="3">コマンドラインオプション</h2>

オプションのコマンドラインオプションを使用してのJHipsterの実行もできます。これらのオプションのリファレンスは、`JHipster app--help`と入力することで見つけることができます。

渡すことができるオプションは次のとおりです。

* `--help` - ジェネレータのオプションと使用法を出力します
* `--blueprint` - 使用するBlueprintを指定します。例：`jhipster --blueprint kotlin`
* `--skip-cache` - プロンプトの応答を記憶しません（デフォルト：false）
* `--skip-git` - 生成されたプロジェクトをGitに自動的に追加しません（デフォルト：false）
* `--skip-install` - 依存関係を自動的にインストールしません（デフォルト：false）
* `--skip-client` - クライアント側アプリケーションの生成をスキップし、Spring Bootバックエンドコードのみを生成します（デフォルト：false）
* `--skip-server` - フロントエンドコードのみが生成されるように、サーバ側アプリケーションの生成をスキップします（デフォルト：false）
* `--skip-user-management` - バックエンドとフロントエンドの両方でユーザ管理の生成をスキップします（デフォルト：false）
* `--i18n` - クライアント側の生成をスキップする場合にi18nを無効または有効にします。それ以外の場合は影響ありません（デフォルト：true）
* `--auth` - サーバ側の生成をスキップする場合に認証タイプを指定します。それ以外の場合は影響ありませんが、`skip-server`を使用する場合は必須です
* `--db` - サーバ側の生成をスキップする場合にデータベースを指定します。それ以外の場合は影響ありませんが、`skip-server`を使用する場合は必須です
* `--with-entities` - 既存のエンティティがすでに生成されている場合は、それらを再生成します（`.jhipster`フォルダ内の構成を使用）（デフォルト：false）
* `--skip-checks` - 必要なツールのチェックをスキップします（デフォルト：false）
* `--jhi-prefix` - サービス、コンポーネント、および状態/ルート名の前にプレフィックスを追加します（デフォルト：jhi）
* `--entity-suffix` - エンティティクラス名の後に接尾辞を追加します（デフォルト：空の文字列）
* `--dto-suffix` - DTOクラス名の後にサフィックスを追加します（デフォルト：DTO）
* `--yarn` - NPMの代わりにYarnを使用します（デフォルト：false）
* `--prettier-java` - すべてのJavaクラスをフォーマットするために[prettier-java](https://github.com/jhipster/prettier-java)を使用します（デフォルト：false）
* `--experimental` - 実験的な機能を有効にします。これらの機能は不安定であり、いつでも重大な変更を受ける可能性があることに注意してください
* `--skip-fake-data` - 開発用のフェイクデータの生成をスキップします
* `--creation-timestamp` - 再現可能なビルドのタイムスタンプを設定します。タイムスタンプは解析可能なjs日付である必要があります（例：2019-01-01）。--with-entitiesまたはimport-jdl（generator-jhipster > 6.5.1）とともに使用する必要があります

<h2 id="4">ヒント</h2>

また、`--force`のようなYeomanコマンドラインオプションを使用して、既存のファイルを自動的に上書きもできます。そのため、エンティティを含むアプリケーション全体を再生成したい場合は、`jhipster--force--with-entities`を実行できます。
