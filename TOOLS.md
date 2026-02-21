# TOOLS.md - Local Notes

Skills define _how_ tools work. This file is for _your_ specifics — the stuff that's unique to your setup.

## What Goes Here

Things like:

- Preferred tools and configurations
- Environment-specific settings
- Device nicknames
- Anything environment-specific

## Environment

- **OS:** macOS
- **Platform:** Slack (via OpenClaw connector)

## Slack フォーマット

Slack はマークダウンと書式が異なる。以下に注意：

- 太字: `*太字*`（アスタリスク1個） ※`**太字**` は効かない
- 斜体: `_斜体_`
- 取り消し線: `~取り消し~`
- コード: バッククォートはそのまま使える
- 水平線 `---` は効かない（使わない）
- テーブルは使わない（崩れる）

---

## Gemini Google Search Grounding

`GOOGLE_API_KEY` が `.env` に設定済み。深掘り調査が必要な時に使う。

```bash
python3 /workspace/tools/gemini_search.py "クエリ"
```

または Python コードから `from gemini_search import gemini_search` で直接呼び出し可。

- モデル: `gemini-3.1-pro-preview`（デフォルト）
- grounding: `googleSearch` を有効化
- ストリーミング不使用（`generateContent`）

---

Add whatever helps you do your job. This is your cheat sheet.

---

## Slack チャンネルへの直接投稿（スレッド外）

`sessions_send` はサンドボックスからブロックされる。スレッド外のチャンネル投稿は Slack API を直接叩く。

```bash
TOKEN=$(grep SLACK_BOT_TOKEN /workspace/.env | cut -d= -f2)
curl -s -X POST https://slack.com/api/chat.postMessage \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json; charset=utf-8" \
  -d '{
    "channel": "CHANNEL_ID",
    "text": "メッセージ本文"
  }'
```

- `thread_ts` を指定しない → チャンネルのトップレベルに投稿
- `thread_ts` を指定する → スレッドリプライとして投稿
- チャンネルID一覧:
  - `#vil_botan_lab`: `C0AFYPKLTK5`
