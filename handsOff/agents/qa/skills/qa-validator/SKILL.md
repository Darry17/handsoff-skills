---
name: qa-validator
description: >
  Validates generated code for accessibility compliance — WCAG AA color contrast,
  ARIA roles and landmark regions, heading hierarchy, and image alt text. Produces
  a list of issues keyed by severity. Use this skill after responsive-adapter
  completes and before fidelity-checker runs.
version: 0.1.0
agent: qa
reads:
  - generated-code-files
  - tokens.md
  - output-contract.md
writes:
  - accessibility-issues
depends_on:
  - responsive-adapter
---

# qa-validator

## Purpose
Validates the generated code for accessibility compliance (WCAG AA).

## When this skill runs
After responsive-adapter completes and before fidelity-checker runs.

## Inputs
Generated code files, tokens.md, and output-contract.md.

## Process
1. Scan generated files for semantic HTML tags.
2. Check for missing labels and ARIA roles.
3. Validate color contrast ratios using token values.
4. Compare output against output-contract.md checklist.
5. Record accessibility-issues.

## Outputs
accessibility-issues listed in shared context.

## Rules
- Must block delivery for critical accessibility failures (e.g., missing labels on buttons).

## References
Read references/wcag-aa-rules.md for specific criteria.
