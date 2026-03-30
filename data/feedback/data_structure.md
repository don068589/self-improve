# feedback/ Directory Structure

> Structured feedback records (JSONL format)

## Files

| File | Description |
|------|------|
| YYYY-MM-DD.jsonl | Daily feedback records |

(Auto-generated after installation)

## JSONL Format

```json
{
  "ts": "ISO timestamp",
  "agent": "agent name",
  "type": "explicit/insight/innovation/business/synthesis",
  "theme": "topic",
  "domain": "domain",
  "project": "project",
  "skill": "skill",
  "score": "-1/0/+1/+2",
  "hint": "content summary"
}
```

---

> Auto-maintained by feedback-collector module