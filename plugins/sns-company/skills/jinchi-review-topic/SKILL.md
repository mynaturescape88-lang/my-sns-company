---
name: jinchi-review-topic
description: >
  人智の外側(jinchi-longform)の工程1「題材確定」のレビュー時に発動。親skill jinchi-longform の工程1末尾（題材確定直後・台本着手前）から呼ばれる成果物レビュー（木の内側の子ノード）。題材のコンセプト適合・バズ性・却下レバー非抵触・結で答えを言える題材かを合否判定する。
---

# jinchi-review-topic（工程1 題材確定の成果物レビュー）

人智の外側 長尺パイプラインの **工程1（題材確定）** 専用の成果物レビュー。親 `jinchi-longform` の工程1末尾から発動され、題材がコンセプトに適合しバズる題材かを合否判定する（Layer2＝工程固有の作り込み合否）。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝工程1固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の6観点）
- ③ `.company/sns_accounts/youtube_ainsight.md`（人智の外側の戦略・役割・コスト前提・却下レバーの照合先）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（確定した題材）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の6観点を1つずつ照合（特に観点4＝youtube_ainsight.md戦略と相違ないか）
- ③ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ④ 全passのみ次工程（工程2 台本）へ進める

## 工程1の観点（合格ラインを内包・1つずつ照合）
- [ ] AIが人間に不可能だったことを可能にした題材か
- [ ] 題材キューと重複・既出でないか
- [ ] ミステリー感でクリックされ誇大でないか
- [ ] 却下済みレバーに抵触していないか [[feedback_youtube_premise_and_rejected_levers]]
- [ ] 結で具体的答えを言い切れる題材か（謎で終わる題材でない）（新規）

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
