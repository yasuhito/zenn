---
title: "PAI Research Skill：複数エージェント並列リサーチシステム"
emoji: "🔬"
type: "tech"
topics: ["ai", "claude", "pai", "research"]
published: true
---

## Research Skill とは

[PAI](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure) のリサーチ機能を担うパック。複数の AI エージェントを並列で走らせ、多角的な調査結果を統合する。

## 3つのリサーチモード

| モード | エージェント数 | 速度 | 用途 |
|--------|--------------|------|------|
| **Quick** | 1 | 10-15秒 | 単純な質問、時間優先 |
| **Standard** | 3 | 15-30秒 | 通常のリサーチ（デフォルト） |
| **Extensive** | 12 | 60-90秒 | 重要な意思決定、徹底調査 |

### Quick モード

Claude の WebSearch だけを使う最速モード：

```
User: "quick research: React 19 の新機能"
```

### Standard モード（デフォルト）

3つの異なる視点でリサーチ：

```
User: "React 19 について調べて"

├─ Claude (WebSearch) → 公式情報、ドキュメント
├─ Gemini → 多角的な視点、コミュニティの反応
└─ Perplexity → 最新情報、引用付き
```

結果を統合して、バランスの取れた回答を生成。

### Extensive モード

9エージェントを3タイプ×3で並列実行：

```
User: "extensive research: 2026年の AI トレンド"

├─ Researchers (×3) → 情報収集
├─ Analysts (×3) → 分析・解釈  
└─ Critics (×3) → 批判的検証
```

重要な意思決定や、徹底的に調べたい場合に使う。

## Extract Alpha ワークフロー

単なる要約ではなく、**高価値のインサイト** を抽出する：

```
User: "この記事から extract alpha して"
```

- 一般論ではない独自の視点
- 他では得られない知見
- 実行可能なアクション

「ふーん」で終わらない、価値ある情報を引き出す。

## URL 検証プロトコル

Research Skill の重要な機能が **URL 検証**。AI がハルシネーションで存在しない URL を生成するのを防ぐ：

```typescript
// 引用する前に URL が実在するか確認
const isValid = await verifyUrl(url);
if (!isValid) {
  // URL を除外、または代替を探す
}
```

## Fabric パターン統合

[Fabric](https://github.com/danielmiessler/fabric) の 242 パターンと統合：

```
User: "この論文を analyze_paper パターンで処理して"
```

主なパターン：
- `extract_wisdom` - 知恵を抽出
- `analyze_claims` - 主張を分析
- `create_summary` - 要約を作成
- `extract_insights` - インサイトを抽出

## YouTube 対応

YouTube 動画からの情報抽出：

```
User: "この YouTube を要約して: [URL]"
```

`fabric -y` を使って、動画の内容をテキスト化・分析できる。

## 他のスキルとの連携

Research Skill は [THE ALGORITHM](https://zenn.dev/yasuhito/articles/pai-the-algorithm) と連携：

```
THE ALGORITHM (EXECUTE フェーズ)
    ↓
Research Skill を起動
    ↓
ISC の「調査」行を DONE に
```

THOROUGH 以上の努力レベルでは、リサーチ結果が ISC に自動追加される。

## まとめ

Research Skill は PAI のリサーチ基盤：

- **3モード** で速度と深さを選択
- **複数エージェント並列** で多角的な調査
- **Extract Alpha** で高価値インサイト抽出
- **URL 検証** でハルシネーション防止
- **Fabric 統合** で 242 パターン活用

「調べて」と言うだけで、複数の AI が協力して調査してくれる。

## 参考リンク

- [PAI GitHub](https://github.com/danielmiessler/PAI)
- [pai-research-skill](https://github.com/danielmiessler/PAI/tree/main/Packs/pai-research-skill)
- [Fabric](https://github.com/danielmiessler/fabric)
