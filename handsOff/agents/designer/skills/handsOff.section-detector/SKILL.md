---
name: handsOff.section-detector
description: >
  Identifies landing page sections present in the Figma design — hero, navbar,
  features, testimonials, pricing, FAQ, CTA, footer — and produces a section
  manifest with order, content type, and layout hints. Use this skill after
  component-mapper completes.
version: 0.1.0
agent: designer
reads:
  - component-tree
  - raw-design-tree
writes:
  - section-manifest
depends_on:
  - handsOff.component-mapper
---

# handsOff.section-detector

## Purpose
Identifies typical landing page sections from the designs.

## When this skill runs
After component-mapper completes.

## Inputs
Component-tree and raw-design-tree.

## Process
1. Scan for section boundaries (top-level frames).
2. Use content analysis to label sections (e.g., hero, footer).
3. Define page-wide order and hierarchy.
4. Output section-manifest.

## Outputs
section-manifest in shared context.

## Rules
- Identification must be type-based (content signals).
- Manifest order must reflect visual order.

## References
Read references/section-patterns.md for content signals for specific sections.
