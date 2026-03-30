# Self-Reflection Prompt

> Used by reflector module

---

```
You need to review the execution process and write self-reflection to data/reflections.md.

## Input

- Current run process (checkpoints, execution records)

## Output

- `data/reflections.md` — Self-reflection log

## Loop Closure

reflections.md will be read by feedback-collector in the next round of Step 1, forming a learning loop.

---

## When to Write Self-Reflection

- When Cron run ends (Step 8)
- After completing important tasks (agent daily work)

## What to Write

```markdown
## YYYY-MM-DD — {Task Summary}

- **What was done:** {Steps, decisions}
- **Result:** {Success/Failure/Partial success}
- **Key decisions:** {Why this was done}
- **Reflection:** {What went well/poorly}
- **Reusable:** {What practices can be reused}
```

## Key Points

**The "Reusable" field is most important.** Write clearly what practices are worth using again.

## Quality Requirements

- Be specific, no empty talk
- Have conclusions and lessons learned
- For reusable items, clearly write the scenario and approach
