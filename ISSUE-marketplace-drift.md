# 課題メモ：marketplace版の陳腐化（3箇所同期漏れ）

- 検出日：2026-07-02（everyone-pmプロジェクトでdev-companyの改修方針を検討中、比較対象としてsns-companyの構造を調べていて発見）
- 起票者：Claude（everyone-pmセッション）／対応：sns-company側で実施予定

## 起きている問題

このプラグインは物理的に3箇所に存在する。

1. **source**（このリポジトリ）: `/Users/Papa/Downloads/dept_App/my-sns-company/plugins/sns-company/`（git管理・正）
2. **cache**（実行時に読まれる本体）: `~/.claude/plugins/cache/my-sns-company/sns-company/1.0.0/skills/sns-company/`
3. **marketplace**（`/plugin install`・`/plugin marketplace update`時の配布元）: `~/.claude/plugins/marketplaces/my-sns-company/plugins/sns-company/skills/sns-company/`

現状：
- source と cache は完全一致（md5一致）→ 今使っている実体は最新。
- **marketplace だけが初回コミット（`d95e9c7` 仮想SNS運営組織プラグイン 初回コミット）のまま停止**しており、source側で進んだ以下4コミット分が未反映：
  - `8616f96` feat(sns-company): アーキ再設計 Phase 1b（review-check skill新設・task-wrapup拡張・テンプレ4層化）
  - `71211c3` perf(sns-company): SKILL.md薄型化＝初回オンボをreferences/onboarding.mdへ退避
  - `8415c73` feat(task-wrapup): 手順(Pd)はSkill昇華時にMEMORY索引行を新設しない分岐を明文化
  - `4937e90` refactor(skills): notes/解体に伴う参照パス張替え＋保存先ルール切替
- 影響範囲：スキル一式で **47ファイルがsourceと不一致**（`sns-company/SKILL.md`単体でも、オンボーディングのAskUserQuestionフロー vs `references/onboarding.md`委譲、保存先が`secretary/notes/`か`secretary/work/`か等、実質的な仕様差分あり）。

## なぜ危険か

今すぐの実害はない（実行時はcacheを読むため）。ただし今後 **`/plugin marketplace update` や `/plugin install`（再インストール／別環境への導入）を行うと、marketplaceの古い内容がcacheへ巻き戻る**。その結果、直近の改修（Reviewer統合・review-check新設・task-wrapup拡張・保存先ルール変更など）が消え、古い仕様に戻ってしまう。

姉妹プラグインのdev-company（`my-dev-company`）は同じ3箇所構造を持つが、SKILL.md内に「編集→cache/marketplaceへcp同期→diffで一致確認」というルールを明文化して運用しており、3箇所が常に完全一致（diff 0件）していることを確認済み。sns-companyにはこの運用ルールが存在しない。

## 確認・対応の進め方（案）

1. 今すぐ：`diff -rq` でsource⇔cache⇔marketplaceの3方向差分を再確認し、cache側が本当にsourceと一致しているか（＝巻き戻す対象がmarketplaceのみか）を確定する。
2. source → marketplace へ `cp -R` 等で同期し、`diff -rq` で一致を確認する。
3. 再発防止として、dev-companyのSKILL.md該当箇所を参考に、sns-company側のSKILL.md（またはCLAUDE.md等の運用ドキュメント）にも「編集後は3箇所同期必須」ルールを明記する。
4. 可能であれば、sns-company側のtask-wrapup等のスキルレビュー観点に「skills/配下を編集したら3箇所同期を忘れずに」という一文を追加し、忘却事故を防ぐ。

## 参考：確認に使ったコマンド

```bash
# 3箇所のSKILL.mdハッシュ比較
md5 "plugins/sns-company/skills/sns-company/SKILL.md" \
    "~/.claude/plugins/cache/my-sns-company/sns-company/1.0.0/skills/sns-company/SKILL.md" \
    "~/.claude/plugins/marketplaces/my-sns-company/plugins/sns-company/skills/sns-company/SKILL.md"

# スキル全体の差分ファイル数
diff -rq "plugins/sns-company" \
         "~/.claude/plugins/marketplaces/my-sns-company/plugins/sns-company"

# source側の未反映コミット確認
git log --oneline -- plugins/sns-company/skills/sns-company/SKILL.md
```
