---
name: handsOff.figma-reader
description: >
  Reads a Figma file via the Figma MCP as the primary method, falling back to
  a Figma share URL if MCP is unavailable. Returns the raw design tree for all
  downstream designer skills. Use this skill whenever a Figma link or file is
  provided and design parsing needs to begin.
version: 0.1.0
agent: designer
reads:
  - figma-file-or-url
writes:
  - raw-design-tree
depends_on:
  - none
---

# handsOff.figma-reader

## Purpose
Reads a Figma file (via MCP or share URL) and returns the raw design tree.

## When this skill runs
Whenever a Figma link or file is provided and design parsing needs to begin.

## Inputs
Figma share URL or Figma file identifier.

## Process
1. Attempt to connect via Figma MCP to fetch node tree.
2. Fallback to Figma Web API if MCP is unavailable.
3. Parse and return the raw-design-tree.

## Outputs
Raw design tree in shared context.

## Rules
- Must include all node properties required for layout analysis.

## References
Read references/figma-mcp-guide.md for instructions on using the Figma MCP.
