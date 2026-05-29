# /handoff — Multi-Agent Context Handoff

Creates a structured, token-efficient handoff file for another AI agent to pick up.
Auto-detects which agent is running and writes to the correct file.

## File Locations (configure these for your project)

```
HANDOFF_DIR = ./handoff/
CLAUDE_TO_GEMINI = ./handoff/claude-to-gemini.md
GEMINI_TO_CLAUDE = ./handoff/gemini-to-claude.md
ARCHIVE_DIR     = ./handoff/archive/
```

## Execution

### Step 1: Archive existing handoff (if DONE)
Check if your outgoing file exists and its status:
```bash
grep "status:" ./handoff/[your-file].md 2>/dev/null
```
- `status: DONE` → move to archive/ before creating new one
- `status: PENDING` → ask user before overwriting

### Step 2: Collect handoff content
Gather from current session context:
1. **Topic** (3–5 words)
2. **Request summary** (1–2 sentences)
3. **Relevant file paths** (paths only, no content)
4. **Background context** (3 lines max)
5. **Tasks** (numbered, with priority: Critical / Normal / Low)
6. **Expected output format**

### Step 3: Write the handoff file

```markdown
---
from: [claude | gemini | codex | cursor]
to: [receiving agent]
created: YYYY-MM-DD HH:mm
status: PENDING
priority: [P1 | P2 | P3]
---
# Handoff: [Topic]

## Request
[1–2 sentence summary of what you need]

## Context (paths only — never paste file content)
- Related files: [path1], [path2]
- Key decisions: [2–3 lines max]

## Tasks
### [Task A - Critical] [Title]
[Specific question or request]

### [Task B - Normal] [Title]
[Specific question or request]

## Expected Output
- Format: [bullets | table | prose]
- When done: write response file, set status → DONE
```

### Step 4: Print completion message

```
✅ Handoff created: ./handoff/[filename]

Tell the receiving agent:
────────────────────────────────────
Read this file and run /handoff-reply:
[absolute path to handoff file]
────────────────────────────────────
No session history needed — all context is in the file.
```

## Writing Rules

| Do | Don't |
|----|-------|
| Reference files by path | Paste file contents |
| Keep under 50 lines | Copy session history |
| Specify exact questions | Write "please review generally" |
| Set priority level | Leave tasks ambiguous |

**Target file size: 30–50 lines (~500–800 tokens)**
