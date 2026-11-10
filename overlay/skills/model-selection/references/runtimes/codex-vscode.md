# Codex VS Code extension routing

Runtime: `codex-vscode`. Checked: 2026-11-11 against official OpenAI documentation. Native execution on the user's extension is **unverified**. Use the actual running extension/engine version, not merely an installed package version. Codex CLI, VS Code Local/Copilot and another chat are different evidence sources.

## Load in an isolated project

For the trial, create a new project with `AGENTS.md` containing the actual independent entry and a verified absolute `model_selection_skill` binding. Copy the complete skill bundle to `.agents/skills/model-selection/`, including the [Codex catalog](../codex-model-catalog.md). Keep a standalone setup guide at `setup_document` in the local ownership record. `overlay/` is a source directory, not an automatically loaded skill location.

Before deployment, obtain the actual entry `overlay/instructions/model-routing.md` and standalone guide `docs/model-routing-setup.md` from the same source distribution; the skill bundle alone is not a complete deployment package. In the prepared trial, the entry is already present in root `AGENTS.md`, the guide is `model-routing-setup.md`, and the local record is `.routing-local/ownership.json`. Read that record's `setup_document` path rather than guessing a home-level docs path.

The official [skills guide](https://learn.chatgpt.com/docs/build-skills) documents `.agents/skills` discovery for repositories. Initialize the new trial directory as a local Git repository to make its root unambiguous; do not add remotes. Open this folder in VS Code, start a **new Codex extension conversation**, and verify actual instruction inclusion and SKILL read history. A `/skills` listing or a link is discovery evidence only. Inspect active personal, project and plugin sources for duplicates without deleting unknown files. Existing official Superpowers must be separately identified and remain unchanged.

## Confirm the native route

Read the [subagents documentation](https://learn.chatgpt.com/docs/agent-configuration/subagents) and the live extension tool schema. Prefer an exposed per-call model argument, specifying the exact account-confirmed ID and supported effort. Do not copy tool names/parameters from this chat. Some schemas require an isolated fork for overrides; use only the current tool's supported context mechanism.

When no per-call override exists, current official docs support project-scoped custom agents under `.codex/agents/`. Confirm discovery, selection and applicability in the running version before using them. Each TOML file requires `name`, `description`, `developer_instructions`; `model` and `model_reasoning_effort` can pin model settings. Confirm precedence against explicit spawn values and other defaults. Do not change global `[agents]` defaults just to pass the experiment.

The following is an **inactive template**, never an installed worker or a verified ID. Replace placeholders and validate current support before saving a trial worker:

```toml
name = "independent-routing-codex-confirmed"
description = "Read-only fixture routing capability test."
model = "<account-confirmed-id>"
model_reasoning_effort = "<supported-effort>"
developer_instructions = "Read only the assigned fixture; do not edit, use the network, or delegate further."
```

A worker inherits permissions; a read-only body instruction is not a separate read-only sandbox. Confirm the parent's permission mode and actual tool surface. Exclude delegation tools where supported and verify the exclusion; when unavailable, record that recursion is prohibited by instruction but not technically disabled. Do not invent `tools` or sandbox fields in TOML, and do not enable unsupported/placeholder definitions. [IDE settings](https://learn.chatgpt.com/docs/ide/settings) and [config basics](https://learn.chatgpt.com/docs/config-file/config-basic) describe distinct editor and Codex configuration layers, including project trust requirements. Do not weaken security settings to make a worker load.

If no supported explicit path exists, hold that probe. A model name in a prompt, changing the parent, invoking a separate CLI session, or requesting an Auto fallback is not explicit child routing.

## Native evidence and small-call protocol

1. Record running VS Code, extension and engine versions, official Superpowers source/version, instruction sources, policy/catalog revisions and ChatGPT contract/bucket/speed. Unavailable values remain unknown.
2. Verify the entry and bound SKILL were actually read. Confirm two exact available model IDs, task suitability, context/output limits and effort support. Reserve 20% of confirmed context; unsupported child-context controls remain inherited only with evidence, otherwise unknown.
3. Establish usage collection **before** calls. The user-authorized trial is a fixed-model capability probe if cost selection is unavailable. Read the same one-line `fixture.txt` once per model, sequentially, using the identical brief. Gather per-call usage or attributable before/after deltas. No measurement-only repeats.
4. Inspect extension child-thread details and native result/log metadata for requested/effective model, effort and context. Check for custom-agent overrides, fallback and rejection. Self-identification is not evidence. If explicit routing is accepted but effective ID is hidden, record accepted route with effective unknown; model-application acceptance stays unverified.
5. Reproduce at most one safe refusal in the first trial. Record only observed overrides/unsupported controls; unreproduced branches stay unverified. Re-qualify after a mismatch, holding the next affected call when constraints fail.

Use the [decision record](../decision-record.md) with `surface_evidence` identifying the IDE session. Local data preparation, another chat's child calls and CLI results never satisfy IDE acceptance. Rate definitions come from the actual contract; aggregate quota changes with uncertain attribution are not per-call actual cost. Do not claim savings from two small samples.

## Deployment lifecycle and handoff

Use the standalone `setup_document` path in the local ownership record. Record the complete relative file set and SHA-256, entry managed bytes, setup guide and any verified worker separately. Update or remove only matching owned content; preserve unknown files and drift. Bundle size is enumerated, not fixed to six files. Verify relative links after copying and actual inclusion after a new conversation. Retain the trial while native evidence is pending, then check removal in another new extension conversation. Local cleanup alone does not pass native lifecycle acceptance.

If this session cannot operate the GUI, provide the existing trial folder path and one initial prompt covering environment/source/read inclusion only. Ask for the resulting minimal non-secret record; do not ask for entire auth/config files or unrelated logs. Proceed to the two calls only after their prerequisites are evidenced. No extra install, login, personal/global settings change or CLI wrapper is required by this adapter.
