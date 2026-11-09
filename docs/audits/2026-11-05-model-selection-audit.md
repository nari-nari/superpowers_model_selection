# Superpowers モデル選択指示の走査結果

確認日: 2026-11-05（Asia/Tokyo）

対象コミット: `a8ea8fefdf74e078f4726eec8af154ac0df61b7e`

利用予定: VS Code と GitHub Copilot CLI の両方。

## 対象と方法

このリポジトリの `.agents/skills/` 全80ファイル（15個の `SKILL.md`、Markdown全65ファイル、その他15ファイル）を対象とした。隠しファイルと ignore 対象を含めて列挙し、全ファイルを読み取り、モデル名・能力・価格帯・派遣・既定値・推論強度・コンテキスト設定に関する文字列を検索して、候補箇所の周辺と参照先を確認した。

プラグインキャッシュ内の別コピー、ユーザー共通設定、Copilot 実行ログ、オンライン上流版は対象外。この報告はソース上の指示の監査であり、実際にどのモデルで課金されたかの診断ではない。スキル本文・参照ファイル・スクリプトは変更していない。

## 集計

- 直接的に曖昧な能力・価格帯を指定する記述: 20件。
- その選択節に依存する呼び出しテンプレート: 3件。
- モデル選択がない派遣・実行手順: 12件。
- 合計: 35件、該当ファイル20個。

「件」は表の1行を単位とする。同じ段落の連続した指示や、同じファイル内の同形の派遣例をまとめている。関連する構造上の不足はこの件数に加算していない。

## A. 直接的に曖昧なモデル選択指示

| ID | 場所 | 記述 | 未確定・欠落している点 |
| --- | --- | --- | --- |
| A01 | [subagent-driven-development/SKILL.md:142](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/subagent-driven-development/SKILL.md:142) | least powerful model that can handle each role | 必要能力の判定と料金の対応が未定義。 |
| A02 | [subagent-driven-development/SKILL.md:144](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/subagent-driven-development/SKILL.md:144) | fast, cheap model | 速さ・安さの基準、候補モデルが未定義。 |
| A03 | [subagent-driven-development/SKILL.md:146](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/subagent-driven-development/SKILL.md:146) | standard model | standard の所属モデル・価格帯が未定義。 |
| A04 | [subagent-driven-development/SKILL.md:148](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/subagent-driven-development/SKILL.md:148) | most capable available model | 能力の順位が未定義で、費用との比較条件もない。 |
| A05 | [subagent-driven-development/SKILL.md:149](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/subagent-driven-development/SKILL.md:149) | 最終レビューも most capable available model | 変更規模や予算に関係なく最高能力モデルを要求。 |
| A06 | [subagent-driven-development/SKILL.md:152](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/subagent-driven-development/SKILL.md:152) | 同様の判断／most capable／cheap-to-mid tier | レビューの判断基準は定性的で、能力・価格帯の対応が未定義。 |
| A07 | [subagent-driven-development/SKILL.md:157](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/subagent-driven-development/SKILL.md:157) | at least one tier above | tier の順序、同等能力の別モデル、上限到達時の扱いが未定義。 |
| A08 | [subagent-driven-development/SKILL.md:164](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/subagent-driven-development/SKILL.md:164) | Turn count beats token price／mid-tier を下限にする | 2–3倍のターン数という一般化の根拠はこの箇所に示されず、レビュー担当・文章仕様から実装する担当に対し、料金・作業別実績との比較なしに中位モデルを下限にする。 |
| A09 | [subagent-driven-development/SKILL.md:168](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/subagent-driven-development/SKILL.md:168) | cheapest tier | 候補集合と、入力・出力・キャッシュ・長文階層のどれで最安を決めるかが未定義。 |
| A10 | [subagent-driven-development/SKILL.md:172](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/subagent-driven-development/SKILL.md:172) | cheap／standard／most capable の再掲 | 上記の曖昧な区分を複雑度シグナルでも繰り返す。 |
| A11 | [subagent-driven-development/implementer-prompt.md:87](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/subagent-driven-development/implementer-prompt.md:87) | re-dispatch with a more capable model | エスカレーション先の能力比較、費用上限が未定義。 |
| A12 | [subagent-driven-development/re-review-prompt.md:104](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/subagent-driven-development/re-review-prompt.md:104) | cheap-to-mid tier | 再レビュー用の帯域も具体的なモデル・費用に結び付かない。 |
| A13 | [subagent-driven-development/references/final-review.md:10](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/subagent-driven-development/references/final-review.md:10) | most capable available model | 最終レビューの最高能力指定が参照ファイルにも重複。 |
| A14 | [subagent-driven-development/references/fix-loop.md:29](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/subagent-driven-development/references/fix-loop.md:29) | more capable model／capability bump | 4–5回目のモデル昇格先が未定義。 |
| A15 | [subagent-driven-development/references/handling-reports.md:9](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/subagent-driven-development/references/handling-reports.md:9) | more capable model | BLOCKED 時の推論能力による昇格先が未定義。 |
| A16 | [executing-plans/SKILL.md:55](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/executing-plans/SKILL.md:55) | mid-tier session model／most capable model | 親の中位モデルと最終レビューの最高能力モデルの対応が未定義。 |
| A17 | [executing-plans/SKILL.md:242](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/executing-plans/SKILL.md:242) | 最終レビューは most capable available model | 最高能力を要求し、未指定時の継承では不足し得るという説明で補強。費用条件はない。 |
| A18 | [writing-plans/SKILL.md:191](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/writing-plans/SKILL.md:191) | most capable／mid-tier／Cheapest and fastest | 方式選択の案内にもモデル区分が埋め込まれる。最安・最速という比較の適用条件も未定義。 |
| A19 | [using-superpowers/references/claude-code-tools.md:18](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/using-superpowers/references/claude-code-tools.md:18) | orchestrator subagent on a mid-tier model | Claude Code 専用の親役モデルの区分が未定義。VS Code／Copilot CLI の手順ではない。 |
| A20 | [using-superpowers/references/codex-tools.md:70](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/using-superpowers/references/codex-tools.md:70) | `<a mid-tier model from your spawn allowlist>` | 既定の子モデル設定例も中位の定義を呼び出し側に委ねる。Codex 専用で Copilot の設定ではない。 |

## B. 曖昧な選択節に依存するテンプレート

| ID | 場所 | 記述 | 未確定・欠落している点 |
| --- | --- | --- | --- |
| B01 | [subagent-driven-development/implementer-prompt.md:6](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/subagent-driven-development/implementer-prompt.md:6) | model: [MODEL … choose per SKILL.md Model Selection] | 指定は必須だが、選定内容は A01–A10 に依存。未指定で session の most expensive one を継承すると断定する表現も、親モデルが最高額であることを前提にしている。 |
| B02 | [subagent-driven-development/task-reviewer-prompt.md:11](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/subagent-driven-development/task-reviewer-prompt.md:11) | model: [MODEL … choose per SKILL.md Model Selection] | A01–A10 に依存。同じ most expensive one の断定がある。L191 のプレースホルダー説明でも同じ選択節を参照。 |
| B03 | [subagent-driven-development/re-review-prompt.md:11](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/subagent-driven-development/re-review-prompt.md:11) | model: [MODEL … choose per SKILL.md Model Selection] | A01–A10 に依存。同じ most expensive one の断定がある。L104–105 の再レビュー価格帯指定は A12 に記載。 |

`model` を明示する指示そのものは具体的だが、値の選定が A の未定義な区分に依存する。テンプレートの `most expensive one` は「最高額モデルを選べ」という命令ではなく、継承結果についての説明である。両者を区別する必要がある。

## C. モデル選択がない派遣・実行手順

| ID | 場所 | 記述 | 未確定・欠落している点 |
| --- | --- | --- | --- |
| C01 | [requesting-code-review/SKILL.md:32](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/requesting-code-review/SKILL.md:32) | general-purpose のレビュー担当を派遣 | モデル選択・推論強度・費用条件がなく、Model Selection 節への参照もない。 |
| C02 | [requesting-code-review/code-reviewer.md:7](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/requesting-code-review/code-reviewer.md:7) | レビュー用の呼び出しテンプレート | description と prompt はあるが model フィールドがない。上位の呼び出し元が指定する場合を除き、選択が環境側の既定に委ねられる。 |
| C03 | [dispatching-parallel-agents/SKILL.md:66](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/dispatching-parallel-agents/SKILL.md:66) | 3件の Subagent (general-purpose) 呼び出し | モデル指定と共通選択ルールへの参照がない。L147以降の別の派遣例でも指定しない。 |
| C04 | [brainstorming/SKILL.md:250](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/brainstorming/SKILL.md:250) | 設計の builder check を新規サブエージェントに派遣 | モデルの能力・費用基準、共通選択ルールへの参照がない。 |
| C05 | [brainstorming/builder-check-prompt.md:1](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/brainstorming/builder-check-prompt.md:1) | fresh general-purpose subagent | 設計確認担当のテンプレートにもモデル選択ルールがない。 |
| C06 | [diagnosing-superpowers/SKILL.md:36](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/diagnosing-superpowers/SKILL.md:36) | 各分析観点の analyst を並列派遣 | 7観点の分析担当についてモデル指定も役割別選択基準もない。 |
| C07 | [diagnosing-superpowers/SKILL.md:57](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/diagnosing-superpowers/SKILL.md:57) | scrub と scrub-audit を派遣 | 整形・秘匿化と監査のモデル選択がない。 |
| C08 | [diagnosing-superpowers/SKILL.md:67](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/diagnosing-superpowers/SKILL.md:67) | 候補セッションごとに similar-session を派遣 | 候補数に応じて派遣が増えるがモデル選択がない。 |
| C09 | [writing-skills/SKILL.md:276](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/writing-skills/SKILL.md:276) | 圧力シナリオをサブエージェントで反復テスト | テスト対象モデルを固定する規則、モデル選択・費用条件がここにはない。 |
| C10 | [writing-skills/SKILL.md:295](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/writing-skills/SKILL.md:295) | API または single-shot subagent、各 variant 5+ reps | 反復する呼び出しのモデル選択・推論強度・費用条件がない。 |
| C11 | [writing-skills/references/testing-skills-with-subagents.md:43](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/writing-skills/references/testing-skills-with-subagents.md:43) | 圧力テストの実行手順 | 実行するテスト用モデルをどう選ぶか、同一条件をどう固定するかを規定しない。 |
| C12 | [writing-skills/references/CLAUDE_MD_TESTING.md:182](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/writing-skills/references/CLAUDE_MD_TESTING.md:182) | subagent test harness を作り各 variant を実行 | 参考キャンペーンの実行例にもモデル指定がない。運用本体より優先度は低い。 |

ここでいう「指定がない」は当該手順とそのテンプレートの記述についての指摘。実行環境側の設定、上位の指示、呼び出し元がモデルを指定している可能性は残る。未指定が必ず高額モデルの選択になるとは、この走査からは断定できない。

## 関連する構造上の不足

### VS Code／Copilot CLI の接続手順

[using-superpowers/SKILL.md:52](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/using-superpowers/SKILL.md:52) の Platform Adaptation は Claude Code、Codex、Pi、Antigravity、Hermes、Muse を列挙するが、VS Code と Copilot CLI を列挙していない。`references/` にも両者向けの専用ファイルがない。`gemini-tools.md` は存在するが、この列挙には含まれない。

Copilot CLI への言及自体はある。[brainstorming/visual-companion.md:84](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/brainstorming/visual-companion.md:84) は補助サーバーの起動、[writing-skills/SKILL.md:12](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/writing-skills/SKILL.md:12) と [using-superpowers/references/gemini-tools.md:28](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/using-superpowers/references/gemini-tools.md:28) はスキルの格納先に関する記述であり、サブエージェントのモデル選択を接続する説明ではない。

### その他の実行環境のマッピング

以下にも派遣のマッピングはあるが、価格帯を具体的なモデル・設定に変換する手順はない。これらは VS Code／Copilot CLI 専用の手順として流用できると確認したものではない。

- [using-superpowers/references/gemini-tools.md:32](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/using-superpowers/references/gemini-tools.md:32)
- [using-superpowers/references/muse-tools.md:29](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/using-superpowers/references/muse-tools.md:29)
- [using-superpowers/references/pi-tools.md:7](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/using-superpowers/references/pi-tools.md:7)
- [using-superpowers/references/antigravity-tools.md:7](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/using-superpowers/references/antigravity-tools.md:7)
- [using-superpowers/references/hermes-tools.md:44](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/using-superpowers/references/hermes-tools.md:44)

### 料金・モデル一覧・コンテキスト・推論強度

走査対象には、具体的な選択用モデル一覧、Copilot の料金表、料金の確認日、長文課金の閾値と単価、通常／拡張コンテキストを選ぶ手順が見当たらない。`long_context|context_size|context_window|max_context|modelPolicy` の検索結果は0件。これはその文字列の不在を確認した結果であり、実行環境にその機能がないという意味ではない。

推論強度の明示指定は [using-superpowers/references/codex-tools.md:62](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/using-superpowers/references/codex-tools.md:62) と同ファイルの設定例にある。しかし、作業別に推論強度を選ぶ具体的な基準はなく、VS Code／Copilot CLI への接続手順もない。

コンテキスト量を抑える定性的な指示は存在する。例えば [subagent-driven-development/SKILL.md:203](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/subagent-driven-development/SKILL.md:203) のタスク brief と履歴を渡さない指示、[using-superpowers/references/codex-tools.md:17](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/using-superpowers/references/codex-tools.md:17) の clean context がある。したがって「コンテキスト対策が皆無」ではなく、「コンテキスト上限・料金階層をタスクごとに選ぶルールがない」という結果である。

## 指摘から除外したもの

- [subagent-driven-development/SKILL.md:160](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/subagent-driven-development/SKILL.md:160) の「モデルを必ず明示」は曖昧な選択指示ではない。具体的な値の決め方の不足を A と B に記録した。
- [using-superpowers/references/codex-tools.md:34](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/using-superpowers/references/codex-tools.md:34) の現行 allowlist 確認は、利用可能性を確認する具体的な指示であり、曖昧なモデル順位ではない。
- [writing-skills/references/anthropic-best-practices.md:132](/Users/nakayama/Documents/Learning/LLM/superpowers_custom/.agents/skills/writing-skills/references/anthropic-best-practices.md:132) の Haiku／Sonnet／Opus の説明は、利用予定モデル全体でスキルをテストするための参考資料。派遣モデルを選ぶ命令ではなく、ファミリー名のみで版・料金も特定しない。L1132 のチェックリストも同じ。
- `diagnosing-superpowers/templates/` の Models seen や model IDs は観測記録欄であり、モデル選択命令ではない。
- mental model、data modeling、MCP、sunk cost、作業自体の costly／expensive はモデル選択と無関係な検索一致として除外した。
- `writing-skills/references/discovery.md` の派遣記述は、文章の圧縮・相互参照の例であり、独立したモデル選択手順ではない。

## 先に見直し対象を把握するための優先順

1. A01–A10: SDD の共通選択節。多くの呼び出しがこの節に依存する。
2. A05・A13・A17・A18: 最終レビューの最高能力指定が複数ファイルに重複する。
3. B01–B03 と C01–C05: 実装・レビュー・設計確認の呼び出し箇所。
4. A11・A14・A15: 失敗時のモデル昇格。
5. VS Code／Copilot CLI の専用マッピングの不在と、両環境の共通ルールの接続。
6. C06–C12: 診断とスキルテストの反復呼び出し。

この報告では選択ルールの設計・実装、料金表の追加、スキルの修正は行っていない。

## 走査対象スキル

- `brainstorming`
- `diagnosing-superpowers`
- `dispatching-parallel-agents`
- `executing-plans`
- `finishing-a-development-branch`
- `receiving-code-review`
- `requesting-code-review`
- `subagent-driven-development`
- `systematic-debugging`
- `test-driven-development`
- `using-git-worktrees`
- `using-superpowers`
- `verification-before-completion`
- `writing-plans`
- `writing-skills`
