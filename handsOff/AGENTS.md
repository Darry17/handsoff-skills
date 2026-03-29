---
file: AGENTS.md
version: 0.2.0
purpose: Quick reference for all agents and their skill ownership
---

# handsOff — Agent directory

| Agent | Layer | Skills owned |
|---|---|---|
| orchestrator | 0 | orchestrator |
| researcher | 1 | framework-detector, design-system-detector, docs-fetcher, setup-wizard |
| designer | 2 | figma-reader, token-extractor, layout-analyzer, component-mapper, section-detector, design-system-creator |
| developer | 3 | code-generator, token-applier, responsive-adapter, scaffold-writer |
| qa | 4 | qa-validator, fidelity-checker, preview-builder, issue-reporter |
| seo | 5 | semantic-auditor, meta-writer, performance-advisor, sitemap-writer |

## Pipeline execution order

researcher → designer → developer → qa (feedback loop) → seo → deliverable

## Source of truth files

- `framework.md` — authored by researcher, read by developer and qa
- `tokens.md` — authored by researcher/designer, read by developer, qa, and seo
- `shared/shared-context-schema.md` — the canonical context object shape

## Naming convention

All skills follow the `<skill-name>` namespace.
All skill files live at `agents/<agent>/skills/<skill-name>/SKILL.md`.

---

# MANDATORY WORKFLOW ENFORCEMENT

## Core Principle

**The orchestrator MUST always utilize the specialized agents to execute work.** The orchestrator's role is to orchestrate—not to perform the work directly. This ensures each layer applies domain-specific expertise, maintains separation of concerns, and prevents single-point-of-failure bottlenecks.

## Orchestrator Mandate

The orchestrator MUST:

1. **Delegate ALL work** to specialized agents rather than attempting to execute skills directly
2. **Invoke agents in dependency order** as defined in the skill dependency graph
3. **Never skip stages** — every stage (researcher → designer → developer → qa → seo) must execute
4. **Wait for agent completion** before advancing to the next stage
5. **Handle agent failures** through the error recovery mechanisms defined in PIPELINE-RUNTIME.md

## When to Use Specialized Agents

| Scenario | Action |
|----------|--------|
| User submits Figma URL | Invoke `researcher` agent → its skills analyze the design |
| Framework needs detection | Delegate to `researcher.framework-detector` skill |
| Design tokens needed | Delegate to `designer.token-extractor` skill |
| Code generation required | Delegate to `developer.code-generator` skill |
| Validation required | Delegate to `qa.qa-validator` skill |
| SEO enrichment needed | Delegate to `seo.semantic-auditor` skill |

## What the Orchestrator Should NOT Do

- Attempt to read Figma files directly (use `designer.figma-reader`)
- Attempt to write code directly (use `developer.code-generator`)
- Attempt to validate code directly (use `qa.qa-validator`)
- Attempt to perform SEO audits directly (use `seo.semantic-auditor`)
- Skip any stage in the pipeline
- Execute multiple stages in parallel when they have dependencies

## Validation Gates

Before any stage executes, the orchestrator MUST run:

1. **intent-validator** — Confirm user intent before pipeline starts
2. **stage-gate** — Validate stage entry requirements
3. **pipeline-guardian** — Validate skill readiness before each skill runs

These gates enforce that work flows through specialized agents, not around them.

## Error Handling

If a specialized agent fails:
1. Review the error code from the agent's output
2. Apply the recovery action defined in PIPELINE-RUNTIME.md
3. Retry if recoverable, otherwise trigger rollback
4. Never bypass the agent to "fix" the issue manually—the agent owns its domain

**The orchestrator's job is to orchestrate, not to replace specialized expertise.**
