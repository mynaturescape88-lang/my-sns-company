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
- **★素材の合法性（絶対基準・恒久）**：**使用する素材はすべて合法であること＝商用利用・公衆送信が可能であること。** 統制は**入口**で行う＝**工程5の取得元リストにある取得元からのみ取得する**（リスト外からは取得しない）。**固定資産（OP/ED）・使い回し・過去に承認済みの素材も対象＝免除は無い。**
  - 確認は**選定時点**（台帳に載せる時）＝**事後確認は不合格**。結果は台帳**§K**へ記入する（空欄・「不明」は不合格）。証跡ファイルは**素材と同じディレクトリに置き、移動時は一緒に運ぶ**。
  - **確認は配布元の一次規約で行う**＝サービス名・ファイル名からの推測、**別サービスの規約の流用は不可**（実例＝tunetank配布の音源にPixabayの規約を当てて著作権違反）。**「確認できない」と「NGと記載されている」は別物**＝第三者取得物は許諾を積極的に確認できることが必要／**正規に提供された実行環境の同梱物**（OS同梱フォント等）は禁止条項が無いことを一次規約で確認できれば可。
  - **★未確定が1件でも残る間は提示・アップロード（非公開含む）・公開をしない**（fail-closed）＝取れる道は①オーナーへ「何が未確定で、決まれば何が変わるか」を提示して判断を仰ぐ ②その素材を使わない、の2つだけ。台帳確定時に `content-compliance` skill を当てる（工程10でも再度当てる）。

## 4. ナレーション（Gemini TTS・無料）＝1章1リクエスト方式
- **確立手法＝`<topic>-narration-clean/` の章別txt（1ファイル=1章）を「1章=1リクエスト」でTTS**（`\n\n`段落分割はしない＝章内の声の揺れをゼロにする）。参照実装＝`scripts/ghost_hominin_gemini_nar.py`（章ごとにcutoff/loop検出リトライ＋端無音トリム＋loudnorm を内包）。汎用スクリプトなら `scripts/gemini_tts.py tts --file <章txt> --whole --voice Charon --style "落ち着いた低い声で、ミステリー番組のナレーターのように。" --output ...` を**章ごとに**回す（`--whole`＝全文1リクエスト）。
- ⚠️無料枠 10回/日・JST16時頃リセット。429で止まったら枠回復後にresume（生成済みの章はスキップ）。無料枠は10 req/分（RPM）＝リトライや連続章を連射しない（各章前に間隔を空ける）。
- ⚠️実施前チェック＝当日の想定利用回数（撃つ予定の章数）が1日上限（10回）を超える場合は、実施前にオーナーへ超過見込みを報告する。
- ⚠️**音量のばらつきは章境界にだけ出る**（1章＝1リクエストなので章内は揺れなし）→各章を端の無音のみトリム＋`loudnorm I=-17:TP=-1.5:LRA=11` で正規化してから連結（章内の自然な「間」は保持）。
- ⚠️**全再生成しない**（他章の良いテンポまで変わる）。直すのは音量正規化＋**該当章のみ**再生成。**誤読は章再生成せず文スプライス**（`splice_sentence.py`・無音境界）。
- ⚠️cutoff（喋り止め後ずっと無音）/loop（同一文反復）は**総尺でなく実発話尺＝字/秒＋whisper文字カバレッジ**で検出（raw総尺は末尾無音に騙される）。
- 尺目安＝日本語100字≒25秒／2000字≒5〜6分。

## 5. 素材の取得（★入口＝取得元で絞る）
- **素材は下表の取得元からのみ取得する**（表に無い取得元からは取得しない）＝**すべて商用利用・公衆送信が可能であること**。
- **ロゴ・企業シンボル・公式シール/紋章は取得元を問わず使わない**＝**社名テキスト表示に置換**する（実装＝`scripts/aivuln_logo_walls.py` の `TEXT_TILES` 方式）。**「判断に時間をかけるより使わない」が正しい。**

| 取得元 | 商用／帰属 | 一次規約URL |
|---|---|---|
| Pexels | 可・帰属不要 | https://www.pexels.com/license/ |
| Pixabay | 可・帰属不要 | https://pixabay.com/service/license-summary/ |
| NASA（画像・映像） | 可・帰属不要 ※**ロゴ/記章はPDでない＝上記の常時除外**  | https://www.nasa.gov/nasa-brand-center/images-and-media/ |
| DOVA-SYNDROME（BGM） | 可・帰属不要 ※Content-IDクレームのゼロ保証はない | http://dova-s.jp/contents/terms |
| 自作（PIL図解・★合成・テキストカード）・自前撮影 | 自社制作物 | — |
| 生成AI＝Higgsfield／MiniMax(API)／Gemini | 出力の商用利用可 | higgsfield.ai/terms-of-use-agreement §4.4 ／ platform.minimax.io/protocol/terms-of-service |
| フォント＝SIL OFL（Zen Maru Gothic 等） | 可・**制作物への表示不要**（帰属義務はフォント再配布時のみ） | https://openfontlicense.org/open-font-license-official-text/ |
| フォント＝ヒラギノ角ゴ（macOS同梱＝正規提供の実行環境同梱物） | 可・帰属不要（権利元SCREENのFAQが**映像コンテンツの公衆送信**を明示許諾）＝**再調査しない** | https://hiragino.screen.co.jp/support/faq/50 |

> **取得時に1点ずつ確認が要る取得元**＝Wikimedia Commons／YouTubeオーディオライブラリ／Openverse／Internet Archive。**これらは取得元が個別作品のライセンスを保証しない**＝**配布元の一次規約で1点ずつ商用可を確認できた物だけ使う**（確認できなければ使わない・事後確認は不可）。

- `scripts/fetch_broll.py --query "..." --type video|image [--download N]`。**ブラウザUA必須**。動画は ffprobe で H.264 自動選別（[[reference_gyre_h264_only_codec]]）。**素材ファイル名は中身と不一致しがち→必ずフレーム抽出で目視選定**。
- 原則＝**本物で作れるものはAIにしない**（空撮・地形・小道具はstock化／人物の古代再現はstock不可＝AI必然）。**安易に無料実写で埋めず、0cr図解・★合成で表現を作る**。公的機関/公開Webの公式画面（NVD/CVE/政府/公開記録）は無改変スクショで事実提示に使う。

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
- BGM＝`_shared/bgm_pool/`（YouTubeオーディオライブラリ＋DOVA-SYNDROME・volume0.10・末尾afade）。SE＝`.company/creator/work/_shared/sfx/pon.wav`等を各章頭に`adelay`配置。
- **★音素材（BGM/SE/ナレーション音源）も映像と同じ扱い**＝工程5の取得元リストから取得し、§Kへ記入する。**証跡ファイル（`LICENSE_AND_SOURCES.txt` 等）は音源と同じディレクトリに置き、整理・移動時は必ず一緒に運ぶ**。可能なら**機械的な同定根拠**（ID3のencoder署名・尺・ファイルハッシュ）も残す。**OP/EDに既に焼き込まれているBGMも同じ扱い**＝工程10の「§F 権利ゲート」で毎弾確認する。

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
