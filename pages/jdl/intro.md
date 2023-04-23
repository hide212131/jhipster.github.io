---
layout: default
title: JHipsterドメイン言語
permalink: /jdl/intro
redirect_from:
  - /jdl/
sitemap:
    priority: 0.5
    lastmod: 2019-10-27T12:00:00-00:00
---

# <i class="fa fa-star"></i> JHipsterドメイン言語(JDL)

JDLはJHipster固有のドメイン言語であり、すべてのアプリケーション、デプロイメント、エンティティ、および
それらの関係を1つのファイル（または複数のファイル）に、使いやすい構文で記述します。

オンライン[JDL-Studio](https://start.jhipster.tech/jdl-studio/)または
[JHipster IDE](https://www.jhipster.tech/jhipster-ide/)プラグイン/拡張機能、以下の開発環境で、JDLファイルの作成とそのUMLのビジュアル化ができます。
  - [IntelliJ](https://plugins.jetbrains.com/plugin/19697-jhipster-jdl)
  - [Eclipse](https://marketplace.eclipse.org/content/jhipster-ide), 
  - [VS Code](https://marketplace.visualstudio.com/items?itemName=jhipster-ide.jdl)

JDLモデルのURLを作成しエクスポートまたは共有もできます。

JDLは、[エンティティ・サブジェネレータ]({{ site.url }}/creating-an-entity/)を使用する代わりとして使用できます。
これは推奨されるアプローチで、
ビジュアルツールを使用して[関係を管理する]({{ site.url }}/managing-relationships/)方がはるかに簡単です。
古典的なYeomanの質問と回答でエンティティを作成する方法よりも優れています。

JDLプロジェクトは[GitHubで利用可能](https://github.com/jhipster/jhipster-core/)であり、
JHipster（Apache 2.0ライセンス）なオープンソースプロジェクトです。JDL構文をパースするためのnodeライブラリとしても使用できます。

_[JHipsterドメイン言語](https://github.com/jhipster/jhipster-core/)が気に入ったら、
[JDL Studio](https://github.com/jhipster/jdl-studio/)または
[JHipster IDE](https://github.com/jhipster/jhipster-ide/)の
[GitHub](https://github.com/jhipster/)にスターを付けるのを忘れないでください - よろしくお願いします_！

---

JDLがまだわからない場合は、まず[JHipsterドメイン言語(JDL) - 入門](/jdl/getting-started)ページにアクセスすることをお勧めします。

ただし、すでにJDLに精通していて、JDLを使用して何ができるかを知っている場合は、詳細の参照もできます。
マニュアルは以下です。
  1. [アプリケーション](/jdl/applications)
  1. [エンティティとフィールド](/jdl/entities-fields)
  1. [列挙型](/jdl/enums)
  1. [リレーションシップ](/jdl/relationships)
  1. [オプション](/jdl/options)
  1. [デプロイメント](/jdl/deployments)
  1. [トラブルシューティング](/jdl/troubleshooting)

<<<<<<< HEAD
また、公式の[JDLサンプルリポジトリ](https://github.com/jhipster/jdl-samples)でも内容チェックできますし、
あなた方よりサンプルをご提案いただくこともできます！
=======
You can also check the official [JDL sample repository](https://github.com/jhipster/jdl-samples) and propose examples 
if you want!

---

In case you already have existing databases and would like to create the JDL representation then you can use
this project [SQL to JDL](https://github.com/Blackdread/sql-to-jdl) which will help you get started quickly.
>>>>>>> upstream/main
