---
name: sns-report
description: >
  SNS運用レポートの作成依頼時に発動。『レポートを見せて』『SNSレポート』『数値分析して』『今月の数字』等で発動。オンデマンドで前回HTMLをベースに数値を最新化（スクリプト一発）→レビュー全pass後に表示。フォーマットは固定。
---

# SNS運用レポート作成スキル v2.0（オンデマンド・決定的）

SNS運用レポートの作成・数値分析を依頼されたときに発動する。
**目的**：レポート手順を「誰がいつ実行しても同じ結果（決定的）かつ高速」にする。前回表示したHTMLをベースに、数値を最新化して表示する。毎回ゼロ生成しない・過去分は再集計しない・多段の往復をしない。

## 大原則（オーナー確定 2026-07-15）

- **オンデマンド**：レポートは依頼された時にその場で作る（自動集計前提は廃止）。
- **フォーマットは固定＝変更一切NG**：HTMLの構造・レイアウト・列/行・カード構成・項目セットを変えない。最新化するのは**セルの値**だけ。
- **前回HTMLがベース**：`seo_growth/latest_report.txt` が指す前回表示HTMLを土台に最新化する。数値は差分だけ動かす＝速い。
- **決定性の担保**：入力（前回HTML）→処理（該当セルのみ更新→合計再計算）→出力（固定フォーマット）を一意に定義。数値最新化は**スクリプト一発**（下記B）。
- **取得不可セルは埋めない**：未集計・API取得不可のセルは無理に埋めず現状維持（"-"）でよい。
- **性能契約＝依頼→Chrome表示まで≦60秒**（目標）。**1分以内に出せない場合はプロセスが誤り**（多段往復・逐次取得・ゼロ再生成が混入している）。取得が遅いPF（特に楽天Playwright）は**タイムアウトしてスキップし、そのセルは現状維持でHTMLを出す**（「取得できないセルは埋めない」を適用＝1PFの遅延で全体を止めない）。

---

## 決定的フロー（A→B→C→D・この順で固定）

### A. 入力を一意に確定（前回HTML＝ベース）
- `seo_growth/latest_report.txt` を読む＝ベースにする前回表示HTMLの絶対パスを一意に特定（無ければ `seo_growth/sns_monthly_report_*.html` の最新mtime）。
- そのHTMLを開き、どのPF・どの項目が埋まっているか（空白セル／既存値）を確認する。

### B. 数値を一発で最新化（作業ノード・往復なし・並列取得）
- **これ1コマンドで決定的に完了**（単一オーケストレーション）：
  ```
  .venv/bin/python seo_growth/render_report_preserving_strategy.py --no-open
  ```
  interpreter は**リポジトリ直下の `.venv`**（google-analytics-data 等入り）。`seo_growth/.venv` はyt-dlp用でGA4が動かないので使わない。
- このスクリプトが決定的に行うこと：
  1. 全SNSの実数値をライブ取得（`collect()`＝youtube/ig/ga4/adsense/search_console/rakuten/note の各report）。**各PF取得は互いに独立＝並列に走らせ、各取得に上限タイムアウトを付す**。**楽天Playwright（最重・約30秒）がタイムアウトしたらスキップ**しそのセルは現状維持でHTMLを出す（1PFの遅延で全体を止めない）。
  2. 当週セルを `report_history.json` に記録（`record()`）＋取得可能な過去週の空セルのみ遡及充填（`backfill()`）。**過去分の再集計はしない**（`calendarize` は呼ばない＝既知の巻き戻しバグを回避）。**フロー数値の週窓＝暦週(月〜日)の直近完了週＝`wk_done=monday_of(today)-7`**（`record()`と一致・ローリング7日/API遅延バッファ禁止＝当週混入や週合算二重計上を招く）[[reference_report_flow_window_must_match_wk_done]]。
  3. 数値表・合計/小計/週合計/MTD を hist から再計算（決定論）。
  4. 出力HTMLの絶対パスを標準出力に印字（`integrated_out_path`＝同一月構成なら同一パスに上書き）。
- **月次コスト（固定費）・収支（KGI対比）は触らない**：コード内固定値。オーナーからの申告があった時のみ `monthly_report_build.py` の `MONTHLY_COSTS` を修正する（毎回自動で触らない）。
- 検証だけしたい時（APIを叩かず現状 hist から温存レンダ）：`--render-only --no-open`。

> ✅ **並列取得は実装済み（2026-07-15）**：`monthly_report_build.collect()` は各PF取得を `ThreadPoolExecutor` で並列起動し、各取得にタイムアウト（既定50s／楽天Playwrightは40s）を付す。超過取得は `"__ERR__"` を返し `parse_*` が全項目None→`record()` がskip＝該当セルは現状維持（1PFの遅延で全体を止めない）。`youtube_report.py` の2回叩き重複も解消済（1回の結果を本体MAINE／人智の外側で共有）。**実測 54.8s→14.6s ＝ ≦60秒契約を安定達成**（読み取りAPIのみ・0cr・冪等・描画バイト不変）。

### C. レビュー（fail-closed・全pass以外は表示しない）
成果物ノードごとに正規レビューskillを1巡当てる。全passでなければ修正してから再度当てる。
1. 数値分析（アナリスト成果物）→ `report-review-analysis`（数値整合・実行順序）
2. 全pass後・表示前 → `pm/review-baseline.md`（Layer1てっぺん総合）を1回
- **提出前ゲート**：全記載を実API/スクリプトの実値・各台帳で裏取り（古いスナップショット/静的memoに依存しない）。全項目の裏が取れるまで「完了」と言わない。

### D. 表示＋保存（今回結果が次回ベース）
- Chrome表示：`open -a "Google Chrome" "<Bで印字された出力HTMLの絶対パス>"`
- **保存＝次回ベースの更新**：`seo_growth/latest_report.txt` を今回の出力HTMLの絶対パスに更新する。これで今回の（数値最新化済み）HTMLが次回のベースになる。
- 秘書はHTMLを表示するだけ＝チャットで数値の長文要約・表の再掲はしない [[feedback_report_show_html_not_chat_summary]]。

---

## 性能契約（依頼→Chrome表示 ≦60秒・超過はプロセスの誤り）
律速を作らないための構造：
- 数値最新化は**Bのスクリプト一発**（並列取得＋タイムアウト付き）で決定的に完了。役割エージェントの多段往復で数値を再集計しない。
- レビュー（C）は**軽量1巡**＝各レビューskillの観点を1回ずつ当てるだけ。
- フォーマット固定・前回HTMLベースなので「バグ発覚→ゼロからやり直し」が起きない。

**超過時の切り分け（≦60秒を超えたら、まずどこで詰まったかを特定）：**
1. **B（取得）が遅い**＝最有力。`collect()` は並列化済みなので、通常は楽天Playwrightがタイムアウト上限まで粘っているケース。→ タイムアウトが効いているか確認し、必要なら楽天だけスキップして他PFで60秒に収まるか確認（楽天セルは現状維持でよい）。
2. **C（レビュー）で重い再取得**＝レビューが数値を再集計している。→ レビューは整合チェックのみ（再集計しない）。
3. **A/Dの取り違え**＝ベースHTMLをlatest_report.txtから取らずゼロ生成している/保存先を誤っている。→ A・Dの一意パスを再確認。

## 対象PF（運用中の全て）
YouTube本体(MAINE)／YouTube人智の外側／YouTube Kids(スカイツリー)／IG @mynaturescape／IG @myraisingdiary／ブログ(GA4/AdSense/Search Console)／楽天ROOM・アフィリ／note。
※Threadsは2026-07-22オーナー決定により運用停止＝レポート集計対象から除外。

## 参照
- レポートフォーマット詳細（表仕様・確定ルール）＝`.company/secretary/report_format.md`。
- 生成物と `latest_report.txt` は `seo_growth/` 側。分析md/researchの置き場＝`.company/analyst/work/`。
- 個別取得スクリプト（Bのcollect()内部が回す・単体デバッグ用の参照）：`youtube_report.py` / `instagram_report.py` / `ga4_report.py` / `adsense_report.py` / `search_console_report.py` / `rakuten_room_report.py`（先に `rakuten_affiliate_report.py`）/ `note_report.py` / `threads_report.py`。通常フローではBのスクリプトが一括で回すので個別に叩かない。
