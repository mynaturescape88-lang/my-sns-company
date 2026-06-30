---
name: jinchi-review-figure
disable-model-invocation: true
description: >
  人智の外側(jinchi-longform)の工程6「図解・合成」のレビュー時に発動。親skill jinchi-longform の工程6末尾（図解/合成カット完成・台帳の該当行を済にする直前）から呼ばれる成果物レビュー（木の内側の子ノード）。ブランド統一・図解2連続回避・グロー縁処理・0cr内製・絵文字0・画面内テキストの健全性を合否判定する。
---

# jinchi-review-figure（工程6 図解・合成の成果物レビュー）

人智の外側 長尺パイプラインの **工程6（図解・合成）** 専用の成果物レビュー。0cr内製の品質ゲート。親 `jinchi-longform` の工程6末尾から発動。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝工程6固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の6観点）
- ③ `.company/sns_accounts/youtube_ainsight.md`（人智の外側の戦略・コスト前提の照合先）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（図解/合成カット）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の6観点を1つずつ照合（特に観点4＝絵文字0/戦略相違・観点6＝0cr前提）
- ③ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ④ 全passのみ台帳の該当行を済にする／次工程へ進める

## 工程6の観点（合格ラインを内包・1つずつ照合）
- [ ] ブランド統一（暗背景/金/ヒラギノW6）で下240pxが字幕帯か
- [ ] 図解2連続を避け間に実写/画像を挟んだか
- [ ] グローは円形Hann窓で縁0・暗めベースに合成したか
- [ ] 0crで内製したか（Higgsfield課金していない）
- [ ] 絵文字0か（装飾はSVG/線画） [[feedback_no_emoji_use_svg_icons]]
- [ ] 画面内テキストが化けず字幕帯と被っていないか（拡大検査）（新規）

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
