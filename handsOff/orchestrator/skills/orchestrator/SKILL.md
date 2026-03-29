---
name: orchestrator
description: >
  Entry point for the handsOff pipeline. Reads user intent, constructs the
  task manifest, delegates to subagents in the correct sequence, monitors the
  QA feedback loop, and assembles the final deliverable. Use this skill whenever
  a user provides a Figma link, mentions design-to-code conversion, or asks
  handsOff to build, generate, or convert a landing page or product site.
version: 0.1.0
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

## When this skill runs
Whenever a user provides a Figma link, mentions design-to-code conversion, or asks handsOff to build, generate, or convert a landing page or product site.

## Inputs
User intent and shared context.

## Process
1. Parse user intent and Figma URL.
2. Construct task manifest in shared context.
3. BEFORE delegating to designer, confirm whether a suitable design system already exists in Figma or other format.
4. If a design system exists: delegate to designer with existing system context.
5. If no design system exists: delegate to designer to create a new tailored system from scratch, asking only essential information:
   - Brand color palette (hex codes, RGB, or CMYK)
   - Typefaces (font names or recommendations)
   - Spacing scale (4px, 8px, or 10px base increments)
   - Accessibility requirements (WCAG AA, WCAG AAA, or none)
6. Delegate to subagents in sequence (Researcher -> Designer -> Developer -> QA -> SEO).
7. Monitor QA feedback and re-delegate if necessary.
8. Assemble final deliverable.

## Outputs
Updated shared context and task manifest.

## Rules
- Must follow the pipeline execution order.
- Must not proceed skip stages unless specified.
- MUST ALWAYS ask the user first if there is a design system to be followed.
- If no design system exists, delegate to the designer who must create a tailored, client-specific design system (avoid generic AI-generated UI kits or SLOPs).

## References
None.
