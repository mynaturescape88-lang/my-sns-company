# Seedance 2.0 を「使うとき」のプロンプト作法（モデル固有）

> ⚠️ **これは Seedance 2.0 という特定モデル固有の作法**。動画生成一般のコツではない。
> **絶対に他モデル（Wan2.7 / Kling / Veo 等）へそのまま流用しない。**
> モデルごとにプロンプトの読み方は違う。ここを「動画生成共通ルール」と誤って一般化すると、
> 別モデルでも同じ書き方を当てて同じ失敗を繰り返す（逆に Wan2.7 の書き方を Seedance に当てても失敗する＝実証済み・下記）。
> **新モデルを使うときは、必ずそのモデルの公式/実証プロンプト例を読んでから作法を別途確立する。**

出典＝SeaArt の Seedance 2.0 プロンプトガイド（実証プロンプト例つき）。本ファイルは要点を自分の言葉で整理＋アンカー1例のみ引用。

---

## 0. この作法を作った経緯（失敗の教訓）

人智の外側・案内人の口パクで Seedance を使った際、**ガイド未読のままWan2.7流の“品質語てんこ盛り”プロンプトを当てて失敗**した。
モデルが変われば作法が変わる、という当たり前を外したのが根因。教訓＝**モデル固有の作法を、そのモデルのreferenceで持つ**。

---

## 1. Seedance がプロンプトをどう読むか（モデルの性質）

- **語順が効く**：`被写体 → 動作 → カメラワーク → スタイル/照明` の順で書く。
- **語数 120〜280語**が安定帯。短すぎ＝不安定／長すぎ＝重要情報がぼやける。
- **@参照タグ駆動**のモデル：
  | タグ | 役割 | 使いどき |
  |---|---|---|
  | `@Image1` | 顔・衣装・見た目を固定 | 複数カットで同一キャラを維持。`Maintain facial features and wardrobe fully consistent with @Image1` と明記 |
  | `@Audio1` | 口パク同期・編集ビート合わせ | 喋らせるカット。決まり文句＝`Lip-sync aligns with @Audio1.` |
  | `@Video1` | 動き・カメラワークを継承 | 特定の動きパターンを再現したいとき |
  - 複合使用時は**参照素材の合計尺を15秒以内**に。
  - 参照画像は**上半身〜中距離のシンプルなポートレート**が安定しやすい。
- **肯定形で書く**（否定形は弱い）：
  - ❌ 「手が崩れない」→ ✅ `Detailed hands / Anatomically correct`
  - ❌ 「不自然に動かない」→ ✅ `Physically accurate / Natural motion`
  - ❌ 「顔が変わらない」→ ✅ `Maintain ... consistent with @Image1`
- **タイムライン記法（`0-4s:` `4-9s:`）は使う場面が限定**：複数ショット／15秒以上／明確な場面転換があるときのみ。
  **単発・単一構図のカットでは使わない**（被写体→動作→カメラ→スタイルの5要素で十分）。

---

## 2. 写実・自然さの出し方（Seedance流）／ 避ける表現

### ✅ 効く（写実は「物理・解剖」の語で出す）
- 物理シミュレーション系：`realistic physical simulation` / `natural fabric inertia` / `light diffusion` / `realistic rain physics`
- 解剖学的自然さ：`anatomically correct` / `detailed hands` / `organic movement` / `natural, measured gestures`
- カメラ：`slow push in` / `gentle push-in` / `tracking shot` / `rack focus` / `pan / whip pan` / `360 orbit` / `POV`
- 照明・色：`soft cinematic lighting` / `three-point lighting` / `film grain` / `teal-orange color grading`

### ❌ 避ける（＝AI光沢・過剰処理の原因）
- **画質誇張語の重ねがけ**：`masterpiece` / `4K ultra HD` / `ultra sharp` / `ultra mega ...` の多重盛り。
- 曖昧形容の連発（`beautiful` だけ繰り返す等）。
- 技術用語の無理な詰め込み。

> 🔑 **混同注意**：ガイドが推奨する `physically accurate / detailed hands / natural motion` は「**解剖学的な自然さ**」の語であって、
> `masterpiece / 4K / ultra sharp` のような「**画質hype**」ではない。過去の失敗（試行3）はこの2つを混同し、hype語を冒頭に山積みしてAI光沢を招いた。

---

## 3. アンカー・テンプレート（実証済みプロスピーカー例＝求める水準）

このモデルで“狙いの写実”が出ている実証例。**書き出しは被写体から・品質hype語ゼロ・自然な散文・約70語**という素直さが核。新規プロンプトはこの骨格に被写体設定だけ差し替えて作る。

> A male speaker in his early 30s, wearing a navy suit with a clean short hairstyle, projects confidence and approachability. Frontal medium shot inside a modern glass-wall office, with a softly blurred urban skyline in the background. A gentle push-in camera move. He pauses briefly, takes a soft breath, and begins to speak: "AI is redefining the way we live." Lip-sync aligns with @Audio1. His hands accompany the speech with natural, measured gestures; he occasionally nods or offers a slight smile. Consistent eye contact with the camera throughout.

骨格の読み解き：
1. **被写体（人物＋装い＋印象）から始める** … `A male speaker ... projects confidence and approachability.`
2. **ショット＋舞台** … `Frontal medium shot inside a modern glass-wall office ...`
3. **カメラ** … `A gentle push-in camera move.`
4. **動作→発話→口パクタグ** … `pauses, soft breath, begins to speak: "..." Lip-sync aligns with @Audio1.`
5. **ジェスチャー・視線（肯定形・自然語）** … `natural, measured gestures; occasionally nods ... Consistent eye contact ...`

ガイドにはこの他、映画予告編／デジタルアバター（ホスト・歴史家）／ダンス／アクション／商品プロモ／日常Vlog 等のシーン別実証例があり、**いずれも同じ素直な骨格**で書かれている＝この水準は Seedance で実現可能。

---

## 4. ✅ プロンプト作成「後」チェックリスト（Seedance専用）

プロンプトを書いたら投入前に必ず通す。1つでも❌があれば直す。

```
□ 書き出しは被写体（人物）からか？ 品質語/hype語で始めていないか
□ masterpiece / 4K / ultra sharp / ultra HD 等の画質誇張語を入れていないか（＝AI光沢の主因）
□ 語順は 被写体→動作→カメラ→スタイル/照明 になっているか
□ 120〜280語に収まっているか（短すぎ=不安定／長すぎ=ぼやける）
□ 動作指示を否定形（avoid〜 / not〜）で書いていないか → 肯定形へ
□ @Image1 で顔・衣装の一貫性を明記したか
□ 口パクは "Lip-sync aligns with @Audio1." を入れたか（喋らせるカットのみ）
□ タイムライン記法（0-4s等）は「複数ショット/15s+/場面転換」のときだけか。単発で過剰に刻んでいないか
□ カメラ/照明/モーションは具体的な実写用語か（slow push in, three-point, measured gestures 等）
□ 写実は「物理・解剖」の語（physically accurate, natural fabric inertia 等）で出しているか。hypeで出していないか
□ 参照素材（@Image/@Audio/@Video）の合計尺は15秒以内か
□ ＜最重要＞これはSeedanceのプロンプトか？ 他モデルの作法を混ぜていないか
```

---

## ★ 実証サンプルから読み取る 指定→結果の因果（喋るホスト系）

> 記事の「人物が正面でカメラに喋る」実証3例（プロスピーカー／商品ホスト／歴史家）を観察し、どの指定がどの結果を生んだかを要約（英語プロンプトは逐語転載せず自分の言葉で再構成）。
> 共通の発見＝**写実は画質hype語ではなく「実在のライティング・自然さの指示・@Audioリップシンク・1つのゆっくりした push-in」で出ている**。3例とも盛り語ゼロ・素直な散文。

| 実証例 | 主な指定（要約） | 出ている結果 | 我々への教訓 |
|---|---|---|---|
| プロスピーカー | 30代前半・現代オフィス・gentle push-in 1つ・soft breath→発話・@Audio・measured gestures・うなずき・一貫アイコンタクト | 自然で信頼感ある話者／肌がツルつかず所作が機械的でない | 写実は素直な情景＋自然な所作で出る。被写体始まり・約70語の素直さが骨格 |
| 商品ホスト | 商品を手に・カメラ目線・restrained gestures・@Audio・中距離・push-in | 発話と小道具提示が両立／手の動きが過剰でない | restrained/measured と抑制を明記すると所作が暴れない。手元小道具（古地図）があっても安定 |
| 歴史家 ⭐我々の鋳型 | 暗い書庫・ろうそく暖色実用光・伏目→視線上げ・film grainは“雰囲気のため”・@Audio・very slow push-in 1つ | 暗所でも写実維持／視線上げが語りかけの説得力／粒子が安っぽさでなく空気感 | 暗室＋暖色実用光＋視線上げ＋film grain（atmospheric/filmic mood のため）＋@Audio1＋単一push-in をそのまま移植 |

### 喋るホスト系の写実レシピ（共通ルール）
1. realismの源は4つ＝(a)実在のライティング記述（three-point／暖色実用光／soft cinematic）(b)自然さの指示（measured/restrained gestures・visible breathing・consistent eye contact・所作が機械的/ループにならない）(c)@Audio1リップシンク (d)1つのゆっくりした push-in。**画質hype語は登場しない**。
2. 盛り語（masterpiece/4K/ultra HD/ultra sharp/realistic skin texture）はむしろAI光沢の主因。
3. 同一性は@Image1に委ね、顔・髪・年齢・衣装をテキスト再記述しない。
4. 「機械的・ループな動きにしない」意図は実証サンプルでも示されるが、表現は§2に従い**肯定形優先**（例：natural, varied, organic movement）。
5. 喋るホスト系の鋳型＝歴史家サンプル。

### 実適用ログ：案内人 中盤②（5s・@Image1=kf_mid2＋@Audio1=hero_g3_question）
- 試行3の盛り語（冒頭6語＋末尾の二重盛り）を全廃→歴史家鋳型へ再設計。
- ★**PM検出の重要点**＝5s尺に約5sの音声ゆえ、発話前の長い所作（"looks up slowly … He then speaks"）は**末尾セリフ途切れ**を招く＋kf_mid2（既にカメラ目線）と**矛盾**。→「既にカメラ目線・ほぼ即発話」に修正で両方解消。
- workingなセリフ/`@Audio1`同期/タイミングは据え置き、同一性は@Image1委譲。

---

## 5. モデルを跨ぐときの鉄則（再掲・最重要）

- **Seedanceの作法を Wan2.7 / Kling / Veo にコピペしない。** 各モデルは別の読み方をする。
- 口パクの厳密さ・写実レンダリングはモデルで挙動が違う（実測：写実は目視で Kling > Wan2.7 > Seedance、厳密リップシンクは Wan2.7 > Seedance > Kling＝[[reference_higgsfield_lipsync_pipeline_reality]]）。
- **新モデル採用時は、そのモデルの実証プロンプト例を読んでから本ファイルと同じ粒度で作法を作る。** 「動画生成一般のコツ」として一般化しない。
