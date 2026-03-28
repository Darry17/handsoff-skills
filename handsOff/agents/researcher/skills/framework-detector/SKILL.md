---
name: framework-detector
description: >
  Scans the user's codebase for framework and styling signals — package.json
  dependencies, config files, file extensions, and import patterns. Outputs a
  structured detection result used by docs-fetcher and the orchestrator. Use
  this skill at the start of every pipeline run before any code is generated.
version: 0.1.0
agent: researcher
reads:
  - codebase-signals
writes:
  - framework-detection-result
depends_on:
  - none
---

# framework-detector

## Purpose
Scans the user's codebase for framework and styling signals to determine the current project environment.

## When this skill runs
At the start of every pipeline run, before any code is generated.

## Inputs
Filesystem and codebase signals (e.g., package.json, config files).

## Process
1. Inspect package.json for dependencies.
2. Check for config files (e.g., tailwind.config.js, astro.config.mjs).
3. Search for specific patterns in source code.
4. Output structured framework-detection-result.

## Outputs
framework-detection-result in shared context.

## Rules
- Must verify specific versions.
- Must identify both framework and styling system.

## References
Read references/signal-patterns.md for known framework indicators.
