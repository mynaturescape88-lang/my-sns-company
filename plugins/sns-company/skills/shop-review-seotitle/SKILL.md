---
name: shop-review-seotitle
description: >
  shop-articleの工程「SEOタイトル・メタ」のレビュー時に発動。親skill shop-article のタイトル/メタ確定後・公開操作着手前から呼ばれる成果物レビュー（木の内側の子ノード）。変更履歴を確認したコロコロ変更回避（メタ=excerpt）・タイトル/メタ文字数・誇大でない結果数字ベースを合否判定する。
---

# shop-review-seotitle（SEOタイトル・メタの成果物レビュー）

ショップ紹介記事パイプラインの **SEOタイトル・メタ** 専用の成果物レビュー。親 `shop-article` のタイトル/メタ確定後・公開操作着手前から発動され、タイトルとメタが基準と履歴に適合するかを合否判定する（Layer2＝工程固有の作り込み合否）。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝SEOタイトル・メタ固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の6観点）
- ③ `.company/sns_accounts/blog.md`（shopの戦略・却下レバーの照合先）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（確定したタイトル/メタ）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の6観点を1つずつ照合（特に観点4＝blog.md戦略と相違ないか）
- ③ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ④ 全passのみ次工程（公開直前の総合レビュー）へ進める

## SEOタイトル・メタの観点（合格ラインを内包・1つずつ照合）
- [ ] SEOタイトル/メタは変更履歴を確認しコロコロ変えていないか（メタ=excerpt） [[feedback_seo_title_meta_no_flipflop]]
- [ ] タイトル25〜45字・メタ80〜160字か [[wordpress-publishing基準]]
- [ ] タイトルが結果・数字ベースで誇大でないか （新規）

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
