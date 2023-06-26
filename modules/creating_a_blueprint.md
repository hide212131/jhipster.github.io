---
layout: default
title: Blueprintの作成
permalink: /modules/creating-a-blueprint/
redirect_from:
  - /creating_a_blueprint.html
  - /modules/creating_a_blueprint.html
sitemap:
    priority: 0.7
    lastmod: 2015-12-05T18:40:00-00:00
---

# <i class="fa fa-cube"></i> Blueprintの作成

JHipsterのBlueprintは、特定のJHipsterサブジェネレータの機能を拡張するために、そのサブジェネレータから[構成](http://yeoman.io/authoring/composability.html)されたYeomanジェネレータです。Blueprintは、サブジェネレータの定義されたゲッターをオーバーライドし、独自のテンプレートと機能を提供できます。

JHipsterのBlueprintは、[JHipsterマーケットプレイス](/modules/marketplace/)に`jhipster-blueprint`というラベルで記載されています。

これにより、JHipsterの特定の部分（例えば、クライアントサイドテンプレートのみ）をオーバーライドできるサードパーティのBlueprintを作成できます。

## Blueprintの使用

Blueprintを使用するには、次のコマンドを実行します。

```bash
jhipster --blueprints <blueprint name>
```

## Blueprintの生成

組み込みの`generate-blueprint`ジェネレータを使用して、Blueprintを作成することをお勧めします。

```bash
mkdir my-blueprint && cd my-blueprint

jhipster generate-blueprint
```
JHipsterのBlueprintは、依存関係として`generator-jhipster`を持つ必要があり、それをオーバーライドするために適切なサブジェネレータをインポートする必要があります。

```javascript
import chalk from 'chalk';
import ClientGenerator from 'generator-jhipster/generators/client';
<<<<<<< HEAD
import {
  INITIALIZING_PRIORITY,
  // その他の優先順位は簡潔にするため省略しています
} from 'generator-jhipster/priorities';
=======
>>>>>>> upstream/main

export default class extends ClientGenerator {
  constructor(args, opts, features) {
    super(args, opts, features);

    if (this.options.help) return;

    if (!this.options.jhipsterContext) {
      throw new Error(`This is a JHipster blueprint and should be used only like ${chalk.yellow('jhipster --blueprints myBlueprint')}`);
    }
  }

  get [ClientGenerator.INITIALIZING]() {
    return {
      // async preInitializingTemplateTask() {},
      ...super._initializing(),
      // async postInitializingTemplateTask() {},
    };
  }

  // その他の優先順位は簡潔にするため省略しています
}
```

## Local Blueprint

Local Blueprintは、プロジェクトの`.blueprint`ディレクトリ内に実装されます。デフォルトで検出され、使用されます。

Kickstartで[Blueprintの生成](#generating-the-blueprint)を使用するか、次のコマンドを実行します。

```
jhipster generate-blueprint --local-blueprint --sub-generators app --all-priorities
```

複数のサブジェネレータは、複数の機能を編成するのに便利です。LocalのBlueprintはスコープが限られている（アプリケーション）ため、アプリケーション（クライアントとサーバ）をカスタマイズするには、メインのサブジェネレータ1つで十分でしょう。

## 開発

### 優先順位

JHipsterから優先順位をカスタマイズする方法はいくつかあります。

1) JHipsterに優先順位を処理させます。Blueprintは何も上書きしません。

```javascript
    get [Generator.INITIALIZING]() {
        return super.initializing;
    }
```

2) 優先順位全体を上書きします。これは、Blueprintが優先順位を制御する場合です。

```javascript
  get [Generator.INITIALIZING]() {
    return {
      myCustomInitPriorityStep() {
        // ここですべての作業を行います。
      },
      myAnotherCustomInitPriorityStep(){
        // ここですべての作業を行います。
      }
    };
  }
```

3) 優先順位を部分的にオーバーライドします。これは、BlueprintがJHipsterから優先順位を取得し、それをカスタマイズする場合となります。

```javascript
    get [Generator.INITIALIZING]() {
        return {
            ...super._initializing(),
            displayLogo() {
                // JHipsterの初期化優先度からdisplayLogoメソッドをオーバーライドします。
            },
            myCustomInitPriorityStep() {
                // ここですべての作業を行います。
            },
        };
    }
```

4) 優先順位を装飾します。これは、JHipsterからの優先順位の前または後に、Blueprintがカスタムステップを実行する場合です。

これは、派生プロパティを生成するための優先順位付けで使用されるプロパティをカスタマイズするのに便利です。

```javascript
    // 親ステップの前または後、あるいはその両方でBlueprint・ステップを実行します。
    get initializing() {
        return {
            myCustomPreInitStep() {
                // JHipsterステップの「前に」行うべき処理です。
                // 例:nameCapitalized、nameLowercaseなどを生成する名前を設定します。
            }
            ...super._initializing(),
            myCustomPostInitStep() {
                // JHipsterステップの「後に」行う処理です。
            }
        };
    }
```
