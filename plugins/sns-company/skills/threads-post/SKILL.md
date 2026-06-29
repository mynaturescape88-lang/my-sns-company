---
name: threads-post
description: >
  Threads投稿文の作成を、蓄積したルールに沿って実施するスキル。「Threadsの投稿文を作って」「スレッズの投稿を書いて」「記事公開のThreads文」等と言われたら発動する。承認必須・記事タイトルそのまま使わない・ハッシュタグ規定を内包し、発動だけで全ルールを反映する。
---

# Threads投稿文 スキル v1.0

Threadsの投稿文を作るときに発動する。
**目的**：creator/CLAUDE.md にルールを書いて毎回読む方式をやめ、Threads投稿タスクのときだけ本スキルを発動して全ルールを過不足なく適用する。

## 発動時に必ず行う（発動＝適用）

1. 下記ルールを適用して投稿文を作る。
2. **必ずオーナーへ提示して承認を得てから投稿する**（[[feedback_go_signal_owner_only_no_soliciting]]・秘書窓口経由）。GitHub Actions の自動投稿ワークフローは既承認のため除く。

> このスキルは Threads 投稿文**専用**。Instagram・note・記事系のルールは含まない（各専用スキルを使う）。

---

## Threads投稿文
- 必ずオーナーへ提示してから投稿する（承認必須）
- 記事タイトルをそのまま使わず、読者の興味を引く導入文にする
- ハッシュタグ：`#観葉植物 #植物好きな人と繋がりたい` + 記事内容に応じた1つ

---

## PMレビュー基準（公開前）

※公開前の法令/著作権/事実/日本語/安全性は content-compliance スキルで確認。

### アカウント別の追加チェック
- **Threads**：fediverse連携により外部SNSに拡散する可能性を考慮した内容か。内輪向けの表現・限定情報が含まれていないか

---

## 提出前 自己チェック

```
□ 記事タイトルをそのまま使わず、興味を引く導入文にしたか
□ ハッシュタグは #観葉植物 #植物好きな人と繋がりたい ＋内容に応じた1つか
□ オーナーへ提示して承認を得る前提か（手動投稿時）
```
> 「レビューに出す＝作り込み済み・既知の改善余地ゼロ」（[[feedback_review_means_finished_no_known_gaps]]）。

---

## SE技術メモ（Threads）

- Threads APIはMeta Graph API経由。投稿済みIDは `threads_posted_ids.txt` で管理
- Stage1→Stage2移行のため、投稿後1時間のコメント返信を人手で対応する必要がある（自動化不可）

---

## ◆成果物／・作業ノードの分類
- ◆投稿文（→ `threads-review`・小さい木）
- ・投稿操作（post_thread／GitHub Actions自動WF＝純作業・承認の遵守は threads-review 観点で担保）

## 子レビューskill 発動連鎖（成果物ノードごと・fail-closedで次へ）
- 投稿文執筆後・オーナー提示前 → `threads-review` を発動（pass のみ提示へ）
- ◆pass後 → `pm/review-baseline.md`（Layer1てっぺん総合）を1回当てる
※投稿操作は作業ノード＝レビューskillを当てない（公開前の法令/著作/事実は content-compliance）
