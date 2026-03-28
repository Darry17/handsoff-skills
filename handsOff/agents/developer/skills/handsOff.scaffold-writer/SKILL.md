---
name: handsOff.scaffold-writer
description: >
  Bootstraps the full file and folder structure for the detected framework when
  no codebase exists. Creates entry files, config stubs, component directories,
  and an initial index. Uses framework.md as the only source of truth for the
  correct file structure. Use this skill when framework-detector or setup-wizard
  indicates no existing codebase was found.
version: 0.1.0
agent: developer
reads:
  - framework.md
  - framework-detection-result
writes:
  - project-file-structure
depends_on:
  - handsOff.docs-fetcher
---

# handsOff.scaffold-writer

## Purpose
Bootstraps the project file and folder structure based on the framework.md source of truth.

## When this skill runs
When framework-detector or setup-wizard indicates no existing codebase is present.

## Inputs
framework.md and detection results.

## Process
1. Match framework.md requirements with directory structure patterns.
2. Initialize directory and necessary configuration files (stubs).
3. Update project-file-structure in shared context.

## Outputs
Updated project-file-structure and initialized directories.

## Rules
- Must not overwrite existing files without user confirmation.
- Structure must be production-ready and idiomatic.

## References
Read references/file-structures.md for typical structure patterns.
