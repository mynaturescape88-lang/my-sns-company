---
name: youtube-operations
description: >
  YouTube概要欄更新・YouTube API実装時に発動。『YouTube概要欄』『YouTube更新ツール』『Shorts リンク』『クォータ』等で発動。Shorts非クリック・プロフィール誘導出し分け・raising_captions.json共有・クォータgraceful-skip等を内包。
---

# YouTube概要欄・API実装スキル v1.0

YouTube概要欄を更新する、またはYouTube APIを実装するときに発動する。
**目的**：se/CLAUDE.md にルールを書いて毎回読む方式をやめ、YouTube概要欄/APIタスクのときだけ本スキルを発動して全ルールを過不足なく適用する。

## 発動時に必ず行う（発動＝適用）

1. 下記の技術メモ（Shorts非クリック・プロフィール誘導の出し分け・raising_captions.json共有・クォータgraceful-skip）を全て適用する。
2. プラットフォーム挙動は推測で断定せず、実機/Web確認すること。

> このスキルは YouTube概要欄/API実装**専用**。

---

## YouTube概要欄の技術メモ（2026-06-05 FB反映）

- **YouTube Shortsは概要欄・コメントのURLが非クリック**（2023-08-31〜のスパム対策・2026も継続）。通常（長尺・61秒以上）動画はクリック可能。**プラットフォーム挙動を推測で断定せず、実機/Web確認すること**（今回「YouTubeなら直URLがクリック可」と誤断定した）。
- Shortsで外部誘導するには **チャンネルのプロフィールリンク**（クリック可）を使い、概要欄は「プロフィールのリンクから」と書く。`youtube_description_updater.py` は Shorts に `TEMPLATE_*_SHORT`（プロフィール誘導）、長尺に直URLを出し分ける。`_needs_update` は直URLのShortsを変換対象に含め、プロフィール化済みは冪等にスキップ。
- **`raising_captions.json` は Instagram投稿（instagram_shorts_scheduler.py）と共有**。custom captionはそのままIGキャプションに使われるため、**IG形式（bio誘導）で保持**し、YouTube適用は `youtube_raising_diary_updater.py` の `_to_youtube` が変換する。**jsonを直URL化してコミットしないこと**（IG投稿が壊れる）。
- YouTube APIは1日10,000ユニット・更新1本=50ユニット。クォータ枯渇時は `_is_quota_error()` でgraceful-skip（失敗にせず正常終了→翌日自動再開）。
