# KB: AI動画/コンテンツ制作パイプライン（ブランディング・台本モデリング・素材調達）

> 対象役割：SE/コンテンツ制作/マーケター。読むべき時：AIで長尺/ショート動画やSNSコンテンツの制作工程・ブランディング・台本設計を扱う時。
> 検証凡例：✅事実(複数2026ソース) / ⚠️誇張・比喩 / ❓未検証。具体%・固有事例はチャンネル独自主張が多く、断定は一次ソース確認後。
> 出典：RKJ動画 `5e5lN2RIoxI`（急成長アカウントの共通点）＋Web裏取り（viewsmax/vidpros/flowith/mymarky, 2026）。今後 Skywork系ワークフロー(`7YxEndCBYlU`)等もここへ追記。

## ブランディング（アカウント名・ロゴ・配色）— コスト0・Claude完結
- ✅ **印象に残るオリジナル名**：一般名詞（"AIクリエイター"等）は埋もれる。コンセプトに紐づく**造語**を作ると差別化＆記憶定着。手順＝ChatGPT/Claudeにコンセプト語（例「勇気」「成長」）の関連オリジナル単語を5案→気に入らなければ再生成→**重複チェック**（既存アカウント名と被らないか）。
- ✅ **色彩心理でロゴ/配色を選ぶ**：黒=高級・洗練／金=力・富／緑=成長・信頼／青=誠実 等。ブランド全体（ロゴ・テロップ・サムネ）で**1〜2色に統一**すると認知が早まる（例：RKJは緑テロップで一貫）。AI生成は条件（アイコン/背景色/文字色/雰囲気）を**追記で段階的に詰める**と暴走しない。
- 🟢 自社適用：既存grayishトーン([[reference_grayish_design_tokens]] INK#535252/PANEL#f6f6f6/OFFWHITE#efece8)を色彩心理で点検。植物系は緑=成長・信頼が王道だが、差別化済みなら無理に変えない。

## 台本/構成のモデリング（"盗む"の正しい解釈）
- ✅ **AIにゼロから台本を書かせない**：「○○の台本書いて」は無難で平板になり、モチベ系等では逆効果。代わりに**バズっている動画を文字起こし→AIで「同タイトル・似内容で書き換え」**＝成功構造を借りて中身を自分の題材に置換。
- ✅ これは2026の**正攻法（content modeling / repurposing）**＝成功コンテンツの**構造（フック→課題→解決→CTAの順序・テンポ）を抽出して再構成**すること。丸コピー（台本/字幕の逐語転用）とは別物。"優れた芸術家は模倣し偉大な芸術家は盗む"は**構造モデリングまでを指す**と解釈する。
- ⚠️ 境界線：**逐語コピー・字幕丸写し・固有表現の流用はパクリ＝NG**（[[feedback_shop_article_rules]] 断定/転用禁止と整合）。借りるのは"型"のみ。
- 🟢 自社適用：植物・子育て系のバズ長尺/ショートの構造を抽出→自社題材で書き換え。Claude完結・既存コンテンツ制作フローに乗る。

### リサーチ→構成化の内製（多言語動画要約・source-grounding）
> 出典：RKJ動画 `tLC5S1dom2c`（解説動画のリサーチ→台本構成・AIマインドマップMapify）＋Web裏取り（mapify公式/Chrome Web Store/bibigpt, 2026）。読むべき時：複数の参考動画/記事から構成を抽出して台本・記事アウトラインを作る時。
- ✅ **解説/参考動画のリサーチ→構成抽出はツール契約不要でClaude＋`youtube-transcript`スキルで内製可**。RKJ流は有料ツール「Mapify」（YouTube/PDF/Web→AIマインドマップ・**100以上の言語に対応しクロス言語で日本語要約**・タイムスタンプで該当箇所へジャンプ・新規30無料cr/全機能はPro）だが、**多言語動画の文字起こし→日本語要約→構成（フック→課題→解決→CTA）抽出はClaudeのコア能力＝同等を新規サブスクなしで実行できる**（当組織の優位）。複数の文字起こしをClaudeに渡し「共通構成を抽出→自社題材で書き換え」＝上記モデリングと同型・WPブログ/note記事のアウトライン設計にも転用可。出典: [mapify YouTube summarizer](https://mapify.so/tools/youtube-video-summarizer) / [Chrome Web Store: Mapify](https://chromewebstore.google.com/detail/mapify-ai-summarizer-mind/fldgacclkckcmflokaialbihppmndpbd)
- ✅ **AI要約は必ず参照元（一次ソース）と照合してから断定＝source-grounding**。AI要約はハルシネーション懸念があり、第三者によるMapify固有の精度評価データも無い＝**Web検索で参照元リンクを出させ人間が正誤を判断するのが正**（[[feedback_shop_article_rules]] 断定/変動情報の禁止・本調査凡例✅⚠️❓と一致）。出典: [bibigpt best YouTube AI summarizer 2026](https://bibigpt.co/en/blog/posts/best-youtube-ai-summarizer-chrome-extensions)
- ⚠️ **アフィリ誘導動画特有の誇張に注意**：RKJ動画の「Safari/Edgeを消してChromeを使え」は不要な断定（Mapify拡張はFirefox/Edge版も存在）／運営者の収益数値（借金300万→1年で利益1000万超）は❓未検証で15本目の実数（登録1万人の広告は半年累計¥100k前後）と桁が乖離＝講座/案件等の付加収益込みの主張と推定・額は参考外（[[kb-youtube-monetization-models]]モデルC＝桁を変えるのは付加収益）。**Mapify Pro/Gammaは新規有料ツール＝契約しない（Claude内製で代替・費用根拠なき採用はしない[[feedback_ask_before_action]]）**。
- 🟢 自社適用：植物/子育ての海外含む人気動画を多言語リサーチ→Claudeで要約・構成抽出（構造モデリング・逐語転用NG）→AI要約は一次ソース照合してから断定→自社題材でアウトライン化。生成クレジット消費は一切不要（テキスト処理のみ）。新ジャンル（解説チャンネル）参入そのものは[[project_revenue_needs_bigger_levers]]の文脈で別途判断＝"型"だけ既存テーマへ転用。
- ✅ **出典付きリサーチエージェント（Skywork等）も中身はClaude＋WebSearch/WebFetch＋`youtube-transcript`で内製可・新規サブスク不要**（出典＝RKJ 18本目`0VSRCyQHh0Y`＝Skyworkの3機能〈ドキュメント=リサーチ/スライド/スクリプト=台本〉で解説動画のリサーチ→台本を回すデモ。**ツールは1/16本目で既出のSkywork・アフィリ誘導動画**）。Skyworkの売り＝"リアルタイム検索→出典付きでまとめる"（GAIAベンチ82.42＝OpenAI/Manus超のトップクラス✅／ただしリーダーボードは更新が速く"1位"は時点依存）だが、**この出典付きリサーチは当組織の調査方針（断定は一次ソース後・source-grounding）と同型で日常運用済＝本調査タスク自身が実証**。台本は「同テーマの既存動画台本をコピペして型を借り→AIに似た構成で書き換え」＝構造モデリング（3/14/17本目既出）。出典: [skywork ultimate AI agent guide](https://skywork.ai/skypage/en/ultimate-skywork-ai-agent/2033789235665649664) / [skywork AI guide GAIA 82.42](https://skywork.ai/skypage/en/skywork-ai-guide/2034281291935395840)
- ✅ **企画方針＝「情報を伝えるチャンネルなら"最も価値が高い情報"に振り切る／中途半端（楽しさも価値も中途半端）が最悪」**（出典＝RKJ 18本目）＝解説/ノウハウ系（noteのAI活用記事・植物の育て方ガイド等）は楽しさと価値の両狙いでなく濃い価値情報に特化。再生数=ハード面(CTR・視聴維持)＋ソフト面(情報の価値)で、ソフト面の改善にリサーチ力が要る（2/17本目の企画起点・内容の濃さで差別化と同型・[[kb-youtube-seo-algorithm]]）。
- ✅ **新ツールは「ベンチマークで見極め＋自分でテストして検証」してから採用**（出典＝RKJ 18本目の締め＝アフィリ誘導動画ながら誠実な一言）＝自社の[[feedback_known_good_method_before_spend]]（課金前に確立手順を調べ切る）/[[feedback_validate_method_at_min_cost_before_scaling]]（最安検証）を補強。Skyworkは新規有料サブスク＝契約しない（同等をClaude内製で代替・既出16本目／[[feedback_ask_before_action]]）。

## 顔出しなし解説動画の全工程テンプレ（企画→台本→素材→編集→サムネ→タイトル）— 大半Claude完結
> 出典：RKJ動画 `pjX1LsT0c4s`（月70万のAI解説動画の作り方・18.3分）＋Web裏取り（democreator公式/vidmore, artgrid公式/footagesecrets, VOICEVOX/CoeFont/VOICEPEAK, 2026）。読むべき時：顔出しなしの解説/ノウハウ系長尺動画を企画から投稿まで設計する時。**個別ノウハウ（構造モデリング/Test&Compare/タイトルKW）は他節・他KBで既出＝ここは"全工程の地図"として統合**。
- ✅ **全工程＝①企画選定（同ジャンルで伸びてる企画を構造モデリング）→②台本（参考動画を文字起こし→新タイトル3案/構成/台本/改善点）→③映像素材（差別化したストック＋AI生成クリップ＋画面録画）→④編集（カット/字幕/ズーム演出）→⑤ナレーション→⑥差別化演出→⑦サムネ→⑧タイトル**。RKJ流は外部ツール群（ChatGPT＋Chrome文字起こし拡張＋Artgrid/Runway/Midjourney＋DemoCreator＋日本語TTS）だが、**当組織は②⑦⑧＝Claude（＋`youtube-transcript`）完結・生成クレジット不要／③クリップ＝NanoBanana→Seedance（Higgsfield MCP）／⑤＝ElevenLabs／⑦画像＝NanoBanana Pro／フリー素材＝Pexelsで一通り代替でき新規サブスク不要**。
- ✅ **②台本はAI丸投げ禁止＝必ず人間（PMレビュー）が"不自然な日本語チェック＋追加情報の書き足し"を行う**。指示書設計＝対象視聴者（年齢/性別/属性・不明なうちは幅広く）・ジャンル・動画長（10分≒台本約1.4万文字）→参考タイトル＋台本を学習→新タイトル3案→構成（目次）→台本→改善点3つ（質問形式の導入・フック強化）。構造モデリング（構造踏襲≠逐語コピー）は本KB上節・3/14/17/18本目既出。
- ✅ **③映像素材の差別化＝無料素材（Pexels/Pixabay）は商用可だが利用集中で既視感が出やすい→AI生成クリップ（NanoBanana→Seedance）＋自前写真で差をつける**。Artgrid（=Artlist系ストック映像・年額フラットで無制限DL・商用含む永続ライセンス＝✅実在）はRKJ流の差別化手段だが当組織は契約せず内製で代替。出典: [artgrid.io](https://artgrid.io/) / [footagesecrets Artgrid pricing](https://www.footagesecrets.com/buyers-guide/artgrid-pricing/)
- △ **④編集オールインワン＝DemoCreator（Wondershare・✅実在）＝画面録画(最大4K/120fps)＋編集＋AIボイスチェンジャー(5種)＋90言語自動字幕(日本語精度高)＋30種AIアバター(リアルタイムlip-sync)＋パン&ズーム(矢印伸縮でキーフレ不要)＋シネマアスペクト/美肌・小顔フィルター＋YouTube直接アップ**。**当組織の編集は無料CapCut＋手動が既定（[[kosodate-loop-video-SUMMARY]]）でPC画面録画系はClaude/MCP範囲外＝代替不可**＝"解説の画面録画→カット→自動字幕→ズーム演出"の工程発想だけ採る。採用は費用根拠提示で承認制（[[feedback_ask_before_action]]）。出典: [democreator features](https://democreator.wondershare.com/democreator-features.html) / [vidmore DemoCreator Review 2026](https://www.vidmore.com/record-video/wondershare-democreator-review/)
- △ **⑤日本語TTS＝VOICEVOX（完全無料・商用可・ずんだもん等）／CoeFont(=声フォント・ブラウザ・有料で商用)／VOICEPEAK(=ボイスピック・感情/読み/アクセント細調整・オフライン)＝✅実在・全て商用ナレーション可**。当組織のナレーションは**ElevenLabs（既存スタック）が基本／日本語が不足する場合のみVOICEVOX（無料・商用可）を候補**。マイクは口元に近づけると音質↑（ASMR的）。出典: [VOICEVOX解説](https://note.com/ai_mitsukaru/n/n7dd05db6e75e) / [CoeFont商用利用](https://www.topvox.jp/blog/review-coefont/) / [VOICEPEAK製品情報](https://www.ah-soft.com/voice/6nare/)
- ✅ **音声内製の選択肢比較（faceless運営のナレーション・出典：RKJ`vphd41_8sLU`＝Fish Audio深掘り＋Web裏取り）＝ElevenLabs◎＞VOICEVOX○＞Fish Audio○条件付き**。**ElevenLabs＝◎当組織がskill保有の既存スタック**＝台本（Claude生成）→ナレーション直結／多言語・日本語対応／ボイスクローン・話者切替の会話劇も可＝新規契約も新規UI操作も不要で第一候補。**VOICEVOX＝○完全無料・商用可・日本語特化**＝ElevenLabsが日本語で不足する時の無料代替。**Fish Audio（フィッシュオーディオ）＝○条件付き**＝日本語native級✅・15〜30秒の短サンプルでボイスクローン可・20万超のコミュニティ音声・最大3万文字TTS・複数キャラ会話劇UI＝優秀だが**(a)新規有料サブスク＝原則契約しない[[feedback_ask_before_action]]／(b)商用利用は有料プラン必須・無料枠は月7分＝"無料で使える"を鵜呑みにせず商用可否を確認／(c)発見タブの一般ユーザー投稿の有名人声モデル（トランプ等）＝パブリシティ/肖像権侵害リスクで✕採用不可＝合法な合成声/自作クローンに限定（本KB下節WAN2.5「芸能人は採用不可・技術のみ」と同境界）**。採用するならElevenLabsで明確に不足する用途＋費用根拠提示で承認制。**「ありふれた無料AI音声は没個性化＝声選定/調整/クローンで差別化」は[[kb-youtube-ban-avoidance]]（AI音声＋静止画＋フリー素材の没個性量産はBAN傾向）と整合＝声の作り込みは差別化に資する**。出典: [smallest.ai Fish Audio pricing 2026](https://smallest.ai/blog/fish-audio-pricing-plans-api-billing-commercial-use-in-2026) / [triad-city-beat Fish Audio Review 2026](https://triad-city-beat.com/fish-audio-review-the-most-natural-sounding-voice-cloning-platform-in-2026/) / [fish.audio terms](https://fish.audio/terms/) / [fish.audio ai-clone-famous-voices](https://fish.audio/blog/ai-clone-famous-voices-2026-guide/)
- ⚠️ **【Fish Audio S2 Proアップデート差分・出典：RKJ`Krd3H7bi8SQ`＝20本目の続編＋Web裏取り】「ElevenLabs超え」は"自社実施ブラインドテスト"が根拠＝割り引く**。Fish S2 Pro＝**2026-03-09にApache2.0でモデル重み/学習コード公開（オープンソース・商用デプロイは別ライセンス）✅／Fish自社の本番トラフィック10%サンプリング10日間ブラインドA/B（5,000超ペア）でS2 ProがBradley-Terry3.07で総合1位・ElevenLabs V3は3位、直接対決60:40でFish勝利✅**＝**ただしFish自身が設計・実行＝プラットフォーム親和性バイアス(リピーター約30%)・配信サブセット非均一＝中立な"超え"断定は未確定❓＝「競合と互角〜上位の実力派」までが妥当・"世界最強クラス"は誇張寄り⚠️**。「1000万時間学習」は❓未検証(数値は断定回避)。新規差分機能＝**①同時2結果生成②感情タグ(手動挿入＋AI「強化」で自動付与)③ボイスチェンジャー(Speech-to-Speech＝当たり出力のイントネーションを保ち声だけ変換でクレジット節約)④最短10秒クローン**＝**いずれもElevenLabs(Voice Changer/v3オーディオタグ/複数バリエーション/Instant Voice Cloning)で同型に代替可＝Fish固有の決定的優位は薄い**。料金再裏取り＝**Plus$15(年$11)月200分1.5万字/Pro$100(年$75)月1,620分3万字/Max$999(年$749)月6,250分・商用は有料必須✅**。**有名人/版権キャラ声(トランプ/サスケ/コナン/ドラえもん等の実演)＝パブリシティ/肖像/著作権リスクで✕採用不可**(本KB下節WAN2.5「芸能人は採用不可」・[[kb-ai-image-generation]]版権キャラ不採用と同境界)。**結論据え置き＝音声内製はElevenLabs◎(既存skill)＝Fish S2 Proへ乗り換える積極理由は薄い／採用はElevenLabs不足用途＋費用根拠で承認制**。感情タグは子育ての感情ナレ(就寝/読み聞かせ)、ボイスチェンジャーは音声引き直しコスト削減([[feedback_validate_method_at_min_cost_before_scaling]]の音声版)に応用余地。出典: [fish.audio blind-tts-provider-comparison-2026](https://fish.audio/blog/blind-tts-provider-comparison-2026/) / [fish.audio plan](https://fish.audio/plan/) / [neuronad Fish Audio vs ElevenLabs](https://neuronad.com/fish-audio-vs-elevenlabs/) / [groundy fish-speech](https://groundy.com/articles/fish-speech-open-source-tts-model-that-s-threatening/)
- 🟡 **【トーキングアバター/リプシンク＝AIアバター動画・出典：RKJ`h_CKaUOZuRU`＝22本目（Topview avatar4）＋Web裏取り】当組織はHiggsfield Lipsync Studio◎で内製可（8/9本目既出）＝Topview新規契約は不要／"人物アバターを主役にするか"はオーナー判断**。**比較＝Higgsfield Lipsync Studio◎＞Topview○条件付き**。**Higgsfield Lipsync Studio＝Speak v2・lipsync-2 Pro・InfiniteTalk・Kling AI Avatar・Kling Lipsync・Veo3を束ねたオールインワン**＝**InfiniteTalk＝無制限尺のトーキングアバター（リプシンク＋頭の動き＋体勢＋表情を一致・1080p/48fps）／lipsync-2 Pro＝動画to動画のスタジオ級リップシンク**＝当組織はMCP接続済の既存スタック＝**台本（Claude）→音声（ElevenLabs・ボイスクローン含む）→元画像（NanoBanana 0cr WebUI無印）→Higgsfield Lipsyncで内製可**。**Topview（トップビュー）＝avatar4/i2v/ボイスクローン/バイラルクローン（Sora 2 powered）を1ツールに統合・UI簡便✅・Pro $16/月（年払い・商用ライセンス付き・per-video $0.5〜0.72）と低廉だが、(a)新規有料サブスク＝原則契約しない[[feedback_ask_before_action]]（同等を既存スタックで保有済＝"安い"≠"契約すべき"）(b)著名人テンプレ〈MJ/アインシュタイン/チャップリン〉＝パブリシティ/肖像権・故人デジタルレプリカ規制〈CA AB1836・NY〉で✕採用不可（本KB下節WAN2.5「芸能人は採用不可」・9/20/21本目と同境界）(c)"NanoBanana 1年無制限"はTopviewキャンペーン文言でUI実値要確認❓**＝採用はHiggsfield不足用途＋費用根拠で承認制。**【最重要・方針論点】"AIアバター（合成人物）を主役にする"のは「顔出しなし」と別物**＝植物/子育てブランドにAI人物プレゼンターが合うかは別問題＝**即採用せずオーナー判断事項**／**非人物オリジナルキャラ（植物擬人化・店舗マスコット・動物キャラ）なら肖像権リスクなく顔出しなし方針と親和的＝版権フリー題材に限定で応用余地（子育てQ&A解説キャラ等）**。**【最重要・SNS規約】AIアバター動画はYouTube 2026（2026-01〜）の合成コンテンツ開示が必須**＝**合成音声（AIナレ/クローン声/TTS）・AI生成ビジュアル・ディープフェイク/リアルな改変・AIが主要な物語を生成した脚本はアップロード時に「改変または合成コンテンツ」にチェック必須／AI検出システム＋3ストライク制（2回目=90日収益化停止）。ただし台本/構成/タイトル/説明/字幕の"制作補助"用途は開示不要（exempt）＝当組織のClaude台本/構成内製は開示不要のまま**＝チェック漏れは収益化停止リスク。🟢 **汎用小技＝日本語TTSの漢字読み間違いはカタカナ/ひらがな入力で回避（ElevenLabs含む全TTSに有効・植物名/店名/専門用語の誤読対策に即流用）**。出典: [higgsfield Lipsync Studio](https://higgsfield.ai/blog/Lipsync-Studio-Turn-Any-Script-Into-Performance) / [scribe Higgsfield Lip-Sync 2026](https://scribehow.com/page/How_to_Create_Talking_AI_Avatars_With_Higgsfields_Lip-Sync_Studio_in_2026__55RJhn-yTIi2yeS3EmookA) / [scribe Topview Review 2026](https://scribehow.com/page/Topview_AI_Review_2026__Is_this_AI_video_agent_worth_it_for_UGC_ads__wmSVRdDCTTCFnUMvctZUqQ) / [influencermarketinghub AI disclosure rules](https://influencermarketinghub.com/ai-disclosure-rules/) / [syncstudio YouTube synthetic disclosure](https://syncstudio.ai/blog/youtube-synthetic-content-disclosure) / [olshanlaw Einstein right of publicity](https://www.olshanlaw.com/Advertising-Law-Blog/Albert-Einstein-Image)
- ✅ **⑥差別化＝"白背景＋左上タイトル＋エンタメ風テロップ"の王道をあえて避ける／ロゴ・テロップ・演出は複数チャンネルの良い所を組合せてオリジナル化（良い所はスクショで貯める）**。模倣対象の切り分け＝**企画/タイトル/サムネ＝最も大事＝必ず模倣・中身＝唯一解なし＝複数の良い所を集めて自分の道**。grayishトーン（[[reference_grayish_design_tokens]]）で既に脱・既視感の方向。
- 🟢 **自社適用**：顔出しなし解説の全工程は当組織の確立済テーマ（植物/子育て）の長尺・WP/note記事設計に同型転用可（②⑦⑧Claude完結・生成不要／③⑦画像はNanoBanana系で要承認・最安検証）。⚠️外部有料ツールは原則契約せず既存スタックで代替。⚠️顔出し前提の素材/美肌・小顔/アバターは顔出しなし方針で型のみ。⚠️解説ジャンルそのものへの新規参入は[[project_revenue_needs_bigger_levers]]で別途判断＝"型"だけ既存テーマへ転用（17/18本目と同じ）。**「月70万」は自己申告＋末尾に講座導線（情報商材構造）＝額は参考外・構造（規模＋付加収益）のみ採用（[[kb-youtube-monetization-models]]モデルC）**。

## 素材調達（B-roll）— 著作権に注意
- ⚠️ RKJ流「他人のYouTube映像/映画ワンシーンを画面録画して欲しい箇所だけ取得」（ScreenFly等の無料録画ソフト）は**手間削減の発想は有用だが、他者映像の転用は著作権リスク**。自社は採用しない。
- 🟢 自社代替：**自前の店舗写真／AI生成素材（Higgsfield）／フリー素材（Pexels）**に限定（[[reference_pexels_cloudflare_ua]]）。"必要部分だけ確保して無駄なくする"という効率思想だけを採り入れる。

### ✅実証済B-roll合成レシピ（NanoBanana/実写背景＋Pexels分離オーバーレイ＋ffmpeg screen＝0cr・Claude完結）
> 2026-06-16実証（B1-1検証）。背景静止画に雪/炎/雨等の"動く要素"を無料合成し「AIっぽくない」実写ドキュ調B-rollを作る確定手順。背景はNanoBanana(0cr WebUI・手動)生成 or Pexels実写静止画で代用可。素材取得・合成・色補正はClaude完結・無料。
- ⚠️**Pexelsの汎用クエリ（"rain"等）は街の実写シーン全体が返る＝オーバーレイに使えない**。**クエリに "black background"/"overlay" を付けて黒背景に要素だけ分離した素材を取る**（例 `snow falling black background`）。雪・炎は分離素材が豊富、雨は少なめ。候補は1フレーム抽出して輝度平均(YAVG)が低い(≈18-23)＝黒背景を確認してから採用。
- ⚠️**ffmpeg blendの色バグ＝合成が全面マゼンタ（緑チャンネル欠落）**＝blend前の入力フォーマット不一致が原因。**blend直前に両入力へ `format=gbrp` を明示・blend後に `format=yuv420p`**で解消。
- 🟢**質感を実写ドキュ調に寄せる**＝背景に `zoompan` のゆるいズームイン（カメラ移動）＋`eq=saturation=0.92:contrast=1.04`（彩度を僅かに落とす）。screen合成は`all_opacity≈0.85`。生成後は必ず実写と並べて質感差チェック（[[feedback_ai_clip_match_stock_footage]]）。
- **確定コマンド**（bg.jpg＝背景／snow.mp4＝黒背景分離オーバーレイ・8秒1080p）：
```
ffmpeg -y -loop 1 -t 8 -i bg.jpg -stream_loop -1 -t 8 -i snow.mp4 -filter_complex "\
[0:v]scale=2304:1296:force_original_aspect_ratio=increase,crop=2304:1296,\
zoompan=z='min(zoom+0.0007,1.15)':x='iw/2-(iw/zoom/2)':y='ih/2-(ih/zoom/2)':d=240:fps=30:s=1920x1080,\
setsar=1,eq=saturation=0.92:contrast=1.04,format=gbrp[bg];\
[1:v]scale=1920:1080,setpts=PTS-STARTPTS,format=gbrp[ov];\
[bg][ov]blend=all_mode=screen:all_opacity=0.85,format=yuv420p[out]" \
-map "[out]" -r 30 -t 8 -c:v libx264 -crf 20 -pix_fmt yuv420p out.mp4
```
- 🟢**残る手動依存＝背景のNanoBanana(0cr WebUI)生成のみ**（Claude自動操作はbot検知で封印・[[feedback_higgsfield_credit_conservation]]）。それ以外は無料・自動。第2弾ヘルクラネウム等のB-rollに即流用可。

## 制作ツールの注意（InVideo AI 等の無料枠）
- ✅ **InVideo AI 無料プランは透かし付き・商用利用不可・書き出し回数/生成分数に制限**。透かし除去は有料（Plus $25/mo〜）。→**収益用途の動画には不向き**。出典: [flowith](https://flowith.io/blog/invideo-pricing-2026-free-vs-plus-vs-max/) / [mymarky](https://mymarky.com/blog/invideo-ai-free-plan-limits-2026)
- 🟢 教訓：紹介ツールは「無料で使える」と言われても**透かし/商用可否/出力制限を必ず確認**。自社は既存スタック（Claude/Higgsfield/Pexels/CapCut）を優先し、新規有料ツールは費用根拠提示の上で承認制（[[feedback_ask_before_action]]）。

## Higgsfield Canvas × Claude(MCP/Skills) でのキャラ一貫性ワークフロー（変身/identity系ショート）
> 出典：RKJ動画 `HjtW-wqY5Wc`（1700万再生ショート・Seedance2.0&Claude）＋Web裏取り（higgsfield公式 canvas-intro/mcp/seedance2.0, vo3ai, 2026）。

- ✅ **Higgsfield Canvas＝ノードベースのワークフロー**が実在。テキストノード・画像ノード・生成ノードを線で接続して連鎖（生成→アニメ化→アップスケール）。**1度組めばテンプレ化して新キャラ/新題材へ流用＝資産化**できるのが最大利点。Figma的に共有リンクで同時編集可。出典: [higgsfield canvas-intro](https://higgsfield.ai/canvas-intro)
- ✅ **キャラクターシートが一貫性の肝**（公式推奨）。効果が高い型＝**3:4比率／上段＝全身3構図＋服装ディテール／下段＝顔クローズアップ＋背景込み全身**。顔参照＋衣装参照を1枚のproduction-readyなシートに融合し、各ショットで照合する。素材は近い既存画像（Pinterest等）を集めて生成ノードをコピー量産→質感の違いは「融合」ノードで統合。出典: [higgsfield seedance/2.0](https://higgsfield.ai/seedance/2.0)
- ✅ **Claude×Higgsfieldの3接続経路**：①Canvas上で直接生成（**最速**）②Claudeチャットに**MCP接続**（Settings→Connectors→カスタムコネクター→MCPのURL貼付→サインオン承認。権限を毎回聞かれないよう「常に許可」に。**生成時はモデル名と画像比率をプロンプトに明記**しないと動かない。**チャットMCPは画像添付しての修正不可＝テキスト生成のみ**／ネットワーク制限）③**Claude Code＋Skill**（Webでなく**デスクトップ/CLI必須**。画像添付しての修正が可能だが開発者向け）。新規アカウントは無料クレジット付き。出典: [higgsfield mcp](https://higgsfield.ai/mcp) / [higgsfield blog MCP](https://higgsfield.ai/blog/Generate-AI-Videos-From-Claude-with-Higgsfield-MCP)
- ✅ **クレジット節約の核＝「複雑度で一発か反復かを先に判断」**：安いツールを探すより、**シンプル構成（登場物少・単純な動き＝ダンス等）は一発成功率が高いので最初から720p**。逆に**難易度高（複数キャラ・落下→変身前→過程→後など多段構成）は4〜10回前提→1発目480p通常モードでテスト→Claude(Skill/Code側)でテキスト修正→2回目から720p**。Seedance2.0は5秒≈25cr/15秒720p≈90cr・1080pは約2倍＝実際に高い（480p/ファストの具体値はソース未確定❓・UI実値を正に）。出典: [vo3ai pricing](https://www.vo3ai.com/blog/seedance-20-pricing-on-runway-vs-higgsfield-vs-topview-real-cost-per-video-in-20-2026-04-08)
- ⚠️ **ファストモードは品質明確に劣る**＝正しいプロンプトでも失敗しうる。**通常モードで解像度を落とす**のが正解。**チャットで修正を送り続けると結果が劣化**→1回目の修正はSkill/Codeでテキスト言語化（写真添付が要る時だけチャット）。動きながら日本語を喋る動画は読み間違いが出るので最低2回前提。
- 🟢 **言語化が難しい動き（変身過程等）はGensparkに動画を1フレーム分析させ日本語描写を取得**して生成プロンプトへ流用＝無料小技。**ハードカット/ジャンプカット禁止**と明記でシームレスなトランジション。
- 🟢 **自社適用**：このワークフローは当組織の既存方針と同型＝[[feedback_validate_method_at_min_cost_before_scaling]]（最安検証→スケール）/[[project_short_video_pr_strategy]]/[[kosodate-loop-video-SUMMARY]]。植物PRは静止物・登場少＝シンプル側で720p寄り候補／ストーリー系は480pテストから。生成は全工程クレジット消費＝**要承認・既定0crはWebUI手動**（[[feedback_higgsfield_credit_conservation]]）。実写人物変身バズは顔出しなし方針で**型のみ転用**（落下→変身のフック・物語性・冒頭の強い動き）、他人バズ動画スクショからのプロンプト化は**構造モデリングまで**（逐語/映像転用NG）。

## Seedance2.0マルチモーダル・プロンプト術（Claude設計→Seedance映像化／実写連携シームレス連結）
> 出典：RKJ動画 `0p8hYs2P_7g`（Seedance2.0×Claude/映画風AI動画）＋Web裏取り（companionlink/seedance-2ai/higgsfield公式/frankx/spreshapp/vo3ai, 2026）。

- ✅ **役割分担＝Claudeが台本/シーン設計/プロンプト言語化、Seedance2.0が映像化**。当組織はClaude Code＋Higgsfield MCPで同型に構築可（4本目の3接続経路の③）。RKJ流は「Higgsfield配布の"動画プロンプトビルダー"SkillをClaude Webに読ませ→日本語で内容を渡し→Seedance用プロンプト生成」。
- ✅ **日本語確認レイヤー**：Claudeに4セクション出力させ**セクション1=英語プロンプト本体／セクション4=日本語の内容確認**。**先にセク4を日本語で点検→OKならセク1を逐語コピー**。長尺になりがちなら「ワンカットで15秒(尺)で」と明示再依頼。複雑な構成は**一発成功しない＝順番が崩れたら順序を言い直して2〜3回修正**前提。[[feedback_prompt_copypaste_full]]（コピペ完全形）と整合。
- ✅ **実写連携シームレストランジションの2大コツ**：①プロンプト末尾に**「途中で一度も切らず最初から最後までずっと繋がった1回の撮影」**＝シームレス化。②**実写なら`NO 3D, NO CG, NO cartoon`**を追記でリアル維持（※2Dイラスト題材には入れない＝逆効果）。複数画像は`@image`/`@image2`、生成済み動画は`@video`で結びつけ「もっとCMっぽく・テキストなし・BGM&効果音あり」等で再アレンジ可。
- ✅ **Seedance2.0は"最後のフレーム以降の動きも自然に継続"**＝従来のキーフレーム連結（最後の写真で動きが止まり不自然）と異なり**後補正不要**。extension=最終フレームを権威ある参照として動き・スタイル・内容の連続性を保ち継続／最初のフレームのみ指定時は forward extrapolation で後続を予測生成。最大15秒まで連続性維持。**植物の成長タイムラプスや子育てループ動画の"連結の不自然さ解消"に直結**。出典: [companionlink](https://www.companionlink.com/blog/2026/02/video-extension-explained-how-to-seamlessly-continue-any-clip-with-seedance-2-0/) / [seedance-2ai first-last-frame](https://seedance-2ai.org/blog/ai-video-first-last-frame-guide)
- ✅ **Soul2.0＝高リアル人物／Soul Cinema＝プロンプト無しで映画的構図・色味に自動調整／Color Transfer＝色見本添付でカラーパレット指定**。Soul Characters設定40cr・以後パラメータ化で1キャンペーン1セッション。Marketing Studioは商品画像1枚から9種広告(Talking Head UGC/Unboxing等)。**Soul画像は安価**(動画では0.5cr/枚と主張＝❓UI実値で確認・参照画像の大量試行向き)。出典: [frankx](https://www.frankx.ai/blog/ultimate-higgsfield-workflow-2026) / [spreshapp](https://spreshapp.com/article/comparing-higgsfield-openart-heygen-for-ugc)
- ⚠️ **「Higgsfieldが最安」は一部誇張**。per-clip最安は**Topview Pro $18/480生成**(マーケ特化・カスタム性低)。Higgsfieldは**"フル創作制御つきでは最安級"**が正確。**Higgsfield上Seedance2.0は米国・日本では非提供**との記載あり＝⚠️**重大な地理制限の可能性＝日本リージョンでの実機確認が前提**(MCP経由は別経路で過去に課金実行実績[[kosodate-loop-video-SUMMARY]]＝MCPなら可の可能性)。**Plus($34〜48/月)以上でないとSeedance2.0 Proは使えずStarterはFastのみ**。出典: [vo3ai pricing](https://www.vo3ai.com/blog/seedance-20-pricing-on-runway-vs-higgsfield-vs-topview-real-cost-per-video-in-20-2026-04-08)
- 🟢 **自社適用**：◎「Claudeで日本語確認付きプロンプト設計→Seedanceで映像化」の役割分担と、◎「切らない1回の撮影／実写は`NO 3D,CG,cartoon`／最終フレーム以降も動き継続」は**植物PR・子育て動画に即流用できる汎用プロンプト技**。○Soul Cinema/Color Transferでgrayishトーン([[reference_grayish_design_tokens]])を色見本にしたブランド一貫色味、○NanoBanana背景生成→Seedanceトランジションでビフォーアフター動画。△実写人物UGC/AIインフルエンサーは**顔出しなし方針で型のみ**（人物を出さず物撮り＋テロップの商品紹介ショートに転用）。生成は全工程**クレジット消費＝要承認・最安検証(480p通常)から**([[feedback_validate_method_at_min_cost_before_scaling]]/[[feedback_higgsfield_credit_conservation]])。植物は静止・カメラのみ[[feedback_plant_video_direction]]を維持。

## 動画生成モデルの選択・コスト・規制（WAN2.5 / Veo3 / Sora2 / Kling）
> 出典：RKJ動画 `T9I-qpfW7ro`（Higgsfield WAN2.5）＋Web裏取り（2026）。読むべき時：どの動画モデルで生成するか・コスト/規制を見積もる時。

- ✅ **WAN 2.5＝Alibaba(DAMO)製・Veo3の低コスト代替**：最大**1080p・10秒・24fps**（4Kはプレビュー）、**テキスト/画像/音声/動画入力**対応、**ネイティブ同期音声（台詞・環境音・BGM）を1プロンプトで同時生成**、多言語(8言語以上)。Higgsfieldで利用可。出典: [higgsfield WAN2.5 Review](https://higgsfield.ai/blog/Introducing-WAN-2.5-Review) / [higgsfield wan-ai-video](https://higgsfield.ai/wan-ai-video)
- ⚠️ **「Veo3/Sora2超え＝最強」は誇張（条件付き）**：勝っているのは**①コストの安さ ②表現規制の緩さ**であって、総合品質の王座ではない。2026の比較では市場は**Veo3.1/Kling3.0を軸に集約**、Sora2は規制が厳しく中央集権モデレーション。「Sora2超え」は**規制自由度の文脈なら妥当・品質総合では誇張**。出典: [Readability "Sora2 vs Veo3 and Wan2.5" 2026](https://www.readability.com/sora-2-vs-veo-3-and-wan-2-5-the-definitive-2026-comparison-of-ai-video-generation-technologies) / [wanvideogenerator "Wan2.5 vs Sora2"](https://wanvideogenerator.com/blog/wan25-vs-sora2)
- ❓ **「規制なし（ポリシーが緩い）」は現在当てにできない＝policy drift**：初期は無検閲出力が話題だったが**直近数ヶ月でモデレーションが強化**との指摘。**「規制が緩い」前提で過激/版権路線を組むのは危険**（[[feedback_shop_article_rules]] とも整合）。自社は健全ブランド＝そもそも過激用途は✕。
- ⚠️ **コスト：「無制限」はPlus/Ultra課金前提＝0cr無料枠ではない**：Higgsfield＝**Starter $15 / Plus $39 / Ultra $99（年契約・月200/1,000/3,000cr）**、WAN系の無制限は**Plus・Ultraの7日ローリング無制限**。**WAN2.5 5秒≒9cr・Veo比1/10**は動画の自己申告（方向性は妥当・一次単価表は未確認＝方向性のみ採用）。MCP生成は常に課金（[[higgsfield_unlimited_model_economics]]）。出典: [imagine.art Higgsfield Pricing 2026](https://www.imagine.art/blogs/higgsfield-ai-pricing) / [vo3ai Higgsfield Pricing 2026](https://www.vo3ai.com/higgsfield-ai-pricing)
- ✅ **モデルは1つに固定せず使い分け（実証レシピ）**：**WAN2.5 Fast（品質差ほぼ無し・安い）で試写→破綻したらKling2.5 Turbo（ダイナミックで安定）に切替**。「複雑度で480p試写/720p一発を切替」方針と同型のコスト最適化。
- ✅ **動作制御プロンプト技（版権/規制リスクゼロで即転用）**：①**動作の順番は番号（1.2.3.）でプロンプトに明記**＋設定で尺を10秒に＝複数アクションの順序制御。②**意図せぬ動きはMotion編集（Prompt to Video）で対象を囲み矢印で方向指定**＋短文補足＝細かな動作方向の補正。リップシンクは**写真1枚から直接ならWAN2.5が楽／真人Lipsync2 Proは動画入力必須で高精度だが手間**。
- ✅ **1本＝複数素材の合成という工程設計**：初期画像→複数カメラアングル（引き/煽りはPhotoshopで周辺生成して距離を作る）→セリフ生成→音声→リップシンク→アクション動画。identity一貫性は**作り込みで担保**＝ワンショットで安定はしない（参照画像WFと併用前提）。
- 🟢 **自社適用**：○**WAN2.5は試写コスト削減＆"1プロンプトで環境音/BGM同時付与"が利点**＝植物PRの環境音・ショートBGMを別工程なしで付与できる可能性（ただし音声はElevenLabs内製が基本＝まず無音生成＋ElevenLabs、WAN2.5同時音声は試験的検証）。○**Fast最安試写→破綻時Kling2.5 Turbo**のモデル使い分け、○**番号で動作順・矢印で方向補正**は植物（静止＋カメラ移動 [[feedback_plant_video_direction]]）/子育てループの連結に即流用。✕**芸能人/過激/血/性的・版権キャラは採用不可**（規約・肖像/パブリシティ権・自社ブランド非整合＝技術だけ抜く）。生成は全工程**クレジット消費＝要承認・5秒Fastの最安から検証**（[[feedback_validate_method_at_min_cost_before_scaling]]）。

### Kling 3.0 Motion Control（写真→参照動画で動きを転写）— i2vを既定とし"特定カット専用"の精密ツール
> 出典：RKJ動画 `LuKmTh8gyQI`（Artlist AI Music ＋ Kling 3.0）＋当組織コスト実測[[reference_kling_multishot_cost_reality]]。読むべき時：静止画（図解・人物図・遺跡図・古写真）に動きを付けるカットで、普通のi2vとMotion Controlのどちらで作るか選ぶ時／オーナーへ採否を提案する時。**結論＝既定はi2v・Motion Controlは"決まった動きが要る特定カット"が出た時だけ提案する**。
- ✅ **Motion Control＝写真1枚＋参照動画→参照動画の動きを被写体に転写**して動画化＝"決まった動き"を正確に再現する**精密ツール**（汎用の画質アップ機能ではない）。Higgsfield保有済み・追加サブスク不要。Klingの動作タイミング指定は実際は**ブラケット構文**（`[actor:X][action:Y][duration:1s]`）。意図せぬ動きはMotion編集（Prompt to Video）で対象を囲み矢印で方向補正（本KB上節WAN2.5の動作制御技と同型）。
- ✅ **普通のi2v（image-to-video）との役割の違い＝採否判断の核**：i2v＝静止画＋テキスト→AIが自然な動きを創作＝**カメラ寄り/パン・光のゆらぎ・スロー演出など"周辺的な動き"が得意**で安く簡単（現行RUNBOOKの既定）。Motion Control＝**人物が特定の所作をする・物体が決まった軌道で動く等の"特定の決まった複雑な動き"を正確に再現したい時だけ**価値が出る。
- ✅ **コストは判断材料にならない（1本100cr上限は余裕でクリア）**：配信先で十分なpro(1080p)なら 5秒≈8.75cr／15秒≈26.25cr（[[reference_kling_multishot_cost_reality]]。4Kは5秒30/15秒90crでオーバースペック）。普通の生成もMotion Controlも同じKling生成で**桁は変わらず**、現行AIインサイト制作は1本≈35cr。⇒**採否はコストでなく"クオリティ／質感適合"だけで決める**。
- ⚠️ **当チャンネル（AIインサイト＝実写ドキュ調）では大半の図解カットがi2vで十分＝Motion Controlの上乗せ効果はほぼゼロ〜わずか**。むしろ人物を動かしすぎると"AIっぽさ"が出て、ストック素材との質感統一（[[feedback_ai_clip_match_stock_footage]]）を崩すリスク＝乱用は逆効果。
- 🟢 **採用判断＝提案する時の型**：「人物・物体の決まった動きが要る特定カット」が出てきた時だけ提案する。**最安構成（pro/1080p/5秒≈8.75cr）で1カット試作→i2vと並べて目に見えて勝てば採用、勝たなければ不採用**（[[feedback_validate_method_at_min_cost_before_scaling]]／[[feedback_higgsfield_credit_conservation]]）。生成はクレジット消費＝**GO（試験生成）はオーナーのみ**・コスト確認（`generate_video`の`get_cost:true`ドライラン）まではcr消費ゼロ。植物動画は静止＋カメラのみ[[feedback_plant_video_direction]]を維持。
- 🟢 **最初の検証機会＝第2弾ヘルクラネウム等で「歴史人物が特定の所作をするカット」が出た時**。古写真・歴史人物を動かすフロー（Seaart ControlNetで実写化→Kling/Seedanceで動かす）の"動かす"工程にも本ツールが該当。

## AIエージェント型の自動パイプライン（Skywork等）— 中身はClaude＋MCPで内製可・放置量産は不採用
> 出典：RKJ動画 `4oZvxqHWmts`（チャンネル開始者向け・自動で動画を作る方法）＋Web裏取り（skywork公式, 2026）。**ツールは1本目`7YxEndCBYlU`/`F40dpeuQl3Y`で既出のSkywork**。読むべき時：「指示書1つで全自動」を謳うAIエージェントの実体と、自社で採るべき部分/避けるべき部分を判断する時。

- ✅ **正体＝Skywork（スカイワーク）AIエージェント**＝**①ツールモード（モデルを選び画像/動画を個別生成）②エージェントモード（指示書1つで台本→画像→動画→出力まで自動連鎖）**の2モード。画像=Nano Banana Pro／動画=Klingを選び、**Canvas形式で写真→動画の順に出力→最終動画リンクからDL**。スマホのブラウザ版でも可。RKJ概要欄のクーポン`RKJ_VIDEO`が紐づく＝アフィリ誘導動画。出典: [skywork AI video creation agent JP](https://skywork.ai/agent/jp/ai-video-creation-agent-2018320364972335104) / [skywork ultimate AI agent guide](https://skywork.ai/skypage/en/ultimate-skywork-ai-agent/2033789235665649664)
- ✅ **有料プランで透かし除去・商用利用可**（Standard≒$28/月〜・クレジット増・高解像度書き出し）。ただし**Nano Banana Pro無制限はUltra課金前提＝0cr無料枠ではない（既出7本目／[[kb-ai-image-generation]]）**。出典: [skywork copilot pricing 2026](https://skywork.ai/skypage/en/skywork-ai-copilot-pricing-2026/2034521488383823872)
- ❓ **チャンネル提示のクレジット値（Standard=24,000/Ultra=144,000・冒頭動画130cr・台本映像化300cr）は実演値で未検証**＝公開情報にPro $19.99/7,000cr等の別体系あり＝**アプリ実値で確認・断定回避**（[[reference_kling_multishot_cost_reality]] UI実値を正）。
- ⚠️ **「指示1つで全自動・面倒なロードを全消去」は一部誇張**＝動画自身も「AIなのでエラーは多少ある」と認める＝完全放置の量産可ではない。
- ✅ **自社の優位＝同等を新規サブスクなしで内製できる**：エージェントの中身（台本→記号でカット分割→各カットのNanoBanana画像プロンプト→Kling動画プロンプト→順次生成）は、**当組織のClaude Code＋Higgsfield MCP（`generate_image`→`generate_video`）で構築でき、Skyworkという別有料ツールは契約不要**（[[feedback_ask_before_action]] 費用根拠なき新規採用はしない）。Claudeがカット割り＋プロンプト設計、MCPが映像化＝4〜5本目の役割分担と同型。
- ✅ **版権/規約リスクゼロで即転用できる汎用プロンプト技**：①**台本のカットは記号で区切る**（カット数を制御）②**画像にテキスト非表示**（`NO TEXT`系）③**参照写真でキャラ・画像スタイルの一貫性を担保**（メインキャラ画像を添付）。既存のキャラシート一貫性WF（本KB上節）に統合。
- ⚠️🚫 **"ボタン1つで25カット/10本を一括自動生成"＝放置量産は当組織では採用しない**：生成は全工程クレジット消費＝コスト暴走＋[[kb-youtube-ban-avoidance]]の「AI量産・属人性ゼロ」BANリスクに直結。**最安検証（1カット/最短/低解像度）→品質OK→人間（PMレビュー）が企画/品質判断→必要本数だけ生成**を厳守（[[feedback_validate_method_at_min_cost_before_scaling]]/[[feedback_higgsfield_credit_conservation]]/[[feedback_workflow_real_roles_not_labels]]・14本目S5「企画/台本をAIに任せると失敗」と整合）。
- ✕ **実写人物（AIタレント）ショート・実在アニメ作品名（鬼滅等）を題材にした"実写映画の裏側"系は版権/肖像・パブリシティ権でNG**（[[feedback_shop_article_rules]]）。型（テンプレ＋自動化プロンプトの2段重ね）だけ、版権フリーの植物/オリジナル題材へ転用。
- 🟢 **自社適用**：植物PR/子育てショートの"カット単位で画像生成→動画化"を、Claudeがカット割り＋各プロンプトを生成しMCPで順次実行する**半自動フロー（ただし最安検証→人間レビュー→必要本数のみ・放置量産はしない）**に落とす。新規サブスク不要・既存スタックで完結。植物は静止＋カメラのみ[[feedback_plant_video_direction]]を維持。
- ✅🆕 **Skyworkの"非動画＝AIオフィススイート"側も同様に内製可**（出典＝RKJ 23本目`F40dpeuQl3Y`「全機能徹底解説」＝動画でなくビジネス文書/スライド/Webページ等の網羅回）。**1指示で6種類を同時出力（文章/スライド/表/Webページ(HTML)/ポッドキャスト/YouTube動画分析〈YouVibe＝最大2hの動画をハイライト/スライド/マインドマップ化〉）＝AIオフィススイート**＝✅実在。**これらも当組織で代替可**：リサーチ→レポート/構成＝Claude＋WebSearch/WebFetch（既出18本目）／YouTube動画分析＝Claude＋`youtube-transcript`（既出17本目Mapify節）／ポッドキャスト＝ElevenLabs skill（19〜22本目）／スライド・Webページ＝ClaudeのHTML/アーティファクト生成／表・定期タスク＝Claude Code＋GitHub Actions＝**Skyワーク新規契約は不要**（16/18本目で結論済・[[feedback_ask_before_action]]）。**型として有用＝「1指示でリサーチ→複数フォーマット同時展開」のワンソース・マルチユース**（1つの調査ネタ＝植物の育て方/子育てグッズ比較を→WP記事＋note＋YouTube台本＋将来ポッドキャストに横展開）。料金＝**無料 初月毎日500cr→以降週500／Pro $19.99/月 7,000cr・DeepResearchフル・商用可／年間 $149.99**（✅裏取り・時点依存）。**「作業効率2倍」は定量根拠なし＝割り引く⚠️／AI要約・レポートは一次ソース照合してから断定（source-grounding・17/18本目）**。出典: [skywork YouVibe agent](https://skywork.ai/blog/a-complete-guide-to-skywork-youvibe-agent/) / [skywork Podcasts agent](https://skywork.ai/blog/a-complete-guide-to-skywork-podcasts-agent/) / [skywork copilot pricing 2026](https://skywork.ai/skypage/en/skywork-ai-copilot-pricing-2026/2034521488383823872) / [skywork super agents PRNewswire](https://www.prnewswire.com/news-releases/skywork-launches-skywork-super-agents-globally-the-ai-powered-office-suite-built-on-deep-research-302463133.html)

## RKJ P3抽出ノウハウ（動画制作技術・一行索引・実施可否は後日判断）
> 2026-06-16・RKJ調査P3の網羅版から「動画制作技術/AI生成/モデル・ツール」系の一行索引をTASKS.mdから移管・逐語保持。実施可否は制作時に個別判断。番号#はP3マスターリストの施策番号。各項目の詳細手法は本KB上節（モデル選択・Seedance/Kling・Higgsfield WF等）と重複する場合あり＝ここは"漏れ防止の索引"。
- **#1-c スマホ完結合成（Hypic切り抜き+CapCutウルトラキー）**：急ぎのショート/Instagram制作向け。コスト0（植物YT/myraisingdiary）
- **#1-d MotionBrush（Runway）部位別動き付与：無料4秒×26回**：Ambient=10で炎/霧/髪のゆらぎ。Higgsfield motion_controlで代替検討可。無料26回はcr節約に直結（AIチャンネル）
- **#1-e カメラパン（左→右）でリアリティ大幅UP**：After Effects 2ノードカメラ/Higgsfield motion_control代替可（AIチャンネル）
- **#1-g 合成色補正：外景と室内の彩度・明度を一致させると合成つなぎ目消える**：ffmpegのeq/scaleで実装可（AIチャンネル）
- **#2-a Veo3 T2V：キャラクター一貫性1分動画・Higgsfield統合済**：当社スタック内・追加サブスク不要（AIチャンネル）
- **#2-b Veo3カメラアングルプロンプト語彙（low angle tracking/crane/close-up+rise/POV）**：プロンプトライブラリに追記（AIチャンネル）
- **#2-c Veo3日本語ナレーション改善：キャラ設定+環境に「Japanese/Japan」追加**：発音・アクセント大幅改善（AIチャンネル）
- **#2-d Veo3字幕消し：末尾「no subtitles」必須/セリフに「/」を入れると字幕バグ**（AIチャンネル）
- **#2-e Veo3縦動画対応：縦写真を横90度回転して入力→縦長動画生成**（Shorts/Instagram Reels向け・植物YT/myraisingdiary）
- **#2-f Veo3弱点整理（声一貫性なし/1動画100cr/拡張でVeo2降格/横のみ）**：コスト判断の基準として参照（AIチャンネル）
- **#3-a Kling/NanoBanana：写真1枚→複数アングル一括マルチカット生成（要検証）**：Higgsfield MCP実装可否不明・WebUI 0crで試せる可能性あり（AIチャンネル）
- **#3-b Claudeとの「番号で答える対話」でプロンプト品質向上**：Claude Code+Higgsfieldで既に同等・意識的な活用が効果的（全般）
- **#3-d モデル選択指針：Kling2.6（高品質課金）/Kling2.5（無料可）/Veo3.1（cr大・高品質カットのみ）**：RUNBOOK Step5のモデル選択基準に補記（AIチャンネル）
- **#3-f 試行前提でcr予算を積む：1回で完璧を期待しない・3回出して良いものを選ぶ**（全般）
- **#4-b ComfyUIローカル実行は当社不可（Mac VRAM不足/100GB+/ハイスペックPC必要）**：Higgsfield MCPで代替済み・ローカル構築不要（AIチャンネル）
- **#5-a 日本語TTS文字数→音声時間目安：100文字≒25秒/2000文字≒5〜6分**：台本チャンク設計・尺見積もりに活用（AIチャンネル）
- **#6-a YouTube動画URL→音声+映像解析→要約/マインドマップ/PPT自動出力（Skywork AI相当）**：Claude+Whisper+python-pptxで内製実装可（全般）
- **#13-b AI動画延長でショートクリップを補完（Filmora/Higgsfield extend）**：2〜3秒素材をShorts尺に延長。最安構成で検証してから採用（植物YT/myraisingdiary）
- **#20-b IPリスク境界線：著名IP再現（Marvel/Disney等）は商業利用不可**：当社（古代ミステリー・植物）では問題なし（全般）
- **#21-b Lyria3（Google製音楽AI）はArtlist月額必須→当社採用見送り**：ElevenLabs+PexelsBGM継続（AIチャンネル）
- **#49-a dzine.ai：キャラクタートレーニング→複数シーンで一貫した人物表現**：1日30cr無料・新規サブスク要・Higgsfieldリップシンクで代替可（AIチャンネル）
- **#52-a Filmora15 AIサウンドエフェクト：日本語テキスト入力→効果音自動生成**：有料前提のため採用見送り（AIチャンネル）
- **#55-a Skybox AI：360°パノラマ背景→横スクロール動画化（遺跡パン動画に転用可）**：⚠️質感衝突リスク（実写ドキュ調vs360°誇張）・採用時は実写と並べて質感差チェック必須（AIチャンネル）
- **#56-b VoiceFront（声フォント）：複数キャラ別音声生成**：複数話者シナリオ時の候補・現時点はGemini TTS継続（AIチャンネル）
- **#58-a .mdスキルファイルによる多ステップ自動化パターン（Manus方式）**：Claude Code+SKILL.mdで実質同等・当社スキル設計の参照として有用（全般）
- **#61-a Veo3日本語セリフ制約：8秒=最大50文字・ローマ字必須**（#2-cと合わせて参照・AIチャンネル）
- **#63-a BGM+ナレーション+効果音セット=YouTube収益化条件実証**：当社スタックが条件を満たす設計の裏付け・収益化申請時の根拠として参照（AIチャンネル/植物YT）
- **#64-a MuRekaAI：動画/写真→イメージに合ったBGM自動生成**：月8ドル/著作権取得付き・FreePD CC0無料が主力のため即採用不要・将来シーン別カスタムBGM時の第一候補（AIチャンネル）
- **#68-a FLF2Vモーフィング成功の必須条件：同背景+近距離**：Kling3.0 FLF2Vにも同原則適用・Seedanceラストフレームルールと合わせて参照（AIチャンネル）
- **#69-a Seaart AI LoRA+ControlNet：同一歴史人物を複数エピソードで一貫維持**：Higgsfield MCPはLoRA非対応→Seaart AIが補完候補・採用時は質感チェック必須（AIチャンネル）
- **#74-a BigP：白黒動画AIカラー化（当社スタック未存在のユニーク機能）**：古代白黒ニュース映像→現代化・月額7,300円〜・導入はGO待ち（AIチャンネル）
- **#75-a 3Dモデル→任意アングルスクショ→画像生成AI参照フレーム活用**：Higgsfield generate_3dでも即活用可・構造物特定アングルの一貫再現に使える（AIチャンネル）
- **#76-a BlenderでFBX透過(RGBA)アニメーション→ffmpeg動画化パイプライン**：Blender+ffmpegは無料・MeshAIはHiggsfield generate_3dで代替可（AIチャンネル）

## 出典
- skywork「AI Video Creation Agent (JP)」: https://skywork.ai/agent/jp/ai-video-creation-agent-2018320364972335104
- skywork「Ultimate AI Agent Guide / Copilot Pricing 2026」: https://skywork.ai/skypage/en/ultimate-skywork-ai-agent/2033789235665649664 ・ https://skywork.ai/skypage/en/skywork-ai-copilot-pricing-2026/2034521488383823872
- higgsfield「WAN 2.5 Review」: https://higgsfield.ai/blog/Introducing-WAN-2.5-Review
- higgsfield「WAN AI Video」: https://higgsfield.ai/wan-ai-video
- Readability「Sora2 vs Veo3 and Wan2.5 2026」: https://www.readability.com/sora-2-vs-veo-3-and-wan-2-5-the-definitive-2026-comparison-of-ai-video-generation-technologies
- wanvideogenerator「Wan2.5 vs Sora2」: https://wanvideogenerator.com/blog/wan25-vs-sora2
- imagine.art「Higgsfield AI Pricing 2026」: https://www.imagine.art/blogs/higgsfield-ai-pricing
- vo3ai「Higgsfield AI Pricing 2026」: https://www.vo3ai.com/higgsfield-ai-pricing
- higgsfield「Canvas」: https://higgsfield.ai/canvas-intro
- higgsfield「MCP / Seedance 2.0」: https://higgsfield.ai/mcp ・ https://higgsfield.ai/seedance/2.0
- vo3ai「Seedance 2.0 Pricing 2026」: https://www.vo3ai.com/blog/seedance-20-pricing-on-runway-vs-higgsfield-vs-topview-real-cost-per-video-in-20-2026-04-08
- viewsmax「Content Repurposing Strategies 2026」: https://blog.viewsmax.com/content-repurposing-strategies/
- vidpros「YouTube Growth Strategies 2026」: https://vidpros.com/youtube-growth-strategies/
- flowith「InVideo Pricing 2026」: https://flowith.io/blog/invideo-pricing-2026-free-vs-plus-vs-max/
- mymarky「InVideo AI Free Plan Limits 2026」: https://mymarky.com/blog/invideo-ai-free-plan-limits-2026
- neoreach「Fastest Growing Instagram Accounts」（Baroji具体数値は未検証）: https://neoreach.com/top-fastest-growing-instagram-accounts/
- companionlink「Seedance2.0 Video Extension」: https://www.companionlink.com/blog/2026/02/video-extension-explained-how-to-seamlessly-continue-any-clip-with-seedance-2-0/
- seedance-2ai「First & Last Frame Guide」: https://seedance-2ai.org/blog/ai-video-first-last-frame-guide
- frankx「Ultimate Higgsfield Workflow 2026（Soul ID/MCP）」: https://www.frankx.ai/blog/ultimate-higgsfield-workflow-2026
- spreshapp「UGC Ads: Higgsfield/OpenArt/HeyGen 2026」: https://spreshapp.com/article/comparing-higgsfield-openart-heygen-for-ugc
- mapify「YouTube Video Summarizer / Chrome Web Store」（多言語要約・タイムスタンプ・無料cr/Pro）: https://mapify.so/tools/youtube-video-summarizer ・ https://chromewebstore.google.com/detail/mapify-ai-summarizer-mind/fldgacclkckcmflokaialbihppmndpbd
- bibigpt「Best YouTube AI Summarizer Chrome Extensions 2026」（AI要約のsource-grounding/ハルシネーション）: https://bibigpt.co/en/blog/posts/best-youtube-ai-summarizer-chrome-extensions
- higgsfield「Lipsync Studio（InfiniteTalk/lipsync-2 Pro・トーキングアバター）」: https://higgsfield.ai/blog/Lipsync-Studio-Turn-Any-Script-Into-Performance
- scribe「How to Create Talking AI Avatars With Higgsfield's Lip-Sync Studio 2026」: https://scribehow.com/page/How_to_Create_Talking_AI_Avatars_With_Higgsfields_Lip-Sync_Studio_in_2026__55RJhn-yTIi2yeS3EmookA
- scribe「Topview AI Review 2026（avatar/Pro$16/商用ライセンス/Sora2ビデオエージェント）」: https://scribehow.com/page/Topview_AI_Review_2026__Is_this_AI_video_agent_worth_it_for_UGC_ads__wmSVRdDCTTCFnUMvctZUqQ
- influencermarketinghub「AI Disclosure Rules by Platform（YouTube 2026合成コンテンツ開示）」: https://influencermarketinghub.com/ai-disclosure-rules/
- syncstudio「YouTube Synthetic Content Disclosure Policy 2026」: https://syncstudio.ai/blog/youtube-synthetic-content-disclosure
- olshanlaw「Einstein Right of Publicity（著名人肖像のAI利用リスク）」: https://www.olshanlaw.com/Advertising-Law-Blog/Albert-Einstein-Image
