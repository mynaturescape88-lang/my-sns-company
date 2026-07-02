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
- [ ] **★全章 自動走査を先に通したか（FB箇所だけ直す＝禁止・恒久2026-07-01）**：`scripts/herculaneum_full_audit.py` で全章の図解カット表示尺を実測一覧化し **R6(15s未満)** と **R-D(ping-pong/-stream_loop残存)** を確認。15s未満の図は「物理的に取れない＝行span最大化済」か1件ずつ判断し achieved を報告に残す（一瞬で消える図＝不合格）。ping-pong反復は0であること。FB箇所だけでなく全図を実測してから台帳を済にする。
- [ ] ブランド統一（暗背景/金/ヒラギノW6）で下240pxが字幕帯か
- [ ] 図解2連続を避け間に実写/画像を挟んだか
- [ ] グローは円形Hann窓で縁0・暗めベースに合成したか
- [ ] 0crで内製したか（Higgsfield課金していない）
- [ ] 絵文字0か（装飾はSVG/線画） [[feedback_no_emoji_use_svg_icons]]
- [ ] 画面内テキストが化けず字幕帯と被っていないか（拡大検査）
- [ ] **B: 図解/合成カットが読み切れる表示尺を確保しているか（恒久・2026-07-01強化）**：図解(image)は目安 **15秒**（`MIN_FIG_SEC=15.0`）。`cut_times` が①同一行の隣接カット（最低 `MIN_SIBLING_SEC=1.6`s）から借り、②不足なら後続の非図解カットを圧縮（最低 `MIN_FOLLOW_SEC=1.8`s）して**行を跨いで図を延長**する。後続b-rollは後ろへずれるだけ＝字幕は絶対時刻overlayなので同期維持。15s取れない図は取れる最大で止め `_FIG_REPORT` に achieved を残し報告する。図が一瞬で消えるのは不合格。
- [ ] **D: 短尺クリップのループ反復（ping-pong寄り引き）を使っていないか（恒久）**：`render_cut` は短尺videoを長窓に充てるとき `-stream_loop` 反復をやめ、**先頭フレーム静止＋一方向 slow push-in**（imageと同じ zoompan）だけ掛ける。寄り引きが反復する見にくい動きは不合格。

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
