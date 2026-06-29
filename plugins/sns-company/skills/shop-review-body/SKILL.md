---
name: shop-review-body
description: >
  shop-articleの工程「本文」のレビュー時に発動。親skill shop-article の本文末尾から呼ばれる成果物レビュー（木の内側の子ノード）。写真にない植物名の非断定・時間/季節/価格/電話/予約/内部管理の非記載・参考プロンプト準拠・オリジナル文・h3直後の本文p・まとめ節・体験記の固有名詞性を合否判定する。
---

# shop-review-body（本文の成果物レビュー）

ショップ紹介記事パイプラインの **本文** 専用の成果物レビュー。親 `shop-article` の本文執筆完了後・写真配置着手前から発動され、本文がルールに適合し断定/カレンダー性のない記事かを合否判定する（Layer2＝工程固有の作り込み合否）。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝本文固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の6観点）
- ③ `.company/sns_accounts/blog.md`（shopの戦略・却下レバーの照合先）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（本文）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の6観点を1つずつ照合（特に観点4＝blog.md戦略と相違ないか）
- ③ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ④ 全passのみ次工程（写真配置）へ進める

## 本文の観点（合格ラインを内包・1つずつ照合）
- [ ] 写真ファイル名に無い植物名を断定せず汎用表現にしたか [[feedback_shop_article_rules]]
- [ ] 時間/季節/価格/電話/予約/内部管理（水揚げ等）を書いていないか [[feedback_shop_article_rules]]
- [ ] 参考プロンプト（正本）を参照して書いたか [[feedback_shop_article_rules]]
- [ ] Web情報をそのまま引用せずオリジナルの言葉にしたか （新規）
- [ ] 見出しはh3のみ・h3直後に必ず本文pが続くか [[feedback_shop_article_rules]]
- [ ] 末尾に「まとめ」節（h3＋本文）があるか （新規）
- [ ] 全体2500字以上・まとめを除く各本文セクションが300字以上か（薄い記事を通さない・生成器正本の必須量）（新規）
- [ ] 体験記要素は固有名詞・実体験ベースで一般化していないか [[feedback_shop_article_rules]]

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
