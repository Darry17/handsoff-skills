---
name: handsOff.semantic-auditor
description: >
  Audits the generated HTML for semantic correctness — checks for a single h1,
  correct heading hierarchy, presence of landmark regions (main, nav, footer,
  aside), and document outline completeness. Use this skill after QA reports
  zero critical issues, as the first step in the SEO layer.
version: 0.1.0
agent: seo
reads:
  - generated-code-files
  - qa-report
writes:
  - semantic-audit-results
depends_on:
  - handsOff.issue-reporter
---

# handsOff.semantic-auditor

## Purpose
Audits generated HTML for semantic correctness and document structure.

## When this skill runs
After QA layer reports zero critical issues.

## Inputs
Generated-code-files and aggregated qa-report.

## Process
1. Verify exactly one h1 is present per page.
2. Ensure heading hierarchy (h1 to h6) is logical and sequential.
3. Confirm landmarks (main, nav, header, footer) are present and correct.
4. Record semantic-audit-results.

## Outputs
semantic-audit-results in shared context.

## Rules
- Double h1 or missing landmarks are critical failures in the audit.

## References
Read references/semantic-rules.md for specific criteria.
