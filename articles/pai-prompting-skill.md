---
title: "PAI Prompting Skill：プロンプトを書くためのプロンプト"
emoji: "📝"
type: "tech"
topics: ["ai", "claude", "pai", "prompt"]
published: true
---

## Prompting Skill とは

[PAI](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure) のメタプロンプティングシステム。**プロンプトを動的に生成する** 仕組みを提供する。

構造は固定、内容はパラメータ化。これにより一貫性と柔軟性を両立。

## 5つのコアテンプレート（Primitives）

Handlebars 形式のテンプレートが 5 つ用意されている：

### 1. Roster（役割定義）

エージェントの役割を定義：

```handlebars
# Role: {{role}}

You are {{personality}}.

## Expertise
{{#each expertise}}
- {{this}}
{{/each}}
```

### 2. Voice（声・トーン）

応答のスタイルを定義：

```handlebars
## Voice Guidelines

Tone: {{tone}}
Formality: {{formality}}

{{#if casual}}
Use contractions and conversational language.
{{/if}}
```

### 3. Structure（出力構造）

出力フォーマットを定義：

```handlebars
## Output Format

{{#if json}}
Return valid JSON with this schema:
{{schema}}
{{else}}
Return markdown with these sections:
{{#each sections}}
- {{this}}
{{/each}}
{{/if}}
```

### 4. Briefing（コンテキスト）

背景情報を注入：

```handlebars
## Context

Project: {{project}}
Tech Stack: {{stack}}

{{#if constraints}}
## Constraints
{{constraints}}
{{/if}}
```

### 5. Gate（検証条件）

出力の検証条件を定義：

```handlebars
## Validation

Before responding, verify:
{{#each gates}}
- [ ] {{this}}
{{/each}}
```

## 5つの評価テンプレート（Evals）

LLM-as-Judge パターン用のテンプレート：

| テンプレート | 用途 |
|-------------|------|
| **Judge.hbs** | 出力の品質評価 |
| **Rubric.hbs** | 評価基準の定義 |
| **Comparison.hbs** | 2つの出力を比較 |
| **Report.hbs** | 評価レポート生成 |
| **TestCase.hbs** | テストケース生成 |

## YAML データファイル

テンプレートに渡すデータを YAML で定義：

### Agents.yaml

```yaml
engineer:
  role: "Senior Software Engineer"
  personality: "pragmatic, detail-oriented"
  expertise:
    - TypeScript
    - System Design
    - TDD

architect:
  role: "System Architect"
  ...
```

### VoicePresets.yaml

```yaml
casual:
  tone: friendly
  formality: low
  contractions: true

professional:
  tone: neutral
  formality: high
  contractions: false
```

## テンプレートレンダリング

CLI でテンプレートを適用：

```bash
bun run RenderTemplate.ts \
  --template Primitives/Roster.hbs \
  --data Agents.yaml \
  --key engineer
```

出力：

```markdown
# Role: Senior Software Engineer

You are pragmatic, detail-oriented.

## Expertise
- TypeScript
- System Design
- TDD
```

## Fabric パターン統合

[Fabric](https://github.com/danielmiessler/fabric) の 242 パターンとも連携。Fabric パターンは本質的に「目的特化型プロンプト」なので、Prompting Skill と相性が良い。

## Claude 4.x ベストプラクティス

Standards.md に Claude 4.x 向けのベストプラクティスが含まれている：

- 構造化された指示
- 具体例の提示
- 明確な制約
- 段階的な思考の促進

## 使用例

### 一貫したエージェント生成

```
User: "新しいセキュリティエンジニアエージェントを作って"

AI:
1. Roster テンプレートを使用
2. Agents.yaml から security 設定を読み込み
3. VoicePresets.yaml から professional を適用
4. 一貫したプロンプトを生成
```

### 出力フォーマット標準化

```
User: "JSON で返して"

AI: Structure テンプレートを適用
→ 常に同じ形式の JSON が返る
```

## まとめ

Prompting Skill は PAI のメタプロンプティング：

- **5つのコアテンプレート** で構造を定義
- **YAML データファイル** でパラメータ化
- **CLI ツール** でテンプレートを適用
- **Fabric 統合** で 242 パターン活用

「プロンプトのプロンプト」で一貫性と品質を確保。

## 参考リンク

- [PAI GitHub](https://github.com/danielmiessler/PAI)
- [pai-prompting-skill](https://github.com/danielmiessler/PAI/tree/main/Packs/pai-prompting-skill)
- [Fabric](https://github.com/danielmiessler/fabric)
