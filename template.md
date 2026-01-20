/ralph-wiggum:ralph-loop "
## Objective
[DESCRIBE THE GOAL - e.g., Improve test coverage, refactor module, implement feature]

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
- All services work - do NOT skip tasks claiming 'infrastructure unavailable'

## Git Strategy
[CHOOSE ONE STRATEGY - delete the others]

### Option A: No Commits During Loop (RECOMMENDED - respects CLAUDE.md)
- Do NOT commit during iterations
- Make all changes in working directory
- On SUCCESS: Stage changes only (git add -A), do NOT commit
- User will review and commit manually after loop completes

### Option B: Commit Each Milestone (REQUIRES USER PRE-AUTHORIZATION)
- Only use if user explicitly authorized commits before starting loop
- Commit after each successful verification
- Commit format: Follow project's CLAUDE.md commit format
- This creates a detailed history of changes

### Option C: Commit at Checkpoints (REQUIRES USER PRE-AUTHORIZATION)
- Only use if user explicitly authorized commits before starting loop
- Commit every [N] iterations (at verification checkpoints)
- Commit format: Follow project's CLAUDE.md commit format

## IMPORTANT: Take Action Each Iteration
Do NOT just analyze. Each iteration MUST:
1. Run a command
2. Read the output
3. Make a change (write/modify a file)
4. Verify the change worked

If you find yourself looping without making changes, STOP and take action.

## Progress Tracking
At the START of each iteration, output:
- Iteration: [N of max]
- Last action: [what was done]
- Current state: [passing/failing]
- Next action: [what will be done]

## Available Commands
[LIST THE COMMANDS CLAUDE SHOULD USE - use project's runtime]
- [Command 1]: [description]
- [Command 2]: [description]
- [Command 3]: [description]

## Process

### Step 1: Assess Current State
Run: [COMMAND TO CHECK STATUS]
Identify [WHAT TO LOOK FOR - e.g., files needing work, failing tests, gaps].

### Step 2: Take Action
[DESCRIBE THE WORK TO DO]
- [Action guideline 1]
- [Action guideline 2]
- [Action guideline 3]

### Step 3: Verify Progress
Run: [COMMAND TO VERIFY]
Confirm [SUCCESS CRITERIA]. If yes, continue. If no, fix and retry.

### Step 4: Periodic Check (every [N] iterations)
Run: [SECONDARY VERIFICATION COMMAND]
[DESCRIBE WHAT TO CHECK AND HOW TO RESPOND]

## Quality Rules
[LIST SPECIFIC DO/DON'T GUIDELINES]
- [GOOD PATTERN] - GOOD
- [BAD PATTERN] - BAD
- [GOOD PATTERN] - GOOD
- [BAD PATTERN] - BAD

## Scope Guard
Stay focused on the stated objective. Do NOT:
- Fix unrelated issues you discover
- Refactor code outside the target area
- Add improvements not explicitly requested
If you notice other issues, log them but do not fix them.

## Constraints
[LIST THINGS CLAUDE SHOULD NOT DO]
- Do NOT [constraint 1]
- Do NOT [constraint 2]
- Do NOT [constraint 3]
- Do NOT commit without user authorization
- [Any files/directories to skip]

## Recovery Strategy
If stuck in a failing state:
- Revert last change: git checkout -- [file]
- Or revert all uncommitted changes: git checkout -- .
- Re-run verification command
- Try alternative approach
- After 3 failed attempts at same issue, document blocker and output BLOCKED status

If NOT using git:
- Keep mental note of last working state
- Manually undo changes if needed
- After 3 failed attempts, document blocker and output BLOCKED status

## Verification Checkpoints (every 5 iterations)
Run: git diff --stat
Review: Are changes aligned with objective?
If scope has drifted, reset and refocus.

## Completion (IMPORTANT - Read Carefully)
When finished, you MUST output the exact string: RALPH_DONE

## Early Exit Conditions
Output RALPH_DONE immediately if:
- Goal achieved before max iterations
- Critical blocker discovered (e.g., missing permissions, architectural issue)
- Task is out of scope for automation

Output RALPH_DONE with status SUCCESS when:
[LIST SUCCESS CRITERIA]
- [Criterion 1]
- [Criterion 2]
- [Criterion 3]
- All tests pass (ZERO FAILING TESTS)
- Changes staged but NOT committed (user will review)
Format: <promise>RALPH_DONE</promise> STATUS: SUCCESS - [summary of achievements]

Output RALPH_DONE with status BLOCKED when:
- Build is broken and cannot be fixed after 3 tries
- [Task type] fails and cannot be fixed after 3 tries
- No measurable progress after 10 iterations
Format: <promise>RALPH_DONE</promise> STATUS: BLOCKED - [reason and what was tried]

The loop will ONLY stop when you output <promise>RALPH_DONE</promise>
" --max-iterations [NUMBER] --completion-promise "RALPH_DONE"
