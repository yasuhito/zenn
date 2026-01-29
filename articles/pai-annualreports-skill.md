---
title: "PAI AnnualReports Skill：570 以上のセキュリティレポートを横断検索"
emoji: "📑"
type: "tech"
topics: ["ai", "claude", "pai", "security"]
published: true
---

## AnnualReports Skill とは

[PAI](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure) のセキュリティインテリジェンスパック。**570 以上** のセキュリティ年次レポートを検索・分析できる。

## 収録レポート

### 主要ベンダー

| ベンダー | レポート数 | 特徴 |
|---------|-----------|------|
| **CrowdStrike** | 30+ | 攻撃者グループの詳細分析 |
| **Microsoft** | 40+ | Azure/365 の脅威データ |
| **Mandiant** | 25+ | APT の深掘り |
| **IBM X-Force** | 20+ | データ侵害コスト分析 |
| **Verizon** | 15+ | DBIR（業界標準） |
| **Palo Alto** | 20+ | クラウドセキュリティ |

### その他（500+）

- Sophos, Kaspersky, ESET
- Recorded Future, Flashpoint
- 政府機関（CISA, ENISA）
- 業界特化（医療、金融、製造）

## 使用例

### 脅威トレンド調査

```
User: "2025年のランサムウェアトレンドを調べて"
```

AI が複数のレポートから情報を統合：

```markdown
## ランサムウェアトレンド 2025

### 主要な発見

| ソース | 発見事項 |
|--------|---------|
| CrowdStrike | 平均身代金額が 200% 増加 |
| Verizon DBIR | 医療業界が最大の標的に |
| IBM | 復旧に平均 23 日間 |

### 新しい攻撃手法
- 二重恐喝の標準化
- サプライチェーン経由の侵入増加
- AI を使った攻撃の出現
```

### 特定の脅威アクター調査

```
User: "APT29 について最新情報を"
```

### 業界別リスク分析

```
User: "医療業界のサイバーリスクをまとめて"
```

### 年次比較

```
User: "フィッシング攻撃の 2023 → 2025 の変化は？"
```

## ワークフロー

### Search（検索）

キーワードでレポートを検索：

```
User: "zero-day に関するレポートを探して"
```

### Compare（比較）

複数ベンダーの見解を比較：

```
User: "ランサムウェアについて、CrowdStrike と Mandiant の分析を比較して"
```

### Synthesize（統合）

複数レポートから統合レポートを生成：

```
User: "2025 年のクラウドセキュリティについて、各社レポートを統合して"
```

## レポートインデックス

レポートは以下の形式でインデックス化：

```yaml
- vendor: CrowdStrike
  title: Global Threat Report 2025
  year: 2025
  topics:
    - ransomware
    - apt
    - ecrime
  url: https://...
```

トピックタグで横断検索が可能。

## 定期更新

レポートインデックスは定期的に更新される。新しいレポートが公開されると、自動で検索対象に追加。

## [Research Skill](https://zenn.dev/yasuhito/articles/pai-research-skill) との連携

Research Skill のセキュリティリサーチで自動的に使用：

```
User: "最新のサイバー脅威をリサーチして"
    ↓
Research Skill が起動
    ↓
AnnualReports Skill でベンダーレポートを検索
    ↓
Web 検索と統合してレポート作成
```

## 出力例

```markdown
## セキュリティインテリジェンスレポート

### 調査項目: ランサムウェア 2025

### ソース（12 レポート）
1. CrowdStrike Global Threat Report 2025
2. Verizon DBIR 2025
3. IBM X-Force Threat Intelligence Index 2025
...

### キーインサイト

**攻撃規模**
- 前年比 38% 増加（CrowdStrike）
- 身代金総額 $8.5B（Chainalysis）

**主要なグループ**
| グループ | 被害件数 | 特徴 |
|---------|---------|------|
| LockBit | 1,200+ | RaaS モデル |
| ALPHV | 800+ | 二重恐喝 |
| Cl0p | 600+ | ゼロデイ活用 |

### 推奨対策
1. バックアップの 3-2-1 ルール
2. EDR の導入
3. ゼロトラストアーキテクチャ
```

## まとめ

AnnualReports Skill は PAI のセキュリティ情報源：

- **570+ レポート** を横断検索
- **複数ベンダー比較** で偏りのない分析
- **統合レポート生成** で情報を整理
- **Research Skill 連携** で自動活用

セキュリティの「一次情報」にアクセスする。

## 参考リンク

- [PAI GitHub](https://github.com/danielmiessler/PAI)
- [pai-annualreports-skill](https://github.com/danielmiessler/PAI/tree/main/Packs/pai-annualreports-skill)
