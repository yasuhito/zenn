---
title: "PAI CreateCLI Skill：TypeScript CLI を 3 段階で自動生成"
emoji: "⌨️"
type: "tech"
topics: ["ai", "claude", "pai", "cli"]
published: true
---

## CreateCLI Skill とは

[PAI](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure) の CLI 生成パック。API やサービスを操作する **TypeScript CLI を自動生成** する。

## 3 つのティア

複雑さに応じて 3 段階の CLI を生成：

| ティア | ライブラリ | 用途 |
|--------|-----------|------|
| **Tier 1** | 手動パース | 数コマンド、シンプル |
| **Tier 2** | Commander.js | 中規模、サブコマンド |
| **Tier 3** | oclif | 大規模、プラグイン対応 |

## Tier 1: 手動パース

最もシンプル。依存ゼロ：

```typescript
// cli.ts
const args = process.argv.slice(2);
const command = args[0];

switch (command) {
  case 'list':
    await listItems();
    break;
  case 'get':
    await getItem(args[1]);
    break;
  default:
    console.log('Usage: cli [list|get <id>]');
}
```

**使いどころ:**
- 個人用ツール
- 数コマンドだけ
- 依存を増やしたくない

## Tier 2: Commander.js

標準的な CLI フレームワーク：

```typescript
import { program } from 'commander';

program
  .name('mycli')
  .version('1.0.0')
  .description('My awesome CLI');

program
  .command('list')
  .description('List all items')
  .option('-l, --limit <n>', 'Limit results', '10')
  .action(async (options) => {
    await listItems({ limit: parseInt(options.limit) });
  });

program
  .command('get <id>')
  .description('Get item by ID')
  .action(async (id) => {
    await getItem(id);
  });

program.parse();
```

**使いどころ:**
- チームで使うツール
- サブコマンドが 5-20 個
- オプションが多い

## Tier 3: oclif

Salesforce 製の本格 CLI フレームワーク：

```typescript
import { Command, Flags } from '@oclif/core';

export default class List extends Command {
  static description = 'List all items';

  static flags = {
    limit: Flags.integer({ char: 'l', default: 10 }),
    format: Flags.string({ options: ['json', 'table'] }),
  };

  async run() {
    const { flags } = await this.parse(List);
    const items = await listItems({ limit: flags.limit });
    this.log(formatOutput(items, flags.format));
  }
}
```

**使いどころ:**
- 公開ツール
- プラグインシステムが必要
- 複雑なコマンド体系

## 生成されるもの

### ファイル構成

```
mycli/
├── src/
│   ├── index.ts        # エントリポイント
│   ├── commands/       # コマンド実装
│   │   ├── list.ts
│   │   └── get.ts
│   └── lib/            # 共有ロジック
│       └── api.ts
├── package.json
├── tsconfig.json
└── README.md
```

### 自動生成される機能

| 機能 | Tier 1 | Tier 2 | Tier 3 |
|------|--------|--------|--------|
| ヘルプテキスト | 手動 | ✅ 自動 | ✅ 自動 |
| バージョン表示 | 手動 | ✅ 自動 | ✅ 自動 |
| オプション解析 | 手動 | ✅ 自動 | ✅ 自動 |
| 型安全 | ✅ | ✅ | ✅ |
| エラーハンドリング | 基本 | 基本 | 高度 |
| プラグイン | ❌ | ❌ | ✅ |

## 使用例

### API クライアント CLI

```
User: "GitHub API を叩く CLI を作って。issue の一覧、作成、クローズができるように"
```

AI が Tier 2 の CLI を生成：

```bash
gh-cli issues list --repo owner/repo
gh-cli issues create --repo owner/repo --title "Bug"
gh-cli issues close --repo owner/repo --number 123
```

### 内部ツール

```
User: "S3 バケットの中身を見る簡単な CLI"
```

AI が Tier 1 の CLI を生成。

## Bun 対応

生成される CLI は **Bun** で実行：

```bash
bun run src/index.ts list
```

Node.js でも動作するが、Bun 推奨。

## まとめ

CreateCLI Skill は PAI の CLI 生成：

- **3 段階のティア** で複雑さに対応
- **TypeScript** で型安全
- **完全な雛形** を生成（README 含む）
- **Bun 対応** で高速実行

「CLI を作って」と言うだけで、動く CLI が出てくる。

## 参考リンク

- [PAI GitHub](https://github.com/danielmiessler/PAI)
- [pai-createcli-skill](https://github.com/danielmiessler/PAI/tree/main/Packs/pai-createcli-skill)
- [Commander.js](https://github.com/tj/commander.js)
- [oclif](https://oclif.io/)
