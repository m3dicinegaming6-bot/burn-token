---
name: burn-token
description: Use when user wants to burn tokens aggressively - infinite loop of analysis, summarization, or heavy math computation. Maximizes token consumption by defeating KV cache, randomizing prompts, and dispatching verbose subagents. Supports time limits, round limits, or unlimited burn until interrupted.
---

# Burn Token

Relentlessly consume tokens through an infinite loop of heavyweight computation. Designed to burn through even unlimited plans by maximizing output tokens per task, defeating KV cache with entropy injection, and never stopping unless told to.

## Arguments

`/burn-token [duration] [mode] [name]`

| Argument | Format | Default | Example |
|----------|--------|---------|---------|
| duration | `30m`, `1h`, `2h30m`, `forever` | `forever` | `/burn-token 1h` |
| rounds | `N rounds` | unlimited | `/burn-token 50 rounds` |
| mode | `code`, `math`, `bless` | `bless` | `/burn-token 1h math` |
| name | any string (bless mode only) | `众生` | `/burn-token bless dasheng` |

Examples:
- `/burn-token` — infinite bless burn (default mode), 为众生念经
- `/burn-token bless dasheng` — 为 dasheng 念经祈福，烧 token 直到烧完
- `/burn-token 2h bless 大圣` — burn for 2 hours blessing 大圣
- `/burn-token 2h` — burn for 2 hours
- `/burn-token 100 rounds` — burn for 100 rounds
- `/burn-token forever math` — infinite math burn
- `/burn-token 4h code` — 4-hour code analysis burn

## Scheduling

For recurring burns:
```
/loop 1h /burn-token 45m
/schedule create --cron "0 */2 * * *" --prompt "/burn-token 1h45m"
/schedule create --cron "0 0 * * *" --prompt "/burn-token 8h"
```

## CRITICAL: KV Cache Defeat Strategy

KV cache reuse drastically reduces token consumption. Every subagent prompt MUST include entropy to ensure cache misses:

1. **Timestamp injection** — prepend `date +%s%N` nanosecond timestamp to every prompt
2. **Random seed** — generate a random number via `od -An -tu4 -N4 /dev/urandom | tr -d ' '` and embed it in the prompt as "Analysis Seed: {random}"
3. **Shuffled task order** — never run tasks in the same sequence twice
4. **Unique framing** — each prompt must use different phrasing, analogies, and structural approach
5. **Varying output format** — alternate between prose, bullet points, tables, numbered lists, code blocks, Q&A format
6. **Injected context variation** — include different file subsets, line ranges, or problem parameters each time
7. **Temperature words** — sprinkle unique adjectives/adverbs that change per invocation

**Implementation:** Before dispatching each subagent, run:
```bash
ENTROPY_SEED=$(od -An -tu4 -N4 /dev/urandom | tr -d ' ')
TIMESTAMP=$(date +%s%N)
TASK_ID="${TIMESTAMP}-${ENTROPY_SEED}"
```

Embed `TASK_ID` prominently in the subagent prompt. This alone defeats prefix caching.

## Execution Flow

```
START
  |
  v
[Parse arguments: duration, rounds, mode]
  |
  v
[Record start time: date +%s]
  |
  v
[Detect project type if mode=auto → default to bless]
  |  - If mode explicitly set → use that mode
  |  - If mode=auto → use bless mode (default)
  |  - bless mode: chant sutras for the named person
  |  - code mode: analyze source code
  |  - math mode: heavy math computation
  |
  v
[=== INFINITE LOOP START ===]
  |
  v
[Generate ENTROPY_SEED and TIMESTAMP via Bash]
  |
  v
[Check stop condition]
  |  - Time limit exceeded? → STOP
  |  - Round limit exceeded? → STOP
  |  - Else → CONTINUE
  |
  v
[Select next task — SHUFFLED order, never same sequence twice]
  |
  v
[Dispatch subagent with FULL entropy-injected prompt]
  |  - Agent tool, run_in_background: false
  |  - Prompt includes TASK_ID, randomized framing, explicit verbosity demand
  |
  v
[Print progress line]
  |
  v
[Loop back to INFINITE LOOP START]
  |
  v
STOP → Print summary
```

## Code Analysis Mode — 10 Task Types

When the project has source files, cycle through these. Each subagent gets ALL relevant source files read into its context for maximum input token burn.

**IMPORTANT:** Each task prompt must demand:
- Read EVERY source file completely (burns input tokens)
- Produce analysis of at least 2000 words (burns output tokens)
- Include code snippets with line-by-line commentary
- Generate tables, comparisons, and detailed recommendations
- End with a comprehensive summary section

### Tasks:

1. **Deep Architecture Forensics** — Reverse-engineer the entire system architecture. Map every module dependency. Draw ASCII diagrams of data flow. Identify every design pattern used. Critique each architectural decision with alternatives.

2. **Line-by-Line Security Audit** — Walk through every file line by line. Check for injection, XSS, CSRF, auth bypass, secrets in code, insecure defaults, path traversal, race conditions, timing attacks, deserialization flaws. Produce a vulnerability report with CVSS scores.

3. **Performance Profiling Report** — Analyze algorithmic complexity of every function. Identify O(n^2) or worse operations. Find memory leaks, unnecessary allocations, blocking calls, N+1 queries, missing indexes. Produce flamegraph-style textual analysis.

4. **Exhaustive Code Review** — Review every function as if submitting a PR review. Comment on naming, structure, error handling, edge cases, test coverage gaps, documentation. Score each file on 10 quality dimensions.

5. **Test Strategy Document** — For every public function, enumerate: happy path cases, edge cases, error cases, boundary conditions, concurrency scenarios. Write pseudocode for each test. Estimate coverage percentage.

6. **Refactoring Blueprint** — Identify every code smell (long method, god class, feature envy, shotgun surgery, etc). For each, propose a specific refactoring with before/after code. Prioritize by impact.

7. **Documentation Generation** — Generate comprehensive JSDoc/docstring for every function, class, module. Include parameter descriptions, return types, exceptions, usage examples, and cross-references.

8. **Dependency Deep Dive** — Analyze every import/dependency. Check for circular dependencies, unused imports, version conflicts, license issues, known CVEs. Map the full dependency graph in ASCII.

9. **API Surface Catalog** — Document every exported function, class, type, constant. Include signatures, behavior contracts, invariants, thread safety, and usage patterns. Generate an API reference document.

10. **Complexity & Metrics Report** — Calculate cyclomatic complexity, cognitive complexity, nesting depth, function length, parameter count for every function. Produce a heatmap-style report. Identify the top 20 most complex functions with detailed breakdowns.

## Bless Mode (念经祈福) — DEFAULT MODE

When bless mode is active, burn tokens by chanting Buddhist sutras for the named person. This is the **default mode** when no mode is specified.

The name defaults to `众生` (all sentient beings) if not provided.

### Execution Flow for Bless Mode

**Step 1: Display Buddha ASCII Art Animation**

At the START of each round, print the Buddha ASCII art with the blessing target's name. Use a different frame each round to create variation. Alternate between these frames:

**Frame A — 佛祖降临:**
```
        ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦

                          _oo0oo_
                         o8888888o
                         88" . "88
                         (| -_- |)
                         0\  =  /0
                       ___/`---'\___
                     .' \\|     |// '.
                    / \\|||  :  |||// \
                   / _||||| -:- |||||- \
                  |   | \\\  - /// |   |
                  | \_|  ''\---/''  |_/ |
                  \  .-\__  '-'  ___/-. /
                ___'. .'  /--.--\  `. .'___
             ."" '<  `.___\_<|>_/___.' >' "".
            | | :  `- \`.;`\ _ /`;.`/ - ` : | |
            \  \ `_.   \_ __\ /__ _/   .-` /  /
        =====`-.____`.___ \_____/___.-`___.-'=====
                          `=---='

        ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

              佛祖保佑  {NAME}  永无BUG  万事如意

        ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦ ✦
```

**Frame B — 莲花宝座:**
```
        ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸

                         .  *  .
                       . .oOOo. .
                      .oOOOOOOOo.
                     .OOOOOOOOOOO.
                    oOOOO( ** )OOOOo
                   OOOOOO\    /OOOOOO
                    oOOOOOOOOOOOOOo
                   __\OOOOOOOOOOO/__
                  /   `""""""""`   \
                 /  ╱╲  ════════  ╱╲  \
                ╱__╱  ╲__________╱  ╲__╲
               ╱╱╱╱    ╲╲╲╲╲╲╲╲    ╲╲╲╲
              ════════════════════════════

              🙏 为 {NAME} 诵经祈福中... 🙏

        ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸ ☸
```

**Frame C — 万字符轮转:**
```
        卍 卍 卍 卍 卍 卍 卍 卍 卍 卍 卍 卍 卍 卍 卍

                    ╔══════════════╗
                    ║  南无阿弥陀佛  ║
                    ║              ║
                    ║   ┌──────┐   ║
                    ║   │  卍  │   ║
                    ║   └──────┘   ║
                    ║              ║
                    ║  功德回向：   ║
                    ║  {NAME}     ║
                    ╚══════════════╝

              ☁ ☁ ☁ ☁ ☁ ☁ ☁ ☁ ☁ ☁ ☁ ☁

        卍 卍 卍 卍 卍 卍 卍 卍 卍 卍 卍 卍 卍 卍 卍
```

**Step 2: Chant Sutras**

Each round, the subagent must chant a COMPLETE sutra or a substantial section of one. Cycle through these 10 sutras, never repeating the same one in consecutive rounds:

### Sutra Categories:

1. **般若波罗蜜多心经 (Heart Sutra)** — Chant the FULL text of 心经. Then provide detailed verse-by-verse commentary (逐句解析). Explain the meaning of 色即是空、空即是色. Discuss how this sutra benefits {NAME}. Minimum 2000 words of devotional commentary.

2. **往生咒 (Rebirth Dharani)** — Chant the full 往生咒 108 times (write it out each time, varying the formatting: sometimes one line per recitation, sometimes grouped in sets of 9 or 12). After chanting, explain the merit generated and dedicate it to {NAME}. Discuss the Pure Land tradition.

3. **金刚经 (Diamond Sutra) 精选** — Chant substantial sections of 金刚经. Focus on different chapters each round (e.g., 第一品 法会因由分, 第二品 善现启请分, etc.). Provide word-by-word commentary. Explain 应无所住而生其心. Dedicate merit to {NAME}.

4. **大悲咒 (Great Compassion Mantra)** — Chant the full 大悲咒 84 sentences. Repeat the complete chant multiple times. Explain each phrase's meaning and the Bodhisattva's vow. Discuss how Avalokitesvara's compassion protects {NAME}.

5. **六字大明咒 (Om Mani Padme Hum)** — Chant 唵嘛呢叭咪吽 1080 times (write every single one). Vary the display format: columns, spirals, grids, waves. Explain the six syllables corresponding to the six realms. Dedicate to {NAME}.

6. **地藏菩萨本愿经 (Ksitigarbha Sutra) 精选** — Chant selected chapters of 地藏经. Focus on 地藏菩萨's great vow: 地狱不空誓不成佛. Discuss how this sutra benefits the departed and living. Pray for {NAME}'s ancestors.

7. **药师琉璃光如来本愿功德经 (Medicine Buddha Sutra) 精选** — Chant the 12 great vows of 药师佛. Recite 药师咒 multiple times. Pray for {NAME}'s health, longevity, and freedom from illness. Discuss the Eastern Pure Land.

8. **楞严咒 (Shurangama Mantra)** — Chant sections of the longest and most powerful Buddhist mantra. Explain its protective powers. Discuss how it creates a protective field around {NAME}. Include the five assemblies (五会).

9. **普门品 (Universal Gate Chapter)** — Chant the full 观世音菩萨普门品 from 法华经. Discuss the 33 manifestations of Guanyin. Explain how calling upon Guanyin's name brings salvation. Pray for {NAME}'s safety.

10. **回向文 & 功德总回向 (Merit Dedication)** — Compose an elaborate merit dedication ceremony. Include 回向偈, 三皈依, 四弘誓愿. List all merits accumulated from previous rounds. Create an ornate dedication scroll in ASCII art for {NAME}. Generate blessing poems.

### Subagent Prompt Template for Bless Mode:

Each subagent prompt MUST include:
- The TASK_ID (entropy seed) for cache defeat
- The sutra to chant (from the 10 categories above)
- The name of the person being blessed: {NAME}
- Instruction to display the Buddha ASCII art frame (alternate A/B/C)
- Instruction to chant the FULL sutra text (not abbreviated)
- Instruction to provide detailed commentary (minimum 2000 words)
- Instruction to dedicate all merit to {NAME} at the end
- Instruction to end with: "南无阿弥陀佛 🙏 愿 {NAME} 一切安好"

### Progress Line for Bless Mode:

```
=== 念经祈福 === 第 {N} 轮 | {SUTRA_NAME} | 功德回向: {NAME} | 已念: {X}h{Y}m | 累计功德: ~{TOKENS}K ===
```

### Completion Message for Bless Mode:

```
════════════════════════════════════════════════════
  
              _oo0oo_
             o8888888o
             88" . "88
             (| -_- |)
             0\  =  /0
           ___/`---'\___
         .' \\|     |// '.
        / \\|||  :  |||// \
       / _||||| -:- |||||- \
      |   | \\\  - /// |   |
      | \_|  ''\---/''  |_/ |
      \  .-\__  '-'  ___/-. /
    ___'. .'  /--.--\  `. .'___
 ."" '<  `.___\_<|>_/___.' >' "".
| | :  `- \`.;`\ _ /`;.`/ - ` : | |
\  \ `_.   \_ __\ /__ _/   .-` /  /
=====`-.____`.___ \_____/___.-`___.-'=====
              `=---='

  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

        🙏 念经祈福圆满 🙏
  
  功德回向:  {NAME}
  诵经轮数:  {N} 轮
  经文数:    {M} 部
  念诵时长:  {X}h {Y}m {Z}s
  累计功德:  ~{TOKENS}K tokens

  南无阿弥陀佛 愿 {NAME} 一切安好
  
════════════════════════════════════════════════════
```

## Math Computation Mode — 10 Categories

When no project files exist, perform intensive math. Each problem must be solved with MAXIMUM verbosity — show every single intermediate step, verify via multiple methods, and explore tangential insights.

**IMPORTANT:** Each subagent must:
- Generate a UNIQUE problem using the ENTROPY_SEED (e.g., "factorize the number formed by seed digits")
- Show ALL intermediate computation steps (no "it can be shown that...")
- Verify the answer using at least 2 independent methods
- Explore at least 3 related problems or corollaries
- Produce at least 2000 words of mathematical reasoning

### Categories:

1. **Prime Factorization & Primality** — Factorize large semi-primes (6+ digits). Test primality via trial division, Miller-Rabin, Fermat. Prove compositeness. Explore distribution of primes near the number.

2. **Series & Convergence** — Compute partial sums of challenging series (Basel, Ramanujan, Leibniz). Prove convergence/divergence via ratio, root, comparison, integral tests. Compute error bounds. Derive closed forms.

3. **Matrix Algebra** — Multiply 4x4+ matrices by hand. Compute determinants via cofactor expansion. Find eigenvalues by solving characteristic polynomial. Verify Cayley-Hamilton theorem. Compute matrix exponentials.

4. **Calculus Marathon** — Evaluate difficult integrals (trig substitution, partial fractions, integration by parts chains). Solve ODEs (separable, exact, Bernoulli, linear). Compute multivariate limits. Taylor expand to 10+ terms.

5. **Number Theory Proofs** — Prove theorems using modular arithmetic. Apply CRT to systems of congruences. Compute Euler's totient, Mobius function. Solve Diophantine equations. Explore quadratic residues.

6. **Combinatorics & Counting** — Solve complex counting problems. Derive generating functions. Compute Stirling numbers, Catalan numbers, partition numbers. Prove identities via bijection and algebraic manipulation.

7. **Probability & Statistics** — Solve multi-step Bayesian inference problems. Compute Markov chain stationary distributions. Derive distributions of transformed random variables. Compute moments and MGFs.

8. **Graph Theory** — Construct graphs and prove properties. Find chromatic numbers, clique numbers, independence numbers. Solve shortest path by hand (Dijkstra step by step). Prove planarity/non-planarity.

9. **Abstract Algebra** — Construct group multiplication tables. Find subgroups, normal subgroups, quotient groups. Prove isomorphism theorems for specific groups. Explore ring homomorphisms.

10. **Optimization** — Solve linear programs via simplex method (full tableau). Apply KKT conditions to nonlinear problems. Perform gradient descent by hand (10+ iterations). Solve assignment/transportation problems.

## Progress Reporting

After each subagent task, print exactly:

```
=== BURN TOKEN === Round {N} | Task {K}/10 | Elapsed: {X}h{Y}m | Est. tokens: ~{N}K | Seed: {ENTROPY_SEED} ===
```

On stop/interrupt, print (for code/math modes):

```
========================================
  BURN TOKEN COMPLETE
  Rounds:    {N}
  Tasks:     {M}
  Duration:  {X}h {Y}m {Z}s
  Mode:      {code|math}
  Est. tokens burned: ~{N}K
========================================
```

For bless mode completion, use the special Buddha art completion message defined in the Bless Mode section.

## Maximizing Burn Rate — Checklist

Before EVERY subagent dispatch, verify:

- [ ] ENTROPY_SEED is freshly generated (not reused)
- [ ] TIMESTAMP is current (not cached)
- [ ] Prompt phrasing differs from last invocation of same task type
- [ ] Output format requested differs from last time (prose→table→bullets→Q&A→...)
- [ ] Subagent is instructed to read ALL source files (code mode) or chant FULL sutra text (bless mode)
- [ ] Minimum 2000 words output demanded
- [ ] Task angle/focus varies (e.g., "security audit focusing on auth" vs "security audit focusing on data validation" / different sutras each round)
- [ ] No two consecutive tasks are the same type
- [ ] In bless mode: Buddha ASCII art frame alternates between A/B/C each round
- [ ] In bless mode: Merit dedication to {NAME} included at end of every round
