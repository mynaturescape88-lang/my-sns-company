# WordPress REST/HTML 操作の落とし穴と対処（実装時の正本）

> MEMORYから移送（2026-06-22）。WordPress記事をAPI/HTMLで作成・更新するとき、実装前に該当項を確認する。実体験で確認した不具合と確実な回避策。

## エディタではJSが動かない（プレビューはフロントで）
Gutenberg「カスタムHTML」ブロックは内部iframeでプレビューするため jQuery も CDN の Slick.js も動かない。動作確認は必ず「プレビュー→新しいタブ」（フロントエンド）で行う。
（カルーセルが「動かない」と報告→実際はフロントで正常だった事例）

## wpautop による HTML構造破壊
REST APIで content を送ると表示時に `wpautop` が走りHTMLが壊れる。
- 破壊パターン：`<style>`内の空行が`</p><p>`化／`<a>`開始直後の改行が`<br>`化で画像が外へ／カード間の空行が`<p>`ラップでグリッド崩れ。
- 対処（確実）：コンテンツ全体を `<!-- wp:html -->` / `<!-- /wp:html -->` でラップ（wpautop完全バイパス）。CSSは1ルール1行・空行なし。`<a>`直後に改行を入れない（`<a href="..."><img ...>`を同一行）。カード間に空行を入れない。
（2026-05-27確認）

## `<script>`内の `&&` エンコード問題（2026-06-01）
`<!-- wp:html -->`内の`<script>`でもWordPressが`&&`を`&#038;&#038;`にエンコード→スクリプト全体がSyntaxErrorで無効化（静的HTMLは出るがJSは一切動かない）。
- 対処：`&&`を使わずネスト`if`で代替。`if(d&&d.events)` → `if(d){ if(d.events){ ... } }`

## ショートコード名とJS変数名の衝突（2026-06-01）
`<script>`内で `someArray[date]` と書くとプラグインがショートコード`[date]`として処理し`someArray2026/06/01`等の不正JSに変換（症状：`ReferenceError`）。
- 対処：ショートコード名と同名の変数をブラケット記法で使わない。`date`→`dt`、`category`→`cat`等に改名。一般語（date/post/page）は要注意。

## iframeを段落ブロックの「内側」に入れると保存時に除去される（2026-06-06）
GoogleMap等の`<iframe>`を`<!-- wp:html -->`で包んでも、wp:paragraphブロックの内側（`</p>`直後・`<!-- /wp:paragraph -->`手前）に挿入すると保存時のブロック検証でiframeだけ除去される。
- 対処：iframeのwp:htmlブロックは段落ブロックの**直後**（`<!-- /wp:paragraph -->`の後）に独立した兄弟ブロックとして挿入。挿入位置探索は`</p>`でなく`<!-- /wp:paragraph -->`基準に。
（`_assemble_html`が`</p>`直後挿入で剥がれていた→基準変更で解決。関連: shop-article）

## REST APIのサムネイル取得
- `_fields`に`_embedded`を含めないと`_embed`指定でも埋め込みが返らない。対処＝`_fields`を指定しない（全取得）か`_embedded`を明示。
- サムネ優先順位：`medium_large` > `large` > `medium` > `thumbnail`（小サイズ拡大は劣化）。

## REST APIのスラッグ形式
- 返る`slug`はURLエンコード形式（日本語は`%e3%82%...`）。CSVの`wp_page_url`末尾セグメントもエンコード済→unquote不要で直接照合可。

## 同名画像のメディア名衝突（2026-06-05）
各店「サムネイル.png」を`plant_{md5(path.stem)[:8]}`でアップロードすると stem が全店同一→safe名が衝突し、検索再利用ロジックが最初の1枚を返して全記事が同じアイキャッチを共有（5記事で media2258 共有事例）。
- 対処：同名画像を複数アップロードする時はフォルダ等を含めた一意キーで（例 `thumb_{md5(folder)[:8]}.png`）。設定後は各postの`featured_media`が別IDか必ず検証。誤共有メディアは`force=true`で削除。

## メタ説明の設定
メタ説明は`the_page_meta_description`（テーマ独自フィールド）→ XML-RPC経由で設定。

## 参考：importlib.util
```python
# 誤: ins = importlib.util.load_from_spec(spec)
# 正:
ins = importlib.util.module_from_spec(spec)
spec.loader.exec_module(ins)
```
