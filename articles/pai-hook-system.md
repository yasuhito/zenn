---
title: "PAI Hook System：Claude Code のイベントを自動化する基盤"
emoji: "🔌"
type: "tech"
topics: ["ai", "claude", "pai", "automation"]
published: true
---

## Hook System とは

[PAI](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure) の基盤となるパックが「Hook System」だ。Claude Code が動作中に発生するイベントをキャッチして、自動で処理を実行する仕組み。

他のすべての PAI パックは、この Hook System の上に構築されている。

## なぜ Hook が必要か

Claude Code は動作中にさまざまなイベントを発火する：

| イベント | タイミング |
|---------|-----------|
| **SessionStart** | 新しいセッション開始時 |
| **UserPromptSubmit** | ユーザーがメッセージ送信時 |
| **PreToolUse** | ツール実行前 |
| **PostToolUse** | ツール実行後 |
| **Stop** | エージェントの応答完了時 |
| **SubagentStop** | サブエージェント完了時 |

デフォルトでは、これらのイベントは何も起こさない。Hook System を入れることで、各イベントに自動処理を紐づけられる。

## できること

### 1. セキュリティ検証（PreToolUse）

危険なコマンドをツール実行前にブロック：

```typescript
// rm -rf / のような危険なコマンドを検出してブロック
if (command.includes('rm -rf')) {
  return { blocked: true, reason: "危険なコマンドです" };
}
```

### 2. コンテキスト自動読み込み（SessionStart）

セッション開始時に、ユーザーの好みや過去の作業を自動で読み込む：

```typescript
// TECHSTACKPREFERENCES.md を読み込んで Claude に渡す
const prefs = await loadUserPreferences();
injectContext(prefs);
```

### 3. 学習キャプチャ（Stop）

応答完了時に、学んだことを自動で記録：

```typescript
// 完了したタスクから学びを抽出して保存
const learnings = extractLearnings(response);
await saveLearnings(learnings);
```

### 4. 音声通知（Stop）

タスク完了を音声で知らせる：

```typescript
// ElevenLabs TTS で完了を読み上げ
await speak("タスクが完了しました");
```

## 含まれるフック（15個）

| フック | イベント | 機能 |
|--------|---------|------|
| SecurityValidator | PreToolUse | 危険コマンドをブロック |
| LoadContext | SessionStart | コンテキスト自動注入 |
| StartupGreeting | SessionStart | 音声で挨拶 |
| CheckVersion | SessionStart | PAI バージョン確認 |
| UpdateTabTitle | UserPromptSubmit | タブタイトル更新 |
| SetQuestionTab | UserPromptSubmit | 質問状態の管理 |
| ExplicitRatingCapture | UserPromptSubmit | 評価キャプチャ |
| FormatEnforcer | Stop | 出力フォーマット強制 |
| StopOrchestrator | Stop | 応答後の処理調整 |
| SessionSummary | Stop | セッション要約 |
| QuestionAnswered | Stop | 質問完了処理 |
| AutoWorkCreation | Stop | 作業記録の自動作成 |
| WorkCompletionLearning | Stop | 学習の自動抽出 |
| SentimentCapture | Stop | 感情分析 |
| AgentOutputCapture | SubagentStop | サブエージェント出力記録 |

## アーキテクチャ

```
$PAI_DIR/
├── hooks/                    # フック実装
│   ├── SecurityValidator.hook.ts
│   ├── LoadContext.hook.ts
│   ├── StartupGreeting.hook.ts
│   └── ...
└── lib/                      # 共有ライブラリ
    ├── hook-utils.ts
    ├── security-rules.ts
    └── ...
```

各フックは独立した TypeScript ファイル。Claude Code の設定で有効化すると、対応するイベント発火時に自動実行される。

## 使用例

### カスタムフックの作成

自分でフックを追加することもできる：

```typescript
// my-custom.hook.ts
export default {
  event: "Stop",
  async handler(context) {
    // 応答完了時に Slack 通知
    if (context.duration > 60000) {
      await notifySlack("長時間タスクが完了しました");
    }
  }
};
```

### フックの有効化

Claude Code の設定ファイルでフックを登録：

```json
{
  "hooks": {
    "Stop": ["~/.claude/hooks/my-custom.hook.ts"]
  }
}
```

## 他のパックとの関係

Hook System は PAI の基盤：

```
pai-hook-system（基盤）
    ↓
pai-core-install（コア機能）
    ↓
pai-voice-system, pai-observability-server, ...
    ↓
各種スキルパック
```

ほぼすべてのパックが Hook System に依存している。[PAI のインストール](https://zenn.dev/yasuhito/articles/pai-ai-installation-system)では、最初にこのパックを入れる。

## まとめ

Hook System は PAI のイベント駆動基盤だ：

- Claude Code のライフサイクルイベントをキャッチ
- セキュリティ、コンテキスト注入、学習、通知を自動化
- カスタムフックで拡張可能

地味だが、PAI の自動化を支える重要なパック。

## 参考リンク

- [PAI GitHub](https://github.com/danielmiessler/PAI)
- [pai-hook-system](https://github.com/danielmiessler/PAI/tree/main/Packs/pai-hook-system)
