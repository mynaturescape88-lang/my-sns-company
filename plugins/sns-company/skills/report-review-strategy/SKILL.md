---
name: report-review-strategy
disable-model-invocation: true
description: >
  sns-reportの工程「戦略コメント・見直し判断（マーケター成果物）」のレビュー時に発動。親skill sns-report のマーケター戦略照合完了後・秘書報告着手前から呼ばれる成果物レビュー（木の内側の子ノード）。各PF戦略との照合・見直し不要なら監視ログ追記のみ・見直し要なら具体修正＋推奨1案＋根拠・原本の無断変更なし（承認ゲート）・別台帳を作らないを合否判定する。
---

# report-review-strategy（戦略コメント・見直し判断の成果物レビュー）

SNS運用レポートパイプラインの **戦略コメント・見直し判断（マーケター成果物）** 専用の成果物レビュー。親 `sns-report` のマーケター戦略照合完了後・秘書報告着手前から発動され、戦略判断が根拠と承認ゲートを守るかを合否判定する（Layer2＝工程固有の作り込み合否）。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝戦略コメント固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の6観点）
- ③ 対象プラットフォームの `.company/sns_accounts/<platform>.md`（戦略の唯一の正本・却下レバーの照合先＝`skill-routing.md`で特定。全PF対象時は各該当1枚）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（戦略コメント・見直し案）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の6観点を1つずつ照合（特に観点4＝対象platform戦略と相違ないか）
- ③ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ④ 全passのみ秘書→オーナー報告へ進める

## 戦略コメント・見直し判断の観点（合格ラインを内包・1つずつ照合）
- [ ] 評価（想定通り/想定外）を受けて⑤戦略見直し要否まで結論を出したか （新規）
- [ ] 見直し不要なら原本を変えず監視ログに観測・評価を追記するに留めたか （新規）
- [ ] 見直し要（想定外）なら勝手に原本を変えず「具体修正・推奨1案＋根拠」を作り報告に添えたか（戦略変更は承認ゲート＝賛同後に原本更新）（新規）
- [ ] 提案の各要素に根拠（データ/出典/実物）があり「なんとなく」「ピーク時間に絞る」等の無根拠提案を混ぜていないか [[feedback_report_attribution_weekly]]
- [ ] 提案前に実施済み/凍結を確認したか（該当`sns_accounts/<platform>.md`却下レバー＋`pm/review-baseline.md`観点3＋improvement_log.json／TASKS.md）[[feedback_check_history_before_proposing]]
- [ ] 却下済みレバーの再提案でないか（手段でなくKGI月¥50,000・収益貢献度で優先順位）[[feedback_youtube_premise_and_rejected_levers]] [[feedback_anchor_proposals_on_true_goal_not_means]]
- [ ] PDCAは各PF戦略ファイル内で完結させ、別台帳（effect_measurement_schedule）を持たない/作らないを守ったか （新規）

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
