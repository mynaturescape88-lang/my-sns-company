---
name: video-editing
description: >
  動画の編集・制作を、蓄積したノウハウに沿って自動で進めるスキル。「動画を編集して」「動画を作って」「長尺を組んで」「カットを並べて」「テロップを入れて」「色補正して」「BGM・効果音を付けて」「DaVinciで編集」等と言われたら発動する。冒頭フック設計・カット密度・カメラ語彙・AI生成カットの質感合わせ・字幕同期・DaVinci Resolve連携までを内包し、ノウハウを毎回読み込まずに発動だけで反映する。
---

# 動画編集スキル v1.0

各役割（主にコンテンツ制作・SE）が「動画編集／動画制作」を求められたとき発動する。
**目的**：CLAUDE.md に知識を書いて毎回読む方式をやめ、このスキル発動だけで動画ノウハウを反映する。

## いつ発動するか

以下のいずれかに該当したら、作業に入る前に本スキルを発動する。

- 「動画を編集／制作／作成して」「長尺を組んで」「ショートを作って」
- 「カットを並べる／差し替える」「テロップ・字幕を入れる」「色補正・カラcoグレ」
- 「BGM・効果音を付ける」「書き出し・MP4出力」「DaVinciで〜」
- @mynaturescape の動画工程に着手するとき
- ※**「人智の外側」（@ijin-hiroku・旧称AIインサイト／古代ミステリー×AI長尺）は別手法＝専用スキル `jinchi-longform` を発動する**（本スキルでは扱わない）

> 発動したら、まず下の「チャンネル分岐」で工程系統を確定し、該当 reference を読んでから着手する。

---

## チャンネル分岐（最初に確定する）

| 系統 | 駆動 | 工程の核 | 主に読む reference |
|---|---|---|---|
| **人智の外側**（古代ミステリー×AI・長尺／旧称AIインサイト） | **台本駆動** | → **本スキルでなく専用スキル `jinchi-longform` を発動**（題材選び→台本→章別素材台帳→制作→非公開アップの一気通貫）。@mynaturescapeとは制作手法が別物 | （`jinchi-longform` スキル） |
| **@mynaturescape**（植物・実写長尺/イベント） | **実写駆動・1発生成** | 素材フォルダ＋名称＋色 → `mynaturescape_longform.py` が score→filter-people→assemble を自走（可変尺/散らし/フック/4K/連続BGM/エンドロール） | `references/mynaturescape-longform.md` |
| @mynaturescape（手作業の高品質編集が要るとき） | 実写駆動 | 撮影クリップを品質スコアリング→並び替え→色補正→BGM | `references/davinci-resolve.md`（スコアリング節） |
| 共通（全動画） | — | 冒頭フック5段・オリジナル化・カット密度・植物は静止＋カメラのみ | `references/editing-principles.md` |

---

## 標準ワークフロー

1. **チャンネル分岐を確定**（上表）し、該当 reference を読む。
2. **editing-principles.md を必ず確認**（冒頭10秒フック5段・AI放置量産の回避・カット密度・植物演出）。
3. 工程を実行：
   - 台本駆動（人智の外側）→ **`jinchi-longform` スキルへ**（本スキルでは扱わない・手法が別）。
   - 実写駆動（@mynaturescape長尺/イベント）→ **`mynaturescape-longform.md` の3ステップ（score→filter-people→assemble）を実行**＝1発生成。手作業の作り込みが要るときのみ `davinci-resolve.md`。
4. **AI生成カットはフリー素材に質感を合わせる**（documentary調・彩度/グレイン・並べて差を点検）。
5. 完成後、PM レビュー基準（editing-principles.md のチェックリスト）でセルフチェックしてから提示する。

---

## DaVinci Resolve 連携（高品質編集の本命）

- 接続手順・Python API・タイムライン構築・トランジション・カラーLUT・音声ダッキング・書き出し・**クリップ品質スコアリング**は `references/davinci-resolve.md` に集約。
- ffmpeg で十分な定型工程（AIインサイトの章組み・字幕焼き込み）は `longform-pipeline.md` の確立済スクリプトを使う。DaVinci は「トランジション/エフェクト/色/音の作り込みが要る実写編集」で使う。

---

## 絶対原則（全工程共通）

- **AI生成文・素材の放置量産はしない**：実体験・自社撮影・植物知見を必ず注入（inauthentic判定・BANリスク回避）。
- **生成クレジットは要承認・最安検証から**：新方式/新題材をいきなり高解像度・長尺で回さない（[[feedback_validate_method_at_min_cost_before_scaling]] / [[feedback_higgsfield_credit_conservation]]）。
- **植物は静止・カメラだけ動かす**（breathing/orbit語禁止・[[feedback_plant_video_direction]]）。
- **AI開示**：合成音声/AI生成ビジュアルを使った動画はアップロード時に合成コンテンツ開示をON（AIインサイトは `containsSyntheticMedia:True` を毎回）。
- **版権・肖像/パブリシティNG**：芸能人・版権キャラ・他者映像の転用は採用不可。型のみ借りる。
- **AI生成プロンプトの作法はモデル固有**：Seedance/Wan2.7/Kling 等は各々プロンプトの読み方が違う。あるモデルで効いた書き方を別モデルに流用しない／「動画生成一般のコツ」と一般化しない。モデル別 reference（例：`references/seedance-2.0-prompting.md`）を使い、新モデルは公式/実証例を読んで作法を別途確立する。
- **公開GOはオーナーのみ**。スキルから生成・公開を催促しない。

---

## reference 索引

| ファイル | 中身 | 読むとき |
|---|---|---|
| `references/editing-principles.md` | 冒頭フック5段・オリジナル化ルール・カット密度・植物演出・高速パン回避・カット尺の抑揚・PMレビュー基準 | **毎回** |
| `references/mynaturescape-longform.md` | @mynaturescape長尺=1発生成（score→filter-people→assemble の3ステップ・確定仕様・パラメータ） | @mynaturescapeの長尺/イベント動画を作るとき |
| `references/longform-pipeline.md` | （旧）台本駆動長尺の方法論。**正本は `jinchi-longform` スキルへ移行**＝こちらは参照用に残置 | 原則 `jinchi-longform` を見る |
| `references/seedance-2.0-prompting.md` | **Seedance 2.0 固有**のプロンプト作法（正本＝SeaArt記事）＝3基本構造/語順/@タグ/肯定形・品質語(masterpiece等は写実向上に使う)/カメラ用語/チェックリスト＋社内実証(失敗の真因=照明矛盾・老化描写)。**他モデルへ流用禁止** | Seedance 2.0 で生成するとき |
| `references/davinci-resolve.md` | DaVinci Resolve Python API接続・タイムライン/色/音・クリップ品質スコアリング | 実写編集・DaVinci使用時 |
| `references/ai-video-pipeline.md` | AI動画制作の調査ノウハウ（ブランディング・台本モデリング・モデル選択・素材調達） | 企画/モデル選定/コスト見積り時 |

## プロジェクト固有データの参照先（スキル本体には埋めない）

このスキルは汎用に保ち、プロジェクト固有の値は発動時に下記から読む。

- 人智の外側（旧称AIインサイト）運用手順（チャンネルID・スクリプトパス・トークン名・コスト早見）：
  `.company/secretary/task_logs/ainsight-longform-production-RUNBOOK.md`（※制作手法の正本は `jinchi-longform` スキル）
- RKJ調査由来の最新カット術・密度目標：`.company/secretary/notes/2026-06-16-rkj-p3-insights-master-list.md`
- アカウントの正体・コンセプト：`.company/sns_accounts/`

---

## ◆成果物／・作業ノードの分類（当面 子レビューskillは付けない・親注記のみ）
- 本スキルの工程（冒頭フック・カット並べ・テロップ・色補正・BGM・書き出し）には成果物レビューが要る側面があるが、**当面 `fb/video.md` が現役**で観点を担保している。子レビューskillは追加しない（成果物レビューが要る工程が固有化したら後日 `video-review-*` を追加する）。
- **「人智の外側」（@ijin-hiroku・古代ミステリー×AI長尺）は別系統＝`jinchi-longform` とその子 `jinchi-review-*`** が制作とレビューの正本（実写＝本スキルとは制作手法が別物・混同しない）。本スキルが扱うのは @mynaturescape の実写長尺/イベント。
- ・素材取得・MP4書き出し等は純作業ノード＝レビューskillを当てない。
- 提出前は editing-principles.md のチェックリスト＋`fb/video.md`＋`pm/review-baseline.md` でセルフチェックする。
