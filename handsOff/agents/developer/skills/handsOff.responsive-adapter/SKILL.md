---
name: handsOff.responsive-adapter
description: >
  Wraps all generated code with mobile-first responsive breakpoints using the
  syntax and conventions defined in framework.md. For Tailwind outputs, applies
  sm/md/lg prefixes. For plain CSS, writes @media blocks. Use this skill after
  token-applier completes.
version: 0.1.0
agent: developer
reads:
  - framework.md
  - generated-code-files
writes:
  - generated-code-files
depends_on:
  - handsOff.token-applier
---

# handsOff.responsive-adapter

## Purpose
Applies responsive styling logic using the conventions in framework.md.

## When this skill runs
After token-applier completes.

## Inputs
framework.md and current generated files.

## Process
1. Determine the styling syntax (utility vs media queries) from framework.md.
2. Apply breakpoints (mobile, tablet, desktop) as defined in tokens/framework.
3. Implement responsive variations for layouts and components.

## Outputs
Responsive-ready code files.

## Rules
- Mobile-first approach must be default.
- Breakpoints must align with design intent from layout-map.

## References
Read references/breakpoint-guide.md for standard breakpoint values.
