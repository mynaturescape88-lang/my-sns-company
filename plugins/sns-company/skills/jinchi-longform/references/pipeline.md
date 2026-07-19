# 人智の外側 長尺パイプライン（11工程・実制作同期版）

> 第1弾ナスカ（2026-06）で全工程を確立・実証。**技術はほぼ自動＝ボトルネックは台本/FCの質**。
> プロジェクト固有の実パス・チャンネルID・トークン名・スクリプト名は
> `.company/secretary/work/task_logs/2026-06/ainsight-longform-production-RUNBOOK.md` を正本（ここは方法論）。
> 実行python＝**system `python3`**（PILあり）。Whisperだけ **`.venv/bin/python`**（faster-whisper）。

## 1. 題材確定
- コンセプト適合＝**AIが人間に不可能だったことを可能にした題材**（古代ミステリー）。第1弾ナスカ／第2弾ヘルクラネウム炭化巻物。
- 題材キュー・cost前提・公開運用は `.company/sns_accounts/youtube_ainsight.md`。

## 2. 台本＋★入念なファクトチェック
- **🔴台本前に `content-planning` スキルを必ず発動**（企画チェック6項目＋起承転結フロー＋#3-cバズ構造モデリング）。
- 構成＝**コールドオープン＋【起】1章【承】3章【転】2章【結】2章＋各章転換teaser＋ED（コーダ＋次弾匂わせstinger）**（The Life Guide型＝ホスト非表示・純ナレーション）。**各章＝独立した1ファイル（章別txt）**＝TTSは「1章=1リクエスト」で生成する（工程4・段落分割しない）。stinger（次弾匂わせ）は純ナレ＋字幕で成立させる（語り部に依存しない）。
- **章転換の「間（ま）」は工程8の黒画面章転換で担保**（黒みフェード＋無音＋ポーン効果音）。
- **数字はかな/漢字表記**（TTS誤読対策）。**FCログを台本末尾に残す**（公開前ゲート）。誤読は工程4で文スプライス。
- 尺見積り＝**ナレ字数 ÷ 312字/分（実測）＋章転換無音/stinger余白**。15分≒ナレ約3,900〜4,400字。
- 原稿＝`.company/creator/work/youtube-jinchi/<topic>/<topic>-narration-clean/` に**章別txt（1ファイル=1章・改行で字幕行）**を置く（旧・単一ファイル空行区切り方式は廃止）。1章=1リクエストでTTSし章内の声の揺れをゼロにする（工程4）。

## 3. ★章別素材台帳を作る（制作の背骨）
- `references/ledger-template.md` に従い**再生順マスターショットリスト**を作る（**テンプレが唯一の正本＝前作の台帳を写さない**）。各カット＝[# / VO要約 / 画 / 区分①②③★(間) / 実写① / 尺s / 素材ID / 済 / 入手経路・注記]（**A表9列固定**）。
- **品質優先（2026-06-18確定）**：①各カットで最高品質の材料を先に決める（実写/0cr画像/図解/★合成）②無理に画像にしない＝動き・順次性・実在感が要る所は実写素材を探し切る。
- **cr（Higgsfieldクレジット）概念は廃止**＝Higgsfield完全解約（2026-07-07失効）で有料AIカット自体が選択肢に無く**全カット0cr確定**。予cr/実cr列・cr収支節・「1本上限300cr」照合は台帳に持ち込まない（`ledger-template.md` 冒頭・`lessons-learned.md` §I）。
- 台帳は task_log のインライン表より優先する**単一の正本**（現行の実証済み例＝`.company/creator/work/youtube-jinchi/ai-vulnerability/ai-vulnerability-cr-ledger.md`）。**ファイル名は `<topic>-cr-ledger.md` のまま＝cr節を廃止しても変更しない**。
- **★素材の合法性（絶対基準・恒久2026-07-19 オーナー指示）**：**使用する動画素材（ロゴ・画像・映像・公式画面スクショ等、画面に映る全アセット）は、すべて合法であること。** 合法だと確認できない素材は使用不可＝この基準を満たさない章は完了・提示・公開しない。

## 4. ナレーション（Gemini TTS・無料）＝1章1リクエスト方式
- **確立手法＝`<topic>-narration-clean/` の章別txt（1ファイル=1章）を「1章=1リクエスト」でTTS**（`\n\n`段落分割はしない＝章内の声の揺れをゼロにする）。参照実装＝`scripts/ghost_hominin_gemini_nar.py`（章ごとにcutoff/loop検出リトライ＋端無音トリム＋loudnorm を内包）。汎用スクリプトなら `scripts/gemini_tts.py tts --file <章txt> --whole --voice Charon --style "落ち着いた低い声で、ミステリー番組のナレーターのように。" --output ...` を**章ごとに**回す（`--whole`＝全文1リクエスト）。
- ⚠️無料枠 ~15回/日・JST16時頃リセット。429で止まったら枠回復後にresume（生成済みの章はスキップ）。無料枠は10 req/分（RPM）＝リトライや連続章を連射しない（各章前に間隔を空ける）。
- ⚠️**音量のばらつきは章境界にだけ出る**（1章＝1リクエストなので章内は揺れなし）→各章を端の無音のみトリム＋`loudnorm I=-17:TP=-1.5:LRA=11` で正規化してから連結（章内の自然な「間」は保持）。
- ⚠️**全再生成しない**（他章の良いテンポまで変わる）。直すのは音量正規化＋**該当章のみ**再生成。**誤読は章再生成せず文スプライス**（`splice_sentence.py`・無音境界）。
- ⚠️cutoff（喋り止め後ずっと無音）/loop（同一文反復）は**総尺でなく実発話尺＝字/秒＋whisper文字カバレッジ**で検出（raw総尺は末尾無音に騙される）。
- 尺目安＝日本語100字≒25秒／2000字≒5〜6分。

## 5. 実写素材（6サイト横断・無料/商用可）
- `scripts/fetch_broll.py --query "..." --type video|image [--download N]`。**ブラウザUA必須**。
- 横断＝Pexels/Pixabay（要キー・帰属不要）＋NASA/Wikimedia/Openverse/Internet Archive（キー不要）。**Internet Archiveは著作権物混入可→収益動画で使う前に個別license確認**。確実に安全＝Pexels/Pixabay/NASA。
- 動画は ffprobe で H.264 自動選別（[[reference_gyre_h264_only_codec]]）。**素材ファイル名は中身と不一致しがち→必ずフレーム抽出で目視選定**。
- 原則＝**本物で作れるものはAIにしない**（空撮・地形・小道具はstock化でcr節約／人物の古代再現はstock不可＝AI必然）。
- **実在の企業/製品/人物/事件を扱う回は"本物の画像・ロゴ"を最優先**（汎用の無料実写や社名ワードマーク板＝プレーン文字板は、本物が使えない時の"代替"であって既定ではない）[[feedback_jinchi_use_real_images_not_only_free_stock]]。**可否の一次軸は著作権(copyright)＝商標ブランドガイドラインではない**。商標のnominative/referential use（推奨/提携を示唆しない解説・言及目的の表示）は一般に許容され、各社ブランドガイドラインは"権利者の運用姿勢"で著作権上の使用可否の最終根拠ではない（＝これでfail-closedして本物を一律排除しない）。ロゴの多くは「ありふれた書体の文字/単純図形」で創作性の閾値(threshold of originality)未満＝**public domain**であり、Wikimedia Commonsに**{{PD-textlogo}}**タグで存在＝**著作権フリーで本物を使える**（複雑な絵柄/独自イラストのロゴは著作権保護＝non-free＝代替）。**手順**＝①Wikimedia Commons等でPD-textlogo/CC/PDの本物を**必ず探索**し著作権的に問題なければ本物を使う→②本当に著作権保護される複雑ロゴ・確認できないものだけ代替（ワードマーク板）。**"調べずに代替"は本物最優先原則の違反**。公的機関/公開Webの公式画面（NVD/CVE/政府/公開記録）は無改変スクショで事実提示に使う。オーナー指定スコープ（本物を使う）を勝手に絞らない[[feedback_secretary_do_not_shrink_owner_specified_scope]]。

## 6. 図解・★合成（0cr内製・Higgsfield課金不要）
- 図解＝PIL（`make_diagrams.py`系）。ブランド統一＝**暗背景／金アクセント／ヒラギノ角ゴW6**。内容は縦中央寄せ・**下~240pxは字幕帯で空ける**。**図解2連続は禁止**（間に実写/画像を挟む）。地上絵はアルキメデス螺旋が一発でclean。
- ★“光が灯る/差す”合成＝numpyグロー（**円形Hann窓で縁を0に**＝四角ハロー枠防止）→ffmpeg `blend=all_mode=screen` で実写に加算。**screen合成は明るい映像で弱い→暗め（夕/曇）ベース**。追加cr0。
- 応用＝背景グリーン(#00FF00)図解→`chromakey=0x00FF00:0.1:0.0` で透過オーバーレイ。

## 7. AIの“撮れないシーン”だけ生成（有料・各カット許可制）
- 原則＝実写不可・素材無し・Ken Burnsでも作れないシーンのみ。**フリー素材に質感を寄せる**（documentary/muted/grain・過度なシネマ/CG化を避ける [[feedback_ai_clip_match_stock_footage]]）。
- **区分④無音モーション・⑤再現カット＝Kling v3.0 std720p sound off**（7.5cr/カット・5s基準）。実物写真あり→start_imageにKling（過去生成の`job_id`を`medias:[{role:"start_image",value:<job_id>}]`で再利用可）。無し→画像生成1cr→確認→Kling。
- **`get_cost:true` で必ず0cr先読み→残高記録**。presetが返り実生成されない時は`declined_preset_id`でリテラル生成。
- **生成後ゲート**＝採用前に額/鼻/頬を拡大し**てかり/老け**を点検→passのみ採用（`lessons-learned.md`）。崩壊カットは末尾にffmpegフェード・トゥ・ブラック(0cr)で救済。
- 顔の一貫性＝soul_2（soul_id=`ca82d851`・髭なし）。soul_2は画面内に文字を幻覚→暗い下地ならcv2 inpaint or クロップで除去。
- 生成物＝`.company/creator/work/youtube-jinchi/<topic>/_ai/`。

## 8. 4Kアセンブル（`build_nazca_4k.py` を題材別にテンプレ流用）★旧 build_nazca_video.py は使わない
- 設計＝**音声駆動セクション尺＋行アンカー同期**。各カットに開始ナレ行(sl)を持たせ、**Whisper実測の行時刻**に合わせる（比例配置は図・AIカットがズレる主因）。章＝1音声源を共有する連続カット群。映像尺を音声尺へ厳密一致（不足は自動bridge延長／過多は自動トリム）。
- 題材ごとに設定＝SECTIONS（章キー・kind=nar/sil・audio・txt・cuts）／TITLE・TEASERS（章の問い）・ENDCARD_MSG（毎回変える）。**次回題材はSECTIONSにkind=guide章・GA/GV参照・GUIDE_FADEを書かない**（The Life Guide型＝純ナレ運用）。※アセンブラのguide処理コード自体は過去動画（ナスカ/ヘルクラ）の再ビルド保護のため残す＝章定義でguideを使わないことで純ナレ化する。
- 仕様＝字幕（Zen Maru Gothic/ヒラギノ系・黒帯なし白文字黒フチ）・左上にグレー50%タイトル常時・**章転換＝黒画面の問い＋ポーン効果音（ナレ無音）**・末尾**終了画面25s**。
- 書き出し＝**25セクション分割→`-c copy`ロスレス連結**（veryfast/crf20・lanczos拡大）。**内容ハッシュでキャッシュ**（出力名=md5(種別+素材+尺+字幕窓)）→変更カットだけ数分で差替。
- ⚠️**画像差替えは必ず新パス or 該当カット削除**（PATHキャッシュ対策＝同名上書きは旧画が残る）。

## 9. ★Whisper字幕同期（最重要・無料）
- `.venv/bin/python scripts/nazca4k_whisper.py`（または `align_captions.py`）→ 行ごとの実発話時刻 → アセンブラの行アンカーへ。文字数比だと実発話と最大±5秒ズレる。**align後に再ビルド**。
- ⚠️**この環境のffmpeg（brew 8.x）はsubtitles/drawtextフィルタ非搭載**→字幕/タイトル/ブランドは**PILでPNG生成しoverlayで焼く**方式（`build_nazca_4k.py`に内蔵）。
- **★行頭アンカーは「読み形」に寄せて照合する（恒久・2026-07-17 第4弾で確立／実装正本＝`scripts/build_aivuln_4k.py`）**：clean台本は**TTS読み表記**（AI→エーアイ 等＝`<topic>_prep_tts_text.py` の `REPL` で変換済）だが、Whisper認識は**綴り**（AI）で返る。この不一致で `find()` が外れた行は **char-ratio補間へ落ち、実発話の「間」を無視した比例配置＝1.0〜1.6s早い字幕**になる（第4弾起1で実測・数値ゲートは誤PASS）。恒久対処＝アセンブラに**読み正規化レイヤ**を持たせる：
  1. `from <topic>_prep_tts_text import REPL` してWhisper側にも同じ変換を当て、**両側を読み形へ寄せて照合**（`_READ_RULES` / `_read_norm_str` / `_read_norm_pairs`）。**辞書はREPLが単一の真実源**＝将来の固有名はREPLに足せば台本・字幕・照合へ自動反映。
  2. REPLに無い**Whisper綴りゆれ**（一つ↔ひとつ／例える↔たとえる 等）は `_READ_EXTRA` で補う。
  3. 完全一致で拾えない行は **difflibの行頭probe（ratio≥0.6）** で拾い、**補間へ落とさない**（`_fuzzy_head_pos`）。
- **★glued限定の実音声スナップ**：`_speech_mask` / `_snap_to_speech` ＝直前語とのwhisper gapが **<0.08s** の「貼り付き語」だけを発話再開位置へ前進スナップする。**真正の「間」では不発**＝演出の間を潰さない。
- **工程9の完了条件（この2つを満たすまで次へ進めない）**：① 章ごとに**アンカー成立＝全行／補間0**（成立率を記録）② **フレーム抽出×独立Whisperの実測で全札|Δ|<0.35s**。`jinchi_caption_sync_check.py` の median/max のPASSは**合格根拠にしない**（最寄りonsetを拾うため誤PASSする）→ 判定は `jinchi-review-caption` の観点で行う。

## 10. BGM＋効果音
- 音声＝ナレ＋無音をセクション連結→loudnorm→**BGMサイドチェインダッキング**→SE配置→alimiter。
- BGM＝archive.org FreePD(CC0)・volume0.10・末尾afade。SE＝`.company/creator/work/_shared/sfx/pon.wav`等を各章頭に`adelay`配置。

## 11. サムネ→非公開アップ→（公開はオーナー）
- サムネ＝**着手前にウェブ調査で同ジャンル高再生サムネをベンチマーク→勝ちパターンを真似る** [[feedback_research_proven_thumbnails_before_creating]]（10字以内/2-3色/ダーク+1アクセント/単一被写体）。実写＋PIL文字。3案出して選定。
- アップ＝`python3 seo_growth/upload_ainsight_video.py`（題材ごとに VIDEO/THUMB/TITLE/DESCRIPTION/TAGS/目次を差替）。**privacy=private・defaultLanguage=ja・category27・`containsSyntheticMedia:True`（毎回ON）・概要欄に目次(0:00..)＋AI明示の免責**。
- ⚠️目次（章タイムスタンプ）は**timeline.json（4Kビルドが出力するセクション開始時刻）から正確に作る**（旧版の目次を流用しない）。
- 認証＝`IJIN_YOUTUBE_TOKEN`。失効時 `python3 seo_growth/setup_ijin_oauth.py`（人智の外側chを選択）。**OAuthがTestingだと7日で再失効→継続運用はProduction公開**。
- 公開（オーナー作業）＝終了画面パーツをStudioで末尾25s枠に配置→**バッファ2〜3本→金/土の夜20-21時JSTで予約公開**（コールドスタート対策）。**公開GOはオーナーのみ**。

---

## カメラ語彙8種（Klingのプロンプトに明示）
| 語彙 | 用途 | 語彙 | 用途 |
|---|---|---|---|
| `slow_zoom_in` | 細部に寄せる | `rack_focus` | 前景→後景フォーカス移動 |
| `dolly_forward` | 奥行き・没入 | `whip_pan` | 場面転換の勢い |
| `orbit_left/right` | 被写体を回り込む | `time_lapse` | 時間経過 |
| `aerial_pullback` | 俯瞰から引く | `static` | 固定・落ち着き |

## AIカット プロンプト設計の鉄則
1. **感情時系列をタイムスタンプで記述**（「0-2s: 遺跡の全景 / 2-5s: 文様にslow_zoom_in」）。
2. **セリフ・口の動き・人物アップ禁止**（Sound非対応のKlingは音声生成不可）＝純ナレ構成では口パクカット自体を作らない。
3. **古代文物・浮世絵の実写化**＝「版画の線質を残す」「彩度low」「film_grain: medium」「aged_photograph感」。過度なCG/彩度UPを避ける。
4. **Claude→生成 最適化ループ**＝「このシーンの核となる動きを10語以内で言語化して」と先に聞いてからプロンプト化＝試行回数減。
5. **連続生成のラストフレームルール**＝前カットのラストに全身（下半身含む）が写っていないと次生成で衣装ランダム化→一貫性崩壊。引いて全身を収めるか inpaintで補完してから次カットへ。
6. **プロンプトはシンプルに肯定で**＝写実語・否定語を盛るほど逆効果（てかり増・要素追加）。最小肯定文がマット良好 [[feedback_simple_positive_prompt_no_negatives]]。**しわ/aged/deep texture等の肌描写は書かない**（顔が老ける）[[feedback_ai_face_prompt_no_wrinkles]]。

## モデル使い分け（Kling / Wan2.5 / Veo3.1）
- **解像度テスト先行**＝初回std(1280x720)→品質合格のみ1080p昇格（リテイクのcr浪費防止）。
- 人物の細かい表情指示→**Wan2.5優位**（プロンプト忠実度）。カメラワーク（引き/回転/軌道）→**Kling優位**（滑らか・背景崩れ少）。品質最優先カットのみVeo3.1（試行コスト高・1本100cr相当）。
- Veo3.1小技＝キャラ前に「Japanese」＋環境「Japan」で日本語発音改善／末尾 `no subtitles`／セリフ中の「/」禁止（字幕バグ）。
- `motion_control` MCPで動作方向を数値補正（ドリフト時）。

## コスト早見（人智の外側 実績）
- 有料＝AIカットのみ（Kling 7.5cr/カット）。**1本上限300cr（オーナー確定）**。第1弾ナスカ実消費＝約170cr（旧・語り部版＝Seedance口パク含む）。
- 無料＝Gemini TTS(枠15/日)・Pexels等・ffmpeg・Whisper・BGM(CC0)・OFLフォント・アップロード。
- 関連＝[[reference_video_pipeline_tool_costs]] / [[project_ainsight_channel_pipeline]] / [[reference_video_0cr_figures_composites]] / [[reference_higgsfield_mcp_video_ops]]
