---
name: note-review-structure
disable-model-invocation: true
description: >
  note-writingの工程「構成・タイトル・価格設定」のレビュー時に発動。親skill note-writing の構成確定後・本文執筆着手前から呼ばれる成果物レビュー（木の内側の子ノード）。有料/無料の構成比・価格・CTA・タイトルの結果数字ベース・無料部で核心を出さない有料価値設計・重い定型免責の不採用を合否判定する。
---

# note-review-structure（構成・タイトル・価格の成果物レビュー）

note記事執筆パイプラインの **構成・タイトル・価格設定** 専用の成果物レビュー。親 `note-writing` の構成確定後・本文執筆着手前から発動され、構成比/価格/CTA/タイトルが規定どおりかを合否判定する（Layer2＝工程固有の作り込み合否）。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝構成・タイトル・価格固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の6観点）
- ③ `.company/sns_accounts/note.md`（noteの戦略・却下レバーの照合先）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（構成案/タイトル/価格設計）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の6観点を1つずつ照合（特に観点4＝note.md戦略と相違ないか）
- ③ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ④ 全passのみ次工程（本文執筆）へ進める

## 構成・タイトル・価格の観点（合格ラインを内包・1つずつ照合）
- [ ] 有料記事は無料35〜40%（背景・結果・全体像）＋有料60〜65%（コード・手順・ハマり）の構成比か [[feedback_note_article_rules]]
- [ ] 価格は `sns_accounts/note.md` の確定方針どおりか（2026-06-15確定＝基本¥290・大型記事¥490で「広める」優先の低価格統一・¥100は不採用／親SKILL.md本文の旧「980円」でなく note.md を正本にする）・無料入門記事は価格設定なし （新規）
- [ ] タイトルが「結果＋数字＋【コード公開】」型で「〜した話」でなく「〜する方法」型か [[feedback_note_article_rules]]
- [ ] 無料入門記事は1,000〜1,500字・ステップ番号付き・末尾に有料記事へのCTAがあるか （新規）
- [ ] 無料部で「手法名・つまずき一覧・解決手順の地図」まで出していないか（AI再現で有料価値が落ちる）[[feedback_note_article_rules]]
- [ ] 重い定型法務免責（動作保証なし・自己責任）を冒頭/末尾に付けていないか（安心材料は本文に自然に溶かす）[[feedback_note_article_rules]]
- [ ] SNS→note誘導／note→SNS誘導を入れていないか（note.md恒久ルール＝SNS↔note誘導は絶対にしない・テーマ不一致/自動化バレ回避・noteは植物SNSと完全分離）[[project_raising_caption_on_hold]]

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
