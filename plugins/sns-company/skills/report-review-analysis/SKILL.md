---
name: report-review-analysis
description: >
  sns-reportの工程「数値分析（アナリスト成果物）」のレビュー時に発動。親skill sns-report のアナリスト分析完了後・マーケター戦略コメント着手前から呼ばれる成果物レビュー（木の内側の子ノード）。データ取得の実行順序遵守・4点構成（直近数値＋帰属＋週次WoW＋打ち手）・伸びの起因切り分け・数値の意味の実体確認・Instagram個別＋合算2視点を合否判定する。
---

# report-review-analysis（数値分析の成果物レビュー）

SNS運用レポートパイプラインの **数値分析（アナリスト成果物）** 専用の成果物レビュー。親 `sns-report` のアナリスト分析完了後・マーケター戦略コメント着手前から発動され、分析が正しい手順と切り分けで作られたかを合否判定する（Layer2＝工程固有の作り込み合否）。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝数値分析固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の6観点）
- ③ 対象プラットフォームの `.company/sns_accounts/<platform>.md`（分析対象chの戦略・期待値の照合先＝`skill-routing.md`で特定。全PF対象時は各該当1枚）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（分析レポート）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の6観点を1つずつ照合（特に観点4＝対象platform戦略と相違ないか）
- ③ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ④ 全passのみ次工程（マーケター戦略コメント）へ進める

## 数値分析の観点（合格ラインを内包・1つずつ照合）
- [ ] STEP0でsns_accountsプロファイルを必読し、データ取得を実行順序（rakuten_affiliate→rakuten_room→各report→threads→note）どおりに実行したか （新規）
- [ ] 4点構成（直近数値＋帰属分析＋週次WoW＋打ち手）で出したか [[feedback_report_attribution_weekly]]
- [ ] 伸びが施策/既存資産/単発バズのどれ起因か切り分けたか [[feedback_report_attribution_weekly]]
- [ ] 報告する数値・件数・ログ行の意味を実体確認したか（記事数か参照URL数か等を推測で語らない）[[feedback_report_attribution_weekly]]
- [ ] Instagramは個別＋合算の2視点で出したか [[feedback_startup_report]]
- [ ] レポート/分析前に楽天アフィリ最新データ（rakuten_affiliate_report.py）を取得したか [[feedback_rakuten_affiliate_on_report]]
- [ ] 各PFの戦略「今後の戦略＋期待値」と観測実数値を突き合わせ①実施中の戦略②期待値③観測数値④評価（想定通り/想定外）まで報告したか （新規）

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
