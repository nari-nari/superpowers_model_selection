---
name: model-selection
description: Use when an agent chooses or changes a model, reasoning effort, or context setting for delegated implementation, review, research, diagnosis, or skill evaluation.
---

# Cost-aware Model Selection

The parent chooses sufficient capability first, then predicted total cost. This policy governs model selection in every calling workflow, including final review and repair rounds. It never removes required tests, reviewers, or repair limits.

## Select before dispatch

1. **Establish constraints.** Honor explicit user models, exclusions and scoped budgets. Identify runtime and billing contract. Read the [catalog](references/copilot-model-catalog.md) and [profiles](references/task-profiles.md) once; retain revision/path for reuse and compaction recovery. Refresh when age >30 days or selection is rejected. If refresh fails, disclose stale estimates once.
2. **Qualify concrete candidates.** Use task risk, language/tools, interfaces and anticipated input/output growth. Verify account access, exact runtime ID and capacity from runtime evidence or a confirmed session overlay. Unknown is not available or free. High-risk authentication, concurrency, destructive or privacy work requires relevant evidence, regardless of file count. A final review has no automatic highest-capability exception.
3. **Compare configurations.** Compare eligible model + supported effort + context combinations; record at most three representative candidates and why others were excluded. Default to regular context and the profile's concrete effort/default. Predict peak occupancy, including output and hidden/runtime overhead; reserve 20% of the window. Above 80%, first consider focused files, log summaries or valid task splitting, then compare extended context/another model without losing needed information.
4. **Compare completion costs.** Use the [cost and record contract](references/decision-record.md): input, output/reasoning, cache, tier thresholds, history growth, required review and handoff. With no comparable retry evidence, compare the upper basic-path estimates and separately show a one-repair path; invent no success probabilities or universal turn multipliers. Prefer less handoff and stronger applicable evidence when uncertainty overlaps.
5. **Apply, then verify.** Explicitly pass the selected ID/settings using the runtime's supported call or a confirmed model-pinned worker. Text mentioning a model is not routing. Use the environment adapter; never invent parameters, change the parent model or launch a separate CLI wrapper. Record requested and effective values separately. Runtime metadata/logs are evidence; the model's own name claim is not. Unobservable effective values/costs stay `unknown`.

## Exceptions and re-selection

- No eligible/priced candidate or no enforceable routing: discover available options first. If unresolved, hold only that dispatch, explain once, and continue independent work. A native automatic fallback is not a successful explicit selection.
- Unsupported child-context control: record inherited/unknown, fit input to the effective window, or hold that dispatch if it cannot fit. Never report an unsupported setting as applied.
- Budget upper estimate exceeds the user's remaining scoped budget: compare alternatives, then ask only for the affected dispatch. No budget is mandatory; this is not hard billing enforcement.
- Missing information/context: improve inputs or split validly. Reasoning/knowledge deficit: re-qualify candidates for that deficit. Tool/auth failure: fix the environment. Smaller re-review: re-evaluate and allow cheaper choices. Keep the workflow's repair cap.
- Skill evaluation: use the experiment's designated model/settings unchanged across variants. Other billing contracts use their own verified prices; this Copilot catalog is not a price source for Codex/Claude API subscriptions.

Keep selection in the parent. Give children their task, relevant artifacts and applied settings, not this entire catalog or another model-selection assignment.
