---
title: "PAI Core Install：パーソナル AI の「OS」となる基盤パック"
emoji: "🧠"
type: "tech"
topics: ["ai", "claude", "pai", "agent"]
published: true
---

## Core Install とは

[PAI](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure) の「オペレーティングシステム」となるパックが「Core Install」だ。スキルのルーティング、応答フォーマット、メモリシステム、セキュリティ設定など、PAI 全体の動作を定義する。

[Hook System](https://zenn.dev/yasuhito/articles/pai-hook-system) の次にインストールする、必須のパック。

## 何が含まれるか

### 1. SYSTEM ドキュメント（19ファイル）

PAI がどう動作するかを定義するドキュメント群：

| ファイル | 内容 |
|---------|------|
| ARCHITECTURE.md | システム全体の構造 |
| RESPONSEFORMAT.md | 応答のフォーマット規則 |
| DELEGATION.md | エージェント委任のルール |
| ROUTING.md | スキルへのルーティング規則 |
| SECURITY.md | セキュリティポリシー |
| ... | |

### 2. USER テンプレート

ユーザー固有の設定を置く場所：

```
CORE/USER/
├── TECHSTACKPREFERENCES.md  # 技術スタックの好み
├── CONTACTS.md              # 連絡先
├── DEFINITIONS.md           # 用語定義
└── TELOS/                   # 目標・信念（後述）
```

### 3. ワークフロー（4つ）

| ワークフロー | 機能 |
|-------------|------|
| Delegation | 他のエージェントへの委任 |
| SessionContinuity | セッション間の継続性 |
| ContextLoading | コンテキスト読み込み |
| MemoryRecall | 記憶の呼び出し |

### 4. CLI ツール（4つ）

| ツール | 機能 |
|--------|------|
| Inference.ts | 推論ヘルパー |
| SessionProgress.ts | セッション進捗表示 |
| ContextManager.ts | コンテキスト管理 |
| MemorySearch.ts | メモリ検索 |

## MEMORY システム

Core Install の重要な機能が MEMORY システム。PAI の「長期記憶」を管理する：

```
MEMORY/
├── Work/           # 作業記録
├── Learning/       # 学んだこと
├── Signals/        # パターン検出
└── Sessions/       # セッション履歴
```

[THE ALGORITHM](https://zenn.dev/yasuhito/articles/pai-the-algorithm) の ISC もここに保存される。

## 応答フォーマット

Core Install は PAI の応答フォーマットを標準化する：

```markdown
## 🎯 タスク: [タスク名]

**ステータス:** 🔄 進行中 | ✅ 完了 | ❌ ブロック

### 完了した項目
- [x] 項目1
- [x] 項目2

### 次のステップ
- [ ] 項目3
```

一貫したフォーマットにより：
- 何が起きているか一目でわかる
- 音声読み上げ（Voice System）と相性が良い
- パース可能で自動処理しやすい

## スキルルーティング

ユーザーのリクエストを適切なスキルに振り分ける：

```
"この記事を要約して" → Research Skill
"バグを直して" → THE ALGORITHM (Engineer)
"画像を作って" → Art Skill
"セキュリティをチェック" → Recon Skill
```

ルーティング規則は ROUTING.md で定義。各スキルの SKILL.md に書かれたトリガーと照合する。

## ユーザーコンテキスト

Core Install は「AI があなたを知っている」状態を作る：

### TECHSTACKPREFERENCES.md

```markdown
## 言語
- TypeScript > JavaScript
- Rust > C++

## フレームワーク
- React (Next.js)
- Bun > Node.js

## スタイル
- 関数型 > オブジェクト指向
- イミュータブル優先
```

これを読み込むことで、「TypeScript で書いて」といちいち言わなくても、好みに合ったコードが出てくる。

### CONTACTS.md

よく出てくる人物の情報を保持。「田中さんにメール」と言えば、メールアドレスを知っている。

## 他のパックとの関係

Core Install は PAI の中核：

```
pai-hook-system
    ↓
pai-core-install ← ほぼ全パックが依存
    ↓
├── pai-voice-system
├── pai-observability-server  
├── pai-algorithm-skill
├── pai-research-skill
└── ...
```

## まとめ

Core Install は PAI の「OS」：

- **SYSTEM ドキュメント** で動作を定義
- **USER テンプレート** でユーザー固有の設定
- **MEMORY システム** で長期記憶
- **応答フォーマット** を標準化
- **スキルルーティング** でリクエストを振り分け

Hook System と Core Install を入れれば、PAI の基盤が整う。あとは必要なスキルを追加していくだけだ。

## 参考リンク

- [PAI GitHub](https://github.com/danielmiessler/PAI)
- [pai-core-install](https://github.com/danielmiessler/PAI/tree/main/Packs/pai-core-install)
