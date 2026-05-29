# agent-handoff

**A lightweight protocol for handing off context between AI agents — without losing your mind (or your tokens).**

```
Claude Code ──── 500 tokens ────▶ Gemini CLI
              (not 45,000)
```

> Stop dumping your entire session history into another agent.  
> Start sending only what matters.

---

## The Problem

You're running Claude Code and Gemini CLI side by side. You want one to verify the other's work, or continue where the other left off.

Current reality:
- Copy-paste entire session logs → **bloated context, wrong answers**
- Manually write summaries every time → **tedious, error-prone**
- Run agents in silos → **no cross-validation, blind spots**

**Result:** Every cross-agent handoff costs 40,000–100,000 tokens and still loses critical context.

---

## The Solution

`agent-handoff` is a **structured protocol** for AI-to-AI context transfer.

- **One active file per direction** — not buried in session logs
- **50-line max** — only what the receiving agent needs to act
- **Status tracking** — `PENDING → DONE → archived`
- **95% token reduction** on cross-agent communication

```
Before: session.md  →  1,596 lines  →  ~45,000 tokens
After:  handoff/claude-to-gemini.md  →  40 lines  →  ~600 tokens
```

Works with: **Claude Code, Gemini CLI, Codex CLI, Cursor, any text-based AI agent.**

---

## Quick Start

```bash
# 1. Clone
git clone https://github.com/godan-tech/agent-handoff
cd agent-handoff

# 2. Install skills to Claude Code
cp skills/*.md ~/.claude/skills/

# 3. Install skills to Gemini CLI (optional)
cp skills/*.md ~/.gemini/skills/

# 4. Initialize your project's handoff directory
cp -r handoff/templates ./handoff
```

That's it. Now use `/handoff` in Claude Code or Gemini CLI.

---

## How It Works

### Directory Structure

```
your-project/
└── handoff/
    ├── claude-to-gemini.md      ← Claude writes here
    ├── gemini-to-claude.md      ← Gemini writes here
    └── archive/                 ← Completed handoffs
```

### The Protocol

**Agent A delegates a task:**
```bash
# In Claude Code:
/handoff
```
Generates `claude-to-gemini.md`:
```markdown
---
from: claude
to: gemini
created: 2026-05-29 14:30
status: PENDING
priority: P1
---
# Handoff: Review BM design

## Request
Verify the revenue model assumptions in the attached design doc.

## Context (paths only — do not copy content)
- Related file: ./docs/bm-design.md
- Key decision: Chose subscription over transactional for B2B segment

## Tasks
### [Task A - Critical] Validate pricing tier logic
Is the $299/mo Enterprise tier defensible against competitors at $199?

## Expected Output
- Format: bullet points per task
- When done: write to gemini-to-claude.md, set status → DONE
```

**Agent B receives and responds:**
```bash
# In Gemini CLI:
/handoff-reply
```

**Agent A checks the response:**
```bash
# In Claude Code:
/check-handoff
```

---

## Skills Included

| Skill | Command | What it does |
|-------|---------|--------------|
| `handoff.md` | `/handoff` | Create a handoff file (auto-detects which agent you are) |
| `handoff-reply.md` | `/handoff-reply` | Read incoming handoff, write structured response |
| `check-handoff.md` | `/check-handoff` | Read the response, archive completed handoff |

---

## The Rules (what makes it work)

```
✅ Reference files by PATH only — never paste content
✅ Keep files under 50 lines
✅ One active file per direction at a time
✅ Always set status to DONE when finished
✅ Archive completed handoffs — never delete them

❌ Do NOT read the other agent's full session file
❌ Do NOT paste session history into the handoff
❌ Do NOT leave PENDING handoffs unanswered
```

---

## Compatibility

| Agent | Supported | Notes |
|-------|-----------|-------|
| Claude Code | ✅ | Primary target |
| Gemini CLI | ✅ | Skill symlinks included |
| Codex CLI | ✅ | Copy skills manually |
| Cursor | ✅ | Use as slash commands |
| Any agent that reads files | ✅ | Use templates directly |

---

## Token Savings Proof

Real measurement from production use:

| Method | Tokens read | Agent accuracy | Time to action |
|--------|-------------|----------------|----------------|
| Full session.md handoff | ~45,000 | Low (noise) | 30+ sec |
| agent-handoff protocol | ~600 | High (focused) | 5 sec |
| **Savings** | **98.7%** | **↑** | **6x faster** |

---

## Why I Built This

I run a solo AI consultancy with two AI agents working in parallel — Claude Code for execution, Gemini CLI for cross-validation. After my session logs hit 1,500+ lines, cross-agent tasks started failing silently.

This protocol is the result of 3 months of iteration on what "just enough context" looks like for an AI agent to act correctly.

---

## Contributing

PRs welcome for:
- Additional agent-specific install guides
- Template variations (debugging, code review, research)
- Integration scripts for other tools

---

## License

MIT

---

*Built for AI builders who work fast and think clearly.*
