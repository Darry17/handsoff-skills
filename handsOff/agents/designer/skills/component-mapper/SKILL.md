---
name: component-mapper
description: >
  Builds the full component hierarchy from the raw design tree — mapping Figma
  components to atoms, molecules, and sections. Produces a component tree with
  names, props, and nesting relationships. Use this skill after layout-analyzer
  completes. All component names it assigns become the canonical names used by
  the developer.
version: 0.1.0
agent: designer
reads:
  - raw-design-tree
  - layout-map
writes:
  - component-tree
depends_on:
  - layout-analyzer
---

# component-mapper

## Purpose
Builds the component hierarchy (atoms, molecules, sections) from the design tree.

## When this skill runs
After layout-analyzer completes.

## Inputs
Raw-design-tree and layout-map.

## Process
1. Group nodes into logical component candidates.
2. Identify repeated patterns (instances).
3. Name components using a consistent scheme.
4. Define relationship hierarchy (nesting).
5. Output structured component-tree.

## Outputs
component-tree in shared context.

## Rules
- Names must be canonical; no duplication.
- Component structure must be logical and modular.

## References
Read references/component-atoms.md for common component atom examples.
