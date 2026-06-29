---
name: planning-review-script
description: >
  content-planningの工程「台本・構成」のレビュー時に発動。親skill content-planning の台本/構成完成後から呼ばれる成果物レビュー（木の内側の子ノード）。起承転結4幕の明示・コールドオープンのフック・各幕の役割充足・転を1点に最大化・冒頭フックへの回収・謎を残さない結・尺見積もり・構造モデリング（逐語コピー禁止）を合否判定する。 ※棲み分け＝**長尺「人智の外側」の台本は jinchi-review-script／汎用企画（植物・子育て・その他ch）の台本は本skill／jinchi-longformから呼ばれたら jinchi-review-script を優先**（重複適用しない）。
---

# planning-review-script（台本・構成の成果物レビュー）

コンテンツ企画パイプラインの **台本・構成** 専用の成果物レビュー（汎用企画の台本用）。親 `content-planning` の台本/構成完成後から発動され、台本が起承転結と保持装置を満たすかを合否判定する（Layer2＝工程固有の作り込み合否）。

## 棲み分け（重複回避・必読）
- 長尺「人智の外側」（@ijin-hiroku・古代ミステリー×AI）の台本レビューは **`jinchi-review-script`** が正本（FC裏取り・語り部3挿入・TTS誤読対策まで内包する★最重要ノード）。
- 本skillは **汎用企画（植物・子育て・その他chの台本/構成）** 専用。
- **`jinchi-longform` から呼ばれた場合は `jinchi-review-script` を優先**し、本skillは適用しない（二重適用しない）。

## 読むのは3つだけ（混在禁止・token効率）
- ① **本skill内の観点リスト**（下記＝台本・構成固有の合格ライン）
- ② `.company/pm/review-baseline.md`（全レビュー普遍の6観点）
- ③ 対象プラットフォームの `.company/sns_accounts/<platform>.md`（台本の載るchの戦略・却下レバー＝`skill-routing.md`で特定）

## フロー（発動＝この順で全実行・fail-closed）
- ① 本skillの観点を成果物（台本・構成）に1つずつ照合（pass/fail）
- ② `pm/review-baseline.md` の6観点を1つずつ照合（特に観点4＝対象platform戦略と相違ないか）
- ③ **fail-closed**：1つでもfailなら提出せず→修正→①へ戻る
- ④ 全passのみ次工程（制作着手）へ進める

## 台本・構成の観点（合格ラインを内包・1つずつ照合）
- [ ] 起承転結を主骨格にし台本上に【起】【承】【転】【結】の4幕を明示したか （新規）
- [ ] 起＝コールドオープンのフック（二人称没入・好奇心ギャップ等）＋「この動画で何が起きるか」提示があるか （新規）
- [ ] 承＝各章に小さな前進と引き（次への問い）を置き冗長な停滞で離脱させない設計か （新規）
- [ ] 転＝物語が最も動く瞬間を1点に絞って最大化したか（見せ場を分散させていないか）（新規）
- [ ] 結＝冒頭フックへの回収（コールバック）で円環を閉じ、コメント誘発CTAがあるか （新規）
- [ ] 解説/ミステリー系は結で具体的な答え・中身を必ず提示したか（謎のまま終わらせない）[[feedback_ainsight_no_mystery_ending]]
- [ ] 尺を「ナレ字数÷312字/分＋章転換無音・終了画面余白」で見積もったか （新規）
- [ ] バズ動画の勝ち筋は構造モデリング（型を借りる）で借用し逐語コピーしていないか [[feedback_copy_proven_prompt_output_pairs]]
- [ ] 役割への指示は意図/ゴールを最小で（過剰演出にしていないか）[[feedback_brief_simply_with_owner_intent]]

## 原則
- レビューに出す＝既知の改善余地ゼロ・自分で直せる弱点を残さない [[feedback_review_means_finished_no_known_gaps]]
- 要件未達は自分で不合格にして作り直す（NGを「採用に足る」で通さない）[[feedback_strict_self_gate_reject_failures]]
- passした物だけ見せる（failを見せて判断を仰がない）
