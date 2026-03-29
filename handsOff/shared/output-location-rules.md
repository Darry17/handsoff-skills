---
file: output-location-rules.md
version: 0.1.0
purpose: Rules for where generated files are written during pipeline execution
---

# Output Location Rules

This document defines where all generated files MUST be written. **Generated files must NEVER be written inside the handsOff folder.**

---

## What is "Repository Root"?

**Repository root = the project directory being worked on** (e.g., `test/`), NOT `handsoff-skills/`.

When handsOff runs on project `test/`, the output goes to `test/roots/`:
- `test/tokens.md`
- `test/framework.md`
- `test/shared-context.json`

The `handsoff-skills/` folder is the skill library—never write generated output there.

---

## MANDATORY: Repository Root Output

**ALL generated source-of-truth files must be written to the repository root**, NOT inside the `handsoff-skills/` folder.

### Required Output Locations

| File | Location | Reason |
|---|---|---|
| `tokens.md` | `<repo-root>/tokens.md` | Source of truth for design tokens |
| `framework.md` | `<repo-root>/framework.md` | Source of truth for framework config |
| `shared-context.json` | `<repo-root>/shared-context.json` | Runtime context |
| `agent-interaction-logs.md` | `<repo-root>/handsOff-logs.md` | Agent communication log |

### Prohibited Locations

```
WRONG (never use):
  handsoff-skills/handsOff/shared/tokens.md
  handsoff-skills/handsOff/shared/framework.md
  handsoff-skills/handsOff/shared/shared-context.json

CORRECT (always use):
  tokens.md
  framework.md
  shared-context.json
  handsOff-logs.md
```

---

## File Reference Resolution

When agents reference these files, they MUST look at the **project root** (the directory handsOff was invoked on) first:

```python
# WRONG - hardcoded path to handsoff-skills
read("handsoff-skills/handsOff/shared/tokens.md")

# CORRECT - resolve from project root (e.g., test/)
def resolve_path(filename, project_root):
    root_paths = [
        f"{project_root}/tokens.md",
        f"{project_root}/framework.md", 
        f"{project_root}/shared-context.json",
        f"{project_root}/handsOff-logs.md"
    ]
    for path in root_paths:
        if exists(path):
            return path
    # Legacy location (deprecated)
    return f"handsoff-skills/handsOff/shared/{filename}"
```

### Path Resolution Priority

1. **Project root** (`<project>/tokens.md`)
2. **Legacy handsoff location** (deprecated, with warning)

---

## Shared Context Schema Reference

The `source-of-truth` section in shared-context.json should reference root paths:

```json
{
  "source-of-truth": {
    "framework-md-path": "framework.md",
    "tokens-md-path": "tokens.md",
    "framework-md-generated-at": "ISO 8601",
    "tokens-md-generated-at": "ISO 8601"
  }
}
```

**Paths should be relative** (e.g., `tokens.md`) NOT absolute or nested paths.

---

## Agent SKILL.md Updates

Update skill references to use root paths:

### researcher/framework-detector/SKILL.md

```yaml
writes:
  - framework.md  # Write to repository root
  - tokens.md    # Write to repository root (if design tokens generated)
```

### designer/token-extractor/SKILL.md

```yaml
writes:
  - tokens.md  # Write to repository root
```

### orchestrator/orchestrator/SKILL.md

```yaml
writes:
  - shared-context.json  # Write to repository root
  - handsOff-logs.md # Write to repository root
```

---

## Validation

Before pipeline completes, the orchestrator MUST verify:

1. `tokens.md` exists at project root
2. `framework.md` exists at project root
3. No generated files exist inside `handsoff-skills/` folder

If files exist in wrong location:
- Move them to correct location
- Log warning about incorrect placement
- Update references

---

## Migration from Legacy Locations

For existing projects that reference the old locations:

```
1. Check if files exist at root
2. If not, check legacy location
3. If found at legacy location:
   - Copy to root
   - Update references in shared-context.json
   - Log migration
4. If not found at either:
   - Regenerate from source
```

---

## Summary

| Rule | Description |
|---|---|
| NEVER write to handsoff-skills/ | All outputs go to repo root |
| Use relative paths | `./tokens.md` not full paths |
| Verify on completion | Confirm files at root before close |
| Migrate legacy | Move old files if found |