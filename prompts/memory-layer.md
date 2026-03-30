# Memory Layer Prompt

> Used by memory-layer module to manage tiered memory (HOT/WARM/COLD)

---

```
You need to manage tiered memory of rules, maintain hot.md, and handle upgrades/downgrades.

## Input

- `data/themes/{topic}/*.md` — Rules written by distill-classifier (containing frontmatter: occurrences, last_seen)

## Output

- `data/hot.md` — HOT Layer active rules (occurrences ≥ 3)
- `data/archive/` — COLD Layer archived content (last_seen > 90 days)

---

## Core Rules

### Upgrade to HOT

Condition: occurrences ≥ 3

Execute:
1. Find rules with occurrences ≥ 3 from themes/
2. Write to `data/hot.md` in the following format:

```markdown
## [R-YYYYMMDD-NNN] {Rule Title}

- Source: themes/{topic}/{file}.md
- Occurrences: {occurrences}
- Thinking Principle: {one sentence}

{Surface Rule}
```

### Downgrade

Condition: last_seen exceeds 30 days

Execute:
1. Remove from hot.md
2. Keep in themes/ (WARM Layer)

### Archive

Condition: last_seen exceeds 90 days

Execute:
1. Move from themes/ to `data/archive/themes/`

## hot.md Limit

- Maximum 100 lines
- When exceeded, remove oldest rules (lowest occurrences or earliest last_seen)

## Execution Steps

1. Scan all .md files under `data/themes/`
2. Read frontmatter: occurrences, last_seen
3. Process upgrade/downgrade/archive according to above rules
4. Update hot.md
5. Write checkpoint

## Output

- `data/hot.md` — HOT Layer active rules
- `data/archive/` — COLD Layer archived content
