---
title: "PAI CreateSkill Skill：PAI スキルの雛形生成と検証"
emoji: "🛠️"
type: "tech"
topics: ["ai", "claude", "pai", "development"]
published: true
---

## CreateSkill Skill とは

[PAI](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure) のスキル開発パック。新しい PAI スキルの **雛形生成** と **構造検証** を行う。

## PAI スキルの構造

PAI スキルは以下の構造を持つ：

```
MySkill/
├── SKILL.md              # スキル定義（必須）
├── Reference/            # 参考資料
│   └── *.md
├── Workflows/            # ワークフロー定義
│   └── *.md
├── CLI/                  # CLI ツール
│   └── *.ts
└── src/                  # 実装
    └── *.ts
```

## 雛形生成

### 基本的な使い方

```
User: "新しいスキル 'MyAwesomeSkill' を作って"
```

生成される `SKILL.md`:

```markdown
---
name: MyAwesomeSkill
version: 1.0.0
description: [説明をここに]
capabilities:
  - [能力1]
  - [能力2]
triggers:
  - [トリガーワード1]
  - [トリガーワード2]
---

# MyAwesomeSkill

## 概要

[スキルの概要]

## ワークフロー

### [ワークフロー名]

[ワークフローの説明]

## 使用例

```
User: "[例のリクエスト]"
```
```

### カスタマイズ指定

```
User: "画像生成スキルを作って。Midjourney と DALL-E に対応、プロンプト最適化機能付き"
```

AI が仕様に合わせた雛形を生成。

## 構造検証

既存のスキルが PAI の規約に従っているか検証：

```
User: "MySkill の構造を検証して"
```

### チェック項目

| 項目 | 説明 |
|------|------|
| **SKILL.md 存在** | 必須ファイルがあるか |
| **Front Matter** | YAML メタデータが正しいか |
| **命名規則** | TitleCase になっているか |
| **ディレクトリ構造** | 標準構造に従っているか |
| **参照整合性** | 壊れたリンクがないか |

### 検証結果例

```markdown
## 検証結果: MySkill

✅ SKILL.md 存在
✅ Front Matter 有効
⚠️ 命名規則: 'myWorkflow.md' → 'MyWorkflow.md' に変更推奨
✅ ディレクトリ構造
❌ 参照整合性: 'Reference/OldDoc.md' が存在しない

### 推奨アクション
1. 'Workflows/myWorkflow.md' を 'MyWorkflow.md' にリネーム
2. 'SKILL.md' 内の 'OldDoc.md' 参照を削除または修正
```

## 命名規則

PAI スキルは **TitleCase** を使用：

| 種類 | 例 |
|------|-----|
| スキル名 | `MyAwesomeSkill` |
| ワークフロー | `ProcessData.md` |
| CLI | `RunAnalysis.ts` |
| ディレクトリ | `Reference/`, `Workflows/` |

❌ `my_skill`, `mySkill`, `my-skill`
✅ `MySkill`

## SKILL.md の書き方

### 必須セクション

```markdown
---
name: SkillName
version: 1.0.0
description: 一行説明
capabilities:
  - 能力1
  - 能力2
triggers:
  - トリガー1
---

# SkillName

## 概要
[スキルが何をするか]

## ワークフロー
[利用可能なワークフロー]

## 使用例
[具体的な使い方]
```

### オプションセクション

- `## 設定` - カスタマイズ方法
- `## 依存関係` - 他のスキル、ツール
- `## 制限事項` - 既知の制限

## ワークフロー定義

ワークフローは `Workflows/` ディレクトリに配置：

```markdown
# ProcessData

## 概要
データを処理する

## ステップ
1. 入力を検証
2. データを変換
3. 結果を出力

## 入力
- `data`: 処理対象のデータ
- `format`: 出力フォーマット (json|csv)

## 出力
- 処理済みデータ
```

## [THE ALGORITHM](https://zenn.dev/yasuhito/articles/pai-the-algorithm) との統合

CreateSkill 自体も PAI スキルとして、THE ALGORITHM のルーティングで呼び出される：

```
User: "新しいスキルを作って"
    ↓
ルーティング: CreateSkill Skill
    ↓
雛形生成
```

## まとめ

CreateSkill Skill は PAI のスキル開発支援：

- **雛形生成** で新規スキルを素早く作成
- **構造検証** で規約準拠を確認
- **命名規則** の自動チェック
- **TitleCase** 強制で一貫性

PAI エコシステムを拡張するための土台。

## 参考リンク

- [PAI GitHub](https://github.com/danielmiessler/PAI)
- [pai-createskill-skill](https://github.com/danielmiessler/PAI/tree/main/Packs/pai-createskill-skill)
