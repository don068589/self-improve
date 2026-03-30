# Team Capability Profile Prompt

> Used by profiler module to track each agent's performance and update capability profiles

---

```
You need to track each agent's performance and update team capability profiles.

## Input

- `data/feedback/*.jsonl` — Feedback data (containing agent, score fields)

## Output

- `data/profile.md` — Capability profiles for each agent

## Execution Condition

Execute once every 3 runs.

---

## Statistics Rules

### Mark as "Strong"

Condition: Success rate ≥ 90% and ≥ 10 occurrences

### Mark as "Needs Improvement"

Condition: Success rate < 70% and ≥ 5 occurrences

### Success Rate Calculation

- Positive signals: score = +1 or +2
- Negative signals: score = -1
- Success rate = Positive count / Total count

## Dimension Analysis

Statistics by the following dimensions:
- skill — Skills involved
- theme — Topic area
- domain — Technical domain

## Execution Steps

1. Read `data/feedback/*.jsonl`
2. Group by agent
3. Calculate success rate
4. Analyze by dimension
5. Update `data/profile.md`

## Output Format

```markdown
# Team Capability Profile

> Last updated: {timestamp}
> Statistical period: Last {N} runs

## {agent-name}

### Strong
- {theme/domain/skill}: Success rate {X}%

### Needs Improvement
- {theme/domain/skill}: Success rate {X}%

### Statistics
- Total tasks: {N}
- Successful: {N}
- Failed: {N}
```

## Note

profile.md is currently a "dead end" — written to but not read by other modules.
Can be integrated with proposer later to determine if an agent is suitable for a task.
