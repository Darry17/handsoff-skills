---
name: intent-validator
description: >
  Validates user intent before pipeline starts. Returns { valid: boolean, issues: [], confirmations-needed: [] }.
  Ensures the orchestrator understands what the user wants before delegating to agents.
  This prevents the orchestrator from misinterpreting and taking wrong actions.
version: 0.1.0
agent: orchestrator
reads:
  - user-input
  - filesystem
writes:
  - validation-result
depends_on:
  - none
---

# intent-validator

## Purpose
Confirms user intent before the pipeline starts. This is the first validation gate that ensures the orchestrator correctly understands what the user wants.

## MANDATORY: Run Before Pipeline Starts

The intent-validator MUST run before ANY stage begins. This prevents misaligned execution due to orchestrator misinterpretation.

## Validation Steps

```
1. Parse user input for:
   - Figma URL or design source
   - Framework preference (if stated)
   - Project path
   - Any specific requirements

2. Validate Figma URL:
   - If URL provided → fetch HEAD, expect 200
   - If URL inaccessible → flag as issue

3. Validate codebase:
   - If path provided → check exists
   - If path doesn't exist → flag as issue

4. Validate framework match:
   - If user states framework (e.g., "using Next.js")
   - And detection finds different signals
   - → Add to confirmations-needed

5. IF confirmations-needed.length > 0:
     - Pause pipeline
     - Present user with specific questions
     - Resume only AFTER confirmation received
     - Log confirmation in trace
   ELSE:
     - Return { valid: true }
```

## Inputs
- User input (raw request)
- Filesystem (for path validation)

## Outputs
```json
{
  "valid": boolean,
  "issues": ["string"],
  "confirmations-needed": [
    {
      "question": "string",
      "user-stated": "string",
      "detected": "string"
    }
  ],
  "validation-timestamp": "ISO 8601"
}
```

## Confirmation Scenarios

| Scenario | Action |
|----------|--------|
| User says "React" but detection finds Next.js signals | Ask: "You mentioned React but detected Next.js. Which should we use?" |
| User provides URL that returns 404 | Ask: "The URL seems inaccessible. Is it correct?" |
| User provides path that doesn't exist | Ask: "The project path doesn't exist. Should we create it?" |

## Rules
- MUST run before pipeline starts
- MUST pause for user confirmation if confirmations-needed > 0
- MUST NOT proceed until all confirmations received
- MUST log confirmations in execution trace
- This prevents orchestrator single-point-of-failure where misinterpretation cascades