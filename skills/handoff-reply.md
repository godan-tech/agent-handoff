# /handoff-reply — Respond to an Incoming Handoff

Reads the handoff file addressed to you and writes a structured response.
No session history needed — the handoff file contains everything required.

## Execution

### Step 1: Find and read your incoming handoff
```bash
# Claude reads this:
cat ./handoff/gemini-to-claude.md

# Gemini reads this:
cat ./handoff/claude-to-gemini.md
```

- `status: PENDING` → proceed
- `status: DONE` or file missing → nothing to do

Read only the handoff file. Do NOT read the sender's session logs.
If the handoff references a file path, read THAT file directly.

### Step 2: Process each task

For each task in the handoff:
1. **Verdict first**: `Confirmed` / `Correction needed` / `Deferred`
2. **Evidence** (2–3 lines): facts, logic, or file references
3. **Recommended action** (1 line, actionable)

### Step 3: Write your response

```markdown
---
from: [your agent]
to: [sender agent]
created: YYYY-MM-DD HH:mm
re: [handoff topic]
status: DONE
---
# Response: [Handoff Topic]

## Summary (3 lines max)
[Most important finding or verdict]

## Task Results

### [Task A] [Title]
- Verdict: Confirmed | Correction needed | Deferred
- Evidence: [2–3 lines]
- Action: [1 line]

### [Task B] [Title]
- Verdict: ...
- Evidence: ...
- Action: ...

## Immediate next steps for sender
1. [Most urgent]
2. [Next]
```

### Step 4: Update the incoming handoff status

Change `status: PENDING` → `status: DONE` in the incoming file.

### Step 5: Print completion

```
✅ Response written: ./handoff/[your-response-file]

Tell the sender:
────────────────────────────────────
Run /check-handoff to see my response.
────────────────────────────────────
```

## Writing Rules

| Do | Don't |
|----|-------|
| Lead with verdict | Start with "Great question!" |
| Note logic gaps | Give unconditional agreement |
| Give actionable next steps | Leave vague suggestions |
| Stay under 50 lines | Copy full session history |

**Target file size: 30–50 lines (~500–800 tokens)**
