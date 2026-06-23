# @mynaturescape 長尺＝1発生成（実写・植物/イベント・人物なし・BGM）

> 「あじさいの素材で動画を作って」「mynaturescapeの長尺を編集して」等で発動。素材フォルダ＋名称（＋サムネ文字色）を渡せば、下記エンジンが**スコアリング→人物除去→組み立て**まで自走し4K完成mp4を出す。最終確認はオーナー。
> 確立＝2026-06-16〜17・旧中川あじさいパイロットで実証。正本ログ＝plant-blog-bots `.company/secretary/task_logs/mynaturescape-longform-pilot.md`。

## エンジン
- スクリプト：plant-blog-bots `video_editing/mynaturescape_longform.py`（$0・ffmpeg/OpenCV/Pillow/librosa・ヘッドレス）
- venv：`.venv-video`（opencv-python-headless / Pillow / librosa）
- フォント：`/System/Library/Fonts/ヒラギノ丸ゴ ProN W4.ttc`（サムネと一致の丸ゴ）

## 3ステップ（コマンドは逐語コピー可）
```bash
cd <plant-blog-bots>; source .venv-video/bin/activate

# 1) スコアリング（可変尺5〜9秒・focus/bright/shakeで良区間抽出）
python video_editing/mynaturescape_longform.py score \
  --src-glob '/path/to/IMG_*.MOV' --out video_editing/<name>_segments.json

# 2) 人物除去（顔特定 or 約3秒以上写り続ける人物のカットだけ除外。遠景小/映り込みは保持）
python video_editing/mynaturescape_longform.py filter-people \
  --scores video_editing/<name>_segments.json \
  --out   video_editing/<name>_segments_nopeople.json

# 3) 組み立て（散らし＋可変尺＋フック＋イントロ/ラベル/フェード＋エンディング2連＋エンドロール＋連続BGM）
python video_editing/mynaturescape_longform.py assemble \
  --scores video_editing/<name>_segments_nopeople.json \
  --out drafts/<name>-4k.mp4 \
  --label "<画面に出す名称>" --color 0xRRGGBB \
  --bgm "<曲ファイル>"            # 省略可＝assets/bgm_pool から直近10未使用を自動選曲 \
  --ending "/Users/Papa/Downloads/SNS関連/youtube_ending3.MOV" \
  --endroll video_editing/endroll_master.txt \
  --res 4k
```
高速確認は `--fast`（480p）/ 本数制限は `--limit N`。

## 確定仕様（オーナー承認済・2026-06-17）
- **解像度**：4K(3840×2160)。素材1080p→ランチョスアップスケール＋低CRF＋弱シャープ（真の4Kは要4K撮影）。
- **色**：自然志向シネマ調＝彩度0.97/軽コントラスト/色相不変（花の悪目立ち回避）。
- **構成**：冒頭フック（見せ場の高速モンタージュ 0.9秒×8・本編の見せ場プレビュー＝**映像重複OK**）→イントロ大タイトル(色連動・**帯なし/フチなし・ソフト影のみ**・1〜10秒)→本編（左上ラベル常時）→本編4秒映像フェード→エンディング(youtube_ending3 を2連)→開始3秒後から30秒スクロールのエンドロール。
- **本編の並べ**：**散らし配置（位相分散）**・同一素材の連続/近接回避・**重複/ループNG**（フックのみ重複OK）。
- **カット尺**：**BGMの強弱に同期した可変尺**。本編は**最短5秒**（静か→長回し最大9秒・展開→短め）。フックは別（高速）。
- **音**：BGMは**フック＋本編＋エンディングを通して連続**。**エンディング素材音はミュート**。**全体末尾4秒でBGMフェードアウト**。BGMは無音除去＋クロスフェードでシームレスループ・音量0.5。
- **BGM選曲**：`assets/bgm_pool/` から直近 `--dedup`（既定10）動画で未使用の曲を自動選曲（台帳 `video_editing/used_bgm.json`）。プールはオーナーがYouTubeオーディオライブラリ（商用可・Content ID無し）から格納。明示`--bgm "曲ファイル"`があれば優先。⚠️**プールの曲数が選曲の前提＝1曲だと毎回同じになる**。「被っても約N本に1回」を満たすにはプールに**N曲以上**入れ`--dedup N`にする（例：20本に1回希望→20曲以上＋`--dedup 20`）。曲数<NだとN本待たずに一周する。
- **人物**：顔が特定できる or 約3秒以上写り続けるカットは除外／遠景・小・一瞬の映り込みはOK（肌色ゲートで葉花の誤検出を排除）。
- **尺**：最低8分（広告）・長いほど良い。20分超はシリーズ分割。
- **エンドロール**：`video_editing/endroll_master.txt`（撮影協力店舗＆イベントの最新マスター・色は現状黄）。ショップ紹介動画なら当該店を末尾追記（イベント動画は追記保留）。
- **キャプション/タイトル/サムネ**：公開済み動画の構成を踏襲（YouTube Data APIで現行動画の概要欄を取得して同型で作る）。サムネは固定デザイン・イントロ文字色＝サムネ文字色。

## 探訪モード＝単一の連続素材（--continuous）（2026-06-23 追加）
- **「1本の連続した店内探訪」素材は散らし配置・BGM同期再カットを使わない**＝`--continuous`。バーチャル店探訪は時系列の連続感が命で、散らすと空間が前後してしまう。
- 動き：時系列のまま、連続する良区間を**長いランに結合**（隙間も残して微小カットを作らない）→ 人物/速パン等で除去した数少ない境目だけ**短いクロスフェード**（`--xfade 0.3`）で繋ぐ。素材がブレ少なければ実質ほぼノーカット。
- **冒頭フックは併用可**（`--hook 8`）。本編の連続性とは独立に冒頭へ前置する。単一素材だとフックの"別クリップ優先"が効かないので、コードは2巡目で同一素材の次点区間を補充して本数確保する。
- 多数イベント素材（旧中川あじさい等）は従来どおり散らしモード（`--continuous`なし）。

## 冒頭タイトルの体裁（2026-06-23 オーナー確定・上書き）
- **グレー半透明帯は廃止／フチ（縁取り）も付けない／ソフト影のみ**でぼかして視認性を確保（明るい店先でも沈まない）。ごちゃつかせない。
- **フォント＝過去動画/サムネ寄りの角ゴ**＝`TITLE_FONT=/System/Library/Fonts/ヒラギノ角ゴシック W6.ttc`（丸ゴは不採用）。左上ラベル/エンドロールは従来`FONT`(丸ゴ)のまま。
- **色＝サムネ文字色の実測値**（サムネPNGから最頻の明黄系画素を抽出し`0xRRGGBB`で`--color`へ。例：Lumière=0xFFE26B）。

## 4Kの所要時間と高速化（2026-06-23）
- 4K`--preset medium`は10分本編のアップスケールで**約3時間**＝非現実的。**`--preset veryfast`で約1.5h**に短縮（アップスケール由来の体感画質差は小・YouTube側で再エンコードされる）。`--preset`引数で指定。
- **BGM差し替えは映像非依存で後付け可**＝完成mp4の映像をコピーし音声だけ再mux（`/tmp/swap_bgm.py`が`trim_silence`→`build_bgm_loop`→volume0.5/末尾4秒フェードを流用）。BGM選定が後から変わっても4K再レンダ不要。
- 量産時は1080p納品やハードウェアエンコード(`h264_videotoolbox`)の検討余地（要相談）。

## 主なパラメータ
- `--pan-pct 0.03 --pan-floor 0.020`：**速いパンの精密除去**（動き量上位3%かつ床0.02超のカットのみ＝本当に速い数カットだけ。閾値を下げすぎる過剰除外をしない）。手動上書きは`--pan-max <値>`。
- `--continuous`（探訪モード）／`--xfade 0.3`（ラン境目のクロスフェード秒）／`--preset`（x264 preset・4Kは`veryfast`推奨）。
- `--hook 8 --hook-cut 0.9`：フックのカット数と1カット長。
- スコアリング定数：`SEG_MIN=5.0 / SEG_MAX=9.0`（本編カット尺レンジ）、`SHAKE_MAX=0.045`（明らかな手ブレ除外）。

## 品質ゲート（[[editing-principles]] も必ず適用）
- **高速パン・速い場面切り替えを残さない**（癒し系の世界観）。自動除去で取りこぼす場合は**オーナーの秒数指定→該当カットをピンポイント除外**（提示版の「再生時刻→素材」対応で照合）。
- **カット尺に抑揚**（一定尺=単調を回避・BGMのテンポ/静動に同期）。
- 提出前に自己レビュー＝既知の粗ゼロ（[[feedback_review_means_finished_no_known_gaps]]）：8分以上・人物大写りなし・色が自然・イントロ/ラベル/エンドロール正常・BGM無音/末尾フェードを実測確認。
- 出力動画(.mp4)・BGM音源はローカル保持＝gitに上げない（[[feedback_video_assets_local_not_github]]）。
