---
name: threads-review
disable-model-invocation: true
description: >
  threads-postの工程「投稿文」のレビュー時に発動。親skill threads-post の投稿文執筆後・オーナー提示前から呼ばれる成果物レビュー（木の内側の子ノード・小さい木）。記事タイトルそのまま使わず興味を引く導入文・ハッシュタグ規定（#観葉植物 #植物好きな人と繋がりたい＋内容に応じた1つ）・fediverse拡散前提で内輪向け表現を避ける・手動投稿はオーナー承認後を合否判定する。
---

# threads-review（Threads投稿文の成果物レビュー）

Threads投稿文パイプラインの **投稿文** 専用の成果物レビュー（小さい木）。親 `threads-post` の投稿文執筆後・オーナー提示前から発動され、投稿文が規定と拡散前提に適合するかを合否判定する（Layer2＝工程固有の作り込み合否）。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝投稿文固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の7観点・★観点7＝権利・合法性）
- ③ `.company/sns_accounts/threads.md`（Threadsの戦略・却下レバーの照合先）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（投稿文）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の7観点を1つずつ照合（★観点7＝権利・合法性はfail-closed／特に観点4＝threads.md戦略と相違ないか）
- ③ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ④ 全passのみオーナーへ提示（手動投稿は承認後・自動WFは除く）

## 投稿文の観点（合格ラインを内包・1つずつ照合）
- [ ] 記事タイトルをそのまま使わず、読者の興味を引く導入文にしたか （新規）
- [ ] ハッシュタグは「#観葉植物 #植物好きな人と繋がりたい＋記事内容に応じた1つ」か （新規）
- [ ] fediverse連携で外部SNSに拡散する前提を考慮し、内輪向け表現・限定情報を含めていないか （新規）
- [ ] 公開コンテンツに Unicode絵文字を使っていないか [[feedback_no_emoji_use_svg_icons]]
- [ ] 手動投稿は投稿文をオーナーへ提示し承認後に投稿する前提か（催促しない・自動WFは除く）[[feedback_go_signal_owner_only_no_soliciting]]

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
