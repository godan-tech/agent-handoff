# Installation Guide

## For Claude Code

```bash
# Copy skills to Claude's global skills directory
cp skills/handoff.md ~/.claude/skills/
cp skills/handoff-reply.md ~/.claude/skills/
cp skills/check-handoff.md ~/.claude/skills/
```

Then in any Claude Code session:
```
/handoff          → create a handoff for another agent
/handoff-reply    → respond to an incoming handoff
/check-handoff    → read a response and archive
```

## For Gemini CLI

```bash
# Copy skills to Gemini's global skills directory
cp skills/handoff.md ~/.gemini/skills/
cp skills/handoff-reply.md ~/.gemini/skills/
cp skills/check-handoff.md ~/.gemini/skills/
```

## For Codex CLI / Cursor / Other Agents

Copy the template files and use them as slash commands or reference documents.

The protocol works with any agent that can read files — no special integration needed.

## Project Setup

Initialize the handoff directory in your project:

```bash
mkdir -p ./handoff/archive
cp handoff/templates/agent-a-to-agent-b.md ./handoff/claude-to-gemini.md
cp handoff/templates/response.md ./handoff/gemini-to-claude.md
```

Add to `.gitignore` if you don't want to commit handoff files:
```
# Optional — keep handoff files local
handoff/claude-to-gemini.md
handoff/gemini-to-claude.md
# But DO commit the archive if you want decision history
```

## Verify Installation

In Claude Code:
```
/handoff
```

You should see the handoff creation prompt. If the skill isn't found, check that the file is in `~/.claude/skills/`.
