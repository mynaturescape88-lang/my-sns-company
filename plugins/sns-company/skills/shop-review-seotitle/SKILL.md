---
name: shop-review-seotitle
disable-model-invocation: true
description: >
  shop-articleの工程「SEOタイトル・メタ」のレビュー時に発動。親skill shop-article のタイトル/メタ確定後・公開操作着手前から呼ばれる成果物レビュー（木の内側の子ノード）。変更履歴を確認したコロコロ変更回避（メタ=excerpt）・タイトル/メタ文字数・誇大でない結果数字ベース・エビデンス提示を合否判定する。
---

# shop-review-seotitle（SEOタイトル・メタの成果物レビュー）

下記の観点がすべて満たしていることを確認する。

- [ ] SEOタイトル/メタは変更履歴を確認しコロコロ変えていないか（メタ=excerpt） [[feedback_seo_title_meta_no_flipflop]]
- [ ] タイトル25〜45字・メタ80〜160字か [[wordpress-publishing基準]]
- [ ] タイトルが結果・数字ベースで誇大でないか
- [ ] SEOタイトル・メタを確定したことをエビデンス（実際の文言）で示せる状態であること

- **fail-closed**：1つでもfailなら提出せず→修正→観点確認へ戻る
- 全passのみ次工程（IGキャプション執筆）へ進める
