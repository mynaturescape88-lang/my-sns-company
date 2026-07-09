---
name: jinchi-review-assemble
disable-model-invocation: true
description: >
  人智の外側(jinchi-longform)の工程8「4Kアセンブル」のレビュー時に発動。親skill jinchi-longform の工程8末尾（4K書き出し完了・字幕同期前 or 字幕反映後の再ビルド時）から呼ばれる成果物レビュー（木の内側の子ノード）。行アンカー同期・口パク内蔵音保持・正規スクリプト使用・PNG overlay字幕・PATHキャッシュ対策・尺を合否判定する。
---

# jinchi-review-assemble（工程8 4Kアセンブルの成果物レビュー）

人智の外側 長尺パイプラインの **工程8（4Kアセンブル）** 専用の成果物レビュー。組上げ成果物のゲート。親 `jinchi-longform` の工程8末尾から発動。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝工程8固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の6観点）
- ③ `.company/sns_accounts/youtube_ainsight.md`（人智の外側の戦略・尺前提の照合先）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（4K書き出し）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の6観点を1つずつ照合（特に観点4＝youtube_ainsight.md戦略と相違ないか）
- ③ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ④ 全passのみ次工程（字幕同期）へ進める

## 工程8の観点（合格ラインを内包・1つずつ照合）
- [ ] 行アンカー同期で組んだか（比例配置でズレていない）
- [ ] 口パク内蔵音をそのまま使ったか（捨てて別音声を上乗せしていない） [[reference_higgsfield_lipsync_pipeline_reality]]
- [ ] build_nazca_4k.pyを使ったか（旧1080p版でない）
- [ ] 字幕がPIL PNG overlay焼きか（この環境はdrawtext非搭載）
- [ ] 画像差替えを新パス/カット削除で行ったか（PATHキャッシュ対策）
- [ ] presetがveryfastで書き出し時間が現実的か（新規） [[project_mynaturescape_longform_oneshot]]
- [ ] 同一素材を2回以上使っていないか（**全編ユニーク＝2回目の使用も不可**・2026-07-01オーナー決定）。実写stockは `{prefix}_02,_03…` を使用回数ぶん `_stock/` に足せば `pick_stock` の `files[i % len]` 巡回で自動的に固有化される（アセンブラ改修不要）／静止画jpg・合成★・図解③は固有ファイルを別名で用意。不足分は**追加0cr素材（PD/CC0/CC/Pexels）を調達してでも全カット固有素材にする**
- [ ] 完成尺が15分以上か（実尺≥15:00）

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
