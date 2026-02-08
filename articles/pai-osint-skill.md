---
title: "PAI OSINT Skill：倫理的な公開情報インテリジェンス"
emoji: "🔍"
type: "tech"
topics: ["ai", "claude", "pai", "osint"]
published: true
---

## OSINT Skill とは

[PAI](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure) のオープンソースインテリジェンス（OSINT）パック。公開情報から **倫理的に** 情報を収集・分析する。

## 3つのドメイン

### 1. People Intelligence（人物調査）

- 経歴、資格の確認
- デジタルフットプリント
- ソーシャルメディア分析

### 2. Company Intelligence（企業調査）

- 会社登記情報
- ドメイン・技術インフラ
- 投資デューデリジェンス（投資前の徹底調査）

### 3. Entity/Threat Intelligence（脅威調査）

- ドメイン分析
- IP レピュテーション
- マルウェアインテリジェンス

## 倫理フレームワーク

OSINT Skill の重要な特徴が **倫理フレームワーク**。

### 認可チェック

調査開始前に必ず認可を確認：

```
User: "この人について調べて"
AI: "どのような目的で調査しますか？"
    1. 採用候補者のバックグラウンドチェック
    2. ビジネスパートナーの確認
    3. 本人からの依頼
```

正当な目的なしの調査は実行しない。

### Red Lines（絶対禁止）

- 不正アクセス
- ソーシャルエンジニアリング
- 非公開情報の取得
- ストーキング目的

### データ取り扱い

- 収集した情報の保持期間を明示
- 機密情報の適切な廃棄
- 目的外利用の禁止

## ワークフロー

### PeopleLookup（7フェーズ）

1. 認可確認
2. 基本情報収集
3. ソーシャルメディア検索
4. 公的記録検索
5. クロスリファレンス
6. 検証
7. レポート作成

### CompanyLookup（7フェーズ）

1. 認可確認
2. 会社登記確認
3. ドメイン・技術調査
4. 経営陣調査
5. 財務情報（公開分）
6. 検証
7. レポート作成

### CompanyDueDiligence（5フェーズ）

投資検討向けの詳細調査。32エージェント以上を使用。

### EntityLookup（9フェーズ）

脅威インテリジェンス。ドメイン、IP、マルウェアの分析。

## ツールリファレンス

OSINT Skill には 90 以上のツールリファレンスが含まれている：

### People Tools（30+）

- LinkedIn, Facebook, Twitter 検索
- 公的記録データベース
- 逆画像検索
- ユーザー名検索

### Company Tools（25+）

- 会社登記データベース
- WHOIS, DNS 検索
- 技術スタック検出
- 投資家情報データベース

### Entity Tools（35+）

- VirusTotal, Shodan
- IP レピュテーション
- C2 検出ツール
- マルウェアサンドボックス

## 信頼度レベル

収集した情報には信頼度を付与：

| レベル | 意味 |
|--------|------|
| **High** | 公式ソースで確認済み |
| **Medium** | 複数ソースで一致 |
| **Low** | 単一ソース、未検証 |

## 使用例

### 採用候補者チェック

```
User: "この採用候補者のバックグラウンドチェックをお願い"
AI: PeopleLookup ワークフローを実行
→ LinkedIn 経歴確認
→ 公開ポートフォリオ確認
→ レポート生成
```

### 投資先調査

```
User: "この会社への投資を検討している。デューデリジェンスして"
AI: CompanyDueDiligence ワークフローを実行
→ 登記確認、財務分析、経営陣調査
→ リスク評価レポート
```

## まとめ

OSINT Skill は PAI の情報収集基盤：

- **3つのドメイン** で人物・企業・脅威を調査
- **倫理フレームワーク** で認可と目的を確認
- **90+ ツール** の統合リファレンス
- **信頼度レベル** で情報品質を明示

公開情報を倫理的に、体系的に収集・分析する。

## 参考リンク

- [PAI GitHub](https://github.com/danielmiessler/PAI)
- [pai-osint-skill](https://github.com/danielmiessler/PAI/tree/main/Packs/pai-osint-skill)
