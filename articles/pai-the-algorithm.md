---
title: "PAI THE ALGORITHM：期待を超える結果を出す 7 フェーズ実行エンジン"
emoji: "🎯"
type: "tech"
topics: ["ai", "claude", "pai", "methodology"]
published: true
---

## THE ALGORITHM とは

[PAI](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure) の核心となる実行エンジンが「THE ALGORITHM」だ。すべてのタスクを **「現在の状態」から「理想の状態」** へと導く、7 フェーズの科学的アプローチ。

目標は単なる「完了」ではない。**期待を超える「うわっ、すごい」な結果** を出すことだ。

## 核心コンセプト：ISC（Ideal State Criteria）

THE ALGORITHM の中心にあるのが **ISC（理想状態の基準）**。「何が達成されたら完璧なのか」を表にしたものだ。

| # | 理想の状態 | 出典 | 担当 | 状態 |
|---|-----------|------|------|------|
| 1 | 良いパターンを調査 | INFERRED | 🔬 perplexity | ⏳ PENDING |
| 2 | トグルが動作する | EXPLICIT | 🤖 engineer | 🔄 ACTIVE |
| 3 | テーマ状態が保存される | EXPLICIT | 🤖 engineer | ⏳ PENDING |
| 4 | TypeScript を使用 | INFERRED | — | ✅ DONE |
| 5 | テストが通る | IMPLICIT | ✅ qa_tester | ⏳ PENDING |

### 出典（Source）の 3 種類

| 種類 | 意味 | 例 |
|------|------|-----|
| **EXPLICIT** | ユーザーが直接言ったこと | 「ログアウトボタン追加して」 |
| **INFERRED** | 文脈から推測 | TypeScript を使う（設定から） |
| **IMPLICIT** | 当然の品質基準 | テストが通る、セキュリティ問題なし |

ISC は固定ではない。作業を進める中でリサーチ結果やエッジケースが見つかれば、行が追加されていく。

## 努力レベル（Effort Levels）

タスクの複雑さに応じて、使える「道具」が変わる。これが PAI の賢いところだ。

| レベル | いつ使う | モデル | 使える能力 |
|--------|----------|--------|-----------|
| **TRIVIAL** | 挨拶、簡単な質問 | - | アルゴリズムをスキップ |
| **QUICK** | 単純作業、タイポ修正 | haiku | Intern エージェント |
| **STANDARD** | 普通の開発作業 | sonnet | Engineer, QA, Designer, リサーチ |
| **THOROUGH** | 重要・複雑な作業 | sonnet | + Architect, Council（4人議論） |
| **DETERMINED** | 絶対成功させる | opus | + RedTeam（32人攻撃）、無限イテレーション |

### 検出キーワード

努力レベルは自動検出される：

- 「ちゃちゃっと」「さくっと」→ QUICK
- 「しっかり」「丁寧に」→ THOROUGH  
- 「なんとしても」「できるまで」→ DETERMINED

明示的に指定もできる：

```
algorithm effort DETERMINED: このバグを直して
```

## 7 つのフェーズ

```
OBSERVE → THINK → PLAN → BUILD → EXECUTE → VERIFY → LEARN
  👁️       🧠      📋      🔨       ⚡        ✅       📚
```

### 1. 👁️ OBSERVE（観察）

リクエストとユーザー文脈を理解し、ISC の行を **作成** する。

- リクエストを解析
- ユーザーの好み（技術スタック等）を読み込み
- EXPLICIT / INFERRED / IMPLICIT の行を作成

**不明確なリクエストの場合はインタビュー：**

1. 「完成したらどんな状態？」
2. 「誰がどう使う？」
3. 「友達に見せたくなるポイントは？」
4. 「似てる既存のものは？」
5. 「絶対やっちゃダメなことは？」

### 2. 🧠 THINK（思考）

漏れがないか確認し、ISC の行を **補完** する。

- 全行をレビュー
- 足りない要件を追加
- エッジケースを考慮

THOROUGH 以上では Council（4エージェント議論）や FirstPrinciples（第一原理思考）を使って深掘りする。

### 3. 📋 PLAN（計画）

ISC の行を **順序付け** する。

- 依存関係を特定
- 並列実行可能な行をマーク
- 実行順序を決定

### 4. 🔨 BUILD（構築）

各行をテスト可能な形に **洗練** する。

| Before | After |
|--------|-------|
| 「うまく動く」 | 「レスポンスが 200ms 以内」 |
| 「見た目がいい」 | 「デザインモックと一致」 |

### 5. ⚡ EXECUTE（実行）

実際に作業を行い、ISC のステータスを **更新** する。

能力に応じて適切なエージェントを起動：

```
Phase A: リサーチ（並列）
├─ 🔬 perplexity → Web 検索
├─ 🔬 gemini → 多角的視点
└─ 🔬 grok → 反対意見

Phase B: 思考
├─ 💡 deep thinking → 創造的解決策
└─ 🗣️ council → 4人議論

Phase C: 実装（並列可）
├─ 🤖 engineer → コーディング
└─ 🤖 designer → UI デザイン

Phase D: 検証
└─ ✅ browser → 画面確認
```

### 6. ✅ VERIFY（検証）

各行を成功基準と照らし合わせて **確認** する。

重要なのは、検証エージェントは常に **懐疑的（skeptical）** であること。実装した人と別のエージェントが「本当にできてる？」とチェックする。

| 結果 | 意味 |
|------|------|
| PASS | 基準通り |
| ADJUSTED | 許容範囲の変更あり |
| BLOCKED | 問題あり → イテレーションへ |

### 7. 📚 LEARN（学習）

結果を **出力** し、ユーザーの評価を待つ。

重要：**AI は自己評価しない**。ユーザーが評価する。これが PAI のメモリシステムにフィードバックされる。

## イテレーション

VERIFY で BLOCKED が出たら、問題に応じて戻る：

```
BLOCKED
  ├─ 理想状態が不明確 → THINK へ
  ├─ アプローチが間違い → PLAN へ  
  └─ 実装エラー → EXECUTE へ
```

イテレーション回数は努力レベルで変わる：

| レベル | 最大回数 |
|--------|---------|
| QUICK | 1回 |
| STANDARD | 2回 |
| THOROUGH | 3-5回 |
| DETERMINED | 成功するまで無制限 |

## Ralph Loop：成功するまで繰り返す

DETERMINED レベルで使える特殊モード。「テストが通るまでひたすら直す」ような場合に使う。

```bash
bun run RalphLoopExecutor.ts \
  --prompt "テストが全部通るまでバグを直す" \
  --completion-promise "All tests pass" \
  --max-iterations 15
```

Claude が終了しようとすると、フックが同じプロンプトを再投入。前回の作業結果を見て続きを実行する。成功条件を満たすまで繰り返す。

## 視覚的フィードバック

THE ALGORITHM は LCARS スタイル（スタートレック風）のステータス表示と音声通知を備えている。

```bash
# 開始（バナー表示 + 音声）
bun run AlgorithmDisplay.ts start THOROUGH -r "ログアウト機能を追加"

# フェーズ移行（表示更新 + 音声）
bun run AlgorithmDisplay.ts phase EXECUTE
```

何が起きているかがリアルタイムでわかる。

## まとめ

THE ALGORITHM の本質：

1. **努力レベル** でどれだけ本気でやるか決める
2. **ISC** で「理想の状態」を定義する
3. **7フェーズ** を順番に実行して理想に近づける
4. **検証** で本当に達成できたか確認
5. 問題があれば **イテレーション** で戻ってやり直す

単に「やった」で終わらせない。**期待を超える** ことを目指す。それが THE ALGORITHM だ。

## 参考リンク

- [PAI GitHub](https://github.com/danielmiessler/PAI)
- [pai-algorithm-skill](https://github.com/danielmiessler/PAI/tree/main/Packs/pai-algorithm-skill)
