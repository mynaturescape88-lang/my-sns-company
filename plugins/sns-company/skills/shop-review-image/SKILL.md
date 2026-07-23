---
name: shop-review-image
disable-model-invocation: true
description: >
  shop-articleの工程「アイキャッチ・画像セット」のレビュー時に発動。親skill shop-article のアイキャッチ/ig画像セット確定後から呼ばれる成果物レビュー（木の内側の子ノード）。アイキャッチと本文1枚目の差異・ブログサムネイル命名・ig_images先頭のinstagramサムネイル・同名アップ時のfeatured_media別ID（衝突なし）を合否判定する。
---

# shop-review-image（アイキャッチ・画像セットの成果物レビュー）

下記の観点がすべて満たしていることを確認する。

- [ ] アイキャッチと本文1枚目が異なる写真か [[feedback_shop_article_rules]]
- [ ] アイキャッチが「ブログサムネイル_〇〇.png」か（Instagramサムネと混同なし）
- [ ] ig_images先頭が「instagramサムネイル_〇〇.png」か
- [ ] 同名「サムネイル.png」を一意キーでアップしfeatured_mediaが別IDか（衝突なし）
- [ ] アイキャッチは優先順位①②③（①既存サムネイル最優先→②YouTube「店名のみ」画像→③自作せずオーナー確認）に従って選定したか

- **fail-closed**：1つでもfailなら提出せず→修正→観点確認へ戻る
- 全passのみ次工程（SEOタイトル・メタ）へ進める
