---
file: README.md
version: 0.2.0
---

# handsOff

> Design to code. Hands off.

handsOff is an orchestrated skill library that converts Figma designs into
production-ready, framework-idiomatic code — fully styled, responsive,
accessibility-validated, and SEO-enriched — without any manual handoff.

## How it works

Five specialised agents coordinate through a shared context:

| Agent | Layer | Responsibility |
|---|---|---|
| researcher | 1 | Detects framework, fetches docs, builds source of truth |
| designer | 2 | Reads Figma, extracts tokens, maps layout and components |
| developer | 3 | Generates idiomatic code from designer's structured output |
| qa | 4 | Validates accessibility, fidelity, and markup correctness |
| seo | 5 | Enriches with meta tags, semantic audit, sitemap, performance hints |

## Pipeline Execution Order

```
User Input → intent-validator → stage-gate → researcher → designer → developer → qa (loop if needed) → seo → deliverable
```

Each skill is validated by `pipeline-guardian` before execution.

## Workflow Enforcement

### Orchestrator Mandate
The orchestrator MUST always delegate work to specialized agents. It does not perform work directly—it orchestrates through domain experts.

### Validation Gates
1. **intent-validator** — Confirms user intent before pipeline starts
2. **stage-gate** — Validates stage entry requirements before each stage
3. **pipeline-guardian** — Validates skill readiness before every skill

### What the Orchestrator MUST NOT Do
- Read Figma files directly (use designer.figma-reader)
- Write code directly (use developer.code-generator)
- Validate code directly (use qa.qa-validator)
- Perform SEO audits directly (use seo.semantic-auditor)
- Skip stages
- Execute stages out of order

### Agent Boundaries
Each agent owns its domain. The orchestrator retries through the same agent on failure—it does not "fix" issues manually.

## Terminal Communication

During pipeline execution, agents communicate in the terminal using a boxed format. Questions and clarifications are handled directly between agents and users.

See **terminal-communication.md** for full terminal output format and user interaction rules.

## Output Locations

**All generated source-of-truth files MUST be written to the repository root**, never inside handsoff-skills/.

| File | Location |
|---|---|
| tokens.md | `./tokens.md` |
| framework.md | `./framework.md` |
| shared-context.json | `./shared-context.json` |
| handsOff-logs.md | `./handsOff-logs.md` |

See **output-location-rules.md** for full rules.