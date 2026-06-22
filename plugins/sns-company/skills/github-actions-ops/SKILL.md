---
name: github-actions-ops
description: >
  GitHub Actionsワークフローの設計・保守・障害対応時に発動。『ワークフローが失敗』『GitHub Actions』『スケジュール実行』『WF保守』等で発動。skip ci・schedule無効化方針・WF一覧を内包。
---

# GitHub Actions 運用・保守スキル v1.0

GitHub Actionsワークフローの設計・保守・障害対応をするときに発動する。
**目的**：se/CLAUDE.md にルールを書いて毎回読む方式をやめ、GitHub Actions保守タスクのときだけ本スキルを発動して全ルールを過不足なく適用する。

## 発動時に必ず行う（発動＝適用）

1. 下記の注意点（skip ci・永続化ファイル・schedule無効化方針）を適用する。
2. push系WF・監視WFを触るときは **権限・push・watchdog の運用則** を必ず適用する。
3. 下記ワークフロー一覧で対象WFの責務・スケジュールを確認してから着手する。

> このスキルは GitHub Actions の**運用・保守専用**。技術資産インベントリ全体は project-infrastructure を使う。

---

## GitHub Actionsの注意点

- `[skip ci]` タグをコミットメッセージに含めると再トリガーを防げる
- `threads_posted_ids.txt` は投稿済みID管理ファイル。コミットで永続化する
- **一回限りの一括処理ワークフローが完了したら、scheduleを無効化してworkflow_dispatchのみ残す** — 再利用の可能性があるため削除はしない。無効化の際はコメントで理由と再利用方法を記載する

---

## 権限・push の運用則（最小権限の明示宣言）

- **既定トークンは read-only**（plant-blog-bots は2026-06-04頃に read-write→read へ切替）。`permissions` 未宣言で `git push` するWFは 403（`Write access to repository not granted`）で失敗する。**`git push` するWFには必ずジョブに `permissions:` → `contents: write` を明示**する。既定をread-writeに戻さず最小権限の明示宣言を採用（なぜ切替わったか不明＝広く緩めない）。
- **`permissions:` を明示すると未記載スコープは自動 none になる**。よって用途ごとに必要スコープを全部書く ＝ push系=`contents: write`／**監視系(watchdog)=`actions: read` も必須**（無いと `gh run list` が403→検知ゼロ。後述）。
- 自動pushは並行push・main進行で non-fast-forward（`! [rejected] (fetch first)`）になる → `git pull --rebase origin main` を挟みリトライする。
- **`.gitignore` 除外ファイルをステージングするときは `git add -f`**。親ディレクトリが除外（例 `shop_images/`）されていると否定ルール（`!shop_images/**/pending.json`）は機能しない＝Gitの制限。`git add shop_images/*/pending.json` は `FileNotFoundError` で失敗するので `git add -f shop_images/*/pending.json` にする。

---

## 自己修復監視 watchdog の運用則

- **通知は「未解決＝オーナー判断が要る障害」だけ**飛ばす。失敗しても後続/リトライで成功したものは通知しない（ノイズ防止）。`monitor_config.yml` の `group:` で本体＋リトライをまとめ、`watchdog.detect()` が「失敗より後にグループ内成功あり＝resolved(自己解決)」を判定→resolved失敗は通知せず台帳に「自己解決」記録。未解決は同一グループで1通に集約。欠落・自動修復失敗・未知シグネチャは従来どおり通知。正本＝`task_logs/wf-self-healing-design.md §9`。
- **watchdog WF には `actions: read` を必ず付ける**。`ops/watchdog.py` は `gh run list` でWF履歴を取得して検知するが、`contents: write` だけだと `actions: read` が無く `gh run list` が403→exit1。`gh_json()` は例外を握りつぶし `[]` を返す設計なので、**全WFで検知0件・通知0件のまま静かに目隠し（沈黙故障）**になる（INC-A7／2026-06-15発覚）。`permissions:` 明示時は監視系スコープを書き忘れない。
- インシデント対応時の台帳同期（シグネチャ源YAML＝`monitor_config` と INCIDENT台帳は別物・両方更新）は技術定数としてMEMORY `reference_watchdog_config_ledger_sync` 側に温存。

---

## GitHub Actions ワークフロー一覧

| ファイル | スケジュール | 用途 |
|---|---|---|
| `jp_weekly.yml` | 火・土 21:00 JST | 国内ブログ記事自動投稿 |
| `overseas_weekly.yml` | 木 21:00 JST | 海外ブログ記事自動投稿 |
| `instagram_raising_daily.yml` | 毎日 07:00・21:00 JST | @myraisingdiary自動投稿 |
| `instagram_raising_retry.yml` | 毎日 08:00・22:00 JST | @myraisingdiary投稿リトライ |
| `youtube_description_update.yml` | 毎日 17:00 JST | YouTube概要欄更新 |
| `threads_on_publish.yml` | WordPress公開トリガー | Threads自動投稿 |
| `rakuten_affiliate_update.yml` | 毎月1日 09:00 JST | 楽天アフィリエイト更新 |
