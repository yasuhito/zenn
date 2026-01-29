---
title: "PAI BrightData Skill：4 段階フォールバックでどんな URL も取得"
emoji: "🌐"
type: "tech"
topics: ["ai", "claude", "pai", "scraping"]
published: true
---

## BrightData Skill とは

[PAI](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure) の Web スクレイピングパック。**4 段階のフォールバック戦略** で、ブロックされにくい URL 取得を実現する。

## 問題：AI のスクレイピングはブロックされがち

Web サイトは AI ボットをブロックする。単純な fetch では：

- CAPTCHA が出る
- 403 Forbidden が返る
- JavaScript レンダリングが必要
- レート制限に引っかかる

## 解決：4 段階フォールバック

```
Tier 1: WebFetch
    ↓ 失敗したら
Tier 2: Curl + ブラウザヘッダー
    ↓ まだダメなら
Tier 3: Playwright（フルブラウザ）
    ↓ CAPTCHA が出たら
Tier 4: Bright Data（プロキシ）
```

### Tier 1: WebFetch

最もシンプルで高速。ほとんどの普通のサイトはこれで取得できる。

```typescript
const content = await webFetch(url);
```

### Tier 2: Curl + ブラウザヘッダー

Chrome のヘッダーを偽装。Bot 検出を回避：

```bash
curl -H "User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)..." \
     -H "Accept: text/html,application/xhtml+xml..." \
     "$URL"
```

### Tier 3: Playwright

フルブラウザを起動。JavaScript レンダリングが必要なサイト向け：

```typescript
const browser = await playwright.chromium.launch();
const page = await browser.newPage();
await page.goto(url);
const content = await page.content();
```

[Browser Skill](https://zenn.dev/yasuhito/articles/pai-browser-skill) を内部で使用。

### Tier 4: Bright Data

最終手段。プロ用プロキシサービスを使用：

- 住宅 IP プロキシ
- CAPTCHA 自動解決
- 地域別 IP

**Bright Data API キーが必要。**

## 使い分け

| サイト種別 | 通常の Tier |
|-----------|-------------|
| 普通のブログ、ドキュメント | Tier 1 |
| ニュースサイト | Tier 1-2 |
| SNS（Twitter, LinkedIn） | Tier 2-3 |
| E コマース（Amazon 等） | Tier 3-4 |
| 強力な Bot 対策サイト | Tier 4 |

## 自動フォールバック

手動で Tier を指定する必要はない。失敗したら自動で次の Tier に進む：

```typescript
const content = await smartFetch(url);
// 内部で Tier 1 → 2 → 3 → 4 と自動フォールバック
```

## レート制限

各 Tier にレート制限を設定可能：

```yaml
brightdata:
  tier1_rate: 100/min
  tier2_rate: 30/min
  tier3_rate: 10/min
  tier4_rate: 5/min  # Bright Data は高コストなので控えめに
```

## コスト意識

Tier 4（Bright Data）は有料サービス。PAI は自動的にコストを最小化：

1. まず無料の方法を試す
2. 失敗した場合のみ有料にフォールバック
3. キャッシュで同じ URL の再取得を避ける

## [Research Skill](https://zenn.dev/yasuhito/articles/pai-research-skill) との連携

Research Skill のリサーチ中、URL 取得に BrightData Skill が使われる：

```
User: "この会社について調べて"
    ↓
Research Skill が関連 URL を発見
    ↓
BrightData Skill で各 URL を取得
    ↓
取得した内容を分析
```

## まとめ

BrightData Skill は PAI のスクレイピング基盤：

- **4 段階フォールバック** で高い成功率
- **自動 Tier 選択** で手間なし
- **コスト最小化** で無駄な課金を避ける
- **Research Skill 連携** でリサーチを支援

「取得できない URL」を減らす。

## 参考リンク

- [PAI GitHub](https://github.com/danielmiessler/PAI)
- [pai-brightdata-skill](https://github.com/danielmiessler/PAI/tree/main/Packs/pai-brightdata-skill)
- [Bright Data](https://brightdata.com/)
