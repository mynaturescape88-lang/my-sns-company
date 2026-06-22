---
name: youtube-operations
description: >
  YouTube概要欄更新・YouTube API実装時に発動。『YouTube概要欄』『YouTube更新ツール』『Shorts リンク』『クォータ』等で発動。Shorts非クリック・プロフィール誘導出し分け・raising_captions.json共有・クォータgraceful-skip等を内包。
---

# YouTube概要欄・API実装スキル v1.0

YouTube概要欄を更新する、またはYouTube APIを実装するときに発動する。
**目的**：se/CLAUDE.md にルールを書いて毎回読む方式をやめ、YouTube概要欄/APIタスクのときだけ本スキルを発動して全ルールを過不足なく適用する。

## 発動時に必ず行う（発動＝適用）

1. 下記の各手順（概要欄正本構成・タグ一括の安全手順・Bot判定時対応・Shorts非クリック/クォータgraceful-skip等の技術メモ）を全て適用する。
2. プラットフォーム挙動は推測で断定せず、実機/Web確認すること。

> このスキルは YouTube概要欄/API実装**専用**。

---

## 概要欄（キャプション）正本構成 — 提案/変更前に必ず参照

- 長尺（植物ショップ巡り・顔出しなしBGM）の概要欄正本構成は **2026-06-15「beans寄せ新フォーマット」で確定**。オーナー明言で**しばらく変更しない／変更は要再承認**（旧2026-06-14版＝絵文字なし/300字は廃止・上書き）。提案・変更前に必ず読む（蒸し返し防止）。
- **9点の確定並び順の全文＝[references/youtube-caption-canonical.md](references/youtube-caption-canonical.md)**（KWフック→挨拶→店紹介→おすすめ→みどころ章→あわせて見たい→このch固定文→フッター→ハッシュタグ）。1行ずつの規約・捏造禁止/〒なし/植物章3行未満は省略まで同ファイルに集約。
- **ツール**＝`seo_growth/youtube_caption_build.py`（統合config→生成・冪等・dry-run/--apply/--backup）。日次WFフッター（`youtube_description_updater.py`）とブログ行マーカーで整合・二重更新防止。
- **実施状況**：<2,000の85本へ一括push＋実機検証済。**>2,000の伸び動画+自宅4本は未決＝要協議**でタスク未クローズ。

---

## タグ一括投入の安全手順（非破壊）

- **概要欄を絶対に変えない**：`youtube_tag_updater.py apply` は `videos().update(part="snippet")` でtitle/description/categoryId/tagsを丸ごと送り直す＝原理上descriptionを巻き込む。**カナリア検証必須**＝1本だけ本適用→`description`をsha256でバイト照合し完全一致＋タイトル不変を確認してから残りを流す。
- **タグは消さず足す**：applyはタグ全置換なので、生成側で既存タグを必ず再フェッチして温存マージ（季節/動画固有タグを失わない）。ツール＝`seo_growth/youtube_longform_tags_build.py`（決定論・冪等・`--yes`で何度流しても非破壊）。上限15個/合計500字。
- **クォータ**：videos.update=50ユニット/本＝1日10,000枠で約200本/日。超過はgraceful-skip→日次WF `youtube_longform_retrofit.yml`（17:00 JST）が冪等drain。
- read-after-writeラグあり＝apply直後の再取得は旧タグのことがある（数秒待つ）。タグのSEO寄与は2026は小〜中だが無料・非破壊なので「やって損なし」枠。

---

## GitHub Actions経由のYT DLはBOT判定される → 事前DL方式が正解

- **機構**：GitHub Actions（Azureデータセンター）IPからYouTubeへアクセスすると、Cookie認証を入れても数時間以内にGoogleがセッションを無効化しBOT判定（`Sign in to confirm you're not a bot`）。初回成功しても次回は必ず失敗する。**この設計（Actionsで動画DL）は採用しない**。
- **正解**：ローカルMacで事前DL→Cloudinary保存→GitHub ActionsはCloudinaryから取得（`raising_captions.json`に`cloudinary_url`を持たせる）。@myraisingdiaryショート自動投稿（`instagram_raising_daily.yml`／`common/instagram_shorts_scheduler.py`）はこの方式。
- **暫定**：`_download_video()`に最大3回リトライ（遅延付き）＋失敗時graceful-skip（未投稿をログ記録しWF継続）。一時緩和時のみ自動回復・根本解決ではない。
- **Cloudinary実装の落とし穴**：`uploader.upload()`を呼ぶ関数だけでなく`uploader.destroy()`を呼ぶ関数にも`cloudinary.config()`を入れる（プリDL経路は`upload()`を飛ばすため`config()`未設定で`destroy()`がエラーになる）。
- **投稿本数の事前確認**：スケジューラ実行前に必ずdry-run等で「次に投稿される動画と本数」をオーナーに確認してから実行（default batch=1でも何が出るか伝える）。

---

## YouTube概要欄の技術メモ（2026-06-05 FB反映）

- **YouTube Shortsは概要欄・コメントのURLが非クリック**（2023-08-31〜のスパム対策・2026も継続）。通常（長尺・61秒以上）動画はクリック可能。**プラットフォーム挙動を推測で断定せず、実機/Web確認すること**（今回「YouTubeなら直URLがクリック可」と誤断定した）。
- Shortsで外部誘導するには **チャンネルのプロフィールリンク**（クリック可）を使い、概要欄は「プロフィールのリンクから」と書く。`youtube_description_updater.py` は Shorts に `TEMPLATE_*_SHORT`（プロフィール誘導）、長尺に直URLを出し分ける。`_needs_update` は直URLのShortsを変換対象に含め、プロフィール化済みは冪等にスキップ。
- **`raising_captions.json` は Instagram投稿（instagram_shorts_scheduler.py）と共有**。custom captionはそのままIGキャプションに使われるため、**IG形式（bio誘導）で保持**し、YouTube適用は `youtube_raising_diary_updater.py` の `_to_youtube` が変換する。**jsonを直URL化してコミットしないこと**（IG投稿が壊れる）。
- YouTube APIは1日10,000ユニット・更新1本=50ユニット。クォータ枯渇時は `_is_quota_error()` でgraceful-skip（失敗にせず正常終了→翌日自動再開）。
