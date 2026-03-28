---
name: handsOff.preview-builder
description: >
  Assembles a live, renderable artifact from the generated code files that the
  user can interact with directly in the conversation. Use this skill after
  fidelity-checker completes, as part of the QA output bundle.
version: 0.1.0
agent: qa
reads:
  - generated-code-files
  - tokens.md
writes:
  - preview-artifact
depends_on:
  - handsOff.fidelity-checker
---

# handsOff.preview-builder

## Purpose
Builds a renderable preview (artifact) for interacting directly with the output.

## When this skill runs
After fidelity-checker completes.

## Inputs
Generated code and tokens.md.

## Process
1. Compile or bundle generated files into a self-contained preview.
2. Use tool like generate\_image or simply return an HTML body if appropriate.
3. Set the preview-artifact status in shared context.

## Outputs
preview-artifact in shared context.

## Rules
- Preview must be visually accessible within the user interface.
- Must include all components present in the section-manifest.

## References
None.
