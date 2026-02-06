---
title: "PAI FirstPrinciples Skill：イーロン・マスク式の第一原理思考"
emoji: "💡"
type: "tech"
topics: ["ai", "claude", "pai", "thinking"]
published: true
---

## FirstPrinciples Skill とは

[PAI](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure) の分析手法パック。イーロン・マスクが使う **第一原理思考** を AI に実装したもの。

「みんながやってるから」ではなく、物事を **根本から分解して考え直す**。

## 問題：類推による思考

人間（と AI）は往々にして「類推」で考える：

```
「バッテリーは高い」
  → なぜ？
「昔からそうだから」「市場価格だから」
```

これは **思考停止** だ。既存の前提を疑わず、改善の余地を見逃す。

## 解決：3ステップフレームワーク

```
DECONSTRUCT → CHALLENGE → RECONSTRUCT
   分解          挑戦         再構築
```

### 1. DECONSTRUCT（分解）

「これは本当は何でできている？」

```markdown
## バッテリーの分解

| 構成要素 | コスト | 本当に必要？ |
|---------|--------|------------|
| コバルト | $XX/kg | 代替可能？ |
| リチウム | $XX/kg | 代替可能？ |
| 製造 | $XX | 効率化可能？ |
| 流通 | $XX | 直販なら？ |
```

### 2. CHALLENGE（挑戦）

「これは本当の制約？それとも思い込み？」

制約を3種類に分類：

| 種類 | 意味 | 例 |
|------|------|-----|
| **Hard** | 物理法則、変えられない | 重力、熱力学 |
| **Soft** | 選択、変えられる | ビジネスモデル、製造方法 |
| **Assumption** | 検証されていない前提 | 「市場価格」「業界標準」 |

多くの「制約」は実は **Assumption** だ。

### 3. RECONSTRUCT（再構築）

「真実だけから、最適解を作る」

Hard 制約だけを前提に、ゼロから最適解を設計：

```markdown
## バッテリーの再構築

- コバルトフリーの化学組成を採用
- 垂直統合で製造コスト 40% 削減
- 直販モデルで流通コスト 0

結果: 従来の 1/3 のコスト
```

## 使用例

### 「バッテリーは高い」を分析

```
User: "バッテリーのコスト問題を first principles で分析して"

AI:
## DECONSTRUCT
バッテリーの実際のコスト構成...

## CHALLENGE
- 「コバルトが必要」→ Assumption（代替化学組成が存在）
- 「製造は専門業者」→ Soft（垂直統合可能）
- 「エネルギー密度限界」→ Hard（物理法則）

## RECONSTRUCT
コバルトフリー + 自社製造で...
```

### 「採用は難しい」を分析

```
User: "エンジニア採用の難しさを first principles で"

AI:
## DECONSTRUCT
採用の構成要素: 認知、応募、選考、オファー...

## CHALLENGE
- 「大企業には勝てない」→ Assumption
- 「給与が決め手」→ Assumption（実際は成長機会）
- 「採用担当が必要」→ Soft

## RECONSTRUCT
...
```

## THE ALGORITHM との統合

[THE ALGORITHM](https://zenn.dev/yasuhito/articles/pai-the-algorithm) の STANDARD 以上で使用可能：

```
THINK フェーズ
    ↓
FirstPrinciples で前提を疑う
    ↓
ISC に「検証すべき前提」を追加
```

## よくある発見

FirstPrinciples を使うと、こんなことがよく見つかる：

1. **「業界標準」は誰も検証していない**
2. **コストの大部分は中間業者**
3. **「できない」は「やったことがない」の言い換え**
4. **技術的制約と思っていたものがビジネス制約**

## まとめ

FirstPrinciples Skill は PAI の根本思考ツール：

- **DECONSTRUCT** で構成要素に分解
- **CHALLENGE** で制約を Hard/Soft/Assumption に分類
- **RECONSTRUCT** で真実だけから最適解を構築

「当たり前」を疑い、本質から考え直す。

## 参考リンク

- [PAI GitHub](https://github.com/danielmiessler/PAI)
- [pai-firstprinciples-skill](https://github.com/danielmiessler/PAI/tree/main/Packs/pai-firstprinciples-skill)
