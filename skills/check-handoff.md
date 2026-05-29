# /check-handoff — Check Response from Another Agent

Reads the response file from the receiving agent, summarizes key findings, and archives the completed handoff pair.

## Execution

### Step 1: Find your incoming response
```bash
# Claude checks this:
cat ./handoff/gemini-to-claude.md

# Gemini checks this:
cat ./handoff/claude-to-gemini.md
```

- `status: DONE` → proceed to Step 2
- `status: PENDING` → "Response not ready yet"
- File missing → "No response file found"

### Step 2: Summarize for the user

Output format:
```
📨 Handoff Response — [Topic]
From: [agent] | Re: [your original request]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Summary:
[3-line summary of key findings]

Immediate actions:
1. [Most urgent]
2. [Next]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Choose:
  [A] Archive (done)
  [B] Archive + add key decisions to session notes
  [C] Send follow-up (/handoff again)
```

### Step 3: Archive (on user confirmation)

```bash
TIMESTAMP=$(date +%Y-%m-%d-%H%M)
TOPIC="[slug from handoff topic]"

# Archive both files
mv ./handoff/claude-to-gemini.md ./handoff/archive/${TIMESTAMP}-C→G-${TOPIC}.md
mv ./handoff/gemini-to-claude.md ./handoff/archive/${TIMESTAMP}-G→C-${TOPIC}.md
```

### Step 4: Optional — Add to session notes

If user selects [B], add to your session notes (concise, key decisions only):

```markdown
## Handoff result — [Topic] ([date])
- [Key verdict 1]
- [Key verdict 2]
→ Full record: handoff/archive/[filename]
```

## Archive Naming Convention

```
YYYY-MM-DD-HHmm-[A→B]-[topic-slug].md

Examples:
2026-06-01-1430-C→G-bm-review.md
2026-06-01-1432-G→C-bm-review.md
```

`C` = Claude, `G` = Gemini, `X` = Codex, `Cu` = Cursor
