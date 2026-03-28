---
name: code-generator
description: >
  Generates framework-idiomatic code from the designer's component tree. Reads
  framework.md before writing a single line — this is the only allowed source
  of truth for patterns and conventions. Outputs one file per component for
  component-based frameworks. Use this skill after all designer skills complete
  and framework.md and tokens.md are confirmed present.
version: 0.1.0
agent: developer
reads:
  - framework.md
  - tokens.md
  - component-tree
  - section-manifest
  - layout-map
writes:
  - generated-code-files
depends_on:
  - section-detector
  - docs-fetcher
---

# code-generator

## Purpose
Generates framework-idiomatic code based on the component tree and framework.md.

## When this skill runs
After designer skills complete and framework.md/tokens.md are available.

## Inputs
Component tree, section manifest, layout map, framework.md, and tokens.md.

## Process
1. Read framework.md for component patterns (structure, naming, style system).
2. Generate code for each entry in component-tree.
3. Align implementation with the section manifest order.
4. Output one file per component (or equivalent).

## Outputs
Generated file objects in shared context.

## Rules
- Must strictly follow framework.md conventions.
- Must not use any styling values outside of tokens.md.

## References
Read references/output-contract.md for quality requirements.
