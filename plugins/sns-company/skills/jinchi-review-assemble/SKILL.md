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
- [ ] **★【素材の実尺 vs 要求カット尺】を全カット検査したか＝承認していないスロー再生／ループ反復が黙って焼かれていないか**：`render_cut` は素材が要求尺に足りないとき**無言で** `setpts` スロー伸長（≤3.0倍）または `-stream_loop` 反復して通してしまう＝**誰も許可していない速度改変が「成功」として焼き込まれる**（2026-07-17 第4弾＝18カットが素通り・最大1.45倍）。**合格＝ビルド前ゲートで `window > nat` を全件列挙しFATALで停止すること**（実装＝`build_aivuln_4k.py:check_material_fit()`／`build_aivuln_4k_chapter.py` が呼びfail-closed）。対処は**素材を要求尺以上で作り直す**（★合成は生成器のJOBS durを上げる）／①実写は代替素材fetch or 台帳のカット設計尺を素材に合わせる。**スロー再生で尺を埋めるのは対処でない**（オーナー未許可）。ゲート結果は章JSONの `slow_cuts` に実測で落とし、PM（Bash不可）がReadで0件を検査する [[reference_jinchi_assembler_hold_and_fit_gates]]
- [ ] **★台帳・skillが約束した機構が、この題材のアセンブラに【実在するか】をgrepで確認したか＝系統退行（前弾にあった機構が後継で消える）を疑ったか**：アセンブラは前弾をコピーして作るため、**コピー元が既に機構を落としていると、以降の全題材が黙って機構なしになる**（2026-07-17＝③図解holdは第2弾`build_herculaneum_4k.py`・第3弾`build_ghost_hominin_4k.py`には在るのに`build_valeriana_4k.py`で消え、それをコピー元にした`build_aivuln_4k.py`にも無かった。台帳は「工程8でhold」と約束していたが**実装が存在しなかった**）。**合格＝「台帳/skillが約束した機構名」を新アセンブラに `grep` して実在を確認**（例＝`MIN_FIG_SEC`／`_FIG_REPORT`）。**シンボルが在るだけでは不足＝実際に呼ばれているか（呼出元grep）と、実配分での achieved を実測**まで見る [[reference_jinchi_assembler_hold_and_fit_gates]] [[reference_symbol_presence_is_not_working_code]]
- [ ] 完成尺が15分以上か（実尺≥15:00）

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
