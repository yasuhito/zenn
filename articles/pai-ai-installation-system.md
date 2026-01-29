---
title: "PAI の AI インストール：エージェントがエージェントをセットアップする仕組み"
emoji: "🤖"
type: "tech"
topics: ["ai", "claude", "automation", "pai"]
published: true
---

## はじめに

[PAI (Personal AI Infrastructure)](https://github.com/danielmiessler/PAI) は、Daniel Miessler が公開している「パーソナル AI スタック」だ。Claude Code などの AI エージェントに、記憶・スキル・フックなどの機能を追加できる。PAI の概要については[前回の記事](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure)で解説した。

PAI の面白い特徴の一つが **「AI によるインストール」**。人間がコマンドを一つずつ打つのではなく、AI エージェントに「このパックをインストールして」と頼むと、AI が対話形式でインストールを進めてくれる。

この記事では、PAI の AI インストールの仕組みを解説する。

## なぜ AI インストールなのか

従来のインストール方法には問題があった。

### 問題 1: トークン制限

大きなパックは単一ファイルだと **25,000 トークンを超える** ことがある。マークダウン内にすべてのコードを埋め込むと、AI が一度に処理できない量になってしまう。

### 問題 2: AI による「改善」

マークダウン内のコードブロックを AI に読ませると、親切心から **コードを「簡略化」してしまう** ことがある。268行の完全な実装が、50行の「同等品」になってしまうのだ。

### 問題 3: 環境依存

ユーザーの環境はそれぞれ異なる。既存のインストールがあるか、依存関係は満たされているか、などを考慮する必要がある。

## PAI の解決策：ウィザード形式 AI インストール

PAI は **5フェーズのウィザード形式** でこれらの問題を解決している。

```
Phase 1: システム分析
    ↓
Phase 2: ユーザーへの質問
    ↓
Phase 3: バックアップ（必要なら）
    ↓
Phase 4: インストール実行
    ↓
Phase 5: 検証
```

## パックの構造

まず、PAI パックの構造を見てみよう：

```
pack-name/
├── README.md      # 概要、アーキテクチャ
├── INSTALL.md     # AI 用インストール手順（ウィザード形式）
├── VERIFY.md      # 検証チェックリスト
└── src/           # 実際のコード
    ├── hooks/     # フック実装
    ├── tools/     # CLI ツール
    ├── skills/    # スキル定義
    └── config/    # 設定ファイル
```

**重要なポイント:** `src/` ディレクトリには **完全なコードファイル** が入っている。マークダウンからコードを抽出するのではなく、実ファイルをそのままコピーする。これにより AI による「簡略化」を防げる。

## Phase 1: システム分析

AI はまずユーザーの環境をチェックする。`INSTALL.md` には、実行すべきコマンドが書かれている：

```bash
PAI_DIR="${PAI_DIR:-$HOME/.claude}"

# 依存パックが入ってるか確認
if [ -f "$PAI_DIR/skills/CORE/SKILL.md" ]; then
  echo "OK pai-core-install is installed"
else
  echo "ERROR pai-core-install NOT installed - REQUIRED!"
fi

# Bun がインストールされてるか確認
if command -v bun &> /dev/null; then
  echo "OK Bun is installed: $(bun --version)"
else
  echo "ERROR Bun not installed - REQUIRED!"
fi

# 既存インストールがあるか確認
if [ -d "$PAI_DIR/skills/THEALGORITHM" ]; then
  echo "WARNING Existing skill found"
else
  echo "OK No existing skill (clean install)"
fi
```

AI はこれらの結果をユーザーにわかりやすく報告する：

> 「あなたの環境を確認しました：
> - pai-core-install: ✅ インストール済み
> - Bun: ✅ v1.0.0
> - 既存の ALGORITHM スキル: ❌ なし（クリーンインストール）」

**重要:** 必須の依存関係がない場合は、ここでインストールを中断する。

## Phase 2: ユーザーへの質問

Claude Code の **AskUserQuestion** ツールを使って、対話形式で選択肢を提示する：

```json
{
  "header": "Conflict",
  "question": "既存のスキルが見つかりました。どうしますか？",
  "multiSelect": false,
  "options": [
    {
      "label": "バックアップして置き換え（推奨）",
      "description": "タイムスタンプ付きバックアップを作成してから新版をインストール"
    },
    {
      "label": "バックアップなしで置き換え",
      "description": "既存ファイルを上書き"
    },
    {
      "label": "キャンセル",
      "description": "インストールを中止"
    }
  ]
}
```

**条件分岐がポイント:** 問題がなければ質問をスキップする。すべての質問を毎回聞くのではなく、必要な質問だけを行う。

### 質問の設計ガイドライン

PAI では質問の設計にもルールがある：

| ヘッダー | 用途 |
|---------|------|
| Runtime | 依存関係についての質問 |
| Conflict | 既存インストールとの競合 |
| Features | オプション機能の選択 |
| Install | 最終確認 |

## Phase 3: バックアップ

ユーザーが「バックアップして置き換え」を選んだ場合のみ実行：

```bash
BACKUP_DIR="$PAI_DIR/Backups/skill-$(date +%Y%m%d-%H%M%S)"
mkdir -p "$BACKUP_DIR"
cp -r "$PAI_DIR/skills/THEALGORITHM" "$BACKUP_DIR/"
echo "Backup created at: $BACKUP_DIR"
```

タイムスタンプ付きなので、複数回インストールしても過去のバックアップが上書きされない。

## Phase 4: インストール実行

Claude Code の **TodoWrite** ツールで進捗を可視化しながら実行する：

```json
{
  "todos": [
    {"content": "ディレクトリ構造を作成", "status": "completed", "activeForm": "作成中..."},
    {"content": "スキルファイルをコピー", "status": "in_progress", "activeForm": "コピー中..."},
    {"content": "依存関係をインストール", "status": "pending"},
    {"content": "検証を実行", "status": "pending"}
  ]
}
```

ユーザーは何が行われているかリアルタイムで確認できる。

実際の作業は以下のようになる：

```bash
PACK_DIR="$(pwd)"
PAI_DIR="${PAI_DIR:-$HOME/.claude}"

# 1. ディレクトリ作成
mkdir -p "$PAI_DIR/skills/THEALGORITHM/"{Data,Tools,Phases,Reference}

# 2. ファイルコピー（src/ から直接コピー）
cp -r "$PACK_DIR/src/skills/THEALGORITHM/"* "$PAI_DIR/skills/THEALGORITHM/"

# 3. 依存関係インストール
cd "$PAI_DIR/skills/THEALGORITHM/Tools"
bun add yaml
```

**ポイント:** `src/` ディレクトリから **そのままコピー** する。AI がコードを解釈・変換する必要がないため、完全なコードが確実にインストールされる。

## Phase 5: 検証

`VERIFY.md` に書かれたチェックリストを実行する：

```bash
PAI_DIR="${PAI_DIR:-$HOME/.claude}"

echo "=== Verification ==="

# ファイル存在確認
[ -f "$PAI_DIR/skills/THEALGORITHM/SKILL.md" ] && echo "OK SKILL.md" || echo "ERROR SKILL.md missing"

# ツール存在確認
for tool in EffortClassifier ISCManager AlgorithmDisplay; do
  [ -f "$PAI_DIR/skills/THEALGORITHM/Tools/${tool}.ts" ] && echo "OK ${tool}.ts" || echo "ERROR ${tool}.ts missing"
done

# ツール実行テスト
bun run "$PAI_DIR/skills/THEALGORITHM/Tools/EffortClassifier.ts" --help && echo "OK Tool executes" || echo "ERROR Tool execution failed"

echo "=== Verification Complete ==="
```

すべてのチェックが通れば成功メッセージ、失敗があればトラブルシューティングの案内を表示する。

### 成功時のメッセージ例

```
PAI Algorithm Skill v2.3.0 installed successfully!

What's available:
- EffortClassifier: タスクの複雑さを自動分類
- ISCManager: 理想状態の基準を管理
- AlgorithmDisplay: 実行状況を視覚表示

Usage: 'Run the algorithm on [task]' で起動
```

## 設計原則：End-to-End 完全性

PAI パックには重要な原則がある：**End-to-End で完全であること**。

### ダメな例

```
「完全なサーバー実装はこのパックの範囲外です」
「お好みの TTS プロバイダーを実装してください」
「スケルトンパターンを参考にしてください」
```

### 良い例

データフローのすべてのコンポーネントがパックに含まれている：

```
Hook → Server → API → Output
  ✅      ✅      ✅     ✅
  すべてパックに含まれている
```

これにより、「インストールしたけど動かない」という事態を防げる。

## AI インストールの利点まとめ

| 利点 | 説明 |
|------|------|
| **環境適応** | ユーザーの環境に合わせて分岐 |
| **対話的** | 必要な質問だけを行う |
| **進捗可視化** | TodoWrite で何が起きているかわかる |
| **コード完全性** | src/ からの直接コピーで簡略化を防止 |
| **検証付き** | インストール後に自動チェック |

## おわりに

PAI の AI インストールは「AI がツールをセットアップする」という新しいパラダイムを示している。

- 人間は「これをインストールして」と頼むだけ
- AI が環境を分析し、対話形式で進め、検証まで行う

この仕組みは、AI エージェントのスキルやプラグインを配布する際の参考になるだろう。

## 関連記事

https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure

## 参考リンク

- [PAI GitHub](https://github.com/danielmiessler/PAI)
- [PAI Packs README](https://github.com/danielmiessler/PAI/tree/main/Packs)
- [Building Effective Agents (Anthropic)](https://anthropic.com/research/building-effective-agents)
