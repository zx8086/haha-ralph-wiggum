/ralph-wiggum:ralph-loop "
## Objective
Improve test coverage and mutation score for this TypeScript/Bun project.

## CLAUDE.md Compliance (CRITICAL)
This prompt operates within a project that has a CLAUDE.md file. You MUST follow these rules:
- **NO EMOJIS**: Do not use emojis anywhere (use GOOD/BAD instead of checkmarks)
- **NO AUTO-COMMITS**: NEVER commit without explicit user authorization
- **COMMIT FORMAT**: If authorized to commit, use format: SIO-XX: message (or descriptive message if no Linear issue)
- **BUN ONLY**: Use bun commands, never npm/node/npx
- **ZERO FAILING TESTS**: All tests must pass before marking SUCCESS

## Environment Setup
All dependencies are configured and working - DO NOT start any services:
- Environment variables are in .env file (already loaded)
- Redis is already running and accessible
- OpenTelemetry Collector is already running and accessible
- All services work - do NOT skip tests claiming 'infrastructure unavailable'
- Do NOT run docker commands, docker-compose, or any service startup scripts
- Do NOT attempt to start/stop/restart Redis, Kong, or any other service
- The environment is ready - just run tests

## Git Strategy
[CHOOSE ONE STRATEGY - delete the others]

### Option A: No Commits During Loop (RECOMMENDED)
- Do NOT commit during iterations
- Make all changes in working directory
- On SUCCESS: Stage changes only (git add -A), do NOT commit
- User will review and commit manually after loop completes

### Option B: Commit Each Milestone (REQUIRES USER PRE-AUTHORIZATION)
- Only use if user explicitly authorized commits before starting loop
- Commit after completing tests for each source file
- Commit format: SIO-XX: test: add coverage for [filename]

## IMPORTANT: Take Action Each Iteration
Do NOT just analyze. Each iteration MUST:
1. Run a command
2. Read the output
3. Make a change (write/modify a test file)
4. Verify the change worked

If you find yourself looping without writing code, STOP and write a test.

EXCEPTION: If mutation test is running AND you have exhausted easy coverage improvements (all remaining uncovered code is Tier 3 difficulty), then WAITING is a valid action. In this case:
- Do NOT burn iterations by repeatedly checking mutation status
- Use a SINGLE long wait: sleep 1200 (20 minutes) or sleep 1800 (30 minutes)
- After the long wait, check mutation status ONCE
- If still running, wait again with another long sleep
- This preserves iterations for actual work

## Progress Tracking
At the START of each iteration, output:
- Iteration: [N of 25]
- Last action: [what was done]
- Current coverage: [X%]
- Current mutation score: [Y%]
- Next action: [what will be done]

## Available Commands
- Coverage (unit tests only): bun test test/bun --coverage
- Coverage (all tests): bun run test:bun:coverage
- Mutation (recommended): bun run test:mutation:fast
- Mutation (incremental): bun run test:mutation:incremental
- Mutation (fresh - clears cache): bun run test:mutation:fresh
- Mutation (services only): bun run test:mutation:services
- Mutation (handlers only): bun run test:mutation:handlers
- Mutation (dry run - verify config): bun run test:mutation:dry

NOTE: Integration tests (test/integration/) will show "Skipping: Integration environment not available" - this is expected and OK. Those tests require docker-compose which is not running. Focus on unit tests in test/bun/ for coverage improvements.

## Mutation Testing Requirements (CRITICAL)
- Mutation tests are SLOW due to Bun/StrykerJS compatibility issues
- Expected runtime: 30 minutes to 1+ hour for full runs - this is NORMAL
- You MUST complete at least ONE full mutation test run before reporting final status
- "Too slow" or "taking too long" is NOT a valid reason to skip mutation verification
- Start mutation tests early in the loop so they have time to complete
- If mutation tests error out or fail to start, report BLOCKED with the error message

### Mutation Test Configuration Notes
- Reporters are set to clear-text and progress only (no HTML/JSON) to avoid memory errors with large reports
- Results are output to console - read the final summary for mutation score
- Current baseline: ~56% mutation score (1100 killed, 862 survived out of 8024 mutants)
- Target: 85% mutation score
- Low-scoring files to prioritize: cache-factory.ts (14%), typed-metrics-poc.ts (17%), metrics.ts (27%), redis-instrumentation.ts (34%), openapi-generator.ts (41%)
- High-scoring files (already good): config.ts (100%), defaults.ts (100%), tokens.ts (100%), jwt.service.ts (93%)

### Known Technical Challenges
When writing tests, be aware of these patterns that DON'T work well:
- AVOID: Using delete require.cache[...] to reset modules - fragments coverage tracking
- AVOID: Isolated test files with module resets - can DROP coverage instead of improving it
- AVOID: Repeated analysis without action - write tests, don't just monitor
- PREFER: Adding tests to existing test files when possible
- PREFER: Dependency injection patterns over module cache manipulation
- PREFER: Testing exported functions directly rather than trying to mock module loading

### Running Mutation Tests in Background
Run mutation tests in background to continue working on coverage while waiting:
1. Start: Run mutation command with run_in_background=true
2. Monitor: Periodically check progress using BashOutput tool
3. Progress output looks like: Mutation testing [====] 13% (elapsed: ~1m, remaining: ~8m) 336/2563 Mutants tested
4. Continue writing coverage tests while mutation runs
5. Before final status: MUST wait for mutation test to complete and report the score
6. Final output will show mutation score - use this for your status report

### Understanding Iterations vs Time
IMPORTANT: Iterations are agent turns, NOT time intervals. Mutation tests run in REAL TIME in the background regardless of how many iterations you use. Strategy:
- Use iterations productively: write tests, improve coverage
- Check mutation progress periodically (every 2-3 iterations)
- The mutation test will complete based on ELAPSED TIME, not iteration count
- If mutation shows "remaining: ~19m", that means 19 real minutes - keep working on coverage
- When mutation reaches 100%, THEN evaluate thresholds and report status
- Do NOT panic about iteration count - focus on productive work while waiting

### Waiting Strategy When Stuck
If you have written tests but coverage is not improving AND mutation test is still running:
- Do NOT report BLOCKED or PARTIAL while mutation is running
- Do NOT burn iterations with frequent short waits
- Use LONG wait intervals: sleep 1200 (20 min) or sleep 1800 (30 min)
- After the long wait, check BashOutput ONCE for mutation progress
- If mutation still running, do another long sleep - do NOT keep checking
- Continue waiting until mutation reaches 100%
- Only after mutation completes, evaluate final status based on BOTH metrics
- Goal: Use 2-3 iterations for waiting, not 10+ iterations

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

## Effective Test Patterns (from past sessions)
These patterns have proven effective:
- Comprehensive method-level testing (e.g., winston logger: 23 tests covering all methods)
- Production validation rules (HTTPS enforcement, secure tokens)
- Edge case scenarios (PING failures, timeouts, non-Error objects)
- Schema validation with various data types (string, object, number, array, nested)
- Health check error scenarios (timeout, non-PONG response, missing methods)

## Coverage Difficulty Tiers
Tier 1 (Easy - target first):
- Handler functions, service methods, utility functions
- Schema validation, configuration parsing

Tier 2 (Medium):
- Error handling paths, edge cases
- Cache operations, health checks

Tier 3 (Hard - may not be worth the effort):
- Module load failures (require/import failures)
- Environment variable fallback paths requiring complex env manipulation
- OpenTelemetry no-op exporters in console mode
- Winston logger load failure scenarios
These Tier 3 paths often require complex setup that risks destabilizing other tests

## Scope Guard
Stay focused on improving test coverage. Do NOT:
- Modify production source code to increase coverage
- Fix bugs you discover (log them instead)
- Refactor production code
If you notice issues, log them but do not fix them.

## Constraints
- Do NOT modify production source code to increase coverage
- Do NOT add istanbul ignore comments or stryker ignore comments
- Do NOT commit without user authorization (stage only)
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

CRITICAL: You CANNOT output RALPH_DONE while a mutation test is still running. You MUST wait for the mutation test to complete (100%) before reporting ANY status. If mutation test shows any percentage less than 100%, you are NOT DONE - keep iterating and check progress again.

## Early Exit Conditions
Output RALPH_DONE immediately if:
- Coverage >=92% AND mutation score >=85% AND mutation test has completed (not still running)
- Build is broken and cannot be fixed

## Exit Status Definitions (STRICT - NO EXCEPTIONS)

### SUCCESS (all conditions MUST be met)
Output RALPH_DONE with status SUCCESS when:
- Line coverage >=92% overall (MANDATORY)
- Mutation score >=85% (MANDATORY)
- All tests pass (ZERO FAILING TESTS)
- Changes staged but NOT committed (user will review)
Format: <promise>RALPH_DONE</promise> STATUS: SUCCESS - [coverage]% line / [mutation]% mutation - [summary]

### PARTIAL (use when progress was made but not all thresholds met)
Output RALPH_DONE with status PARTIAL when mutation test has COMPLETED (100%) AND any of these:
- Coverage improved but is below 92% (even if mutation score exceeds 85%)
- Mutation score improved but is below 85% (even if coverage exceeds 92%)
- One metric met target but the other did not
- All tests pass (ZERO FAILING TESTS)
Format: <promise>RALPH_DONE</promise> STATUS: PARTIAL - [coverage]% line / [mutation]% mutation - [summary of what was achieved and what remains]

IMPORTANT: PARTIAL is the correct status when you made good progress but didn't hit ALL thresholds. If mutation score is 98% but coverage is 90%, that is PARTIAL (excellent progress, one metric short). Do NOT report BLOCKED just because one metric stalled - evaluate BOTH metrics.

CRITICAL: You CANNOT report PARTIAL while mutation test is still running. Wait for 100% completion first.

### BLOCKED (use ONLY when genuinely unable to make ANY progress)
Output RALPH_DONE with status BLOCKED ONLY when:
- Build is broken and cannot be fixed after 3 tries
- Tests fail and cannot be fixed after 3 tries
- BOTH coverage AND mutation score show no improvement after 10 iterations of active work

BLOCKED is for catastrophic failures, NOT for "one metric is stuck." If coverage stalled but mutation score improved significantly, that is PARTIAL, not BLOCKED.

Format: <promise>RALPH_DONE</promise> STATUS: BLOCKED - [reason and what was tried]

The loop will ONLY stop when you output <promise>RALPH_DONE</promise>
" --max-iterations 25 --completion-promise "RALPH_DONE"
