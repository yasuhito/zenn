---
title: "PAI Agents Skill：動的エージェント生成とパーソナリティシステム"
emoji: "🤖"
type: "tech"
topics: ["ai", "claude", "pai", "agent"]
published: true
---

## Agents Skill とは

[PAI](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure) のエージェント管理パック。タスクに最適なエージェントを **動的に生成** し、それぞれに **固有のパーソナリティと声** を与える。

## 2種類のエージェント

### Named Agents（名前付きエージェント）

あらかじめ定義されたキャラクター：

| 名前 | 役割 | 特徴 |
|------|------|------|
| **Engineer** | 実装担当 | TDD 重視のプリンシパルエンジニア |
| **Architect** | 設計担当 | PhD レベルのシステム設計者 |
| **Designer** | UI/UX 担当 | デザインスクール出身 |
| **QATester** | 品質担当 | ブラウザ自動化でテスト |
| **Pentester** | セキュリティ担当 | 元グレーハット |
| **Artist** | ビジュアル担当 | 画像生成 |
| **Intern** | 雑務担当 | 並列で単純作業をこなす |

各エージェントには固有の ElevenLabs 音声 ID が割り当てられている。

### Dynamic Agents（動的エージェント）

タスクに応じてその場で生成：

```
User: "5人の科学者エージェントでこのデータを分析して"
```

特性を組み合わせて、ユニークなエージェントを作る。

## 特性（Traits）システム

動的エージェントは3つの軸で定義：

### Expertise（専門性）

```
security, legal, finance, medical, technical,
research, creative, business, data, communications
```

### Personality（性格）

```
skeptical, enthusiastic, cautious, bold, analytical,
creative, empathetic, contrarian, pragmatic, meticulous
```

### Approach（アプローチ）

```
thorough, rapid, systematic, exploratory,
comparative, synthesizing, adversarial, consultative
```

### 組み合わせ例

```typescript
// 懐疑的な金融アナリスト
{
  expertise: "finance",
  personality: "skeptical",
  approach: "analytical"
}

// 大胆なクリエイティブディレクター
{
  expertise: "creative",
  personality: "bold",
  approach: "exploratory"
}
```

## Voice Mapping

特性の組み合わせによって、自動で適切な声が選ばれる：

| 組み合わせ | 声 | 特徴 |
|-----------|-----|------|
| skeptical + analytical | George | 知的で温かみ |
| enthusiastic + creative | Jeremy | ハイエナジー |
| bold + business | Domi | アサーティブ CEO |

45種類以上の声がマッピングされている。

## 並列エージェント実行

複数エージェントを同時に走らせる：

```typescript
// 3人のエンジニアで並列実装
Task({ subagent_type: "Engineer", run_in_background: true })
Task({ subagent_type: "Engineer", run_in_background: true })
Task({ subagent_type: "Engineer", run_in_background: true })
```

[THE ALGORITHM](https://zenn.dev/yasuhito/articles/pai-the-algorithm) の努力レベルによって、並列数の上限が決まる：

| 努力レベル | 最大並列数 |
|-----------|-----------|
| QUICK | 1 |
| STANDARD | 3 |
| THOROUGH | 5 |
| DETERMINED | 10 |

## AgentFactory

エージェントを生成するファクトリー：

```bash
bun run AgentFactory.ts \
  --task "このコードをレビューして" \
  --traits "skeptical,meticulous,security"
```

特性を指定すると、適切なエージェントが生成され、タスクを実行する。

## コンテキストローディング

エージェントはマークダウンベースのコンテキストファイルを読み込める：

```
contexts/
├── project-overview.md
├── coding-standards.md
└── security-guidelines.md
```

これにより、プロジェクト固有の知識を持ったエージェントを作れる。

## 使用例

### Council（4人議論）

```
User: "この設計について Council で議論して"

Architect: システム的な観点から...
Designer: UX の観点から...
Engineer: 実装の現実として...
Researcher: 先行事例では...
```

4人が異なる視点で議論し、合意点を見つける。

### RedTeam（32人攻撃）

```
User: "このアイデアを RedTeam で攻撃して"
```

32人のエージェント（Engineer×8, Architect×8, Pentester×8, Intern×8）が徹底的に弱点を探す。[RedTeam Skill](https://github.com/danielmiessler/PAI/tree/main/Packs/pai-redteam-skill) で詳しく解説。

## まとめ

Agents Skill は PAI のエージェント基盤：

- **Named Agents** で定番キャラクター
- **Dynamic Agents** でタスク最適化
- **Traits システム** で特性を組み合わせ
- **Voice Mapping** で声を自動選択
- **並列実行** で効率化

「誰にやらせるか」を AI が自動で決めてくれる。

## 参考リンク

- [PAI GitHub](https://github.com/danielmiessler/PAI)
- [pai-agents-skill](https://github.com/danielmiessler/PAI/tree/main/Packs/pai-agents-skill)
