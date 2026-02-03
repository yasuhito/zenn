---
title: "PAI Council Skill：4 エージェント議論で最良の答えを見つける"
emoji: "🗣️"
type: "tech"
topics: ["ai", "claude", "pai", "multiagent"]
published: true
---

## Council Skill とは

[PAI](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure) のマルチエージェント議論システム。4人の専門家エージェントが **3ラウンドの議論** を行い、多角的な視点から最良の答えを見つける。

## Council vs RedTeam

PAI には2つのマルチエージェントシステムがある：

| システム | エージェント数 | モード | 目的 |
|---------|---------------|--------|------|
| **Council** | 4 | 協調的 | 最良の答えを見つける |
| **RedTeam** | 32 | 敵対的 | 弱点を徹底的に攻撃 |

Council は「議論して合意を形成」、RedTeam は「攻撃して穴を見つける」。

## 4人のメンバー

| 役割 | 視点 | 声 |
|------|------|-----|
| **Architect** | システム設計、長期的視点 | Serena Blackwood |
| **Designer** | UX、ユーザーニーズ | Aditi Sharma |
| **Engineer** | 実装の現実、技術的負債 | Marcus Webb |
| **Researcher** | データ、先行事例 | Ava Chen |

[Agents Skill](https://zenn.dev/yasuhito/articles/pai-agents-skill) の Named Agents を使用。

## 3ラウンド構造

### Round 1: Initial Positions（初期立場）

各エージェントが自分の視点から意見を述べる：

```
Architect: システム的に見ると、マイクロサービスが適切だ。
Designer: ユーザー体験を考えると、レスポンス速度が最優先。
Engineer: 現実的に、2週間でマイクロサービスは無理。
Researcher: 先行事例では、段階的移行が成功している。
```

### Round 2: Responses & Challenges（反論・質問）

互いの意見に対して反論や質問：

```
Architect → Engineer: 2週間で無理というが、どこがボトルネック？
Engineer → Architect: 認証基盤の移行だけで1週間かかる。
Designer → Researcher: 段階的移行中のUX劣化はどう対処した？
Researcher → Designer: A/Bテストで影響を最小化している。
```

### Round 3: Synthesis（統合）

合意点、相違点、最終推奨を整理：

```
## 合意点
- 段階的移行が現実的
- 認証基盤を優先

## 相違点
- タイムライン（2週間 vs 4週間）

## 推奨
認証基盤のマイクロサービス化を4週間で実施。
その後、段階的に他のサービスを分離。
```

## 2つのワークフロー

### DEBATE（フル議論）

3ラウンドすべてを実行：

```
User: "この設計について Council で議論して"
```

所要時間: 30-90秒

### QUICK（クイックチェック）

Round 1 のみ、各視点を素早く収集：

```
User: "この案について quick council して"
```

所要時間: 10-15秒

## 議論トランスクリプト

Council の特徴は **議論の過程が見える** こと。誰が何を言い、どう反論されたかがすべて記録される：

```markdown
## Council Transcript

**[Round 1 - Initial Positions]**

**Architect (Serena):**
> マイクロサービスアーキテクチャを推奨する。
> 理由：スケーラビリティ、独立デプロイ、技術的負債の分離。

**Designer (Aditi):**
> ユーザー視点では、現状の遅延が問題。
> アーキテクチャより、パフォーマンス改善を優先すべき。

...
```

意思決定の根拠が残り、後から振り返れる。

## THE ALGORITHM との統合

[THE ALGORITHM](https://zenn.dev/yasuhito/articles/pai-the-algorithm) の THOROUGH 以上で自動的に Council が使われる：

```
努力レベル: THOROUGH
    ↓
THINK フェーズで Council を起動
    ↓
複数視点で要件を検討
    ↓
ISC に発見事項を追加
```

## 使用例

### 技術選定

```
User: "React vs Vue vs Svelte について Council で議論して"
```

### 設計レビュー

```
User: "この API 設計を Council でレビューして"
```

### 意思決定

```
User: "リファクタリング vs 作り直し、Council で議論して"
```

## まとめ

Council Skill は PAI の協調的議論システム：

- **4人の専門家** が異なる視点で議論
- **3ラウンド** で意見→反論→統合
- **トランスクリプト** で議論の過程が見える
- **THE ALGORITHM 統合** で自動活用

一人で考えると見落としがちなことも、複数視点で検討すれば気づける。

## 参考リンク

- [PAI GitHub](https://github.com/danielmiessler/PAI)
- [pai-council-skill](https://github.com/danielmiessler/PAI/tree/main/Packs/pai-council-skill)
