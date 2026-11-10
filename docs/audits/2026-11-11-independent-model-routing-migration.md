# 独立モデルルーティング移行台帳

確認日: 2026-11-11。開始コミット: `1a7d90322c6d54c3d07e4f0f670e52b2286c140c`。
開始時のユーザー差分なし。旧実装保存: `4792d5e`。元のmainは変更せず、`feat/independent-model-routing`で実装する。

## 上流原本

公式取得元: https://github.com/obra/superpowers 。検証対象はタグ`v7.0.2`、peelしたコミット`22f0b463e7a5e579904dd6211a520321035526f7`、`skills/`のGit tree `fcfa2f8a624b4cd711cbade5bd3d89d43a52cfe4`。
初期取り込み`a8ea8fe:.agents/skills`と公式`v7.0.2:skills`はGit treeが一致する。全80ファイルでパス・blob・modeが一致し、内容差分0、権限差分0。比較したv7.0.0は31件、v7.0.1は14件の不一致。最新版だからではなく、完全一致する出自としてv7.0.2を選択した。
公式履歴の一時cloneを読み取り、原本を改変していない。導入先の版・ソースの一致は別途実機確認する。本体はこのリポジトリへ再同梱しない。

## 独自差分32件の移行先

| 状態 | 旧パス | 移行先・扱い |
| --- | --- | --- |
| M | `.agents/skills/brainstorming/SKILL.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| M | `.agents/skills/brainstorming/builder-check-prompt.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| M | `.agents/skills/diagnosing-superpowers/SKILL.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| M | `.agents/skills/dispatching-parallel-agents/SKILL.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| M | `.agents/skills/executing-plans/SKILL.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| A | `.agents/skills/model-selection/SKILL.md` | overlay/skills/model-selection/SKILL.md |
| A | `.agents/skills/model-selection/references/copilot-model-catalog.md` | overlay/skills/model-selection/references/copilot-model-catalog.md |
| A | `.agents/skills/model-selection/references/decision-record.md` | overlay/skills/model-selection/references/decision-record.md |
| A | `.agents/skills/model-selection/references/task-profiles.md` | overlay/skills/model-selection/references/task-profiles.md |
| M | `.agents/skills/requesting-code-review/SKILL.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| M | `.agents/skills/requesting-code-review/code-reviewer.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| M | `.agents/skills/subagent-driven-development/SKILL.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| M | `.agents/skills/subagent-driven-development/implementer-prompt.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| M | `.agents/skills/subagent-driven-development/re-review-prompt.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| M | `.agents/skills/subagent-driven-development/references/final-review.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| M | `.agents/skills/subagent-driven-development/references/fix-loop.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| M | `.agents/skills/subagent-driven-development/references/handling-reports.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| M | `.agents/skills/subagent-driven-development/task-reviewer-prompt.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| M | `.agents/skills/using-superpowers/SKILL.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| M | `.agents/skills/using-superpowers/references/antigravity-tools.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| M | `.agents/skills/using-superpowers/references/claude-code-tools.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| M | `.agents/skills/using-superpowers/references/codex-tools.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| A | `.agents/skills/using-superpowers/references/copilot-cli-tools.md` | overlay/skills/model-selection/references/runtimes/copilot-cli.md |
| M | `.agents/skills/using-superpowers/references/gemini-tools.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| M | `.agents/skills/using-superpowers/references/hermes-tools.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| M | `.agents/skills/using-superpowers/references/muse-tools.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| M | `.agents/skills/using-superpowers/references/pi-tools.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| A | `.agents/skills/using-superpowers/references/vscode-tools.md` | overlay/skills/model-selection/references/runtimes/vscode.md |
| M | `.agents/skills/writing-plans/SKILL.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| M | `.agents/skills/writing-skills/SKILL.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| M | `.agents/skills/writing-skills/references/CLAUDE_MD_TESTING.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |
| M | `.agents/skills/writing-skills/references/testing-skills-with-subagents.md` | 公式原本の役割手順を別途利用。モデル方針はoverlay/instructions/model-routing.md。旧コピーは有効配置から除去 |

## 走査35項目の対応

上流の場所は下記パスの`skills/`配下、v7.0.2原本を指す。旧行番号は歴史的索引。原本自体は書き換えない。

| ID | 上流原本の場所 | 分類 | 取り扱い・根拠 |
| --- | --- | --- | --- |
| A01 | `subagent-driven-development/SKILL.md` | 共通入口で扱うモデル方針 | 価格帯・最高能力・昇格の既定を能力と総コストで具体化。レビュー・修正上限は原本どおり維持 |
| A02 | `subagent-driven-development/SKILL.md` | 共通入口で扱うモデル方針 | 価格帯・最高能力・昇格の既定を能力と総コストで具体化。レビュー・修正上限は原本どおり維持 |
| A03 | `subagent-driven-development/SKILL.md` | 共通入口で扱うモデル方針 | 価格帯・最高能力・昇格の既定を能力と総コストで具体化。レビュー・修正上限は原本どおり維持 |
| A04 | `subagent-driven-development/SKILL.md` | 共通入口で扱うモデル方針 | 価格帯・最高能力・昇格の既定を能力と総コストで具体化。レビュー・修正上限は原本どおり維持 |
| A05 | `subagent-driven-development/SKILL.md` | 共通入口で扱うモデル方針 | 価格帯・最高能力・昇格の既定を能力と総コストで具体化。レビュー・修正上限は原本どおり維持 |
| A06 | `subagent-driven-development/SKILL.md` | 共通入口で扱うモデル方針 | 価格帯・最高能力・昇格の既定を能力と総コストで具体化。レビュー・修正上限は原本どおり維持 |
| A07 | `subagent-driven-development/SKILL.md` | 共通入口で扱うモデル方針 | 価格帯・最高能力・昇格の既定を能力と総コストで具体化。レビュー・修正上限は原本どおり維持 |
| A08 | `subagent-driven-development/SKILL.md` | 共通入口で扱うモデル方針 | 価格帯・最高能力・昇格の既定を能力と総コストで具体化。レビュー・修正上限は原本どおり維持 |
| A09 | `subagent-driven-development/SKILL.md` | 共通入口で扱うモデル方針 | 価格帯・最高能力・昇格の既定を能力と総コストで具体化。レビュー・修正上限は原本どおり維持 |
| A10 | `subagent-driven-development/SKILL.md` | 共通入口で扱うモデル方針 | 価格帯・最高能力・昇格の既定を能力と総コストで具体化。レビュー・修正上限は原本どおり維持 |
| A11 | `subagent-driven-development/implementer-prompt.md` | 共通入口で扱うモデル方針 | 価格帯・最高能力・昇格の既定を能力と総コストで具体化。レビュー・修正上限は原本どおり維持 |
| A12 | `subagent-driven-development/re-review-prompt.md` | 共通入口で扱うモデル方針 | 価格帯・最高能力・昇格の既定を能力と総コストで具体化。レビュー・修正上限は原本どおり維持 |
| A13 | `subagent-driven-development/references/final-review.md` | 共通入口で扱うモデル方針 | 価格帯・最高能力・昇格の既定を能力と総コストで具体化。レビュー・修正上限は原本どおり維持 |
| A14 | `subagent-driven-development/references/fix-loop.md` | 共通入口で扱うモデル方針 | 価格帯・最高能力・昇格の既定を能力と総コストで具体化。レビュー・修正上限は原本どおり維持 |
| A15 | `subagent-driven-development/references/handling-reports.md` | 共通入口で扱うモデル方針 | 価格帯・最高能力・昇格の既定を能力と総コストで具体化。レビュー・修正上限は原本どおり維持 |
| A16 | `executing-plans/SKILL.md` | 共通入口で扱うモデル方針 | 価格帯・最高能力・昇格の既定を能力と総コストで具体化。レビュー・修正上限は原本どおり維持 |
| A17 | `executing-plans/SKILL.md` | 共通入口で扱うモデル方針 | 価格帯・最高能力・昇格の既定を能力と総コストで具体化。レビュー・修正上限は原本どおり維持 |
| A18 | `writing-plans/SKILL.md` | 共通入口で扱うモデル方針 | 価格帯・最高能力・昇格の既定を能力と総コストで具体化。レビュー・修正上限は原本どおり維持 |
| A19 | `using-superpowers/references/claude-code-tools.md` | 対象外 | Codex／Claude用の別契約・別環境。原本の接続は維持し、Copilot単価を流用しない |
| A20 | `using-superpowers/references/codex-tools.md` | 対象外 | Codex／Claude用の別契約・別環境。原本の接続は維持し、Copilot単価を流用しない |
| B01 | `subagent-driven-development/implementer-prompt.md` | 本体に残る役割要件 | 派遣テンプレートと明示指定は原本。モデル値は親の共通入口で選ぶ |
| B02 | `subagent-driven-development/task-reviewer-prompt.md` | 本体に残る役割要件 | 派遣テンプレートと明示指定は原本。モデル値は親の共通入口で選ぶ |
| B03 | `subagent-driven-development/re-review-prompt.md` | 本体に残る役割要件 | 派遣テンプレートと明示指定は原本。モデル値は親の共通入口で選ぶ |
| C01 | `requesting-code-review/SKILL.md` | 共通入口で扱うモデル方針 | 派遣前に選択。調査・設計確認・診断・秘匿化監査の役割と独立性は原本どおり維持 |
| C02 | `requesting-code-review/code-reviewer.md` | 共通入口で扱うモデル方針 | 派遣前に選択。調査・設計確認・診断・秘匿化監査の役割と独立性は原本どおり維持 |
| C03 | `dispatching-parallel-agents/SKILL.md` | 共通入口で扱うモデル方針 | 派遣前に選択。調査・設計確認・診断・秘匿化監査の役割と独立性は原本どおり維持 |
| C04 | `brainstorming/SKILL.md` | 共通入口で扱うモデル方針 | 派遣前に選択。調査・設計確認・診断・秘匿化監査の役割と独立性は原本どおり維持 |
| C05 | `brainstorming/builder-check-prompt.md` | 共通入口で扱うモデル方針 | 派遣前に選択。調査・設計確認・診断・秘匿化監査の役割と独立性は原本どおり維持 |
| C06 | `diagnosing-superpowers/SKILL.md` | 共通入口で扱うモデル方針 | 派遣前に選択。調査・設計確認・診断・秘匿化監査の役割と独立性は原本どおり維持 |
| C07 | `diagnosing-superpowers/SKILL.md` | 共通入口で扱うモデル方針 | 派遣前に選択。調査・設計確認・診断・秘匿化監査の役割と独立性は原本どおり維持 |
| C08 | `diagnosing-superpowers/SKILL.md` | 共通入口で扱うモデル方針 | 派遣前に選択。調査・設計確認・診断・秘匿化監査の役割と独立性は原本どおり維持 |
| C09 | `writing-skills/SKILL.md` | 共通入口で扱うモデル方針 | 評価実験のモデルと設定を固定。費用のためにvariant条件を変えない |
| C10 | `writing-skills/SKILL.md` | 共通入口で扱うモデル方針 | 評価実験のモデルと設定を固定。費用のためにvariant条件を変えない |
| C11 | `writing-skills/references/testing-skills-with-subagents.md` | 共通入口で扱うモデル方針 | 評価実験のモデルと設定を固定。費用のためにvariant条件を変えない |
| C12 | `writing-skills/references/CLAUDE_MD_TESTING.md` | 共通入口で扱うモデル方針 | 評価実験のモデルと設定を固定。費用のためにvariant条件を変えない |

## パッケージ所有ファイル（開始時）

下記86ファイルだけを旧パッケージの除去対象とする。利用者の後続変更は除去前に再確認する。履歴上の実行スクリプトは編集せず、本体を別途利用するためそのコピーごと有効配置から外す。

```text
.agents/skills/brainstorming/SKILL.md
.agents/skills/brainstorming/builder-check-prompt.md
.agents/skills/brainstorming/scripts/frame-template.html
.agents/skills/brainstorming/scripts/helper.js
.agents/skills/brainstorming/scripts/server.cjs
.agents/skills/brainstorming/scripts/start-server.sh
.agents/skills/brainstorming/scripts/stop-server.sh
.agents/skills/brainstorming/visual-companion.md
.agents/skills/diagnosing-superpowers/SKILL.md
.agents/skills/diagnosing-superpowers/prompts/analyst-common.md
.agents/skills/diagnosing-superpowers/prompts/cost-and-time.md
.agents/skills/diagnosing-superpowers/prompts/plan-adherence.md
.agents/skills/diagnosing-superpowers/prompts/quality-evidence.md
.agents/skills/diagnosing-superpowers/prompts/repeated-work.md
.agents/skills/diagnosing-superpowers/prompts/request-conflicts.md
.agents/skills/diagnosing-superpowers/prompts/scrub-audit.md
.agents/skills/diagnosing-superpowers/prompts/scrub.md
.agents/skills/diagnosing-superpowers/prompts/similar-session.md
.agents/skills/diagnosing-superpowers/prompts/skill-timeline.md
.agents/skills/diagnosing-superpowers/prompts/stumbles.md
.agents/skills/diagnosing-superpowers/references/context-safety.md
.agents/skills/diagnosing-superpowers/references/github-issues.md
.agents/skills/diagnosing-superpowers/references/redaction-policy.md
.agents/skills/diagnosing-superpowers/references/session-discovery.md
.agents/skills/diagnosing-superpowers/templates/bundle-README.md
.agents/skills/diagnosing-superpowers/templates/case.md
.agents/skills/diagnosing-superpowers/templates/issue.md
.agents/skills/diagnosing-superpowers/templates/report.md
.agents/skills/dispatching-parallel-agents/SKILL.md
.agents/skills/executing-plans/SKILL.md
.agents/skills/executing-plans/scripts/task-done
.agents/skills/executing-plans/scripts/task-start
.agents/skills/finishing-a-development-branch/SKILL.md
.agents/skills/model-selection/SKILL.md
.agents/skills/model-selection/references/copilot-model-catalog.md
.agents/skills/model-selection/references/decision-record.md
.agents/skills/model-selection/references/task-profiles.md
.agents/skills/receiving-code-review/SKILL.md
.agents/skills/requesting-code-review/SKILL.md
.agents/skills/requesting-code-review/code-reviewer.md
.agents/skills/subagent-driven-development/SKILL.md
.agents/skills/subagent-driven-development/implementer-prompt.md
.agents/skills/subagent-driven-development/re-review-prompt.md
.agents/skills/subagent-driven-development/references/batching.md
.agents/skills/subagent-driven-development/references/cannot-verify.md
.agents/skills/subagent-driven-development/references/final-review.md
.agents/skills/subagent-driven-development/references/fix-loop.md
.agents/skills/subagent-driven-development/references/handling-reports.md
.agents/skills/subagent-driven-development/references/preflight.md
.agents/skills/subagent-driven-development/references/resuming.md
.agents/skills/subagent-driven-development/scripts/review-package
.agents/skills/subagent-driven-development/scripts/sdd-repo
.agents/skills/subagent-driven-development/scripts/sdd-workspace
.agents/skills/subagent-driven-development/scripts/task-brief
.agents/skills/subagent-driven-development/task-reviewer-prompt.md
.agents/skills/systematic-debugging/SKILL.md
.agents/skills/systematic-debugging/condition-based-waiting-example.ts
.agents/skills/systematic-debugging/condition-based-waiting.md
.agents/skills/systematic-debugging/defense-in-depth.md
.agents/skills/systematic-debugging/find-polluter.sh
.agents/skills/systematic-debugging/root-cause-tracing.md
.agents/skills/test-driven-development/SKILL.md
.agents/skills/test-driven-development/writing-good-tests.md
.agents/skills/using-git-worktrees/SKILL.md
.agents/skills/using-superpowers/SKILL.md
.agents/skills/using-superpowers/references/antigravity-tools.md
.agents/skills/using-superpowers/references/claude-code-tools.md
.agents/skills/using-superpowers/references/codex-tools.md
.agents/skills/using-superpowers/references/copilot-cli-tools.md
.agents/skills/using-superpowers/references/gemini-tools.md
.agents/skills/using-superpowers/references/hermes-tools.md
.agents/skills/using-superpowers/references/muse-tools.md
.agents/skills/using-superpowers/references/pi-tools.md
.agents/skills/using-superpowers/references/vscode-tools.md
.agents/skills/verification-before-completion/SKILL.md
.agents/skills/writing-plans/SKILL.md
.agents/skills/writing-skills/SKILL.md
.agents/skills/writing-skills/references/CLAUDE_MD_TESTING.md
.agents/skills/writing-skills/references/anthropic-best-practices.md
.agents/skills/writing-skills/references/bulletproofing.md
.agents/skills/writing-skills/references/discovery.md
.agents/skills/writing-skills/references/flowcharts.md
.agents/skills/writing-skills/references/graphviz-conventions.dot
.agents/skills/writing-skills/references/persuasion-principles.md
.agents/skills/writing-skills/references/testing-skills-with-subagents.md
.agents/skills/writing-skills/render-graphs.js
```

## 移行判定

抽出・検証後に旧コピーを除去する。全体の実機受入・利用先の切替完了は未確認。[新検証記録](../superpowers/validation/independent-model-routing.md)を参照。
