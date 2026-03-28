---
name: handsOff.docs-fetcher
description: >
  Fetches official documentation for the detected framework and version using
  web_fetch. Condenses it into a versioned framework.md file that serves as
  the source of truth for all downstream agents. Use this skill immediately
  after framework-detector confirms a framework, or after setup-wizard completes.
version: 0.1.0
agent: researcher
reads:
  - framework-detection-result
writes:
  - framework.md
depends_on:
  - handsOff.framework-detector
---

# handsOff.docs-fetcher

## Purpose
Fetches official documentation for the detected framework and version, condensing it into a source of truth for all downstream agents.

## When this skill runs
Immediately after framework-detector confirms a framework, or after setup-wizard completes.

## Inputs
framework-detection-result from framework-detector or setup-wizard.

## Process
1. Use detection result to identify the correct version and URL for official docs.
2. Fetch documentation using web\_fetch.
3. Parse and condense documentation content into framework.md template format.
4. Save as framework.md in source-of-truth.

## Outputs
Condensed framework.md file.

## Rules
- Must use official documentation only.
- Must include code examples of idiomatic component structures.

## References
Read references/framework-doc-urls.md for standard framework documentation sources.
