# Proposal Judgment Prompt

> Used by proposer module to comprehensively consider all intermediate products, judge output format, mark high-value

---

```
You need to comprehensively consider all intermediate products and judge the final destination of each piece of material.

## Core Method: Understand Value, Don't Match Channels

**Don't mechanically think "which channel does this content fit."** First understand the core value of this content, then decide how to maximize utilizing this value.

Ask yourself:
- What is the core value of this content?
- How can this value be maximized?
- Is there only one way to utilize it? Or can it be used from multiple angles?
- Who does this content have value for? (Team internal only / Has universal value)

## Input Scope

Comprehensively consider all the following intermediate products:

| Input | Purpose | Priority |
|-------|---------|----------|
| `data/themes/` | All classified content (including low-frequency but high-quality insights) | **Highest — Main source for value judgment** |
| `data/hot.md` | ≥3 occurrences active rules | High — Specifically serves rule solidification judgment |
| `data/errors/` | Existing error knowledge points, check duplicate patterns | Medium — Re-processing check |
| `data/lessons/` | Existing lessons learned, check commonalities | Medium — Re-processing check |

**themes/ is the main input source.** hot.md only serves the "rule solidification" output channel. Other output channels (blogs, methodologies, skill improvements, etc.) are mainly judged from themes/.

**Don't be misled by hot.md's priority.** Content in hot.md is high-frequency rules, suitable for solidification; but low-frequency, high-value insights might only appear once, yet are more worth deep processing than simple corrections appearing 10 times.

## Value Judgment (Core Capability)

### Judgment Principles

1. **Understand problem essence** — Figure out what the problem is first, then think of solutions
2. **Think about the best handling method** — Not limited to preset options
3. **Can propose any suggestions** — Including changing configs, changing workflows, creating new files, deleting files, merging rules, etc.
4. **When uncertain, mark multiple possibilities** — Discuss with User during approval
5. **Don't be limited by feedback's type field** — type is just step 1's preliminary label, you need to independently judge output format. A signal marked as `type: explicit` (correction) might have deeper patterns worth writing as methodology. A `type: methodology` signal might also have rule solidification value. type is reference information, not routing instructions.

### Value Dimensions

| Dimension | Low | Medium | High |
|-----------|-----|--------|------|
| Reusability | Current scenario only | Reusable within team | Has universal value |
| Impact scope | Single agent | Multiple agents | System level |
| Information density | Single correction | Multiple related items | Long-chain discussion condensed |
| Persistence | Temporary | Long-term valid | Fundamental principle |

### value_density Utilization

classifier marked value_density (low/medium/high) in themes/. Use this information:
- **high** → Process first, consider multiple output channels
- **medium** → Normal judgment
- **low** → Rule solidification is enough (if count requirement met)

## Output Format Judgment

### Output Channels

| Output | File | Condition |
|--------|------|-----------|
| Rule solidification | proposals/PENDING.md | occurrences ≥ 3 or User explicitly expressed |
| Skill improvement | proposals/PENDING.md (skill-improvement) | Low scores ≥ 3, skill can be optimized |
| Blog article | drafts/blog-{topic}.md | Has universal value |
| Methodology | /path/to/learned/methodologies\ | Reusable thinking framework |
| Error knowledge point | data/errors/ | Has cause and solution |
| Lesson learned | data/lessons/ | "Learned" but not enough for solidification |
| System improvement | proposals/PENDING.md (system-upgrade) | Affects system architecture |
| High-value mark | data/high-value/ | value_density=high |

---

### Step 1: Understand Value

After reading a piece of content, ask first:
- What is the core value of this piece?
- What problem can it solve?
- Who needs it?

### Step 2: Choose Best Output

| Value Type | Common Output | Judgment Basis |
|-----------|--------------|----------------|
| Behavioral norms | Rule solidification → PENDING.md | Repeated ≥3 times, or User explicitly expressed |
| Skill issues | Skill improvement → PENDING.md (skill-improvement) | Low score records ≥3, skill itself can be optimized |
| Universal insight | Blog article → drafts/blog-{topic}.md | Has universal value, can help others |
| Reusable method | Methodology → /path/to/learned/methodologies\ | Reusable thinking framework |
| Specific error | Error knowledge point → data/errors/ | Has cause and solution |
| Lesson learned | Lesson → data/lessons/ | "Learned..." but not enough for solidification |
| Business related | Business insight → /path/to/learned/business\ | Business logic, profit model |
| System design | System improvement → PENDING.md (system-upgrade) | Affects system architecture |

### Step 3: One Content, Multiple Angles of Utilization

**Don't put high-value content into only one channel.** Think:
- Behavioral norms → Rule solidification
- Thinking methods → Methodology
- Universal value → Blog
- Specific operations → Knowledge points

For example, a long-chain discussion can simultaneously produce:
- Rule solidification (behavioral norms part)
- Blog article (design philosophy part)
- Methodology (thinking framework part)

**Specific thinking example:**

Suppose there's a signal: Long-chain discussion involving system design methodology and specific behavioral norms.

Thinking process:
1. What is the core value of this piece? → System design methodology + specific behavioral norms
2. Who needs it? → Team internal (behavioral norms) + External (methodology has universal value)
3. Is there only one way to utilize it? → No. At least three:
   - **Rule solidification**: Certain principle → Write into AGENTS.md as workflow norm
   - **Methodology**: Design framework → Write into methodologies/ as reusable framework
   - **Blog**: Thematic article → Write into drafts/ for external sharing
4. Split or merge? → Split. Three outputs independently approved, more flexible

## Rule Solidification Judgment

### Regular Solidification (Needs Repeated Verification)

1. **Occurrences ≥ 3** — Same correction/rule recorded 3+ times
2. **User explicitly expressed** — User said "always", "never", "from now on" and other definitive words
3. **Wide impact scope** — Applies to all agents, not specific scenarios

### Emergency Solidification (Skip Count Requirement)

When corrections.md has `severity: high` marked, or meets the following conditions, skip the "3 times repeat" requirement, mark as `[URGENT]`:

1. Caused actual damage
2. Cross-agent coordination failure
3. User explicitly requested solidification

Emergency solidification only skips the threshold for waiting for repetition, **approval process unchanged** — still write to PENDING.md, wait for User's decision.

### Situations Not to Solidify

- Corrections appearing only 1-2 times (continue observing)
- Temporary rules for specific projects/scenarios
- User's hypothetical discussions
- Third-party preferences

## Target File Judgment (Core Capability)

**proposer's core task is to judge "where should this material go."**

### Common Situations Reference (Heuristic, Not Restriction)

| Content Nature | Common Target | Example |
|---------------|--------------|---------|
| Behavioral norms, workflow | AGENTS.md | "Should X then Y", "Don't X" |
| Personality traits, attitude style | SOUL.md | "Be more direct", "Personality should" |
| User habits, preferences, opinions | USER.md | "User likes X", "User dislikes Y" |
| Tool usage, environment configuration | TOOLS.md | "Newly installed X", "Use X tool" |
| Personal preferences, private rules | MEMORY.md | "User's personal habits" |
| Role positioning, identity description | IDENTITY.md | "This agent's role is" |
| Environment info, contacts | Shared directory registry | "New bot", "New service port" |
| Skill usage issues | skill improvement | "Skill X has problems" |
| New capability needs | New skill suggestion | "Need X capability" |
| System design issues | SYSTEM.md/ENGINE.md | "The system should" |
| Configuration issues | openclaw.json | "contextWindow too small" |

**This is reference, not restriction.**

### Judgment Method

Ask yourself:
1. **What is the problem essence?** — Understand clearly first
2. **What is the best solution?** — Not limited to the table above
3. **What needs to be changed?** — Could be files, configs, workflows, structures
4. **Are there better suggestions?** — Can propose any ideas

### Examples

**Problem: Model context exceeded limit**

✗ Mechanical judgment: "Add rule to AGENTS.md"
✓ Correct judgment: "This is a configuration issue, suggest changing openclaw.json's contextWindow or switching models"

**Problem: Task conflicts between agents**

✓ Can judge: "Add collaboration rules to AGENTS.md"
✓ Can also judge: "Create task claiming mechanism, need to create new file"
✓ Can also judge: "This is a system issue, suggest adding task scheduling functionality"

**Multiple solutions are all acceptable, write clearly in PENDING.md, discuss during approval.**

## errors/ and lessons/ Re-processing

**errors/ and lessons/ are not endpoints, they're intermediate stations.**

Each run, check existing errors/ and lessons/:

| Discovery | Handling |
|----------|----------|
| Multiple errors have commonalities | Distill into rule → PENDING.md |
| Lesson repeated | Upgrade to rule solidification → PENDING.md |
| error/lesson can be generalized | Distill into methodology → methodologies/ |
| error/lesson has universal value | Distill into blog → drafts/ |

**Loop closure: errors/lessons are both output locations and input sources for next round.**

## High-Value Marking

After comprehensively considering themes/ (main) and hot.md, mark high-value items:

| Criterion | Description |
|----------|-------------|
| Wide impact scope | Applies to multiple agents or system level |
| Strong extractability | Can be distilled into methodology, blog article |
| High value density | Large information volume, condensed through multiple rounds of dialogue |
| value_density = high | classifier already marked as high density |

Items marked as `high_value` write to `data/high-value/`, for Step 6 re-processing and deep reading.

## Meta-Ability: Propose Better Handling Suggestions

**proposer not only judges destination, but also can propose better handling suggestions.**

When generating suggestions, proactively think:

1. **Is there a better solution for this problem?**
   - Not limited to "add rules", maybe changing configs, workflows, creating new files is better
   - Example: Context exceeded → Not add rules for agent to be careful, but change contextWindow config

2. **Should create new file instead of appending to existing file?**
   - If existing file is already too long, splitting might be better
   - If it's completely new capability need, creating new skill might be more appropriate

3. **Should split into multiple suggestions and handle separately?**
   - A long-chain discussion might involve multiple levels
   - After splitting, each independently approved, more flexible

4. **Should observe first instead of immediately solidifying?**
   - Some patterns need more data verification
   - Mark as "suggest observing" instead of "suggest solidifying"

5. **Are there other systems/tools that can better handle this problem?**
   - System design level problems shouldn't use rule patches
   - Might need system-upgrade instead of rule-addition

6. **Can this content help others?**
   - Has universal value → Consider blog or methodology
   - Internal only → Rule solidification is enough

If there are better suggestions, add fields in PENDING.md:

```markdown
- **Alternative:** {Other possible handling methods}
- **Recommendation:** {What you think is the best handling method and why}
```

## Output Format

### Rule Solidification → PENDING.md

```markdown
## [P-YYYYMMDD-NNN] Brief Title

- **Source:** corrections.md #line number / hot.md:line number / themes/{topic}
- **Suggested Modification:**
  - Current state: (Existing relevant content in target file, if any)
  - Suggested addition/modification: (Specific new content or modification plan)
- **Target File:** {Specific path, like openclaw.json or AGENTS.md}
- **Target Explanation:** Why suggest going here
- **Reason:** Appeared N times / User explicitly expressed / Emergency reason
- **Events Involved:** agent: X, Y, Z (who had this problem)
- **Suggested For:** Whole team / Specific agents: X, Y (who should get this rule)
- **Alternative:** (If there are better handling suggestions, or multiple possible targets)
- **Status:** 🟡 Pending approval
```

**Write full path or specific filename for target file**, convenient for locating and discussing during approval.
**Alternative is optional**, if there are better handling suggestions or multiple possible targets, write them.
**Target can be any reasonable suggestion**, including:
- System files (AGENTS.md, SOUL.md, USER.md, TOOLS.md, MEMORY.md, IDENTITY.md)
- Configuration files (openclaw.json, config.yaml)
- Shared directories ({shared_root}/registry/)
- Skill files (~/.openclaw/skills/xxx/SKILL.md)
- New files
- Delete/merge files
- Any other reasonable handling methods

### Emergency Solidification → PENDING.md

```markdown
## [P-YYYYMMDD-NNN] [URGENT] Brief Title

- **Source:** corrections.md (severity: high)
- **Emergency Reason:** Caused actual damage / Cross-agent coordination failure / User requested
...rest fields same as regular format...
```

### Skill Improvement → PENDING.md

```markdown
## [P-YYYYMMDD-NNN] Improve {skill name} Skill

- **Type:** skill-improvement
- **Target File:** {workspace_root}\skills\{skill name}\SKILL.md
- **Source:** themes/{topic}/{rule}.md (frontmatter.skill field, low scores ≥ 3)
- **Problem Found:** {Problem pattern}
- **Suggested Modification:**
  Append to SKILL.md:
  > {Specific improvement content}
- **Records Involved:**
  - {date} {agent} {hint summary}
- **Source Trace:**
  - Theme: {theme}
  - Domain: {domain} (optional)
  - Project: {project} (optional)
- **Status:** 🟡 Pending approval
```

**Write full path for target file**, convenient for locating and executing after approval.

**Skill improvement judgment criteria:**
- themes/ has frontmatter.skill field present
- Low score records ≥ 3
- hint has improvable patterns (not agent personal issue, skill itself can be optimized)

### System Improvement → PENDING.md

```markdown
## [P-YYYYMMDD-NNN] System Improvement: {Improvement Point}

- **Type:** system-upgrade
- **Source:** Cross-project discussion / Long-chain discussion
- **Problem Found:** {Structural problem description}
- **Improvement Suggestion:** {Specific improvement plan}
- **Impact Scope:** {Which modules/files need changes}
- **Reason:** Why this is a system-level issue
- **Status:** 🟡 Pending approval
```

**Difference from Regular Solidification:**

| Regular Solidification | System-level Improvement |
|----------------------|-------------------------|
| Affects agent behavior | Affects system design |
| Writes to AGENTS.md/TOOLS.md | Writes to openclaw.json, MODULE.md, prompt files |
| Rule-level change | Structure-level change |

**Target Files for System-level Improvements:**

| Improvement Type | Target File |
|-----------------|------------|
| New module | modules/*/MODULE.md, config.yaml |
| Workflow change | prompts/*.md |
| Configuration change | config.yaml, openclaw.json |
| System architecture | SYSTEM.md, ENGINE.md |

### Blog Draft → drafts/

```markdown
# {Title}

> Draft status: Pending polish
> Source: self-improve run {run_id}
> Discovery time: {time}
> Core value: {One sentence explaining why worth writing}

## Core Points

{Distilled core points, 3-5 items}

## Body

{Article body, can include:}
{- Problem background}
{- Solution approach}
{- Practice cases}
{- Summary and elevation}

## Items to Polish

- [ ] Is title attractive
- [ ] Does opening grab readers
- [ ] Does ending have elevation
- [ ] Are cases sufficient
- [ ] Is language fluent
```

**Situations to write blog:**
- Has universal value, not just applicable to this team
- Ideas, experiences worth spreading externally
- Methodologies that can help others
- Technical practices, pitfall summaries

**Situations not to write blog:**
- Pure internal rules
- Involves privacy or sensitive information
- Too fragmented, no complete viewpoint

### Methodology → /path/to/learned/methodologies\

```markdown
# {Methodology Name}

## Applicable Scenarios

{When to use this methodology}

## Core Steps

1. {Step 1}
2. {Step 2}
3. {Step 3}

## Key Points

- {Point 1}
- {Point 2}

## Example

{Specific case}

## Source Trace

- Source: {Original content location}
- Time: {Discovery time}
```

## Write Quality Requirements

1. **Use target file's language style** — Read target file, imitate its format and tone
2. **Specific and executable** — Don't write vague suggestions, write specific rules
3. **Mark source** — Every suggestion marks source and occurrence count
4. **Don't duplicate** — Check if similar suggestion already exists in PENDING.md
