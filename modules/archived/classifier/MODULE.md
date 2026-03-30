# classifier

> Version: 3.0.0
> Status: archived (functionality merged)

## Responsibility
Classify and label, write to themes/, count occurrences.

## Input
- `data/corrections.md` — Correction records
- `data/feedback/*.jsonl` — Feedback data (contains multi-dimensional fields)

## Output
- `data/themes/{theme}/*.md` — Stored by theme, contains frontmatter labels

## Execution Rules
1. Read corrections.md and feedback/
2. Determine theme/domain/project labels
3. Check if same content already exists in themes/
4. Exists -> occurrences +1, update last_seen
5. Does not exist -> create new file, occurrences = 1
6. Update theme index

## Dependencies
- feedback-collector — Requires corrections.md and feedback/
