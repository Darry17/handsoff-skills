---
file: agent-interaction-logs.md
version: 0.2.0
purpose: Runtime logs of agent-to-agent interactions and decisions made during pipeline execution
---

# Agent Interaction Logs

This file captures all communications between agents during a pipeline run. Each entry includes the agent, timestamp, decision made, and relevant context.

## See Also

- **terminal-communication.md** — Terminal output format and user interaction rules

## Log Format

```json
{
  "run-id": "uuid",
  "entries": [
    {
      "timestamp": "ISO 8601",
      "from": "agent-name",
      "to": "agent-name | broadcast",
      "action": "decision-type",
      "decision": "description of decision",
      "context": { /* relevant context data */ }
    }
  ]
}
```

## Example Log — Build Complete

```json
{
  "run-id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
  "entries": [
    {
      "timestamp": "2026-03-29T10:00:00Z",
      "from": "orchestrator",
      "to": "broadcast",
      "action": "pipeline-start",
      "decision": "Initializing handsOff pipeline for project: e-commerce-platform",
      "context": { "project-name": "e-commerce-platform", "target-framework": "react" }
    },
    {
      "timestamp": "2026-03-29T10:00:01Z",
      "from": "orchestrator",
      "to": "researcher",
      "action": "delegate",
      "decision": "Delegating to researcher for framework and design system detection",
      "context": { "task": "detect-framework-and-design-system" }
    },
    {
      "timestamp": "2026-03-29T10:00:15Z",
      "from": "researcher",
      "to": "orchestrator",
      "action": "complete",
      "decision": "Framework detected: React 18.2.0 with TypeScript. Design system source: Figma file.",
      "context": {
        "framework": "react",
        "framework-version": "18.2.0",
        "typescript": true,
        "styling": "tailwind",
        "design-system-source": "figma"
      }
    },
    {
      "timestamp": "2026-03-29T10:00:16Z",
      "from": "orchestrator",
      "to": "designer",
      "action": "delegate",
      "decision": "Delegating to designer for Figma design extraction and component mapping",
      "context": { "task": "extract-design-and-map-components", "figma-file-url": "https://figma.com/file/..." }
    },
    {
      "timestamp": "2026-03-29T10:01:30Z",
      "from": "designer",
      "to": "orchestrator",
      "action": "complete",
      "decision": "Extracted 24 components from Figma. Mapped to 18 React components. Generated tokens.md with 156 design tokens.",
      "context": {
        "component-tree": { "atoms": 8, "molecules": 10, "organisms": 6 },
        "section-manifest": ["hero", "product-grid", "cart-sidebar", "footer"],
        "layout-map": { "hero": "flex-col", "product-grid": "grid", "cart-sidebar": "fixed-right" },
        "tokens-count": 156
      }
    },
    {
      "timestamp": "2026-03-29T10:01:31Z",
      "from": "orchestrator",
      "to": "developer",
      "action": "delegate",
      "decision": "Delegating to developer for code generation and scaffolding",
      "context": {
        "task": "generate-code-and-scaffold",
        "framework": "react",
        "component-tree": "# from designer",
        "tokens-md-path": "tokens.md"
      }
    },
    {
      "timestamp": "2026-03-29T10:03:45Z",
      "from": "developer",
      "to": "orchestrator",
      "action": "complete",
      "decision": "Generated 24 component files, 8 pages, and full project scaffold. All files written to src/",
      "context": {
        "generated-files": [
          { "path": "src/components/Button.tsx", "framework": "react", "component-name": "Button" },
          { "path": "src/components/ProductCard.tsx", "framework": "react", "component-name": "ProductCard" },
          { "path": "src/pages/Home.tsx", "framework": "react", "component-name": "Home" },
          { "path": "src/App.tsx", "framework": "react", "component-name": "App" }
        ],
        "project-structure": { "directories": ["src/components", "src/pages", "src/hooks", "src/utils"] }
      }
    },
    {
      "timestamp": "2026-03-29T10:03:46Z",
      "from": "orchestrator",
      "to": "qa",
      "action": "delegate",
      "decision": "Delegating to qa for accessibility and fidelity validation",
      "context": {
        "task": "validate-accessibility-and-fidelity",
        "target-dir": "src"
      }
    },
    {
      "timestamp": "2026-03-29T10:05:20Z",
      "from": "qa",
      "to": "orchestrator",
      "action": "feedback",
      "decision": "Found 3 critical accessibility issues and 5 fidelity issues. Need developer fixes.",
      "context": {
        "accessibility-issues": [
          { "rule": "wcag-lang", "severity": "critical", "file": "src/App.tsx", "line": 1, "message": "html lang attribute missing" },
          { "rule": "wcag-focus", "severity": "critical", "file": "src/components/Button.tsx", "line": 15, "message": "focus indicator missing" },
          { "rule": "wcag-alt", "severity": "critical", "file": "src/components/ProductCard.tsx", "line": 42, "message": "img missing alt text" }
        ],
        "fidelity-issues": [
          { "type": "color-mismatch", "severity": "warning", "expected": "#1a1a1a", "actual": "#000000", "file": "src/components/Header.tsx" },
          { "type": "spacing-mismatch", "severity": "warning", "expected": "24px", "actual": "16px", "file": "src/components/Hero.tsx" }
        ]
      }
    },
    {
      "timestamp": "2026-03-29T10:05:21Z",
      "from": "orchestrator",
      "to": "developer",
      "action": "delegate",
      "decision": "Retry 1/3: Developer must fix critical accessibility issues",
      "context": {
        "task": "fix-critical-issues",
        "retry-count": 1,
        "issues-to-fix": ["wcag-lang", "wcag-focus", "wcag-alt"]
      }
    },
    {
      "timestamp": "2026-03-29T10:07:10Z",
      "from": "developer",
      "to": "orchestrator",
      "action": "complete",
      "decision": "Fixed all 3 critical accessibility issues. Added lang='en' to html, focus styles to Button, alt props to images.",
      "context": {
        "fixed-issues": ["wcag-lang", "wcag-focus", "wcag-alt"],
        "files-modified": ["src/App.tsx", "src/components/Button.tsx", "src/components/ProductCard.tsx"]
      }
    },
    {
      "timestamp": "2026-03-29T10:07:11Z",
      "from": "orchestrator",
      "to": "qa",
      "action": "delegate",
      "decision": "Re-validating after developer fixes",
      "context": { "task": "revalidate-after-fix" }
    },
    {
      "timestamp": "2026-03-29T10:08:00Z",
      "from": "qa",
      "to": "orchestrator",
      "action": "complete",
      "decision": "All critical issues resolved. Preview artifact generated. QA validation passed.",
      "context": {
        "qa-report": {
          "critical": [],
          "warning": 5,
          "suggestion": 12,
          "passed": true
        },
        "preview-artifact": "preview-build-abc123.html"
      }
    },
    {
      "timestamp": "2026-03-29T10:08:01Z",
      "from": "orchestrator",
      "to": "seo",
      "action": "delegate",
      "decision": "Delegating to seo for semantic audit and meta tag generation",
      "context": {
        "task": "seo-optimization",
        "preview-artifact": "preview-build-abc123.html"
      }
    },
    {
      "timestamp": "2026-03-29T10:10:30Z",
      "from": "seo",
      "to": "orchestrator",
      "action": "complete",
      "decision": "SEO audit complete. Generated meta tags, sitemap.xml, robots.txt. Found 2 performance issues.",
      "context": {
        "semantic-audit-results": {
          "h1-count": 1,
          "landmarks": ["header", "main", "footer"],
          "issues": []
        },
        "meta-tags": {
          "title": "E-Commerce Platform | Premium Products",
          "description": "Shop the best products at unbeatable prices",
          "og-title": "E-Commerce Platform",
          "og-description": "Discover our premium product collection",
          "og-image": "/og-image.png"
        },
        "performance-issues": [
          { "type": "large-image", "severity": "warning", "file": "src/assets/hero.jpg", "recommendation": "Compress to WebP" }
        ],
        "sitemap-xml": "<?xml version=\"1.0\"...>",
        "robots-txt": "User-agent: *\nAllow: /",
        "seo-report": { "critical": [], "warning": 2, "suggestion": 8 }
      }
    },
    {
      "timestamp": "2026-03-29T10:10:32Z",
      "from": "orchestrator",
      "to": "broadcast",
      "action": "pipeline-complete",
      "decision": "Pipeline complete. All deliverables generated. Ready for delivery.",
      "context": {
        "status": "complete",
        "deliverables": [
          "src/ - Full React application",
          "tokens.md - Design tokens",
          "framework.md - Framework configuration",
          "preview-build-abc123.html - QA preview",
          "sitemap.xml - SEO sitemap",
          "robots.txt - SEO robots.txt"
        ]
      }
    }
  ]
}
```

## Action Types

| Action | Meaning |
|---|---|
| `pipeline-start` | Orchestrator initializing pipeline |
| `delegate` | Agent delegating task to another agent |
| `complete` | Agent completed their task successfully |
| `feedback` | QA/validator found issues requiring fixes |
| `retry` | Developer retrying after fixes |
| `pipeline-complete` | Orchestrator closing pipeline |
| `error` | Agent encountered an error |

## Decision Logging Requirements

All agents must log their decisions to this file using the shared context. Each decision must include:
1. Timestamp (ISO 8601)
2. Source agent (from)
3. Target agent(s) (to)
4. Action type
5. Decision description
6. Relevant context data

(End of file - total 180 lines)