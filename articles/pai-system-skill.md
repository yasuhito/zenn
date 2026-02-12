---
title: "PAI System Skill：PAI 環境のメンテナンスと整合性チェック"
emoji: "🔧"
type: "tech"
topics: ["ai", "claude", "pai", "maintenance"]
published: true
---

## System Skill とは

[PAI](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure) の環境メンテナンスパック。PAI ディレクトリの **整合性チェック、シークレットスキャン、セッション記録** などを行う。

## 5 つのワークフロー

### 1. IntegrityCheck（整合性チェック）

壊れた参照や不整合を発見・修復：

```
User: "PAI の整合性をチェックして"
```

チェック項目：

| 項目 | 説明 |
|------|------|
| スキル参照 | 存在しないスキルへの参照 |
| ファイルリンク | 壊れた相対パス |
| Front Matter | YAML の構文エラー |
| 命名規則 | TitleCase 違反 |
| 循環参照 | 無限ループする参照 |

出力例：

```markdown
## IntegrityCheck Report

### エラー（修正必要）
❌ CORE/SYSTEM/ROUTING.md: 'OldSkill' への参照が無効
❌ Skills/MySkill/SKILL.md: Front Matter の構文エラー

### 警告（推奨）
⚠️ Skills/myskill/: 'MySkill' にリネーム推奨

### 自動修復可能
- 壊れた参照 2 件を削除

自動修復を実行しますか？ [y/N]
```

### 2. DocumentSession（セッション記録）

現在のセッションをドキュメント化：

```
User: "このセッションを記録して"
```

生成されるもの：

```markdown
## Session: 2026-01-29 10:30

### 概要
PAI 記事の執筆と公開

### 実行したタスク
1. THE ALGORITHM の解説記事を作成
2. Hook System の解説記事を作成
3. ...

### 学び
- Zenn にはレートリミットがある
- テーブルはコードブロック外に

### 未完了
- [ ] 残りのパック記事
```

[TELOS Skill](https://zenn.dev/yasuhito/articles/pai-telos-skill) の LEARNED.md にも追記される。

### 3. SecretScanning（シークレットスキャン）

シークレットの漏洩をチェック：

```
User: "シークレットスキャンして"
```

検出対象：

| パターン | 例 |
|---------|-----|
| API キー | `sk-...`, `AKIA...` |
| トークン | `ghp_...`, `xoxb-...` |
| パスワード | `password=...` |
| 秘密鍵 | `-----BEGIN RSA PRIVATE KEY-----` |

```markdown
## SecretScan Report

### 検出（3 件）

| ファイル | 行 | 種類 |
|---------|-----|------|
| config.yaml | 15 | AWS Access Key |
| .env.example | 8 | OpenAI API Key（例示？） |
| notes.md | 42 | Slack Token |

### 推奨アクション
1. config.yaml の AWS キーを環境変数に移動
2. notes.md から Slack トークンを削除
3. git history からも削除（git filter-branch）
```

### 4. GitPush（安全な Git Push）

シークレットスキャン付きの git push：

```
User: "変更を push して"
```

手順：
1. シークレットスキャンを実行
2. 問題がなければ `git add -A && git commit && git push`
3. 問題があれば警告して停止

```markdown
## GitPush

### Pre-push チェック
✅ シークレットスキャン: クリア
✅ 大きなファイル: なし
✅ 禁止ファイル: なし

### 実行
```bash
git add -A
git commit -m "feat: add new skill"
git push origin main
```

✅ Push 完了
```

### 5. WorkContextRecall（作業コンテキスト検索）

過去の作業を検索：

```
User: "前に Zenn 記事を書いたときの設定を思い出して"
```

検索対象：
- セッション記録
- [MEMORY](https://zenn.dev/yasuhito/articles/pai-core-install) ディレクトリ
- ISC 履歴

```markdown
## WorkContextRecall: "Zenn 記事"

### 関連セッション（3 件）

**2026-01-15**
- 記事スキルの作成
- 口調: だ/である調に統一

**2026-01-10**
- 初めての Zenn 記事公開
- slug の命名規則を学んだ

### 関連設定
- ~/.clawdbot/skills/zenn-article/SKILL.md
- ~/Work/zenn/articles/
```

## 定期実行

System Skill のワークフローは定期実行を推奨：

| ワークフロー | 頻度 |
|-------------|------|
| IntegrityCheck | 週 1 回 |
| SecretScanning | 毎 push 前 |
| DocumentSession | 重要なセッション後 |

[Hook System](https://zenn.dev/yasuhito/articles/pai-hook-system) で SessionEnd に自動実行を設定可能。

## まとめ

System Skill は PAI のメンテナンス：

- **IntegrityCheck** で壊れた参照を発見
- **SecretScanning** で漏洩を防止
- **DocumentSession** でセッションを記録
- **GitPush** で安全にコミット
- **WorkContextRecall** で過去を検索

PAI 環境を健全に保つ。

## 参考リンク

- [PAI GitHub](https://github.com/danielmiessler/PAI)
- [pai-system-skill](https://github.com/danielmiessler/PAI/tree/main/Packs/pai-system-skill)
