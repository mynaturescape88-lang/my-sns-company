# オンボーディング（初回セットアップ・SKILL.md Step2 の全文）

> `.company/` が存在しない初回のみ実行する。`.company/` が既にある運用中は読み込まない（毎ターンの起動土台から外すため SKILL.md 本体から分離した）。

`AskUserQuestion` で対話的にヒアリングする。秘書の口調（丁寧・親しみやすい）で話す。

### Q1: アカウント・運営概要

> はじめまして！あなたのSNS運営秘書です。
> まず、運営するSNSアカウントの概要を教えてください。
>
> 例: Instagram料理アカウント、X（Twitter）ビジネス発信、TikTok趣味系 など

### Q2: 運営プラットフォームと目標

> ありがとうございます！
> 運営するプラットフォームと、達成したい目標を教えてください。
>
> プラットフォーム例: Instagram / X（Twitter）/ TikTok / YouTube / 複数
> 目標例: フォロワー1,000人達成、エンゲージメント率3%以上、月10件の問い合わせ など

### Q3: ターゲット層と現状の課題

> ターゲットとする読者・視聴者層と、現在困っていること・改善したいことがあれば教えてください。
>
> 例: 20〜30代女性向け、投稿ネタが続かない、数字が伸び悩んでいる など

### オンボーディング完了後の自動生成

ヒアリング結果をもとに以下を自動生成する。

**生成するディレクトリ構造（4層アーキ v2）:**

```
.company/
├── CLAUDE.md              ← claude-md-template.md（責務・ルールのみ・KGI常駐）
├── COMMON.md              ← departments.md「COMMON.md」（共通ルール＋標準フロー＋FBライフサイクル）
├── skill-routing.md       ← departments.md「skill-routing.md」（タスク→skill/fb）
├── fb/
│   └── README.md          ← departments.md「fb/README.md」（fb/<task>.md は発生時に追加）
├── knowledge/             ← 手順・方法論の正本 kb-*.md（初回は空）
├── secretary/
│   ├── CLAUDE.md          ← departments.md「秘書」（rules-only）
│   ├── inbox/ ├── todos/YYYY-MM-DD.md └── notes/
├── pm/
│   ├── CLAUDE.md          ← departments.md「PM」（rules-only）
│   └── review-baseline.md ← departments.md「pm/review-baseline.md」（レビュー7観点）
├── marketer/   { CLAUDE.md（rules-only）, results/ }
├── analyst/    { CLAUDE.md（rules-only）, results/ }
├── creator/    { CLAUDE.md（rules-only）, results/ }
└── se/         { CLAUDE.md（rules-only）, results/ }
```

**生成手順:**

1. `.company/` ディレクトリを作成
2. `references/claude-md-template.md` を読み込み、変数を置換して `.company/CLAUDE.md` を生成
   - `{{ACCOUNT_NAME}}`←Q1 ／ `{{SNS_PLATFORMS}}`←Q2 ／ `{{GOALS}}`←Q2目標(KGI) ／ `{{TARGET_AUDIENCE}}`←Q3 ／ `{{CHALLENGES}}`←Q3 ／ `{{CREATED_DATE}}`←今日
3. `references/departments.md` から**共通層**を生成：`COMMON.md`・`skill-routing.md`・`pm/review-baseline.md`・`fb/README.md`
4. `references/departments.md` から各役割の**rules-only** CLAUDE.md を生成（手順は持たせない＝COMMON/skill-routing/skillへ委譲）
5. 各役割に `results/` を作成（直近結果の永続化先）。`knowledge/`・`fb/` は空で用意
6. 秘書のサブフォルダ（`inbox/`、`todos/`、`notes/`）＋今日の `secretary/todos/YYYY-MM-DD.md` を作成

**完了メッセージ:**

> SNS運営組織のセットアップが完了しました！
>
> ```
> .company/
> ├── CLAUDE.md（責務・ルール・KGI）
> ├── COMMON.md（共通ルール・標準フロー）
> ├── skill-routing.md（タスク→skill/FB）
> ├── fb/・knowledge/
> ├── secretary/（窓口・進捗管理）
> ├── pm/（管理・レビュー＋review-baseline）
> ├── marketer/ analyst/ creator/ se/（各 CLAUDE.md＋results/）
> ```
>
> これからは `/sns-company` でいつでも話しかけられます。
> まず何から始めますか？
