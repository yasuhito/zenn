---
title: "PAI Recon Skill：セキュリティ偵察の 5 つのワークフロー"
emoji: "🔎"
type: "tech"
topics: ["ai", "claude", "pai", "security"]
published: true
---

## Recon Skill とは

[PAI](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure) のセキュリティ偵察パック。ドメイン、IP、ネットワークの情報を **パッシブに** 収集する。

[OSINT Skill](https://zenn.dev/yasuhito/articles/pai-osint-skill) が「人物・企業」を調査するのに対し、Recon Skill は「技術インフラ」を調査する。

## 5 つのワークフロー

### 1. PassiveRecon（パッシブ偵察）

ターゲットに直接アクセスせず、公開情報のみを収集：

| 収集項目 | ソース |
|---------|--------|
| DNS レコード | 公開 DNS |
| WHOIS 情報 | WHOIS データベース |
| SSL 証明書 | Certificate Transparency |
| サブドメイン | 証明書、DNS 履歴 |
| 技術スタック | BuiltWith, Wappalyzer |

```
User: "example.com を passive recon して"
```

### 2. DomainRecon（ドメイン偵察）

ドメイン全体をマッピング：

- サブドメイン列挙
- DNS ゾーン転送チェック
- MX, TXT, SPF, DKIM レコード
- 関連ドメインの発見

```
User: "example.com のドメイン構造を調べて"
```

### 3. IpRecon（IP 偵察）

IP アドレスの詳細調査：

- 地理的位置
- ASN 情報
- 逆引き DNS
- 同一 IP 上の他ドメイン
- ブラックリスト登録状況

```
User: "203.0.113.1 を調べて"
```

### 4. NetblockRecon（ネットブロック偵察）

CIDR 範囲のスキャン：

- 範囲内のアクティブ IP
- 各 IP のサービス
- ネットワーク構造の推測

```
User: "203.0.113.0/24 をスキャンして"
```

### 5. BountyPrograms（バグバウンティ発見）

対象のバグバウンティプログラムを探す：

- HackerOne
- Bugcrowd
- 独自プログラム

```
User: "example.com にバグバウンティある？"
```

## 使用ツール

Recon Skill は内部で多数のツールを使用：

| ツール | 用途 |
|--------|------|
| **dig** | DNS クエリ |
| **whois** | ドメイン登録情報 |
| **nslookup** | 名前解決 |
| **subfinder** | サブドメイン列挙 |
| **httpx** | HTTP プローブ |
| **nuclei** | 脆弱性スキャン |

## 倫理的制約

Recon Skill は **パッシブ偵察** に限定：

✅ 許可されること：
- 公開 DNS へのクエリ
- 公開情報の収集
- 自分が管理するシステムの調査

❌ 禁止されること：
- アクティブスキャン（許可なし）
- 侵入テスト（許可なし）
- サービス妨害

## 出力フォーマット

```markdown
## Recon Report: example.com

### DNS Records
| Type | Value |
|------|-------|
| A | 203.0.113.1 |
| MX | mail.example.com |
| TXT | "v=spf1 include:_spf.google.com ~all" |

### Subdomains (12 found)
- www.example.com
- api.example.com
- mail.example.com
...

### Technology Stack
- Web Server: nginx/1.18
- CMS: WordPress 6.0
- CDN: Cloudflare

### Security Observations
- SPF record configured ✅
- DMARC not found ⚠️
- Open ports: 80, 443
```

## [THE ALGORITHM](https://zenn.dev/yasuhito/articles/pai-the-algorithm) との統合

THOROUGH 以上の努力レベルで、セキュリティ関連タスクに自動適用：

```
User: "このサイトのセキュリティをチェックして"
努力レベル: THOROUGH
    ↓
Recon Skill で技術情報収集
    ↓
ISC に発見事項を追加
```

## まとめ

Recon Skill は PAI のセキュリティ偵察：

- **5 つのワークフロー** で目的別に調査
- **パッシブ限定** で倫理的に
- **複数ツール統合** で包括的な情報収集
- **構造化レポート** で結果を整理

技術インフラの「見える化」を実現する。

## 参考リンク

- [PAI GitHub](https://github.com/danielmiessler/PAI)
- [pai-recon-skill](https://github.com/danielmiessler/PAI/tree/main/Packs/pai-recon-skill)
