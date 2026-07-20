---
name: jinchi-review-thumbnail
disable-model-invocation: true
description: >
  人智の外側(jinchi-longform)の工程11a「サムネ」のレビュー時に発動。親skill jinchi-longform の工程11で3案出した直後（選定→アップ前）から呼ばれる成果物レビュー（木の内側の子ノード）。高再生ベンチマーク踏襲・10字以内/2-3色・3案選定・絵文字0、およびアップ直前メタ（目次/パス/合成開示）の最終確認を合否判定する。
---

# jinchi-review-thumbnail（工程11a サムネの成果物レビュー）

人智の外側 長尺パイプラインの **工程11a（サムネ）** 専用の成果物レビュー。クリック率を決める成果物のゲート。親 `jinchi-longform` の工程11でサムネ3案を出した直後（選定→アップ前）から発動。
※工程11b（非公開アップ）自体はレビュー対象にしないが、`lessons-learned.md` の「目次旧版流用」「VIDEO/THUMBパス旧版指定」「containsSyntheticMedia必須」の致命3事故は、本skillの「アップ直前メタ」観点でここで吸収する。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝工程11a固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の7観点・★観点7＝権利・合法性）
- ③ `.company/sns_accounts/youtube_ainsight.md`（人智の外側の戦略・サムネ勝ち筋の照合先）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（サムネ3案＋選定案＋アップ直前メタ）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の7観点を1つずつ照合（★観点7＝権利・合法性はfail-closed／特に観点4＝絵文字0/戦略相違）
- ③ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ④ 全passのみアップへ進める（アップ前に親手順で `pm/review-baseline.md` Layer1てっぺん総合を1回当てる）

## 工程11aの観点（合格ラインを内包・1つずつ照合）
- [ ] 高再生サムネをベンチマークし勝ちパターンを真似たか [[feedback_research_proven_thumbnails_before_creating]]
- [ ] 10字以内/2-3色/単一被写体・誇大でないか
- [ ] 3案出して選定したか（1案即決でない）
- [ ] サムネ文字に絵文字0か [[feedback_no_emoji_use_svg_icons]]
- [ ] アップ前メタが最終版か＝目次がtimeline.json実尺/VIDEO・THUMBパス最新/containsSyntheticMedia:True・private・AI免責（新規）

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
