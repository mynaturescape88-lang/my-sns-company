---
name: report-review-analysis
disable-model-invocation: true
description: >
  sns-reportの工程「数値分析（アナリスト成果物）」のレビュー時に発動。親skill sns-report のアナリスト分析完了後・マーケター戦略コメント着手前から呼ばれる成果物レビュー（木の内側の子ノード）。データ取得の実行順序遵守・4点構成（直近数値＋帰属＋週次WoW＋打ち手）・伸びの起因切り分け・数値の意味の実体確認・Instagram個別＋合算2視点を合否判定する。
---

# report-review-analysis（数値分析の成果物レビュー）

SNS運用レポートパイプラインの **数値分析（アナリスト成果物）** 専用の成果物レビュー。親 `sns-report` のアナリスト分析完了後・マーケター戦略コメント着手前から発動され、分析が正しい手順と切り分けで作られたかを合否判定する（Layer2＝工程固有の作り込み合否）。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝数値分析固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の7観点・★観点7＝権利・合法性）
- ③ 対象プラットフォームの `.company/sns_accounts/<platform>.md`（分析対象chの戦略・期待値の照合先＝`skill-routing.md`で特定。全PF対象時は各該当1枚）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（分析レポート）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の7観点を1つずつ照合（★観点7＝権利・合法性はfail-closed／特に観点4＝対象platform戦略と相違ないか）
- ③ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ④ 全passのみ次工程（マーケター戦略コメント）へ進める

## 数値分析の観点（合格ラインを内包・1つずつ照合）
- [ ] **フロー数値の週窓＝暦週(月〜日)の直近完了週（`wk_done=monday_of(today)-7`）**か＝**ローリング7日・API遅延バッファ足しは不可**（当週混入/週合算二重計上を招く）（新規）[[reference_report_flow_window_must_match_wk_done]]
- [ ] **5項目構成か**＝①位置付け（狙い＋数値目標）／②戦略（実施中のみ）／③期待値（数値の想定値）／④評価（調子＋多期間＋施策ログ照合）／⑤見直し、で旧構成の残骸（独立した④観測欄・⑤評価/⑥見直しの番号ズレ）が無いか（新規）
- [ ] **④評価の現在地数値がHTMLの表セル（最新完了週列）と一致**するか＝別取得値とドリフトしていれば表の値へ照合したか（旧④観測は廃止＝現在地の実値は④評価内で③と対比して述べる）（新規）
- [ ] **③期待値が記録項目ごとの想定値＝数値**か＝そのPFの表に出ている指標単位で**数値**で書かれ（定性語だけで終えていない）、**外部要因項目（投稿本数等）やKGI逆算KPI（月◯再生等・①へ）を③に混入していないか**（新規）
- [ ] **④評価が多期間（前週/先々週/約1ヶ月前）で戦略の狙い/期待値を基準に調子（好調/現状維持/停滞/不調）を判定**しているか＝生の増減だけで調子を決めていないか（新規）
- [ ] **原本が現状へ最新化されているか**＝各PF実状を確認し、原本の更新漏れ（例＝塗り絵が公開済みなのに原本「未公開」）を放置して②③を照合していないか（新規）
- [ ] STEP0でsns_accountsプロファイルを必読し、データ取得を実行順序（rakuten_affiliate→rakuten_room→各report→threads→note）どおりに実行したか （新規）
- [ ] 4点構成（直近数値＋帰属分析＋週次WoW＋打ち手）で出したか [[feedback_report_attribution_weekly]]
- [ ] 伸びが施策/既存資産/単発バズのどれ起因か切り分けたか [[feedback_report_attribution_weekly]]
- [ ] 報告する数値・件数・ログ行の意味を実体確認したか（記事数か参照URL数か等を推測で語らない）[[feedback_report_attribution_weekly]]
- [ ] Instagramは個別＋合算の2視点で出したか [[feedback_startup_report]]
- [ ] レポート/分析前に楽天アフィリ最新データ（rakuten_affiliate_report.py）を取得したか [[feedback_rakuten_affiliate_on_report]]
- [ ] 各PFで①位置付け(狙い＋数値目標)②実施中の戦略③期待値(数値の想定値)④評価(③に対する現在地の調子＋理由・多期間)⑤見直し まで、実数値と原本を突き合わせて出したか （新規）
- [ ] **④評価で各PFの施策ログ（`sns_accounts/<platform>.md`「過去に行った施策と結果」の実施日×現在数値）を突き合わせ、効果が出てそうか否かを帰属切り分け込みで述べたか**＝個別の効果測定タスクは作らずこの照合で完結（2026-06-29恒久ルール・正本=`secretary/report_format.md`／手順=`knowledge/kb-analysis-procedure.md`「施策ログ照合ゲート」）。測定未到来なら再判定日を明記。必要数値が取得スクリプトで取れない場合はその旨を明記（推測判定NG）。突き合わせ無し＝fail [[feedback_effect_measurement_needs_script_exec_not_just_readwrite]]

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
