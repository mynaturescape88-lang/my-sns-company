---
name: jinchi-review-caption
disable-model-invocation: true
description: >
  人智の外側(jinchi-longform)の工程9「Whisper字幕同期」のレビュー時に発動。親skill jinchi-longform の工程9末尾（align後・再ビルド後）から呼ばれる成果物レビュー（木の内側の子ノード）。実発話時刻の反映・エーアイ→AI置換・PNG overlay字幕方式・音と字幕のズレ0を合否判定する。
---

# jinchi-review-caption（工程9 Whisper字幕同期の成果物レビュー）

人智の外側 長尺パイプラインの **工程9（Whisper字幕同期）** 専用の成果物レビュー。字幕成果物のゲート。親 `jinchi-longform` の工程9末尾から発動。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝工程9固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の6観点）
- ③ `.company/sns_accounts/youtube_ainsight.md`（人智の外側の戦略・表記前提の照合先）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（同期字幕・再ビルド動画）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の6観点を1つずつ照合（特に観点4＝youtube_ainsight.md戦略と相違ないか）
- ③ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ④ 全passのみ次工程へ進める

## 工程9の観点（合格ラインを内包・1つずつ照合）
- [ ] Whisper実発話時刻を行アンカーへ反映して再ビルドしたか
- [ ] 字幕の「エーアイ」を「AI」へ置換したか [[reference_jinchi_subtitle_ai_voice_eraii]]
- [ ] 字幕がPIL PNG overlay焼き（白文字黒フチ）か
- [ ] 音と字幕のズレが0か（先頭/中盤/末尾を実測）（新規）

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
