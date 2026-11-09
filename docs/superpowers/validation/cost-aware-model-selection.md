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
