---
name: instagram-posting-pipeline
description: >
  @mynaturescape等のInstagram投稿パイプライン（遡及投稿・スケジュール・画像事前キャッシュ）の実装・改修時に発動。『@mynaturescape投稿』『遡及投稿』『pending.json』『投稿経路』『選定画像』等で発動。データ破壊防止のマージ方式・投稿経路通し検証を内包。
---

# Instagram投稿パイプライン（技術/投稿経路）スキル v1.0

@mynaturescape等のInstagram投稿パイプライン（遡及投稿・スケジュール・画像事前キャッシュ）を実装・改修するときに発動する。
**目的**：se/CLAUDE.md にルールを書いて毎回読む方式をやめ、Instagram投稿経路の実装/改修タスクのときだけ本スキルを発動して全ルールを過不足なく適用する。

## 発動時に必ず行う（発動＝適用）

1. 既存ファイルを上書き初期化せず、マージ方式（読み込み→必要キーのみ更新→書き戻し）を徹底する。
2. 「完了報告」前に投稿経路を通しで検証する。
3. 選定画像はgit管理外＝投稿前に必ず事前キャッシュする。

> このスキルはInstagram投稿の**技術/投稿経路専用**。Instagramキャプション文の作成は別スキル instagram-caption を使う。

---

## データ破壊防止・投稿経路の通し検証（2026-06-05 FB転記）

### 既存ファイルを「上書き初期化」しない（マージ方式必須）
- スケジュール/一括設定スクリプトが既存 pending.json を `{approved, scheduled_date, order}` の最小形で**上書きし、wp_post_id・ig_image_urls・ig_caption・ig_image_files を消失**させた事故が発生（commit 432ce9d）。
- 既存JSON/設定を更新する際は必ず**読み込み→必要キーのみ更新→書き戻し**（マージ）。全置換は禁止。

### 「完了報告」前に投稿経路を通しで検証する
- pending.json の存在確認だけでは不十分。実際に投稿関数（`do_publish`/`do_auto_post`）が読むフィールド（wp_post_id・画像URL・キャプション）まで揃っているか、最小実行（アップロードURL取得・選定シミュレーション等）で確認してから完了とする。[[feedback_verify_after_fix]] 準拠。

### @mynaturescapeショップ遡及投稿の投稿経路（2026-06-05改修）
- `do_auto_post`：scheduled_date<=今日 かつ未投稿を古い順に1件。選定画像 `ig_image_files`（選定順）を `_resolve_selected_image_urls` でWPメディアにアップロード→公開URL化（`ig_image_url_map`にキャッシュ）→`post_carousel`。`needs_article:true` はスキップ。カルーセル上限20枚。
- 写真選定HTMLツール（caption_preview）はブラウザDOM/localStorageのみでディスク保存しない。要注意。

### ⚠️ 選定画像はgit管理外＝投稿前に必ず事前キャッシュが必要（2026-06-06 FB転記・障害対応 commit a61c2ac）
- `shop_images/` は `.gitignore` 除外で `pending.json` だけ追跡。**選定画像（image_NN.jpeg）はGitHub Actionsランナーに存在しない**ため、ランナーが `ig_image_files` をローカルから解決しようとすると「選定画像が見つかりません→投稿用画像URLが用意できません」で失敗する（ローカルMacには画像があり手元では動く食い違い）。6/6 the Farm 投稿が全失敗した。
- **対策（恒久）**：投稿前にローカルで `python jp/shop_article_generator.py --prepare-images`（特定ショップは `--shop "名前"`）を実行→選定画像をWPへアップロードし公開URLを `pending.json` の `ig_image_url_map`/`ig_image_urls` にキャッシュ→コミット&プッシュ。ランナーはキャッシュ済みURLで投稿（ローカル画像不要・冪等）。**新フォルダを選定したら都度 --prepare-images を実行すること**。
- `do_prepare_images` はフォルダ単位でエラーを握りつぶし途中経過を保存して継続（一過性のWPアップロードタイムアウトで全体停止しない／再実行で続きから）。`_resolve_selected_image_urls` は `url_map` を早期に `pending` へ紐づけ、失敗時も成功分を保持（二重アップロード防止）。
- **追跡済み pending.json のステージは `git add -u shop_images/`**。`git add 'shop_images/*'` は親ディレクトリ除外でignoreに弾かれる（[[github_actions_gitignore_fix]] 準拠）。
