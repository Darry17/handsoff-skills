---
name: handsOff.issue-reporter
description: >
  Aggregates all issues from qa-validator and fidelity-checker into a single
  structured report with critical, warning, and suggestion severity tags. Sends
  critical issues back to the orchestrator for re-delegation to the developer.
  Ships warning and suggestion issues in the report without blocking delivery.
version: 0.1.0
agent: qa
reads:
  - accessibility-issues
  - fidelity-issues
writes:
  - qa-report
depends_on:
  - handsOff.preview-builder
---

# handsOff.issue-reporter

## Purpose
Aggregates and formats all QA issues into a unified report.

## When this skill runs
After preview-builder completes.

## Inputs
accessibility-issues and fidelity-issues.

## Process
1. Consolidate issues into a single list.
2. Filter issues by severity (critical, warning, suggestion).
3. Generate the qa-report markdown content.
4. Signal the orchestrator if critical issues are present.

## Outputs
qa-report in shared context.

## Rules
- Critical issues must explicitly block deliverable finalization.
- Report must group issues by category and file/node reference.

## References
Read references/severity-guide.md for rules on tagging.
