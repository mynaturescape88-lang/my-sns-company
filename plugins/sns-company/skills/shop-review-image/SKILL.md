---
name: shop-review-image
description: >
  shop-articleの工程「アイキャッチ・画像セット」のレビュー時に発動。親skill shop-article のアイキャッチ/ig画像セット確定後から呼ばれる成果物レビュー（木の内側の子ノード）。アイキャッチと本文1枚目の差異・ブログサムネイル命名・ig_images先頭のinstagramサムネイル・同名アップ時のfeatured_media別ID（衝突なし）を合否判定する。
---

# shop-review-image（アイキャッチ・画像セットの成果物レビュー）

ショップ紹介記事パイプラインの **アイキャッチ・画像セット** 専用の成果物レビュー。親 `shop-article` のアイキャッチ/ig画像セット確定後から発動され、画像セットの命名と割当が正しいかを合否判定する（Layer2＝工程固有の作り込み合否）。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝アイキャッチ・画像セット固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の6観点）
- ③ `.company/sns_accounts/blog.md`（shopの戦略・却下レバーの照合先）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（確定したアイキャッチ/ig画像セット）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の6観点を1つずつ照合（特に観点4＝blog.md戦略と相違ないか）
- ③ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ④ 全passのみ次工程（SEOタイトル・メタ）へ進める

## アイキャッチ・画像セットの観点（合格ラインを内包・1つずつ照合）
- [ ] アイキャッチと本文1枚目が異なる写真か [[feedback_shop_article_rules]]
- [ ] アイキャッチが「ブログサムネイル_〇〇.png」か（Instagramサムネと混同なし） （新規）
- [ ] ig_images先頭が「instagramサムネイル_〇〇.png」か （新規）
- [ ] 同名「サムネイル.png」を一意キーでアップしfeatured_mediaが別IDか（衝突なし） （新規）

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
