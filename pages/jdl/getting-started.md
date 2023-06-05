---
layout: default
title: JHipsterドメイン言語(JDL) - 入門
permalink: /jdl/getting-started
sitemap:
  priority: 0.5
  lastmod: 2021-03-08T12:00:00-00:00
---

# <i class="fa fa-star"></i> JHipsterドメイン言語（JDL） - 入門

## 概要

このページでは、JDLとアプリケーションとその周辺のすべてを作成する方法について学びます。

1. [コンテンツの生成](#コンテンツの生成)
   1. [ファイルの使用](#ファイルの使用)
   1. [インラインJDLの使用](#インラインjdlの使用)
1. [アプリケーションの生成](#アプリケーションの生成)
1. [エンティティの生成](#エンティティの生成)
1. [フィールドの生成](#フィールドの生成)
1. [列挙型](#列挙型)
1. [リレーションシップの追加](#リレーションシップの追加)
1. [オプション](#オプション)
1. [デプロイ](#デプロイ)
1. [定数](#定数)
1. [JDLファイルへのエクスポート](#jdlファイルへのエクスポート)

---

## コンテンツの生成

### ファイルの使用

JDLファイルを使用してエンティティを生成できます。

- 拡張子が'.jh'または'.jdl'のファイルを作成します。
アプリケーション、デプロイ、エンティティ、およびリレーションシップを宣言するか、または[JDL-Studio](https://start.jhipster.tech/jdl-studio/)や[JHipster IDE](https://www.jhipster.tech/jhipster-ide/)を使ってファイルを生成、ダウンロードします。
- エンティティのみを作成する場合は、JHipsterアプリケーションのルートフォルダで`jhipster jdl my_file`を実行します。
- アプリケーションを作成する場合は、フォルダ内で`jhipster jdl my_file.jdl`を実行します。

_ジャジャーン_ おしまいです！

チームで作業する場合は、1つではなく複数のファイルが必要になることがあります。
その場合、手動ですべてのファイルを1つに連結するのではなく、次のコマンドを実行します。

    jhipster jdl my_file1.jdl my_file2.jdl

JDLのインポート中にエンティティを再生成したくない場合は `--json-only` フラグを使用して
エンティティ作成部分をスキップし、`.jhipster`フォルダ内のファイルのみを作成します。

    jhipster jdl ./my-jdl-file.jdl --json-only

デフォルトでは`jdl`は変更されたエンティティのみを再生成します。すべてのエンティティを再生成したい場合は
`--force`フラグを渡します。
これにより、エンティティファイルに対するローカルの変更がすべて上書きされることに注意してください。

    jhipster jdl ./my-jdl-file.jdl --force

JDLをプロジェクトで使用する場合は、次のようにして追加できます。

- NPM: `npm install jhipster-core --save`
- Yarn: `yarn add jhipster-core`

ローカルにインストールされ、`package.json`に書き込まれます。

---

### インラインJDLの使用

コンテンツを生成するもう1つの方法は、次のようにCLIでJDLコードを渡すことです。
`jhipster jdl --inline "application { config { baseName jhipster, applicationType microservice } }"`.

このコンテンツの生成方法は、エンティティを生成する場合に特に便利です。

---

<<<<<<< HEAD
ここでは、コンテンツを生成するさまざまな方法を理解するために、小さなJDLコンテンツから始めます。
構文については他のセクションで説明しますが、ここでは生成に焦点を当てます。
=======
For now, we'll start with small JDL content to get to know the various ways to generate content.
The focus here is on the generation of code. Explanation about the syntax is made in other sections.
>>>>>>> upstream/main

ここでは、基本的なコンテンツを使用します。

```jdl
application {
  config {
    baseName jhipster
    applicationType microservice
  }
}
```

これは、"jhipster"という名前の非常に基本的なマイクロサービスアプリケーションです。このサンプルから
アプリケーションを生成するさまざまな方法について説明します。

この小さなサンプルで、アプリケーションをゼロから作成できることがわかります。

---

## リモートJDLファイルを使用する

URLは `jdl` コマンドを使用します。次のように、ファイル名の代わりにURLを渡してください。

```
jhipster jdl https://my-site.com/my.jdl


jhipster jdl https://gist.githubusercontent.com/user/id/raw/id/myapp.jdl
```

ファイル名を指定するだけで、[JDLサンプルリポジトリ](https://github.com/jhipster/jdl-samples)からリモートJDLファイルを取得もできます。URLは自動的に解決されます。

```
jhipster jdl default.jdl
```

## アプリケーションの生成

前の例で見たように、アプリケーションの生成は非常に簡単です。前の例を見てみましょう。
さらに要素を追加します。

```jdl
application {
  config {
    baseName jhipster
    applicationType microservice
    serverPort 4242
    buildTool gradle
  }
}
```

内訳を見てみましょう。

- `application`は、アプリケーションであることの宣言を示すキーワードです。
- `config`は、設定を宣言したいことを示します。
  - 後で説明しますが、アプリケーションでエンティティの宣言もできます。
- `baseName`、`applicationType`などは、アプリケーションを微調整するためのキーワードです。

これが、JDLを使用してアプリケーションを作成する方法です。
サポートされているすべてのアプリケーションのオプションを確認するには、[このページ](/jdl/applications)に移動します。

---

## エンティティの生成

エンティティの生成はそれほど簡単ではありません。
専用の[エンティティページ](/jdl/entities-fields)にアクセスして、エンティティで実行できる操作の詳細を確認できます。

### 基本的なエンティティの生成

```jdl
entity A
```

このエンティティにはフィールドがなく、明示的なテーブル名もありません（ただしJHipsterはエンティティの
名前からテーブル名を設定します）。
これは、エンティティを宣言する最も簡単な方法です。

この形式は次と同じです。

```jdl
entity A(a) {}
```

テーブル名と中括弧が追加されました。
デフォルトでは、JHipsterは指定されたエンティティ名に基づいてテーブル名を生成します。

中括弧は、フィールドを宣言するときに必要です。

### コメントの追加

エンティティにコメントを追加する方法は、次のとおりです。

```jdl
/**
 * This is a simple entity
 */
entity A
```

バックエンドがJavaの場合は、Javadocコメントが追加されます。

### アプリケーション内のエンティティ

アプリケーション内の一部のエンティティのみを生成するには、`entities`キーワードを使用できます。

```jdl
application {
  config {}
  entities A, B
}

application {
  config {}
  entities C
}

entity A
entity B
entity C
```

これはマイクロサービスアーキテクチャでは特に有用です。

---

## フィールドの生成

フィールドは、エンティティに対してボディを指定することによって、エンティティで宣言されます。

```jdl
entity MyEntity {
  name String
  closed Boolean
}
```

これらの2つの型以外にもあります。[エンティティとフィールドのページ](/jdl/entities-fields)で確認してください。

### コメントと検証の追加

エンティティにコメントを追加したのと同じ方法で、フィールドにコメントを追加できます。

```jdl
entity MyEntity {
  /** My field */
  name String required minlength(2) maxlength(50)
}
```

検証はフィールド型によって異なり、[エンティティとフィールドのページ](/jdl/entities-fields)にも詳細が記載されています。

---

## 列挙型

列挙型は、固定値を持つ型です。

```jdl
enum Type {
  A,
  B(b)
}

entity E {
  name Type
}
```

列挙型の値がオプションであることに注意してください。

ただ一つの`required`の検証を持っています。

enumの詳細については、専用の[enumページ](/jdl/enums)を確認できます。

---

## リレーションシップの追加

エンティティ間のリレーションシップも利用可能であり、`relationship`キーワードで宣言されます。

```jdl
entity A
entity B

relationship OneToOne {
  A{a} to B{b}
}
```

次のように表示されます。

- `OneToOne`はリレーションシップの型です。
- `OneToMany`、`ManyToMany`、`ManyToOne`もあります。
- リレーションシップの元と先（`A`から`B`へ）を宣言します。
- 各エンティティに注入されたフィールド（`B`内に`a`、`A`内に`b`）（訳注：であれば`A{a} to B{b}`は`A{b} to B{a}`の誤植かもしれないです）も宣言します。
- これは、リレーションシップが双方向であることを意味します。

リレーションシップの詳細については、[専用ページ](/managing_relationships)を参照してください。

### 単一方向または双方向のリレーションシップ

モデルの設計方法によっては、双方向のリレーションシップではなく単方向のリレーションシップが必要になる場合があります。
これは、次のように注入フィールドを指定しないことで実現されます。

```jdl
relationship OneToOne {
  A{a} to B
}
```

指定しないことも可能で、少なくとも1つ（ソース側）はデフォルトで注入されます。

```jdl
relationship OneToOne {
  A to B
}
```

### リレーションシップのコメントと検証

リレーションシップにはコメントと検証もあります（`required`1つだけ）。

```jdl
relationship OneToOne {
  A{a} to B{b required}
}
```

この例では、次のことがわかります。

- `required`は、リレーションシップの片側が必須かどうかを指定します。
- この1対1のリレーションシップでは、0..1ではなく、一方がnilでないことが必要です。

リレーションシップの詳細については、専用の[リレーションシップページ](/jdl/relationships)にアクセスしてください。

---

## オプション

CLIでエンティティにオプションを適用できるのと同じ方法で、JDLでも適用できます。

```jdl
entity A
entity B
entity C

readOnly A
dto * with mapstruct
service * with serviceImpl
paginate A, B with pager
```

ここでは興味深いことが起きています。

- `dto`, `paginate`, `service`はバイナリオプションです。エンティティリストと値が必要です。
  - `with`はオプション値を指定するために使用されます。
  - `*`はオプションがすべてのエンティティに適用されることを意味することに注意してください。
- `readOnly`は単項オプションです。つまり、このようなオプションはエンティティリストのみを取り扱います。

エンティティリストを宣言する方法は複数あります。

- 1つずつ列挙：`A, B, C`
- すべてを選択：`*`または`all`
  - エンティティを除外する例外を設定可能：`service * with serviceImpl except A, B`

### アノテーション

アノテーションはオプションを宣言するもう1つの方法です。前の例を書き直しましょう。

```jdl
@readOnly
@dto(mapstruct)
@service(serviceImpl)
@paginate(pager)
entity A

@dto(mapstruct)
@service(serviceImpl)
@paginate(pager)
entity B

@dto(mapstruct)
@service(serviceImpl)
entity C
```

JavaまたはTypescriptと同様に、アノテーションはエンティティのオプションである「デコレータ」です。

この例と前の例は同等であり、同じコードを生成できます。

オプションの詳細については、[オプションページ](/jdl/options)を参照してください。

---

## デプロイ

最後に、JHipsterと互換性のある`deployment`キーワードを使用して、JDLファイルからデプロイ環境の生成もできます。
v5.7以降になります。

```jdl
deployment {
  deploymentType docker-compose
  appsFolders [foo, bar]
  dockerRepositoryName "yourDockerLoginName"
}
```

_1つまたは複数のデプロイをインポートするためにJHipsterアプリケーションフォルダにいる必要はありません。_

デプロイについては、[デプロイのページ](/jdl/deployments)で説明されています。

JHipsterのデプロイは、他のすべてのプロパティはデフォルト値を持つような構成があり、前述の宣言にすると
デプロイにおいてデフォルト値が使用されます（特に選択がなかった場合も同様）。
結果としてデプロイ環境は次のようになります。

- deploymentType: `docker-compose`
- appsFolders: `foo, bar`
- dockerRepositoryName: `yourDockerLoginName`
- serviceDiscoveryType: `eureka`
- gatewayType: `SpringCloudGateway`
- directoryPath: `../`
- etc.

ここで、カスタムオプションが必要な場合は、次のようになります。

```jdl
deployment {
  deploymentType kubernetes
  appsFolders [store, invoice, notification, product]
  dockerRepositoryName "yourDockerLoginName"
  serviceDiscoveryType no
  istio autoInjection
  kubernetesServiceType Ingress
  kubernetesNamespace jhipster
  ingressDomain "jhipster.192.168.99.100.nip.io"
}
```

これらのオプションは、JDLで利用可能なもののサンプルにすぎません。
オプションの完全なリストは、[デプロイのページ](/jdl/deployments)にあります。

---

## 定数

JDLは数値定数をサポートしています。
次に例を示します。

```jdl
DEFAULT_MIN_LENGTH = 1
DEFAULT_MAX_LENGTH = 42
DEFAULT_MIN_BYTES = 20
DEFAULT_MAX_BYTES = 40
DEFAULT_MIN = 0
DEFAULT_MAX = 41

entity A {
  name String minlength(DEFAULT_MIN_LENGTH) maxlength(DEFAULT_MAX_LENGTH)
  content TextBlob required
  count Integer min(DEFAULT_MIN) max(DEFAULT_MAX)
}
```

---

## JDLファイルへのエクスポート

アプリケーションにすでにエンティティがあり、JDLファイルが必要な場合も、心配する必要はありません！　サブジェネレータがそれをしてくれるので、
スクラッチから作成する必要はありません。

アプリケーションのルートフォルダで`jhipster export-jdl <ファイル名>`を実行すると、すべてのアプリケーション、エンティティ、
リレーションシップ、オプションが、単一のJDLファイルへエクスポートされます。

注意：サブジェネレータにファイル名の指定はできません。エクスポートされたJDLファイルには、アプリケーションの名前にちなんだ名前になります。
例えば、もしアプリケーションの名前が'mySuperApp'なら、JDLファイルは`mySuperApp.jdl`になります。
