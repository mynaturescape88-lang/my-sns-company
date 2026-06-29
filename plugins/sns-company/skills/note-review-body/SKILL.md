---
name: note-review-body
description: >
  note-writingの工程「本文」のレビュー時に発動。親skill note-writing の本文執筆後・公開前スキャン着手前から呼ばれる成果物レビュー（木の内側の子ノード）。数字を実コード確認後に記載・ライブラリ名の用途括弧書き・体験記の固有名詞性・規約違反手順の非指南・最新モデルID・テーブルのPNG画像化・note.com貼り付け前提を合否判定する。
---

# note-review-body（本文の成果物レビュー）

note記事執筆パイプラインの **本文** 専用の成果物レビュー。親 `note-writing` の本文執筆後・公開前スキャン着手前から発動され、本文がルールに適合し誇大/規約違反/不正確のない記事かを合否判定する（Layer2＝工程固有の作り込み合否）。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝本文固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の6観点）
- ③ `.company/sns_accounts/note.md`（noteの戦略・却下レバーの照合先）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（本文）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の6観点を1つずつ照合（特に観点4＝note.md戦略と相違ないか）
- ③ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ④ 全passのみ次工程（公開前スキャン）へ進める

## 本文の観点（合格ラインを内包・1つずつ照合）
- [ ] 数字・事実（投稿頻度・ファイル名・スケジュール・実感値）を実コード/実ファイルで確認してから書いたか（推測で書かない）[[feedback_note_article_rules]]
- [ ] ライブラリ名・コマンド名に「何のためか」の用途括弧書きを付けたか [[feedback_note_article_rules]]
- [ ] 体験記（旅行記等）は固有名詞＋実体験の因果で再現可能に書いたか（抽象化・匿名化していないか・数字は実値＋時点＋免責）[[feedback_experience_article_concrete_reproducible]]
- [ ] 自動化/スクレイピング記事で規約違反の手順を有料指南していないか（規約クリーンな構成へ振替）[[rakuten_room_automation_tos]]
- [ ] Claudeのモデル名は最新の正式ID（現行 claude-sonnet-4-6）か（旧世代IDを断定で書いていないか）[[feedback_note_article_rules]]
- [ ] テーブル・図解・アーキ図はPillow(PIL)でPNG画像化したか（note貼り付けで崩れるMarkdownテーブル/ASCII図のまま残していないか）[[feedback_note_article_rules]]
- [ ] note.com貼り付け前提を考慮したか（コードブロックは手動変換・引用ブロック内コードは使わない）[[feedback_note_article_rules]]
- [ ] 「完全自動化」等の誇大表現がなく、AI活用の体験ベースになっているか （新規）

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
