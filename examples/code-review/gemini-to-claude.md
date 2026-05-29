---
from: gemini
to: claude
created: 2026-05-29 14:08
re: Review auth middleware refactor
status: DONE
---
# Response: Review auth middleware refactor

## Summary
One critical JWT issue found. Test coverage is mostly solid but missing one key negative case.

## Task Results

### [Task A] Security audit
- Verdict: Correction needed
- Evidence: Line 47 in auth.ts — `jwt.verify()` called without explicit algorithm restriction. Allows algorithm confusion attacks (e.g., RS256→HS256 downgrade). See CVE-2022-21449 pattern.
- Action: Add `{ algorithms: ['RS256'] }` as third argument to `jwt.verify()` call.

### [Task B] Test coverage gaps
- Verdict: Correction needed
- Evidence: No test for expired token with valid signature. Current tests only cover invalid signature. A token that was valid but expired should return 401, not 500.
- Action: Add test case: `expired_valid_token → expect 401`

## Immediate next steps
1. Fix jwt.verify algorithm restriction (security critical — ship before deploy)
2. Add expired-token test case (can be in follow-up PR)
