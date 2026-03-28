---
name: handsOff.token-applier
description: >
  Injects design token values from tokens.md into generated code — as Tailwind
  theme extensions, CSS custom properties, SCSS variables, or framework-specific
  theme objects depending on what framework.md specifies. Use this skill after
  code-generator completes.
version: 0.1.0
agent: developer
reads:
  - tokens.md
  - framework.md
  - generated-code-files
writes:
  - generated-code-files
depends_on:
  - handsOff.code-generator
---

# handsOff.token-applier

## Purpose
Applies design token values to the generated code files.

## When this skill runs
After code-generator completes.

## Inputs
tokens.md, framework.md, and current generated files.

## Process
1. Identify the project's styling approach from framework.md.
2. Replace hardcoded placeholders or inject definitions (like theme.config) for tokens defined in tokens.md.
3. Ensure token names are used exclusively for styles.

## Outputs
Updated generated code files with token mappings.

## Rules
- Mapping must be consistent.
- Must result in no untracked hex/px values.

## References
None.
