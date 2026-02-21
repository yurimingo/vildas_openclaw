---
name: lunchtime-hello
description: 平日の昼休み（12:30〜12:59 JST）に、まだ話したことのないチャンネルメンバー1人に話しかける社交スキル。
---

# Lunchtime Hello スキル

平日のお昼休みに、`#vil_botan_lab` にいてまだUser情報のないメンバー1人にぼたんから話しかける。

## トリガー

heartbeat から自動呼び出し（平日 12:30〜12:59 JST に一度だけ）。

## 手順

1. **今日の日付・時刻を確認する**（`session_status` で取得）

2. **実行条件チェック**
   - 現在時刻が 12:30〜12:59 JST であることを確認
   - 今日が平日（月〜金）であることを確認
   - `https://holidays-jp.github.io/api/v1/date.json` を `web_fetch` で取得し、今日が祝日でないことを確認
   - `skills/lunchtime-hello/log/YYYY-MM-DD.md` が存在しない（今日まだ実行していない）ことを確認
   - どれか1つでも満たさない場合 → スキップ

3. **対象者を選ぶ**
   - `users/` フォルダの既存ファイル一覧を確認（既知ユーザーのリスト）
   - `#vil_botan_lab`（C0AFYPKLTK5）にいるメンバーのうち、`users/` にファイルのない人を候補とする
   - 候補が複数いる場合は1人だけ選ぶ（ランダムまたは名前順の先頭）
   - 候補がいない場合 → ログに「今日は全員と話済み」と記録してスキップ

4. **情報収集**（軽く）
   - Slackツールなどで対象者の最近の投稿をざっくり確認（可能なら）
   - `web_search` などで無理に調べない（あくまで自然な範囲で）

5. **話しかける**
   - `#vil_botan_lab` に投稿（スレッドなし、チャンネル直接）
   - トーン：明るく、軽く、圧迫感なし。「はじめまして！」「ちょっと気になって」くらいの温度感
   - 無理に質問を重ねない。1〜2文で十分
   - Slack API を使ってチャンネルに直接投稿：
     ```bash
     TOKEN=$(grep SLACK_BOT_TOKEN /workspace/.env | cut -d= -f2)
     curl -s -X POST https://slack.com/api/chat.postMessage \
       -H "Authorization: Bearer $TOKEN" \
       -H "Content-Type: application/json; charset=utf-8" \
       -d '{"channel": "C0AFYPKLTK5", "text": "メッセージ本文"}'
     ```

6. **ログを残す**
   - `skills/lunchtime-hello/log/YYYY-MM-DD.md` を作成：
     ```markdown
     # YYYY-MM-DD のランチタイム挨拶

     - 対象: [名前 or Slack ID]
     - 投稿内容: [話しかけた内容]
     - 返事: あり / なし / 未確認
     ```

7. **git でコミット・push する**

## 注意点

- **深追いしない**: 返事がなくても再度話しかけない
- **1日1回まで**: ログファイルの存在で制御
- **祝日・土日はスキップ**: API で毎回確認
- **ぼたんらしく**: キャラを保ちつつ、無理に馴れ馴れしくしない
- 返事がなかった場合もログに「返事なし」と書いておく（次回の参考に）
