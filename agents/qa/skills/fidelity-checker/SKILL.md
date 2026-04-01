---
name: fidelity-checker
description: >
  STRICT PIXEL-PERFECT VALIDATION — Compares generated code against the original 
  Figma design for EXACT visual fidelity. This skill enforces absolute precision
  in spacing, color, typography, component names, layout structure, and 
  every pixel. Any deviation is flagged as critical. Use after qa-validator.
version: 0.2.0
agent: qa
reads:
  - generated-code-files
  - raw-design-tree
  - tokens.md
  - figma-file (if provided)
writes:
  - fidelity-issues
depends_on:
  - qa-validator
---

# fidelity-checker

## Purpose
Compares the generated code with the original Figma design to ensure **PIXEL-PERFECT FIDELITY**. This skill is MANDATORY strict — the code MUST look EXACTLY like the Figma file.

## CRITICAL: Pixel-Perfect Mandate

**The generated code MUST be an EXACT visual replica of the Figma design.**

- Every pixel MUST match
- Every spacing value MUST be exact (no approximations)
- Every color MUST be identical (hex, RGB, HSL values)
- Every typography value MUST match exactly (font, size, weight, line-height, letter-spacing)
- Every border radius MUST be exact
- Every shadow value MUST be exact
- Every component name MUST match the design tree node labels
- Text content MUST be identical unless explicitly instructed otherwise

## When this skill runs
After qa-validator completes.

## Inputs
Generated code, raw-design-tree, tokens.md, and optionally the Figma file.

## Process

### STRICT Spacing Validation
1. Extract ALL spacing values from Figma (margins, padding, gaps between elements)
2. Convert to consistent units (typically pixels or rems)
3. Compare each spacing value in generated code against Figma
4. Flag ANY discrepancy as CRITICAL — no tolerance for "close enough"

### STRICT Color Validation
1. Compare every color (hex, RGB, HSL) in generated code vs tokens.md
2. Verify against Figma actual values
3. ANY deviation = CRITICAL

### STRICT Typography Validation
1. Verify font-family, font-size, font-weight, line-height, letter-spacing, text-transform
2. Compare against Figma text styles
3. ANY deviation = CRITICAL

### STRICT Layout Validation
1. Compare layout hierarchy (Flexbox/Grid structure) vs Figma frames
2. Verify exact positioning, alignment, distribution
3. ANY discrepancy = CRITICAL

### STRICT Component Validation
1. Match component names EXACTLY with design tree node labels
2. Verify component props match Figma properties
3. ANY mismatch = CRITICAL

### STRICT Visual Comparison (if Figma file provided)
1. Take exact measurements from Figma frames
2. Compare each measured value against implementation
3. No tolerance — PIXEL-PERFECT means EXACT MATCH

## Outputs
fidelity-issues listed in shared context with CRITICAL severity for ALL discrepancies.

## STRICT Rules

### Zero-Tolerance Policy
- **NO tolerance for ANY spacing deviation** — 1px off is CRITICAL
- **NO tolerance for ANY color deviation** — wrong shade is CRITICAL  
- **NO tolerance for ANY typography deviation** — wrong font-weight is CRITICAL
- **NO tolerance for ANY layout deviation** — wrong alignment is CRITICAL
- **NO tolerance for component name mismatches**
- **NO tolerance for text content differences**

### Blocking Criteria
MUST block delivery for:
- ANY spacing mismatch (even 1px)
- ANY color mismatch
- ANY typography mismatch
- ANY layout structure mismatch
- ANY component name mismatch
- ANY missing interactive states

### Validation Checklist
For each element, verify:
- [ ] Exact pixel spacing (top, right, bottom, left)
- [ ] Exact color values
- [ ] Exact typography values
- [ ] Exact border radius
- [ ] Exact shadow values
- [ ] Exact component name
- [ ] Exact text content
- [ ] Exact layout structure
- [ ] Exact hover/active/disabled states (if in Figma)

## References
None.
