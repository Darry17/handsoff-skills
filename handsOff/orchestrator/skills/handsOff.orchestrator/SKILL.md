---
name: handsOff.orchestrator
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

# handsOff.orchestrator

## Purpose
Entry point for the handsOff pipeline. Reads user intent, constructs the task manifest, delegates to subagents in the correct sequence, monitors the QA feedback loop, and assembles the final deliverable.

## When this skill runs
Whenever a user provides a Figma link, mentions design-to-code conversion, or asks handsOff to build, generate, or convert a landing page or product site.

## Inputs
User intent and shared context.

## Process
1. Parse user intent and Figma URL.
2. Construct task manifest in shared context.
3. Delegate to subagents in sequence (Researcher -> Designer -> Developer -> QA -> SEO).
4. Monitor QA feedback and re-delegate if necessary.
5. Assemble final deliverable.

## Outputs
Updated shared context and task manifest.

## Rules
- Must follow the pipeline execution order.
- Must not proceed skip stages unless specified.

## References
None.
