---
name: affiliate-review-price
description: >
  affiliate-writingの工程「価格・数値・データ」のレビュー時に発動。親skill affiliate-writing の価格を扱う記事（子育て節約等）で本文確定後から呼ばれる成果物レビュー（木の内側の子ノード）。価格の正確さ最優先・既存ファイルの相場観プレースホルダ鵜呑み禁止・楽天API中央値の手動補正・¥0や桁違いの非混入・サマリー整合・同一商品の必要/不要両出し矛盾なしを合否判定する。
---

# affiliate-review-price（価格・数値・データの成果物レビュー）

アフィリエイト・収益系ライティングパイプラインの **価格・数値・データ** 専用の成果物レビュー。親 `affiliate-writing` の価格を扱う記事（子育て節約等）で本文確定後から発動され、価格と数値が実勢と整合し矛盾がないかを合否判定する（Layer2＝工程固有の作り込み合否）。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝価格・数値固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の6観点）
- ③ `.company/sns_accounts/rakuten.md`（楽天ROOM/アフィリの戦略・却下レバーの照合先）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（価格表・サマリー・マッピング）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の6観点を1つずつ照合（特に観点4＝rakuten.md戦略と相違ないか）
- ③ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ④ 全passのみ次工程（ASP提出/HTML確定）へ進める

## 価格・数値・データの観点（合格ラインを内包・1つずつ照合）
- [ ] 価格の正確さを最優先で検証したか（価格はマスター一元管理＝room-kosodate-mappingを正本にしたか）[[project_kosodate_savings_article_series]]
- [ ] 既存ファイルの「相場観／参考価格」をプレースホルダの可能性ありとして鵜呑みにしていないか [[feedback_affiliate_asp_article_rules]]
- [ ] 楽天API自動取得の中央値が安価アクセサリ/高級モデルを拾う点を価格帯floor/手動補正（PRICE_FIX）で実勢に合わせたか [[feedback_affiliate_asp_article_rules]]
- [ ] ¥0や明らかな桁違いの混入がないか・サマリー各値の整合（全部＝必要＋買わず）がとれているか [[feedback_affiliate_asp_article_rules]]
- [ ] 丸めで過小/過大表示（0.5万丸め等）になっていないか・非商品エントリ（選定・準備・移行検討等）を金額に混ぜていないか [[feedback_affiliate_asp_article_rules]]
- [ ] 同一/類似商品が必要側と不要側の両方に出る矛盾（重複・誤マッピング）がないか [[feedback_affiliate_asp_article_rules]]
- [ ] ROOM価格はitem_url直読みで取得したか（API商品名検索は参考に留めたか）[[reference_rakuten_room_price_fetch]]

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
