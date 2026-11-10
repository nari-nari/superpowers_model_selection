# Copilot model catalog

Snapshot 2026-11-10.1. This is an initial shortlist, not an exhaustive model list or a capability ranking. Only token-based Copilot AI credits are covered; premium-request multipliers and other providers’ API contracts are outside this snapshot.

Official prices and CLI IDs were checked on 2026-11-10. **User-account availability, VS Code IDs, context/output limits and native dispatch are unverified.** The input pricing threshold is NOT a context-window limit. Never enable an unknown candidate by assuming its name or price implies access or sufficient capacity.

```json
{
  "catalog_revision": "2026-11-10.1",
  "verified_at": "2026-11-10",
  "pricing_mode": "copilot-ai-credits-token",
  "currency": "USD",
  "price_unit": "1M tokens",
  "credit_usd": 0.01,
  "models": [
    {
      "model_key": "gpt6-luna",
      "display_name": "GPT-6 Luna",
      "version": "GPT-6 Luna",
      "runtime_ids": {
        "vscode-local": "unknown",
        "vscode-copilot": "unknown",
        "copilot-cli": "gpt-6-luna"
      },
      "availability": {
        "vscode-local": {
          "state": "unknown",
          "verified_at": "unknown",
          "method": "not checked in user account"
        },
        "vscode-copilot": {
          "state": "unknown",
          "verified_at": "unknown",
          "method": "not checked in user account"
        },
        "copilot-cli": {
          "state": "unknown",
          "verified_at": "unknown",
          "method": "not checked in user account"
        }
      },
      "capability_evidence": {
        "status": "provisional",
        "summary": "Small, bounded repetitive tasks; official comparison only.",
        "source": "https://docs.github.com/en/copilot/reference/ai-models/model-comparison",
        "verified_at": "2026-11-10",
        "native_evidence": "unknown"
      },
      "context_limits": {
        "default": "unknown",
        "long_context": "unknown"
      },
      "output_limit": "unknown",
      "reasoning_values": {
        "vscode-local": "unknown",
        "vscode-copilot": "unknown",
        "copilot-cli": "unknown"
      },
      "cache_write_basis": "unknown",
      "price_tiers": [
        {
          "tier": "default",
          "threshold": 272000,
          "operator": "<=",
          "threshold_basis": "input tokens per official pricing table",
          "tier_application": "unknown",
          "input": 0.1,
          "cached_input": 0.01,
          "cache_write": 0.125,
          "output": 0.5
        },
        {
          "tier": "long_context",
          "threshold": 272000,
          "operator": ">",
          "threshold_basis": "input tokens per official pricing table",
          "tier_application": "unknown",
          "input": 0.2,
          "cached_input": 0.02,
          "cache_write": 0.25,
          "output": 0.75
        }
      ],
      "sources": [
        "https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing",
        "https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference",
        "https://docs.github.com/en/copilot/reference/ai-models/model-comparison"
      ]
    },
    {
      "model_key": "gpt61-terra",
      "display_name": "GPT-6.1 Terra",
      "version": "GPT-6.1 Terra",
      "runtime_ids": {
        "vscode-local": "unknown",
        "vscode-copilot": "unknown",
        "copilot-cli": "gpt-6.1-terra"
      },
      "availability": {
        "vscode-local": {
          "state": "unknown",
          "verified_at": "unknown",
          "method": "not checked in user account"
        },
        "vscode-copilot": {
          "state": "unknown",
          "verified_at": "unknown",
          "method": "not checked in user account"
        },
        "copilot-cli": {
          "state": "unknown",
          "verified_at": "unknown",
          "method": "not checked in user account"
        }
      },
      "capability_evidence": {
        "status": "provisional",
        "summary": "Everyday coding and terminal tasks; official comparison only.",
        "source": "https://docs.github.com/en/copilot/reference/ai-models/model-comparison",
        "verified_at": "2026-11-10",
        "native_evidence": "unknown"
      },
      "context_limits": {
        "default": "unknown",
        "long_context": "unknown"
      },
      "output_limit": "unknown",
      "reasoning_values": {
        "vscode-local": "unknown",
        "vscode-copilot": "unknown",
        "copilot-cli": "unknown"
      },
      "cache_write_basis": "unknown",
      "price_tiers": [
        {
          "tier": "default",
          "threshold": 272000,
          "operator": "<=",
          "threshold_basis": "input tokens per official pricing table",
          "tier_application": "unknown",
          "input": 1,
          "cached_input": 0.05,
          "cache_write": 1.25,
          "output": 5
        },
        {
          "tier": "long_context",
          "threshold": 272000,
          "operator": ">",
          "threshold_basis": "input tokens per official pricing table",
          "tier_application": "unknown",
          "input": 2,
          "cached_input": 0.1,
          "cache_write": 2.5,
          "output": 7.5
        }
      ],
      "sources": [
        "https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing",
        "https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference",
        "https://docs.github.com/en/copilot/reference/ai-models/model-comparison"
      ]
    },
    {
      "model_key": "sonnet55",
      "display_name": "Claude Sonnet 5.5",
      "version": "Claude Sonnet 5.5",
      "runtime_ids": {
        "vscode-local": "unknown",
        "vscode-copilot": "unknown",
        "copilot-cli": "claude-sonnet-5.5"
      },
      "availability": {
        "vscode-local": {
          "state": "unknown",
          "verified_at": "unknown",
          "method": "not checked in user account"
        },
        "vscode-copilot": {
          "state": "unknown",
          "verified_at": "unknown",
          "method": "not checked in user account"
        },
        "copilot-cli": {
          "state": "unknown",
          "verified_at": "unknown",
          "method": "not checked in user account"
        }
      },
      "capability_evidence": {
        "status": "provisional",
        "summary": "General coding with multistep tool use; official comparison only.",
        "source": "https://docs.github.com/en/copilot/reference/ai-models/model-comparison",
        "verified_at": "2026-11-10",
        "native_evidence": "unknown"
      },
      "context_limits": {
        "default": "unknown",
        "long_context": "unknown"
      },
      "output_limit": "unknown",
      "reasoning_values": {
        "vscode-local": "unknown",
        "vscode-copilot": "unknown",
        "copilot-cli": "unknown"
      },
      "cache_write_basis": "unknown",
      "price_tiers": [
        {
          "tier": "default",
          "threshold": "not_applicable",
          "operator": "not_applicable",
          "threshold_basis": "not_applicable",
          "tier_application": "not_applicable",
          "input": 2,
          "cached_input": 0.1,
          "cache_write": 2.5,
          "output": 10
        }
      ],
      "sources": [
        "https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing",
        "https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference",
        "https://docs.github.com/en/copilot/reference/ai-models/model-comparison"
      ]
    }
  ]
}
```

## Refresh and completion

At session start check revision and date. Age >30 calendar days is stale (exactly30 is within the freshness interval). Recheck official sources once when stale, unavailable or rejected; reuse a valid session snapshot. If refresh fails, report age once and label estimates provisional. Never fill an unknown price with zero. Numeric zero is a verified free rate, `not_applicable` means the billing component does not exist, and `unknown` requires discovery or conservative bounds. A missing rate with no defensible upper bound makes that candidate unpriced and excludes it from price-based selection.

Confirm availability and exact ID in each intended account/runtime, then record the tool/UI/log evidence and date for that session. Query available context, output and reasoning capabilities rather than inferring them from token-price thresholds. Record confirmed values in a session overlay or update this catalog with evidence. Do not change user/global defaults. A new model needs the same fields and evidence.

`cache_write_basis` and long-tier application scope have not been established from the price table alone. Obtain the model-specific billing definition before an exact calculation. Until then use a conservative range: ordinary uncached input plus potential write charges, and full higher-tier input/output at the high end when a threshold can be crossed. Do not call the upper bound an exact bill. Actual costs remain unknown unless independent usage/billing evidence is exposed.

Only documented, applicable discounts may enter an estimate. Auto/Fusion promotions do not automatically apply to explicit model routing. No discount is assumed in this snapshot.

## Sources

- [Copilot pricing](https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing): price buckets and credit conversion.
- [CLI supported models](https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference): CLI IDs, not proof of account access.
- [Copilot model comparison](https://docs.github.com/en/copilot/reference/ai-models/model-comparison): provisional task suitability, not task-specific validation.
