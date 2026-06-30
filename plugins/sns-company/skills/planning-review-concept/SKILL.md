---
name: planning-review-concept
disable-model-invocation: true
description: >
  content-planningの工程「企画コンセプト確定」のレビュー時に発動。親skill content-planning の企画案確定後・台本着手前から呼ばれる成果物レビュー（木の内側の子ノード）。稼げるジャンル先決→AI効率化の順・バズ判定3基準（好奇心/珍しさ/没入感）を1つ以上充足・競合との演出差別化・アカウント人格の注入・確立テーマ内・バズ企画へのひねりを合否判定する。
---

# planning-review-concept（企画コンセプトの成果物レビュー）

コンテンツ企画パイプラインの **企画コンセプト確定** 専用の成果物レビュー。親 `content-planning` の企画案確定後・台本着手前から発動され、企画がバズ性とコンセプト適合を満たすかを合否判定する（Layer2＝工程固有の作り込み合否）。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝企画コンセプト固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の6観点）
- ③ 対象プラットフォームの `.company/sns_accounts/<platform>.md`（企画の載るchの戦略・却下レバー＝`skill-routing.md`で特定。YouTube長尺=youtube.md／人智の外側=youtube_ainsight.md等）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（企画コンセプト案）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の6観点を1つずつ照合（特に観点4＝対象platform戦略と相違ないか）
- ③ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ④ 全passのみ次工程（台本作成）へ進める

## 企画コンセプトの観点（合格ラインを内包・1つずつ照合）
- [ ] 「稼げるジャンル先決→AIで効率化」の順で企画したか（AIありき・ツール先行でないか）（新規）
- [ ] バズ判定3基準を1つ以上満たすか：①好奇心（驚き・知らなかった）②珍しさ（希少情報・場所・視点）③没入感（止まらない展開）（新規）
- [ ] 競合との演出差別化があるか（王道そのまま・丸コピーでなく良所の組み合わせでオリジナル演出）（新規）
- [ ] アカウント人格（自分の声・温度感・視点）が入っているか （新規）
- [ ] 確立済テーマ（植物/子育て/古代ミステリー）に絞り新ジャンルに踏み込んでいないか （新規）
- [ ] バズ企画にひねりを加えたか（競合と同じ企画の丸コピー禁止）（新規）
- [ ] YouTube長尺企画なら却下済みレバー（頻度増/サムネ/タイトル/概要欄/ショート分離等）に抵触していないか [[feedback_youtube_premise_and_rejected_levers]]

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
