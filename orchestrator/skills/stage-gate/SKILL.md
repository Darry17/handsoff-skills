---
name: stage-gate
description: >
  Validates stage entry requirements. Returns { allowed: boolean, missing: [] }.
  Runs BEFORE stage execution begins.
  Ensures the orchestrator delegates to specialized agents with proper context.
version: 0.1.0
agent: orchestrator
reads:
  - meta.current-stage
  - shared-context
writes:
  - stage-gate-result
depends_on:
  - none
---

# stage-gate

## Purpose
Validates that a stage has all required inputs before executing. This ensures the orchestrator properly prepares context before delegating to specialized agents.

## When to Run
Before each stage begins:
- Before researcher stage
- Before designer stage
- Before developer stage
- Before qa stage
- Before seo stage

## Input Contracts Per Stage

| Stage | Required Context Keys |
|-------|---------------------|
| `researcher` | `detection` (may be populated) |
| `designer` | `detection.framework`, `detection.design-system-source` |
| `developer` | `source-of-truth.framework-md-path`, `source-of-truth.tokens-md-path`, `design.component-tree`, `design.section-manifest`, `design.layout-map` |
| `qa` | `code.generated-files` |
| `seo` | `code.generated-files`, `qa.qa-report` (if passed) |

## Validation Logic

```
1. Identify current stage from meta.current-stage
2. Lookup required context keys for that stage
3. For each required key:
   - If key missing or null in shared-context → add to missing
4. IF missing.length > 0:
     - Return { allowed: false, missing }
   ELSE:
     - Return { allowed: true }
```

## Inputs
- Current stage from meta
- Shared context

## Outputs
```json
{
  "allowed": boolean,
  "missing": ["string"],
  "stage": "string",
  "validation-timestamp": "ISO 8601"
}
```

## Rules
- MUST run before every stage
- MUST NOT allow stage execution if required inputs missing
- MUST return specific missing keys so orchestrator can populate them
- The orchestrator is responsible for populating missing context before re-validating