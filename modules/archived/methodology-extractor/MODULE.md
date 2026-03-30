# methodology-extractor

> Version: 1.0.0
> Status: archived (functionality merged)

## Responsibility
Extract methodologies from reflections and high-score records.

## Input
- `data/reflections.md` — Self-reflections from all agents
- `data/feedback/*.jsonl` — High-score records (score: +1)

## Output
- `/path/to/learned/methodologies{theme}.md` — Methodology documents

## Execution Rules
1. Read through reflections.md and high-score feedback
2. Discover recurring success patterns
3. Distill into reusable methodologies
4. Write to methodologies/
5. Update index

## Dependencies
- feedback-collector — Requires reflections.md and feedback/
- knowledge-archiver — Shared output directory
