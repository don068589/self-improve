# SKILL.md - Self-Improve Framework

A pluggable self-improvement framework for AI agents. Automatically learns from mistakes, corrections, and feedback to continuously improve execution quality.

## Description

Self-Improve enables your agent team to evolve over time:
- Scans agent memory logs for learning signals
- Extracts reusable experience rules
- Proposes improvements to system files (with approval workflow)
- Maintains a 3-tier memory system (HOT/WARM/COLD)

## When to Use

- **Automatic (Cron)**: Runs every 3 days by default
- **Manual trigger**: When user asks to "run self-improve" or "learn and improve"
- **After significant events**: User can request immediate run after major corrections

## Installation

```bash
# Clone to your preferred location
git clone https://github.com/openclaw/self-improve.git
cd self-improve

# Run setup with your config
node scripts/setup.mjs --config user-config.yaml

# Add the generated Cron task to OpenClaw
# See proposals/PENDING.md after setup
```

## Quick Start

### 1. Configure Paths

Edit `user-config.yaml`:
```yaml
storage:
  root: "/path/to/self-improve"
  knowledge_root: "/path/to/learned"
  workspace_root: "/path/to/.openclaw"

owner:
  name: "YourName"
  timezone: "Asia/Shanghai"
```

### 2. Run Setup

```bash
node scripts/setup.mjs --config user-config.yaml
```

### 3. Approve Cron Task

After setup, check `proposals/PENDING.md` for the suggested Cron task. Add it to your OpenClaw config.

### 4. Verify

```bash
node scripts/status.mjs
```

## How It Works

```
Scan memory logs → Extract signals → Classify by theme
       ↓
Promote/demote rules between memory tiers
       ↓
Propose outputs:
  → System file changes (needs approval)
  → Knowledge base entries
  → Blog drafts / methodologies
```

## Memory Tiers

| Tier | Location | Purpose |
|------|----------|---------|
| HOT | `data/hot.md` | Frequently used rules (≤100 lines) |
| WARM | `data/themes/` | Theme-based rules (≤200 lines each) |
| COLD | `data/archive/` | Archived rules (on-demand) |

## Files Structure

```
self-improve/
├── SKILL.md              # This file
├── SYSTEM.md             # Full documentation
├── ENGINE.md             # Trigger rules
├── config.yaml           # Module registry
├── user-config.yaml      # User configuration
├── modules/              # Pluggable modules
├── data/                 # All data files
├── proposals/            # Approval queue
└── scripts/              # CLI tools
```

## Manual Run

To run immediately without waiting for Cron:
```bash
node scripts/run-all.mjs
```

## Integration with OpenClaw

After installation, add to your main agent's TOOLS.md:

```markdown
## Self-Improve System

- **Path**: /path/to/self-improve
- **Cron**: Runs every 3 days at 4 AM
- **Purpose**: Learns from agent corrections and feedback
- **Check**: proposals/PENDING.md for pending improvements
```

## Dependencies

- OpenClaw >= 2026.3.0
- Node.js >= 18.0.0

## License

MIT License
