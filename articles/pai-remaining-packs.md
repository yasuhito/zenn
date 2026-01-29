---
title: "PAI その他のパック：StatusLine, BrightData, Recon, CreateCLI など"
emoji: "📦"
type: "tech"
topics: ["ai", "claude", "pai", "tools"]
published: true
---

## はじめに

[PAI](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure) には 23 のパックがある。主要なものは個別記事で解説したので、ここでは残りのパックをまとめて紹介する。

## Infrastructure

### pai-statusline

ターミナルにステータス情報を表示：

```
── │ PAI STATUSLINE │ ───────────────────────────
LOC: Tokyo │ 14:30 │ 18°C Clear
ENV: CC: 1.0.17 │ PAI: v2.3 │ Skills: 65
──────────────────────────────────────────────────
◉ CONTEXT: ⛁⛁⛁⛁⛁⛁⛁⛁⛁⛁ 23% (47k/200k)
◈ PWD: .claude │ Branch: main │ Mod: 3
✦ "The only way to do great work..."
```

表示内容：
- コンテキスト使用量
- Git ステータス
- メモリ統計
- 学習スコア（スパークライン）
- 名言

## Scraping

### pai-brightdata-skill

4段階フォールバック・スクレイピング：

```
Tier 1: WebFetch（シンプル）
    ↓ ブロックされたら
Tier 2: Curl + Chrome ヘッダー
    ↓ まだダメなら
Tier 3: Playwright（フルブラウザ）
    ↓ CAPTCHA が出たら
Tier 4: Bright Data（プロ用プロキシ）
```

ほとんどの URL は Tier 1-2 で取得できる。Bright Data は最終手段。

## Security

### pai-recon-skill

セキュリティ偵察スキル：

- **PassiveRecon** - 安全な情報収集（DNS, WHOIS, 証明書）
- **DomainRecon** - ドメイン全体のマッピング
- **IpRecon** - IP アドレス調査
- **NetblockRecon** - CIDR 範囲スキャン
- **BountyPrograms** - バグバウンティプログラム発見

### pai-privateinvestigator-skill

倫理的な人探し：

- 昔の友人を見つける
- 疎遠になった人との再接続
- 公開情報のみ使用

[OSINT Skill](https://zenn.dev/yasuhito/articles/pai-osint-skill) と似ているが、こちらは「再接続」に特化。

### pai-annualreports-skill

**570 以上** のセキュリティ年次レポートを検索・分析：

- CrowdStrike, Microsoft, IBM, Mandiant, Verizon
- 脅威トレンドの調査
- 業界横断的な分析

```
User: "2025 年のランサムウェアトレンドを調べて"
AI: 複数のセキュリティベンダーレポートから情報を統合
```

## Development

### pai-createcli-skill

TypeScript CLI を自動生成：

```
User: "この API を叩く CLI を作って"
AI: bun + TypeScript で完全な CLI を生成
    - ヘルプテキスト
    - エラーハンドリング
    - 型安全
```

3 つのティア：
1. 手動パース（シンプル）
2. Commander.js（標準）
3. oclif（本格的）

### pai-createskill-skill

PAI スキルを作成・検証：

- 新しいスキルの雛形生成
- 既存スキルの構造検証
- TitleCase 命名規則の強制
- ディレクトリ構造のチェック

### pai-system-skill

システムメンテナンス：

- **IntegrityCheck** - 壊れた参照を発見・修復
- **DocumentSession** - セッションを記録
- **SecretScanning** - シークレットの漏洩チェック
- **GitPush** - 安全な git push
- **WorkContextRecall** - 過去の作業を検索

## まとめ

PAI の 23 パックをすべて紹介した。

| カテゴリ | パック数 |
|---------|---------|
| Infrastructure | 5 |
| Research | 1 |
| Agents | 1 |
| Analysis | 4 |
| Security | 4 |
| Automation | 2 |
| Development | 3 |
| Life OS | 2 |
| Voice | 1 |

必要なものだけインストールして使える。[AI インストール](https://zenn.dev/yasuhito/articles/pai-ai-installation-system)で簡単にセットアップ可能。

## 関連記事

- [PAI 概要](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure)
- [THE ALGORITHM](https://zenn.dev/yasuhito/articles/pai-the-algorithm)
- [Hook System](https://zenn.dev/yasuhito/articles/pai-hook-system)
- [Core Install](https://zenn.dev/yasuhito/articles/pai-core-install)
- [Research Skill](https://zenn.dev/yasuhito/articles/pai-research-skill)
- [Agents Skill](https://zenn.dev/yasuhito/articles/pai-agents-skill)

## 参考リンク

- [PAI GitHub](https://github.com/danielmiessler/PAI)
- [PAI Packs](https://github.com/danielmiessler/PAI/tree/main/Packs)
