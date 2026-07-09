---
name: jinchi-review-ledger
disable-model-invocation: true
description: >
  人智の外側(jinchi-longform)の工程3「章別素材台帳」のレビュー時に発動。親skill jinchi-longform の工程3末尾（台帳が再生順に埋まった直後・生成着手前）から呼ばれる成果物レビュー（木の内側の子ノード）。区分割当・尺整合・最高品質材料の先決・最新単価でのcr積算・300cr照合・✅の正当性を合否判定する。
---

# jinchi-review-ledger（工程3 章別素材台帳の成果物レビュー）

人智の外側 長尺パイプラインの **工程3（章別素材台帳）** 専用の成果物レビュー。台帳＝制作の背骨。親 `jinchi-longform` の工程3末尾から発動。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝工程3固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の6観点）
- ③ `.company/sns_accounts/youtube_ainsight.md`（人智の外側の戦略・1本300cr上限・コスト前提の照合先）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（章別素材台帳）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の6観点を1つずつ照合（特に観点4＝youtube_ainsight.md戦略・観点6＝cr前提と相違ないか）
- ③ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ④ 全passのみ次工程（生成・制作）へ進める

## 工程3の観点（合格ラインを内包・1つずつ照合）
- [ ] 全カットに区分①〜⑥が割当たり再生順リストになっているか
- [ ] 台帳の尺の誤差が0か（実合計＝集計欄・「要敷き詰め」を残さない） [[feedback_report_verify_before_reporting]]
- [ ] 各カットで最高品質の材料を先に決めたか（cr節約で安易に画像化しない） [[feedback_plan_best_quality_first_then_estimate]]
- [ ] 予crを最新単価(get_cost)で積算したか（古い単価で積まない） [[feedback_reconcile_params_with_canon_before_generation]]
- [ ] 最新単価での総crが300cr以内か
- [ ] 予crと実crを分けて記録し、既存沈没素材の実crを0にしていないか
- [ ] 本物で作れるものをAIにしていないか（人物再現だけAI）
- [ ] 台帳が単一正本(cr-ledger)として置かれているか（新規） [[feedback_reconcile_params_with_canon_before_generation]]
- [ ] 台帳に未記載（空欄）が0か
- [ ] 本番素材が未完成のカットに✅がついていないか [[feedback_ledger_done_mark_means_final_asset]]
- [ ] **全カットが固有素材で、同一素材の使い回しが無いか（全編ユニーク＝2回目の使用も不可）**：台帳の全カットの素材（実写prefix・静止画・合成★・図解③）が延べ本数ぶん固有か。同一prefixがN回登場するなら固有バリアントがN本（`{prefix}_01…_0N`）用意される計画になっているか。不足は追加0cr素材の調達で埋める（generic stockの再掲で埋めない・2026-07-01オーナー決定）

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
