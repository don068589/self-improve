# Self-Improve: Agent Evolution System

> A pluggable, modular AI Agent self-improvement framework.

## What Is This?

Self-Improve enables AI Agents to learn from errors, corrections, and feedback, accumulate reusable experience rules, and continuously improve execution quality.

**Core Features:**
- 🔄 **Automatic Learning**: Runs on schedule (default every 3 days)
- 📦 **Module Architecture**: Add/remove modules as needed
- 🧠 **Multi-layer Memory**: HOT/WARM/COLD three-layer memory
- ✅ **Approval Workflow**: System file modifications require explicit approval
- 🔌 **OpenClaw Native**: Designed for OpenClaw Agent teams

## Installation

### Method 1: Clone from GitHub (Recommended)

```bash
git clone https://github.com/don068589/self-improve.git
cd self-improve

# Copy and edit configuration
cp user-config.yaml my-config.yaml
# Edit my-config.yaml, fill in your paths

# Run installation
node scripts/setup.mjs --config my-config.yaml
```

### Method 2: Download ZIP

Download from [GitHub Releases](https://github.com/don068589/self-improve/releases), extract, then:

```bash
cd self-improve
cp user-config.yaml my-config.yaml
# Edit my-config.yaml
node scripts/setup.mjs --config my-config.yaml
```

### Verify Installation

```bash
node scripts/status.mjs
```

## Quick Start

Edit `user-config.yaml`:

```yaml
storage:
  root: "/path/to/self-improve"      # Installation path
  knowledge_root: "/path/to/learned" # Knowledge output path
  workspace_root: "/path/to/.openclaw"  # OpenClaw workspace

owner:
  name: "Your name"
  timezone: "Asia/Shanghai"

agent:
  main_agent: "main"  # Main agent ID
```

### After Installation

1. Check Cron Task suggestions in `proposals/PENDING.md`
2. Approve the Cron Task in OpenClaw configuration
3. The system will automatically run every 3 days

## How It Works

```
Scan logs → Distill rules → Classify and archive
    ↓
Memory elevation (HOT/WARM/COLD)
    ↓
Determine output:
  → System file modification (requires approval)
  → Knowledge base
  → Blog draft
```

## Memory Layers

| Layer | Location | Limit | Usage |
|-------|----------|-------|-------|
| HOT | `data/hot.md` | ≤100 lines | Loaded before solidification |
| WARM | `data/themes/` | Each file ≤200 lines | Load on demand |
| COLD | `data/archive/` | Unlimited | Explicit query only |

## Directory Structure

```
self-improve/
├── SKILL.md              # OpenClaw Skill definition
├── SYSTEM.md             # System documentation
├── ENGINE.md             # Power mechanism
├── RUNTIME.md            # Runtime mechanism
├── config.yaml           # Module configuration
├── user-config.yaml      # User configuration template
├── modules/              # Module directory
├── prompts/              # Execution instructions
├── scripts/              # CLI tools
└── data/                 # Data directory
```

## Manual Run

```bash
node scripts/run-all.mjs
```

## Security

### Never Store
- Passwords, API Keys, Tokens
- Financial/medical information
- Third-party personal data
- Location patterns

### Transparency
- "What did you remember" → Full export
- Each Rule has source and timestamp
- "Forget X" → Delete from all layers

## License

MIT License

## Acknowledgments

Built for the OpenClaw Agent team. Inspired by the need for Agents to truly improve over time.
