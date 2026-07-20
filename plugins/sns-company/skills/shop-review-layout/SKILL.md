---
name: shop-review-layout
disable-model-invocation: true
description: >
  shop-articleの工程「写真配置・レイアウト」のレビュー時に発動。親skill shop-article の写真配置末尾から呼ばれる成果物レビュー（木の内側の子ノード）。各h3最低1枚/2段落ごと1枚の写真・GoogleMapの所在地直下配置・関連記事エリア非新規・WP HTMLの健全性を合否判定する。
---

# shop-review-layout（写真配置・レイアウトの成果物レビュー）

ショップ紹介記事パイプラインの **写真配置・レイアウト** 専用の成果物レビュー。親 `shop-article` の写真配置完了後・アイキャッチ選定着手前から発動され、写真配置と WP HTML が健全かを合否判定する（Layer2＝工程固有の作り込み合否）。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝写真配置・レイアウト固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の7観点・★観点7＝権利・合法性）
- ③ `.company/sns_accounts/blog.md`（shopの戦略・却下レバーの照合先）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（写真配置済みレイアウト）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の7観点を1つずつ照合（★観点7＝権利・合法性はfail-closed／特に観点4＝blog.md戦略と相違ないか）
- ③ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ④ 全passのみ次工程（アイキャッチ選定）へ進める

## 写真配置・レイアウトの観点（合格ラインを内包・1つずつ照合）
- [ ] 各h3に最低1枚・2段落ごとに1枚の写真があるか [[feedback_shop_article_rules]]
- [ ] 冒頭に「訪問日／所在地」のpがあり、外観カテゴリの写真が最初のh3の前に配置されているか（生成器正本の必須構成）（新規）
- [ ] GoogleMap埋め込みが所在地直下か（末尾でない） （新規）
- [ ] 関連記事エリアを新規作成していないか（テーマ自動表示のため不要） （新規）
- [ ] WP HTML：div開閉0／p内ブロック要素なし／Gutenbergタグ混入なし／toc重複なし [[reference_wp_alignfull_section_insert]]

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
