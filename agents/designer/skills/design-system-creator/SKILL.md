---
name: design-system-creator
description: >
  Creates a tailored, client-specific design system from scratch when no existing
  design system is found in the Figma file. Asks only essential, non-overwhelming
  information needed to craft a high-quality system. Avoids generic AI-generated
  UI kits or SLOPs in favor of bespoke, client-specific design tokens.
version: 0.1.0
agent: designer
reads:
  - task-manifest
writes:
  - tokens.md
  - raw-design-tree
depends_on:
  - none
---

# design-system-creator

## Purpose
Creates a tailored, client-specific design system from scratch when no existing design system is found. Asks only essential information to craft a high-quality, bespoke system.

## When this skill runs
When the orchestrator confirms that no existing design system exists in the client's Figma file.

## Inputs
Task manifest with client requirements.

## Process
1. Ask the user for essential design system information (one question at a time to avoid overwhelming):
   - Brand color palette (hex codes, RGB, or CMYK)
   - Spacing scale (4px, 8px, or 10px base increments)
   - Accessibility requirements (WCAG AA, WCAG AAA, or none)
2. For each question, provide sensible defaults with "I'll decide later" option.
3. Create the design system tokens.md with the gathered information.
4. Build the raw-design-tree with client-specific tokens.
5. Document the design rationale for each decision.

## Outputs
- tokens.md with client-specific colors, typography, spacing, and component tokens
- raw-design-tree with structured design system data

## Rules
- NEVER use generic AI-generated UI kits or SLOPs.
- ONLY ask essential questions - never overwhelm with options.
- Provide "I'll decide later" option for each question.
- Create a bespoke, client-specific system - not a copy of existing kits.
- Default to WCAG AA accessibility if not specified.

## References
- tokens.md template: shared/tokens.md.template
- component-atoms: component-mapper/references/component-atoms.md