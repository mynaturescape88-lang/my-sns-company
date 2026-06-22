# 長尺動画パイプライン（台本駆動・人智の外側方式）

> ⚠️【移行済】**正本は専用スキル `jinchi-longform` へ移行**（題材選び→台本→章別素材台帳→制作→非公開アップの一気通貫・第1弾ナスカの課題を次回デフォルト化済み）。
> 「人智の外側」（@ijin-hiroku・旧称AIインサイト）の長尺を作るときは `jinchi-longform` スキルを発動すること。本ファイルは旧参照として残置（内容は `jinchi-longform/references/pipeline.md` が最新）。
>
> 人智の外側（古代ミステリー×AI・落ち着いたナレで10〜15分）で全工程を確立。
> **技術面はほぼ自動＝ボトルネックは台本/ファクトチェックの質**。
> プロジェクト固有の実パス・チャンネルID・トークン名は
> `.company/secretary/task_logs/ainsight-longform-production-RUNBOOK.md` を正本とする（ここは方法論）。

## 工程（順番）

1. **題材確定 → 台本 → ★入念なファクトチェック**
   - コンセプト適合（AIが人間の不可能を可能にした題材）。
   - 台本＝コールドオープン＋本編6章（hook/setting/technique/theories/ai/outro 相当）。各章＝空行区切りチャンク。
   - **数字はかな/漢字表記**（TTS誤読対策）。FCログを台本に残す。
2. **ナレーション（Gemini TTS・無料）**
   - 無料枠 ~15回/日・JST16時頃リセット。1本=7チャンク前後。429で止まったら枠回復後resume（既存partスキップ）。
   - 音量がチャンク毎にばらつく→全partを `loudnorm I=-17:TP=-1.5` で正規化してから連結。durations不変。
   - **全再生成しない**（冒頭の良いテンポが変わる）。直すのは音量正規化＋該当チャンクのみ。
   - 尺目安：日本語 100文字≒25秒 / 2000文字≒5〜6分（英語は約半分〜60%）。
   - 複数話者が必要なら Google AI Studio（Gemini native TTS）が無料でマルチスピーカー対応。
3. **実写素材（6サイト横断・無料/商用可）** — ブラウザUA必須。**本物素材で作れるものはAIにしない**。
   - 収集は `scripts/fetch_broll.py --query "..." --type video|image [--download N]` を使う（Pexels一本で足りない時の標準）。
   - 横断先：Pexels/Pixabay（要キー・帰属不要）＋NASA/Wikimedia/Openverse/Internet Archive（キー不要）。商用OKのみ・動画は ffprobe で H.264 自動選別（[[reference_gyre_h264_only_codec]]）・帰属要素材はMDに自動でクレジット記録。
   - **Internet Archiveは著作権物の転載が混入し得る**→収益動画で使う前に個別license確認必須。確実に安全＝Pexels/Pixabay/NASA（帰属不要）。
4. **図解（自作・無料）** — 内容は縦中央寄せ・下部~y820までに収め字幕と被らせない。
   - 応用：背景グリーン(#00FF00)の図解PNG→ffmpeg `chromakey=0x00FF00:0.1:0.0` で透過オーバーレイ。
5. **AIの“撮れないシーン”だけ生成（有料・各シーン許可制）** — 実写不可・素材無し・Ken Burnsでも作れないシーンのみ。
   - フリー素材に質感を寄せる（documentary/muted/grain・過度なシネマ/CG化を避ける [[feedback_ai_clip_match_stock_footage]]）。
   - 実物写真あり→start_imageにKling。無し→画像生成1cr→確認→Kling。
   - `get_cost:true` で事前確認→balance記録。preset提案は `declined_preset_id` で辞退。
6. **動画ビルド（ffmpegテンプレ）** — 題材ごとに TITLE/TEASERS/ENDCARD/RECIPE/AI_CLIPS/図解トークンを設定。
   - 確立仕様：字幕=Zen Maru Gothic Bold 44px・黒帯なし白文字黒フチ・左上にグレー50%タイトル常時・章転換=黒画面の問いかけ+ポーン効果音(ナレ無音3s)・末尾終了画面25s。
7. **★字幕同期（Whisper実測・無料）※最重要** — `align_captions.py` で実発話タイミングを取得。
   - 文字数比だと実発話と最大±5秒ズレる→Whisper実測を最優先。align実行後に再ビルド。
8. **BGM＋効果音合成** — BGM=archive.org FreePD(CC0)・volume0.10・末尾afade。SFX=各章頭にadelay配置。`amix normalize=0`。
9. **サムネ（無料・自作）** — 実写＋PIL文字。3案出して選定。
10. **アップロード（非公開）** — privacy=private・defaultLanguage=ja・**`containsSyntheticMedia:True`（AI開示・毎回ON）**・概要欄に目次＋AI明示の免責。
11. **公開（オーナー作業）** — 終了画面パーツをStudioで手動配置。バッファ2〜3本→金/土の夜20-21時JSTで予約公開。**公開GOはオーナーのみ**。

## カメラ語彙8種（Kling/Seedanceのプロンプトに明示する）

| 語彙 | 用途 |
|---|---|
| `slow_zoom_in` | 細部に引き寄せる |
| `dolly_forward` | 奥行き強調・没入感 |
| `orbit_left/right` | 被写体を回り込む |
| `aerial_pullback` | 俯瞰から引いていく |
| `rack_focus` | 前景→後景フォーカス移動 |
| `whip_pan` | 場面転換の勢い |
| `time_lapse` | 時間経過の演出 |
| `static` | 固定カメラ・落ち着いた演出 |

## AIカット プロンプト設計の鉄則

1. **感情時系列をタイムスタンプで記述**：「0-2s: 遺跡の全景 / 2-5s: 文様にゆっくりslow_zoom_in」。
2. **セリフ・口の動き・人物アップ禁止**（SoundモデルでないKling/Seedanceは音声生成不可）。
3. **古代文物・浮世絵の実写化**：「版画の線質を残す」「彩度low」「film_grain: medium」「aged_photograph感」。過度なCG/彩度UPを避ける。
4. **Claude→生成 最適化ループ**：「このシーンの核となる動きを10語以内で言語化して」と先に聞いてからプロンプト化＝試行回数減。
5. **連続生成のラストフレームルール**：前カットのラストに全身（下半身含む）が写っていないと次生成で衣装ランダム化→一貫性崩壊。カメラを引いて全身を収めるか、inpaintで全身補完してから次カットへ。

## モデル使い分け（Kling / Wan2.5 / Veo3.1）

- **解像度テスト先行**：初回std(1280x720)で生成→品質合格のみ1080pへ昇格（リテイクのcr浪費防止）。
- **人物の細かい表情指示（口角・目・眉・顔の傾き）→ Wan2.5優位**（プロンプト忠実度が高い）。
- **カメラワーク（引き・回転・軌道の滑らかさ）→ Kling優位**（動きが滑らか・背景崩れ少）。
- **品質最優先カットのみ Veo3.1**（試行コスト高・1本100cr相当）。AIインサイトは WAN2.5（安価）を優先。
- `mcp__claude_ai_HIggsfield__motion_control` で動作方向を数値補正（ドリフト発生時）。

### Veo3.1 プロンプト小技
- キャラ前に「Japanese」＋環境「Japan」で日本語発音改善。
- プロンプト末尾に `no subtitles`。セリフ中に「/」を入れると字幕がランダム出現するバグ→「/」禁止。
- セリフ固定＝`he says [ローマ字日本語]`。縦動画は横画像を90度回転入力で迂回。

## ナスカ第1弾で確立した実制作の勘所（0cr内製・声統一・MCP・4Kアセンブラ）
> 「人智の外側」第1弾ナスカ改修(2026-06)で実証。詳細は各[[memory]]を正本とする。
- **図解・合成カットは0cr内製**（Higgsfield課金不要）[[reference_video_0cr_figures_composites]]：図解=PIL（暗背景/金アクセント/ヒラギノW6・下~240pxは字幕帯で空ける）。地上絵はアルキメデス螺旋が一発でclean。"光が灯る/差す"合成=numpyグロー→ffmpeg `blend=all_mode=screen`で実写に加算（黒地グローは追加cr0）。ハマり所＝①グローは**円形Hann窓**で縁を0に（四角ハロー枠防止）②screen合成は明るい映像で弱い→暗め(夕/曇)ベース③broll素材のファイル名は中身と不一致→**必ずフレーム抽出で目視選定**。
- **Higgsfield MCP実務**[[reference_higgsfield_mcp_video_ops]]：i2vのstart_imageは過去生成の`job_id`を`medias:[{role:"start_image",value:<job_id>}]`で再利用可（再アップ不要）。`get_cost:true`で必ず0cr先読み（Kling std720p sound off=1.5cr/s・Seedance口パク=4.5cr/s最小4s）。presetが返って実生成されない時は`declined_preset_id`でリテラル生成。崩壊カットは末尾にffmpegフェード・トゥ・ブラック(0cr)で暗転救済。口パク出力の内蔵音は捨て最終アセンブリでマスター音声を上乗せ。
- **語り部など"毎回同じ声"はGemini TTS不可**[[reference_tts_voice_consistency_and_4k_assembler]]：同条件でも生成毎に基本周波数が不定(77-108Hz実測)・声の保存/再利用ができず構造的に不向き。恒久策=ElevenLabs保存ボイス(Starter $6/月・30k cr・商用込＝語り部月78本相当)。応急の声統一=`rubberband -3 -p <半音> --formant`一択（声色保持・asetrate/afftdnは別人化/劣化でNG）。半音=`12*log2(目標Hz/現Hz)`。口パクはピッチを変えても口位置=タイミング不変＝同期は崩れない。
- **誤読は章再生成せず文スプライス**[[reference_tts_voice_consistency_and_4k_assembler]]：1語の誤読は該当文だけ再生成→無音境界でスプライス(`splice_sentence.py`)。章丸ごと再生成は語速が変わり浮く。表示字幕は元のまま・TTS入力だけ読みをかな化。
- **4Kアセンブラ＝行アンカー同期**(`build_nazca_4k.py`)[[reference_tts_voice_consistency_and_4k_assembler]]：各カットに開始ナレ行(sl)を持たせWhisper実測の行時刻に合わせる（比例配置は図・口パクがズレる主因）。このffmpeg(brew8.x)はsubtitles/drawtext非搭載→字幕/タイトルはPILでPNG生成しoverlayで焼く。内容ハッシュ・キャッシュ(出力名=md5(種別+素材+尺+字幕窓))で変更カットだけ再書き出し→セクションは`-c copy`連結（タイミングの土台=行アンカーを変えない限り次回は該当箇所のみ数分で差替）。

## コスト早見（AIインサイト実績）
- 有料＝AIカットのみ（kling 7.5cr/本×数カット＝1本35cr前後）。
- 無料＝Gemini TTS(枠15/日)・Pexels・ffmpeg・Whisper・BGM(CC0)・OFLフォント・アップロード。
- 関連メモリ：[[reference_video_pipeline_tool_costs]] / [[project_ainsight_channel_pipeline]]
