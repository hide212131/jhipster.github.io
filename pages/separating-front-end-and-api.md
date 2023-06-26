---
layout: default
title: フロントエンドとAPIサーバの分離
permalink: /separating-front-end-and-api/
sitemap:
    priority: 0.7
    lastmod: 2021-03-08T12:00:00-00:00
---

# <i class="fa fa-unlink"></i> フロントエンドとAPIサーバの分離

## はじめに

JHipsterは「フルスタック」開発ツールであり、その目標はフロントエンドコード（Angular/React）とバックエンドコード（Spring Boot）で効率的に作業できるようにすることです。

ただし、フロントエンド・コードとバックエンド・コードを分離することは一般的な要件です。これは通常、これらのコードが異なるチームによって開発され、異なるライフサイクルを持つためです。

**注意** これはデフォルトのJHipsterの作業方法ではありません。複雑ではなく、うまく機能しますが、高度なトピックです。JHipsterを使い始める場合は、私たちの標準的な作業方法を使用することから始めることをお勧めします。

## フロントエンド・アプリケーションまたはバックエンド・アプリケーションのみを生成

JHipsterバックエンドまたはJHipsterフロントエンドアプリケーションのみを生成するように選択できます。生成時には、[アプリケーション生成ドキュメント]({{ site.url }}/creating-an-app/)に記載されているフラグを選択するだけです。

- `jhipster --skip-client`はバックエンドアプリケーションのみを生成します（これは通常JHipsterマイクロサービスがそうなります）
- `jhipster --skip-server [options]`はフロントエンドアプリケーションのみを生成します（例：`jhipster --skip-server --db=sql --auth=jwt`）

これは、マイクロサービス（いずれにしてもフロントエンドがない）やゲートウェイ（Spring Cloud Gatewayサービスが有効になっているモノリス）ではあまり意味がないため、モノリスでのみうまく機能するはずです。

## ディレクトリのレイアウト

JHipsterは、標準のMavenディレクトリ・レイアウトを使用します。バックエンドで作業する場合は、[Maven標準ディレクトリ・レイアウト・ドキュメント](https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html)を参照できます。

フロントエンドで作業する場合は、次の2つのディレクトリを知っておく必要があります。

- `src/main/webapp`は、クライアントアプリケーションが開発される場所です
- `target/classes/static`は、クライアントアプリケーションがパッケージ化される場所です

フロントエンドとバックエンドで別々のチームが作業している場合は、2つのソリューションがあります。

- 両方のチームが同じプロジェクトで作業します。ディレクトリが分離されているので、チーム間の競合はあまりありません。さらにクリーンにするために、両方のチームが別々のブランチで作業もできます。
- フロントエンドコードを特定のGitプロジェクトに格納し、Gitサブモジュールとしてメインのバックエンドプロジェクトにインポートできます。これには、クライアントサイドのビルドスクリプトを移動する必要があります。

## HTTPリクエストのルーティングとキャッシング

フロントエンドとバックエンドが分離されると、問題はHTTPリクエストの処理方法になります。

<<<<<<< HEAD
- すべてのAPI呼び出しは`/api`プレフィックスを使用します。Angularを使用している場合、`webpack.common.js`設定で定義された特定の`SERVER_API_URL`定数もあり、このプレフィックスを強化できます。例えば、`"http://api.jhipster.tech:8081/"`をバックエンドAPIサーバとして使用できます（これを行う場合は、以下のCORSに関するドキュメントをお読みください）。
- `/index.html`は、ブラウザまたはサーバによってキャッシュされるべきではありません。
- （フロントエンドの）静的アセットである`/app`（クライアント側アプリケーションを含む）および`/content`（画像やCSSなどの静的コンテンツを含む）を提供する`/`の呼び出しは、これらのアセットがハッシュ化されるため、プロダクション環境でキャッシュされるべきです。
- 存在しないルートへのコールは、リクエストを`index.html`に転送する必要があります。これは通常、`ClientForwardController`を介してバックエンドで処理されます。クライアントを個別にデプロイする場合、これを設定する必要があります。いくつかの例については、[Angular](https://angular.io/guide/deployment#server-configuration)または[React](https://facebook.github.io/create-react-app/docs/deployment)のドキュメントを参照してください。
=======
- All API calls will use a `/api` prefix. If you are using Angular, there is also a specific `SERVER_API_URL` constant, defined in the `webpack.common.js` configuration, that can enrich this prefix. For example, you can use `"http://api.jhipster.tech:8081/"` as a back-end API server (If you do this, please read our documentation on CORS below).
- `/index.html` should not be cached by the browser or server.
- Calls to `/` that serve static assets (from the front-end) `/app` (which contains the client-side application) and `/content` (which contains the static content, like images and CSS) should be cached in production, as those assets are hashed.
- Calls to `/i18n` can be cached but files itsefs not are hashed, url query string are used
- Calls to a non-existent route should forward the request to `index.html`. This is normally handled in the backend through `ClientForwardController`. When deploying the client separately, this needs to be configured.  See the [Angular](https://angular.io/guide/deployment#server-configuration) or [React](https://facebook.github.io/create-react-app/docs/deployment) documentation for several examples.
>>>>>>> upstream/main

# BrowserSyncの使用

`dev`モードでは、JHipsterはフロントエンドアプリケーションのホットリロードにBrowserSyncを使用します。BrowserSyncには、`/api`からバックエンドサーバ（デフォルトでは`http://127.0.0.1:8080`）にリクエストをルーティングするプロキシ（[ここにドキュメントがあります](https://www.browsersync.io/docs/options#option-proxy)）があります。

これは`dev`モードでのみ動作しますが、フロントエンドから異なるAPIサーバにアクセスするための非常に強力な方法です。

## CORSの使用

CORS ([Cross-origin request sharing](https://wikipedia.org/wiki/Cross-origin_resource_sharing))を使用すると、プロキシを設定せずに、同じフロントエンドを使用して異なるバックエンドサーバにアクセスできます。

これは使いやすいソリューションですが、本番環境では安全性が低くなる可能性があります。

JHipsterは、すぐに利用できるCORS構成を提供します。

- CORSは、[JHipster共通アプリケーションプロパティ]({{ site.url }}/common-application-properties/)で定義されているように、`JHipster.cors`プロパティを使用して構成できます。
- モノリスとゲートウェイでは、`dev`モードでデフォルトで有効になっています。マイクロサービスでは、ゲートウェイを介してアクセスすることになっているため、デフォルトでは無効になっています。
- セキュリティ上の理由から、`prod`モードではデフォルトでオフになっています。

## NGinxの使用

フロントエンドコードとバックエンドコードを分離するもう1つの解決策は、プロキシサーバを使用することです。これは本番環境では非常に一般的であり、一部のチームは開発でもこの手法を使用しています。

この設定は特定のユースケースに応じて変更されるため、これをジェネレータで自動化はできません。実際に動作する設定を次に示します。

`src/main/docker/nginx.yml`でDocker構成ファイルを作成します。

    version: '2'
    services:
      nginx:
        image: nginx:1.15-alpine
        volumes:
        - ./../../../target/static:/usr/share/nginx/html
        - ./nginx/site.conf:/etc/nginx/conf.d/default.conf
        ports:
        - "8000:80"

このDockerイメージは、`target/static`から静的アセットを読み取るNGinxサーバを設定します。これは、JHipsterフロントエンドアプリケーションがデフォルトで生成される場所です。プロダクション環境では、このための特定のフォルダがあると思われます。

また、`./nginx/site.conf`ファイルも読み込みます。これはNGinx固有の設定ファイルです。
### lambdaの設定
以下に`site.conf`の例を示します。

    server {
        listen 80;
        index index.html;
        server_name localhost;
        error_log  /var/log/nginx/error.log;

        root /usr/share/nginx/html;

        location /api {
            proxy_pass http://api.jhipster.tech:8081/api;
        }
        location /management {
            proxy_pass http://api.jhipster.tech:8081/management;
        }
        location /swagger-resources {
            proxy_pass http://api.jhipster.tech:8081/swagger-resources;
        }        
        location /v2 {
           proxy_pass http://api.jhipster.tech:8081/v2;
        }
        location /auth {
           proxy_pass http://api.jhipster.tech:8081/auth;
        }
 
        location / {
            try_files $uri $uri/ /index.html;
        }
    }

この設定は、次のことを意味します。

- NGinxはポート`80`で動作します。
- フォルダ`/usr/share/nginx/html`の静的アセットを読み込みます。
- これは`/api`から`http://api.jhipster.tech:8081/api`へのプロキシとして動作します。
- 処理されない要求はすべて`index.html`に転送されます。

この構成は、特定のニーズに応じて調整する必要がありますが、ほとんどのアプリケーションにとって十分な出発点となるはずです。
