---
name: meta-writer
description: >
  Generates and injects meta title, meta description, and Open Graph tags
  (og:title, og:description, og:image) into the generated HTML. Derives content
  from the section manifest and Figma text content. Use this skill after
  semantic-auditor completes.
version: 0.1.0
agent: seo
reads:
  - generated-code-files
  - section-manifest
  - semantic-audit-results
writes:
  - generated-code-files
  - meta-tags-report
depends_on:
  - semantic-auditor
---

# meta-writer

## Purpose
Generates and injects meta and Open Graph tags into the generated HTML.

## When this skill runs
After semantic-auditor completes.

## Inputs
Generated files, section-manifest, and semantic-audit-results.

## Process
1. Extract page title and intent from section-manifest.
2. Deduce descriptive content from hero text.
3. Generate meta tags (title, description, OG etc.).
4. Inject tags into page headers.
5. Create meta-tags-report.

## Outputs
Updated generated code and meta-tags-report.

## Rules
- Titles must be 50-60 chars; descriptions 150-160 chars.
- Open Graph tags must be present for standard sites.

## References
Read references/meta-patterns.md for naming conventions.
