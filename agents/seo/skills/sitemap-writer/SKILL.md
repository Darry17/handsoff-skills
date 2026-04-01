---
name: sitemap-writer
description: >
  Generates a sitemap.xml and robots.txt from the detected section and page
  structure. For single-page sites, produces a single-entry sitemap. Robots.txt
  defaults allow all crawlers with a sitemap reference. Use this skill after
  performance-advisor completes.
version: 0.1.0
agent: seo
reads:
  - section-manifest
  - generated-code-files
writes:
  - sitemap.xml
  - robots.txt
depends_on:
  - performance-advisor
---

# sitemap-writer

## Purpose
Generates discovery files (sitemap.xml and robots.txt).

## When this skill runs
After performance-advisor completes.

## Inputs
section-manifest and generated files.

## Process
1. List all public URLs based on structure.
2. Generate sitemap.xml compliant file.
3. Generate robots.txt allowing all crawlers.
4. Use section-manifest for page priority/frequency hints.

## Outputs
sitemap.xml and robots.txt files.

## Rules
- robots.txt must reference the sitemap URL.

## References
Read references/sitemap-spec.md for standard XML sitemap format.
