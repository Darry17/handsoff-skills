---
name: performance-advisor
description: >
  Identifies performance issues in the generated code that affect search ranking
  — images missing width/height attributes, images below the fold missing lazy
  loading, and render-blocking script or style patterns. Produces a report with
  specific file and line references. Use this skill after meta-writer completes.
version: 0.1.0
agent: seo
reads:
  - generated-code-files
writes:
  - performance-issues
depends_on:
  - meta-writer
---

# performance-advisor

## Purpose
Identifies and reports performance-related SEO issues in generated code.

## When this skill runs
After meta-writer completes.

## Inputs
Generated-code-files.

## Process
1. Check for width/height on all images.
2. Verify lazy loading for off-screen assets.
3. Scan for render-blocking scripts without async/defer.
4. record performance-issues report.

## Outputs
performance-issues report.

## Rules
- Recommendations must include file/line reference.

## References
Read references/performance-rules.md for criteria.
