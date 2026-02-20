# HEARTBEAT.md

## 定期タスク

### セッション開始時（毎回）
- `git pull` で最新の状態に同期する
  ```bash
  git pull
  ```
  失敗した場合は TOOLS.md の git 操作方法を確認する

### ファイル編集後
- 変更をコミットしてpushする
  ```bash
  git add -A
  git commit -m "Update: [変更内容の概要]"
  git push
  ```
  コミットメッセージは変更内容がわかるように書く（例: "Update SOUL.md - 性格の核を更新"）
