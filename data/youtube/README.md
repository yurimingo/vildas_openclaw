# baton YouTube DB

baton関連YouTubeチャンネルの動画解析データを管理するファイルベースDB。

## 構造

```
data/youtube/
  channels.json        # チャンネル一覧（手動管理）
  videos.jsonl         # 動画メタデータ（1行1件）
  transcripts/
    {video_id}.json    # 動画ごとの解析結果
  README.md
```

## channels.json

baton関連チャンネルの一覧。フィールド：
- `group`: "QuizKnock" | "AlmaZarQ"
- `type`: "group"（グループ公式）| "member"（メンバー個人）
- `verified`: URL確認済みかどうか
- `note`: 補足情報

## videos.jsonl

1行1動画のJSONL形式。
```json
{"video_id":"xxx","channel_id":"quizknock-main","title":"...","published_at":"2025-01-01","analyzed_at":"2026-02-20","has_transcript":true}
```

## transcripts/{video_id}.json

Gemini APIによる動画解析結果。
```json
{
  "video_id": "3i-SH93mr0E",
  "channel_id": "quizknock-main",
  "title": "...",
  "performers": ["鶴崎修功", "東言"],
  "summary": "...",
  "transcript": [
    {"time": "00:00", "text": "..."}
  ],
  "raw": "（Geminiの生出力テキスト）",
  "analyzed_at": "2026-02-20",
  "model": "gemini-3.1-pro-preview"
}
```

## 解析の実行

```bash
# 1本解析
python3 tools/youtube_analyze.py https://www.youtube.com/watch?v=xxxxx

# DBに保存（tools/youtube_db.py で管理予定）
python3 tools/youtube_db.py analyze https://www.youtube.com/watch?v=xxxxx --channel-id quizknock-main
```

## 検索

```bash
# テロップからキーワード検索
grep -r "ヒルベルト" data/youtube/transcripts/

# 動画一覧確認
cat data/youtube/videos.jsonl | python3 -m json.tool
```

## 現在のDB状況

- チャンネル: `channels.json` 参照
- verified=false のチャンネルは要確認
- こうちゃん（河村拓哉）はQK卒業済み。含めるか要確認
