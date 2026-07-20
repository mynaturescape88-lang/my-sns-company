---
name: caption-review
disable-model-invocation: true
description: >
  instagram-captionの工程「キャプション」のレビュー時に発動。親skill instagram-caption のキャプション執筆後から呼ばれる成果物レビュー（木の内側の子ノード）。アカウント分岐（@mynaturescape＝絵文字/フッター/訴求なし・一人称の刺さる言葉・ハッシュタグ1〜2個／@myraisingdiary＝【〇日目】フォーマット・英語タグ含む5個・個人特定情報なし）・ショップ投稿の地名タグ/フッター種別/カルーセル構成を1skill内で分岐して合否判定する。
---

# caption-review（Instagramキャプションの成果物レビュー）

Instagramキャプションパイプラインの **キャプション** 専用の成果物レビュー。親 `instagram-caption` のキャプション執筆後から発動され、対象アカウントの方針どおりかをアカウント分岐で合否判定する（Layer2＝工程固有の作り込み合否）。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝キャプション固有の合格ライン・アカウント分岐）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の7観点・★観点7＝権利・合法性）
- ③ 対象アカウントの `.company/sns_accounts/instagram_mynaturescape.md` **または** `instagram_myraisingdiary.md`（対象1枚のみ＝`skill-routing.md`で特定）

## フロー（発動＝この順で全実行・fail-closed）
- ① まず対象アカウント（@mynaturescape / @myraisingdiary）と投稿種別（ショップ/自宅植物/子育て）を確定
- ② 本skillの観点（共通＋該当アカウント分岐）を成果物に1つずつ照合（pass/fail）
- ③ `pm/review-baseline.md` の7観点を1つずつ照合（★観点7＝権利・合法性はfail-closed／特に観点4＝対象アカウント戦略と相違ないか）
- ④ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ⑤ 全passのみ提出（投稿は別＝既存キャプション更新はアプリ手動編集のみ有効）

## キャプションの観点（合格ラインを内包・1つずつ照合）

### 共通
- [ ] 投稿前に実物アカウントの内容を読んだか（ラベル/プロファイルだけで想像していないか）[[feedback_view_real_account_content_before_creating]]
- [ ] 公開コンテンツに Unicode絵文字を使っていないか（@mynaturescapeは特に絵文字なしが必須）[[feedback_no_emoji_use_svg_icons]]
- [ ] 既存投稿のキャプション更新はアプリ手動編集のみ有効と理解しているか（APIはsuccess偽装で未反映）[[feedback_instagram_caption_api_limit]]

### @mynaturescape（植物・ショップ/自宅）
- [ ] 絵文字なし・フッターなし・訴求なし・ハッシュタグは植物名等1〜2個のみか [[feedback_instagram_caption_strategy]]
- [ ] 一人称の体験・感情・観察が入り「刺さる言葉」（グッ/ハッ/共感）があるか（自動生成的な均一文体でない）[[feedback_instagram_caption_strategy]]
- [ ] ショップ投稿：地名タグ1つ・フッター種別（YouTube動画あり=「YouTubeにて詳細を公開」／なし=「植物ブログにて詳細を公開」）が正しいか （新規）
- [ ] カルーセル：1枚目「Instagram用サムネイル.png」固定・最大10枚（API上限10枚・11枚以上は400失敗）・構成順（サムネ→外観→店頭→観葉→店内）を守ったか [[reference_instagram_carousel_api_10_limit]]

### @myraisingdiary（子育て）
- [ ] 【〇日目】フォーマットを維持したか （新規）
- [ ] ハッシュタグは英語タグ（#baby等）を含む5個（#育児記録 #赤ちゃんのいる暮らし #baby＋月齢タグ＋#赤ちゃん動画）か（外国人視聴者多数）[[feedback_view_real_account_content_before_creating]]
- [ ] 子どもの顔・個人を特定できる情報の映り込み/記載がなく保護者配慮の表現か （新規）
- [ ] 長すぎる日記captionにしていないか（反応薄）[[feedback_view_real_account_content_before_creating]]

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
