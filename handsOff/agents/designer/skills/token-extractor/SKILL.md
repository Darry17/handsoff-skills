---
name: token-extractor
description: >
  Extracts all design tokens from the raw design tree — colors, typography,
  spacing, shadows, border radii — and writes them to tokens.md. Tokens from
  Figma variables take priority over inferred values. Use this skill after
  figma-reader completes and before any code generation begins.
version: 0.1.0
agent: designer
reads:
  - raw-design-tree
  - design-system-detection-result
writes:
  - tokens.md
depends_on:
  - figma-reader
---

# token-extractor

## Purpose
Extracts design tokens from the raw design tree and writes them to tokens.md.

## MANDATORY: Output Location

**tokens.md MUST be written to the repository root**, NOT inside handsoff-skills/.

```
WRONG: handsoff-skills/handsOff/shared/tokens.md
CORRECT: ./tokens.md (repository root)
```

See output-location-rules.md for full rules.

## When this skill runs
After figma-reader completes and before any code generation begins.

## Inputs
Raw-design-tree and design-system-detection-result.

## Process
1. Scan Figma variables and collection files.
2. Fallback to inferring tokens from common styles on components.
3. Format tokens into tokens.md template format.
4. Save as tokens.md in shared source-of-truth.

## Outputs
tokens.md in source-of-truth.

## Rules
- Must use priority: Variables > Explicit Styles > Inferred Scales.
- Must use human-readable token names.

## References
None.
