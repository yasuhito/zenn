---
title: "PAI TELOS Skill：人生 OS で目標・信念・学びを管理"
emoji: "🎯"
type: "tech"
topics: ["ai", "claude", "pai", "lifeos"]
published: true
---

## TELOS Skill とは

[PAI](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure) の「人生 OS」パック。自分の **目標・信念・学び・戦略** を構造化し、AI があなたを深く理解できるようにする。

TELOS = Telic Evolution and Life Operating System

## なぜ人生を構造化するか

AI アシスタントは通常、セッションごとにリセットされる。「あなたが何を目指しているか」を知らない。

TELOS を使えば、AI は：
- あなたの長期目標を知っている
- 価値観に沿った提案ができる
- 過去の学びを活かせる

## 15 のファイル

TELOS は以下のファイルで構成される：

| ファイル | 内容 |
|---------|------|
| **MISSION.md** | 人生のミッション |
| **GOALS.md** | 長期・短期目標 |
| **BELIEFS.md** | 核心的な信念 |
| **MODELS.md** | 世界の見方、メンタルモデル |
| **STRATEGIES.md** | 目標達成の戦略 |
| **NARRATIVES.md** | 自分の物語 |
| **LEARNED.md** | 学んだこと |
| **CHALLENGES.md** | 現在の課題 |
| **IDEAS.md** | アイデアのストック |
| **PROJECTS.md** | 進行中のプロジェクト |
| **BOOKS.md** | 影響を受けた本 |
| **MOVIES.md** | 影響を受けた映画 |
| **PEOPLE.md** | 大切な人々 |
| **HABITS.md** | 習慣 |
| **REVIEWS.md** | 定期レビュー |

## 使用例

### BELIEFS.md

```markdown
## 核心的信念

### 仕事
- 価値を生み出すことが最重要
- 完璧より完了
- 学び続ける者が勝つ

### 人間関係
- 信頼は行動で築く
- 与えることが先

### 技術
- シンプルさは究極の洗練
- 自動化できることは自動化する
```

### GOALS.md

```markdown
## 2026 年の目標

### キャリア
- [ ] OSS プロジェクトをローンチ
- [ ] 技術ブログ 50 記事

### 健康
- [ ] 週 3 回運動
- [ ] 7 時間睡眠

### 学習
- [ ] Rust を実務で使えるレベルに
```

## AI との連携

TELOS ファイルは [Core Install](https://zenn.dev/yasuhito/articles/pai-core-install) でセッション開始時に読み込まれる：

```
SessionStart
    ↓
LoadContext Hook
    ↓
TELOS ファイルを読み込み
    ↓
AI が目標・信念を理解した状態で会話開始
```

## ワークフロー

### Update ワークフロー

TELOS ファイルを更新：

```
User: "今日学んだこと: X について"
AI: LEARNED.md に追記しました
```

### WriteReport ワークフロー

McKinsey スタイルのレポートを生成：

```
User: "今月の進捗をレポートにして"
AI: GOALS.md と PROJECTS.md から進捗レポートを生成
```

### InterviewExtraction ワークフロー

インタビュー内容から TELOS を更新：

```
User: "[インタビュー音声のテキスト]"
AI: 信念、学び、アイデアを抽出して各ファイルに追加
```

## 自動バックアップ

TELOS ファイルは更新のたびにタイムスタンプ付きバックアップが作成される。誤って削除しても復元可能。

## Dashboard

Next.js ベースのダッシュボードテンプレートも付属。TELOS データを視覚化できる：

- 目標の進捗状況
- 学びのタイムライン
- プロジェクトの状態

## まとめ

TELOS Skill は PAI の人生 OS：

- **15 ファイル** で人生を構造化
- **AI が目標・信念を理解** した上で会話
- **自動バックアップ** で安心
- **レポート生成** で振り返り

「AI があなたを知っている」状態を作る。

## 参考リンク

- [PAI GitHub](https://github.com/danielmiessler/PAI)
- [pai-telos-skill](https://github.com/danielmiessler/PAI/tree/main/Packs/pai-telos-skill)
