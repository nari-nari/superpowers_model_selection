# VS Code model routing

Official instructions, subagent and custom-agent references rechecked 2026-11-11. Native execution in the user's VS Code is **unverified**. Determine whether the session uses Local or Copilot; the editor alone does not identify its tool schema. Apply [model-selection](../../SKILL.md) before dispatch and retain its [decision record](../decision-record.md). No MCP or global-setting change is required by this workflow.

## Load the independent entry

Local uses a user-profile instruction created through Agent Customizations; Copilot uses its native user-level instruction locations. Personal skills can be placed at `~/.copilot/skills/model-selection/`. For a project, use supported `.github/copilot-instructions.md` or `AGENTS.md` plus `.github/skills/model-selection/`. Confirm Local's inclusion settings. The management directory `overlay/` is not a discovered skill directory. Include the entry's actual rules and verified `model_selection_skill` binding, read the installed skill, and check the resulting request/read history in a new chat. Linked documents are not guaranteed to be included. Check all active project, personal and plugin sources for duplicate old skills. No universal instruction-file precedence is assumed.

## Local

1. Confirm the VS Code version, selected harness and exposed `agent/runSubagent`/`runSubagent` schema. Populate the catalog's `vscode-local` ID from model completions/picker; do not copy a CLI slug or guess a provider suffix.
2. If the call exposes a model parameter, pass the selected exact ID there. Otherwise invoke a registered custom worker with that model. Tool names/arguments come from the live schema; prose in `prompt` is not model selection.
3. Local precedence is explicit call model, custom agent model, eligible Auto, then main model. An explicit/custom selection can be rejected when it exceeds the main model's cost tier. Re-qualify the permitted candidates; don't switch the main model automatically or silently remove the explicit selection.
4. Read returned agent metadata or open the subagent indicator/log to confirm the model. Record `unknown` when unavailable; a model describing itself is not evidence. Context and effort controls are separate capabilities: never infer them from model selection.

## Copilot harness

Confirm the native `task` schema instead of forwarding Local arguments. Use its supported per-call model override if present; otherwise a confirmed custom worker. Copilot uses runtime model IDs; Local qualified display names are not portable. The `target` header controls file interpretation, not which harness is running.

The editor recognizes `model`, `reasoning-effort` and `model-policy`, but recognition does not prove runtime forwarding: plugin conversion does not forward `model-policy` and several other fields. Local `agents`/handoffs are not a Copilot routing API. Verify the live conversion/version; report unapplied or unknown settings instead of relying on a header to enforce them.

## Optional model-pinned worker

Only when a per-call override is unavailable, register `independent-routing-<runtime>-<model-key>` with an ID confirmed for that runtime. Personal workers use the runtime's supported user location (Copilot: `~/.copilot/agents/`; Local: confirm its offered profile location); project workers use `.github/agents/`. The following is an **inactive editing template**, not an installed worker or a verified ID:

Use this actual frontmatter shape after replacing the model with a picker-confirmed ID (the placeholder is not executable configuration):

```yaml
---
name: independent-routing-vscode-local-confirmed
description: Use for delegated tasks with this explicitly selected model.
model: '<exact model ID from this harness>'
user-invocable: false
tools: ['read', 'search']
---
```

The body follows only the delegated task/brief and does not spawn children. This example is read-only; for implementation the configured tools must also permit authorized editing/testing, as advertised by the harness. Do not broaden a reviewer's tools. Use exact agent names and confirm discovery before dispatch; changing the file does not prove a running session reloaded it.

For a Local worker, omit the `agent` tool and use `agents: []` where supported. For Copilot, confirm the allowed tool set excludes delegation in the actual harness. A body instruction alone is not proof that recursive invocation is disabled. Do not enable a worker while any model placeholder or unsupported required field remains.

No worker is enabled by this repository until account access, ID and routing have been confirmed. If neither direct routing nor a registered worker works, hold only the affected dispatch and explain the needed setup once.

## Native acceptance checklist

Record VS Code/version, harness, catalog revision, IDs, account access, context/output limits and effort support. For each harness, use two eligible actual models for the same small read-only request such as finding the repository's package name; request the models via supported native routing and compare independent metadata. Check a refused model and an unavailable child-context control without fabricating success. Keep these results separate from Codex fixture simulations. Default context reserve is20%; unsupported controls stay inherited/unknown.

[Instructions](https://code.visualstudio.com/docs/agents/reference/custom-instructions-reference) · [Subagents](https://code.visualstudio.com/docs/agents/run/subagents) · [Custom-agent file reference](https://code.visualstudio.com/docs/agents/reference/custom-agent-file-reference)

## Deployment lifecycle

Follow the standalone lifecycle guide at `setup_document` in the local deployment record. Deployment copies that guide separately and records its path and hash; the enumerated complete skill bundle does not assume a repository or a home-level `docs/` directory exists. If the binding is missing, locate the guide from the source distribution before updating or removing ownership-managed files. Preserve existing instruction bytes, record the installed bundle and managed-block hashes locally, and stop scoped replacement/removal on drift. A separate copy of the entry is derived from its source, not another policy authority. Restart the intended harness and verify inclusion; source preparation is not native acceptance.
