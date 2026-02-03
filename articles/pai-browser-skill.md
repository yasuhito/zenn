---
title: "PAI Browser Skill：デバッグファーストのブラウザ自動化"
emoji: "🌐"
type: "tech"
topics: ["ai", "claude", "pai", "playwright"]
published: true
---

## Browser Skill とは

[PAI](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure) のブラウザ自動化パック。Playwright を使い、**デバッグ情報を最初から全部取る** 設計。

## デバッグファースト哲学

「問題が起きてからログを有効化」ではない。**最初から全部記録** する。

記録される情報：
- **Console logs** - `console.log`, `console.error` など
- **Network requests** - 全リクエスト、ヘッダー、タイミング
- **Network responses** - ステータスコード、レスポンス時間
- **Page errors** - 未キャッチ例外、Promise rejection

## トークン削減効果

MCP 経由の Playwright より **99% 以上のトークン削減**：

| アプローチ | トークン数 |
|-----------|-----------|
| Playwright MCP | ~13,700 (ロード時) |
| Code-first | ~50-200 (操作ごと) |

直接 Playwright API を叩くので、MCP のオーバーヘッドがない。

## 主要コマンド

### ナビゲーション（診断付き）

```bash
bun run Browse.ts https://example.com
```

出力：

```
Screenshot: /tmp/browse-1704614400.png
Console Errors (2): ...
Failed Requests (1): ...
Network: 34 requests | 1.2MB | avg 120ms
Page: "Example" loaded successfully
```

### スクリーンショット

```bash
bun run Browse.ts screenshot https://example.com /tmp/shot.png
```

### 要素検証

```bash
bun run Browse.ts verify https://example.com "button.logout"
```

## セッション自動開始

明示的な `session start` は不要。最初のコマンドで自動的にブラウザが起動する。

## ワークフロー

### Extract（コンテンツ抽出）

Web ページからコンテンツを抽出：

```
User: "このページから価格情報を抽出して"
```

### Interact（フォーム操作）

フォーム入力やクリック：

```
User: "ログインフォームに入力して送信"
```

### Screenshot（スクリーンショット）

証跡としてスクリーンショットを保存：

```
User: "このページのスクリーンショットを撮って"
```

### VerifyPage（ページ検証）

ページが正しく読み込まれたか検証：

```
User: "このURLが正しく表示されるか確認して"
```

## THE ALGORITHM との統合

[THE ALGORITHM](https://zenn.dev/yasuhito/articles/pai-the-algorithm) の VERIFY フェーズで使用：

```
ISC 行: "ログアウトボタンが表示される"
    ↓
Browser Skill でスクリーンショット
    ↓
視覚的に検証
    ↓
ISC 行を PASS/BLOCKED に更新
```

STANDARD 以上の努力レベルで利用可能。

## API 例

### Playwright インスタンス取得

```typescript
import { PlaywrightBrowser } from './index';

const browser = new PlaywrightBrowser();
await browser.navigate('https://example.com');

// 診断情報を取得
const diagnostics = browser.getDiagnostics();
console.log(diagnostics.consoleErrors);
console.log(diagnostics.failedRequests);
```

### スクリーンショット

```typescript
await browser.screenshot('/tmp/page.png');
```

## まとめ

Browser Skill は PAI のブラウザ自動化：

- **デバッグファースト** で最初から全情報記録
- **トークン 99% 削減** で効率的
- **セッション自動開始** で手間なし
- **THE ALGORITHM 統合** で VERIFY フェーズに活用

Web アプリの検証を自動化する。

## 参考リンク

- [PAI GitHub](https://github.com/danielmiessler/PAI)
- [pai-browser-skill](https://github.com/danielmiessler/PAI/tree/main/Packs/pai-browser-skill)
- [Playwright](https://playwright.dev/)
