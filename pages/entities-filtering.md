---
layout: default
title: フィルタリング
permalink: /entities-filtering/
sitemap:
    priority: 0.7
    lastmod: 2017-08-22T00:00:00-00:00
---

# <i class="fa fa-filter"></i> エンティティのフィルタリング

## 導入

基本的なCRUD機能がエンティティに実装された後、エンティティの属性に対してさまざまなフィルタを作成したいという、非常に一般的な要求があります。
これにより、サーバをより効果的に使用できるようになります。これらのフィルタは、すべてのクライアント（およびすべてのブラウザ）がそれを使用できるように、リクエストパラメータとして送信される必要があります。
さらに、これらのフィルタは、合理的で簡潔なパターンに従う必要があり、自由に組み合わせることができなければなりません。

## アクティブにする方法

_注意_：`filter`は`reactive`と互換性がありません。
`jhipster entity`コマンドを使用してエンティティを生成する際に、サービスまたはサービス実装を選択して、このエンティティのフィルタリングを有効にします。

既存のエンティティのフィルタを有効にする場合は、プロジェクトの`.jhipster`ディレクトリでエンティティ構成を変更します。変更するには、`service`を`serviceClass`に、または`serviceImpl`を`no`とし、`jpaMetamodelFiltering`を`true`に設定して、`jhipster entity <エンティティ名>`で再生成します。

JDLを使用する場合は、JDLファイルに行`filter <エンティティ名>`を追加し、`jhipster jdl`コマンドを使用して定義を再インポートします。

## パブリック・インタフェース

エンティティごとに、エンティティジェネレータでフィルタリングを有効にできます。その後、次のパラメータを使用して`/api/my-entity`GETエンドポイントを呼び出すことができます。

* 各*xyz*フィールド用
    * *xyz.equals=someValue*
        - xyzが'someValue'な、すべてのエンティティをリストします。
    * *xyz.in=someValue,otherValue*
        - xyzが'someValue'または'otherValue'な、すべてのエンティティをリストします。
    * *xyz.specified=true*
        - xyzはNULLではない、すべてのエンティティをリストします。
    * *xyz.specified=false*
        - xyzはNULLまたは未指定となる、すべてのエンティティをリストします。
* *xyz*のタイプが文字列の場合
    * *xyz.contains=something*
        - xyzに'something'が含まれる、すべてのエンティティをリストします。
* *xyz*が数値型または日付型のいずれかである場合
    * *xyz.greaterThan=someValue*
        - xyzが'someValue'より大きくなる、すべてのエンティティをリストします。
    * *xyz.lessThan=someValue*
        - xyzが'someValue'より小さくなる、すべてのエンティティをリストします。        
    * *xyz.greaterThanOrEqual=someValue*
        - xyzが'someValue'以上となる、すべてのエンティティをリストします。
    * *xyz.lessThanOrEqual=someValue*
        - xyzが'someValue'以下となる、すべてのエンティティをリストします。

これらは自由に組み合わせられます。

このフィルターAPIの表現力を体験する良い方法は、JHipsterアプリケーションのAPIドキュメント・ページにあるswagger-uiから使用することです。

![]({{ site.url }}/images/entities_filtering_swagger.png)

## 実装

この機能を有効にすると、`EntityQueryService`という名前の新しいサービスと`EntityCriteria`が生成されます。Springはリクエストパラメータを`EntityCriteria`クラスのフィールドに変換します。
`EntityQueryService`では、クライテリアオブジェクトを静的でタイプセーフなJPAクエリオブジェクトに変換します。このためには、ビルドで**静的メタモデル生成が有効になっている**ことが**必要**です。詳細については、[JPA静的メタモデルジェネレータのドキュメント](http://docs.jboss.org/hibernate/orm/current/topical/html_single/metamodelgen/MetamodelGenerator.html)を参照してください。

<<<<<<< HEAD
生成されたクライテリアが機能し、Springが適切に構成されていることを証明するために、個々のフィルタごとに1つずつといった多くのテストケースを持つ`EntityResourceIntTest`が作られます。
=======
In the `EntityQueryService`, we convert the criteria object into a static, and type safe, JPA query object. For this, it is **required** that the **static metamodel generation is enabled** in the build. See the [JPA Static Metamodel Generator documentation](https://docs.jboss.org/hibernate/orm/current/userguide/html_single/Hibernate_User_Guide.html#tooling-modelgen) for details.

To prove that the generated criteria is working, and Spring is well configured, the `EntityResourceIntTest` is extended with lots of test cases, one for each individual filter.
>>>>>>> upstream/main

### Angular

Angularを使用する場合、この便利な機能を利用する適切な方法は、次のようになります。

* Equals（`contains`と`notEquals`にも当てはまります）
```javascript
this.bookService.query({'title.equals':someValue}).subscribe(...);
```
* greaterThan（`date`および`number`データ型を使用する場合、`lessThan`、`greaterThanOrEqual`、および`lessThanOrEqual`にも当てはまります）
```javascript
this.bookService.query({'id.greaterThan':value}).subscribe(...);
this.bookService.query({'birthday.lessThanOrEqual':value}).subscribe(...);
```
* In（`notIn`にも当てはまります）
```javascript
this.bookService.query({'id.in':[value1, value2]}).subscribe(...);
```
* Specified
```javascript
this.bookService.query({'author.specified':true}).subscribe(...);
```

## 制限事項

現在、SQLデータベース（JPAを使用）のみがサポートされており、個別のサービスまたは個別のサービス実装/インタフェースの組み合わせが提供されます。
