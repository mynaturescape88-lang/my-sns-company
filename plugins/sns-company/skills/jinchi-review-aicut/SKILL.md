---
name: jinchi-review-aicut
disable-model-invocation: true
description: >
  人智の外側(jinchi-longform)の工程7「AI生成カット」のレビュー時に発動。親skill jinchi-longform の工程7で各カット生成「前」（cr消費前の照合ゲート）＋生成「後」（採否ゲート）の2点から呼ばれる成果物レビュー（木の内側の子ノード・cr消費に関わる最重要ゲート）。生成前は尺/画角/単価照合・口パクレシピ・流用をfail-closedで、生成後はてかり/老け/質感寄せ/崩壊救済を合否判定する。
---

# jinchi-review-aicut（工程7 AI生成カットの成果物レビュー）★cr消費・2点ゲート

人智の外側 長尺パイプラインの **工程7（AI生成カット）** 専用の成果物レビュー。有料生成のゲート。親 `jinchi-longform` の工程7から、**各カット生成「前」（cr消費前の照合ゲート）と生成「後」（採否ゲート）の2点**で発動する。生成前ゲートはfail-closed＝一つでも未照合なら生成しない。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝工程7固有の合格ライン・生成前/後の2セクション）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の7観点・★観点7＝権利・合法性）
- ③ `.company/sns_accounts/youtube_ainsight.md`（人智の外側の戦略・1本300cr上限・コスト前提の照合先）

## フロー（発動＝この順で全実行・fail-closed）
- ① **生成前**：下記「生成前ゲート」の観点を計画（尺/画角/単価/プロンプト/流用）に1つずつ照合（pass/fail）。**一つでもfailなら生成しない**（cr消費前に止める）
- ② `pm/review-baseline.md` の7観点を照合（★観点7＝権利・合法性はfail-closed／特に観点6＝最新単価での総cr≤300・費用対効果）
- ③ **生成後**：実生成カットに「生成後ゲート（採否）」の観点を1つずつ照合（pass/fail）
- ④ **fail-closed**：いずれの段でも1つでもfailなら採用せず→修正/再対応→該当段へ戻る
- ⑤ 全passのみ次工程へ進める

## 工程7の観点A（生成前ゲート・fail-closed＝未照合なら生成しない）
- [ ] 尺/画角/区分をcr-ledger A表で照合したか（二次メモの値を信じない） [[feedback_reconcile_params_with_canon_before_generation]]
- [ ] 最新単価(get_cost)での総crが300cr以内か（静的A表の予crをそのまま信じない）
- [ ] 口パクのgenerate_audioがtrueか（内蔵音を捨てていないか） [[reference_higgsfield_lipsync_pipeline_reality]]
- [ ] 口パクが確立レシピ（顔1枚＋audio・start_imageなし・最小肯定文）に沿うか
- [ ] 実証済み素材/過去job_idを流用したか（一から作り直していない） [[feedback_reuse_own_proven_assets_before_creating]]
- [ ] 画像生成プロンプトが過去の確立構文から逸脱していないか [[reference_image_gen_prompt_craft]]
- [ ] 動画生成プロンプトが過去の確立構文から逸脱していないか [[feedback_copy_proven_prompt_output_pairs]]

## 工程7の観点B（生成後ゲート・採否）
- [ ] 額/鼻/頬を拡大しててかり/老けがないか [[feedback_ai_human_realism_no_sheen_bar]]
- [ ] プロンプトに「しわ/aged/deep texture」を入れていないか [[feedback_ai_face_prompt_no_wrinkles]]
- [ ] 彩度/グレイン/画角がストック素材と揃っているか [[feedback_ai_clip_match_stock_footage]]
- [ ] 口パク以外にセリフ/口の動き/人物アップを入れていないか
- [ ] 崩壊カットを再生成せず0cr暗転で救済したか
- [ ] soul_2の幻覚文字を除去したか（新規）

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
