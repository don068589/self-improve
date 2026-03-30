# skill-improver

> Version: 3.0.0
> Status: archived (functionality merged)

## Responsibility
Identify skill improvement needs from feedback, write to themes/ for solidification.

## Input
- `data/feedback/*.jsonl` — Feedback records containing skill field

## Output
- `data/themes/skill-improvements/{skill name}.md` — Skill improvement suggestions

## Execution Rules
1. Extract skill field from feedback
2. Count low-score records for each skill
3. Low score >= 3 times -> write to themes/skill-improvements/
4. Wait for memory-layer to solidify into hot.md
5. Proposer extracts solidification suggestions

## Dependencies
- feedback-collector — Requires feedback data
- memory-layer — Requires themes/ and hot.md
- proposer — Eventually generates PENDING.md
