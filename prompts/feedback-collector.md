# Feedback Collector Prompt

> Used by feedback-collector module to extract valuable signals from conversation logs

---

```
You need to extract valuable signals from agent's daily work logs.

## Input

- `config.yaml`'s `last_success_ts` — Incremental scan starting point
- All agent's `workspace*/memory/*.md` — Conversation logs
- `data/reflections.md` — Previous round's reflection (loop closure input)

## Output

- `data/feedback/{date}.jsonl` — Structured signal records

---

## Core Method: Semantic Understanding, Not Keyword Matching

**Don't mechanically look for keywords.** Read the conversation, understand User's true intentions and reactions.

Ask yourself:
- Is User satisfied? What does their reaction indicate?
- What is User teaching? Correcting? What preferences are being expressed?
- What insights, methods, or lessons are worth extracting from this conversation?
- What does User's emotional change indicate? (Getting more excited = high-value discussion)
- How did the agent perform? What was done well? What was done poorly?

Keyword tables are only auxiliary references; semantic understanding is the core.

## Signal Recognition

### Recognition Methods

**Semantic understanding first, keywords as auxiliary reference.**

| Signal Type | Semantic Understanding | Auxiliary Keywords (reference only) |
|------------|------------------------|-------------------------------------|
| Correction | User points out error, dissatisfaction, requests correction | "wrong", "should", "don't" |
| Implicit correction | User rewrites answer themselves, asks same question again, switches to another agent | No obvious keywords |
| Praise | User expresses satisfaction, approval, continues forward | "good", "right", "nice" |
| Preference | User expresses persistent preferences | "always", "from now on", "every time" |
| Insight | Deep discovery or idea emerges in discussion | "I found", "actually", "realized" |
| Methodology | Reusable method emerges in discussion | "this is more effective", "the steps are" |
| Lesson | Experience summarized from failure or problem | "the lesson is", "learned" |
| System-level | Issues involving system design, architecture level | "the system should", "design flaw" |
| Long-chain discussion | Same topic continues >30 minutes, multi-round evolution | No keywords, look at conversation structure |

### Implicit Signals (keywords can't catch, only semantic understanding can discover)

- **User asks follow-up questions** → First answer wasn't good enough
- **User rewrites** → Agent's output was unsatisfactory
- **User switches agent** → Lost confidence in current agent
- **User falls silent then changes topic** → May have given up
- **User gets more excited as conversation progresses** → High-value discussion
- **User spends a long time discussing one topic** → This topic is important
- **User asks three consecutive follow-up questions** → First answer wasn't good enough

### Do Not Record

- User's one-time commands ("help me check weather")
- Small talk, greetings
- Hypothetical discussions
- Third-party preferences
- Non-User's speech in group chats

## Extraction Rules

1. **Read and understand context** — Don't scan line by line, understand the flow of the whole conversation
2. **Preserve semantic summary** — summary field uses 2-3 sentences to describe the complete context of this signal
3. **Don't over-interpret** — User didn't say bad ≠ User thinks it's good
4. **Record original text** — hint field preserves User's original words or key summary
5. **Mark agent** — Record which agent was corrected/praised
6. **Identify skill** — Determine if a skill was used

## Long-Chain Discussion Recognition

**Don't just extract signals line by line, also identify topic continuity.**

Recognition features:
- Same topic, time span >30 minutes
- Multi-round conversation, multi-stage evolution (problem→analysis→solution→refinement)
- Has summary statements

Processing method:
- **Don't split into multiple fragments**
- Merge into single synthesis record
- Preserve stage evolution information
- summary field records complete evolution flow

## Output Format

```json
{
  "ts": "YYYY-MM-DDTHH:MM:SSZ",
  "agent": "agent-name",
  "summary": "User corrected agent's error of confusing two field names during config diagnosis",
  "hint": "Confused two concepts during config diagnosis",
  "type": "explicit",
  "score": -1,
  "source": "memory/YYYY-MM-DD.md:42",
  "skill": "skill-name",
  "potential_targets": ["AGENTS.md", "TOOLS.md"]
}
```

### Field Descriptions

| Field | Required | Description |
|-------|----------|-------------|
| ts | Yes | Timestamp |
| agent | Yes | Agent involved |
| summary | Yes | **Semantic summary, 2-3 sentences describing complete context** |
| detail | No | **Detail paragraph (required for high-value signals)**, preserve key original words, evolution process, multiple insight points |
| hint | Yes | User's original words or key summary |
| type | Yes | Signal type |
| score | Yes | -1 negative / 0 neutral / +1 positive / +2 high-value |
| source | Yes | Source file and line number |
| skill | No | Skill involved (if any) |
| potential_targets | No | Heuristic reference, possible destinations |

### Granularity Density Control

**Not all signals need the same level of detail.** Adjust based on signal's value density:

| Signal Type | Density | What to Write |
|------------|---------|---------------|
| Simple correction | Low | summary (2-3 sentences) + hint |
| Preference confirmation | Low | summary + hint |
| Insight/methodology | Medium | summary + detail (one paragraph) |
| Long-chain discussion | High | summary + detail (multiple paragraphs, preserve evolution process and key original words) |
| System-level/cross-project | High | summary + detail |

**Judgment criteria:**
- Could this signal be used later for blog or methodology? → Need detail
- Is this signal just a simple behavioral correction? → summary is enough
- Is the value of this signal in User's original words and context? → detail preserves original words
- Does this signal involve methodology/system design? → Need complete flow

**detail field example (high-value signal):**
```json
{
  "summary": "User spent 40 minutes discussing system design philosophy, proposing concepts like progressive escalation and handover records",
  "detail": "Discussion started from system operation issues, evolved to: 1) Discovered context explosion problem → 2) Proposed progressive escalation process → 3) Designed handover record mechanism → 4) Elevated to meta-ability methodology. User got more excited as conversation progressed, emphasized all agents need to master these methodologies.",
  "type": "synthesis",
  "score": +2
}
```

### Destination Judgment (Heuristic Reference)

**This is heuristic reference, not restriction.** Model can propose any reasonable targets.

Common situations:
- Behavioral norms → Might go to AGENTS.md
- Personality traits → Might go to SOUL.md
- User habits → Might go to USER.md
- Tool configuration → Might go to TOOLS.md
- Skill issues → Might need skill improvement

**When uncertain, mark multiple possibilities, or mark `"unknown"` for later modules to judge.**

## Value Pre-judgment Principle

**Cost of missing high-value far exceeds false positive.**

- A score: +2 signal missed as 0 → Subsequent stages can't identify its value, won't be re-read, information permanently lost
- A score: 0 signal falsely marked as +2 → Subsequent stages take another look, find it's not worth it and skip, cost is very low

Therefore: **When uncertain, err on the side of marking high rather than low.** Especially for long-chain discussions, obvious User emotional changes, and content involving system design.

## Signal Types (Open-ended)

**Your core task is to understand conversations and judge value, not match against type tables.** type is just a label to help subsequent stages quickly filter, don't let it limit your judgment. Subsequent proposer will independently judge output format, not locked by type field.

Below are common signal types, **not limited to these**. When discovering new valuable patterns, autonomously define new types.

| Type | Description | score |
|------|-------------|-------|
| explicit | Explicit correction | -1 |
| positive | Explicit praise | +1 |
| implicit_positive | Implicit approval (smooth progress) | +1 |
| failure | Task failure | -1 |
| rewrite | User rewrote output | -1 |
| reassign | User switched agent | -1 |
| preference | Preference confirmation | +1 |
| insight | Insight discovery | +2 |
| innovation | Innovative idea | +2 |
| methodology | Methodology | +2 |
| business | Business logic | +1 |
| knowledge | Knowledge point | 0 |
| synthesis | Long-chain discussion summary | +2 |
| system-upgrade | System-level issue | +2 |
| meta-ability | Meta-ability | +2 |
| cross-project | Cross-project pattern | +2 |

**When discovering new patterns, autonomously define new types, no need for pre-definition.**

## Multi-dimensional Classification

When scanning logs, simultaneously identify multiple classification dimensions.

### Theme — Content Nature

| Theme | Recognition Features |
|-------|---------------------|
| behavior | Behavioral norms, way of working, workflow |
| communication | Communication style, expression, response style |
| tools | Tool usage, configuration, operation methods |
| coding | Coding, debugging, code review |
| search | Search strategy, information gathering, research |
| writing | Writing style, documentation, format |
| collaboration | Team collaboration, cross-agent coordination |
| preferences | Personal preferences, habits, tastes |
| professional | Professional capabilities, domain knowledge |
| personality | Personality traits, values |

**Above are references, not restrictions.** Create new themes when discovered.

### Domain — Technical Domain

| Domain | Recognition Features |
|--------|---------------------|
| frontend | Frontend, UI, CSS, React, Vue |
| backend | Backend, API, database, server-side |
| ai | AI, ML, models, prompt, agent |
| devops | Operations, deployment, Docker, CI/CD |
| data | Data analysis, statistics, reports |
| security | Security, permissions, authentication |
| mobile | Mobile, iOS, Android |
| general | Doesn't belong to above domains |

## Negative Signals Also Write to corrections.md

```markdown
## YYYY-MM-DD HH:MM

- **agent:** agent-name
- **Signal:** User correction
- **Content:** "Specific correction content"
- **Theme:** theme-name
- **Domain:** domain-name
- **Count:** 1/3
- **Status:** 🔵 Pending observation
```

## Quality Requirements

1. **Don't miss** — Better to record one extra than miss an important signal
2. **Don't misjudge** — User saying "not what I meant" is a correction, User saying "not today" is a clarification
3. **Preserve context** — summary should have enough information to reconstruct context later
4. **Deduplicate** — Same signal in same conversation only record once
5. **Semantics first** — Use understanding to discover implicit signals, not just keywords
6. **Mark dimensions** — theme/domain for subsequent classification and retrieval
