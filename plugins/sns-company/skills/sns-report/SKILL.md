---
name: sns-report
description: >
  SNS運用レポートの作成依頼時に発動。『レポートを見せて』『SNSレポート』『数値分析して』『今月の数字』等で発動。STEP0=sns_accountsプロファイル必読、データ取得スクリプトの実行順序(必須)、分析ワークフロー順、レポートフォーマット参照を内包。
---

# SNS運用レポート作成スキル v1.0

SNS運用レポートの作成・数値分析を依頼されたときに発動する。
**目的**：secretary/CLAUDE.md にレポート手順を書いて毎回読む方式をやめ、レポートタスクのときだけ本スキルを発動して全手順を過不足なく適用する。

## 発動時に必ず行う（発動＝適用）

1. **STEP0：`.company/sns_accounts/` のプロファイルを必読**（README＋関係アカウント）。アカウントの位置づけを取り違えない。
2. **実行順序（必須）どおりにデータ取得スクリプトを実行**してから分析に入る。
3. **分析ワークフロー順**（指示書→PM→アナリスト→PM→SNSマーケター→PMレビュー）で進める。
4. **レポートフォーマット**（`.company/secretary/report_format.md`）に従い、秘書がオーナーへ報告する。

> このスキルはSNS運用レポート**専用**。データ取得は実行順序を厳守し、分析・解釈はアナリスト、戦略コメントはSNSマーケターが行う。

---

### SNS運用レポート作成フロー

> 🔴 **STEP 0（最優先・スキップ厳禁）**：データ取得・分析の前に、必ず `.company/sns_accounts/` の各プロファイル（README＋関係アカウント）を**フルで読む**。各アカウントの位置づけ・コンセプト・沿革・過去施策・戦略を取り違えると分析もマーケも全て失敗する（特にYouTube＝「植物ショップ長尺で収益化した本体／子供ショートは後発の派生」を読み違えない）。

以下スクリプトで最新データを取得してから分析フローを実行する。
**実行順序（必須）:**
1. `python seo_growth/rakuten_affiliate_report.py` ← 楽天アフィリエイト注文明細を更新（先に実行）
2. `python seo_growth/rakuten_room_report.py` ← ROOMフォロワー＋アフィリエイト統合レポート
3. `python seo_growth/ga4_report.py` / `instagram_report.py` / `adsense_report.py` / `search_console_report.py` / `youtube_report.py`（YouTube も毎回実行を試み、クォータ切れ時はスキップ）
4. `python seo_growth/threads_report.py` ← Threads 2アカウント（@mynaturescape・@myraisingdiary）フォロワー・表示回数・いいね・返信・リポスト・引用
5. `python seo_growth/note_report.py` ← note フォロワー数・記事別スキ数・前回比較

> レポートは運用中の全SNSが対象（ブログ＝GA4/AdSense/Search Console・Instagram・YouTube・Threads・楽天ROOM・楽天アフィリエイト・note）。レポート依頼時はすべて毎回実行する。

⚠️ `rakuten_affiliate_report.py` はPlaywrightを使用するため約30秒かかる。分析・レポート依頼時は必ず先に実行して最新データを取得してから分析すること。

秘書→指示書→PM→アナリスト（数値・傾向・課題分析）→PM→SNSマーケター（戦略コメント）→PM レビュー→秘書→オーナー報告。

### レポートフォーマット

`.company/secretary/report_format.md` 参照。
