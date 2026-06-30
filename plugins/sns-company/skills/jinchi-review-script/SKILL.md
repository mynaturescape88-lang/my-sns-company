---
name: jinchi-review-script
disable-model-invocation: true
description: >
  人智の外側(jinchi-longform)の工程2「台本＋ファクトチェック」のレビュー時に発動。親skill jinchi-longform の工程2末尾（台本＋FCログ完成・台帳着手前）から呼ばれる成果物レビュー（木の内側の子ノード・★最重要）。起承転結・語り部3挿入・FC裏取り・TTS誤読対策・尺・謎を残さないかを合否判定する。
---

# jinchi-review-script（工程2 台本＋FCの成果物レビュー）★最重要

人智の外側 長尺パイプラインの **工程2（台本＋ファクトチェック）** 専用の成果物レビュー。本chの律速・品質の核＝最重要ノード。親 `jinchi-longform` の工程2末尾から発動。
**前提＝`content-planning` スキルを実発動済みであること**（発動＝完了でない・手順を実際に回したか [[feedback_skill_activation_must_execute_workflow]]）。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝工程2固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の6観点）
- ③ `.company/sns_accounts/youtube_ainsight.md`（人智の外側の戦略・役割・コスト前提・却下レバーの照合先）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（台本＋FCログ）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の6観点を1つずつ照合（特に観点4＝youtube_ainsight.md戦略と相違ないか）
- ③ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ④ 全passのみ次工程（工程3 台帳）へ進める

## 工程2の観点（合格ラインを内包・1つずつ照合）
- [ ] 結で具体的答えを提示しているか（謎で終わっていない） [[feedback_ainsight_no_mystery_ending]]
- [ ] 起承転結・コールドオープン・章立てになっているか
- [ ] 語り部3挿入（開幕/中盤転→結/ED）と各所の「間」があるか
- [ ] FCログが台本末尾にあり、数字・年代・固有名詞を一次情報で裏取りしたか
- [ ] 数字がかな/漢字表記か（TTS誤読対策）
- [ ] ナレ字数が15分以上相当（約4,400字以上）か
- [ ] 即興でなく実証済みの作法で書いたか [[feedback_copy_proven_prompt_output_pairs]]
- [ ] 落ち着いた語り・世界観・FCがありAI放置量産でないか（新規）
- [ ] 語り部セリフが直近2本と重複していないか
- [ ] 語り部セリフの構成が直近2本と同型でないか

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
