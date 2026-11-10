# Copilot CLI model routing

Official reference checked 2026-11-10; this environment has no `copilot` executable, so native behavior is **unverified**. Use `model-selection` as the model-choice authority. Model prices apply only to the confirmed Copilot AI-credit token contract.

## Confirm and dispatch

1. On the intended machine, run `copilot --version`; inspect `/model`, `/subagents` and the available `task` schema. Confirm account/organization access and exact IDs, not only public model availability. The catalog's CLI slugs are official identifiers, not evidence that this account can use them.
2. Apply a per-call model/effort override only when the live schema exposes it. Otherwise invoke a registered custom agent with the selected model. Record actual override precedence rather than deleting explicit values and hoping inheritance is cheap.
3. The reference describes per-call overrides before subagent settings, agent fields, then parent inheritance. It also documents Auto sessions inheriting the resolved parent instead of the agent's `model`. Treat Auto as a possible routing limitation, not the same as this policy. Confirm actual behavior/version; do not switch the parent without user instruction.
4. Compare requested and effective model from task metadata/logs or UI. If inaccessible use unknown. A fallback or model mismatch requires a new capability/permission/cost check; it is not proof that the requested choice was applied.

## Optional model-pinned worker

When direct selection is absent, use `.github/agents/superpowers-cli-{luna,terra,sonnet}.agent.md` for CLI-specific definitions. A Copilot worker may be shared only after actual compatibility confirmation. This is an inactive template; replace placeholders and confirm discovery before use:

```yaml
---
name: superpowers-cli-luna
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
