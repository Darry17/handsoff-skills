---
name: setup-wizard
description: >
  Guides the user through selecting a framework and scaffolding a design system
  when detection finds neither or only one. Asks focused questions, collects
  answers, and produces the same output format as the detectors so the pipeline
  can continue. Use this skill when framework-detector or design-system-detector
  returns a partial or empty result.
version: 0.1.0
agent: researcher
reads:
  - framework-detection-result
  - design-system-detection-result
writes:
  - framework-detection-result
  - design-system-detection-result
depends_on:
  - framework-detector
  - design-system-detector
---

# setup-wizard

## Purpose
Guides the user through manual selection of a framework and scaffolding of a design system when automatic detection is inconclusive.

## When this skill runs
When framework-detector or design-system-detector returns a partial or empty result.

## Inputs
Current partial detection results.

## Process
1. Query the user for framework preference.
2. Request styling system details.
3. Ask about design system source (Figma vs configuration).
4. Synchronize user answers into structured detection results.

## Outputs
Updated framework-detection-result and design-system-detection-result.

## Rules
- Questioning must be focused and minimal.
- Must provide sensible defaults (e.g., React + Tailwind).

## References
None.
