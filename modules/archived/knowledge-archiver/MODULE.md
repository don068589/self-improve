# knowledge-archiver

> Version: 1.2.0
> Status: archived (functionality merged)

## Responsibility
Process knowledge content that cannot be solidified, store in external knowledge base.

## Input
- `data/corrections.md` — Source of error knowledge points
- `data/reflections.md` — Source of lessons learned

## Output
- `/path/to/learned/errors{theme}.md` — Error knowledge points
- `/path/to/learned/lessons{theme}.md` — Lessons learned
- `/path/to/learned/methodologies{theme}.md` — Methodologies
- `/path/to/learned/business{theme}.md` — Business insights
- `/path/to/learned/innovations{theme}.md` — Innovation points
- `/path/to/learned/articles{theme}.md` — Long-form articles

## Execution Rules
1. Determine if content meets solidification conditions (occurrences >= 3)
2. Does not meet solidification but has retention value -> write to /path/to/learned/
3. Update index

## Dependencies
- feedback-collector — Requires corrections.md and reflections.md
