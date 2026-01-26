---
title: "AIエージェントのメモリ系プロジェクト比較（2026年1月）"
emoji: "🧠"
type: "tech"
topics: ["ai", "llm", "agent", "memory"]
published: true
---

## はじめに

AIエージェントが「記憶」を持つことで、長期的な文脈を保持し、よりパーソナライズされた応答が可能になります。2026年1月時点で注目されているメモリ系プロジェクトを比較してみました。

## プロジェクト一覧

| ⭐️ | 名前 | 概要 |
| ------ | ---------- | ----------------------------------------------------------- |
| 46,042 | [mem0](https://github.com/mem0ai/mem0) | 最人気。User/Session/Agentメモリ、YC出身 |
| 43,710 | [Clawdbot](https://github.com/clawdbot/clawdbot) | MEMORY.md + memory/*.md のシンプル構成 |
| 15,185 | [claude-mem](https://github.com/anthropics/claude-mem) | Claude Code用。セッション自動記録→AI圧縮→次回注入 |
| 5,914 | [PAI (UOCS)](https://github.com/pAI-OS/pAI) | Agentic AI Infrastructure for magnifying HUMAN capabilities |
| 4,714 | [MemOS](https://github.com/MemTech/MemOS) | OS-Level Memory Layer（長期・作業・外部） |
| 3,498 | [MIRIX](https://github.com/MIRIX-AI/MIRIX) | 画面キャプチャからメモリ構築 |
| 2,540 | [Memobase](https://github.com/memobase-ai/memobase) | ユーザープロファイルベース長期記憶 |

※ ⭐️はGitHubスター数（2026年1月時点）

## 各プロジェクトの特徴

### mem0
YCombinator出身の最も人気のあるプロジェクト。User/Session/Agentの3レベルでメモリを管理でき、ベクトルDBと連携した検索機能が充実しています。

### Clawdbot
`MEMORY.md` と `memory/*.md` というシンプルなMarkdownファイル構成でメモリを管理。複雑なインフラ不要で、git管理との相性が良いのが特徴。

### claude-mem
Claude Code向けに特化。セッション終了時に自動で会話を記録し、AIが重要な情報を圧縮して次回セッション開始時に注入します。

### PAI (UOCS)
「人間の能力を拡張する」をコンセプトにしたAgentic AIインフラ。メモリだけでなく、エージェント全体のライフサイクル管理を担います。

### MemOS
OSレベルでメモリレイヤーを提供するアプローチ。長期記憶・作業記憶・外部記憶の3層構造で、複数のAIアプリケーション間でメモリを共有できます。

### MIRIX
ユニークなアプローチで、画面キャプチャから自動的にコンテキストを抽出してメモリを構築。ユーザーの作業内容を暗黙的に学習します。

### Memobase
ユーザープロファイルをベースにした長期記憶システム。ユーザーの好みや過去のやり取りを学習し、パーソナライズされた応答を実現します。

## まとめ

メモリ系プロジェクトは、シンプルなファイルベース（Clawdbot）から、本格的なインフラ（mem0, MemOS）まで様々なアプローチがあります。用途に応じて選択するのが良さそうです。

- **手軽に始めたい** → Clawdbot, claude-mem
- **本格的なプロダクション** → mem0, MemOS
- **ユニークなアプローチ** → MIRIX, Memobase
