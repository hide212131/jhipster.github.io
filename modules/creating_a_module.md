---
layout: default
title: Moduleの作成
permalink: /modules/creating-a-module/
redirect_from:
  - /creating_a_module.html
  - /modules/creating_a_module.html
sitemap:
    priority: 0.7
    lastmod: 2015-12-05T18:40:00-00:00
---

# <i class="fa fa-cube"></i> スタンドアロンのBlueprint(別名Module)の作成

JHipster v7.9.0では、ModuleサポートがBlueprintサポートにマージされました。同じルールが適用されます。

Blueprintを作成する前に、必ず[Blueprinの基本](/modules/extending-and-customizing/#-blueprint-basics)を読んでください。

## 例

[JHipster Ionic](https://github.com/jhipster/generator-jhipster-ionic)はModuleからBlueprintに変換されました。

## 移行

- モジュールアプリとエンティティのジェネレータ（存在する場合）の名前をapp-moduleやentity-moduleのような別の名前に変更します。

```sh
mv generators/app generators/app-module
mv generators/entity generators/entity-module
```

- generator-jhipster generatorに一致する他のすべてのジェネレータの名前を変更します（そうでない場合は、Blueprintと呼ばれます）。
その後、参照を更新します。
- カスタムCLI(`cli/cli.mjs`)を追加します。

```javascript
#!/usr/bin/env node

import { runJHipster, done, logger } from 'generator-jhipster/cli';
import { readFile } from 'fs/promises';
import { fileURLToPath } from 'url';
import { dirname, basename } from 'path';

// 名前空間として使用するパッケージ名を取得します。
// Blueprintのエイリアスを許可します。
const packagePath = dirname(dirname(fileURLToPath(import.meta.url)));
const packageFolderName = basename(packagePath);

(async () => {
  const { version, bin } = JSON.parse(await readFile(new URL('../package.json', import.meta.url)));
  const executableName = Object.keys(bin)[0];

  runJHipster({
    executableName,
    executableVersion: version,
    defaultCommand: 'app-module', // `yo`コマンドを置き換えるためのエントリポイントとして使用されるジェネレータ
    blueprints: {
      [packageFolderName]: version,
    },
    lookups: [{ packagePaths: [packagePath], lookups: ['generators'] }],
  }).catch(done);
})();

process.on('unhandledRejection', up => {
  logger.error('Unhandled promise rejection at:');
  logger.fatal(up);
});
```

- `package.json`にcliを追加します。

```json
{
  "bin": {
    "jhipster-module": "cli/cli.mjs"
  }
}
```

### フック

JHipster 8では、フックのサポートが削除されます。移行の場合は、次のside-by-sideブループリントを使用してフックをシミュレートできます。

- 次のジェネレータを追加します。

以下は、事後のアプリケーションフック用のアプリケーションジェネレータ（`generators/app/index.mjs`）です。

```javascript
import chalk from 'chalk';
import GeneratorBaseApplication from 'generator-jhipster/generators/base-application';
import { INSTALL_PRIORITY } from 'generator-jhipster/priorities';

export default class extends GeneratorBaseApplication {
  constructor(args, opts, features) {
    super(args, opts, features);

    if (this.options.help) return;

    if (!this.options.jhipsterContext) {
      throw new Error(`This is a JHipster blueprint and should be used only like ${chalk.yellow('jhipster --blueprints myBlueprint')}`);
    }

    this.sbsBlueprint = true;
  }

  get [GeneratorBaseApplication.INSTALL]() {
    return {
      async afterRunHook() {
        await this.composeWithJHipster(`my-blueprint:app-module`, {
          appConfig: this.configOptions,
        });
      },
    };
  }
}
```

以下は、事後のエンティティフック用のエンティティジェネレータ（`generators/entity/index.mjs`）です。

```javascript
import chalk from 'chalk';
import GeneratorBaseApplication from 'generator-jhipster/generators/base-application';

export default class extends GeneratorBaseApplication {
  constructor(args, opts, features) {
    super(args, opts, features);

    if (this.options.help) return;

    if (!this.options.jhipsterContext) {
      throw new Error(`This is a JHipster blueprint and should be used only like ${chalk.yellow('jhipster --blueprints myBlueprint')}`);
    }

    this.sbsBlueprint = true;
  }

  get [GeneratorBaseApplication.INSTALL]() {
    return {
      async afterRunHook() {
        await this.composeWithJHipster(`my-blueprint:entity-module`, {
          entityConfig: this.options.jhipsterContext.context,
        });
      },
    };
  }
}
```
