---
name: review-check
description: >
  成果物の自己チェック・PMレビューを実行するときに発動。『レビューして』『チェックして』『自己点検』『品質確認』『出す前に確認』『PMレビュー』等で発動。読むのは3つだけ＝そのタスクの fb/<task>.md ＋ pm/review-baseline.md ＋ 対象プラットフォームの sns_accounts/<platform>.md（戦略相違チェック用）。fail-closed＝全pass以外は提出せず修正、passした物だけ提示する。
---

# 成果物レビュー・チェックスキル v1.0

- 役割が成果物を出す前の**自己チェック**、およびPMの**レビュー**で発動する共通フロー
- 目的＝チェック観点を各CLAUDE.mdに書いて毎回読む方式をやめ、レビュー時だけ本スキルで過不足なく当てる

## 読むのは3つだけ（混在禁止・token効率）
- ① そのタスクの `.company/fb/<task>.md`（ドメイン固有FB）＝ `.company/skill-routing.md` の「fb/」列で決定
- ② `.company/pm/review-baseline.md`（全レビュー普遍の6観点）
- ③ **対象プラットフォームの `.company/sns_accounts/<platform>.md`（戦略相違チェックの照合先）**＝ `skill-routing.md`「タスク→対象platform」で**該当1枚だけ**特定（他媒体は読まない）。横断タスクは `knowledge/kb-marketing-strategy.md`。
- ※他タスクの fb/・他媒体の sns_accounts は読まない（1タスク1ファイル）。深掘りは `[[feedback_...]]` で memory 正本へ

## フロー（発動＝この順で全実行）
- ① 対象タスクを特定 → `skill-routing.md` で該当 `fb/<task>.md` を決める（無ければ普遍のみ）
- ② `fb/<task>.md` の各「□」を成果物に1つずつ照合（pass/fail）
- ③ `pm/review-baseline.md` の6観点を1つずつ照合（pass/fail）。**観点4＝★戦略相違チェック**＝提案/施策が対象 `sns_accounts/<platform>.md` の戦略（役割・勝ち筋・コスト前提・却下レバー）と**相違していないか**を照合（相違＝fail）
- ④ **fail-closed**：1つでもfailなら**提出しない**→該当箇所を修正→②へ戻る
- ⑤ 全passのみ提出（自己チェック＝役割→PMへ／PMレビュー＝PM→秘書→オーナー）
- ⑥ レビューで新たな指摘が出たら `COMMON.md`「FBの消化ライフサイクル」で該当 `fb/<task>.md` か `pm/review-baseline.md` へ1行追記（昇華は `task-wrapup` が担う）

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
- agentレビューに渡す前に当セッションのオーナー決定を canon/task_log へ反映してから渡す [[feedback_reflect_decisions_in_canon_before_agent_review]]

## 実行前/完了前チェック
```
□ ① 対象タスクの fb/<task>.md を skill-routing.md で正しく特定したか（他タスクのfbを混ぜていないか）
□ ② fb/<task>.md の全「□」を成果物に照合したか
□ ③ pm/review-baseline.md の6観点を全て照合したか（特に観点4＝対象platform戦略と相違ないか・観点6＝経費前提）
□ ④ failが1つでもあれば提出せず修正→再照合したか（fail-closed）
□ ⑤ 全passの成果物だけを次工程へ渡したか
□ ⑥ 新規指摘は該当 fb/ or review-baseline へ追記したか（昇華本処理は task-wrapup）
```
