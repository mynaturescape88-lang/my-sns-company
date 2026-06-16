---
name: project-infrastructure
description: >
  このプロジェクトの技術基盤（言語/API/スクリプト/ワークフロー資産/投稿スケジュール）を参照するときに発動。『どのスクリプトがある』『使っているAPI』『技術スタック』『Pinterest実装』等で発動。SE実装時の資産インベントリ。
---

# プロジェクト技術基盤インベントリ スキル v1.0

このプロジェクトの技術基盤（言語・API・スクリプト・ワークフロー資産・投稿スケジュール）を参照するときに発動する。
**目的**：se/CLAUDE.md にルールを書いて毎回読む方式をやめ、技術基盤の参照が必要なタスクのときだけ本スキルを発動して全インベントリを過不足なく参照する。

## 発動時に必ず行う（発動＝適用）

1. 実装・調査の前に、下記の技術スタック・既存スクリプト一覧を参照し、既存資産を再発明しないか確認する。
2. 投稿スケジュール（確定済み）と既存スクリプトの責務を踏まえて設計する。

> このスキルはこのプロジェクトの**技術資産インベントリ専用**。GitHub Actions保守は github-actions-ops、YouTube概要欄は youtube-operations、Instagram投稿経路は instagram-posting-pipeline を使う。

---

## 技術スタック

### 言語・実行環境
- Python 3.11
- GitHub Actions（CI/CD・定期実行）

### 投稿・SNS API
| API | 用途 |
|---|---|
| WordPress REST API | 記事作成・更新・公開 |
| Meta Graph API（Threads） | Threads投稿 |
| Instagram Graph API | @mynaturescape・@myraisingdiary投稿・分析 |
| YouTube Data API v3 | 概要欄更新・動画アップロード |
| YouTube Analytics API | 再生数・視聴維持率取得 |

### 分析・収益API
| API | 用途 |
|---|---|
| Google Search Console API | CTR・検索順位取得 |
| Google Analytics 4 API（GA4） | PV・セッション・流入元取得 |
| Google AdSense API | 収益・RPM取得 |
| 楽天市場API | アフィリエイトリンク・商品情報取得 |
| note API | スキ数・フォロワー数取得（月1回） |
| Pinterest API | ピン自動投稿（審査通過後） |

### 画像・メディア
| ツール | 用途 |
|---|---|
| Pixabay API | フリー画像取得（ブログ） |
| Pexels API | フリー画像取得（偉人秘録・ブログ） |
| Cloudinary | 動画・画像の一時ホスティング（Instagram投稿用） |
| yt-dlp | YouTube動画ダウンロード（@myraisingdiary素材） |

### AI・音声
| ツール | 用途 |
|---|---|
| Anthropic Claude API（Haiku） | 記事生成・コメント返信・キャプション生成 |
| Anthropic Claude API（Sonnet） | 高品質記事・台本生成 |
| VOICEVOX | 音声合成（偉人秘録） |
| FFmpeg | 動画合成・字幕焼き込み（偉人秘録） |
| Pillow | 画像生成・サムネイル（偉人秘録） |

### 投稿スケジュール（確定済み）
| ワークフロー | スケジュール |
|---|---|
| 国内ブログ記事 | 火・土 21:00 JST |
| 海外ブログ記事 | 木 21:00 JST |
| @myraisingdiary投稿 | 毎日 07:00・21:00 JST（81日目以降は07:00のみ） |
| @myraisingdiary投稿リトライ | 毎日 08:00・22:00 JST（失敗時） |
| YouTube概要欄更新 | 毎日 17:00 JST |
| 楽天アフィリエイト更新 | 毎月1日 09:00 JST |
| Threads自動投稿 | WordPress記事公開後 30分以内 |

---

## 既存スクリプト一覧

### common/（共通ツール）
| スクリプト | 用途 |
|---|---|
| `instagram_comment_replier.py` | Instagramコメント返信案生成・投稿 |
| `instagram_photo_poster.py` | Instagram写真投稿 |
| `instagram_shorts_scheduler.py` | @myraisingdiary 毎日自動投稿 |
| `mynaturescape_cleanup.py` | @mynaturescape 投稿整理 |
| `sanitizer.py` | テキストサニタイズ（共通） |
| `threads_poster.py` | Threads投稿 |
| `wp_comment_replier.py` | WordPressコメント返信 |
| `wp_threads_auto.py` | WordPress公開連動Threads自動投稿 |
| `youtube_comment_replier.py` | YouTubeコメント返信案生成・投稿 |

### jp/（国内ブログ記事生成）
| スクリプト | 用途 |
|---|---|
| `main.py` | 国内植物ニュース記事 自動生成・投稿 |
| `shop_article_generator.py` | ショップ紹介記事生成 |
| `scraper.py` | ニューススクレイピング |
| `summarizer.py` | Claude APIによる記事要約 |
| `image_fetcher.py` | Pixabay/Pexels画像取得 |
| `content_checker.py` | コンテンツ重複・品質チェック |
| `wp_poster.py` | WordPress投稿 |
| `url_log.py` | 投稿済みURL管理 |

### overseas/（海外ブログ記事生成）
| スクリプト | 用途 |
|---|---|
| `main.py` | 海外植物ニュース記事 自動生成・投稿 |
| （jp/と同構成） | — |

### seo_growth/（SEO・成長分析）
| スクリプト | 用途 |
|---|---|
| `adsense_report.py` | AdSense収益レポート |
| `article_scanner.py` | 記事品質スキャン |
| `check_and_retry_post.py` | @myraisingdiary投稿リトライ確認 |
| `competitor_analyzer.py` | 競合サイト分析 |
| `ctr_optimizer.py` | Search Console連動 CTR改善 |
| `ga4_report.py` | GA4アクセス分析レポート |
| `generate_mapping.py` | ショップ↔YouTube対応マッピング生成 |
| `health_report.py` | GitHub Actions稼働状況レポート |
| `improvement_report.py` | 改善施策効果追跡 |
| `instagram_caption_updater.py` | Instagramキャプション一括更新 |
| `instagram_report.py` | Instagramアナリティクスレポート |
| `performance_alerts.py` | KPIアラート通知 |
| `predownload_raising_videos.py` | @myraisingdiary動画事前ダウンロード |
| `rakuten_room_report.py` | 楽天ROOMレポート |
| `search_console_report.py` | Search Consoleレポート |
| `wp_affiliate_inserter.py` | 楽天アフィリエイト自動挿入 |
| `wp_footer_updater.py` | WordPressフッター一括更新 |
| `youtube_description_updater.py` | YouTube概要欄一括更新（677本） |
| `youtube_embed.py` | YouTubeショップ動画埋め込み |
| `youtube_report.py` | YouTubeパフォーマンスレポート |

### ijin_channel/（偉人秘録パイプライン・棚上げ中）
| スクリプト | 用途 |
|---|---|
| `run_pipeline.py` | 全工程一括実行エントリポイント |
| `pipeline/script_generator.py` | Claude API 台本生成 |
| `pipeline/tts_generator.py` | VOICEVOX 音声合成 |
| `pipeline/media_fetcher.py` | Pexels/Wikimedia 素材取得 |
| `pipeline/video_composer.py` | FFmpeg 動画合成・字幕焼き込み |
| `pipeline/thumbnail_generator.py` | Pillow サムネイル生成 |
| `pipeline/uploader.py` | YouTube動画アップロード |

---

## Pinterest実装時の技術メモ（審査通過後に着手）

- **リッチピン**：WordPress記事のOGPメタタグを正しく設定するとリッチピンが有効になる（価格・タイトル・説明を自動同期）
- **投稿頻度**：1日6〜15ピン（スイートスポット8〜12ピン）→ 予約投稿APIまたはGitHub Actionsで自動化を検討
- **画像サイズ**：2:3の縦型（例：1000×1500px）が推奨。既存のブログ画像を変換する処理が必要な場合は事前に確認する
- **実装順序**：OAuth認証 → 自動ピン投稿スクリプト → GitHub Actions定期実行
