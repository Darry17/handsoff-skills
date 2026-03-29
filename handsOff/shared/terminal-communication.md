---
file: terminal-communication.md
version: 0.1.0
purpose: Terminal output format and agent-to-user communication rules for pipeline execution
---

# Terminal Communication Rules

This document defines how agents communicate in the terminal during pipeline execution and how users interact directly with agents.

## Terminal Output Format

All agent communication in the terminal follows this format:

```
╔══════════════════════════════════════════════════════════════╗
║  🤖 AGENT_NAME                                          ║
║  Action: <what the agent is doing>                        ║
╚══════════════════════════════════════════════════════════════╝
<output message>
```

### Agent Prefix Colors

| Agent | Color Code | Prefix |
|---|---|---|
| orchestrator | cyan | 🔵 |
| researcher | green | 🟢 |
| designer | magenta | 🟣 |
| developer | yellow | 🟡 |
| qa | blue | 🔵 |
| seo | red | 🔴 |

## Communication Patterns

### 1. Pipeline Start

```
╔══════════════════════════════════════════════════════════════╗
║  🔵 orchestrator                                        ║
║  Initializing pipeline                                  ║
╚══════════════════════════════════════════════════════════════╝
Initializing handsOff pipeline for: <project-name>
Target framework: <framework>
Project path: <path>
```

### 2. Delegation (Orchestrator → Agent)

```
╔══════════════════════════════════════════════════════════════╗
║  🔵 orchestrator                                        ║
║  Delegating to researcher                               ║
╚══════════════════════════════════════════════════════════════╝
→ Delegating to 🟢 researcher for framework and design system detection
  Figma file: <figma-url>
```

### 3. Agent Progress (During Execution)

```
╔══════════════════════════════════════════════════════════════╗
║  🟢 researcher                                        ║
║  Analyzing Figma file                                   ║
╚══════════════════════════════════════════════════════════════╝
✓ Extracted component data from Figma
  - 24 frames analyzed
  - 8 components identified
```

### 4. Agent Complete (Agent → Orchestrator)

```
╔══════════════════════════════════════════════════════════════╗
║  🟢 researcher                                        ║
║  Task complete                                         ║
╚══════════════════════════════════════════════════════════════╝
✓ Framework detected: React 18.2.0 with TypeScript
✓ Styling: Tailwind CSS 3.x
✓ Design system: Figma (8eqbL8AOpfm01PJKldCFXu)
```

### 5. QA Feedback Loop

```
╔══════════════════════════════════════════════════════════════╗
║  🔵 qa                                                  ║
║  Validation results                                      ║
╚══════════════════════════════════════════════════════════════╝
⚠ Found 3 critical issues:
  1. [wcag-lang] src/App.tsx:1 - html lang attribute missing
  2. [wcag-focus] src/components/Button.tsx:15 - focus indicator missing
  3. [wcag-alt] src/components/ProductCard.tsx:42 - img missing alt text
```

### 6. Pipeline Complete

```
╔══════════════════════════════════════════════════════════════╗
║  🔵 orchestrator                                        ║
║  Pipeline complete                                     ║
╚══════════════════════════════════════════════════════╝
✓ Pipeline completed successfully
  Deliverables:
    - src/components/ (24 component files)
    - src/pages/ (8 page files)
    - tokens.md (156 design tokens)
    - framework.md (React 18.2.0 config)
```

---

## Agent-to-User Communication

**When agents have questions or need clarifications, they MUST ask the user directly—not through the orchestrator.**

### Question Format

```
╔══════════════════════════════════════════════════════════════╗
║  🟢 researcher                                          ║
║  Question for user                                      ║
╚══════════════════════════════════════════════════════════════╝
💬 <agent_name> needs clarification:

<question>

Options:
  1. <option-a>
  2. <option-b>
  3. <option-c>
  [Type your answer]: _
```

### User Response Format

```
user@project$ <response>
```

---

## When Agents Ask Users Directly

| Scenario | Agent | Question |
|---|---|---|
| Framework ambiguity | researcher | "You mentioned React but I detected Next.js. Which should we use?" |
| Design system missing | designer | "No design system found. Should I create a new one?" |
| Unknown component | designer | "What should this unlabeled button do on click?" |
| Accessibility conflict | qa | "Button has both aria-label and visible text. Should I keep both?" |
| SEO conflict | seo | "Multiple h1 tags found. Which is the main heading?" |
| Missing assets | developer | "Image 'hero.jpg' not found. Should I skip or use placeholder?" |

### Example: Researcher Asks Framework Question

```
╔══════════════════════════════════════════════════════════════╗
║  🟢 researcher                                          ║
║  Question for user                                      ║
╚══════════════════════════════════════════════════════════════╝
💬 researcher needs clarification:

You've indicated you want to use React, but I detected Next.js signals in your codebase:
  - package.json contains "next": ">=14.0.0"
  - pages/ directory exists

Which framework should I use for the generated code?

Options:
  1. Next.js (recommended - matches your existing codebase)
  2. React (as you requested)

user@project$ 1

✓ Using Next.js for code generation
```

### Example: Designer Asks About Unknown Component

```
╔══════════════════════════════════════════════════════════════╗
║  🟣 designer                                            ║
║  Question for user                                      ║
╚══════════════════════════════════════════════════════════════╝
💬 designer needs clarification:

I found an unlabeled button in the Hero section (figma-node: 12:34).

What should this button do?

Options:
  1. Link to /signup
  2. Link to /pricing
  3. Scroll to #contact
  4. Open modal

user@project$ 2

✓ Button will link to /pricing
```

### Example: QA Asks About Accessibility Fix

```
╔══════════════════════════════════════════════════════════════╗
║  🔵 qa                                                  ║
║  Question for user                                      ║
╚══════════════════════════════════════════════════════════════╝
💬 qa needs clarification:

Found a link with only an icon and no accessible text in Header.tsx (line 23).
The link appears to open a menu.

Options:
  1. Add aria-label="Open menu"
  2. Add text inside the link
  3. Convert to <button> element

user@project$ 1

✓ Added aria-label="Open menu"
```

---

## Agent Communication Protocol

### Message Types

| Type | Format | When |
|---|---|---|
| `delegate` | `→ Delegating to <agent> for <task>` | Orchestrator assigns task |
| `progress` | `✓ <status message>` | Agent reports progress |
| `complete` | `✓ <result>` | Agent finishes task |
| `question` | `💬 <agent> needs clarification:` | Agent needs user input |
| `error` | `✗ <error message>` | Agent encounters error |
| `warning` | `⚠ <warning>` | Agent warns of issue |
| `feedback` | `⚠ Found <n> issues:` | QA reports issues |

### Progress Indicators

```
[░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░] 0%
[████░░░░░░░░░░░░░░░░░░░░░░░░░░░░] 20%
[████████████████████░░░░░░░░░░░░░░] 60%
[█████████████████████████████] 100%
```

### Agent Self-Introduction

Each agent announces itself at the start of execution:

```
╔══════════════════════════════════════════════════════════════╗
║  🟢 researcher                                        ║
║  Ready                                              ║
╚══════════════════════════════════════════════════════════════╝
🟢 researcher — Framework & Design System Detection
   → Detecting framework from codebase signals
   → Fetching design system from Figma
   → Building source of truth (framework.md, tokens.md)
```

---

## Integration with agent-interaction-logs.md

The terminal output is logged to `agent-interaction-logs.md` in real-time:

```json
{
  "run-id": "uuid",
  "terminal-output": [
    {
      "timestamp": "ISO 8601",
      "format": "terminal",
      "from": "agent-name",
      "message": "formatted message"
    }
  ]
}
```

---

## Rules Summary

1. **All terminal output MUST use the boxed format** with agent prefix
2. **Agents MUST ask users directly** when clarifications are needed—not via orchestrator
3. **User responses are always prompted** with numbered options
4. **Progress MUST be indicated** with visual progress bars
5. **Errors and warnings MUST use** `✗` and `⚠` prefixes
6. **All terminal output is logged** to `agent-interaction-logs.md`

**The orchestrator does NOT intercept user questions. Each agent is responsible for managing its own user interactions.**