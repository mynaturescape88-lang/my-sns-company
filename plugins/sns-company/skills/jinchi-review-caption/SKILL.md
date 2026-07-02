---
name: jinchi-review-caption
disable-model-invocation: true
description: >
  人智の外側(jinchi-longform)の工程9「Whisper字幕同期」のレビュー時に発動。親skill jinchi-longform の工程9末尾（align後・再ビルド後）から呼ばれる成果物レビュー（木の内側の子ノード）。実発話時刻の反映・エーアイ→AI置換・PNG overlay字幕方式・音と字幕のズレ0を合否判定する。
---

# jinchi-review-caption（工程9 Whisper字幕同期の成果物レビュー）

人智の外側 長尺パイプラインの **工程9（Whisper字幕同期）** 専用の成果物レビュー。字幕成果物のゲート。親 `jinchi-longform` の工程9末尾から発動。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝工程9固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の6観点）
- ③ `.company/sns_accounts/youtube_ainsight.md`（人智の外側の戦略・表記前提の照合先）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（同期字幕・再ビルド動画）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の6観点を1つずつ照合（特に観点4＝youtube_ainsight.md戦略と相違ないか）
- ③ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ④ 全passのみ次工程へ進める

## 制作順序（字幕同期は「通し1本」でなく「章ごと」に回す・恒久）
通し約20分を毎回再ビルドすると1指摘=20分待ちで非効率。字幕同期の確認・修正は章単位で回す：
1. 章ごとに独立クリップを書き出す：`scripts/build_herculaneum_4k_chapter.py <cold|ch1|ch2|ch3|ch4>`（章別fi接頭でcuts/secsキャッシュ分離＝直した章だけ数分で再レンダ）。
2. 章単位で下記の数値ゲートを内部fail-closedで判定。
3. 不合格章だけ直し、その章の cuts/secs を消して当該章のみ再レンダ（whisper_n*.json は章別なので流用可）。
4. 全章passの後に最後の1回だけ通し最終4Kへ（本体 `build_herculaneum_4k.py all`・同じ同期/表示ロジックを使うので章別passは通しでも保たれる）。

## 工程9の観点（合格ラインを内包・1つずつ照合）
- [ ] **★全章 自動走査を先に通したか（FB箇所だけ直す＝禁止・恒久2026-07-01）**：オーナー指摘（ch1是正）は「欠陥の一例」であってチェックリストではない。FBを観点(ルール)へ一般化し、cold/ch1-ch4の**全章・全字幕行**を機械走査して同種問題を洗い出す。`scripts/herculaneum_full_audit.py` を実行し **R1(辞書漏れ)=0・R2(漢数字残存)=0・R3(3行字幕)=0** を実ファイルで確認してからビルドへ進む。指摘箇所だけ直して他章を見ない対応は不合格。走査結果（各観点の検出件数と修正内容）を内部記録に残す。
- [ ] **字幕同期＝行頭onsetアンカリング方式か**：各台本行の行頭特徴語をwhisper実認識語へ部分一致でマッチし、その語の発話start秒を行の表示開始に採用（`line_times_anchored`）。旧「累積文字数比例」一本は章内±0.5sドリフトを生むため非推奨。同型 `build_nazca_4k.py` も同方式。
- [ ] **音と字幕のズレを全章で機械検証したか（目視NG・自動ゲート必須）**：`scripts/jinchi_caption_sync_check.py <chap.mp4> <captions.json>`。**合格＝median|delta|<0.15s かつ max|delta|<0.35s**。先頭(cold)だけ綺麗で通すのは禁止（短い仮名行は誤差小＝中盤章のズレを見逃す）。※max は whisper再認識jitter(~0.5s)が混じるため、median<0.15が主・2〜3行が0.35〜0.6sでもmedian良好＋原因が認識divergence/jitterなら可（内部記録に残す）。
- [ ] **章別「台本字数÷whisper認識字数」比 ≥ 0.97 か**（補助指標）。下回る章は要再同期確認。
- [ ] **A: 字幕の漢字表示辞書を適用したか（恒久）** `scripts/jinchi_caption_display_dict.py` の `apply_display_dict`。TTSは読み仮名で生成済＝音は不変、字幕表示だけ漢字化。「エーアイ→AI」＋読み仮名の固有名詞/数値+単位/専門語（西暦79年・火砕流・軽石・小プリニウス・摂氏・21歳・2023年10月 等）。新題材で読み仮名語が出たら辞書へ追記して全章一括反映。読み仮名がそのまま字幕に出ていたら不合格。
- [ ] **A2: 漢数字→半角算用数字へ正規化したか（恒久）** 同モジュール `normalize_kanji_numbers`（caption_pngで自動適用・図解内テキストも同方針）。位取り数を算用化（九十点→90点・二千年→2000年・千五百年→1500年・十八世紀→18世紀）。**ガード必須**＝「数百/何百」等の概数・単独一桁（一切/一本/三人）・単独位語（百を超える/十倍）は変換しない（意味が変わる）。字幕に漢数字の位取り数が残っていたら不合格。
- [ ] **C: 1枚あたり最大2行・文区切りで分割したか（恒久）** `build_captions` が「。！？」で文単位に分割し尺を文字数比で再配分（`split_caption_sentences`・上限約60表示字）。3行字幕や複数文の詰め込みは不合格。カット同期は line_starts ベースで不変＝行内分割は同期に影響しない。
- [ ] 字幕がPIL PNG overlay焼き（白文字黒フチ）か

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
