---
name: jinchi-review-narration
disable-model-invocation: true
description: >
  人智の外側(jinchi-longform)の工程4「ナレーション生成」のレビュー時に発動。親skill jinchi-longform の工程4末尾（連結ナレ完成・アセンブル前）から呼ばれる成果物レビュー（木の内側の子ノード）。loudnorm正規化・誤読のスプライス修正・自然ピッチ維持・字幕原文保持・語り部声の担保を合否判定する。
---

# jinchi-review-narration（工程4 ナレーションの成果物レビュー）

人智の外側 長尺パイプラインの **工程4（ナレーション生成）** 専用の成果物レビュー。音声成果物の品質ゲート。親 `jinchi-longform` の工程4末尾から発動。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝工程4固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の6観点）
- ③ `.company/sns_accounts/youtube_ainsight.md`（人智の外側の戦略・トーン前提の照合先）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（連結ナレ音声）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の6観点を1つずつ照合（特に観点4＝youtube_ainsight.md戦略と相違ないか）
- ③ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ④ 全passのみ次工程（アセンブル）へ進める

## 工程4の観点（合格ラインを内包・1つずつ照合）
- [ ] 全partをloudnorm正規化してから連結したか（音量差なし）
- [ ] 全再生成でなく誤読は文スプライスで直したか
- [ ] ピッチ統一せず自然ピッチの良いテイクを使ったか（別人化NG）
- [ ] 表示字幕は元のままTTS入力だけかな化したか
- [ ] 語り部声を口パク内蔵音/保存ボイスで担保したか（Gemini TTSに同一声を期待しない）（新規） [[reference_tts_voice_consistency_and_4k_assembler]]

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
