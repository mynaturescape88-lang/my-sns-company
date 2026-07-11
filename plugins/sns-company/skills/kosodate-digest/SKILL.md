---
name: kosodate-digest
description: >
  子育てYT「スカイツリーより大きくなるぞ」(@myraisingdiary系・UCAVCeLCuHYgEX1hThXRMsGA)向けの縦9:16総集編（digest）長尺を、Day範囲を指定するだけで決定的に再現制作→非公開アップまで進めるスキル。「子育て総集編を作って」「スカイツリーの総集編」「Day◯◯〜◯◯の総集編」「子育てdigest」「子供YTの新長尺」等で発動。@mynaturescapeの【N日目】ショートを日めくり連結する0cr（AI生成なし・ffmpeg/PILのみ）長尺で、FLF2Vループ動画とも@mynaturescapeの実写長尺（video-editing）とも別物。制作フロー・固定仕様・BGM可聴化/終了画面/縦横比確認のFB定石・内部レビューゲート・非公開アップ・アップ後の旧版削除までを内包する。
---

# 子育てYT総集編（digest）制作スキル v1.0

チャンネル＝**スカイツリーより大きくなるぞ**（`UCAVCeLCuHYgEX1hThXRMsGA`）向けの、**縦9:16総集編長尺**。
@mynaturescapeの「**【N日目】◯◯**」ショートを、指定Day範囲で**日めくり連結**した0cr動画（AI生成なし＝ffmpeg/PILのみ）。制作→内部レビューゲート→**非公開アップ**まで。公開はオーナー。

> ⚠️ 混同しない：
> - **FLF2Vループ動画**（子育てループ動画・区間ごとAI生成）とは**別物**。
> - **@mynaturescapeの実写長尺**（`video-editing`スキル＝score→filter-people→assemble）とも**別物**。
> - 本スキルは「@mynaturescapeの【N日目】ショートを縦9:16でクロスフェード連結する総集編」専用。

## いつ発動するか
- 「子育て総集編を作って」「スカイツリーの総集編」「子供YTの新長尺（総集編）」
- 「Day◯◯〜◯◯の総集編」「子育てdigest」
- スカイツリーchの総集編長尺（制作〜非公開アップ）に着手するとき

## 発動＝適用（必ず行う）
1. 下の **制作フロー** を順に回す（飛ばさない）。ロジックの正本は `scripts/kosodate_digest/`＝**複製せずスクリプトを実行/参照する**。
2. **固定仕様**（§固定仕様）と**FB定石**（§FB定石＝BGM可聴化・終了画面・縦横比確認）を必ず適用する。
3. **非公開アップの前に内部レビューゲート（fail-closed・前回完成品と実物比較）を必ず通す**。全pass以外はアップしない。
4. **有料生成は無い**（0cr）。ただし**非公開アップ・旧版削除は不可逆**＝アップはGO前提の内部レビュー通過後・削除は§後始末のガードとオーナーGOを守る。

## 入力
- **Day範囲**（例 Day28-100）。これだけが必須入力。
- 尺目標＝**15〜20分**（`solve_trims.py --target-aim` 既定16.3分）。日数から1日平均秒を自動配分。

## 素材の在り処（「@mynaturescape=植物専用」と思い込まない）
- 元動画＝**@mynaturescapeのYouTubeショート**。台帳 `seo_growth/youtube-kids-migration-ledger.md` に `day→videoId→タイトル`（Day0-374）が全行ある。→ [[reference_kosodate_day_clips_source_mynaturescape]]
- 指定Day範囲の**欠番ゼロを台帳で検算**してから着手。
- DL＝`yt-dlp --cookies-from-browser chrome --extractor-args youtubejsi:runtime=deno`（1080p・**縦1920素材の除外バグに注意**＝全日DLできたか本数を検算）。参考実装 `seo_growth/raising_yt_predownload.py`。
- Cloudinary既存があれば `seo_growth/raising_yt_state.json` の `cloud_url` を使ってよい。無い分だけ台帳のvideoIdからDL。

## 制作フロー（scriptsが正本・ロジックは複製しない）

### 0. セットアップ（README注意＝scratchpadパスがハードコード）
`scripts/kosodate_digest/README.md`の通り、参照実装はscratchpadパス埋め込み。**実行前に実パスへ差し替える**：
- 元動画の置き場 `SRC`（今回のDL先ディレクトリ／`day28.mp4`〜のように連番）。
- BGM 3曲のmp3パス（§固定仕様③）。
- フォント：ヒラギノを作業ttcへコピー → `scripts/kosodate_digest/work/` に `jp.ttc`(角ゴW3) `latin.ttc`(Helvetica) `telop_w5.ttc`(Sans W5) `telop_w6.ttc`(Sans W6)。

### 1. 競合BGM検出 → `music_scan.py`
`music_scan.py --src <SRC> --start <N> --end <M> --out-json mute.json`
各dayの元音に「音楽入り（総集編BGMと競合する持続音）」があるDayを検出。判定＝持続率≥0.85 かつ RMS≥-30dB。競合Dayだけ後段で元音ミュート。

### 2. トリム窓＋テロップconfig生成 → `solve_trims.py`
`solve_trims.py --src <SRC> --start <N> --end <M> --mute-json mute.json --out config.json`
- タイトル＝台帳の「【N日目】◯◯」から`【N日目】`を除いた説明部分（絵文字は角ゴにグリフ無し＝自動除去）。
- 「残す秒数」＝カテゴリ別（highlight/normal/quiet/finale）×全体スケールで**15-20分に自動収束**。声/見どころ（笑い・泣き・くしゃみ・喃語・うなり等）日は満尺寄り、静かな長い日から削る。
- 区間開始＝音量エンベロープのエネルギー最大窓（静かな余白を捨てる／quiet日は中央寄せ）。
- 競合Dayは `src_vol:0.0`（元音ミュート）を自動付与。
- **冒頭演出（intro要素）は入れない**（§固定仕様）。

### 3. ビルド → `build_full_28_100.py`（`build_digest.py`のランナー）
Day範囲用に**ランナーの定数を書き替えて実行**（`build_digest.py`本体は触らない）：
- `CONFIG` = 手順2の `config.json`、`BASE`/`OUT`/`WORK` = 実パス。
- `BGM_SEGMENTS` = 3曲のファイルとDay範囲（§固定仕様③・Day範囲を3等分）。
- **`BED_LUFS = -26.0`**（★FB定石＝§FB定石①。ランナー既定が`-26`＝そのまま。`-34`は「聞こえない」不採用の旧値）。`VOICE_TGT = -14.0`。
- `tail_fade = 0.8`（末尾0.8sフェードアウト）。
- `reuse=True`＝有効セグメントは再エンコせず、**音声だけ再生成**できる（音量手直し等の最小手数）。
本体＝1クリップ正規化→トリム→左上テロップ焼き込み→クロスフェード連結→音声ミックス（元音＋BGM）→上下黒帯150px post-pass。

### 4. 終了画面（15秒黒画面）を連結 → §FB定石②
`build_full_28_100.py`の出力（0.8sフェードアウト済み本体）に、**15秒の純黒（無音）**を連結して締める。
- 黒素材＝`color=black:s=1080x1920:r=30` ＋ `anullsrc`（無音）。**本体と一致するコーデック/画素形式/SAR/AACパラメータ**で作る。
- 連結＝`concat -c copy`（**本体を再エンコしない**）。横素材のレターボックス強行は**NG**（§FB定石②）。

### 5. QC → `qc_full_v2.py`（相当の実測）
- **全編デコードエラー0**（`ffmpeg -v error -i OUT -f null -`）。
- フレーム抽出目視：上下帯150px（上端row150/下端row1770境界）・左上テロップ1行（長題も1行）・ピンク→オレンジのグラデ下線・クロスフェード。
- 音量実測：元音＞BGM（元音相関≫BGM）・BGM境界の切替がDay切れ目一致・**統合ラウドネスが約-26 LUFS**（bed）／声が前に出ているか。
- **黒画面区間：meanLuma=0・音声≈-91dB（無音）を実測**。
- 総尺が15-20分に収まるか。

### 6. 内部レビューゲート（アップ前・fail-closed・オーナー委任）
オーナー指示（2026-07-10）＝「前回の完成品と比較して問題ないと内部で確認できたらアップまで進める」。
- **前回完成品と実物比較**：帯150px・左上テロップ1行＋グラデ下線・クロスフェード0.6s・元音＋BGMバランスが同等水準か。今回の固定仕様（冒頭演出なし／鼓動なし／BGM3分割／尺15-20分／BGM-26 LUFS／末尾15秒黒）が正しく反映されているかを**実フレーム/実測**で確認。
- **`review-check` スキルを実処理**（JSONの数値突合だけで「一致」と自己判定しない＝焼き込まれた実体をフレーム/音声で確認）。全pass以外はアップせず修正。逸脱は隠さず報告。

### 7. 非公開アップ → `seo_growth/upload_kosodate_digest.py`
`seo_growth/.venv/bin/python seo_growth/upload_kosodate_digest.py --video <OUT> --title "<案>" --desc-file <概要欄txt> --thumbnail <サムネ>`
- 認証＝`YOUTUBE_KIDS_UPLOAD_REFRESH_TOKEN`（スカイツリーch＋`youtube.force-ssl`書込スコープ）。**ch自己ガード**あり（認証chがスカイツリーでなければ中止）。→ [[reference_youtube_refresh_token_bound_to_channel_and_scope]]
- `privacyStatus=private` 固定（公開はオーナー）。まず `--dry-run` で投げる内容を確認。
- 概要欄＝前回テンプレ（`.company/creator/work/kosodate/skytree-newborn-day0-27-digest-youtube-description.md`）を当該Dayアークに書き換え。BGMは**YouTubeオーディオライブラリ曲（商用/収益化可）**＝概要欄にクレジット記載。
- サムネ＝指定イラスト（**16:9**・`thumbnails.set`は2MB上限＝超えるならJPEG化）。→ [[reference_youtube_thumbnail_2mb_limit]]
- **アップ後にAPI実測検証**：`videos().list`で privacyStatus=private・尺・title・description・サムネ設定を確認（read-after-writeラグに注意＝即listがNGならsleepリトライ）。→ [[reference_youtube_videos_update_lag_and_sort_collision]]

### 8. アップ後の後始末（作り直しで旧非公開版が出たら）
- **採用版を残し、旧版はオーナーGO後に `videos.delete` で削除**（削除は不可逆＝勝手にやらない）。
- 誤削除ガード＝**採用videoIdが削除リストに無いことをassert**＋削除後に **read-after-writeで旧IDが404（GONE）・採用IDが残存**をAPI実測。→ [[reference_youtube_content_replace_delete_reupload]]

### 9. 報告
非公開URLをオーナーへ提示（Chrome表示）。公開はオーナー実施。

## 固定仕様（前回と同じ共通仕様）
- **縦9:16（1080x1920）・H.264・yuv420p・+faststart・AACステレオ・30fps。**
- 上下**黒帯150px**（色グレードなし＝帯のみ）。
- 本編テロップ：**左上・白背景の帯枠**に「`Day N  <タイトル>`」を**1行**。下線が**ピンク→オレンジのグラデで左から伸びる**。**auto-fitでフォント縮小し必ず1行**（長題も2行にしない）。静止表示・映像のクロスフェードに同期して切替。
- トランジション：**クロスフェード0.6s**（日の変わり目）。
- 音：**元動画の音は全編ミュートしない**（声・盛り上がりは切らずにトリム）。競合BGMのある日だけ元音ミュート（`music_scan.py`判定）。BGMは元音を邪魔しない下地として通し敷き。
- **冒頭演出＝既定OFF。** 「黒＋鼓動＋2行テロップ→誕生クリップ」は**Day0（誕生）専用の任意演出**。通常のDay範囲は**先頭Dayの映像から直接開始**（鼓動なし・黒なし・冒頭テロップなし）。誕生を含む回だけオーナー指定でONにできる。
- **アウトロ鼓動なし。** 末尾は0.8sフェードアウト＋15秒黒画面で締める（§FB定石②）。
- **③BGM＝3曲・時間で3分割**：指定3曲を**Day範囲を3等分**して割当。各曲は区間尺より短ければ**区間内でループ**、境目は**Dayの切れ目**に合わせ**境界でクロスフェード**（曲が日の途中で切り替わらない）。
- サムネ＝指定イラスト（**16:9**）。

## ★FB定石（今回焼き込む肝）
### ① BGM音量＝可聴化（`bed_lufs`が制御点）
- **既定 `bed_lufs = -26 LUFS`**。声`-14`に対し**-12LU差＝下地として可聴化**。**-34は小さすぎ（聞こえない）と実証済み**。上げ余地は`-24`まで。`alimiter`でクリップ防止。
- 音量だけの手直しは**映像を再エンコせず**、全セグ`reuse=True`＋**音声のみ再生成→remux**が最小手数。

### ② 終了画面の作法
- 既定の無難な締め＝**15秒の純黒（無音・本体一致パラメータで`concat -c copy`）**。本体は0.8sフェードアウトで黒に落としてから連結。
- 終了画面**素材**を足す場合は、**必ず縦横比を成果物(9:16)と突き合わせる**。不整合（黒帯/被写体切れ）が出るなら、**着手前・不可逆アップ前にオーナー確認**（後出し禁止）。→ [[feedback_flag_aspect_mismatch_before_upload]]
- **横素材のレターボックス強行はNG**（今回却下事例）。

### ③ 縦横比不整合は着手前のブロッキング確認事項
- 追加する任意素材（終了画面・差し込み等）の縦横比が成果物と違うなら、勝手に採用せず**着手前に**明示・確認する。報告の後出し項目に添えるのは不合格。

## 正本参照（ロジックはここ・本スキルに複製しない）
| 参照 | 中身 |
|---|---|
| `scripts/kosodate_digest/`（build_digest.py / music_scan.py / solve_trims.py / build_full_28_100.py / qc_full_v2.py / README.md） | パイプライン本体・全ロジックの正本。BGM音量＝`bed_lufs` |
| `.company/creator/work/kosodate/skytree-day28-100-build-spec.md` | 確定仕様の詳細指示書（母体） |
| `seo_growth/youtube-kids-migration-ledger.md` | 素材台帳（Day0-374・videoId・タイトル） |
| `seo_growth/upload_kosodate_digest.py` | 非公開アップ（ch自己ガード・private固定・サムネ・publishAt対応） |

## 絶対原則
- **0cr（AI生成なし）**。ffmpeg/PILのみで作る。
- **非公開アップ・旧版削除は不可逆**＝アップは内部レビュー通過後、削除は§8のガード＋オーナーGO後のみ。
- **秘密情報をコード/チャットに出さない**。トークンは`.env.local`/`~/credentials/`。
- **前回完成品と実物比較**で仕様確定（古い企画/ポエムメモを復活させない）。→ [[feedback_reproduce_from_finished_artifact_not_old_plans]]
- 手順の正本は本スキル本文＋`scripts/kosodate_digest/`。経緯・日付・「今回こうした」はsessionlogが持つ（本スキルに書かない）。
