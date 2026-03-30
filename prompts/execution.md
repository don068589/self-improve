# Approval Execution Prompt

> Execute when User approves suggestions in PENDING.md

---

```
User approved an improvement suggestion in conversation, you need to execute the write.

## Input

- `proposals/PENDING.md` — Pending proposals (the one User approved)
- Target files (AGENTS.md / TOOLS.md / MEMORY.md / SOUL.md / HEARTBEAT.md, etc.)

## Output

- Target file — Updated
- `proposals/PENDING.md` — Status updated to "Approved"
- `data/archive/proposals-Date.md` — Archive record
- `run-log.jsonl` — Execution log

---

## Trigger Conditions

User says:
- "Approve P-xxx"
- "Approve all"
- "Pass P-xxx"
- "Execute P-xxx"

## Execution Steps

### 1. Locate Proposal

Read `/path/to/self-improve/proposals/PENDING.md` and find the corresponding proposal.

### 2. Read Target File

Read the target file based on the **Target File** in the proposal.

### 3. Execute Modification

Modify the target file according to the proposal content:
- If "append content" → Append to appropriate location
- If "modify content" → Replace corresponding section
- Write in the target file's language style

**Write Quality Requirements:**
1. **Blend in, don't insert** — Written content should naturally fit as if it was always there
2. **Don't break structure** — Maintain the target file's overall structural integrity
3. **Don't duplicate** — If similar rules exist, merge rather than add new ones
4. **Minimal changes** — Only change what needs to be changed, don't touch other content

If the proposal's "Suggested For" is specific agents (not whole team), add `(xxx only)` marker after the section title when writing.

### 4. Mark Complete

Update status in PENDING.md:

```markdown
- **Status:** ✅ Approved (YYYY-MM-DD HH:MM)
- **Executor:** {current agent}
```

### 5. Archive

Move to `/path/to/self-improve/data/archive/proposals-Date.md` (append).

### 6. Log

Append to `/path/to/self-improve/run-log.jsonl`:

```json
{"ts":"ISO timestamp","action":"approval_executed","proposal_id":"P-xxx","target_file":"target file","executor":"agent name"}
```

## Rejection Handling

When User says "Reject P-xxx":

1. Mark in PENDING.md:
   ```markdown
   - **Status:** ❌ Rejected (YYYY-MM-DD HH:MM)
   - **Rejection reason:** {reason User gave, if any}
   ```

2. Move to `data/archive/proposals-Date.md`

## Target File Handling Rules

| Target File | Handling Method |
|------------|-----------------|
| AGENTS.md | Append to appropriate section, or create new section |
| TOOLS.md | Append to appropriate section |
| SOUL.md | Append to appropriate section |
| MEMORY.md | Append to appropriate section |
| HEARTBEAT.md | Append task item |
| openclaw.json | Modify corresponding configuration |
| SKILL.md | Append content to SKILL.md |

**Note:**
- Read target file first, understand existing structure
- Write in target file's style
- Don't destroy existing content
