# Module Specification Template

> All MODULE.md files must follow this format

```markdown
# {Module Name}

> Version: x.x.x
> Status: active / inactive

## Responsibility
{One sentence describing what this module does}

## Input
- {file path} — {description}
- {file path} — {description}

## Output
- {file path} — {description}

## Execution Rules
1. {step 1}
2. {step 2}

## Dependencies
- {module name} — {why dependent}
- {external system} — {description}
```

---

## Module Dependency Definition Rules

**Dependency notation:**
- `Dependencies: None (base module)` — No upstream module
- `Dependencies: {module name}` — Depends on output of a module

**Example:**
```
## Dependencies
- feedback-collector — Requires feedback/*.jsonl
- memory-layer — Requires themes/ and hot.md
- None (base module)
```

**Judgment criteria:**
- If Module A needs to read Module B's output → A depends on B
- If a module can run independently → No dependencies
