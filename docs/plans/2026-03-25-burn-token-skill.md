# Burn Token Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create a Claude Code skill that continuously consumes tokens through meaningful work (code analysis, summarization, math computation) with configurable duration, quantity, and scheduling support.

**Architecture:** Skill dispatches an infinite loop of subagents performing work against the current project. A controller loop detects project content to choose strategy (code analysis vs math), tracks elapsed time/iterations, and respects stop conditions. Scheduling leverages the existing `/loop` and `/schedule` skills.

**Tech Stack:** Claude Code Skill (Markdown), Bash (time tracking), Agent tool (subagent dispatch)

---

## File Structure

```
~/.claude/skills/burn-token/
  SKILL.md              # Main skill file - all logic and flow
```

No additional files needed. The skill is self-contained documentation that instructs Claude how to behave.

---

## Task 1: Create Skill Directory and SKILL.md Skeleton

**Files:**
- Create: `~/.claude/skills/burn-token/SKILL.md`

- [ ] **Step 1: Create the skill directory**

```bash
mkdir -p ~/.claude/skills/burn-token
```

- [ ] **Step 2: Write SKILL.md with frontmatter and overview**

```markdown
---
name: burn-token
description: Use when user wants to continuously consume tokens by performing repeated analysis, summarization, or computation. Supports time-limited burns, iteration-limited burns, and scheduled/recurring execution.
---
```

- [ ] **Step 3: Commit**

```bash
git add ~/.claude/skills/burn-token/SKILL.md
git commit -m "feat: add burn-token skill skeleton"
```

---

## Task 2: Implement Project Detection Logic

The skill needs to determine what strategy to use based on project contents.

**Files:**
- Modify: `~/.claude/skills/burn-token/SKILL.md`

- [ ] **Step 1: Add project detection section**

Skill should instruct Claude to:
1. Run `find . -name '*.py' -o -name '*.js' -o -name '*.ts' -o -name '*.go' -o -name '*.rs' -o -name '*.java' -o -name '*.md' -o -name '*.txt' | head -20` to detect project files
2. If files found → use **Code Analysis Strategy**
3. If no files found → use **Math Computation Strategy**

Decision flowchart:

```markdown
## Project Detection

On invocation, detect project type:

1. Use Glob to search for source files (`**/*.{py,js,ts,go,rs,java,rb,c,cpp,swift}`)
2. Use Glob to search for documents (`**/*.{md,txt,rst,doc}`)
3. If source files exist → **Code Analysis Mode**
4. Else if documents exist → **Document Analysis Mode**
5. Else → **Math Computation Mode**
```

- [ ] **Step 2: Commit**

---

## Task 3: Implement Code Analysis Strategy

**Files:**
- Modify: `~/.claude/skills/burn-token/SKILL.md`

- [ ] **Step 1: Add code analysis loop**

This section defines repeatable analysis tasks Claude cycles through:

```markdown
## Code Analysis Mode

Cycle through these tasks repeatedly, each in a fresh subagent:

### Round N (repeat):
1. **Architecture Summary** - Read all source files, produce a detailed architecture overview
2. **Code Quality Review** - Analyze code for potential bugs, style issues, complexity
3. **Security Audit** - Check for common vulnerabilities (OWASP top 10 patterns)
4. **Performance Analysis** - Identify potential bottlenecks, suggest optimizations
5. **Test Coverage Analysis** - Identify untested paths, suggest test cases
6. **Refactoring Suggestions** - Identify code smells, propose refactoring
7. **Documentation Generation** - Generate detailed inline documentation
8. **Dependency Analysis** - Analyze dependencies, check for issues
9. **API Surface Review** - Document all public interfaces
10. **Complexity Metrics** - Calculate cyclomatic complexity, nesting depth

Each task should produce verbose, detailed output to maximize token consumption.
After completing all 10 tasks, increment round counter and restart from task 1.
Each round should approach from a different angle or focus area.
```

- [ ] **Step 2: Commit**

---

## Task 4: Implement Math Computation Strategy

**Files:**
- Modify: `~/.claude/skills/burn-token/SKILL.md`

- [ ] **Step 1: Add math computation loop**

```markdown
## Math Computation Mode

When no project files exist, perform intensive mathematical computations:

### Computation Categories (cycle through):
1. **Prime Number Analysis** - Find primes, prove primality, factorize large numbers
2. **Series & Sequences** - Compute partial sums, convergence analysis, Taylor series
3. **Matrix Operations** - Manual matrix multiplication, eigenvalue computation, determinants
4. **Calculus Problems** - Symbolic integration, differential equations, limit proofs
5. **Number Theory** - Modular arithmetic, Chinese Remainder Theorem, Euler's totient
6. **Combinatorics** - Permutations, partitions, generating functions
7. **Probability** - Bayesian problems, Markov chains, expected value computations
8. **Graph Theory** - Shortest paths, coloring problems, network flow
9. **Abstract Algebra** - Group theory problems, ring operations, field extensions
10. **Optimization** - Linear programming (simplex by hand), gradient descent steps

Each problem should be solved with maximum detail:
- Show every intermediate step
- Verify results multiple ways
- Explore edge cases
- Prove correctness

Generate increasingly difficult problems each round.
```

- [ ] **Step 2: Commit**

---

## Task 5: Implement Stop Conditions

**Files:**
- Modify: `~/.claude/skills/burn-token/SKILL.md`

- [ ] **Step 1: Add argument parsing and stop conditions**

```markdown
## Arguments

`/burn-token [options]`

| Argument | Format | Default | Description |
|----------|--------|---------|-------------|
| (none) | - | Run forever | Burn until context limit or user interrupts |
| time | `30m`, `1h`, `2h30m` | - | Stop after specified duration |
| rounds | `5`, `10`, `100` | - | Stop after N complete rounds |
| mode | `code`, `math`, `auto` | `auto` | Force specific mode |

### Examples:
- `/burn-token` — burn indefinitely (default)
- `/burn-token 1h` — burn for 1 hour
- `/burn-token 10 rounds` — burn for 10 complete rounds
- `/burn-token 30m math` — force math mode for 30 minutes

## Controller Loop

At the start of each round:
1. Check elapsed time: `date +%s` compared to start timestamp
2. Check round counter against limit
3. If stop condition met → print summary and stop
4. Else → dispatch next subagent task

Track and display progress:
- Current round / total (if limited)
- Elapsed time / total (if time-limited)
- Tasks completed this session
- Estimated tokens consumed (rough: ~4000 tokens per task)
```

- [ ] **Step 2: Commit**

---

## Task 6: Implement Scheduling Support

**Files:**
- Modify: `~/.claude/skills/burn-token/SKILL.md`

- [ ] **Step 1: Add scheduling section**

```markdown
## Scheduled Execution

For recurring token burns, use the existing scheduling infrastructure:

### Using /loop (interval-based):
```
/loop 1h /burn-token 30m
```
Runs a 30-minute burn every hour.

### Using /schedule (cron-based):
```
/schedule create --cron "0 */4 * * *" --prompt "/burn-token 1h"
```
Runs a 1-hour burn every 4 hours.

### Recommended Schedules:
| Goal | Command |
|------|---------|
| Steady burn | `/loop 2h /burn-token 1h` |
| Business hours | `/schedule create --cron "0 9-17 * * 1-5" --prompt "/burn-token 50m"` |
| Overnight | `/schedule create --cron "0 22 * * *" --prompt "/burn-token 8h"` |
| Maximum burn | `/burn-token` (run indefinitely) |
```

- [ ] **Step 2: Commit**

---

## Task 7: Implement Subagent Dispatch Pattern

**Files:**
- Modify: `~/.claude/skills/burn-token/SKILL.md`

- [ ] **Step 1: Add subagent dispatch instructions**

```markdown
## Execution Pattern

Each analysis/computation task runs as a **separate subagent** to:
- Maximize token consumption (fresh context each time)
- Prevent context window overflow
- Allow independent, verbose output

### Dispatch Template:

For each task, use the Agent tool:
- description: "Burn token: [task name] round [N]"
- prompt: Full task description with instructions to be maximally verbose
- run_in_background: false (sequential to maintain control loop)

### Progress Reporting:

After each subagent completes, print:
```
[burn-token] Round N/M | Task K/10 | Elapsed: XXm | ~NNNK tokens burned
```

### Completion Summary:

When stop condition met:
```
[burn-token] Complete
  Rounds: N
  Tasks: M
  Duration: Xh Ym
  Mode: code/math/auto
  Estimated tokens: ~NNNK
```
```

- [ ] **Step 2: Commit**

---

## Task 8: Assemble Final SKILL.md

**Files:**
- Modify: `~/.claude/skills/burn-token/SKILL.md`

- [ ] **Step 1: Assemble all sections into complete, polished SKILL.md**

Combine all sections in this order:
1. Frontmatter (name, description)
2. Overview (1-2 sentences)
3. Arguments table
4. Project Detection flowchart
5. Code Analysis Mode
6. Document Analysis Mode (subset of code analysis)
7. Math Computation Mode
8. Controller Loop & Stop Conditions
9. Subagent Dispatch Pattern
10. Scheduling Support
11. Progress Reporting

- [ ] **Step 2: Review for CSO (Claude Search Optimization)**

Verify:
- Description starts with "Use when..."
- Keywords present: burn, token, consume, analysis, computation, schedule, loop
- Name is hyphenated, lowercase

- [ ] **Step 3: Test the skill manually**

```bash
# Verify skill is discoverable
ls ~/.claude/skills/burn-token/SKILL.md
```

- [ ] **Step 4: Final commit**

```bash
git add ~/.claude/skills/burn-token/SKILL.md
git commit -m "feat: complete burn-token skill"
```

---

## Summary

| Task | Description | Est. Complexity |
|------|-------------|----------------|
| 1 | Skeleton + frontmatter | Simple |
| 2 | Project detection logic | Simple |
| 3 | Code analysis strategy (10 task types) | Medium |
| 4 | Math computation strategy (10 categories) | Medium |
| 5 | Stop conditions (time/rounds/unlimited) | Medium |
| 6 | Scheduling support (/loop, /schedule) | Simple |
| 7 | Subagent dispatch pattern | Medium |
| 8 | Final assembly + polish | Simple |
