---
name: affiliate-review-body
disable-model-invocation: true
description: >
  affiliate-writingの工程「本文・コメント」のレビュー時に発動。親skill affiliate-writing の紹介文/コメント/記事本文執筆後から呼ばれる成果物レビュー（木の内側の子ノード）。体験偽装の非記載・数字や配送在庫の非約束・推薦スタイル維持・インフルエンサー名/第三者推薦の不引用・子育て節約の倹約家ペルソナと非断定・必需品を不要に混ぜない・コメントの自己矛盾なしを合否判定する。
---

# affiliate-review-body（本文・コメントの成果物レビュー）

アフィリエイト・収益系ライティングパイプラインの **本文・コメント** 専用の成果物レビュー。親 `affiliate-writing` の紹介文/コメント/記事本文の執筆後から発動され、本文が体験偽装/約束/分類矛盾のない文かを合否判定する（Layer2＝工程固有の作り込み合否）。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝本文・コメント固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の7観点・★観点7＝権利・合法性）
- ③ `.company/sns_accounts/rakuten.md`（楽天ROOM/アフィリの戦略・却下レバーの照合先）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（本文・コメント）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の7観点を1つずつ照合（★観点7＝権利・合法性はfail-closed／特に観点4＝rakuten.md戦略と相違ないか）
- ③ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ④ 全passのみ次工程（価格・数値検証）へ進める

## 本文・コメントの観点（合格ラインを内包・1つずつ照合）
- [ ] 種類数・レビュー件数・ランキング・価格の数字を本文に書いていないか（「たくさんの種類」等に）[[feedback_affiliate_asp_article_rules]]
- [ ] 「翌日届く」「即日発送」等の配送・在庫の約束をしていないか [[feedback_affiliate_asp_article_rules]]
- [ ] 使っていない商品に一人称体験談（使ってみた・気に入っています）を書かず推薦スタイル（おすすめ・気になってる）に留めたか [[feedback_affiliate_asp_article_rules]]
- [ ] インフルエンサー名・第三者推薦の引用を入れていないか（自分が見つけた視点で書く）[[feedback_affiliate_asp_article_rules]]
- [ ] 子育て節約記事は倹約家ペルソナ＋非断定（必要・不要は家庭ごと／我が家では使わなかった）で書いたか [[project_kosodate_savings_article_series]]
- [ ] 安全・法的必須品（チャイルドシート等）に「車をお使いの方は必須」の注記があり"買わなくていい"と読ませていないか [[project_kosodate_savings_article_series]]
- [ ] コメントで他商品を引用するとき（○○で代用）、引用先が必要側/不要側の分類と矛盾していないか（未確定の物を代用例に使わない）[[project_kosodate_savings_article_series]]

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
