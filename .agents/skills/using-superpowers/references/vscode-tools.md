# VS Code model routing

Checked against official documentation on 2026-11-10. Native execution in the user's VS Code is **unverified**. Determine whether the session uses Local or Copilot; the editor alone does not identify its tool schema. Apply `model-selection` before dispatch and retain its decision record. No MCP or global-setting change is required by this workflow.

## Local

1. Confirm the VS Code version, selected harness and exposed `agent/runSubagent`/`runSubagent` schema. Populate the catalog's `vscode-local` ID from model completions/picker; do not copy a CLI slug or guess a provider suffix.
2. If the call exposes a model parameter, pass the selected exact ID there. Otherwise invoke a registered custom worker with that model. Tool names/arguments come from the live schema; prose in `prompt` is not model selection.
3. Local precedence is explicit call model, custom agent model, eligible Auto, then main model. An explicit/custom selection can be rejected when it exceeds the main model's cost tier. Re-qualify the permitted candidates; don't switch the main model automatically or silently remove the explicit selection.
4. Read returned agent metadata or open the subagent indicator/log to confirm the model. Record `unknown` when unavailable; a model describing itself is not evidence. Context and effort controls are separate capabilities: never infer them from model selection.

## Copilot harness

Confirm the native `task` schema instead of forwarding Local arguments. Use its supported per-call model override if present; otherwise a confirmed custom worker. Copilot uses runtime model IDs; Local qualified display names are not portable. The `target` header controls file interpretation, not which harness is running.

The editor recognizes `model`, `reasoning-effort` and `model-policy`, but recognition does not prove runtime forwarding: plugin conversion does not forward `model-policy` and several other fields. Local `agents`/handoffs are not a Copilot routing API. Verify the live conversion/version; report unapplied or unknown settings instead of relying on a header to enforce them.

## Optional model-pinned worker

Only when a per-call override is unavailable, create the plan's matching `.github/agents/superpowers-local-{luna,terra,sonnet}.agent.md` or `superpowers-copilot-{luna,terra,sonnet}.agent.md` with an ID confirmed for that runtime. The following is an **inactive editing template**, not an installed worker or a verified ID:

Use this actual frontmatter shape after replacing the model with a picker-confirmed ID (the placeholder is not executable configuration):

```yaml
---
name: superpowers-local-luna
description: Use for delegated tasks with this explicitly selected model.
model: '<exact model ID from this harness>'
user-invocable: false
tools: ['read', 'search']
---
```

The body follows only the delegated task/brief and does not spawn children. This example is read-only; for implementation the configured tools must also permit authorized editing/testing, as advertised by the harness. Do not broaden a reviewer's tools. Use exact agent names and confirm discovery before dispatch; changing the file does not prove a running session reloaded it.

No worker is enabled by this repository until account access, ID and routing have been confirmed. If neither direct routing nor a registered worker works, hold only the affected dispatch and explain the needed setup once.

## Native acceptance checklist

Record VS Code/version, harness, catalog revision, IDs, account access, context/output limits and effort support. For each harness, use two eligible actual models for the same small read-only request such as finding the repository's package name; request the models via supported native routing and compare independent metadata. Check a refused model and an unavailable child-context control without fabricating success. Keep these results separate from Codex fixture simulations. Default context reserve is20%; unsupported controls stay inherited/unknown.

[Subagents](https://code.visualstudio.com/docs/agents/run/subagents) · [Custom-agent file reference](https://code.visualstudio.com/docs/agents/reference/custom-agent-file-reference)
