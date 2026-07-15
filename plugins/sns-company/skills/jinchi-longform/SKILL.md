---
name: jinchi-longform
description: >
  YouTube「人智の外側」（@ijin-hiroku・旧称AIインサイト／古代ミステリー×AI長尺）の動画を、題材選び→台本→章別素材台帳→制作→非公開アップまで一気通貫で作るスキル。「人智の外側の動画を作って」「古代ミステリー長尺」「次の長尺の題材/台本/制作」「ナスカ/ヘルクラネウムの続き」等で発動。構成＝The Life Guide型（ホスト非表示・純ナレーション）。@mynaturescapeの実写長尺とは制作手法が別物（混同しない／あちらは video-editing スキル）。題材選定基準・起承転結台本・章別素材台帳・0cr素材最大化・4Kアセンブル・Whisper同期・非公開アップ、および第1弾ナスカで出た課題の次回デフォルトを内包する。
---

# 人智の外側 長尺制作スキル v1.0（一気通貫）

チャンネル＝**人智の外側**（@ijin-hiroku / `UCWqUEgQiqgWKLyjhbkrNHiA`・旧称「AIインサイト」＝**この名称は廃止**）。
コンセプト＝**「人間には解けなかった真実を、AIが解き明かす」古代ミステリー×AI**。落ち着いたナレで15分以上。
第1弾ナスカで全工程を確立済み。**技術面はほぼ自動＝ボトルネックは台本／ファクトチェックの質**。

> ⚠️ @mynaturescape の実写長尺は**別手法**（`video-editing` スキル＝score→filter-people→assembleの1発生成）。本スキルは「人智の外側」専用。混ぜない。

## いつ発動するか
- 「人智の外側／古代ミステリー長尺を作って」「次の長尺の題材・台本・制作」「ナスカ/ヘルクラネウムの続き」
- 「AIが謎を解く動画」
- @ijin-hiroku の長尺工程（題材選定〜非公開アップ）に着手するとき

## 発動＝適用（必ず行う）
1. 下の **11工程** を順に回す（飛ばさない）。各工程の詳細は該当 reference を読む。
2. **台本を書く前に `content-planning` スキルを必ず発動**（企画チェックリスト＋起承転結フロー＋バズ構造モデリング）。発動＝完了でなく手順を実際に回す [[feedback_skill_activation_must_execute_workflow]]。
3. **章別素材台帳（reference `ledger-template.md`）を制作の背骨として作る**＝全カットの「画・区分・尺・予cr/実cr/済」を1枚に。台帳が埋まる＝制作の進捗。
4. **第1弾ナスカで出た課題（reference `lessons-learned.md`）を着手前に通読**し、次回デフォルトとして反映する（同じ失敗を繰り返さない）。
5. **生成GO・公開GOはオーナーのみ**。crを使うカットは `get_cost`(0cr)で先読み→残高記録→承認後に生成。
6. **オーナーへの章別提示＝narration/assemble/captionの3レビュー全passの完成系のみ**（未チェックのまま見せない・[[feedback_jinchi_owner_check_per_chapter_9]]）。各章の検証は必ず**制作役が正規レビューskillを実ファイル**（書き出し済み動画のフレーム抽出＋音声抽出→whisper再解析）で実施する。**秘書がJSON(captions.json/timeline.json)の数値突合だけで「一致」と自己判定し提示することは禁止**＝データモデル上の整合と、実際に焼き込まれた動画の実体が一致しているかは別物（2026-07-08/09 ch2・ch3で実際に見逃した事故）。

## 11工程（順番・正本＝`references/pipeline.md`）
1. **題材確定** … コンセプト適合（AIが人間の不可能を可能にした題材）。題材キューは .company/sns_accounts/youtube_ainsight.md。**手法の型（発見/解読/復元/検出 等）が既出弾（ナスカ=発見/ヘルクラ=解読/ghost hominin=検出）と被る題材は避ける＝例外はインパクト大の題材か、構成・伝え方で「また◯◯系ね」と思わせない差別化方針を明文化した場合のみ**。題材候補の要約表には必ず「手法の型」列を設ける。→ 確定したら `jinchi-review-topic` を発動して合否を取る（pass のみ工程2へ）。
2. **台本＋★ファクトチェック** … `content-planning`発動→起承転結。数字はかな/漢字表記。FCログを台本に残す（公開前ゲート）。→ 台本＋FC完成後 `jinchi-review-script` を発動（★最重要・fail-closed）。
3. **★章別素材台帳を作る** … `ledger-template.md`。再生順マスターショットリストに区分①〜⑤を割当→予cr積算→1本上限300crと照合。→ 台帳を埋めたら `jinchi-review-ledger` を発動（尺の実合算を必ず検算）。
4. **ナレーション（Gemini TTS・無料）** … 全part loudnorm正規化→連結。**全再生成しない**（誤読は文スプライス）。→ ナレ連結後 `jinchi-review-narration` を発動。
5. **実写素材（6サイト横断・無料）** … `fetch_broll.py`。本物で作れるものはAIにしない。H.264選別。
6. **図解・★合成（0cr内製）** … PIL図解（暗背景/金/ヒラギノW6・下240pxは字幕帯）＋numpyグロー×ffmpeg `blend=screen`。Higgsfield課金不要。→ 図解/合成カット完成後 `jinchi-review-figure` を発動。
7. **AIの“撮れないシーン”だけ生成（有料・各カット許可制）** … 無音モーション・再現＝Kling。フリー素材に質感を寄せる。→ 各カットは生成"前"に `jinchi-review-aicut`（生成前ゲート・fail-closed）、生成"後"に同skill（採否ゲート）を発動。
8. **4Kアセンブル（`build_nazca_4k.py`を題材別にテンプレ流用）** … **音声駆動セクション尺＋行アンカー同期**。純ナレ章nar＋無音章sil＋黒画面の章転換のみ（次回題材はkind=guide章・GA/GV参照・GUIDE_FADEを書かない＝純ナレ運用）。→ 4K書き出し後 `jinchi-review-assemble` を発動。
9. **★Whisper字幕同期（最重要）** … `nazca4k_whisper.py`／`align_captions.py`で実発話時刻を取得。**この環境のffmpegはsubtitles/drawtext非搭載→字幕はPIL PNGをoverlay焼き**。→ align・再ビルド後 `jinchi-review-caption` を発動。
10. **BGM＋効果音** … BGM=archive.org FreePD(CC0)・vol0.10・サイドチェインダッキング。章頭にSE。
11. **サムネ→非公開アップ** … サムネはウェブ調査で高再生型を真似る [[feedback_research_proven_thumbnails_before_creating]]。`upload_ainsight_video.py`（privacy=private・`containsSyntheticMedia:True`必須・概要欄に目次＋AI免責）。公開はオーナー（Studioで終了画面配置→金/土夜予約公開）。→ サムネ3案後 `jinchi-review-thumbnail` を発動（アップ直前メタもここで最終確認）。アップ前に `pm/review-baseline.md`（Layer1てっぺん総合）を1回当てる。

## reference 索引
| ファイル | 中身 | 読むとき |
|---|---|---|
| `references/pipeline.md` | 11工程の詳細手順・カメラ語彙8種・Kling/Seedance/Wan/Veo使い分け・コスト早見 | 制作に入るとき（毎回） |
| `references/ledger-template.md` | 章別素材台帳テンプレ（区分①〜⑤・予cr/実cr/済・尺整合・cr収支）＋記入例 | 工程3（台帳作成）のとき |
| `references/lessons-learned.md` | 第1弾ナスカで出た課題と次回デフォルト（声=自然ピッチ／口パク確立レシピ／4Kアセンブラ／生成後ゲート／PATHキャッシュ／しわ・てかり禁止／既存素材流用） | 着手前に通読・各工程で随時 |

### 他スキル／正本への参照（本スキルに埋めない）
- **台本の企画・起承転結・バズ構造** → `content-planning` スキル（工程2で必ず発動）。
- **冒頭フック5段・カット密度・AI放置量産の回避・PMレビュー基準** → `video-editing` スキル `references/editing-principles.md`。
- **プロジェクト固有の実パス・チャンネルID・トークン名・スクリプト一覧** → `.company/secretary/work/task_logs/2026-06/ainsight-longform-production-RUNBOOK.md`（実行値の正本）。
- **アカウントの正体・コスト前提・1本上限300cr** → `.company/sns_accounts/youtube_ainsight.md`。

## 絶対原則（全工程共通）
- **AI放置量産はしない**：ファクトチェック・落ち着いた語りを必ず注入（inauthentic/BAN回避）。
- **1本あたり上限300cr**（2026-06-21オーナー確定）。超える計画は本生産前に報告。0cr素材を最大化し有料カットは決めカットのみ。
- **AI開示**：合成音声/AI生成ビジュアルを含む＝アップ時 `containsSyntheticMedia:True` を毎回ON。
- **謎のまま終わらせない**：結で具体的な答え・到達点を言い切る [[feedback_ainsight_no_mystery_ending]]。
- **画面内に『参考：他都市』『参考：近縁種』等の注記テロップを焼かない**（チャンネル既定・第1/2弾に前例なし）。近縁都市・代替素材を使う場合もバッジで注記しない。
- **生成・公開GOはオーナーのみ**。スキルから催促しない。
