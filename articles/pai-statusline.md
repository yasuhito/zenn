---
title: "PAI StatusLine：ターミナルに AI の状態を常時表示"
emoji: "📊"
type: "tech"
topics: ["ai", "claude", "pai", "terminal"]
published: true
---

## StatusLine とは

[PAI](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure) のターミナル UI パック。Claude Code のセッション中、**コンテキスト使用量・Git 状態・メモリ統計** などをリアルタイムで表示する。

## 表示例

```
── │ PAI STATUSLINE │ ───────────────────────────
LOC: Tokyo │ 14:30 │ 18°C Clear
ENV: CC: 1.0.17 │ PAI: v2.3 │ Skills: 65
──────────────────────────────────────────────────
◉ CONTEXT: ⛁⛁⛁⛁⛁⛁⛁⛁⛁⛁ 23% (47k/200k)
◈ PWD: .claude │ Branch: main │ Mod: 3
✦ "The only way to do great work..."
```

## 表示項目

### 環境情報（上段）

| 項目 | 内容 |
|------|------|
| LOC | 現在地（IP ジオロケーション） |
| 時刻 | 現地時間 |
| 天気 | 気温と天候 |
| CC | Claude Code バージョン |
| PAI | PAI バージョン |
| Skills | 読み込み済みスキル数 |

### セッション情報（中段）

| 項目 | 内容 |
|------|------|
| CONTEXT | コンテキストウィンドウ使用率（バー表示） |
| PWD | 現在のディレクトリ |
| Branch | Git ブランチ |
| Mod | 変更ファイル数 |

### スパークライン

学習スコアの推移をスパークラインで表示。セッション中の「学び」の蓄積が可視化される。

### 名言

ランダムな名言を表示。気分転換に。

## なぜ必要か

Claude Code は長時間セッションでコンテキストが埋まっていく。StatusLine があれば：

- **コンテキスト残量** が一目でわかる
- **Git 状態** を常に意識できる
- **環境情報** を確認するためにコマンドを打たなくていい

## 設定

### 表示のカスタマイズ

```yaml
statusline:
  show_weather: true
  show_quote: true
  context_warning: 80  # 80% で警告色に
```

### 更新間隔

デフォルトは 30 秒ごとに更新。コンテキスト使用量は操作ごとに更新。

## Hook 連携

[Hook System](https://zenn.dev/yasuhito/articles/pai-hook-system) と連携し、以下のタイミングで更新：

- **SessionStart** - 初期表示
- **PostToolUse** - ツール実行後にコンテキスト更新
- **Stop** - 応答完了時に学習スコア更新

## まとめ

StatusLine は PAI のターミナル UI：

- **コンテキスト使用量** をバー表示
- **Git 状態** を常時表示
- **環境情報** を一目で確認
- **学習スコア** をスパークラインで可視化

AI との作業中、重要な情報を常に見える場所に。

## 参考リンク

- [PAI GitHub](https://github.com/danielmiessler/PAI)
- [pai-statusline](https://github.com/danielmiessler/PAI/tree/main/Packs/pai-statusline)
