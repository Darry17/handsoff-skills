---
name: handsOff.layout-analyzer
description: >
  Translates Figma auto-layout, constraints, and spacing rules into explicit
  CSS intent — flex vs grid, gap values, padding rhythms, overflow behaviour.
  Produces a layout map keyed by section name. Use this skill after figma-reader
  completes, before component-mapper runs.
version: 0.1.0
agent: designer
reads:
  - raw-design-tree
writes:
  - layout-map
depends_on:
  - handsOff.figma-reader
---

# handsOff.layout-analyzer

## Purpose
Translates Figma layout rules into explicit CSS layout intent (flexbox, grid, etc.).

## When this skill runs
After figma-reader completes, before component-mapper runs.

## Inputs
Raw-design-tree.

## Process
1. Inspect Auto Layout settings (direction, gap, padding, alignment).
2. Infer if the container should be flex or grid.
3. Map constraints to specific responsive behaviors.
4. Output section-keyed layout-map.

## Outputs
layout-map in shared context.

## Rules
- Must preserve relative spacing rhythms.
- Must identify overflow behaviors where explicit.

## References
Read references/layout-patterns.md for mapping Figma rules to CSS properties.
