---
file: PIPELINE-RUNTIME.md
version: 0.2.0
purpose: Runtime enforcement and error recovery for handsOff pipeline
---

# Pipeline Runtime Plan

This document addresses all identified workflow concerns in a single cohesive system.

## Orchestrator Mandate: Always Use Specialized Agents

The orchestrator's core responsibility is **delegation, not execution**. The orchestrator MUST always utilize the specialized agents (researcher, designer, developer, qa, seo) to perform work rather than attempting to execute skills directly.

### Orchestrator Rules

1. **Delegate ALL work** — Never execute skills directly; always invoke the appropriate agent
2. **Respect agent boundaries** — Each agent owns its domain; don't bypass them
3. **Invoke in dependency order** — Follow the skill dependency graph
4. **Enforce validation gates** — Run pipeline-guardian, stage-gate, and intent-validator
5. **Handle failures through agents** — Don't manually fix; retry through the agent

### Anti-Patterns (Strictly Prohibited)

- Orchestrator directly reading Figma files
- Orchestrator directly writing code
- Orchestrator skipping stages to "save time"
- Orchestrator performing validation directly instead of delegating to qa
- Orchestrator executing SEO tasks directly instead of delegating to seo

---

## Problems Addressed

| # | Concern | Risk |
|---|---------|------|
| 1 | Dependency ordering enforcement | Skills run before prerequisites exist |
| 2 | No atomic transactions | Corrupt state on QA failure |
| 3 | Bidirectional writes | Race conditions in concurrent execution |
| 4 | Missing error handling | No recovery paths |
| 5 | Orchestrator single point of failure | Misinterpretation cascades |
| 6 | No skill readiness validation | Premature execution |
| 7 | No execution engine | Pipeline can't run |

---

## Solution: Pipeline State Machine

### Updated Meta Object

```json
{
  "meta": {
    "run-id": "uuid",
    "timestamp": "ISO 8601",
    "status": "pending | initializing | running | blocked | qa-loop | rollback | seo | complete | failed",
    "current-stage": "orchestrator | researcher | designer | developer | qa | seo",
    "stage-attempts": { "developer": 0, "qa": 0 },
    "error-source": "string | null",
    "rollback-point": "string — stage name",
    "locked-keys": [],
    "execution-trace": []
  }
}
```

### Stage Status Values

| Status | Meaning | Can Proceed |
|--------|---------|-------------|
| `pending` | Not started | No |
| `initializing` | Setting up context | No |
| `running` | Skill executing | No |
| `blocked` | Dependency missing | No |
| `qa-loop` | In retry cycle | Yes (limited) |
| `rollback` | Reverting state | No |
| `seo` | Final enrichment | Yes |
| `complete` | Success | N/A |
| `failed` | Unrecoverable | N/A |

---

## 1. Dependency Enforcement

### Skill Dependency Graph

```
orchestrator
  └─> researcher
        ├─> framework-detector
        ├─> docs-fetcher
        └─> design-system-detector
  └─> designer
        ├─> figma-reader
        ├─> token-extractor
        ├─> layout-analyzer
        ├─> component-mapper
        └─> section-detector
  └─> developer
        ├─> code-generator (requires: [framework.md, tokens.md, component-tree, section-manifest])
        ├─> token-applier (requires: [generated-code-files, tokens.md])
        ├─> responsive-adapter (requires: [generated-code-files])
        └─> scaffold-writer (requires: [framework.md])
  └─> qa
        ├─> qa-validator (requires: [generated-code-files])
        ├─> fidelity-checker (requires: [generated-code-files])
        ├─> preview-builder (requires: [generated-code-files])
        └─> issue-reporter (requires: [qa-report])
  └─> seo
        ├─> semantic-auditor (requires: [generated-code-files])
        ├─> meta-writer (requires: [semantic-audit-results])
        ├─> performance-advisor (requires: [generated-code-files])
        └─> sitemap-writer (requires: [section-manifest])
```

### Validator Rule

Before any skill executes, the runtime MUST validate:

1. All `depends_on` skills have `status: complete` in execution trace
2. All `reads` keys exist in shared context with non-null values
3. No `locked-keys` conflict with skill's `writes`

If validation fails → set `meta.status: blocked`, record missing dependencies in trace.

---

## 2. Write Locking Mechanism

### Lock Protocol

```json
{
  "meta": {
    "locked-keys": ["generated-code-files"],
    "locks-held-by": { "meta-writer": ["generated-code-files"] }
  }
}
```

### Lock Rules

1. Skill requests locks BEFORE writing by declaring `locks` in SKILL.md
2. Runtime grants locks in order received (FIFO)
3. Held locks are released AFTER skill completes successfully
4. Failed skills keep locks held for rollback to release
5. No skill may write to a locked key without holding its lock

### Modified SKILL.md Frontmatter

```yaml
---
name: meta-writer
locks:
  - generated-code-files
reads:
  - generated-code-files
  - section-manifest
writes:
  - generated-code-files
  - meta-tags-report
---
```

---

## 3. Error States and Recovery

### Per-Skill Error Output

Each skill MUST output a standardized result:

```json
{
  "skill": "code-generator",
  "status": "success | blocked | failed",
  "error": { "code": "E_DEPENDENCY_MISSING", "message": "", "recoverable": true },
  "outputs": {},
  "stage-complete-at": "ISO 8601"
}
```

### Error Codes

| Code | Meaning | Recoverable |
|------|---------|-------------|
| `E_DEPENDENCY_MISSING` | Prerequisite not satisfied | Yes |
| `E_DEPENDENCY_FAILED` | Prerequisite failed | No (requires rollback) |
| `E_CONTEXT_MISSING` | Required context key null | Yes |
| `E_LOCK_CONFLICT` | Cannot acquire lock | Yes (retry) |
| `E_OUTPUT_INVALID` | Output failed validation | Yes (retry) |
| `E_INTERNAL` | Unexpected error | No |

### Recovery Actions

| Error | Action |
|-------|--------|
| `E_DEPENDENCY_MISSING` | Block and request prerequisite |
| `E_DEPENDENCY_FAILED` | Trigger rollback to last checkpoint |
| `E_CONTEXT_MISSING` | Request missing data from previous stage |
| `E_LOCK_CONFLICT` | Wait 1s, retry (max 3) |
| `E_OUTPUT_INVALID` | Report to skill, allow retry |
| `E_INTERNAL` | Mark failed, trigger rollback |

---

## 4. Rollback Mechanics

### Checkpoint Strategy

Each stage creates a checkpoint BEFORE executing:

```json
{
  "checkpoints": {
    "developer": {
      "created-at": "ISO 8601",
      "snapshot": { "code": { "generated-files": [...] } }
    }
  }
}
```

### Rollback Rules

1. Checkpoints created at stage entry (before first skill runs)
2. On stage failure → restore to most recent checkpoint
3. Increment `meta.stage-attempts.<stage>`
4. If attempts exceed max → advance to error handling
5. Reset locks and clear blocked status after rollback
6. Log rollback in execution trace with reason

### Stage Max Retries

| Stage | Max Retries |
|-------|------------|
| `developer` | 3 |
| `qa` | 3 |
| All others | 1 |

---

## 5. Skill Readiness Validator

### New Skill: pipeline-guardian

Location: `orchestrator/skills/pipeline-guardian/SKILL.md`

```yaml
---
name: pipeline-guardian
description: >
  Validates skill readiness by checking dependencies, locks, and context
  readiness. Returns { ready: boolean, blockers: [], locks-needed: [] }.
  Runs before EVERY skill execution to prevent premature execution.
version: 0.1.0
agent: orchestrator
reads:
  - meta.execution-trace
  - meta.locked-keys
  - shared-context
writes:
  - validation-result
depends_on: []
---
```

### Validation Protocol

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

---

## 6. Stage Boundary Validation

### Input Contract Per Stage

Each stage MUST validate inputs before executing ANY skill:

| Stage | Required Context Keys |
|-------|---------------------|
| `researcher` | `detection` (may be populated) |
| `designer` | `detection.framework`, `detection.design-system-source` |
| `developer` | `source-of-truth.framework-md-path`, `source-of-truth.tokens-md-path`, `design.component-tree`, `design.section-manifest`, `design.layout-map` |
| `qa` | `code.generated-files` |
| `seo` | `code.generated-files`, `qa.qa-report` (if passed) |

### Validation Skill: stage-gate

```yaml
---
name: stage-gate
description: >
  Validates stage entry requirements. Returns { allowed: boolean, missing: [] }.
  Runs BEFORE stage execution begins.
version: 0.1.0
agent: orchestrator
reads:
  - meta.current-stage
  - shared-context
writes:
  - stage-gate-result
---
```

---

## 7. Execution Trace

### Trace Structure

```json
{
  "meta": {
    "execution-trace": [
      {
        "run-id": "uuid",
        "skill": "orchestrator",
        "status": "complete",
        "started-at": "ISO 8601",
        "completed-at": "ISO 8601",
        "outputs": { "task-manifest": {} }
      },
      {
        "run-id": "uuid",
        "skill": "framework-detector",
        "status": "complete",
        "started-at": "ISO 8601",
        "completed-at": "ISO 8601",
        "outputs": { "detection": {} }
      }
    ]
  }
}
```

### Trace Uses

1. **Debugging** — Replay pipeline to find failure point
2. **Dependency resolution** — Check if prerequisite completed
3. **Audit** — Document what ran and when
4. **Resume** — Continue from last checkpoint after crash

---

## 8. Orchestrator Redundancy

### Intent Validation

Before executing pipeline, orchestrator MUST:

1. Extract Figma URL from user input
2. Validate URL is accessible (fetch HEAD, expect 200)
3. Check codebase exists in provided path
4. Confirm framework signals match user's stated framework
5. If mismatch → prompt user to confirm

### New Skill: intent-validator

```yaml
---
name: intent-validator
description: >
  Validates user intent before pipeline starts. Returns { valid: boolean, issues: [], confirmations-needed: [] }.
version: 0.1.0
agent: orchestrator
reads:
  - user-input
  - filesystem
writes:
  - validation-result
---
```

### Confirmation Workflow

```
IF intent-validator returns confirmations-needed:
  - Pause pipeline
  - Present user with specific questions
  - Resume only AFTER confirmation received
  - Log confirmation in trace
```

---

## Implementation Priority

| Order | Task | Files to Modify |
|-------|------|----------------|
| 1 | Update shared-context-schema.md | `shared/shared-context-schema.md` |
| 2 | Add error handling to skills | All `SKILL.md` files |
| 3 | Create pipeline-guardian | New file |
| 4 | Create stage-gate | New file |
| 5 | Create intent-validator | New file |
| 6 | Add locks to skills | All `SKILL.md` files |
| 7 | Add checkpoint logic | Orchestrator skill |

---

## Summary

This plan addresses all 7 concerns:

| Concern | Solution |
|---------|----------|
| Dependency ordering | pipeline-guardian validates before every skill |
| Atomic transactions | Checkpoints at stage entry, rollback on failure |
| Bidirectional writes | Lock mechanism prevents conflicts |
| Error handling | Standardized error codes with recovery actions |
| Single point failure | intent-validator confirms before running |
| Skill readiness | Stage-gate validates entry requirements |
| No execution engine | This spec defines the engine behavior |

The runtime MUST implement these rules to ensure pipeline integrity. All skill authors MUST use the updated frontmatter and error output format.