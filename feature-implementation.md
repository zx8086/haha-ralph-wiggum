/ralph-wiggum:ralph-loop "
## Objective
Implement feature: [DESCRIBE FEATURE]

## CLAUDE.md Compliance (CRITICAL - if project has CLAUDE.md)
This prompt may operate within a project that has a CLAUDE.md file. If present, you MUST follow these rules:
- **NO EMOJIS**: Do not use emojis anywhere (use GOOD/BAD instead of symbols)
- **NO AUTO-COMMITS**: NEVER commit without explicit user authorization
- **COMMIT FORMAT**: If authorized to commit, follow project's commit format (e.g., ISSUE-XX: message)
- **RUNTIME**: Use project's specified runtime (e.g., bun not npm/node)
- **ZERO FAILING TESTS**: All tests must pass before marking SUCCESS
- **CHECK CLAUDE.md FIRST**: Read and follow any additional project-specific rules

## Requirements
[LIST SPECIFIC REQUIREMENTS]
- [Requirement 1]
- [Requirement 2]
- [Requirement 3]

## Acceptance Criteria
- [ ] [Criterion 1 - testable statement]
- [ ] [Criterion 2 - testable statement]
- [ ] [Criterion 3 - testable statement]
Check off as you complete each.

## Environment Setup
All dependencies are configured and working:
- Environment variables are in .env file (already loaded)
- [Database/Cache]: Running and accessible (if needed)
- [Other services]: Running and accessible (if needed)
- All services work - do NOT skip implementation claiming 'infrastructure unavailable'

## Git Strategy
[CHOOSE ONE STRATEGY - delete the others]

### Option A: No Commits During Loop (RECOMMENDED - respects CLAUDE.md)
- Do NOT commit during iterations
- Make all changes in working directory
- On SUCCESS: Stage changes only (git add -A), do NOT commit
- User will review and commit manually after loop completes

### Option B: Commit Each Acceptance Criterion (REQUIRES USER PRE-AUTHORIZATION)
- Only use if user explicitly authorized commits before starting loop
- Commit after each acceptance criterion is met
- Commit format: Follow project's CLAUDE.md commit format (e.g., ISSUE-XX: feat: criterion description)
- This creates logical, reviewable commits

### Option C: Commit at Checkpoints (REQUIRES USER PRE-AUTHORIZATION)
- Only use if user explicitly authorized commits before starting loop
- Commit every 5 iterations (at verification checkpoints)
- Commit format: Follow project's CLAUDE.md commit format

## IMPORTANT: Take Action Each Iteration
Do NOT just analyze. Each iteration MUST:
1. Run a command
2. Read the output
3. Make a change (write code or tests)
4. Verify the change worked

If you find yourself looping without writing code, STOP and write code.

## Progress Tracking
At the START of each iteration, output:
- Iteration: [N of 30]
- Last action: [what was done]
- Current state: [passing/failing]
- Next action: [what will be done]
- Acceptance criteria completed: [X of Y]

## Available Commands
- Run tests: bun test
- Type check: bun run typecheck
- Build: bun run build
- Dev server: bun run dev

## Process

### Step 1: Write Failing Test
Create a test that defines expected behavior for the next requirement.
Run: bun test
Confirm test fails (red).

### Step 2: Implement Minimum Code
Write just enough code to make the test pass.
Run: bun test
Confirm test passes (green).

### Step 3: Refactor If Needed
Clean up the code while keeping tests green.
Run: bun test && bun run typecheck

### Step 4: Next Requirement (every iteration)
Check off completed acceptance criterion.
Move to the next requirement. Repeat Steps 1-3.

## Quality Rules
- Write test first (TDD) - GOOD
- Write code without tests - BAD
- Small incremental changes - GOOD
- Large monolithic changes - BAD
- Handle edge cases - GOOD
- Only happy path - BAD

## Scope Guard
Stay focused on the stated requirements. Do NOT:
- Add features not in the requirements
- Refactor code outside the feature area
- Add improvements not explicitly requested
If you notice other issues, log them but do not fix them.

## Constraints
- Do NOT skip writing tests
- Do NOT break existing functionality
- Do NOT deviate from requirements
- Do NOT commit without user authorization
- Follow existing code patterns and conventions

## Recovery Strategy
If stuck in a failing state:
- Revert last change: git checkout -- [file]
- Or revert all uncommitted changes: git checkout -- .
- Re-run verification command
- Simplify the implementation approach
- After 3 failed attempts at same issue, document blocker and output BLOCKED status

## Verification Checkpoints (every 5 iterations)
Run: git diff --stat
Review: Are changes aligned with requirements?
If scope has drifted, reset and refocus.

## Completion (IMPORTANT - Read Carefully)
When finished, you MUST output the exact string: RALPH_DONE

## Early Exit Conditions
Output RALPH_DONE immediately if:
- All acceptance criteria are met
- Requirements are unclear or contradictory (document questions)
- Implementation requires unavailable dependencies

Output RALPH_DONE with status SUCCESS when:
- All requirements are implemented
- All acceptance criteria checked off
- All tests pass (ZERO FAILING TESTS)
- Type checking passes
- Code follows project conventions
- Changes staged but NOT committed (user will review)
Format: <promise>RALPH_DONE</promise> STATUS: SUCCESS - [summary of implementation]

Output RALPH_DONE with status BLOCKED when:
- Requirements are unclear or contradictory
- Implementation requires unavailable dependencies
- No progress after 10 iterations
Format: <promise>RALPH_DONE</promise> STATUS: BLOCKED - [reason and what was tried]

The loop will ONLY stop when you output <promise>RALPH_DONE</promise>
" --max-iterations 30 --completion-promise "RALPH_DONE"
