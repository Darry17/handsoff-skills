---
file: AGENTS.md
version: 0.1.0
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
