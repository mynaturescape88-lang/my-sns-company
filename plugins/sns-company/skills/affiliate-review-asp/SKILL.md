---
name: affiliate-review-asp
disable-model-invocation: true
description: >
  affiliate-writingの工程「ASP提出・収益系記事の体裁」のレビュー時に発動。親skill affiliate-writing のASP案件記事のHTML/メタ確定後・提出前から呼ばれる成果物レビュー（木の内側の子ノード）。高額報酬表記の不採用・収益額の日時/エリア＋指定免責・商標スラッグ禁止・招待コード非掲載・登録/提出URLのドメイン一致・参考記事と同スタイル付きHTMLのレンダリング目視・構造健全性を合否判定する。
---

# affiliate-review-asp（ASP提出・体裁の成果物レビュー）

アフィリエイト・収益系ライティングパイプラインの **ASP提出・収益系記事の体裁** 専用の成果物レビュー。親 `affiliate-writing` のASP案件記事のHTML/メタ確定後・提出前から発動され、提出体裁が審査基準に適合するかを合否判定する（Layer2＝工程固有の作り込み合否）。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝ASP提出・体裁固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の6観点）
- ③ `.company/sns_accounts/rakuten.md`（楽天ROOM/アフィリの戦略・却下レバーの照合先）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（ASP提出用HTML/メタ）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の6観点を1つずつ照合（特に観点4＝rakuten.md戦略と相違ないか）
- ③ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ④ 全passのみ提出/公開へ進める

## ASP提出・体裁の観点（合格ラインを内包・1つずつ照合）
- [ ] 「月100万」等の高額報酬表記を入れず現実的な範囲（日給ベース等）に留めたか [[feedback_affiliate_asp_article_rules]]
- [ ] 収益額を書くとき日時・エリアを明記し、指定どおりの免責文言（読点指定含む）を記載したか [[feedback_affiliate_asp_article_rules]]
- [ ] 商標語をURL・スラッグ（英語・スペルミス含む）に使っていないか [[feedback_affiliate_asp_article_rules]]
- [ ] 招待コード・紹介コードを掲載していないか・登録URLと提出URLのドメインが一致しているか [[feedback_affiliate_asp_article_rules]]
- [ ] 参考記事と同じスタイル付きHTML（装飾・見た目）で作りブラウザでレンダリング目視したか（プレーンMarkdown提出でない）[[feedback_affiliate_asp_article_rules]]
- [ ] 構造健全性：タイトル25-45字／div・table開閉0／`<p>`内`<div>`無／CTAにnofollow+target="_blank"／wp:post-content・[toc]混入無 [[feedback_affiliate_asp_article_rules]]
- [ ] 楽天ROOMへの自動"投稿"をしていないか（登録は手動コピペ・取得は公式API）[[rakuten_room_automation_tos]]

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
