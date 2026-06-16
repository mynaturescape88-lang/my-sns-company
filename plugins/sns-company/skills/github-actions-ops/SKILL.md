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
2. 下記ワークフロー一覧で対象WFの責務・スケジュールを確認してから着手する。

> このスキルは GitHub Actions の**運用・保守専用**。技術資産インベントリ全体は project-infrastructure を使う。

---

## GitHub Actionsの注意点

- `[skip ci]` タグをコミットメッセージに含めると再トリガーを防げる
- `threads_posted_ids.txt` は投稿済みID管理ファイル。コミットで永続化する
- **一回限りの一括処理ワークフローが完了したら、scheduleを無効化してworkflow_dispatchのみ残す** — 再利用の可能性があるため削除はしない。無効化の際はコメントで理由と再利用方法を記載する

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
