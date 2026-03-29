---
file: shared-context-schema.md
version: 0.1.0
purpose: Canonical schema for the handsOff shared context object
---

# Shared context schema

## Root object

```json
{
  "meta": {
    "run-id": "string — uuid for this pipeline run",
    "timestamp": "string — ISO 8601",
    "status": "string — pending | running | qa-loop | seo | complete | failed"
  },
  "detection": {
    "framework": "string — react | vue | astro | svelte | html | null",
    "framework-version": "string — semver or null",
    "styling": "string — tailwind | css-modules | plain-css | scss | null",
    "typescript": "boolean",
    "design-system-source": "string — figma | token-file | css | none",
    "codebase-exists": "boolean"
  },
  "source-of-truth": {
    "framework-md-path": "string — path to framework.md",
    "tokens-md-path": "string — path to tokens.md",
    "framework-md-generated-at": "string — ISO 8601",
    "tokens-md-generated-at": "string — ISO 8601"
  },
  "design": {
    "raw-design-tree": "object — full Figma node tree",
    "layout-map": "object — section-name → CSS intent",
    "component-tree": "object — atom/molecule/section hierarchy",
    "section-manifest": "array — ordered list of detected sections"
  },
  "code": {
    "generated-files": "array — { path, content, framework, component-name }",
    "project-structure": "object — directory tree if scaffold-writer ran"
  },
  "qa": {
    "accessibility-issues": "array — { rule, severity, file, line, message }",
    "fidelity-issues": "array — { type, severity, expected, actual, file }",
    "qa-report": "object — { critical[], warning[], suggestion[], passed }",
    "preview-artifact": "string — renderable HTML artifact content",
    "retry-count": "integer — times developer was re-delegated (max: 3)"
  },
  "seo": {
    "semantic-audit-results": "object — { h1-count, landmarks[], issues[] }",
    "meta-tags": "object — { title, description, og-title, og-description, og-image }",
    "performance-issues": "array — { type, severity, file, recommendation }",
    "sitemap-xml": "string — sitemap.xml content",
    "robots-txt": "string — robots.txt content",
    "seo-report": "object — { critical[], warning[], suggestion[] }"
  },
  "agent-logs": {
    "log-file-path": "string — path to agent-interaction-logs.md",
    "entries": "array — { timestamp, from, to, action, decision, context }",
    "latest-entry": "object — most recent log entry"
  }
}
```

## Severity levels

| Level | Meaning | Blocks delivery |
|---|---|---|
| `critical` | Must be fixed before pipeline closes | Yes |
| `warning` | Should be fixed, ships with report | No |
| `suggestion` | Optional improvement, ships with report | No |

## QA retry limit

`qa.retry-count` must not exceed 3. If the developer cannot resolve critical issues within 3 retries, the orchestrator must halt the pipeline and report the unresolved issues to the user.
