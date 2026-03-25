# burn-token 🔥

**Burn your AI coding tokens to the ground.**

An aggressive token-burning skill for Claude Code / Codex / Claude CLI. It runs an infinite loop of heavyweight analysis and computation, designed to maximize token consumption — even on unlimited plans.

---

**burn-token 🔥**

**把你的 AI 编程额度烧干。**

一个用于 Claude Code / Codex / Claude CLI 的极限 Token 燃烧技能。通过无限循环的重度分析和计算，最大化 Token 消耗——即使是无限额度计划也能烧完。

---

## What it does / 功能介绍

| Feature | Description |
|---------|-------------|
| **Auto-detect mode** | Has code? → endless code analysis. Empty project? → hardcore math problems. |
| **Infinite loop** | Runs forever by default. Only stops when you say so (or tokens run out). |
| **KV Cache defeat** | Injects entropy (random seeds, timestamps, shuffled order) into every prompt to prevent cache hits and maximize actual token consumption. |
| **Configurable** | Set time limits, round limits, or let it rip. |
| **Schedulable** | Combine with `/loop` or `/schedule` for automated recurring burns. |

| 功能 | 说明 |
|------|------|
| **自动检测模式** | 有代码？→ 无限代码分析。空项目？→ 硬核数学计算。 |
| **无限循环** | 默认永远运行，直到你手动停止或 Token 耗尽。 |
| **击穿 KV Cache** | 每次请求注入随机种子、时间戳、打乱顺序，防止缓存命中，确保每个 Token 都是实打实消耗。 |
| **灵活配置** | 可设定时间限制、轮次限制，或直接无限燃烧。 |
| **定时执行** | 配合 `/loop` 或 `/schedule` 实现自动化定期燃烧。 |

---

## Installation / 安装

### Claude Code

```bash
# Clone the repo / 克隆仓库
git clone https://github.com/shengxinjing/burn-token.git

# Copy skill to Claude Code skills directory
# 复制技能到 Claude Code 技能目录
cp -r burn-token/skill ~/.claude/skills/burn-token
```

Or manually:

```bash
mkdir -p ~/.claude/skills/burn-token
# Copy SKILL.md into the directory
# 将 SKILL.md 复制到该目录
cp burn-token/skill/SKILL.md ~/.claude/skills/burn-token/SKILL.md
```

Verify installation / 验证安装:

```bash
cat ~/.claude/skills/burn-token/SKILL.md | head -5
```

You should see the `---` frontmatter with `name: burn-token`.

### Codex CLI (OpenAI)

```bash
mkdir -p ~/.agents/skills/burn-token
cp burn-token/skill/SKILL.md ~/.agents/skills/burn-token/SKILL.md
```

### Other AI Coding Agents / 其他 AI 编程工具

Any agent that supports skill files can use burn-token. Just place `SKILL.md` in the agent's skill directory:

任何支持技能文件的 AI 编程工具都可以使用 burn-token，只需将 `SKILL.md` 放入对应工具的技能目录：

| Agent | Skill Directory |
|-------|----------------|
| Claude Code | `~/.claude/skills/burn-token/` |
| Codex CLI | `~/.agents/skills/burn-token/` |
| Custom agents | Check your agent's skill loading path |

---

## Usage / 使用方法

### Basic — Infinite Burn / 基础用法 — 无限燃烧

```
/burn-token
```

Runs forever. Auto-detects whether to analyze code or do math. Ctrl+C to stop.

永远运行。自动检测是分析代码还是做数学题。Ctrl+C 停止。

### Time-Limited / 限时燃烧

```
/burn-token 1h          # burn for 1 hour / 燃烧1小时
/burn-token 30m         # burn for 30 minutes / 燃烧30分钟
/burn-token 4h30m       # burn for 4.5 hours / 燃烧4.5小时
```

### Round-Limited / 限轮次燃烧

```
/burn-token 10 rounds   # burn for 10 rounds (100 tasks) / 燃烧10轮（100个任务）
/burn-token 50 rounds   # burn for 50 rounds (500 tasks) / 燃烧50轮（500个任务）
```

### Force Mode / 强制模式

```
/burn-token math        # force math mode even if project has code / 强制数学模式
/burn-token code        # force code analysis mode / 强制代码分析模式
/burn-token 2h math     # 2-hour math burn / 2小时数学燃烧
```

### Scheduled Burns / 定时燃烧

```bash
# Burn 45 minutes every hour / 每小时燃烧45分钟
/loop 1h /burn-token 45m

# Burn during business hours (Mon-Fri, 9am-5pm) / 工作日白天自动燃烧
/schedule create --cron "0 9-17 * * 1-5" --prompt "/burn-token 50m"

# Overnight burn / 夜间燃烧
/schedule create --cron "0 22 * * *" --prompt "/burn-token 8h"

# Burn every 2 hours around the clock / 全天候每2小时燃烧一次
/schedule create --cron "0 */2 * * *" --prompt "/burn-token 1h45m"
```

---

## How It Works / 工作原理

### Mode Selection / 模式选择

```
Project has source files? ──yes──> Code Analysis Mode (10 task types)
         |                         - Architecture forensics
         no                        - Security audit (line-by-line)
         |                         - Performance profiling
         v                         - Exhaustive code review
Project has documents? ──yes──>    - Test strategy generation
         |                         - Refactoring blueprint
         no                        - Documentation generation
         |                         - Dependency deep dive
         v                         - API surface catalog
    Math Mode (10 categories)      - Complexity metrics report
    - Prime factorization
    - Series & convergence
    - Matrix algebra
    - Calculus marathon
    - Number theory proofs
    - Combinatorics
    - Probability & statistics
    - Graph theory
    - Abstract algebra
    - Optimization
```

### KV Cache Defeat / 击穿 KV 缓存

Most AI APIs cache repeated prompt prefixes. This skill defeats caching by:

大多数 AI API 会缓存重复的提示词前缀。本技能通过以下方式击穿缓存：

1. **Nanosecond timestamps** — unique prefix every call / 纳秒时间戳 — 每次调用前缀唯一
2. **Random entropy seed** — from `/dev/urandom` / 随机熵种子 — 来自 `/dev/urandom`
3. **Shuffled task order** — never the same sequence / 打乱任务顺序 — 永不重复相同序列
4. **Varied prompt phrasing** — different words each time / 变化提示词措辞 — 每次用不同的表达
5. **Rotating output formats** — prose, tables, bullets, Q&A / 轮换输出格式 — 散文、表格、列表、问答

This ensures every single token is freshly computed, not served from cache.

确保每一个 Token 都是全新计算的，而非从缓存中获取。

### Burn Rate Maximization / 最大化燃烧速率

Each subagent task is designed to burn tokens on both input and output:

每个子代理任务同时最大化输入和输出 Token 消耗：

- **Input:** Reads ALL project files into context (not just relevant ones) / 读取所有项目文件（不仅是相关的）
- **Output:** Minimum 2000 words per task, with detailed step-by-step analysis / 每个任务最少 2000 字，包含详细的逐步分析
- **No shortcuts:** Demands line-by-line analysis, multiple verification methods / 不走捷径：要求逐行分析、多种验证方法
- **Fresh context:** Each task runs as a new subagent (no context reuse) / 全新上下文：每个任务作为新的子代理运行

---

## Progress Output / 进度输出

During burn:

```
=== BURN TOKEN === Round 1 | Task 3/10 | Elapsed: 0h12m | Est. tokens: ~48K | Seed: 2847193650 ===
=== BURN TOKEN === Round 1 | Task 4/10 | Elapsed: 0h16m | Est. tokens: ~64K | Seed: 1039284756 ===
...
```

On completion:

```
========================================
  BURN TOKEN COMPLETE
  Rounds:    7
  Tasks:     70
  Duration:  1h 58m 34s
  Mode:      code
  Est. tokens burned: ~1120K
========================================
```

---

## FAQ

**Q: Will this actually burn through a Claude Max plan?**

A: Yes. Claude Max plans have rate limits, not true unlimited tokens. This skill is designed to hit those rate limits continuously by running heavyweight tasks with cache-defeating entropy.

**Q: 这真的能烧完 Claude Max 计划吗？**

A: 能。Claude Max 计划有速率限制，并非真正无限。本技能通过持续运行重度任务并击穿缓存来不断触达速率限制。

**Q: Why defeat KV cache?**

A: Cached responses consume fewer actual compute tokens. If the API recognizes a repeated prefix, it serves a cached KV state instead of recomputing. By injecting entropy, we force fresh computation every time.

**Q: 为什么要击穿 KV 缓存？**

A: 缓存的响应消耗更少的实际计算 Token。如果 API 识别到重复前缀，会使用缓存的 KV 状态而非重新计算。通过注入随机性，我们强制每次都进行全新计算。

**Q: Is this safe?**

A: The skill only reads and analyzes code — it never modifies files, pushes code, or makes external requests. It's read-only by design.

**Q: 这安全吗？**

A: 本技能只读取和分析代码——从不修改文件、推送代码或发起外部请求。设计上就是只读的。

---

## License

MIT
