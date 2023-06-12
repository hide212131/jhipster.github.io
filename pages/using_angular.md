---
layout: default
title: Angularの使用
permalink: /using-angular/
sitemap:
    priority: 0.7
    lastmod: 2015-01-29T23:41:00-00:00
---

# <i class="fa fa-html5"></i> Angularの使用

## ツール

AngularはJavaScriptの代わりにTypeScriptを使用しており、その結果、効率的に動作するためには特定のツールが必要になります。Angular 2+アプリケーションの[開発]({{ site.url }}/development/)ワークフローは次のとおりです。`yarn`ではなく`npm`を使用することをお勧めします。

1. アプリケーションを生成すると、ファイルが作成され、生成の最後に`npm install`タスクがトリガーされます。
2. `npm install`が完了すると、`package.json`の`postInstall`スクリプトが呼び出され、このステップによって`webapp:build`タスクがトリガーされます。
3. これで、選択されたビルドツール（MavenまたはGradle）に基づいて、すべてのファイルが生成され、`target`または`build`フォルダ内の`www`フォルダにコンパイルされたはずです。
4. 次に、`./mvnw`または`./gradlew`を実行してアプリケーションサーバーを起動すると、[localhost:8080](localhost:8080)で使用可能となるはずです。これは、上記の手順でコンパイルされたクライアント側のコードも提供します。
5. 新しいターミナルで`npm start`または`yarn start`を実行して、BrowserSyncを使ってWebpack dev-serverを起動します。これにより、TypeScriptコードのコンパイルとブラウザの自動リロードが行われます。

`npm start`または`yarn start`を実行せずにクライアント側のコードに変更を加え始めた場合、変更はコンパイルされないので何も反映されません。そのため、変更後に`npm run webapp:build`を手動で実行するか、`npm start`や`yarn start`を実行する必要があります。

起動時に`./mvnw -Pdev,webapp`のような`webapp`プロファイルを渡すことで、Mavenに`webapp:dev`タスクを強制的に実行させることもできます。

**注**フロントエンドが変更された場合、Gradleは自動的に`dev`プロファイルでwebpackコンパイルを実行します（起動時のみ、ライブリロードには`npm start`または`yarn start`を使用してください）。

その他の使用可能なyarn/npmコマンドは、プロジェクトの`package.json`ファイルの`scripts`セクションにあります。

- ブラウザでコードを操作するには、[Angular DevTools](https://angular.io/guide/devtools)を使用することをお勧めします。Angular DevToolsは、Angularアプリケーションのデバッグとプロファイリング機能を提供するブラウザ拡張です（**注**Angular DevToolsはAngular v12以降をサポートしています）。

## プロジェクト構造

JHipsterのクライアントコードは`src/main/webapp`にあり、[Angularスタイルガイド](https://angular.io/guide/styleguide)に厳密に従っています。私たちのアプリケーション構造、ファイル名、TypeScriptの規則について質問がある場合は、まずこのガイドを読んでください。

このスタイルガイドはAngularチームによって承認されており、すべてのAngularプロジェクトが従うべきベストプラクティスを提供しています。

Angularルートの場合は、URLが明確で一貫性のあるものになるように、dash caseの命名規則に従います。
エンティティを生成すると、この規則に従ってルート名、ルートURL、およびREST APIエンドポイントURLが生成され、必要に応じてエンティティ名が自動的に複数化されます。

プロジェクトの主な構成は次のとおりです。

    webapp
<<<<<<< HEAD
    ├── app                               - アプリケーション
    │   ├── account                       - ユーザー・アカウント管理UI
    │   ├── admin                         - 管理UI
    │   ├── blocks                        - 設定やインターセプターなどの共通のビルディング・ブロック
    │   ├── entities                      - 生成されたエンティティ（詳細は以下を参照）
    │   ├── home                          - ホームページ
    │   ├── layouts                       - ナビゲーションバーやエラーページなどの一般的なページレイアウト
    │   ├── shared                        - 認証や国際化などの一般的なサービス
    │   ├── app.main.ts                   - メインアプリケーションクラス
    │   ├── app.module.ts                 - アプリケーションモジュールの設定
    │   ├── app-routing.module.ts         - メインアプリケーションルータ
    ├── content                           - 静的コンテンツ
    │   ├── css                           - CSSスタイルシート
    │   ├── images                        - 画像
    ├── i18n                              - 翻訳ファイル
    ├── scss                              - オプションを選択すると、ここにSassスタイルシートファイルが表示されます
    ├── swagger-ui                        - Swagger UIフロントエンド
    ├── 404.html                          - 404ページ
    ├── favicon.ico                       - お気に入りアイコン
    ├── index.html                        - 索引ページ
    ├── robots.txt                        - ボットおよびWebクローラの構成
=======
    ├── app                               - Your application
    │   ├── account                       - User account management UI
    │   ├── admin                         - Administration UI
    │   ├── config                        - Some utilities files
    │   ├── core                          - Common building blocks like configuration and interceptors
    │   ├── entities                      - Generated entities (more information below)
    │   ├── home                          - Home page
    │   ├── layouts                       - Common page layouts like navigation bar and error pages
    │       ├── main                      - Main page
    │           ├── main.component.ts     - Main application class
    │   ├── login                         - Login page
    │   ├── shared                        - Common services like authentication and internationalization
    │   ├── app.module.ts                 - Application modules configuration
    │   ├── app-routing.module.ts         - Main application router
    ├── content                           - Static content
    │   ├── css                           - CSS stylesheets
    │   ├── images                        - Images
    │   ├── scss                          - Sass style sheet files will be here if you choose the option
    ├── i18n                              - Translation files
    ├── swagger-ui                        - Swagger UI front-end
    ├── 404.html                          - 404 page
    ├── favicon.ico                       - Fav icon
    ├── index.html                        - Index page
    ├── robots.txt                        - Configuration for bots and Web crawlers
>>>>>>> upstream/main

[エンティティサブジェネレータ]({{ site.url }}/creating-an-entity/)を使用して`Foo`という名前の新しいエンティティを作成すると、`src/main/webapp`の下に次のフロントエンドファイルが生成されます。

    webapp
    ├── app
    │   ├── entities
    │       ├── foo                                    - FooエンティティのCRUDフロントエンド
    │           ├── foo.component.html                 - リストページのHTMLビュー
    │           ├── foo.component.ts                   - リストページのコントローラ
    │           ├── foo.model.ts                       - Fooエンティティを表すモデル
    │           ├── foo.module.ts                      - FooエンティティのAngularモジュール
    │           ├── foo.route.ts                       - Angularルータの設定
    │           ├── foo.service.ts                     - Foo RESTリソースにアクセスするサービス
    │           ├── foo-delete-dialog.component.html   - Fooを削除するためのHTMLビュー
    │           ├── foo-delete-dialog.component.ts     - Fooを削除するためのコントローラ
    │           ├── foo-detail.component.html          - Fooを表示するためのHTMLビュー
    │           ├── foo-detail.component.ts            - コントローラまたはFooの表示
    │           ├── foo-dialog.component.html          - Fooを編集するためのHTMLビュー
    │           ├── foo-dialog.component.ts            - Fooを編集するためのコントローラ
    │           ├── foo-popup.service.ts               - 作成/更新ダイアログのポップアップを処理するサービス
    │           ├── index.ts                           - すべてをエクスポートするためのバレル
    ├── i18n                                           - 翻訳ファイル
    │   ├── en                                         - 英訳
    │   │   ├── foo.json                               - Foo名、フィールド、...の英語翻訳
    │   ├── fr                                         - フランス語翻訳
    │   │   ├── foo.json                               - Foo名、フィールド、...のフランス語翻訳

デフォルトの言語変換は、アプリケーションの生成時に選択した内容に基づいて行われることに注意してください。'en'および'fr'は、ここではデモンストレーションのためにのみ示されています。

## 認証

JHipsterは[Angularルータ](https://angular.io/docs/ts/latest/guide/router.html)を使用して、クライアントアプリケーションのさまざまな部分を整理します。

各ステートごとに、必要な権限がステートのデータにリストされています。権限リストが空の場合は、ステートに匿名でアクセスできることを意味します。

権限はサーバー側でもクラス`AuthoritiesConstants.java`で定義されており、論理的にはクライアント側とサーバー側の権限は同じでなければなりません。

次の例では、'sessions'ステートは、`ROLE_USER`権限を持つ認証済みユーザーによってのみアクセスされるように設計されています。

    export const sessionsRoute: Route = {
        path: 'sessions',
        component: SessionsComponent,
        data: {
            authorities: ['ROLE_USER'],
            pageTitle: 'global.menu.account.sessions'
        },
        canActivate: [UserRouteAccessService]
    };

これらの権限がルータで定義されると、`jhiHasAnyAuthority`ディレクティブを通して、引数のタイプに応じた2種類のバリエーションで使用できます。

- 単一の文字列の場合、ユーザーが必要な権限を持っている場合にのみ、ディレクティブはHTMLコンポーネントを表示します。
- 文字列の配列の場合、ユーザーがリストされた権限のいずれかを持っている場合、ディレクティブはHTMLコンポーネントを表示します。

たとえば、次のテキストは、`ROLE_ADMIN`権限を持つユーザーにのみ表示されます。

    <h1 *jhiHasAnyAuthority="'ROLE_ADMIN'">Hello, admin user</h1>

たとえば、次のテキストは、`ROLE_ADMIN`または`ROLE_USER`権限のいずれかを持つユーザーにのみ表示されます。

    <h1 *jhiHasAnyAuthority="['ROLE_ADMIN', 'ROLE_USER']">Hello, dear user</h1>

*注意* これらのディレクティブは、クライアント側のHTMLコンポーネントを表示または非表示にするだけであり、サーバー側でもコードを保護する必要があります！

## ng-jhipsterライブラリ

ng-jhipsterライブラリはフリーでOSSであり、[https://github.com/jhipster/ng-jhipster](https://github.com/jhipster/ng-jhipster)から入手できます。

ng-jhipsterライブラリには、Angular 2+アプリケーションで使用されるユーティリティ関数と共通コンポーネントが含まれています。次のようなものがあります。

- 検証ディレクティブ
- 国際化コンポーネント
- 大文字化、順序付け、単語の切り詰めなどの一般的に使用されるパイプ
- Base64、日付、およびページ区切り処理サービス
- 通知システム（下記参照）

### 通知システム

JHipsterは、サーバー側からクライアント側にイベントを送信するためにカスタム通知システムを使用し、生成されたアプリケーション全体で使用できるi18n対応の`JhiAlertComponent`および`JhiAlertErrorComponent`コンポーネントを備えています。

デフォルトでは、JHipsterはHTTP応答からエラーがキャッチされたときにエラー通知を表示します。

カスタムの通知またはアラートを表示するには、コントローラ、ディレクティブ、またはサービスに`AlertService`を挿入した後、次のメソッドを使用します。

短縮メソッドである`success`、`info`、`warning`、`error`のデフォルトのタイムアウトは5秒で、設定は可能です。

    this.alerts.push(
        this.alertService.addAlert(
            {
                type: 'danger',
                msg: 'このボタンを押すべきではありません。',
                timeout: 5000,
                toast: false,
                scoped: true
            },
            this.alerts
        )
    );

## Angular CLIの使用

Angular CLIは、JHipsterアプリケーションの構築とテストに使用されます。
ただし、BrowserSync、ESLint（Angular CLIは現在もTSLint上にあります）の追加、JSON変換ファイルのマージ、ビルドが完了または失敗したときの通知の追加により、開発者のエクスペリエンスを向上させるために、カスタムWebpack設定ファイルを追加しました。

### 概要

[Angular CLI](https://cli.angular.io/)は、Angularアプリケーションを開発、構築、維持するためのツールです。JHipsterはAngular CLI設定ファイルを生成し、Angular CLIワークフローはJHipsterと連携します。

この統合は、アプリケーションルートフォルダに`angular.json`ファイルを生成し、その依存関係を`package.json`ファイルに追加することによって行われます。

### 使用方法

```bash
ng help
```

### ビルド

フロントエンドの構築には`ng build`を使用できますが、`npm start`、`npm run build`などの提供されているNPMスクリプトを使用することをお勧めします。[開発での使用のドキュメント]({{ site.url }}/development/)および[プロダクション環境での使用のドキュメント]({{ site.url }}/production/)を確認してください。

### コンポーネント、ディレクティブ、パイプ、およびサービスの生成

`ng generate`（または`ng g`）コマンドを使用して、Angularコンポーネントを生成できます。

```bash
ng generate component my-new-component
ng g component my-new-component # 別名の使用

# コンポーネントは相対パスの生成をサポート
# src/app/feature/に移動して実行
ng g component new-cmp
# コンポーネントはsrc/app/feature/new-cmpに生成されます
# ただし以下を実行すると
ng g component ../newer-cmp
# コンポーネントはsrc/app/newer-cmpに生成されます
```
以下に、使用可能なすべてのブループリントの表を示します。

Scaffold  | Usage
---       | ---
[Component](https://github.com/angular/angular-cli/wiki/generate-component) | `ng g component my-new-component`
[Directive](https://github.com/angular/angular-cli/wiki/generate-directive) | `ng g directive my-new-directive`
[Pipe](https://github.com/angular/angular-cli/wiki/generate-pipe)           | `ng g pipe my-new-pipe`
[Service](https://github.com/angular/angular-cli/wiki/generate-service)     | `ng g service my-new-service`
[Class](https://github.com/angular/angular-cli/wiki/generate-class)         | `ng g class my-new-class`
[Guard](https://github.com/angular/angular-cli/wiki/generate-guard)         | `ng g guard my-new-guard`
[Interface](https://github.com/angular/angular-cli/wiki/generate-interface) | `ng g interface my-new-interface`
[Enum](https://github.com/angular/angular-cli/wiki/generate-enum)           | `ng g enum my-new-enum`
[Module](https://github.com/angular/angular-cli/wiki/generate-module)       | `ng g module my-module`


### テスト

JHipsterアプリケーションでの一貫性のためのテストは`npm`コマンドを通じて利用できます。

```bash
npm test
```

### i18n

JHipsterは、翻訳のために`ngx-translate`依存関係を使用しています。Angular CLI i18nは、JHipsterと互換性のないデフォルトのAngular i18nサポートに基づいています。

### サーバの実行

Angular CLIを使用してアプリケーションを開発する場合は、専用のコマンドを使用してサーバを直接実行できます。

```bash
ng serve
```

### まとめ

Angular CLIの詳細については、公式Webサイト[https://cli.angular.io/](https://cli.angular.io/)を参照してください。
