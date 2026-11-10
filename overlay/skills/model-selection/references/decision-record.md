# Cost and decision record

## Cost contract

For each model call: `USD = sum(billable_tokens × corresponding_USD_per_million) / 1,000,000`. Sum the calls in the completion path; `AI credits = USD / 0.01` for this Copilot billing contract. Include growing history, tool-output input, output/reasoning, mandatory review, handoff and repair. Do not multiply first-call input by turn count and label it exact.

Apply the documented pricing buckets. Cache-write prices may replace or supplement other charges depending on the model contract: establish that basis before exact arithmetic, avoiding duplicate accounting. Unknown cache reuse means no assumed read discount; uncertain write charges belong in a conservative bound. Never substitute zero for unknown. If no defensible bound exists, the candidate is unpriced and needs clarification rather than a fake numeric estimate.

For input near a pricing threshold calculate both sides. Window size, threshold-basis input count and effective occupancy are different quantities. Check strict/inclusive comparison and whether the rate applies to all tokens or only excess; when unknown, use full higher input/output rates for the upper estimate and label it uncertain. A context UI selection alone does not establish the charged tier.

Without comparable measured retry data, rank by the basic-path upper estimate. Show a one-repair/handoff path separately and disclose a changed ranking; do not invent probability-weighted expectations. Reliable task/runtime-specific observations can refine estimates, with sample size and source. Actual discount eligibility must be evidenced: explicit model selection does not inherit an Auto promotion.

## Compact record

Append to the existing ledger/work log; don't paste full tables into every dispatch. Recover policy path/revision after compaction. Reuse completed checks until inputs change.

```yaml
task: task-id / role
runtime: vscode-local | vscode-copilot | copilot-cli
routing_revision: policy-revision
catalog_revision: revision
superpowers_revision: observed-revision | unknown
runtime_version: observed-version | unknown
profile: mechanical | integration-debugging | design-high-risk | task-review | final-review | research-audit | skill-evaluation
model_requested: exact-runtime-id
model_effective: exact-runtime-id | unknown
effort_requested: supported-value | default
effort_effective: observed-value | unknown
context_requested: default | long_context | inherited
context_effective: known-setting | inherited | unknown
estimate_credits: [lower, upper] # or unknown with the missing pricing evidence
reason: qualification, representative alternatives and cost reason in one sentence
actual_cost: measured-value | unknown
evidence: runtime-result-or-log-reference | unavailable
```

An inherited setting needs the parent's observed setting as evidence; otherwise use unknown. A native reported model mismatch requires re-pricing and rechecking capability/permission before proceeding. If acceptable, record and continue; otherwise hold the next affected dispatch. No observation can reverse already-incurred charges.

Accepted supported explicit routing with inaccessible effective metadata can proceed within the other constraints; keep effective values unknown and native acceptance unverified. When there is no supported explicit route at all, hold that dispatch. Repeated session-wide revisions may be recorded once and referenced. Worker resumption reuses the last decision only while conditions/settings are unchanged; restore this record and the policy path after compaction.

## Worked example: fictional prices, not runtime IDs

A normal-risk final review has two confirmed adequate candidates. `fixture-a` costs $0.11 (11 credits), `fixture-b` $0.204 (20.4 credits). Choose a; b's additional high-risk capability is irrelevant here. For an authentication change a lacks the required evidence, so exclude it before price comparison. Never actually dispatch a fixture ID.

## Unsupported-routing example

With Auto resolving the parent, no model parameter, no registered worker and no effective metadata: `model_requested` can record the intended candidate, `model_effective: unknown`, `context_effective: unknown`, `actual_cost: unknown`. Hold the affected dispatch while seeking a supported route; do not send a textual request and call it applied. A predicted peak160001 with limit200000 crosses the 80% planning threshold, so first reduce input or validly split. Neither a model nor an extended window was configured by writing those requests.

## Reporting and mistakes

Routine choices belong in existing progress updates. Report model changes, long-tier exposure, missing prices and unapplied choices. Unknown actual usage is not a measured bill; arithmetic re-pricing is also an estimate. Control and changed evaluation variants keep the same designated evaluator and settings.

| Temptation | Required action |
| --- | --- |
| Final review means highest model | Qualify for the actual whole-branch risk, then compare total cost |
| A cheaper input rate wins | Include outputs, long tiers, cache and completion path |
| Model named in prompt was applied | Require native routing and independent effective evidence |
| Missing charge is zero | Bound conservatively or mark unpriced |
| Switch test model to save money | Keep the experiment's designated model/settings fixed |
