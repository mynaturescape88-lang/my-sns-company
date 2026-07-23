---
name: shop-review-layout
disable-model-invocation: true
description: >
  shop-articleの工程「写真配置・レイアウト」のレビュー時に発動。親skill shop-article の写真配置末尾から呼ばれる成果物レビュー（木の内側の子ノード）。各h3最低1枚/2段落ごと1枚の写真・GoogleMapの所在地直下配置・関連記事エリア非新規・WP HTMLの健全性を合否判定する。
---

# shop-review-layout（写真配置・レイアウトの成果物レビュー）

下記の観点がすべて満たしていることを確認する。

- [ ] 各h3に最低1枚・2段落ごとに1枚の写真があるか [[feedback_shop_article_rules]]
- [ ] 冒頭に「訪問日／所在地」のpがあり、外観カテゴリの写真が最初のh3の前に配置されているか（生成器正本の必須構成）
- [ ] GoogleMap埋め込みが所在地直下か（末尾でない）
- [ ] 関連記事エリアを新規作成していないか（テーマ自動表示のため不要）
- [ ] WP HTML：div開閉0／p内ブロック要素なし／Gutenbergタグ混入なし／toc重複なし [[reference_wp_alignfull_section_insert]]
- [ ] 同一被写体・同一構図の写真を連続配置していないか
- [ ] 画像が `[IMAGE:ファイル名]` マーカー形式で本文中に配置されているか

- **fail-closed**：1つでもfailなら提出せず→修正→観点確認へ戻る
- 全passのみ次工程（アイキャッチ選定）へ進める
