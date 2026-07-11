---
name: sns-report
description: >
  SNS運用レポートの作成依頼時に発動。『レポートを見せて』『SNSレポート』『数値分析して』『今月の数字』等で発動。STEP0=sns_accountsプロファイル必読、データ取得スクリプトの実行順序(必須)、分析ワークフロー順、レポートフォーマット参照を内包。
---

# SNS運用レポート作成スキル v1.0

SNS運用レポートの作成・数値分析を依頼されたときに発動する。
**目的**：secretary/CLAUDE.md にレポート手順を書いて毎回読む方式をやめ、レポートタスクのときだけ本スキルを発動して全手順を過不足なく適用する。

## 🟢 集計は自動化済み＝レポート依頼時はその場で集計しない（2026-07-08オーナー承認）

- **数値集計は毎週火曜8:00の自動ジョブ**（launchd `com.snscompany.weekly-report` → `seo_growth/weekly_report_aggregate.py`）が全SNSデータ取得→カレンダー週是正→月次HTML再生成まで無人で行い、最新HTMLの絶対パスを `seo_growth/latest_report.txt` に書く。
- **したがってレポート依頼時のフローは「表示のみ」**＝その場でデータ取得スクリプトを叩き直さない。以下だけ行う:
  1. `seo_growth/latest_report.txt` を読む（無ければ `seo_growth/sns_monthly_report_*.html` の最新mtimeを使う）＝開くべきHTMLを一意に特定。
  2. そのHTMLを Chrome で開く: `open -a "Google Chrome" "$(cat seo_growth/latest_report.txt)"`。
  3. 秘書は「最新レポートを表示しました」程度に留める（数値の長文要約・表の再掲はしない [[feedback_report_show_html_not_chat_summary]]）。
- **戦略照合・見直し判断（下記STEP5）は引き続きマーケターが行う**＝表示済みHTMLの実数値を各PF戦略と突き合わせる。これは"集計"でなく"解釈"なので自動化対象外。
- **緊急時／臨時の再集計が要るときだけ手動実行**: `.venv/bin/python seo_growth/weekly_report_aggregate.py`（読み取りAPIのみ・graceful・冪等・commitしない。完了後 latest_report.txt が最新を指す）。個別スクリプトの逐次実行は自動集計ジョブ側の責務であって依頼時フローの責務ではない。

## 発動時に必ず行う（発動＝適用）

1. **STEP0：`.company/sns_accounts/` のプロファイルを必読**（README＋関係アカウント）。アカウントの位置づけを取り違えない。
2. **データ取得は自動集計済み**＝`latest_report.txt` が指す最新HTMLを開いて表示する（依頼時に取得スクリプトを再実行しない。緊急再集計のみ上記の手動実行）。
3. **分析・戦略照合ワークフロー**（表示済みHTML→アナリスト解釈→PM→SNSマーケター戦略照合→PMレビュー）で進める。
4. **レポートフォーマット**（`.company/secretary/report_format.md`）に従い、秘書がオーナーへ報告する。
5. **🔴 各PFごとに戦略を参照して照合する（このレポートの核）**＝`sns_accounts/<platform>.md` の「今後の戦略＋期待値」を読み、観測した実数値と突き合わせて「①実施中の戦略 ②期待値 ③観測数値 ④評価(想定通り/想定外) ⑤戦略見直し要否」を報告する。**④評価では各PFの「過去に行った施策と結果（＝施策ログ）」の実施日×現在数値を必ず突き合わせ、効果が出てそうか否かを帰属切り分け込みで述べる（2026-06-29恒久ルール＝個別の効果測定タスクは作らずここで完結。測定未到来なら再判定日を明記）。** **見直し不要なら何もしない（監視ログに観測・評価を追記）／必要(想定外)なら勝手に原本を変えず、マーケターが"見直し案"(具体修正・推奨1案＋根拠)を作り報告に添える→オーナー賛同後に原本を更新し監視ログに適用差分を記す**（戦略変更は承認ゲート）。PDCAは各PF戦略ファイル内で完結＝**別台帳(effect_measurement_schedule)は持たない/作らない**。

> このスキルはSNS運用レポート**専用**。データ取得は実行順序を厳守し、分析・解釈はアナリスト、**戦略との照合・見直し判断**はSNSマーケター（per-platform戦略の唯一の更新者）が行う。

**レポート成果物（分析md・research）の置き場**＝`.company/analyst/work/`。※月次HTMLの生成物と `latest_report.txt` は `seo_growth/` 側（本文の自動集計参照）。

---

### SNS運用レポート作成フロー

> 🔴 **STEP 0（最優先・スキップ厳禁）**：データ取得・分析の前に、必ず `.company/sns_accounts/` の各プロファイル（README＋関係アカウント）を**フルで読む**。各アカウントの位置づけ・コンセプト・沿革・過去施策・戦略を取り違えると分析もマーケも全て失敗する（特にYouTube＝「植物ショップ長尺で収益化した本体／子供ショートは後発の派生」を読み違えない）。

> **通常フローではここでデータ取得スクリプトを叩かない**（集計は自動ジョブ済み）。`latest_report.txt` が指す最新HTMLを開いて分析フローへ入る。以下の実行順序は **自動集計ジョブ `seo_growth/weekly_report_aggregate.py`（＝緊急時の手動再集計）が守る責務**であり、参照用に残す。

**データ取得の実行順序（自動集計ジョブの責務・緊急手動再集計時のみ手で追う）:**
1. `python seo_growth/rakuten_affiliate_report.py` ← 楽天アフィリエイト注文明細を更新（先に実行）
2. `python seo_growth/rakuten_room_report.py` ← ROOMフォロワー＋アフィリエイト統合レポート
3. `python seo_growth/ga4_report.py` / `instagram_report.py` / `adsense_report.py` / `search_console_report.py` / `youtube_report.py`（YouTube も毎回実行を試み、クォータ切れ時はスキップ）
4. `python seo_growth/threads_report.py` ← Threads 2アカウント（@mynaturescape・@myraisingdiary）フォロワー・表示回数・いいね・返信・リポスト・引用
5. `python seo_growth/note_report.py` ← note フォロワー数・記事別スキ数・前回比較
6. `python seo_growth/calendarize_report_history.py --write` ← カレンダー週の是正
7. `python seo_growth/monthly_report_build.py --no-open` ← 月次HTML再生成（Chromeは開かない）

> レポートは運用中の全SNSが対象（ブログ＝GA4/AdSense/Search Console・Instagram・YouTube・Threads・楽天ROOM・楽天アフィリエイト・note）。**これらの毎回実行は自動集計ジョブの中で完結する**＝依頼時に人手で回さない。

⚠️ `rakuten_affiliate_report.py` はPlaywrightを使用するため約30秒かかる（自動集計ジョブが吸収する）。緊急で手動再集計する場合のみ `weekly_report_aggregate.py` が先頭でこれを実行する。
⚠️ interpreter は **リポジトリ直下の `.venv`**（google-analytics-data 等が入っている）。`seo_growth/.venv` は yt-dlp 用で GA4 が動かない。

秘書→指示書→PM→アナリスト（数値・傾向・課題分析）→PM→SNSマーケター（戦略コメント）→PM レビュー→秘書→オーナー報告。

### レポートフォーマット

`.company/secretary/report_format.md` 参照。

---

## ◆成果物／・作業ノードの分類
- ・データ取得スクリプト実行（純作業＝レビューskillを当てない／実行順序の遵守は report-review-analysis 観点で担保）
- ◆数値分析（アナリスト成果物／→ `report-review-analysis`）
- ◆戦略コメント・見直し判断（マーケター成果物／→ `report-review-strategy`）
- ・監視ログ追記・原本更新（承認後の反映＝作業／承認ゲートの遵守は report-review-strategy 観点で担保）

## 子レビューskill 発動連鎖（成果物ノードごと・fail-closedで次へ）
- アナリスト分析完了後 → `report-review-analysis` を発動（pass のみ戦略コメントへ）
- マーケター戦略照合完了後 → `report-review-strategy` を発動
- 全◆pass後・秘書報告前 → `pm/review-baseline.md`（Layer1てっぺん総合）を1回当てる
※データ取得・ログ追記は作業ノード＝レビューskillを当てない。戦略原本の変更は承認ゲート

## 提出前ゲート（オーナー報告前・必須・fail-closed）
- **全記載を裏取り**してから出す＝実API/スクリプトの実値・`TASKS.md`/`task_logs`/`sns_accounts`プロファイル/各台帳で実物照合（古いスナップショット・静的memoに依存しない＝ground truth）。②戦略は実施済みのみ・GO待ちはTASKS起票済みを確認 [[feedback_report_verify_before_issue]]
- **レビュースキルを必ず通す**＝`report-review-analysis`（数値整合）＋`report-review-strategy`（戦略照合）＋`pm/review-baseline.md`を当て、全pass以外は提出しない。
- **全項目の裏が取れるまで「完了」と言わない**（一部裏取り・"それっぽく"pass禁止）[[feedback_report_no_empty_retrievable_cells]]
