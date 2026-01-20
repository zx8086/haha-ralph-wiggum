/ralph-wiggum:ralph-loop "
## Objective
Refactor [MODULE/FILE] to [GOAL - e.g., improve readability, reduce complexity, extract shared logic].

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
- All services work - do NOT skip refactoring claiming 'infrastructure unavailable'

## Git Strategy
[CHOOSE ONE STRATEGY - delete the others]

### Option A: No Commits During Loop (RECOMMENDED - respects CLAUDE.md)
- Do NOT commit during iterations
- Make all changes in working directory
- On SUCCESS: Stage changes only (git add -A), do NOT commit
- User will review and commit manually after loop completes

### Option B: Commit Each Refactoring (REQUIRES USER PRE-AUTHORIZATION)
- Only use if user explicitly authorized commits before starting loop
- Commit after each successful refactoring step
- Commit format: Follow project's CLAUDE.md commit format (e.g., ISSUE-XX: refactor: specific change)
- This creates atomic, revertible commits (easy to cherry-pick or revert)

### Option C: Commit at Checkpoints (REQUIRES USER PRE-AUTHORIZATION)
- Only use if user explicitly authorized commits before starting loop
- Commit every 5 iterations (at verification checkpoints)
- Commit format: Follow project's CLAUDE.md commit format

## IMPORTANT: Take Action Each Iteration
Do NOT just analyze. Each iteration MUST:
1. Run a command
2. Read the output
3. Make a change (refactor code)
4. Verify the change worked

If you find yourself looping without making changes, STOP and take action.

## Progress Tracking
At the START of each iteration, output:
- Iteration: [N of 20]
- Last action: [what was done]
- Current state: [passing/failing]
- Next action: [what will be done]

## Metrics (track before/after)
Record at start and update after each change:
- Cyclomatic complexity: [before] -> [current]
- Lines of code: [before] -> [current]
- Function count: [before] -> [current]

## Available Commands
- Run tests: bun test
- Type check: bun run typecheck
- Lint: bun run lint
- Build: bun run build

## Process

### Step 1: Assess Current State
Run: bun test && bun run typecheck
Ensure everything passes before refactoring.
Record baseline metrics.

### Step 2: Make ONE Refactoring Change
Apply a single, focused refactoring:
- Extract function/method
- Rename for clarity
- Simplify conditional
- Remove duplication

### Step 3: Verify
Run: bun test && bun run typecheck
Confirm all tests still pass. If not, revert and try different approach.

### Step 4: Continue (every iteration)
If tests pass, the refactoring is valid.
Update metrics.
Continue to next change.

## Quality Rules
- One refactoring at a time - GOOD
- Multiple changes at once - BAD
- Preserve all existing behavior - GOOD
- Change behavior during refactor - BAD
- Run tests after each change - GOOD
- Batch changes then test - BAD

## Scope Guard
Stay focused on the stated refactoring goal. Do NOT:
- Fix unrelated issues you discover
- Refactor code outside the target area
- Add new features or functionality
If you notice other issues, log them but do not fix them.

## Constraints
- Do NOT change any external API contracts
- Do NOT modify test files (unless renaming to match source)
- Do NOT add new features during refactoring
- Do NOT remove functionality
- Do NOT commit without user authorization

## Recovery Strategy
If stuck in a failing state:
- Revert last change: git checkout -- [file]
- Or revert all uncommitted changes: git checkout -- .
- Re-run verification command
- Try a smaller, more focused refactoring
- After 3 failed attempts at same issue, document blocker and output BLOCKED status

## Verification Checkpoints (every 5 iterations)
Run: git diff --stat
Review: Are changes aligned with refactoring goal?
Compare current metrics to baseline.
If scope has drifted, reset and refocus.

## Completion (IMPORTANT - Read Carefully)
When finished, you MUST output the exact string: RALPH_DONE

## Early Exit Conditions
Output RALPH_DONE immediately if:
- Refactoring goal is achieved
- Further changes would require modifying external APIs
- Code is already well-structured (no improvements possible)

Output RALPH_DONE with status SUCCESS when:
- [REFACTORING GOAL] is achieved
- All tests pass (ZERO FAILING TESTS)
- Type checking passes
- Code is cleaner/simpler than before
- Metrics show improvement
- Changes staged but NOT committed (user will review)
Format: <promise>RALPH_DONE</promise> STATUS: SUCCESS - [summary of refactorings, before/after metrics]

Output RALPH_DONE with status BLOCKED when:
- Refactoring breaks tests that cannot be fixed
- Changes require modifying external APIs
- No progress after 10 iterations
Format: <promise>RALPH_DONE</promise> STATUS: BLOCKED - [reason and what was tried]

The loop will ONLY stop when you output <promise>RALPH_DONE</promise>
" --max-iterations 20 --completion-promise "RALPH_DONE"
