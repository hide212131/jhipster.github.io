---
layout: default
title: JHipsterドメイン言語(JDL) - アプリケーション
permalink: /jdl/applications
sitemap:
    priority: 0.5
    lastmod: 2019-10-27T12:00:00-00:00
---

# <i class="fa fa-star"></i> JHipsterドメイン言語(JDL) - アプリケーション

## 概要

1.  [構文](#構文)
1.  [アプリケーションのオプション](#アプリケーションのオプション)
1.  [例](#例)
    1.  [基本の例](#基本の例)
    1.  [複数のアプリケーション](#複数のアプリケーション)
    1.  [エンティティを付与](#エンティティを付与)
    1.  [オプションを付与](#オプションを付与)
1.  [マイクロサービスワークフロー](#マイクロサービスワークフロー)
1.  [完全なサンプルのブレークダウン](#完全なサンプルのブレークダウン)
1.  [使用可能なアプリケーション構成オプション](#使用可能なアプリケーション構成オプション)
1.  [関連項目](#関連項目)

***

### 構文

アプリケーション宣言は、次のように行われます。

```
application {
  config {
    <アプリケーション・オプション名> <アプリケーション・オプション値>
  }
  [entities <アプリケーション・エンティティのリスト>]
  [<オプション>]
}
```

  - アプリケーションの設定キー/値は `config` の下に指定されます（`application`の中にある必要があります）。
  -   0、1、または任意のアプリケーション・オプションを指定できます（有効な場合）。
  - アプリケーション内で生成されるエンティティは、`entities`のリストにします。これは、アプリケーションでエンティティを生成するための
    推奨される方法です。
    - これは省略できますが、その場合アプリケーション内でエンティティを生成するには、以下を行う必要があります。
        - アプリケーション内の別のJDLファイルから生成
        - またはCLIを使用
  - `entities`キーワードはオプションです。省略できますが、JDLファイル内のすべてのエンティティが
  アプリケーション内で生成されることになります。
-   アプリケーションには通常のオプション（`dto`または`service`）を持つことができます。詳細については[次の](#options-in-applications)セクションで説明します。

---

### アプリケーションのオプション

オプションの宣言（`dto`, `service`, `skipServer`など）はJDLのアプリケーションでサポートされていますが、いくつかのルールがあります。

たとえば、次のJDLファイルがあるとします。
```jdl
application {
  config {
    baseName app1
  }
  entities A, B, C
  dto * with mapstruct
}

application {
  config {
    baseName app2
  }
  entities C, D
  paginate * with pagination except D 
}

application {
  config {
    baseName app3
  }
  entities * except A, B, C, D, F
  service * with serviceClass
}

entity A
entity B
entity C
entity D
entity E
entity F

paginate * with infinite-scroll
```

このサンプルでは、いくつかのことがわかります。
  - JDLファイルには、`A, B, C, D, E, F`の6つの宣言されたエンティティがあります。
  - 3つのアプリケーション`app1, app2, app3`があります。
    - `app1`は`A, B, C`を使用します。
    - `app2`は`C, D`を使用します。
    - `app3`は`E`を使用します（`* except A, B, C, D, F`により）
  - 各アプリケーションではオプションと**グローバルな** オプションを宣言しています。
    - `app1`は`dto`宣言を`A, B and C`に対して行います。
    - `app2`は`paginate`宣言を`C`に対して行います（除外があるため）。
    - `app3`は`service`を`E`に対して行います。
    - グローバルで`pagination`を（全てのエンティティに対して）宣言します。

どのようにファイルが生成されるかを以下に示します。
  - `app1`
    - `A`: `infinite-scroll の paginate`（グローバルオプションはローカルオプションを上書きしません）と
      `dto mapstruct`を使用
    - `B`: も同様のオプションを使用
    - `C`: も同様のオプションを使用
  - `app2`:
    - `C`: `pagination の paginate`（ローカルが優先のため`infinite-scroll`は無し）
    - `D`: それまでのオプションが`D`を含めていないため`infinite-scroll の paginate`を使用
  - `app3`:
    - `E`: `infinite-scroll の paginate`と`serviceClass の service`

この例は、**シャドーイング**の原理を示しています。グローバルオプションはサポートされており、アプリケーションでオプションが宣言**されていない限り**、
宣言されたすべてのアプリケーションで使用されます。

また、次のスニペットは`app3`の前のサンプルから引用したものです。
```jdl
entities * except A, B, C, D, F
service * with serviceClass
```

基本として`app3`は`E`のみを使用し、アプリケーションのエンティティは`service`オプションを使用するという意味です。
これは`E`のみであり、`AからF`ではありません。

最後に、`F`エンティティどのアプリケーションにも存在せず、そのため、このエンティティは生成されません。

_注：現時点では、すべての標準オプションがサポートされています。_

---

### 例

#### 基本の例

```jdl
application {
  config {
    baseName exampleApp
    applicationType gateway
  }
}
```

---

#### 複数のアプリケーション

```jdl
application {
  config {
    baseName exampleApp1
    applicationType microservice
    serverPort 9001
  }
}

application {
  config {
    baseName exampleApp2
    applicationType microservice
    serverPort 9002
  }
}

application {
  config {
    baseName exampleApp3
    applicationType gateway
    serverPort 9000
  }
}
```

---

#### エンティティを付与

```jdl
application {
  config {
    baseName exampleApp1
    applicationType microservice
    serverPort 9001
  }
  entities A
}

application {
  config {
    baseName exampleApp2
    applicationType microservice
    serverPort 9002
  }
  entities * except A
}

entity A
entity B
entity C
```

---

#### オプションを付与

```jdl
application {
  config {
    baseName exampleApp1
    applicationType microservice
    serverPort 9001
  }
  entities A
  dto A with mapstruct 
}

application {
  config {
    baseName exampleApp2
    applicationType microservice
    serverPort 9002
  }
  entities * except A
  paginate C with pagination
}

entity A
entity B
entity C
```

---

### 完全なサンプルのブレークダウン

例1：

```jdl
application {
  config {
    baseName myMonolith
    applicationType monolith
  }
  entities * except C, D
}
application {
  config {
    baseName myGateway
    applicationType gateway
    serverPort 9042
  }
  entities * except A, B
}
application {
  config {
    baseName microserviceA
    applicationType microservice
  }
  entities C
}
application {
  config {
    baseName microserviceB
    applicationType microservice
    serverPort 8082
  }
  entities D
}
entity A
entity B
entity C
entity D
dto * with mapstruct
service * with serviceClass
paginate D with pagination
```

これらのアプリケーションおよびフォルダを生成すると、いくつかの処理が行われます。
  - 4つのアプリケーションが作成されます。
    - サーバーポートを`8080`とし、`./myMonolith`以下にmyMonolithを作成
    - サーバーポートを`9042`とし、`./myGateway`以下にmyGatewayを作成
    - サーバーポートを`8081`とし、`./microserviceA`以下にmicroserviceAを作成
      - サーバー・ポートを指定しなかったとしても、JHipsterはデフォルトでポートを設定します。
      - マイクロサービスの場合、デフォルトは`8081`です。
      - ゲートウェイとモノリスの場合は`8080`です。
    - サーバーポートを`8082`とし、`./microserviceB`以下にmicroserviceBを作成
  - 4つのエンティティが生成されます。
    - モノリス内に`A`と`B`
    - ゲートウェイ内に`C`と`D`
      - 最初のマイクロサービスに`C`
      - 2番目のマイクロサービスに`D`
  - `microservice`オプションは、`C`および`D`に対して暗黙的に付けられます。
    - 2つのマイクロサービスで生成されるため、このオプションはデフォルトで設定されます。
  - オプションは前述と同じように機能します。

デフォルト値が存在しない場合、ジェネレータは（`databaseType`のように）デフォルト値を設定することに注意してください。
JHipster Coreはまったく同じ動作をしてくれます。

---

例2：オプションの使用
[オプションのセクション](#options-in-applications)を参照してください。

---

### マイクロサービスワークフロー

マイクロサービスを扱うのは難しい作業ですが、JDLには、エンティティを適切に処理するためのオプションがいくつか用意されています。
`microservice <エンティティ> with <マイクロサービスアプリの名前>`により、どのマイクロサービスでどのエンティティを生成するかを指定できます。

例えば、次のように設定します。
```
entity A
entity B
entity C
microservice A with firstMS
microservice B with secondMS
```

2つのJHipsterアプリケーション（'firstMS'と'secondMS'）を使用した場合、JDLファイルをインポートすると次のようになります。
次の2つのアプリケーションについて：
  - 'firstMS'では、エンティティ`A`と`C`が生成されます。
  - 'secondMS'では、エンティティ`B`と`C`が生成されます。

`C`は両方で生成されます。なぜなら、このエンティティが生成される場所を指定するマイクロサービスオプションがない場合でも
両方のアプリケーションで生成されるからです。

このJDLをモノリスアプリケーションにインポートすると、すべてのエンティティが生成されます（このJDL内には、モノリスは制約に関する
オプションが無いため）。

_注:同じエンティティを2つの異なるマイクロサービスで生成したい場合は、JDLファイルを更新するのではなく、
その都度、複数のJDLファイルを作成することで実現できます。_

上記の例は次のような記述はできません。
```
entity A
entity B
entity C
microservice * except B with firstMS
microservice * except A with secondMS
```

結果はこうなります。
  - 'firstMS'では、エンティティ`C`のみが生成されます。
  - 'secondMS'では、エンティティ`B`と`C`が生成されます。

なぜなら、構文解析時に、あるオプションが別のオプションと重複する場合には、後者が優先されるからです。
JDLを使用してマイクロサービススタック全体の作成もできます。例として[このブログを参照してください](https://medium.com/@deepu105/create-full-microservice-stack-using-jhipster-domain-language-under-30-minutes-ecc6e7fc3f77)。

---

### 使用可能なアプリケーション構成オプション

JDLでサポートされているアプリケーションオプションは次のとおりです。

_あなたが探しているものではありませんか?[通常のオプション](/jdl/options#available-options)をチェックしてください。_

<table class="table table-striped table-responsive">
  <tr>
    <th>JDLオプション名</th>
    <th>デフォルト値</th>
    <th>指定可能な値</th>
    <th>コメント</th>
  </tr>
  <tr>
    <td>applicationType</td>
    <td>monolith</td>
    <td>monolith, microservice, gateway</td>
    <td></td>
  </tr>
  <tr>
    <td>authenticationType</td>
    <td>jwt</td>
    <td>jwt, session, oauth2</td>
    <td>jwt</td>
  </tr>
  <tr>
    <td>baseName</td>
    <td>jhipster</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><s>blueprint</s></td>
    <td>DEPRECATED</td>
    <td><s>追加のブループリントの名前</s></td>
    <td><a href="#blueprints">blueprints</a>を参照</td>
  </tr>
  <tr>
    <td id="blueprints">blueprints</td>
    <td>[]</td>
    <td>追加のBlueprintの名前。<a href="https://www.jhipster.tech/modules/marketplace/#/list">マーケットプレイス</a>を参照。内部で公開されているカスタムブループリントも含まれます</td>
    <td>使用するBlueprint配列。例:<code>[blueprint1,blueprint2]</code></td>
  </tr>
  <tr>
    <td>buildTool</td>
    <td>maven</td>
    <td>maven, gradle</td>
    <td></td>
  </tr>
  <tr>
    <td>cacheProvider</td>
    <td>ehcache or hazelcast</td>
    <td>caffeine, ehcache, hazelcast, infinispan, memcached, redis, no</td>
    <td>モノリスやゲートウェイにはehcache、それ以外にはHazelcast</td>
  </tr>
  <tr>
    <td>clientFramework</td>
    <td>angularX</td>
    <td>angularX, angular, react, vue, svelte, no</td>
    <td></td>
  </tr>
  <tr>
    <td>clientPackageManager</td>
    <td>npm</td>
    <td>npm, yarn</td>
    <td></td>
  </tr>
  <tr>
    <td>clientTheme</td>
    <td>none</td>
    <td>何かを指定もしくはnone</td>
    <td>（yetiのように）動作することがわかっている場合は、任意の値を設定できます</td>
  </tr>
  <tr>
    <td>clientThemeVariant</td>
    <td></td>
    <td>何かを指定もしくはnone</td>
    <td>（darkやlightのように）動作することがわかっている場合は、任意の値を設定できます。空欄も可</td>
  </tr>
  <tr>
    <td>databaseType</td>
    <td>sql</td>
    <td>sql, mongodb, cassandra, couchbase, no</td>
    <td></td>
  </tr>
  <tr>
    <td>devDatabaseType</td>
    <td>h2Disk</td>
    <td>h2Disk, h2Memory, *</td>
    <td>* + プロダクションデータベースのタイプ</td>
  </tr>
  <tr>
    <td>dtoSuffix</td>
    <td>DTO</td>
    <td></td>
    <td>DTOsの接尾辞。空の文字列の場合はfalse。</td>
  </tr>
  <tr>
    <td>enableHibernateCache</td>
    <td>true</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>enableSwaggerCodegen</td>
    <td>false</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>enableTranslation</td>
    <td>true</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>entitySuffix</td>
    <td></td>
    <td></td>
    <td>エンティティの接尾辞。空の文字列の場合はfalse。</td>
  </tr>
  <tr>
    <td>jhiPrefix</td>
    <td>jhi</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>languages</td>
    <td>[en, fr]</td>
    <td>JHipsterで使用可能な言語</td>
    <td>中括弧は必須</td>
  </tr>
  <tr>
    <td>messageBroker</td>
    <td>no</td>
    <td>kafka, pulsar, no</td>
    <td></td>
  </tr>
  <tr>
    <td>nativeLanguage</td>
    <td>en</td>
    <td>JHipsterがサポートするすべての言語</td>
    <td></td>
  </tr>
  <tr>
    <td>packageName</td>
    <td>com.mycompany.myapp</td>
    <td></td>
    <td>packageFolderオプションが設定</td>
  </tr>
  <tr>
    <td>prodDatabaseType</td>
    <td>mysql</td>
    <td>mysql, mariadb, mssql, postgresql, oracle, no</td>
    <td></td>
  </tr>
  <tr>
    <td>reactive</td>
    <td>false</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>searchEngine</td>
    <td>no</td>
    <td>elasticsearch, couchbase, no</td>
    <td></td>
  </tr>
  <tr>
    <td>serverPort</td>
    <td>8080, 8081 or 9999</td>
    <td></td>
    <td>アプリケーションのタイプに依存</td>
  </tr>
  <tr>
    <td>serviceDiscoveryType</td>
    <td>no</td>
    <td>eureka, consul, no</td>
    <td></td>
  </tr>
  <tr>
    <td>skipClient</td>
    <td>false</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>skipServer</td>
    <td>false</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>skipUserManagement</td>
    <td>false</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>testFrameworks</td>
    <td>[]</td>
    <td>cypress, protractor, cucumber, gatling</td>
    <td>中括弧は必須</td>
  </tr>
  <tr>
    <td>websocket</td>
    <td>no</td>
    <td>spring-websocket, no</td>
    <td></td>
  </tr>
</table>

---

### 関連項目

通常のオプションは[ここ](/jdl/options)にあります。
