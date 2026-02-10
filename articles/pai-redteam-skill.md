---
title: "PAI RedTeam Skill：32 エージェントでアイデアを徹底攻撃"
emoji: "🔴"
type: "tech"
topics: ["ai", "claude", "pai", "analysis"]
published: true
---

## RedTeam Skill とは

[PAI](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure) の敵対的分析システム。**32人のエージェント** がアイデアや計画の弱点を徹底的に攻撃する。

[Council](https://zenn.dev/yasuhito/articles/pai-council-skill) が「協調的に議論」なら、RedTeam は「敵対的に破壊」。

## 目的は「破壊」ではない

RedTeam の目的は単なる批判ではない。**根本的な欠陥** を見つけることだ。

> 構造全体を崩壊させる「一つの致命的欠陥」を見つける

表面的な問題をたくさん指摘するのではなく、「これが崩れたら全部ダメになる」という核心を探す。

## 32 エージェントの構成

| タイプ | 人数 | 視点 |
|--------|------|------|
| **Engineer** | 8 | 技術的・論理的厳密性 |
| **Architect** | 8 | 構造的・システム的問題 |
| **Pentester** | 8 | 攻撃者目線、悪用方法 |
| **Intern** | 8 | 素朴な疑問、初心者視点 |

各エージェントは異なる性格と攻撃角度を持つ。

## 出力：Steelman + Counter-Argument

RedTeam は2つのものを出力する：

### 1. Steelman（最強の擁護論）

まず、アイデアを **最も強い形で表現** する：

```markdown
## Steelman（8ポイント）

1. このアプローチは X の問題を根本解決する
2. 既存の Y より 10 倍効率的
3. Z の成功事例が存在する
4. ...
```

「わら人形」を叩くのではなく、最強の形にしてから攻撃する。

### 2. Counter-Argument（反論）

その上で、**根本的な欠陥** を指摘：

```markdown
## Counter-Argument（8ポイント）

1. [致命的] X の前提が成り立たない場合、全体が崩壊
2. Y の効率性は特定条件下のみで成立
3. Z の成功事例は規模が異なる
4. ...
```

## Parallel Analysis ワークフロー

```
1. アイデアを24の原子的主張に分解
    ↓
2. 32エージェントが並列で分析
   - 各エージェントは強みと弱み両方を検討
    ↓
3. 収束するインサイトを統合
    ↓
4. Steelman + Counter-Argument を生成
```

所要時間: 60-120秒

## Adversarial Validation ワークフロー

より厳密な3ラウンドプロトコル：

### Round 1: Competing Proposals

異なるアプローチを提案させる

### Round 2: Brutal Critique

互いの提案を徹底批判

### Round 3: Collaborative Synthesis

批判を経て、より強い案を合成

## THE ALGORITHM との統合

[THE ALGORITHM](https://zenn.dev/yasuhito/articles/pai-the-algorithm) の **DETERMINED レベル** で自動的に RedTeam が使われる：

```
努力レベル: DETERMINED
    ↓
THINK フェーズで RedTeam を起動
    ↓
アイデアの弱点を発見
    ↓
ISC に「避けるべきこと」を追加
```

## 使用例

### ビジネスプラン検証

```
User: "このビジネスプランを RedTeam で攻撃して"
```

### 技術設計レビュー

```
User: "このアーキテクチャを RedTeam でストレステスト"
```

### 主張の検証

```
User: "『AI は人間の仕事を奪う』を RedTeam で分析"
```

## Council との使い分け

| 場面 | 使うもの |
|------|---------|
| 設計の方向性を議論 | Council |
| 決定前の最終チェック | RedTeam |
| 複数案から選択 | Council |
| 一つの案を徹底検証 | RedTeam |

Council で方向性を決め、RedTeam で穴がないか確認、という流れが効果的。

## まとめ

RedTeam Skill は PAI の敵対的分析：

- **32エージェント** が多角的に攻撃
- **Steelman** でまず最強の形にする
- **根本的欠陥** を探す（表面的批判ではなく）
- **DETERMINED レベル** で自動活用

「このアイデアの致命的な弱点は何か？」を知りたいときに使う。

## 参考リンク

- [PAI GitHub](https://github.com/danielmiessler/PAI)
- [pai-redteam-skill](https://github.com/danielmiessler/PAI/tree/main/Packs/pai-redteam-skill)
