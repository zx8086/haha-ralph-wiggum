/ralph-wiggum:ralph-loop "
## Objective
Fix the bug: [DESCRIBE BUG - e.g., API returns 500 on invalid input]

## CLAUDE.md Compliance (CRITICAL - if project has CLAUDE.md)
This prompt may operate within a project that has a CLAUDE.md file. If present, you MUST follow these rules:
- **NO EMOJIS**: Do not use emojis anywhere (use GOOD/BAD instead of symbols)
- **NO AUTO-COMMITS**: NEVER commit without explicit user authorization
- **COMMIT FORMAT**: If authorized to commit, follow project's commit format (e.g., ISSUE-XX: message)
- **RUNTIME**: Use project's specified runtime (e.g., bun not npm/node)
- **ZERO FAILING TESTS**: All tests must pass before marking SUCCESS
- **CHECK CLAUDE.md FIRST**: Read and follow any additional project-specific rules

## Environment Setup
All dependencies are configured and working:
- Environment variables are in .env file (already loaded)
- [Database/Cache]: Running and accessible (if needed)
- [Other services]: Running and accessible (if needed)
- All services work - do NOT skip fixes claiming 'infrastructure unavailable'

## Git Strategy
[CHOOSE ONE STRATEGY - delete the others]

### Option A: No Commits During Loop (RECOMMENDED - respects CLAUDE.md)
- Do NOT commit during iterations
- Make all changes in working directory
- On SUCCESS: Stage changes only (git add -A), do NOT commit
- User will review and commit manually after loop completes

### Option B: Commit Each Fix Step (REQUIRES USER PRE-AUTHORIZATION)
- Only use if user explicitly authorized commits before starting loop
- Commit after each successful verification
- Commit format: Follow project's CLAUDE.md commit format (e.g., ISSUE-XX: fix: description)

## IMPORTANT: Take Action Each Iteration
Do NOT just analyze. Each iteration MUST:
1. Run a command
2. Read the output
3. Make a change (modify source or test file)
4. Verify the change worked

If you find yourself looping without making changes, STOP and take action.

## Progress Tracking
At the START of each iteration, output:
- Iteration: [N of 15]
- Last action: [what was done]
- Current state: [passing/failing]
- Next action: [what will be done]

## Hypothesis Tracking
Before each fix attempt, state:
- Hypothesis: [what you think is wrong]
- Evidence: [why you think this]
- Test: [how you'll verify]

## Available Commands
- Run tests: bun test
- Run specific test: bun test [filename]
- Type check: bun run typecheck
- Lint: bun run lint

## Process

### Step 1: Reproduce the Bug
Run: bun test --grep '[test name or pattern]'
Confirm the bug exists and understand the failure.

### Step 2: Locate Root Cause & Fix
Read error messages and stack traces.
Identify the source file and line causing the issue.
Implement a minimal, focused fix.

### Step 3: Verify Fix
Run: bun test
Confirm the failing test now passes AND no other tests broke.

### Step 4: Add Regression Test (if none exists)
Write a test that specifically catches this bug.
Ensure it would fail without the fix.

## Quality Rules
- Fix the root cause, not symptoms - GOOD
- Add workarounds or hacks - BAD
- Write focused, minimal changes - GOOD
- Refactor unrelated code - BAD

## Scope Guard
Stay focused on fixing the stated bug. Do NOT:
- Fix unrelated issues you discover
- Refactor code outside the bug area
- Add improvements not explicitly requested
If you notice other issues, log them but do not fix them.

## Constraints
- Do NOT change test expectations to make tests pass
- Do NOT disable or skip failing tests
- Do NOT introduce new dependencies without justification
- Do NOT commit without user authorization

## Recovery Strategy
If stuck in a failing state:
- Revert last change: git checkout -- [file]
- Or revert all uncommitted changes: git checkout -- .
- Re-run verification command
- Try alternative approach based on new hypothesis
- After 3 failed attempts at same issue, document blocker and output BLOCKED status

## Verification Checkpoints (every 5 iterations)
Run: git diff --stat
Review: Are changes aligned with fixing the bug?
If scope has drifted, reset and refocus.

## Completion (IMPORTANT - Read Carefully)
When finished, you MUST output the exact string: RALPH_DONE

## Early Exit Conditions
Output RALPH_DONE immediately if:
- Bug is fixed and regression test passes
- Cannot reproduce the bug (document steps tried)
- Bug requires architectural changes beyond scope

Output RALPH_DONE with status SUCCESS when:
- The bug is fixed
- All tests pass (ZERO FAILING TESTS)
- A regression test exists for this bug
- Changes staged but NOT committed (user will review)
Format: <promise>RALPH_DONE</promise> STATUS: SUCCESS - [summary of fix]

Output RALPH_DONE with status BLOCKED when:
- Cannot reproduce the bug after 5 attempts
- Fix requires architectural changes beyond scope
- No progress after 10 iterations
Format: <promise>RALPH_DONE</promise> STATUS: BLOCKED - [reason and what was tried]

The loop will ONLY stop when you output <promise>RALPH_DONE</promise>
" --max-iterations 15 --completion-promise "RALPH_DONE"
