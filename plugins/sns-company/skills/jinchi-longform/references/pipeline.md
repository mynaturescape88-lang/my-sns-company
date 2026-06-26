# 人智の外側 長尺パイプライン（11工程・実制作同期版）

> 第1弾ナスカ（2026-06）で全工程を確立・実証。**技術はほぼ自動＝ボトルネックは台本/FCの質**。
> プロジェクト固有の実パス・チャンネルID・トークン名・スクリプト名は
> `.company/secretary/task_logs/ainsight-longform-production-RUNBOOK.md` を正本（ここは方法論）。
> 実行python＝**system `python3`**（PILあり）。Whisperだけ **`.venv/bin/python`**（faster-whisper）。

## 1. 題材確定
- コンセプト適合＝**AIが人間に不可能だったことを可能にした題材**（古代ミステリー）。第1弾ナスカ／第2弾ヘルクラネウム炭化巻物。
- 題材キュー・cost前提・公開運用は `.company/sns_accounts/youtube_ainsight.md`。

## 2. 台本＋★入念なファクトチェック
- **🔴台本前に `content-planning` スキルを必ず発動**（企画チェック6項目＋起承転結フロー＋#3-cバズ構造モデリング）。
- 構成＝**コールドオープン＋【起】1章【承】3章【転】2章【結】2章＋語り部（案内人）3挿入**（開幕／中盤②＝転→結／ED＝コーダ＋次弾匂わせstinger）。各章＝空行区切りチャンク。
- **語り部の出入りには必ず「間（ま）」**＝入り[ナレ終話→約0.5s黒フェード→約1.0s無言のため→案内人が話す]／戻り[終話→約0.8s余韻→約0.5sフェード→本編復帰]。暗室はBGM/環境音を本編と変える（静けさ寄り）。
- **数字はかな/漢字表記**（TTS誤読対策）。**FCログを台本末尾に残す**（公開前ゲート）。誤読は工程4で文スプライス。
- 尺見積り＝**ナレ字数 ÷ 312字/分（実測）＋語り部約330字＋章転換無音/stinger余白**。15分≒ナレ約3,900〜4,400字。
- 原稿＝`drafts/<topic>-narration-text-v2.txt`（空行で章、改行で字幕行）。

## 3. ★章別素材台帳を作る（制作の背骨）
- `references/ledger-template.md` に従い**再生順マスターショットリスト**を作る。各カット＝[# / シーン / セリフ要約 / 画 / 区分①〜⑥ / 尺 / 予cr / 実cr / 済 / 備考]。
- **品質優先（2026-06-18確定）**：①各カットで最高品質の材料を先に決める（実写/0cr画像/図解/AI動画）②無理に画像にしない＝動き・順次性・実在感が要る所はAI動画③そのうえで予crを積算→**1本上限300cr**と照合→超過時のみ最小劣化で削る。
- **cr＝予cr（get_cost見積）と実cr（実消費）を分けて記録**。生成済み・既存沈没素材も実crを記入（0にしない）。
- 台帳は task_log のインライン表より優先する**単一の正本**（例＝`drafts/nazca-cr-ledger.md`）。

## 4. ナレーション（Gemini TTS・無料）
- `python3 scripts/gemini_tts.py tts --file drafts/<topic>-narration-text-v2.txt --voice Charon --style "落ち着いた低い声で、ミステリー番組のナレーターのように。" --output drafts/<topic>-narration.mp3 --partsdir assets/<topic>_broll/_audio`
- ⚠️無料枠 ~15回/日・JST16時頃リセット。429で止まったら枠回復後にresume（既存partスキップ）。
- ⚠️チャンク毎に音量がばらつく→全partを `loudnorm I=-17:TP=-1.5` で正規化してから連結（durations不変）。
- ⚠️**全再生成しない**（冒頭の良いテンポまで変わる）。直すのは音量正規化＋該当チャンクのみ。**誤読は章再生成せず文スプライス**（`splice_sentence.py`・無音境界）。
- 尺目安＝日本語100字≒25秒／2000字≒5〜6分。
- ⚠️**語り部の「毎回同じ声」はGemini TTSでは不可**（基本周波数が生成毎に不定77-108Hz）。語り部は工程7の口パク動画の内蔵音 or 保存ボイス（ElevenLabs）で担保。`lessons-learned.md`参照。

## 5. 実写素材（6サイト横断・無料/商用可）
- `scripts/fetch_broll.py --query "..." --type video|image [--download N]`。**ブラウザUA必須**。
- 横断＝Pexels/Pixabay（要キー・帰属不要）＋NASA/Wikimedia/Openverse/Internet Archive（キー不要）。**Internet Archiveは著作権物混入可→収益動画で使う前に個別license確認**。確実に安全＝Pexels/Pixabay/NASA。
- 動画は ffprobe で H.264 自動選別（[[reference_gyre_h264_only_codec]]）。**素材ファイル名は中身と不一致しがち→必ずフレーム抽出で目視選定**。
- 原則＝**本物で作れるものはAIにしない**（空撮・地形・小道具はstock化でcr節約／人物の古代再現はstock不可＝AI必然）。

## 6. 図解・★合成（0cr内製・Higgsfield課金不要）
- 図解＝PIL（`make_diagrams.py`系）。ブランド統一＝**暗背景／金アクセント／ヒラギノ角ゴW6**。内容は縦中央寄せ・**下~240pxは字幕帯で空ける**。**図解2連続は禁止**（間に実写/画像を挟む）。地上絵はアルキメデス螺旋が一発でclean。
- ★“光が灯る/差す”合成＝numpyグロー（**円形Hann窓で縁を0に**＝四角ハロー枠防止）→ffmpeg `blend=all_mode=screen` で実写に加算。**screen合成は明るい映像で弱い→暗め（夕/曇）ベース**。追加cr0。
- 応用＝背景グリーン(#00FF00)図解→`chromakey=0x00FF00:0.1:0.0` で透過オーバーレイ。

## 7. AIの“撮れないシーン”だけ生成（有料・各カット許可制）
- 原則＝実写不可・素材無し・Ken Burnsでも作れないシーンのみ。**フリー素材に質感を寄せる**（documentary/muted/grain・過度なシネマ/CG化を避ける [[feedback_ai_clip_match_stock_footage]]）。
- **区分④語り部口パク＝Seedance 2.0 std720p**（4.5cr/s・最小4s）。**確立済レシピ**＝画像1枚（role=image＝マットな顔close-up）＋audio・**start_imageなし**・CRAFT最小肯定文（**肌/照明/否定語は一切書かない**）＋映画用語カメラ（Tracking/Truck/locked/push-in）＋ショット種別（膝上等）。正本＝`video-editing`スキル `seedance-2.0-prompting.md`§6。**2画像差し（start_image+image）は失敗→1枚に限定**。
- **区分⑤無音モーション・⑥再現カット＝Kling v3.0 std720p sound off**（7.5cr/カット・5s基準）。実物写真あり→start_imageにKling（過去生成の`job_id`を`medias:[{role:"start_image",value:<job_id>}]`で再利用可）。無し→画像生成1cr→確認→Kling。
- **`get_cost:true` で必ず0cr先読み→残高記録**。presetが返り実生成されない時は`declined_preset_id`でリテラル生成。
- **生成後ゲート**＝採用前に額/鼻/頬を拡大し**てかり/老け**を点検→passのみ採用（`lessons-learned.md`）。崩壊カットは末尾にffmpegフェード・トゥ・ブラック(0cr)で救済。
- 顔の一貫性＝soul_2（soul_id=`ca82d851`・髭なし）。soul_2は画面内に文字を幻覚→暗い下地ならcv2 inpaint or クロップで除去。
- 生成物＝`assets/<topic>_broll/_ai/`、語り部動画＝`drafts/<topic>-guide-video/`。

## 8. 4Kアセンブル（`build_nazca_4k.py` を題材別にテンプレ流用）★旧 build_nazca_video.py は使わない
- 設計＝**音声駆動セクション尺＋行アンカー同期**。各カットに開始ナレ行(sl)を持たせ、**Whisper実測の行時刻**に合わせる（比例配置は図・口パクがズレる主因）。章＝1音声源を共有する連続カット群。映像尺を音声尺へ厳密一致（不足は自動bridge延長／過多は自動トリム）。
- 題材ごとに設定＝SECTIONS（章キー・kind=nar/guide/sil・audio・txt・cuts）／TITLE・TEASERS（章の問い）・ENDCARD_MSG（毎回変える）。
- 仕様＝字幕（Zen Maru Gothic/ヒラギノ系・黒帯なし白文字黒フチ）・左上にグレー50%タイトル常時・**章転換＝黒画面の問い＋ポーン効果音（ナレ無音）**・末尾**終了画面25s**。**🔴口パク動画は内蔵音（＝audio入力に渡したクローン声がリップシンクされた音）をそのまま使う＝generate_audioをfalseにしない・内蔵音を捨てて別音声を上乗せしない**（口は内蔵音にしか同期せず、後から別音声を当ててもViseme/タイミングが合わずロックしない＝第2弾ヘルクラでcr157.5浪費して確定）。BGM/SEは内蔵音の下に敷くだけ。
- 書き出し＝**25セクション分割→`-c copy`ロスレス連結**（veryfast/crf20・lanczos拡大）。**内容ハッシュでキャッシュ**（出力名=md5(種別+素材+尺+字幕窓)）→変更カットだけ数分で差替。
- ⚠️**画像差替えは必ず新パス or 該当カット削除**（PATHキャッシュ対策＝同名上書きは旧画が残る）。

## 9. ★Whisper字幕同期（最重要・無料）
- `.venv/bin/python scripts/nazca4k_whisper.py`（または `align_captions.py`）→ 行ごとの実発話時刻 → アセンブラの行アンカーへ。文字数比だと実発話と最大±5秒ズレる。**align後に再ビルド**。
- ⚠️**この環境のffmpeg（brew 8.x）はsubtitles/drawtextフィルタ非搭載**→字幕/タイトル/ブランドは**PILでPNG生成しoverlayで焼く**方式（`build_nazca_4k.py`に内蔵）。

## 10. BGM＋効果音
- 音声＝ナレ＋語り部＋無音をセクション連結→loudnorm→**BGMサイドチェインダッキング**→SE配置→alimiter。
- BGM＝archive.org FreePD(CC0)・volume0.10・末尾afade。SE＝`assets/sfx/pon.wav`等を各章頭に`adelay`配置。

## 11. サムネ→非公開アップ→（公開はオーナー）
- サムネ＝**着手前にウェブ調査で同ジャンル高再生サムネをベンチマーク→勝ちパターンを真似る** [[feedback_research_proven_thumbnails_before_creating]]（10字以内/2-3色/ダーク+1アクセント/単一被写体）。実写＋PIL文字。3案出して選定。
- アップ＝`python3 seo_growth/upload_ainsight_video.py`（題材ごとに VIDEO/THUMB/TITLE/DESCRIPTION/TAGS/目次を差替）。**privacy=private・defaultLanguage=ja・category27・`containsSyntheticMedia:True`（毎回ON）・概要欄に目次(0:00..)＋AI明示の免責**。
- ⚠️目次（章タイムスタンプ）は**timeline.json（4Kビルドが出力するセクション開始時刻）から正確に作る**（旧版の目次を流用しない）。
- 認証＝`IJIN_YOUTUBE_TOKEN`。失効時 `python3 seo_growth/setup_ijin_oauth.py`（人智の外側chを選択）。**OAuthがTestingだと7日で再失効→継続運用はProduction公開**。
- 公開（オーナー作業）＝終了画面パーツをStudioで末尾25s枠に配置→**バッファ2〜3本→金/土の夜20-21時JSTで予約公開**（コールドスタート対策）。**公開GOはオーナーのみ**。

---

## カメラ語彙8種（Kling/Seedanceのプロンプトに明示）
| 語彙 | 用途 | 語彙 | 用途 |
|---|---|---|---|
| `slow_zoom_in` | 細部に寄せる | `rack_focus` | 前景→後景フォーカス移動 |
| `dolly_forward` | 奥行き・没入 | `whip_pan` | 場面転換の勢い |
| `orbit_left/right` | 被写体を回り込む | `time_lapse` | 時間経過 |
| `aerial_pullback` | 俯瞰から引く | `static` | 固定・落ち着き |

## AIカット プロンプト設計の鉄則
1. **感情時系列をタイムスタンプで記述**（「0-2s: 遺跡の全景 / 2-5s: 文様にslow_zoom_in」）。
2. **セリフ・口の動き・人物アップ禁止**（Sound非対応のKling/Seedanceは音声生成不可）＝口パクは④の専用レシピで。
3. **古代文物・浮世絵の実写化**＝「版画の線質を残す」「彩度low」「film_grain: medium」「aged_photograph感」。過度なCG/彩度UPを避ける。
4. **Claude→生成 最適化ループ**＝「このシーンの核となる動きを10語以内で言語化して」と先に聞いてからプロンプト化＝試行回数減。
5. **連続生成のラストフレームルール**＝前カットのラストに全身（下半身含む）が写っていないと次生成で衣装ランダム化→一貫性崩壊。引いて全身を収めるか inpaintで補完してから次カットへ。
6. **プロンプトはシンプルに肯定で**＝写実語・否定語を盛るほど逆効果（てかり増・要素追加）。最小肯定文がマット良好 [[feedback_simple_positive_prompt_no_negatives]]。**しわ/aged/deep texture等の肌描写は書かない**（顔が老ける）[[feedback_ai_face_prompt_no_wrinkles]]。

## モデル使い分け（Kling / Wan2.5 / Veo3.1 / Seedance）
- **解像度テスト先行**＝初回std(1280x720)→品質合格のみ1080p昇格（リテイクのcr浪費防止）。
- 人物の細かい表情指示→**Wan2.5優位**（プロンプト忠実度）。カメラワーク（引き/回転/軌道）→**Kling優位**（滑らか・背景崩れ少）。語り部口パク→**Seedance 2.0**（§6レシピ）。品質最優先カットのみVeo3.1（試行コスト高・1本100cr相当）。
- Veo3.1小技＝キャラ前に「Japanese」＋環境「Japan」で日本語発音改善／末尾 `no subtitles`／セリフ中の「/」禁止（字幕バグ）。
- `motion_control` MCPで動作方向を数値補正（ドリフト時）。

## コスト早見（人智の外側 実績）
- 有料＝AIカットのみ（Kling 7.5cr/カット・Seedance口パク4.5cr/s）。**1本上限300cr（オーナー確定）**。第1弾ナスカ実消費＝約170cr。
- 無料＝Gemini TTS(枠15/日)・Pexels等・ffmpeg・Whisper・BGM(CC0)・OFLフォント・アップロード。
- 関連＝[[reference_video_pipeline_tool_costs]] / [[project_ainsight_channel_pipeline]] / [[reference_video_0cr_figures_composites]] / [[reference_higgsfield_mcp_video_ops]]
