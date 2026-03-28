---
name: handsOff.design-system-detector
description: >
  Reads Figma variables, token files, theme configs, and existing CSS custom
  properties to determine whether a design system exists. Outputs a structured
  token detection result. Use this skill at the start of every pipeline run,
  in parallel with framework-detector.
version: 0.1.0
agent: researcher
reads:
  - figma-variables
  - token-files
  - existing-css
writes:
  - design-system-detection-result
depends_on:
  - none
---

# handsOff.design-system-detector

## Purpose
Determines whether a design system exists by reading Figma variables, token files, and theme configs.

## When this skill runs
At the start of every pipeline run, usually in parallel with framework-detector.

## Inputs
Figma variables, local token files, theme configurations, and existing CSS custom properties.

## Process
1. Extract variables from Figma raw intent.
2. Search for theme/token files in the codebase.
3. Parse CSS custom properties from existing stylesheets.
4. Synthesize detection results.

## Outputs
design-system-detection-result in shared context.

## Rules
- Must detect sources for color, typography, and spacing.

## References
Read references/token-sources.md for common locations of design tokens.
