---
title: "PAI Observability Server：マルチエージェント監視ダッシュボード"
emoji: "📊"
type: "tech"
topics: ["ai", "claude", "pai", "monitoring"]
published: false
---

## Observability Server とは

[PAI](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure) のリアルタイム監視パック。複数の AI エージェントが **今何をしているか** をダッシュボードで可視化する。

## なぜ監視が必要か

PAI では複数のエージェントが並列で動く。[Agents Skill](https://zenn.dev/yasuhito/articles/pai-agents-skill) で見たように、DETERMINED レベルでは最大 10 エージェントが同時稼働する。

問題は「誰が何をしているかわからない」こと。Observability Server があれば：
- 全エージェントの状態が一目でわかる
- どのタスクが進行中/完了/ブロックかわかる
- 問題があればすぐ気づける

## 機能

### WebSocket ストリーミング

イベントがリアルタイムでダッシュボードに反映：

```
[10:01:05] Engineer: ファイル編集開始
[10:01:07] QATester: テスト実行中
[10:01:10] Engineer: ファイル編集完了
[10:01:12] QATester: テスト成功
```

ポーリングではなく WebSocket なので、遅延なく状態がわかる。

### エージェント Swim Lanes

複数エージェントの活動を並べて比較：

```
┌─────────────┬─────────────┬─────────────┐
│  Engineer   │   QATester  │  Designer   │
├─────────────┼─────────────┼─────────────┤
│ ████████░░░ │ ██░░░░░░░░░ │ ░░░░░░░░░░░ │
│ (コーディング) │ (待機中)    │ (未開始)    │
└─────────────┴─────────────┴─────────────┘
```

### タスクトラッキング

バックグラウンドタスクの進捗を追跡：

```markdown
## アクティブタスク

| タスク | エージェント | 状態 | 経過 |
|--------|-------------|------|------|
| リファクタリング | Engineer | 🔄 進行中 | 2m 30s |
| テスト実行 | QATester | ⏳ 待機中 | - |
| UIレビュー | Designer | ✅ 完了 | 1m 45s |
```

### イベントタイムライン

すべての操作を時系列で表示：

```
10:00:00 [SessionStart] main セッション開始
10:00:05 [ToolUse] Read: src/index.ts
10:00:10 [ToolUse] Edit: src/index.ts
10:00:15 [SubagentStart] Engineer 起動
10:00:45 [SubagentStop] Engineer 完了
```

## アーキテクチャ

```
PAI Hooks → JSONL ログ → Server (Bun) → WebSocket → Dashboard (Vue)
```

### サーバー（Bun + TypeScript）

```
observability/apps/server/
├── src/
│   ├── index.ts        # HTTP + WebSocket サーバー
│   ├── file-ingest.ts  # JSONL ファイル監視
│   ├── task-watcher.ts # タスク監視
│   └── db.ts           # インメモリ DB
```

### クライアント（Vue 3 + Vite）

```
observability/apps/client/
├── src/
│   ├── App.vue         # メインダッシュボード
│   └── components/     # UI コンポーネント（15+）
```

## セットアップ

### 1. サーバー起動

```bash
./manage.sh start
```

### 2. ダッシュボードアクセス

```
http://localhost:8080
```

### 3. フック有効化

[Hook System](https://zenn.dev/yasuhito/articles/pai-hook-system) の AgentOutputCapture フックを有効化すると、自動でイベントがログに書き込まれる。

## Menu Bar アプリ（macOS）

macOS 用のネイティブメニューバーアプリも付属：

- サーバーの起動/停止
- アクティブエージェント数の表示
- ダッシュボードへのクイックアクセス

## まとめ

Observability Server は PAI の監視基盤：

- **WebSocket** でリアルタイム更新
- **Swim Lanes** で複数エージェントを比較
- **タスクトラッキング** で進捗把握
- **イベントタイムライン** で全操作を記録

「AI が何をしているか」を見える化する。

## 参考リンク

- [PAI GitHub](https://github.com/danielmiessler/PAI)
- [pai-observability-server](https://github.com/danielmiessler/PAI/tree/main/Packs/pai-observability-server)
