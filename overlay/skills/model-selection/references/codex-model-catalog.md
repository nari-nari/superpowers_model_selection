# Codex contract and candidate catalog

Revision: `2026-11-11.codex.1`. Checked: 2026-11-11. This is a documented reference snapshot, **not an account allowlist**. [OpenAI Pricing](https://learn.chatgpt.com/docs/pricing) and [Models](https://learn.chatgpt.com/docs/models) are the official verification entry points. Recheck after 30 days or a rejected selection. Do not infer exact runtime IDs, access or capacity from display names or token-price tiers.

## Identify the contract first

The trial user confirmed ChatGPT login and Plus. Start with `chatgpt-plus-bucket-unknown`; identify whether the test uses included limits or purchased credits, and the active speed. Login alone does not prove that credit billing applies.

| Contract / condition | Cost evidence and selection |
| --- | --- |
| ChatGPT included subscription usage | Credit prices alone do not determine included usage. Verify the dashboard and applicable usage definitions. Without a defensible per-call bound, estimate_cost remains unknown and price-based selection is held |
| Applicable Codex credit billing, Standard speed | Use the official credit token rates below after confirming eligibility. Credit purchase USD prices/discounts depend on the contract; do not assume a USD conversion |
| Fast / Ultrafast | Official purchased-credit rates use different multipliers (2x / 6x where available); included usage has different rules. Confirm actual speed, contract and availability before applying a multiplier |
| Legacy Enterprise or USD agreement | Use that agreement's applicable rate card, not this Standard table |
| API key | Use separately verified API pricing and account access. The current trial is not API-key usage |

Copilot credits and Codex credits are different units. The Copilot `USD / 0.01` formula never applies here. Unknown is not zero or free. Official Codex credit billing has no separate cache-write charge; this does not establish another contract's cache-write treatment.

## Reference rates and candidate identity

Standard Codex-credit rates per 1M tokens, from the official Pricing page. These rates are **conditional** on the applicable credit contract. They do not price the user's included Plus allowance. The ID column is a documented candidate slug, not an ID verified in this extension.

| model_key | Candidate documented ID | Input | Cached input | Output | Account access / runtime ID / capacity / supported effort |
| --- | --- | --- | --- | --- | --- |
| gpt6-luna | gpt-6-luna | 2.5 | 0.25 | 12.5 | unknown / unknown / unknown / unknown |
| gpt61-terra | gpt-6.1-terra | 25 | 1.25 | 125 | unknown / unknown / unknown / unknown |
| gpt61-sol | gpt-6.1-sol | 50 | 2.5 | 250 | unknown / unknown / unknown / unknown |

Capability qualification uses the official model description plus evidence for the task, tools, risk and capacity. These are discovery candidates, not a ranking or exhaustive list. Populate confirmed session entries with exact ID, access evidence, context/output limits, supported effort, source/date and task suitability. Missing capacity cannot be invented from a pricing threshold. At most three representative eligible configurations are compared per decision; no need to send the whole catalog to a child.

For confirmed credit billing, sum `uncached_input × input_rate + cached_input × cached_rate + billable_output × output_rate`, then divide by 1M. Establish how runtime usage counts reasoning and cached tokens before exact arithmetic; do not double count fields. Unknown cache reuse earns no assumed discount. An uncertain charge with no defensible bound makes cost selection unavailable. Include subsequent turns, mandatory review, handoff and a separately shown one-repair path; no guessed success probabilities.

## Discovery and capability probes

Use the [Codex extension adapter](runtimes/codex-vscode.md). A live allowlist in another chat or CLI is not extension-account evidence. Keep observed and requested settings separate in the [decision record](decision-record.md).

An explicitly user-authorized fixed-model capability experiment may proceed without cost ranking once account access, routing and capacity are confirmed. The initial trial is two small read-only calls, one per real model, sequentially. Record `selection_mode: fixed-capability-probe`, unknown costs and unverified cost acceptance. This does not make an unpriced candidate eligible for ordinary cost-selected work. If the native route is missing, hold that call.
