---
title: "PAI Art Skill：一貫したスタイルで AI 画像生成"
emoji: "🎨"
type: "tech"
topics: ["ai", "claude", "pai", "image"]
published: true
---

## Art Skill とは

[PAI](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure) の画像生成パック。ブログのヘッダー画像、技術図、データ可視化などを **一貫したスタイル** で生成する。

## 問題：毎回バラバラなスタイル

AI 画像生成でありがちな問題：

1. **スタイルが毎回違う** - ブランドの統一感がない
2. **デザインシステムがない** - タイポグラフィがバラバラ
3. **毎回プロンプトを考える** - 手間がかかる
4. **テキストが崩れる** - AI は文字が苦手

## 解決：7つの専用ワークフロー

| ワークフロー | 用途 |
|-------------|------|
| **Essay** | 記事用イラスト |
| **Visualize** | データ可視化 |
| **TechnicalDiagrams** | アーキテクチャ図 |
| **Mermaid** | Excalidraw 風の図 |
| **Frameworks** | 2x2 マトリクス、メンタルモデル |
| **Stats** | 統計カード |
| **CreatePAIPackIcon** | PAI パックのアイコン |

## デザインシステム

すべての画像が統一されたパレットを使用：

```
背景: #0a0a0f (dark)
プライマリ: #4a90d9 (electric blue)
アクセント: #8b5cf6 (purple, 10-15%)
```

## 使用例

### 記事用イラスト

```
User: "AI と人間の協働についてのイラストを作って"
```

Essay ワークフローが起動し、エディトリアルスタイルのイラストを生成。

### アーキテクチャ図

```
User: "マイクロサービスアーキテクチャの図を作って"
```

TechnicalDiagrams ワークフローが起動。

### 統計カード

```
User: "『開発者の 73% が AI を使用』のビジュアルを作って"
```

Stats ワークフローが統計を視覚化。

## Generate ツール

CLI で直接画像生成：

```bash
bun run Generate.ts \
  --model nano-banana-pro \
  --prompt "A developer collaborating with AI, editorial style" \
  --size 1K \
  --aspect-ratio 16:9 \
  --output ~/Downloads/header.png
```

### 背景透過

アイコンなど透過が必要な場合：

```bash
bun run Generate.ts \
  --prompt "..." \
  --remove-bg \
  --output icon.png
```

`--remove-bg` が重要。これがないと、透過ではなくチェッカーボードパターンが焼き込まれる。

## 画像モデル

| モデル | 特徴 |
|--------|------|
| **nano-banana-pro** | Gemini 3 Pro Image、高品質 |
| **flux** | 代替モデル |

## PAI パックアイコン

PAI パック用のアイコンを生成する専用ワークフロー：

```
User: "pai-research-skill 用のアイコンを作って"
```

仕様：
- 256x256 ピクセル
- 透過 PNG
- 青 (#4a90d9) + 紫 (#8b5cf6)
- 64x64 でも読めるシンプルさ

## まとめ

Art Skill は PAI のビジュアル生成：

- **7つのワークフロー** で用途別に最適化
- **統一デザインシステム** でブランド一貫性
- **Generate CLI** で直接操作
- **背景透過** でアイコン生成

デザインスキルなしで、プロ品質のビジュアルを作れる。

## 参考リンク

- [PAI GitHub](https://github.com/danielmiessler/PAI)
- [pai-art-skill](https://github.com/danielmiessler/PAI/tree/main/Packs/pai-art-skill)
