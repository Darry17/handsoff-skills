---
name: pipeline-guardian
description: >
  Validates skill readiness by checking dependencies, locks, and context readiness.
  Returns { ready: boolean, blockers: [], locks-needed: [] }.
  Runs before EVERY skill execution to prevent premature execution.
  This skill enforces that work flows through specialized agents, not around them.
version: 0.1.0
agent: orchestrator
reads:
  - meta.execution-trace
  - meta.locked-keys
  - shared-context
writes:
  - validation-result
depends_on:
  - none
---

# pipeline-guardian

## Purpose
Validates skill readiness before execution. Ensures all dependencies are met, required context exists, and no lock conflicts exist. This is the gate that prevents premature skill execution.

## MANDATORY: Always Run Before Every Skill

The pipeline-guardian MUST run before **every single skill** in the pipeline. No skill may execute without passing validation.

## Validation Protocol

```
FOR each skill scheduled to run:
  1. Get skill's declared depends_on, reads, locks
  2. FOR each depends_on:
       - Find in execution-trace where status != success
       - If not found → BLOCK, add to blockers
  3. FOR each reads key:
       - If key missing or null in context → BLOCK, add to blockers
  4. FOR each locks:
       - If key is locked by different skill → BLOCK, add to blockers
  5. IF blockers.length > 0:
       - Set meta.status: blocked
       - Return { ready: false, blockers }
     ELSE:
       - Acquire locks on behalf of skill
       - Return { ready: true }
```

## Inputs
- Execution trace from meta
- Locked keys from meta
- Shared context

## Outputs
```json
{
  "ready": boolean,
  "blockers": ["string"],
  "locks-needed": ["string"],
  "validation-timestamp": "ISO 8601"
}
```

## Error Codes

| Code | Meaning | Action |
|------|---------|--------|
| E_DEPENDENCY_MISSING | Prerequisite not completed | Block skill |
| E_CONTEXT_MISSING | Required context key null | Block skill |
| E_LOCK_CONFLICT | Cannot acquire lock | Block skill, retry |

## Rules
- MUST run before every skill without exception
- MUST NOT allow skill execution if blockers exist
- MUST validate against execution trace, not assumptions
- MUST acquire locks on behalf of skill before allowing execution