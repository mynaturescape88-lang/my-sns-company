# local-file-cleanup スキル（編集用ソース）

ローカルファイルを安全に整理するスキルの**編集用ソース**です。

## 置き場所と版管理

このスキルは `my-sns-company` リポジトリの**リポジトリ直下 `skills/`**（プラグイン配布ツリー `plugins/sns-company/` の**外**）で版管理します。
`local-file-cleanup` は sns-company プラグインの所属スキルではなく `~/.claude/skills/` 直下の独立スキルであり、
プラグインの source→cache→marketplace 3拠点同期の対象に**混ぜてはいけない**ためです。

## 仕組み（編集＝即反映）

このディレクトリは `~/.claude/skills/local-file-cleanup` から**シンボリックリンク**で参照されています。
そのため **ここで `SKILL.md` を編集すれば、そのまま有効なスキルに即反映**されます（同期・再インストール作業は不要）。

```
~/.claude/skills/local-file-cleanup  ->  ~/Downloads/dept_App/my-sns-company/skills/local-file-cleanup/
```

## 編集方法

1. このフォルダの `SKILL.md` を編集
2. 保存すれば次回スキル起動時から反映（リンク経由のため即時）
3. `my-sns-company` は main 直push可。編集後はコミットして push する

## リンクが切れた/効かない場合の復旧

```bash
ln -sfn ~/Downloads/dept_App/my-sns-company/skills/local-file-cleanup ~/.claude/skills/local-file-cleanup
```

※もしClaude Codeがシンボリックリンクを読み込まない環境の場合は、リンクの代わりに
`SKILL.md` を `~/.claude/skills/local-file-cleanup/` へコピーする運用に切り替えてください。
