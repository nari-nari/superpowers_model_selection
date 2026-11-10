# Copilot CLI model routing

Official reference rechecked 2026-11-11; this environment has no `copilot` executable, so native behavior is **unverified**. Use [model-selection](../../SKILL.md) as the model-choice authority and its [decision record](../decision-record.md). Model prices apply only to the confirmed Copilot AI-credit token contract.

## Load the independent entry

CLI reads its own `~/.copilot/copilot-instructions.md` (or supported modular instructions) and personal skills under `~/.copilot/skills/`. For a project use supported `.github/copilot-instructions.md`/`AGENTS.md` plus `.github/skills/model-selection/`. Include the actual entry rules and the verified `model_selection_skill` binding, then confirm the installed skill is read in a new session. Editor settings and links do not establish CLI inclusion. Inspect personal/project/plugin sources for old same-name skills before enabling the new setup. Do not replace existing user instructions wholesale.

## Confirm and dispatch

1. On the intended machine, run `copilot --version`; inspect `/model`, `/subagents` and the available `task` schema. Confirm account/organization access and exact IDs, not only public model availability. The catalog's CLI slugs are official identifiers, not evidence that this account can use them.
2. Apply a per-call model/effort override only when the live schema exposes it. Otherwise invoke a registered custom agent with the selected model. Record actual override precedence rather than deleting explicit values and hoping inheritance is cheap.
3. The reference describes per-call overrides before subagent settings, agent fields, then parent inheritance. It also documents Auto sessions inheriting the resolved parent instead of the agent's `model`. Treat Auto as a possible routing limitation, not the same as this policy. Confirm actual behavior/version; do not switch the parent without user instruction.
4. Compare requested and effective model from task metadata/logs or UI. If inaccessible use unknown. A fallback or model mismatch requires a new capability/permission/cost check; it is not proof that the requested choice was applied.

## Optional model-pinned worker

When direct selection is absent, use `independent-routing-copilot-cli-<model-key>` under `~/.copilot/agents/` or project `.github/agents/`. A VS Code Copilot worker may be shared only after actual compatibility confirmation. This is an inactive template; replace placeholders and confirm discovery before use:

```yaml
---
name: independent-routing-copilot-cli-confirmed
description: Use for bounded read-only tasks assigned to this model.
model: '<account-confirmed CLI ID>'
modelPolicy: required
reasoningEffort: '<supported effort value>'
tools: ['view', 'grep', 'glob']
---
```

Body: perform only the delegated task, read the supplied brief, return findings, and do not spawn agents. Omit an unsupported effort field instead of inventing one, recording its effective default/unknown. Implementation workers additionally need authorized editing/execution tools; this read-only example is not an implementer.

The CLI reference uses `modelPolicy`/`reasoningEffort`; the VS Code editor may show `model-policy`/`reasoning-effort`. They are not interchangeable merely because both files are Markdown. Required-model behavior, Auto, settings overrides and plugin conversion need native confirmation. Do not modify `~/.copilot/settings.json` or any MCP configuration as part of setup. A `models` preference list is ordered availability fallback, not task-by-task cost optimization.

Repository instructions are not included in CLI custom subagents by default. Add the documented `include-custom-instructions: true` only where needed, and verify handling; otherwise pass required conventions and the existing no-subagents contract in the task brief. Do not copy full session history or the entire pricing table into workers.

## Context and acceptance

`/context` reports usage, and `/model` exposes available model/context choices. `--context default|long_context` configures session startup; it does not prove a child-context parameter exists. This implementation never wraps children in separate CLI sessions. When child control is absent record inherited/unknown, reduce inputs within confirmed effective limits, or hold only the affected dispatch.

On the user's machine record version, account-access checks, two supported model IDs and the native requested/effective results for the same short read-only delegation. Also record Auto/override handling, a rejected choice, and context/effort metadata that is unavailable. Do not change global defaults to make the test pass. If required metadata cannot be observed, leave that acceptance condition unverified.

[Official CLI reference](https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference)

## Deployment lifecycle

Follow the standalone lifecycle guide at `setup_document` in the local deployment record. Deployment copies that guide separately and records its path and hash; the enumerated complete skill bundle does not assume a repository or a home-level `docs/` directory exists. If the binding is missing, locate the guide from the source distribution before updating or removing ownership-managed files. Preserve existing instruction bytes, record the installed bundle and managed-block hashes locally, and stop scoped replacement/removal on drift. A separate copy of the entry is derived from its source, not another policy authority. Restart the intended harness and verify inclusion; source preparation is not native acceptance.
