---
file: agent-interaction-logs.md
version: 0.3.0
purpose: Centralized, searchable repository for ALL interactions—user input, orchestrator, designer, developer, SEO specialist, QA—capturing full conversation, context, decisions, and outcomes
enforcement: mandatory-logging
---

# Agent Interaction Logs

This file is the centralized, searchable repository for ALL interactions in the handsOff system. Every communication—whether from users, orchestrators, designers, developers, SEO specialists, or QA—MUST be logged in full detail. Logging all interactions is MANDATORY and enforced across all communication channels.

## Logging Enforcement

| Requirement | Enforcement |
|---|---|
| All interactions MUST be logged | Hard requirement—no interaction may proceed without logging |
| User input MUST be captured | Log before any processing begins |
| Agent decisions MUST be recorded | Log at decision point |
| Outcomes MUST be persisted | Log after task completion |
| Context MUST be preserved | Include all relevant state |

## Centralized Repository Location

All interaction logs are stored in: `handsoff-skills/HandsOff/shared/logs/` (run-id subdirectories)

## Log Format

```json
{
  "run-id": "uuid",
  "source": "user | orchestrator | designer | developer | seo | qa",
  "channel": "terminal | api | file-upload | skill-invocation",
  "entries": [
    {
      "timestamp": "ISO 8601",
      "entry-id": "uuid",
      "from": "source-name",
      "to": "target-name | broadcast",
      "channel": "communication-channel",
      "action": "action-type",
      "conversation": {
        "message": "original message content",
        "attachments": ["file urls or references"],
        "intent": "detected intent"
      },
      "context": {
        "project-state": {},
        "available-agents": [],
        "pipeline-phase": "current-phase"
      },
      "decision": "description of decision",
      "outcome": {
        "status": "success | failure | partial",
        "artifacts": [],
        "metrics": {}
      },
      "metadata": {
        "duration-ms": 0,
        "tokens-used": 0,
        "retries": 0
      }
    }
  ]
}
```

## Communication Channels

| Channel | Description | Logging Requirement |
|---|---|---|
| `terminal` | User CLI interaction | Log all input/output |
| `api` | External API calls | Log request/response |
| `file-upload` | File or Figma URL submission | Log metadata and content |
| `skill-invocation` | Inter-agent skill calls | Log invocation and result |
| `handoff` | Stage transitions between agents | Log source, target, and payload |
| `feedback` | QA/validation feedback loops | Log issues and resolutions |

## Actor Types

| Actor | Role | Logging Scope |
|---|---|---|
| `user` | End user submitting requests | All input, intent, attachments |
| `orchestrator` | Pipeline coordinator | All delegations, decisions, outcomes |
| `designer` | Design extraction | All designs analyzed, tokens extracted |
| `developer` | Code generation | All code written, modifications made |
| `seo` | SEO specialist | All audits performed, optimizations applied |
| `qa` | QA validator | All validations, issues found, resolutions |

## Example Log — Complete Interaction Record

```json
{
  "run-id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
  "source": "user",
  "channel": "terminal",
  "entries": [
    {
      "entry-id": "entry-uuid-001",
      "timestamp": "2026-03-29T10:00:00Z",
      "from": "user",
      "to": "orchestrator",
      "channel": "terminal",
      "action": "input",
      "conversation": {
        "message": "Generate an e-commerce platform from Figma design at https://figma.com/file/abc123",
        "attachments": ["https://figma.com/file/abc123"],
        "intent": "generate-application-from-design"
      },
      "context": {
        "project-state": { "initialized": false },
        "available-agents": ["orchestrator", "researcher", "designer", "developer", "qa", "seo"],
        "pipeline-phase": "idle"
      },
      "decision": "User submitted project request",
      "outcome": {
        "status": "success",
        "artifacts": [],
        "metrics": { "input-tokens": 45 }
      },
      "metadata": {
        "duration-ms": 50,
        "tokens-used": 45,
        "retries": 0
      }
    },
    {
      "entry-id": "entry-uuid-002",
      "timestamp": "2026-03-29T10:00:01Z",
      "from": "orchestrator",
      "to": "broadcast",
      "channel": "handoff",
      "action": "pipeline-start",
      "conversation": {
        "message": "Initializing handsOff pipeline for project: e-commerce-platform",
        "attachments": [],
        "intent": "initialize-pipeline"
      },
      "context": {
        "project-state": { "initialized": true, "project-name": "e-commerce-platform" },
        "available-agents": ["orchestrator", "researcher", "designer", "developer", "qa", "seo"],
        "pipeline-phase": "research"
      },
      "decision": "Pipeline initialized, delegating to researcher",
      "outcome": {
        "status": "success",
        "artifacts": [],
        "metrics": {}
      },
      "metadata": {
        "duration-ms": 100,
        "tokens-used": 120,
        "retries": 0
      }
    },
    {
      "entry-id": "entry-uuid-003",
      "timestamp": "2026-03-29T10:00:02Z",
      "from": "orchestrator",
      "to": "researcher",
      "channel": "skill-invocation",
      "action": "delegate",
      "conversation": {
        "message": "Detect framework and design system from Figma file",
        "attachments": [],
        "intent": "detect-framework-design-system"
      },
      "context": {
        "project-state": { "initialized": true, "project-name": "e-commerce-platform" },
        "available-agents": ["orchestrator", "researcher", "designer", "developer", "qa", "seo"],
        "pipeline-phase": "research"
      },
      "decision": "Delegated to researcher for framework detection",
      "outcome": {
        "status": "pending",
        "artifacts": [],
        "metrics": {}
      },
      "metadata": {
        "duration-ms": 0,
        "tokens-used": 0,
        "retries": 0
      }
    },
    {
      "entry-id": "entry-uuid-004",
      "timestamp": "2026-03-29T10:00:15Z",
      "from": "researcher",
      "to": "orchestrator",
      "channel": "skill-invocation",
      "action": "complete",
      "conversation": {
        "message": "Framework detection complete",
        "attachments": [],
        "intent": "detect-framework-design-system"
      },
      "context": {
        "project-state": { "framework": "react", "framework-version": "18.2.0", "typescript": true, "styling": "tailwind", "design-system-source": "figma" },
        "available-agents": ["orchestrator", "researcher", "designer", "developer", "qa", "seo"],
        "pipeline-phase": "design"
      },
      "decision": "Framework detected: React 18.2.0 with TypeScript. Design system source: Figma file.",
      "outcome": {
        "status": "success",
        "artifacts": ["framework.md", "tokens.md"],
        "metrics": { "tokens-identified": 156 }
      },
      "metadata": {
        "duration-ms": 13000,
        "tokens-used": 850,
        "retries": 0
      }
    },
    {
      "entry-id": "entry-uuid-005",
      "timestamp": "2026-03-29T10:01:30Z",
      "from": "designer",
      "to": "orchestrator",
      "channel": "skill-invocation",
      "action": "complete",
      "conversation": {
        "message": "Design extraction and component mapping complete",
        "attachments": [],
        "intent": "extract-design-map-components"
      },
      "context": {
        "project-state": { "components-extracted": 24, "components-mapped": 18, "tokens-count": 156 },
        "available-agents": ["orchestrator", "researcher", "designer", "developer", "qa", "seo"],
        "pipeline-phase": "development"
      },
      "decision": "Extracted 24 components from Figma. Mapped to 18 React components. Generated tokens.md with 156 design tokens.",
      "outcome": {
        "status": "success",
        "artifacts": ["tokens.md", "component-map.json"],
        "metrics": { "atoms": 8, "molecules": 10, "organisms": 6 }
      },
      "metadata": {
        "duration-ms": 74000,
        "tokens-used": 2100,
        "retries": 0
      }
    },
    {
      "entry-id": "entry-uuid-006",
      "timestamp": "2026-03-29T10:03:45Z",
      "from": "developer",
      "to": "orchestrator",
      "channel": "skill-invocation",
      "action": "complete",
      "conversation": {
        "message": "Code generation and scaffolding complete",
        "attachments": [],
        "intent": "generate-code-scaffold"
      },
      "context": {
        "project-state": { "files-generated": 32, "framework": "react" },
        "available-agents": ["orchestrator", "researcher", "designer", "developer", "qa", "seo"],
        "pipeline-phase": "qa"
      },
      "decision": "Generated 24 component files, 8 pages, and full project scaffold. All files written to src/",
      "outcome": {
        "status": "success",
        "artifacts": ["src/components", "src/pages", "src/App.tsx"],
        "metrics": { "files-created": 32, "lines-of-code": 8540 }
      },
      "metadata": {
        "duration-ms": 134000,
        "tokens-used": 4200,
        "retries": 0
      }
    },
    {
      "entry-id": "entry-uuid-007",
      "timestamp": "2026-03-29T10:05:20Z",
      "from": "qa",
      "to": "orchestrator",
      "channel": "feedback",
      "action": "feedback",
      "conversation": {
        "message": "Validation complete - issues found",
        "attachments": [],
        "intent": "validate-accessibility-fidelity"
      },
      "context": {
        "project-state": { "files-validated": 32, "issues-found": 8 },
        "available-agents": ["orchestrator", "researcher", "designer", "developer", "qa", "seo"],
        "pipeline-phase": "qa"
      },
      "decision": "Found 3 critical accessibility issues and 5 fidelity issues. Need developer fixes.",
      "outcome": {
        "status": "partial",
        "artifacts": ["qa-report.json"],
        "metrics": { "critical": 3, "warnings": 5, "suggestions": 12 }
      },
      "metadata": {
        "duration-ms": 94000,
        "tokens-used": 1800,
        "retries": 0
      }
    },
    {
      "entry-id": "entry-uuid-008",
      "timestamp": "2026-03-29T10:07:10Z",
      "from": "developer",
      "to": "orchestrator",
      "channel": "handoff",
      "action": "complete",
      "conversation": {
        "message": "Critical issues fixed",
        "attachments": [],
        "intent": "fix-critical-issues"
      },
      "context": {
        "project-state": { "issues-resolved": 3 },
        "available-agents": ["orchestrator", "researcher", "designer", "developer", "qa", "seo"],
        "pipeline-phase": "qa"
      },
      "decision": "Fixed all 3 critical accessibility issues. Added lang='en' to html, focus styles to Button, alt props to images.",
      "outcome": {
        "status": "success",
        "artifacts": ["src/App.tsx", "src/components/Button.tsx", "src/components/ProductCard.tsx"],
        "metrics": { "issues-fixed": 3 }
      },
      "metadata": {
        "duration-ms": 109000,
        "tokens-used": 1200,
        "retries": 1
      }
    },
    {
      "entry-id": "entry-uuid-009",
      "timestamp": "2026-03-29T10:08:00Z",
      "from": "qa",
      "to": "orchestrator",
      "channel": "skill-invocation",
      "action": "complete",
      "conversation": {
        "message": "Re-validation complete - all issues resolved",
        "attachments": [],
        "intent": "revalidate-after-fix"
      },
      "context": {
        "project-state": { "validation-passed": true },
        "available-agents": ["orchestrator", "researcher", "designer", "developer", "qa", "seo"],
        "pipeline-phase": "seo"
      },
      "decision": "All critical issues resolved. Preview artifact generated. QA validation passed.",
      "outcome": {
        "status": "success",
        "artifacts": ["preview-build-abc123.html", "qa-report.json"],
        "metrics": { "critical": 0, "warnings": 5, "suggestions": 12 }
      },
      "metadata": {
        "duration-ms": 49000,
        "tokens-used": 950,
        "retries": 0
      }
    },
    {
      "entry-id": "entry-uuid-010",
      "timestamp": "2026-03-29T10:10:30Z",
      "from": "seo",
      "to": "orchestrator",
      "channel": "skill-invocation",
      "action": "complete",
      "conversation": {
        "message": "SEO audit and optimization complete",
        "attachments": [],
        "intent": "seo-optimization"
      },
      "context": {
        "project-state": { "seo-optimized": true },
        "available-agents": ["orchestrator", "researcher", "designer", "developer", "qa", "seo"],
        "pipeline-phase": "delivery"
      },
      "decision": "SEO audit complete. Generated meta tags, sitemap.xml, robots.txt. Found 2 performance issues.",
      "outcome": {
        "status": "success",
        "artifacts": ["sitemap.xml", "robots.txt", "meta-tags.json"],
        "metrics": { "critical": 0, "warnings": 2, "suggestions": 8 }
      },
      "metadata": {
        "duration-ms": 150000,
        "tokens-used": 1650,
        "retries": 0
      }
    },
    {
      "entry-id": "entry-uuid-011",
      "timestamp": "2026-03-29T10:10:32Z",
      "from": "orchestrator",
      "to": "broadcast",
      "channel": "handoff",
      "action": "pipeline-complete",
      "conversation": {
        "message": "Pipeline execution complete",
        "attachments": [],
        "intent": "pipeline-completion"
      },
      "context": {
        "project-state": { "status": "complete", "deliverables-ready": true },
        "available-agents": ["orchestrator", "researcher", "designer", "developer", "qa", "seo"],
        "pipeline-phase": "complete"
      },
      "decision": "Pipeline complete. All deliverables generated. Ready for delivery.",
      "outcome": {
        "status": "success",
        "artifacts": ["src/", "tokens.md", "framework.md", "preview-build-abc123.html", "sitemap.xml", "robots.txt"],
        "metrics": { "total-duration-ms": 632000, "total-tokens-used": 12915 }
      },
      "metadata": {
        "duration-ms": 2000,
        "tokens-used": 45,
        "retries": 0
      }
    }
  ]
}
```

## Searchable Repository Structure

Logs are stored in `handsoff-skills/HandsOff/shared/logs/` with the following structure:

```
logs/
├── run-id-1/
│   ├── interaction-log.json    # Full interaction record
│   ├── conversation.json       # Message history
│   ├── context.json           # Pipeline state at each step
│   ├── decisions.json         # All decisions made
│   └── outcomes.json          # All outcomes and artifacts
├── run-id-2/
│   └── ...
└── index.json                 # Searchable index by source, actor, date
```

## Search Capabilities

The repository supports searching by:
- **run-id**: Retrieve all entries for a specific pipeline run
- **source**: Filter by user, orchestrator, designer, developer, seo, qa
- **channel**: Filter by communication channel
- **date-range**: Filter by timestamp range
- **status**: Filter by outcome status
- **artifact**: Search for specific artifacts
- **actor**: Search by specific actor involved

## Action Types

| Action | Meaning |
|---|---|
| `input` | User submitted input/request |
| `pipeline-start` | Orchestrator initializing pipeline |
| `delegate` | Agent delegating task to another agent |
| `skill-invocation` | Skill being executed |
| `complete` | Agent completed their task successfully |
| `feedback` | QA/validator found issues requiring fixes |
| `retry` | Developer retrying after fixes |
| `pipeline-complete` | Orchestrator closing pipeline |
| `error` | Agent encountered an error |

## Logging Enforcement Rules

1. **Pre-execution logging**: All user input MUST be logged before any pipeline processing begins
2. **Decision-point logging**: All agent decisions MUST be logged at the point of decision
3. **Post-execution logging**: All outcomes MUST be logged after task completion
4. **Context preservation**: All relevant context MUST be preserved in each log entry
5. **Full conversation capture**: Every message, intent, and attachment MUST be captured
6. **Channel enforcement**: All communication channels MUST log to the centralized repository
7. **No exceptions**: There are NO exceptions to logging requirements—every interaction MUST be logged

## Mandatory Logging Checklist

For every interaction, ensure the following are logged:

- [ ] entry-id (unique identifier)
- [ ] timestamp (ISO 8601)
- [ ] source (actor initiating)
- [ ] target (actor receiving)
- [ ] channel (communication channel)
- [ ] action (action type)
- [ ] conversation.message (original input)
- [ ] conversation.intent (detected intent)
- [ ] conversation.attachments (files/urls)
- [ ] context (current state)
- [ ] decision (what was decided)
- [ ] outcome.status (success/failure/partial)
- [ ] outcome.artifacts (generated files)
- [ ] outcome.metrics (quantitative results)
- [ ] metadata.duration-ms (execution time)
- [ ] metadata.tokens-used (token consumption)
- [ ] metadata.retries (retry count)

(End of file - total 180 lines)