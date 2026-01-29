---
title: "PAI Voice System：AI に声を与える TTS 通知サーバー"
emoji: "🔊"
type: "tech"
topics: ["ai", "claude", "pai", "tts"]
published: true
---

## Voice System とは

[PAI](https://zenn.dev/yasuhito/articles/pai-personal-ai-infrastructure) に音声フィードバック機能を追加するパック。AI エージェントの応答を **ElevenLabs TTS で読み上げる**。

作業中に画面を見ていなくても、タスク完了や重要な通知を音声で知らせてくれる。

## なぜ音声通知か

AI エージェントはサイレントに動作する。長時間タスクが終わっても、ターミナルを見ていなければ気づかない。

Voice System があれば：
- タスク完了を音声で通知
- 画面から目を離していても状況がわかる
- 複数エージェントの完了を聞き分けられる

## アーキテクチャ

```
[Stop Hook] → [Prosody Enhancer] → [Voice Server :8888] → [ElevenLabs API] → [Audio]
```

1. **Stop Hook** が応答完了を検出
2. **Prosody Enhancer** が感情マーカーを追加（自然な発話のため）
3. **Voice Server** が HTTP リクエストを受信
4. **ElevenLabs API** で音声生成
5. **ffplay** で再生

## エージェントごとの声

[Agents Skill](https://zenn.dev/yasuhito/articles/pai-agents-skill) と連携し、エージェントごとに異なる声を使う：

| エージェント | 声 | 特徴 |
|-------------|-----|------|
| Engineer | Marcus | 落ち着いた技術者 |
| Architect | Serena | 知的で威厳 |
| Designer | Aditi | 温かみのある |
| Pentester | Domi | 鋭い |
| Intern | Young voice | 元気 |

誰が話しているか、声で判別できる。

## Prosody Enhancement

単にテキストを読み上げるのではなく、自然な発話になるよう調整：

```typescript
// Before
"タスクが完了しました"

// After (Prosody markers added)
"タスクが... 完了しました！"
```

ElevenLabs の SSML 機能を活用し、間（ポーズ）や抑揚を追加。

## セットアップ

### 1. Voice Server 起動

```bash
./start.sh  # サーバー起動
./status.sh # 状態確認
./stop.sh   # 停止
```

### 2. 環境変数

```bash
# .env
ELEVENLABS_API_KEY=your_key_here
DEFAULT_VOICE_ID=voice_id_here
```

### 3. フック有効化

[Hook System](https://zenn.dev/yasuhito/articles/pai-hook-system) で Stop フックを有効化すると、自動で音声通知が動く。

## macOS 対応

ElevenLabs API キーがない場合、macOS の `say` コマンドにフォールバック：

```bash
say "タスクが完了しました"
```

品質は劣るが、とりあえず動作する。

## Menu Bar アプリ

macOS 用のメニューバーアプリも含まれている：

- サーバーの起動/停止
- 音量調整
- 声の切り替え
- 最近の通知履歴

## 使用シーン

### 長時間タスクの完了通知

```
User: "このリポジトリをリファクタリングして"
...（数分後）...
Voice: "リファクタリングが完了しました"
```

### マルチエージェント作業

```
Voice (Engineer): "実装が完了しました"
Voice (QATester): "テストが通りました"
Voice (Architect): "レビュー完了です"
```

声で誰が終わったかわかる。

### エラー通知

```
Voice: "エラーが発生しました。API キーが無効です"
```

問題があればすぐに気づける。

## まとめ

Voice System は PAI の音声フィードバック：

- **ElevenLabs TTS** で高品質な音声
- **エージェントごとの声** で識別可能
- **Prosody Enhancement** で自然な発話
- **macOS say フォールバック** でキーなしでも動作

AI が「声を持つ」ことで、人間との協働がよりスムーズになる。

## 参考リンク

- [PAI GitHub](https://github.com/danielmiessler/PAI)
- [pai-voice-system](https://github.com/danielmiessler/PAI/tree/main/Packs/pai-voice-system)
- [ElevenLabs](https://elevenlabs.io/)
