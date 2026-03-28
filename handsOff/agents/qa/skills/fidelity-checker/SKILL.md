---
name: fidelity-checker
description: >
  Compares the generated code against the original Figma design for visual
  fidelity — spacing, color, typography, component names, and layout structure.
  Flags discrepancies by severity. Use this skill after qa-validator completes.
version: 0.1.0
agent: qa
reads:
  - generated-code-files
  - raw-design-tree
  - tokens.md
writes:
  - fidelity-issues
depends_on:
  - qa-validator
---

# fidelity-checker

## Purpose
Compares the generated code with the original Figma design to ensure visual fidelity.

## When this skill runs
After qa-validator completes.

## Inputs
Generated code, raw-design-tree, and tokens.md.

## Process
1. Match component names in code with design tree node labels.
2. Confirm colors, fonts, and spacing in generated code match tokens.md.
3. Observe layout hierarchy vs original visual nesting.
4. record fidelity-issues with severity tags.

## Outputs
fidelity-issues listed in shared context.

## Rules
- Discrepancies in primary colors or layout structure are critical.
- Text content should be identical unless instruction says otherwise.

## References
None.
