---
name: note-review-scan
description: >
  note-writingの工程「公開前スキャン・コードセキュリティ」のレビュー時に発動。親skill note-writing の本文完成後・公開操作着手前から呼ばれる成果物レビュー（木の内側の子ノード・★秘匿の最終ゲート）。ブログ名/SNSアカウント名/アプリ名/実ドメイン/実ID・トークン値の非記載（コード・コメント・末尾含む全文）・APIキー等のプレースホルダー化を合否判定する。
---

# note-review-scan（公開前スキャン・コードセキュリティの成果物レビュー）

note記事執筆パイプラインの **公開前スキャン・コードセキュリティ** 専用の成果物レビュー。親 `note-writing` の本文完成後・公開操作着手前から発動され、秘匿情報と秘密情報の混入がゼロかを合否判定する（Layer2＝工程固有の作り込み合否・★秘匿の最終ゲート）。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝公開前スキャン固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の6観点）
- ③ `.company/sns_accounts/note.md`（noteの戦略・却下レバーの照合先）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（全文＝本文＋コードブロック＋コメント＋末尾の一言）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の6観点を1つずつ照合（特に観点4＝note.md戦略と相違ないか）
- ③ **fail-closed**：1つでもfailなら提出せず→プレースホルダー置換→①へ戻る
- ④ 全passのみ公開操作へ進める

## 公開前スキャン・コードセキュリティの観点（合格ラインを内包・1つずつ照合）
- [ ] ブログ名（例：MAINEのShop-List）が全文に無いか（一般表現に置換したか）[[feedback_note_article_rules]]
- [ ] SNSアカウント名（@mynaturescape・@myraisingdiary）が全文に無いか [[feedback_note_article_rules]]
- [ ] アプリ名・システムユーザー名（例：maine-analytics）が全文に無いか [[feedback_note_article_rules]]
- [ ] 実ドメイン（例：mynaturescape-event.com）が全文に無いか（your-blog.example.com等へ置換）[[feedback_note_article_rules]]
- [ ] 実際のID・トークン値（システムユーザーID等の数値列）が全文に無いか [[feedback_note_article_rules]]
- [ ] スキャン対象を本文だけでなくコードブロック内・コメント・docstring・末尾の一言まで全文に通したか [[feedback_note_article_rules]]
- [ ] APIキー・アクセストークン・パスワードの実値がコードサンプルに無いか（os.environ/${{ secrets }}/EAAxxxx 等のプレースホルダー化）[[feedback_ai_coding_security_policy]]

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
