# Task profiles

Use the catalog’s concrete `model_key`, not a cost/capability tier. These initial normal-risk assignments are **provisional design judgments** based on official descriptions, not measured success rates. Validate the task’s language/tools, risk, available capacity and actual account access before use. `recommended` requires relevant task/runtime evidence; `excluded` is a known mismatch. Prices are independent and live only in the catalog.

## Contract scope

The candidate JSON below belongs to the [Copilot catalog](copilot-model-catalog.md) and remains unchanged. For Codex, reuse the profile's task/risk requirements, then qualify candidates from the [Codex catalog](codex-model-catalog.md) and confirmed session evidence. A shared display name or model_key does not establish a shared runtime ID, account access, capacity or price. Missing Codex evidence is unknown; no Copilot candidate list is an implicit Codex allowlist. Keep high-risk suitability evidence separate from a model's general coding description. Fixed evaluator settings remain unchanged across variants.

```json
[
  {
    "profile": "mechanical",
    "candidates": [
      {
        "model_key": "gpt6-luna",
        "status": "provisional",
        "conditions": "bounded normal-risk work with directly runnable checks",
        "evidence": "copilot-model-catalog.md capability_evidence; official description only, task applicability is a provisional design assignment",
        "reasoning_requested": "default"
      },
      {
        "model_key": "gpt61-terra",
        "status": "provisional",
        "conditions": "normal-risk coding",
        "evidence": "copilot-model-catalog.md capability_evidence; official description only, task applicability is a provisional design assignment",
        "reasoning_requested": "default"
      },
      {
        "model_key": "sonnet55",
        "status": "provisional",
        "conditions": "normal-risk coding",
        "evidence": "copilot-model-catalog.md capability_evidence; official description only, task applicability is a provisional design assignment",
        "reasoning_requested": "default"
      }
    ]
  },
  {
    "profile": "integration-debugging",
    "candidates": [
      {
        "model_key": "gpt61-terra",
        "status": "provisional",
        "conditions": "normal-risk integration with bounded hypotheses and tests",
        "evidence": "copilot-model-catalog.md capability_evidence; official description only, task applicability is a provisional design assignment",
        "reasoning_requested": "default"
      },
      {
        "model_key": "sonnet55",
        "status": "provisional",
        "conditions": "normal-risk multi-step coding with tests",
        "evidence": "copilot-model-catalog.md capability_evidence; official description only, task applicability is a provisional design assignment",
        "reasoning_requested": "default"
      }
    ]
  },
  {
    "profile": "design-high-risk",
    "candidates": []
  },
  {
    "profile": "task-review",
    "candidates": [
      {
        "model_key": "gpt6-luna",
        "status": "provisional",
        "conditions": "mechanical normal-risk diff with explicit spec",
        "evidence": "copilot-model-catalog.md capability_evidence; official description only, task applicability is a provisional design assignment",
        "reasoning_requested": "default"
      },
      {
        "model_key": "gpt61-terra",
        "status": "provisional",
        "conditions": "normal-risk module diff",
        "evidence": "copilot-model-catalog.md capability_evidence; official description only, task applicability is a provisional design assignment",
        "reasoning_requested": "default"
      },
      {
        "model_key": "sonnet55",
        "status": "provisional",
        "conditions": "normal-risk cross-file diff",
        "evidence": "copilot-model-catalog.md capability_evidence; official description only, task applicability is a provisional design assignment",
        "reasoning_requested": "default"
      }
    ]
  },
  {
    "profile": "final-review",
    "candidates": [
      {
        "model_key": "gpt6-luna",
        "status": "provisional",
        "conditions": "small normal-risk branch with explicit interfaces",
        "evidence": "copilot-model-catalog.md capability_evidence; official description only, task applicability is a provisional design assignment",
        "reasoning_requested": "default"
      },
      {
        "model_key": "gpt61-terra",
        "status": "provisional",
        "conditions": "normal-risk branch with bounded cross-file interfaces",
        "evidence": "copilot-model-catalog.md capability_evidence; official description only, task applicability is a provisional design assignment",
        "reasoning_requested": "default"
      },
      {
        "model_key": "sonnet55",
        "status": "provisional",
        "conditions": "normal-risk multi-step branch",
        "evidence": "copilot-model-catalog.md capability_evidence; official description only, task applicability is a provisional design assignment",
        "reasoning_requested": "default"
      }
    ]
  },
  {
    "profile": "research-audit",
    "candidates": [
      {
        "model_key": "gpt6-luna",
        "status": "provisional",
        "conditions": "bounded lookup or summary; not privacy/security audit",
        "evidence": "copilot-model-catalog.md capability_evidence; official description only, task applicability is a provisional design assignment",
        "reasoning_requested": "default"
      },
      {
        "model_key": "gpt61-terra",
        "status": "provisional",
        "conditions": "normal-risk code research",
        "evidence": "copilot-model-catalog.md capability_evidence; official description only, task applicability is a provisional design assignment",
        "reasoning_requested": "default"
      },
      {
        "model_key": "sonnet55",
        "status": "provisional",
        "conditions": "normal-risk cross-file research",
        "evidence": "copilot-model-catalog.md capability_evidence; official description only, task applicability is a provisional design assignment",
        "reasoning_requested": "default"
      }
    ]
  },
  {
    "profile": "skill-evaluation",
    "candidates": []
  }
]
```

`design-high-risk` has no validated initial candidates. Authentication, concurrency, destructive changes and security/privacy audits require relevant evidence before a candidate is added, even if the diff is one file. Do not substitute the mechanical pool. Record missing evidence, seek available qualified candidates and hold only the affected dispatch if none qualify.

`skill-evaluation` uses the explicitly designated evaluation model and settings; populate that case’s concrete ID from the experiment, not a cheap default. Keep it fixed across variants. The inherited parent model is also a candidate only after its contract, ID, price and suitability are established.

For every profile, reasoning `default` means the runtime’s known default or an explicitly recorded unknown effective setting, not assumed inheritance. Override only with supported concrete values and a task reason. A changed re-review scope is reassessed for risk; the final-review name alone never demands a highest-ranked model.

Use `research-audit` for research. Its provisional candidates do not qualify for a high-risk redaction/security audit without additional evidence. All suitability judgments are task-local and remain separate from native runtime acceptance.
