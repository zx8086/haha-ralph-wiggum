/ralph-wiggum:ralph-loop "
## Objective
Improve test coverage and mutation score for this project.

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
- All services work - do NOT skip tests claiming 'infrastructure unavailable'

## Git Strategy
[CHOOSE ONE STRATEGY - delete the others]

### Option A: No Commits During Loop (RECOMMENDED - respects CLAUDE.md)
- Do NOT commit during iterations
- Make all changes in working directory
- On SUCCESS: Stage changes only (git add -A), do NOT commit
- User will review and commit manually after loop completes

### Option B: Commit Per File Covered (REQUIRES USER PRE-AUTHORIZATION)
- Only use if user explicitly authorized commits before starting loop
- Commit after completing tests for each source file
- Commit format: Follow project's CLAUDE.md commit format (e.g., ISSUE-XX: test: add coverage for filename)

### Option C: Commit at Checkpoints (REQUIRES USER PRE-AUTHORIZATION)
- Only use if user explicitly authorized commits before starting loop
- Commit every 5 iterations (at verification checkpoints)
- Commit format: Follow project's CLAUDE.md commit format

## IMPORTANT: Take Action Each Iteration
Do NOT just analyze. Each iteration MUST:
1. Run a command
2. Read the output
3. Make a change (write/modify a test file)
4. Verify the change worked

If you find yourself looping without writing code, STOP and write a test.

## Progress Tracking
At the START of each iteration, output:
- Iteration: [N of 25]
- Last action: [what was done]
- Current coverage: [X%]
- Current mutation score: [Y%]
- Next action: [what will be done]

## Available Commands
- Coverage: bun test --coverage
- Mutation (fast): bun run test:mutation:incremental
- Mutation (full): bun run test:mutation

## Process

### Step 1: Assess Current State
Run: bun test --coverage
Identify the file with LOWEST coverage percentage (ignore test/ files).

### Step 2: Take Action
Write tests for uncovered lines:
- Execute those lines
- Assert SPECIFIC values (not just .toBeTruthy())
- Would FAIL if code logic changed

### Step 3: Verify Progress
Run: bun test --coverage
Confirm coverage increased. If yes, continue to next file. If no, fix the test.

### Step 4: Mutation Test (every 3rd iteration)
Run: bun run test:mutation:incremental
Kill any surviving mutants by adding specific assertions.

## Quality Rules
- expect(x).toBe(42) - GOOD
- expect(x).toBeTruthy() - BAD
- expect(fn).toThrow('specific message') - GOOD
- expect(fn).toThrow() - BAD
- Test edge cases and boundaries - GOOD
- Only test happy path - BAD

## Scope Guard
Stay focused on improving test coverage. Do NOT:
- Modify production source code to increase coverage
- Fix bugs you discover (log them instead)
- Refactor production code
If you notice issues, log them but do not fix them.

## Constraints
- Do NOT modify production source code to increase coverage
- Do NOT add istanbul ignore comments or stryker ignore comments
- Do NOT commit without user authorization
- Skip test helper files (test/shared/*, test/integration/setup.ts)

## Recovery Strategy
If stuck in a failing state:
- Revert last change: git checkout -- [file]
- Or revert all uncommitted changes: git checkout -- .
- Re-run verification command
- Try testing a different file
- After 3 failed attempts at same issue, document blocker and output BLOCKED status

## Verification Checkpoints (every 5 iterations)
Run: git diff --stat
Review: Are changes only in test files?
If production code was modified, revert and refocus.

## Completion (IMPORTANT - Read Carefully)
When finished, you MUST output the exact string: RALPH_DONE

## Early Exit Conditions
Output RALPH_DONE immediately if:
- Coverage >=90% and mutation score >=80%
- All remaining uncovered lines are unreachable/dead code
- Build is broken and cannot be fixed

Output RALPH_DONE with status SUCCESS when:
- Line coverage >=90% overall
- All testable code paths covered
- Mutation score >=80%
- All tests pass (ZERO FAILING TESTS)
- Changes staged but NOT committed (user will review)
Format: <promise>RALPH_DONE</promise> STATUS: SUCCESS - [summary: before/after coverage, mutation score]

Output RALPH_DONE with status BLOCKED when:
- Build is broken and cannot be fixed after 3 tries
- Tests fail and cannot be fixed after 3 tries
- No measurable progress after 10 iterations
Format: <promise>RALPH_DONE</promise> STATUS: BLOCKED - [reason and what was tried]

The loop will ONLY stop when you output <promise>RALPH_DONE</promise>
" --max-iterations 25 --completion-promise "RALPH_DONE"
