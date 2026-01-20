# Ralph Wiggum Prompt Templates

A collection of structured prompt templates for the [Ralph Wiggum](https://github.com/anthropics/claude-code/tree/main/plugins/ralph-wiggum) Claude Code plugin.

## What is Ralph Wiggum?

Ralph Wiggum is a Claude Code plugin that enables **autonomous AI agent loops**. Based on Geoffrey Huntley's methodology, it uses a Stop hook to intercept Claude's exit attempts, feeding the same prompt back repeatedly until a completion condition is met.

### How It Works

1. You run `/ralph-loop "Your task" --max-iterations N --completion-promise "DONE"`
2. Claude works on the task
3. Claude tries to exit
4. The Stop hook blocks the exit and re-feeds the prompt
5. Claude continues working
6. Loop ends when Claude outputs the completion promise string

### Installation

```bash
/plugin install ralph-wiggum@claude-plugins-official
```

### Basic Commands

| Command | Purpose |
|---------|---------|
| `/ralph-loop "task" --max-iterations N` | Start an autonomous loop |
| `/cancel-ralph` | Stop the active loop |
| `/ralph-loop:help` | Show documentation |

## Templates

This repository contains ready-to-use prompt templates for common development tasks:

| Template | File | Use Case | Suggested Iterations |
|----------|------|----------|---------------------|
| Base Template | [template.md](template.md) | Starting point for custom prompts | varies |
| Test Coverage | [test-coverage.md](test-coverage.md) | Improve coverage & mutation score | 25 |
| Bug Fix | [bug-fix.md](bug-fix.md) | Fix specific bugs | 15 |
| Refactoring | [refactoring.md](refactoring.md) | Clean up code without changing behavior | 20 |
| Feature Implementation | [feature-implementation.md](feature-implementation.md) | Build new functionality with TDD | 30 |

## Template Structure

All templates follow a consistent structure:

```
## Objective
What needs to be accomplished

## Environment Setup
Available services and infrastructure

## IMPORTANT: Take Action Each Iteration
Reminder to make changes, not just analyze

## Available Commands
Commands Claude should use

## Process
Step-by-step workflow

## Quality Rules
Do/Don't guidelines with examples

## Constraints
What Claude should NOT do

## Completion
Success/Blocked criteria and promise format
```

## Usage

### Using a Template

1. Copy the contents of the desired template
2. Replace placeholder text in `[BRACKETS]` with your specifics
3. Run with `/ralph-loop`

### Example: Bug Fix

```bash
/ralph-loop "
## Objective
Fix the bug: Users get 500 error when submitting empty form

## Environment Setup
All dependencies are configured and working:
- Environment variables are in .env file (already loaded)
- Database: Running and accessible
...
" --max-iterations 15 --completion-promise "RALPH_DONE"
```

## Best Practices

### Always Set Iteration Limits

```bash
# Good - explicit limit
/ralph-loop "task" --max-iterations 20

# Risky - might run indefinitely
/ralph-loop "task"
```

### Use Specific Completion Conditions

The `--completion-promise` uses exact string matching. Structure your prompts to output a clear completion string:

```
Format: <promise>RALPH_DONE</promise> STATUS: SUCCESS - [summary]
```

### Break Large Tasks into Phases

```bash
# Phase 1: Setup
/ralph-loop "Phase 1: Create data models..." --max-iterations 15

# Phase 2: Implementation  
/ralph-loop "Phase 2: Build API endpoints..." --max-iterations 20

# Phase 3: Testing
/ralph-loop "Phase 3: Write integration tests..." --max-iterations 15
```

## Cost Awareness

Autonomous loops consume tokens continuously. A 50-iteration loop on a large codebase can cost $50-100+ depending on context size. Always:

- Set reasonable `--max-iterations` limits
- Use phased approaches for large tasks
- Monitor progress and cancel if stuck

## Creating Custom Templates

Start with `template.md` and customize:

1. Define a clear, measurable objective
2. List specific commands Claude should use
3. Create a step-by-step process
4. Add quality rules with good/bad examples
5. Set explicit success criteria
6. Define when to report as BLOCKED

## Requirements

- Claude Code with the Ralph Wiggum plugin installed
- `jq` installed (required dependency)

## License

MIT
