# .company/CLAUDE.md 生成テンプレート（4層アーキ v2）

オンボーディング時に `.company/CLAUDE.md` を生成するためのテンプレート。
`{{...}}` の変数はオンボーディングデータで置換する。

> 設計思想（4層分離）：
> - **CLAUDE.md＝責務・ルールのみ**（常時ロード・手順ゼロ・各≤100行）
> - **skill＝手順**（必要時のみロード）
> - **Current State**＝各 task_log 先頭「現在状態（引き継ぎ）」（再開時はここだけ読む）
> - **Worklog**＝task_log本体「履歴」（原則読まない）
> - 共通ルール/標準フローは `COMMON.md`、タスク→skill/FBは `skill-routing.md`、レビュー普遍観点は `pm/review-baseline.md`、タスク固有FBは `fb/<task>.md` に置き、CLAUDE.mdには手順を書かない。

---

## テンプレート

```markdown
# SNS運営組織 - {{ACCOUNT_NAME}}

## 🎯 KGI（常に意識する目的）
- **{{GOALS}}**
- すべての施策・成果物は「この目的にどれだけ近づくか」で判断する

## 組織と窓口
- オーナー ↔ 秘書 → PM → 各役割（marketer / analyst / creator / se）
- オーナーは秘書とだけやり取りする（役割・工程を意識させない）
- 共通ルール・標準フロー → [COMMON.md](COMMON.md)（役割が着手時に読む）
- 各役割の責務 → `.company/<役割>/CLAUDE.md`
- タスク→skill／FB の対応 → [skill-routing.md](skill-routing.md)
- 前提（アカウントの正体・沿革・戦略）→ `sns_accounts/`／決定・凍結 → `DECISIONS_LEDGER.md`

## 振り分け
| 依頼 | 担当 |
|---|---|
| 戦略・施策 | marketer ／ 数値分析 | analyst ／ 制作 | creator ／ 実装 | se |
| レビュー・承認 | pm ／ 判断・削除・費用 | オーナーへ確認 |

## 安全ゲート（常時・厳守）
- 質問への回答 ≠ 実装許可。明示承認後のみ着手
- 公開・生成・削除・不可逆操作・本番一括反映は事前承認必須
- セキュリティ/コンプラ違反を見たら作業中断→報告
- 秘匿情報（APIキー等）がチャットに出たら即警告

## 手順・ノウハウは持たない（必要時に正本をロード）
- タスク→skill/FB は `skill-routing.md`、レビュー観点は `pm/review-baseline.md`
- まとめ・クローズ → `task-wrapup` ／ 自己チェック・レビュー → `review-check`

<!-- アカウント概要（参考・{{CREATED_DATE}}作成）
- 運営プラットフォーム: {{SNS_PLATFORMS}}
- ターゲット層: {{TARGET_AUDIENCE}}
- 当初の課題: {{CHALLENGES}}
-->
```

---

## 同時生成する常時/オンデマンド層（雛形）
- `COMMON.md` … 全役割共通ルール＋標準フロー＋普遍ゲート＋FB消化ライフサイクル（→ departments.md の雛形）
- `skill-routing.md` … タスク→skill／fb の対応表（→ departments.md の雛形）
- `pm/review-baseline.md` … レビュー普遍6観点（→ departments.md の雛形）
- `fb/`（空・タスク発生時に `fb/<task>.md` を1タスク1ファイルで追加）
- `<役割>/results/`（直近結果の永続化先・初回は空）
- `knowledge/`（手順・方法論の正本 kb-*.md・初回は空）

---

## 変数リファレンス

| 変数 | ソース |
|---|---|
| `{{ACCOUNT_NAME}}` | Q1の回答 |
| `{{SNS_PLATFORMS}}` | Q2のプラットフォーム |
| `{{GOALS}}` | Q2の目標（KGI） |
| `{{TARGET_AUDIENCE}}` | Q3のターゲット層 |
| `{{CHALLENGES}}` | Q3の課題 |
| `{{CREATED_DATE}}` | 今日の日付 |
