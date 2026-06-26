# Seedance 2.0 プロンプト作法（正本＝SeaArt記事）

> ⚠️ **これは Seedance 2.0 という特定モデル固有の作法**。動画生成一般のコツではない。**他モデル（Wan2.7 / Kling / Veo 等）へそのまま流用しない。** 新モデルを使うときは、必ずそのモデルの公式/実証プロンプト例を読んでから作法を別途確立する。
>
> **正本＝SeaArt「Seedance 2.0プロンプト」記事**（https://www.seaart.ai/ja/blog/seedance-2-0-prompt ）。
> §1〜§5は記事の**構成・内容に準拠**（記事を正とする）。§6は記事に無い**社内実証の補足**で、**記事と食い違う場合は記事を正**とする。

---

## 0. メタ教訓（なぜ何度も失敗したか・最優先）

過去、人智の外側・案内人の口パクで写実が出ず5回以上失敗した。**原因はモデルでなく我々の作り方**で、最大のメタ原因は——
- **一次ソース未確認で社内二次資料を鵜呑みにし、正本同士を矛盾させた**（例：記事は「masterpiece / ultra sharp は写実向上に推奨」と書いているのに、本ファイルは一時「避けろ＝AI光沢の主因」と誤読・記載していた。2026-06-18 オーナー指摘で発覚・訂正）。
- **教訓：判断基準は必ず一次ソース（記事）で検証する。社内の言い換えを正本にしない。正本に矛盾を見つけたら生成前に必ず解消する。**

---

## 1. プロンプトの3つの基本構造（記事）

記事は用途別に3つの構造を提示。**まずどれを使うか選ぶ。**

### ① 五つの要素（初心者向け・単発カットの基本）
`主題 ＋ シーン/雰囲気 ＋ 動作/パフォーマンス ＋ カメラの動き ＋ スタイル・照明`
- 情報の並べ順は **被写体 → 動作 → カメラ → スタイル**。

### ② CRAFTマルチモーダルフレームワーク（複数素材＝@Image/@Audio/@Video を使うとき）
`Context（文脈）/ Reference（参照素材）/ Action（動作）/ Framing・Timing（構図・尺）/ Tone・Audio（トーン・音）` の5要素で整理し、**生成時はマルチモーダルモードをON**。

### ③ タイムラインストーリーボード（複数ショットの物語向け）
`0〜4s:` `4〜8s:` のように**時間区間ごとに何が起こるかを記述**。**単発・単一構図のカットでは①の5要素で十分**（過剰に刻まない）。

---

## 2. 実践テクニック（記事 ①〜④）

### ① マルチモーダル参照の優先順位（@タグの使い分け）
| タグ | 役割（記事） | 決まり文句 |
|---|---|---|
| `@Image` | 顔・衣装・全体スタイルを安定させる | `Maintain facial features and wardrobe fully consistent with @Image1` |
| `@Audio` | 口パク同期・編集タイミングのビート合わせ | `Lip-sync aligns with @Audio1` |
| `@Video` | 人物の動き・カメラワークを引き継ぐ | （動きを再現したいとき） |
- 参照画像は**構図がシンプルな上半身〜中距離のポートレート**が安定しやすい。
- 参照素材の**合計再生時間は15秒以内**。**スタイルの強さはほどほど**に（参照とモデルの創造性のバランス）。

### ② 映画制作の用語を取り入れる
カメラ表現を入れると映像が引き締まる（用語は §3）。

### ③ キャラクターの一貫性を保つ
1. **明示指示**：`Maintain facial features and wardrobe fully consistent with @Image1`。
2. **テキストでも重要特徴を記載**：`short silver hair` / `mole under left eye` 等の識別特徴を書き添える（記事推奨）。
3. **同一参照画像（@Image1）を継続使用**。
> ※旧記載「特徴をテキスト再記述しない」は記事と矛盾していたので**削除・訂正**（記事は“書き添える”を推奨）。

### ④ 長さ・情報量・書き方のバランス
- **語数 120〜280語**（短すぎ＝不安定／長すぎ＝重要情報がぼやける）。
- **肯定形で書く**（「〜しない」より「こうしてほしい」の方が理解されやすい）。記事の推奨例：
  - `Detailed hands` / `Anatomically correct` → 手・身体が自然になりやすい
  - `Physically accurate` / `Natural motion` → 動きが自然になる
  - `Ultra sharp` / `Masterpiece` → **画質・ビジュアルの質感が向上しやすい（記事が明確に推奨＝使ってよい）**
> ⚠️ **社内実証による上書き（てかり対策・詳細§6-5）**：上記は記事の一般原則。**我々の素体（マットな soul_2 KF）からの image-to-video 口パクでは、肌の写実語（`skin texture` / `pores` / `dry skin` / `matte skin`）を足すと Seedance が皮脂を描いて“てかる”**（語り部6本比較で実証）。→ **肌写実語は足さない／否定語（no specular 等）も使わない／品質語（masterpiece 等）も盛らず最小に。素体活用時は最小肯定＋保存指示を既定**（§6-5）。

---

## 3. カメラワーク用語（記事）

- **特殊ショット**：`POV`（主観）/ `360 orbit`（周囲の回り込み）/ `Rack focus`（フォーカス送り）
- **パン／ティルト**：`Pan left`（横パン）/ `Whip pan`（高速パンでダイナミックに切替）
- **トラック／ドリー**：`Tracking shot`（被写体追従）/ **`Truck left` / `Truck right`（水平移動＝横移動で追う）**
- **プッシュ／プル**：`Slow push in`（ゆっくり寄って感情強調）/ `Dramatic pull out`（引きで環境表現）

---

## 4. 照明・写実・リップシンクの出し方（記事）

- **照明・スタイル**：`cinematic style` / `film grain` 等を明示。
- **物理演算で写実**：`realistic physical simulation` / `light diffusion` / `natural fabric inertia`（布の自然な動き）/ `realistic rain/wind physics`。
- **品質水準**：`4K ultra HD` / `realistic` 等を記載（使ってよい）。
- **リップシンク**：`Lip-sync aligns with @Audio1`（音声ファイルとの同期を明示）。CRAFTで整理し**マルチモーダルモードON**。

---

## 5. ✅ プロンプト チェックリスト

### 生成前（記事準拠）
```
□ 3基本構造のどれを使うか選んだか（①5要素／②CRAFT／③タイムライン）
□ 語順は 被写体→動作→カメラ→スタイル/照明 か
□ 120〜280語に収まっているか
□ 肯定形で書いたか（「〜しない/avoid/not」を避け「こうしてほしい」へ）
□ @Image1 で一貫性を明記＋重要特徴をテキストにも書き添えたか
□ 喋るカットは "Lip-sync aligns with @Audio1." を入れたか
□ 参照素材の合計尺は15秒以内／スタイル強度はほどほどか
□ カメラ・照明は具体的な映画用語か（slow push in / truck right / film grain 等）
□ 品質語（masterpiece / ultra sharp / 4K ultra HD / realistic / physically accurate）で写実を補強したか（記事推奨・避けない）
□ タイムライン記法（0-4s等）は複数ショット/物語のときだけか
□ ＜最重要＞これはSeedance専用か（他モデルの作法を混ぜていないか）
```

### 生成前（社内・失敗対策＝§6由来。記事を上書きしない補足）
```
□ 写実意図と逆の照明（matte × 片側レイキング光）を同居させていないか（＝てかりの主因）
□ 老化に寄る肌描写（wrinkles / しわ / aged / deep texture）を入れていないか（＝老け顔の主因）
□ image-to-videoは照明/質感の大半をKF(@Image1)が支配→KFの額/鼻/頬を先に拡大点検
□ get_cost で実cr確認
```

### 生成後ゲート（社内・採用判定／1つでもfailなら不採用 or ポスト前提）
```
□ 額/鼻/頬を拡大 → てかり/光沢が見えたら fail
□ 顔が老けていない／@Image1と同一人物（同一性）
□ 口パク同期（音と口）
□ アニメ/CG化していない
→ passした物だけ採用。残るてかりはポストで roll-off（low contrast / lifted shadows / 明部roll-off。※フラットなオーバーキャスト光に置換しない＝低照度ムードを維持）
```

---

## 6. 社内実証の補足（記事に無い／記事と矛盾しない・矛盾時は記事優先）

### 6-1. アンカー例（喋るホスト＝歴史家鋳型・実証済み）
記事のデジタルアバター（歴史家）例と同型で、暗室の語り部に“狙いの写実”が出た骨格。新規プロンプトはこの骨格に被写体だけ差し替える。
> A male speaker in his early 30s, wearing a navy suit with a clean short hairstyle, projects confidence and approachability. Frontal medium shot inside a modern glass-wall office, with a softly blurred urban skyline in the background. A gentle push-in camera move. He pauses briefly, takes a soft breath, and begins to speak: "AI is redefining the way we live." Lip-sync aligns with @Audio1. His hands accompany the speech with natural, measured gestures; he occasionally nods or offers a slight smile. Consistent eye contact with the camera throughout.

骨格＝①被写体（人物＋装い＋印象）②ショット＋舞台 ③カメラ（1つのゆっくりした動き）④動作→発話→`Lip-sync aligns with @Audio1` ⑤自然な所作・視線（肯定形）。
- 暗室の語り部に移植する要素＝暗室＋暖色実用光＋視線の動き＋`film grain`（雰囲気のため）＋`@Audio1`＋ゆっくりした単一カメラ移動。realismは(a)実在ライティング (b)自然な所作（measured/restrained gestures・visible breathing） (c)@Audio1同期 (d)ゆっくりした単一カメラ移動 (e)品質語、で出す。

### 6-2. 写実が崩れる「5回失敗」の真因と対策（2026-06-18 確定）
**真因は品質語ではない。** 実際の原因と対策：
1. **照明矛盾**：matte/写実を狙いつつ片側レイキング光（皮脂を真横から舐め＝てかり最大化）。→ 顔は **soft, even な低照度光**。否定語（no specular）は効かない。
2. **老化描写**：`wrinkles / しわ / aged / deep texture` → 老け顔（[[feedback_ai_face_prompt_no_wrinkles]]）。→ 入れない。
3. **言葉で合格判定**（[[feedback_ai_human_realism_no_sheen_bar]]）→ §5生成後ゲートで**拡大点検**（[[feedback_strict_self_gate_reject_failures]]）。
4. **てかり除去の本命はポスト**（low contrast / lifted shadows / 明部roll-off）。プロンプト照明書き換えは補助。
5. **未検証の即興を避け、実証output-pairの骨格を流用**（[[feedback_copy_proven_prompt_output_pairs]]）。

### 6-3. モデルを跨ぐときの鉄則
- Seedanceの作法を Wan2.7 / Kling / Veo にコピペしない。
- 実測：写実は目視で **Kling > Wan2.7 > Seedance**、厳密リップシンクは **Wan2.7 > Seedance > Kling**（[[reference_higgsfield_lipsync_pipeline_reality]]）。

### 6-4. 実適用ログ
- **案内人 中盤②（5s）**：5s尺に約5sの音声ゆえ、発話前の長い所作（"looks up slowly … then speaks"）は**末尾セリフ途切れ**＋KF（既にカメラ目線）と矛盾。→「既にカメラ目線・ほぼ即発話」に修正で両方解消。
- **ED-1（#127）**：本対策の未適用版だが**採用確定＝再生成しない**（2026-06-18 オーナー指示・cr節約）。**次カット（ED-3等）から §5/§6 を必須適用。**
- **ED-3（#129）成功・採用（2026-06-18・job `7fb98d89`・36cr）＝てかり解消の成功レシピ**：**画像1枚（role=`image`＝マットな顔close-up）＋`audio`、start_imageなし、CRAFT最小肯定（肌語/照明語/否定語ゼロ）＋映画用語カメラ（`Tracking shot`／`Truck right`／`locked static`／`slow push-in`）＋膝上framingを明示**。結果＝**マット・写実（ED-1の脂光沢から大幅改善）**。→ §6-2/§6-5 が実証された。
  - ★**2画像差しは失敗**：start_image＋image＋audio の同時投入は**failed＋返金**（job `3558c87b`）。**Seedance口パクは画像1枚に限定。**
  - ★**image＝人物(顔)参照であり構図は決めない**：start_imageなし・@Image1が顔close-upでも**動画は顔close-upから始まらず**、プロンプトのFramingどおり（歩く脚→人物へ）構図が出た＝実証。**start_image＝開始フレーム/動きの役割**で別物。
  - ★**start_imageを使わないなら、ショット種別（膝上/全身）と動きをプロンプトで明示補強**しないと構図がモデル任せになる。
  - ★**🔴語り部口パクは内蔵音（audioに渡したクローン声がリップシンクされた音）をそのまま使う＝generate_audio=falseにしない・内蔵音を捨ててマスター音声を上乗せしない**。口は内蔵音にしか同期せず、後当ては原理的にロックしない（第2弾ヘルクラでcr157.5浪費して確定・一定オフセット/線形/非線形リタイムすべて無効）。固有語誤読は“モデル自前TTS生成”時のみで、クローン声をaudio入力で渡す本方式では起きない（[[reference_higgsfield_lipsync_pipeline_reality]]）。

### 6-5. ★素体（マットKF）→ image-to-video 口パクは「最小肯定＋保存指示」が既定（語り部6本実証・2026-06-18）
記事（§2④）は品質語・120〜280語を勧めるが、それは**一般カット/ゼロから構築する場合**。**良い素体（マットでてかり無しの soul_2 KF）から口パク動画を作るときは逆で、descriptorを盛るほどSeedanceが肌を再レンダリングして“てかる”。**
- **てかりは素体継承ではなくSeedanceが生成時に後付け＝プロンプトが主因**（素体6本比較：素体はマット確認済／盛った版ほどてかり強・最小版はマット）。[[feedback_ai_human_realism_no_sheen_bar]]
- **やること**：①狙いを一言＋欲しい状態を肯定で簡潔に ②**保存指示**＝`Keep the lighting, skin tone and matte finish of @Image1. Do not relight the face.` ③**足さない**＝肌写実語（skin texture/pores/dry skin/matte skin）・否定語（no specular/matte/NOT〜）・品質語の山積み。[[feedback_simple_positive_prompt_no_negatives]]
- **実証済み安全構成＝#4（マット素体＋最小肯定プロンプト）**。この場合は120〜280語に満たなくてよい（最小でよい）。
- 残るてかりは§5生成後ゲート＋ポスト roll-off で処理。
> スコープ整理：**構成・語順・@タグ・カメラ用語・タイムラインの枠組み＝§1〜§4（SeaArt記事が正）／素体口パクの写実・てかり対策のdescriptor＝本§6-5（社内実証が正）。**

### 6-6. 確定プロンプトは未検証で安易に変えない（cr浪費防止）
一度「出力を実機確認して採用」したプロンプトは、思いつきで書き換えない。**変更するときは理由と差分を明示し、承認を得てから**。未検証の変更は再生成crを浪費し、使えていた構成をボツ化しやすい（[[feedback_dont_change_confirmed_prompt_lightly]] [[feedback_iterate_generations_on_different_scenes]]）。検証は「同一シーンの再生成」でなく**違うシーン**で回す（使える素材を潰さない）。
