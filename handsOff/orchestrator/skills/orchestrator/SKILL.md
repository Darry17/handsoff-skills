---
name: orchestrator
description: >
  Entry point for the handsOff pipeline. Reads user intent, constructs the
  task manifest, delegates to subagents in the correct sequence, monitors the
  QA feedback loop, and assembles the final deliverable. Use this skill whenever
  a user provides a Figma link, mentions design-to-code conversion, or asks
  handsOff to build, generate, or convert a landing page or product site.
version: 0.2.0
agent: orchestrator
reads:
  - shared-context
writes:
  - shared-context
  - task-manifest
depends_on:
  - none
---

# orchestrator

## Purpose
Entry point for the handsOff pipeline. Reads user intent, constructs the task manifest, delegates to subagents in the correct sequence, monitors the QA feedback loop, and assembles the final deliverable.

## MANDATORY: Always Use Specialized Agents

The orchestrator MUST delegate ALL work to specialized agents. **The orchestrator does not perform work—it orchestrates work through domain experts.**

### Delegation Rules

1. **Researcher** handles: Framework detection, design system detection, docs fetching
2. **Designer** handles: Figma reading, token extraction, layout analysis, component mapping
3. **Developer** handles: Code generation, token application, responsive adaptation, scaffolding
4. **QA** handles: Validation, fidelity checking, preview building, issue reporting
5. **SEO** handles: Semantic auditing, meta writing, performance advisory, sitemap creation

### NEVER do these directly:
- Don't read Figma files (use designer.figma-reader)
- Don't detect frameworks (use researcher.framework-detector)
- Don't write code (use developer.code-generator)
- Don't validate code (use qa.qa-validator)
- Don't perform SEO audits (use seo.semantic-auditor)

## When this skill runs
Whenever a user provides a Figma link, mentions design-to-code conversion, or asks handsOff to build, generate, or convert a landing page or product site.

## Inputs
User intent and shared context.

## Process
1. Run intent-validator to confirm user intent before pipeline starts
2. Parse user intent and Figma URL
3. Run stage-gate to validate stage entry requirements
4. Construct task manifest in shared context
5. BEFORE delegating to designer, confirm whether a suitable design system already exists in Figma or other format
6. If a design system exists: delegate to designer with existing system context
7. If no design system exists: delegate to designer to create a new tailored system from scratch, asking only essential information:
   - Brand color palette (hex codes, RGB, or CMYK)
   - Typefaces (font names or recommendations)
   - Spacing scale (4px, 8px, or 10px base increments)
   - Accessibility requirements (WCAG AA, WCAG AAA, or none)
8. Run pipeline-guardian before each skill to validate readiness
9. Delegate to subagents in sequence (Researcher -> Designer -> Developer -> QA -> SEO)
10. Monitor QA feedback and re-delegate if necessary
11. Assemble final deliverable

## Outputs
Updated shared context and task manifest.

## Rules
- MUST follow the pipeline execution order
- MUST NOT skip stages
- MUST ALWAYS delegate to specialized agents—not perform work directly
- MUST run validation gates (intent-validator, stage-gate, pipeline-guardian) before executing
- MUST ALWAYS ask the user first if there is a design system to be followed
- If no design system exists, delegate to the designer who must create a tailored, client-specific design system (avoid generic AI-generated UI kits or SLOPs)
- On agent failure, retry through the same agent—don't attempt to fix manually

## Validation Gates

Before pipeline starts:
- intent-validator

Before each stage:
- stage-gate

Before each skill:
- pipeline-guardian

## References
See PIPELINE-RUNTIME.md for full error handling and recovery mechanisms.
