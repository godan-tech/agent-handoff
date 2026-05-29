# The agent-handoff Protocol

## Core Principle

> An agent should receive only what it needs to act — not everything that happened before.

The most common mistake in multi-agent workflows: dumping your full session history into another agent's context. This causes:
- Token waste (10–100x more than necessary)
- Noise drowning out the signal
- Wrong or unfocused responses
- Slow, expensive round-trips

The solution is a **structured, bounded handoff file** — like a well-written brief, not a meeting transcript.

---

## The Three Files

At any moment, a two-agent workflow has at most two active files:

```
handoff/
├── claude-to-gemini.md      ← Claude's outgoing request
├── gemini-to-claude.md      ← Gemini's response
└── archive/                 ← All completed pairs
```

Rules:
- **One active file per direction** at a time
- **Max 50 lines** per file
- **Status must be tracked**: `PENDING → DONE → archived`

---

## Status Lifecycle

```
[created]      status: PENDING   ← sender wrote this
[received]     (reading)         ← receiver reads, acts
[responded]    status: DONE      ← receiver wrote response
[archived]     (moved to archive/)  ← sender archived the pair
```

A file stays `PENDING` until the receiving agent sets it to `DONE`.
A `PENDING` file should never be overwritten — ask first.

---

## What Goes In a Handoff

### Include
- The specific question or task
- File **paths** the receiver needs to read
- 2–3 lines of relevant background
- Expected output format

### Never include
- Full session history
- File contents (use paths instead)
- Context the receiver doesn't need
- More than 3 tasks per handoff (split if needed)

---

## Measuring Success

A good handoff:
- Is under 50 lines
- Gets an accurate response without follow-up clarification
- Costs less than 1,000 tokens to read

If you need more than 3 back-and-forth exchanges to resolve a task, the original handoff was probably too vague.

---

## Multi-Agent Topology

This protocol works for any number of agents, not just two:

```
Claude ──────────▶ Gemini (verify)
  │
  └──────────────▶ Codex (implement)
                       │
                       └──▶ Claude (review)
```

Each edge is one handoff file. Each agent reads only its incoming file.

---

## Archive Strategy

Archive completed handoffs — don't delete them. They become:
- A record of decisions made
- Training data for improving your prompts
- Documentation of the reasoning chain

Naming convention:
```
YYYY-MM-DD-HHmm-[Sender→Receiver]-[topic].md
```
