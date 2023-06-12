---
layout: default
title: テストの実行
permalink: /running-tests/
redirect_from:
  - /running_tests.html
sitemap:
    priority: 0.7
    lastmod: 2019-04-19T00:00:00-00:00
---

# <i class="fa fa-shield"></i> テストの実行

## はじめに

JHipsterには広範なテストセットが用意されており、生成された各アプリケーションには次のものがあります。

*   [JUnit 5](https://junit.org/junit5/){:target="_blank" rel="noopener"}を使用したユニットテスト
*   Spring Test Contextフレームワークを使用した統合テスト
*   [Jest](https://facebook.github.io/jest/){:target="_blank" rel="noopener"}を使用したUIテスト
*   [ArchUnit](https://www.archunit.org/){:target="_blank" rel="noopener"}を使用したアーキテクチャテスト

オプションで、JHipsterは次も生成できます。

*   [Gatling](http://gatling.io/){:target="_blank" rel="noopener"}を使用したパフォーマンステスト
*   [Cucumber](https://cucumber.io/){:target="_blank" rel="noopener"}を使用した動作駆動型テスト
*   [Cypress](https://www.cypress.io/){:target="_blank" rel="noopener"}または[Protractor](https://angular.github.io/protractor/#/){:target="_blank" rel="noopener"}を使用したAngular/React/Vue統合テスト

これらのテストの生成には、2つの目標があります。

*   テストはすべてのアプリケーションの非常に有用な部分であると私たちは信じているので、すべてのJHipsterユーザーがベストプラクティスに従うことを支援します。
*   生成されたものが正しいことを検証します。そのため、これらのテストをまったく使用する予定がない場合でも、アプリケーションの生成後に`./mvnw clean verify`および`npm test`を実行することは、すべてが正常であるかどうかを確認するための優れた方法です。テストが時間の無駄であると思われる場合は、これらのテストを無視してもかまいません!

これらのテストはすべて、標準のMaven`src/test`フォルダに生成されます。

## 統合テスト

統合テストはSpring Test Contextフレームワークを使用して実行され、`src/test/java`フォルダに格納されます。JHipsterは、すべてのテストで再利用される特定のSpringテストコンテキストを次のように起動します。

*    Spring Beanはステートレスでスレッドセーフである必要があるため、さまざまなテストスイートで再利用できます。
*    すべてのテストに対して1つのSpringコンテキストのみを起動する方が、各テストに対して新しいSpringコンテキストを起動するよりもはるかに高速です。

このSpringテスト・コンテキストでは、特定のテスト・データベースを使用してテストを実行します。

*   SQLデータベースを使用する場合、JHipsterは統合テストに一時データベースを使用するためにインメモリH2インスタンスを起動します。または、`testcontainers`プロファイルを使用することにより、JHipsterは[Testcontainers](https://www.testcontainers.org/modules/databases/){:target="_blank" rel="noopener"}を使用してコンテナ化されたバージョンのプロダクションデータベースを起動します。いずれの場合も、Liquibaseが自動的に実行され、データベーススキーマが生成されます。
*    Cassandraを使用している場合、JHipsterは、[Testcontainers](https://www.testcontainers.org){:target="_blank" rel="noopener"}を使用して、Dockerでコンテナ化されたバージョンのCassandraを起動します。
*    MongoDBを使用している場合、JHipsterは[de.flapdoodle.embed.mongo](https://github.com/flapdoodle-oss/de.flapdoodle.embed.mongo){:target="_blank" rel="noopener"}を使用してインメモリMongoDBインスタンスを起動します。
*    Elasticsearchを使用している場合、JHipsterはSpring Data Elasticsearchを使用してインメモリElasticsearchインスタンスを起動します。
*    Couchbaseを使用している場合、JHipsterは、[Couchbase Testcontainers](https://github.com/differentway/testcontainers-java-module-couchbase){:target="_blank" rel="noopener"}を使用して、DockerでCouchbaseのコンテナ化バージョンを起動します。
*    Neo4jを使用している場合、JHipsterは、[Neo4j Testcontainers](https://www.testcontainers.org/modules/databases/Neo4j/){:target="_blank" rel="noopener"}を使用して、DockerでNeo4jのコンテナ化バージョンを起動します。

これらのテストは、各テストクラスを右クリックするか、`./mvnw clean verify`（Gradleを使用している場合は`./gradlew test integrationTest`）を実行することによって、IDEで直接実行できます。

**制限事項：** 生成されたエンティティで検証が有効になっている場合、JHipsterは検証ルールに応じて正しい値を生成できません。これらのルールは非常に複雑である可能性があるため（Regexパターンが使用されている場合など）、これは不可能です。この場合、テストは検証に失敗し、テストで使用されるデフォルト値は、検証ルールに合格できるように手動で変更する必要があります。

## UIテスト

JHipsterのUIテストには、JestによるユニットテストとProtractorによる統合テストの2種類があります。デフォルトではJestのみが提供されていますが、アプリケーションのテストカバレッジを良好にしたい場合は、両方のツールを一緒に使用することをお勧めします。

### Jest

UIユニットテストは`src/test/javascript/spec`フォルダにあります。それらは[Jest](https://facebook.github.io/jest/){:target="_blank" rel="noopener"}を使用します。

これらのテストはアプリケーションのRESTエンドポイントへのアクセスをモックアップするため、Javaバックエンドを起動せずにUIレイヤーをテストできます。

*   これらのテストは`npm test`を使用して実行できます。
*   ヒント：1つのテストに集中したい場合は、モジュールの記述を`describe('...', function() {`から`fdescribe('...', function() {`に変更すると、Jestはこのテストのみを実行します。

### Cypress/Protractor

UI統合テストは、[Cypress](https://www.cypress.io/){:target="_blank" rel="noopener"}または[Protractor](https://angular.github.io/protractor/#/){:target="_blank" rel="noopener"}で行われ、`src/test/javascript/e2e`フォルダにあります。

これらのテストでは、ブラウザを起動し、実際のユーザーと同じようにアプリケーションを使用するため、データベースを設定した実際のアプリケーションを実行する必要があります。

これらのテストは、`npm run e2e`を使用して実行できます。

## アーキテクチャテスト

特定の制約とベストプラクティスを適用するアーキテクチャテストは、[ArchUnit](https://www.archunit.org/){:target="_blank" rel="noopener"}を使用して実行されます。
[公式ドキュメント](https://www.archunit.org/userguide/html/000_Index.html){:target="_blank" rel="noopener"}に従って、ビルド時にアーキテクチャのカスタム制約をチェックするための独自のルールを作成できます。

## パフォーマンステスト

<<<<<<< HEAD
パフォーマンステストは[Gatling](http://gatling.io/){:target="_blank" rel="noopener"}で行われ、`src/test/gatling`フォルダにあります。これらはエンティティごとに生成され、多くの同時ユーザー要求でそれぞれをテストできます。

Gatlingテストを実行するには、次の手順を実行する必要があります。

1. [Gatlingをダウンロードします](https://gatling.io/open-source/){:target="_blank" rel="noopener"}。
2. ファイルを抽出し、その場所を`PATH`に追加します。
3. cdで`src/test/gatling`に移動し、お使いのOSに応じて`gatling.sh`または`gatling.bat`を実行します。
=======
Performance tests are done with [Gatling](http://gatling.io/){:target="_blank" rel="noopener"}, and are located in the `src/test/java/gatling/simulations` folder. They are generated for each entity, and allow to test each of them with a lot of concurrent requests.
>>>>>>> upstream/main

**警告!** 現時点では、これらのテストではエンティティに適用した検証ルールが考慮されていません。また、別のエンティティと必要な関係を持つエンティティを作成するためのテストは、そのままでは失敗します。いずれにしても、ビジネス・ルールに従ってこれらのテストを変更する必要があるため、テストを改善するためのヒントをいくつか示します。

*   実行中のアプリケーションで、`管理 > ログ`画面に移動し、`org.springframework`を`debug`モードにします。例えば、検証エラーが表示されます。
*   アプリケーションを通常に使用し、Chromeの`console log`を開くと、HTTPヘッダーを含むすべてのパラメータを持つRESTのリクエストを確認できます。

マイクロサービスアプリケーションでGatlingテストを実行するには、次のことが必要です。

*   レジストリの実行
*   ゲートウェイの実行
*   マイクロサービスアプリケーションの実行
*   その後、Gatlingテストを実行できます

<<<<<<< HEAD
### Gatlingを実行するためのMaven/Gradleの使用

Gatlingテストを実行するためのMavenまたはGradleの設定は生成しません。これは、他のプラグインでクラスパスの問題（主にScalaを使用時）が発生する可能性があるためです。
ただし、公式の[Mavenプラグイン](https://gatling.io/docs/current/extensions/maven_plugin/){:target="_blank" rel="noopener"}または[Gradleプラグイン](https://gatling.io/docs/current/extensions/gradle_plugin/){:target="_blank" rel="noopener"}を利用してGatlingテストの実行は可能です。

#### Mavenの使用

`pom.xml`を変更する必要があります。

1.  `test`スコープのGatling依存関係を追加
2.  Gatlingプラグインの追加
3.  プラグイン構成をJHipsterのレイアウトと命名規則に適合

```
...
<dependency>
  <groupId>io.gatling.highcharts</groupId>
  <artifactId>gatling-charts-highcharts</artifactId>
  <version>3.5.0</version>
  <scope>test</scope>
</dependency>
<!-- jhipster-needle-maven-add-dependency -->
...
<plugin>
  <groupId>io.gatling</groupId>
  <artifactId>gatling-maven-plugin</artifactId>
  <version>3.1.1</version>
  <configuration>
    <runMultipleSimulations>true</runMultipleSimulations>
    <resourcesFolder>${project.basedir}/src/test/gatling/conf</resourcesFolder>
    <simulationsFolder>${project.basedir}/src/test/gatling/user-files/simulations</simulationsFolder>
  </configuration>
</plugin>
<!-- jhipster-needle-maven-add-plugin -->
...
```

すべてのGatlingテストは`./mvnw gatling:test`で実行できます。
#### Gradleの使用

`build.gradle`を変更する必要があります。

1. Gatlingプラグインをプラグインセクションに追加する
2. ソースセットをJHipsterレイアウトに適合させる
3. 含まれているシミュレーションをJHipsterの命名規則に適合させる

リアクティブオプションを使用している場合は、[Spring Bootが管理するNettyバージョンが、Gatlingが必要とするバージョンと干渉しないことを確認する必要があります](https://gatling.io/docs/current/extensions/gradle_plugin/#spring-boot-and-netty-version){:target="_blank" rel="noopener"}。

```
plugins {
    ...
    id "io.spring.nohttp"
    // Gatlingプラグインを追加するには、https://plugins.gradle.org/plugin/io.gatling.gradle で最新バージョンを確認してください
    id 'io.gatling.gradle' version "3.5.0" 
    //jhipster-needle-gradle-plugins - JHipsterはここにgradleプラグインを追加します。
}

...
// ソースセットをJHipster固有のレイアウトに適合させます
sourceSets {
   gatling {
    scala.srcDirs = ["src/test/gatling/user-files/simulations"]
    resources.srcDirs = ["src/test/gatling/conf"]
  }
} 

gatling {
    simulations = { include "**/*Test*.scala" }
}
...
```
=======
### Using Maven to run Gatling

You can execute all Gatling tests with `./mvnw gatling:test`.
### Using Gradle to run Gatling
>>>>>>> upstream/main

すべてのGatlingのテストは`./gradlew gatlingRun`で実行できます。
## ビヘイビア駆動開発（BDD）

振る舞い駆動開発（BDD）は、[Cucumber](https://cucumber.io/){:target="_blank" rel="noopener"}とその[JVM実装](https://github.com/cucumber/cucumber-jvm){:target="_blank" rel="noopener"}を使用して利用できます。

[Gherkin](https://docs.cucumber.io/gherkin/reference/){:target="_blank" rel="noopener"}のfeatureは、`src/test/features`ディレクトリに書き込む必要があります。
