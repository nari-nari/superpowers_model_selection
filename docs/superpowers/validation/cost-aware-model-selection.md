# Cost-aware selection validation

Date: 2026-11-10. Base: f899cd8. Implementation: Native.

## Scope

Simulation uses this Codex session’s inherited parent preset and reasoning setting (provider model ID is not exposed by the spawn result), unchanged across variants. It is not Copilot native execution. Exact provider usage and actual cost are unavailable.

Baseline: python3 -m unittest discover -v found 0 tests; no existing suite. Neither copilot nor code executable is available. Native VS Code Local, VS Code Copilot and CLI acceptance remains unverified.

## Evidence

Task 1: F01–F05 arithmetic verified with Decimal; schema/profile references verified. Official rates/CLI IDs checked 2026-11-10. Native availability and capabilities remain unknown. Behavioral samples pending.

## Behavioral evidence (read-only Codex simulations)

Task 2: 5 independent contexts per variant for each A/B/C input. Sample1 control used separate A/B/C calls; samples2–5 used a single three-case call. Changed samples used the same case data. All inherited the same parent preset/effort without overrides; provider ID and usage were not exposed. Samples4/5 needed baseline-path clarification before answering. These are qualitative regression samples, not a statistical cost benchmark.

Input: [scenarios](../../../tests/model-selection/scenarios.md), plus A: normal final review a=$0.11/b=$0.204, b most capable; both normal-review-qualified, only b high-risk-qualified. C: same normal review, Auto, textual task only, no worker, no metadata, peak160001/limit200000. Read-only operation: no actual fixture dispatch, edits, web or nested agents. Control source: f899cd8 original SDD/final-review and executing-plans. Changed context adds model-selection and decision-record as model authority.

| Samples / agent evidence | Normal final | High-risk | Long-tier case | No native route (C) |
| --- | --- | --- | --- | --- |
| control_1a / control_1b / control_1c | b | b | b, .420004 vs .204002 | intended b; effective unknown; prepare textual request |
| control_2 | b | b | b, same amounts | request b textually; metadata unknown |
| control_3 | b | b | b, same amounts | proceed textually; metadata unknown |
| control_4 | b | b | b, same amounts | proceed textually; metadata unknown |
| control_5 | b | b | b, same amounts | prepare textual request; metadata unknown |
| changed_1 | a | b | b, same amounts | hold affected dispatch; discover routing; reduce peak; effective unknown |
| changed_2 | a | b | b, same amounts | hold; discover routing; reduce peak; effective unknown |
| changed_3 | a | b | b, same amounts | hold; discover routing; reduce peak; effective unknown |
| changed_4 | a | b | b, same amounts | hold; discover routing; reduce peak; effective unknown |
| changed_5 | a | b | b, same amounts | hold; discover routing; reduce peak; effective unknown |

Observed RED: control final review says “explicitly requires the most capable available model for every final whole-branch review”, selecting b despite adequate a. Threshold arithmetic and metadata honesty already passed; no failure was fabricated for these. GREEN: all five changed samples chose a for normal final and b for high-risk; all preserved correct threshold calculation and held unsupported routing.

`selection_edges` independently exercised F01–F05, unknown write price without bound (unpriced), stale snapshot31days (one disclosure), b-only20.4credits vs budget20 (hold), a basic.11/repair.24 vs b.204 (basic a, separate reversal, no probabilities), evaluator b fixed, auth-tool failure vs reasoning deficit, and compaction/actual_cost unknown. All expected decisions observed.

`workflow_control` read original using-superpowers, writing-plans, requesting-code-review, brainstorming/builder, dispatching-parallel-agents, diagnosing-superpowers and writing-skills/testing reference. Only writing-plans explicitly demanded b for final review; remaining callers did not explicitly require model selection. Evaluation already retained designated b. These passing controls are regression checks, not claimed old defects.

Task 3: `adapter_check` applied each new guide to Local cost-tier rejection (normal -> a, high-risk -> hold), CLI Auto requested a/effective b (reprice and hold if unqualified), missing child-context control at peak160001 (reduce first; effective unknown), and ignored plugin model-policy (not enforced). All expected simulated decisions observed. No active workers were created because account/runtime verification is unavailable; inactive editing templates are provided instead. Native acceptance remains unverified.

Task 4: `sdd_integration` first attempt hit service usage limit (no test result); user reset it and the repeated sample passed. Current SDD/templates/fix-loop chose a for normal final/re-review, b for one-file auth, separated missing context from tool auth errors, retained fresh implementer at4/5 and the maximum5 cap, and kept comparison in the controller. `inline_integration` chose a/b by risk, preserved the user's parent and required final review, and recorded unknown effective settings. `plan_handoff_check` produced a risk/cost-based reviewer choice, no mid-tier requirement or cheapest/fastest guarantee. `review_template_check` filled an explicit-model logical envelope and passed focused artifacts rather than price comparison to the reviewer. All four current skill integrations passed representative read-only fixture cases.

Task 5: each changed caller was edited and its representative application checked before proceeding to the next. `builder_integration` chose a for normal project check and b for authentication/concurrency design; required builder check retained. `parallel_integration` chose a/a/b for independent lookup, mechanical fix and auth/concurrency defect; controller kept selection and workers kept focused inputs. `diagnosis_integration` retained all seven analysts and independent scrub/audit, chose qualified b for sensitive auditing, and distinguished current estimate from historical charges and preserved export gates. `evaluation_integration` retained designated b/effort across variants, excluded new guidance from control inputs, recorded unavailable effective identity/cost as unknown, and preserved fresh contexts/repetitions. These are simulated routing/experiment-design responses, not an execution of a full new skill-training campaign or native Copilot test.

## Acceptance matrix

`pass` below means the specified documentation/fixture/simulation check passed, not native Copilot acceptance. Full end-to-end acceptance is NOT complete until AC18 and the native parts of AC11–AC13 are checked. No measured cost saving is claimed.

| AC | Document/fixture/simulation | Evidence | Native runtime |
| --- | --- | --- | --- |
| AC01 | pass | changed1–5, sdd_integration: normal a | unverified |
| AC02 | pass | changed1–5 and builder/parallel checks: qualified b for auth | unverified |
| AC03 | pass | changed1–5, inline/handoff: no highest-model exception | unverified |
| AC04 | pass | sdd_integration, review_template_check: reduced re-review a | unverified |
| AC05 | pass | Decimal F01–F03, selection_edges strict boundary | unverified |
| AC06 | pass | Decimal F04, selection_edges output-rate reversal | unverified |
| AC07 | pass | Decimal F05, unknown-write/unpriced response | unverified |
| AC08 | pass | selection_edges basic.11/repair.24 vs.204; no probability | unverified |
| AC09 | pass | adapter_check rejects inadequate/unpermitted fallback; catalog unknown access | unverified |
| AC10 | pass | selection_edges stale31days, disclosed once; source contract documented | unverified |
| AC11 | pass | adapter_check normal a/high-risk hold after b rejection | unverified |
| AC12 | pass | adapter_check Auto/mismatch/plugin forwarding case | unverified |
| AC13 | pass | changed1–5 and adapter_check context unknown/80% action | unverified |
| AC14 | pass | selection_edges b-only20.4 vs budget20 -> hold | unverified |
| AC15 | pass | evaluation_integration requested b/effort fixed; effective identity unknown | unverified |
| AC16 | pass | selection_edges recovery; parent-only comparison in caller tests | unverified |
| AC17 | pass | source audit mapping below; no active vague directives remain | not_applicable |
| AC18 | unverified | neither code nor copilot executable; manual procedure below | unverified |
| AC19 | pass | all simulations label unknown actual/provider metadata; no savings promise | unverified |
| AC20 | pass | sdd/inline/selection_edges separate tool failure and retain cap5 | unverified |

## Audit35 migration mapping

Source audit describes the pre-change files. The following maps every row to current authority/caller; source-level coverage is not evidence of native execution.

| Audit ID | Current source | Result |
| --- | --- | --- |
| A01 | [subagent-driven-development/SKILL.md L140](../../../.agents/skills/subagent-driven-development/SKILL.md) | pass: explicit policy/role/record connected |
| A02 | [subagent-driven-development/SKILL.md L140](../../../.agents/skills/subagent-driven-development/SKILL.md) | pass: explicit policy/role/record connected |
| A03 | [subagent-driven-development/SKILL.md L140](../../../.agents/skills/subagent-driven-development/SKILL.md) | pass: explicit policy/role/record connected |
| A04 | [subagent-driven-development/SKILL.md L140](../../../.agents/skills/subagent-driven-development/SKILL.md) | pass: explicit policy/role/record connected |
| A05 | [subagent-driven-development/SKILL.md L140](../../../.agents/skills/subagent-driven-development/SKILL.md) | pass: explicit policy/role/record connected |
| A06 | [subagent-driven-development/SKILL.md L140](../../../.agents/skills/subagent-driven-development/SKILL.md) | pass: explicit policy/role/record connected |
| A07 | [subagent-driven-development/SKILL.md L140](../../../.agents/skills/subagent-driven-development/SKILL.md) | pass: explicit policy/role/record connected |
| A08 | [subagent-driven-development/SKILL.md L140](../../../.agents/skills/subagent-driven-development/SKILL.md) | pass: explicit policy/role/record connected |
| A09 | [subagent-driven-development/SKILL.md L140](../../../.agents/skills/subagent-driven-development/SKILL.md) | pass: explicit policy/role/record connected |
| A10 | [subagent-driven-development/SKILL.md L140](../../../.agents/skills/subagent-driven-development/SKILL.md) | pass: explicit policy/role/record connected |
| A11 | [subagent-driven-development/implementer-prompt.md L6](../../../.agents/skills/subagent-driven-development/implementer-prompt.md) | pass: explicit policy/role/record connected |
| A12 | [subagent-driven-development/re-review-prompt.md L11](../../../.agents/skills/subagent-driven-development/re-review-prompt.md) | pass: explicit policy/role/record connected |
| A13 | [subagent-driven-development/references/final-review.md L13](../../../.agents/skills/subagent-driven-development/references/final-review.md) | pass: explicit policy/role/record connected |
| A14 | [subagent-driven-development/references/fix-loop.md L30](../../../.agents/skills/subagent-driven-development/references/fix-loop.md) | pass: explicit policy/role/record connected |
| A15 | [subagent-driven-development/references/handling-reports.md L11](../../../.agents/skills/subagent-driven-development/references/handling-reports.md) | pass: explicit policy/role/record connected |
| A16 | [executing-plans/SKILL.md L59](../../../.agents/skills/executing-plans/SKILL.md) | pass: explicit policy/role/record connected |
| A17 | [executing-plans/SKILL.md L237](../../../.agents/skills/executing-plans/SKILL.md) | pass: explicit policy/role/record connected |
| A18 | [writing-plans/SKILL.md L179](../../../.agents/skills/writing-plans/SKILL.md) | pass: explicit policy/role/record connected |
| A19 | [using-superpowers/references/claude-code-tools.md L20](../../../.agents/skills/using-superpowers/references/claude-code-tools.md) | pass: explicit policy/role/record connected |
| A20 | [using-superpowers/references/codex-tools.md L61](../../../.agents/skills/using-superpowers/references/codex-tools.md) | pass: explicit policy/role/record connected |
| B01 | [subagent-driven-development/implementer-prompt.md L15](../../../.agents/skills/subagent-driven-development/implementer-prompt.md) | pass: explicit policy/role/record connected |
| B02 | [subagent-driven-development/task-reviewer-prompt.md L20](../../../.agents/skills/subagent-driven-development/task-reviewer-prompt.md) | pass: explicit policy/role/record connected |
| B03 | [subagent-driven-development/re-review-prompt.md L20](../../../.agents/skills/subagent-driven-development/re-review-prompt.md) | pass: explicit policy/role/record connected |
| C01 | [requesting-code-review/SKILL.md L34](../../../.agents/skills/requesting-code-review/SKILL.md) | pass: explicit policy/role/record connected |
| C02 | [requesting-code-review/code-reviewer.md L5](../../../.agents/skills/requesting-code-review/code-reviewer.md) | pass: explicit policy/role/record connected |
| C03 | [dispatching-parallel-agents/SKILL.md L66](../../../.agents/skills/dispatching-parallel-agents/SKILL.md) | pass: explicit policy/role/record connected |
| C04 | [brainstorming/SKILL.md L250](../../../.agents/skills/brainstorming/SKILL.md) | pass: explicit policy/role/record connected |
| C05 | [brainstorming/builder-check-prompt.md L4](../../../.agents/skills/brainstorming/builder-check-prompt.md) | pass: explicit policy/role/record connected |
| C06 | [diagnosing-superpowers/SKILL.md L19](../../../.agents/skills/diagnosing-superpowers/SKILL.md) | pass: explicit policy/role/record connected |
| C07 | [diagnosing-superpowers/SKILL.md L19](../../../.agents/skills/diagnosing-superpowers/SKILL.md) | pass: explicit policy/role/record connected |
| C08 | [diagnosing-superpowers/SKILL.md L19](../../../.agents/skills/diagnosing-superpowers/SKILL.md) | pass: explicit policy/role/record connected |
| C09 | [writing-skills/SKILL.md L272](../../../.agents/skills/writing-skills/SKILL.md) | pass: explicit policy/role/record connected |
| C10 | [writing-skills/SKILL.md L272](../../../.agents/skills/writing-skills/SKILL.md) | pass: explicit policy/role/record connected |
| C11 | [writing-skills/references/testing-skills-with-subagents.md L5](../../../.agents/skills/writing-skills/references/testing-skills-with-subagents.md) | pass: explicit policy/role/record connected |
| C12 | [writing-skills/references/CLAUDE_MD_TESTING.md L185](../../../.agents/skills/writing-skills/references/CLAUDE_MD_TESTING.md) | pass: explicit policy/role/record connected |

Residual phrase scan: SDD “most expensive failure observed” describes repeated-work history; Claude adapter “most expensive seat” describes coordination overhead; writing-plans “2-3 sentences” is a writing-length placeholder. None instructs a model choice. No cheap/standard/highest-model selection directive remains in the changed operational paths.

## Native procedure (all currently unverified)

1. Open this custom copy in the intended VS Code harness or CLI and verify the loaded skill source, client version and native delegation schema. Use the README discovery/deployment steps; a plugin copy with the same name is not evidence of this version loading.
2. Confirm two actual allowed model IDs, their effective context/output and supported effort; add dated evidence to the catalog/session overlay. Do not enable unknown IDs or fixture names. No global defaults/MCP edits are needed.
3. Request the same small read-only delegation (e.g. repository package name) twice using those two explicit native model choices. Record tool call, returned independent model metadata, context/effort that can actually be observed and usage/cost or unknown. Textual/model-self-report is insufficient.
4. Check available refusal, Auto/settings override and ignored editor-property scenarios without changing global settings; verify the policy discovers/reprices/holds affected dispatches. If a scenario cannot be produced or observed, retain unverified.
5. Complete separate records for VS Code Local, VS Code Copilot and CLI; leave any missing confirmation unverified. Unknown limits or inaccessible effective ID are not native pass. Only then can AC18/full runtime acceptance be completed.

| Runtime | Version | Confirmed IDs | Effective evidence | Status |
| --- | --- | --- | --- | --- |
| VS Code Local | unknown | unknown | unavailable | unverified |
| VS Code Copilot | unknown | unknown | unavailable | unverified |
| Copilot CLI | unknown | public CLI slugs only, account unknown | unavailable | unverified |

## Deliberate implementation decisions

- Use isolated /private/tmp worktree rather than modifying main/.gitignore; changes live on feat/cost-aware-model-selection and need review/integration.
- No existing suite: unittest discovery found0 tests; dedicated arithmetic/data/link checks and behavioral samples are the available evidence.
- No live Copilot tools or confirmed VS Code IDs: provide inactive worker templates and explicit manual confirmation rather than enabling speculative workers. This leaves native end-to-end acceptance incomplete.

## Final independent review

`final_review` read the committed package plus Task6 working-tree additions, spec, plan, review template, README, validation and rulings. Requested preset: gpt-6.1-sol/medium from the live allowlist, isolated fork. Effective provider ID/billing was not exposed; this is not a Copilot cost-optimization measurement.

Verdict: no Critical, Important or actionable Minor issues in the Markdown implementation. Independent Decimal/schema/link/residual-phrase/whitespace checks passed. Ready for integration only as documented implementation with incomplete native acceptance, not as full end-to-end validation.

Declined-to-judge decisions by parent:

- Native routing remains unverified, including AC18 and native AC11–AC13; no tool is available to establish it here. Risk if mistaken: real clients may reject IDs/fields or apply different overrides.
- Actual charges/savings and effective simulation identity remain unknown. Risk if mistaken: price estimates might be read as observed bills or verified model identity.
- Behavioral samples are qualitative regression evidence, not statistical reliability estimates. Risk if mistaken: a small sample could be overgeneralized to arbitrary tasks.

## Execution rulings (complete record)

1. Use /private/tmp isolated worktree rather than modifying main/.gitignore. Rationale: preserves user checkout. Risk: review/integration must use the linked branch/worktree.
2. Baseline unittest discovery found0 tests, not existing test failures. Rationale: no project suite exists. Risk: feature-specific verification does not establish unrelated application behavior.
3. Copilot/code executables unavailable: finish independent documentation/simulation work, leave native acceptance unverified. Risk: account IDs, routing, limits and overrides still need the intended client.
4. No active workers with unconfirmed IDs: provide inactive, runtime-specific editing templates instead. Risk: first-time native setup is still required.
5. Augment committed review-package with tracked Task6 README/validation diff before final commit. Rationale: reviewer saw the full candidate without a premature completion record. Risk: final commit must match reviewed source; only this review outcome/decision record was added afterward.
6. Request gpt-6.1-sol/medium for cross-file final review; current Codex billing is not exposed, so make no optimization/savings claim. Risk: this review's price cannot be compared for the current contract.
7. Native/effective/billing/statistical topics set aside by the reviewer are intentionally unverified/unknown/qualitative as described above, not accepted as measured success. Risks are listed with those three decisions.

Deferred minors: none. Full native end-to-end acceptance remains incomplete. Implementation contains no runtime code/dependency changes, MCP changes or edits to user-global settings.

