# burn-token 🔥

**把你的 AI 编程额度烧干。**

[English](./README.md) | [中文](./README.zh-CN.md)

一个用于 Claude Code / Codex / Claude CLI 的极限 Token 燃烧技能。通过无限循环的重度分析和计算，最大化 Token 消耗——即使是 Claude Max 无限计划也能烧完。

---

## 功能介绍

| 功能 | 说明 |
|------|------|
| **自动检测模式** | 有代码？→ 无限代码分析。空项目？→ 硬核数学计算。 |
| **无限循环** | 默认永远运行，直到你手动停止或 Token 耗尽。 |
| **击穿 KV Cache** | 每次请求注入随机种子、时间戳、打乱顺序，防止缓存命中，确保每个 Token 都是实打实消耗。 |
| **灵活配置** | 可设定时间限制、轮次限制，或直接无限燃烧。 |
| **定时执行** | 配合 `/loop` 或 `/schedule` 实现自动化定期燃烧。 |

---

## 安装

### Claude Code

```bash
git clone https://github.com/shengxinjing/burn-token.git
cp -r burn-token/skill ~/.claude/skills/burn-token
```

或者手动安装：

```bash
mkdir -p ~/.claude/skills/burn-token
curl -o ~/.claude/skills/burn-token/SKILL.md \
  https://raw.githubusercontent.com/shengxinjing/burn-token/main/skill/SKILL.md
```

### Codex CLI (OpenAI)

```bash
git clone https://github.com/shengxinjing/burn-token.git
cp -r burn-token/skill ~/.agents/skills/burn-token
```

### 其他 AI 编程工具

将 `SKILL.md` 放入你的工具对应的技能目录即可：

| 工具 | 技能目录 |
|------|---------|
| Claude Code | `~/.claude/skills/burn-token/` |
| Codex CLI | `~/.agents/skills/burn-token/` |
| 其他工具 | 查看对应工具的技能加载路径 |

### 验证安装

```bash
head -5 ~/.claude/skills/burn-token/SKILL.md
```

看到 `---` 开头的 frontmatter 包含 `name: burn-token` 即安装成功。

---

## 使用方法

### 无限燃烧（默认）

```
/burn-token
```

永远运行。自动检测是分析代码还是做数学题。`Ctrl+C` 停止。

### 限时燃烧

```
/burn-token 1h           # 燃烧 1 小时
/burn-token 30m          # 燃烧 30 分钟
/burn-token 4h30m        # 燃烧 4.5 小时
```

### 限轮次燃烧

```
/burn-token 10 rounds    # 燃烧 10 轮（100 个任务）
/burn-token 50 rounds    # 燃烧 50 轮（500 个任务）
```

### 强制模式

```
/burn-token math         # 强制数学模式（即使项目有代码）
/burn-token code         # 强制代码分析模式
/burn-token 2h math      # 组合：2 小时 + 数学模式
```

### 定时燃烧

```bash
# 每小时燃烧 45 分钟
/loop 1h /burn-token 45m

# 工作日白天自动燃烧（周一至周五，9 点到 17 点）
/schedule create --cron "0 9-17 * * 1-5" --prompt "/burn-token 50m"

# 夜间燃烧
/schedule create --cron "0 22 * * *" --prompt "/burn-token 8h"

# 全天候每 2 小时燃烧一次
/schedule create --cron "0 */2 * * *" --prompt "/burn-token 1h45m"
```

---

## 工作原理

### 模式选择

```
项目有源代码？ ──是──> 代码分析模式（10 种任务类型）
      |                 - 架构逆向分析
      否                - 逐行安全审计
      |                 - 性能剖析报告
      v                 - 全面代码审查
项目有文档？ ──是──>    - 测试策略生成
      |                 - 重构蓝图
      否                - 文档生成
      |                 - 依赖深度分析
      v                 - API 接口目录
  数学模式（10 类）      - 复杂度指标报告
  - 素数分解
  - 级数与收敛
  - 矩阵代数
  - 微积分马拉松
  - 数论证明
  - 组合数学
  - 概率统计
  - 图论
  - 抽象代数
  - 最优化
```

### 击穿 KV 缓存

大多数 AI API 会缓存重复的提示词前缀。本技能通过以下方式击穿缓存：

1. **纳秒时间戳** — 每次调用前缀唯一
2. **随机熵种子** — 来自 `/dev/urandom`
3. **打乱任务顺序** — 永不重复相同序列
4. **变化提示词措辞** — 每次用不同的表达
5. **轮换输出格式** — 散文、表格、列表、问答交替

确保每一个 Token 都是全新计算的，而非从缓存中获取。

### 最大化燃烧速率

每个子代理任务同时最大化输入和输出 Token 消耗：

- **输入端：** 读取项目中的所有文件（不仅是相关的）
- **输出端：** 每个任务最少 2000 字，包含详细的逐步分析
- **不走捷径：** 要求逐行分析、多种验证方法
- **全新上下文：** 每个任务作为新的子代理运行，不复用上下文

---

## 进度输出

运行中：

```
=== BURN TOKEN === Round 1 | Task 3/10 | Elapsed: 0h12m | Est. tokens: ~48K | Seed: 2847193650 ===
=== BURN TOKEN === Round 1 | Task 4/10 | Elapsed: 0h16m | Est. tokens: ~64K | Seed: 1039284756 ===
```

完成时：

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

## 常见问题

**Q: 这真的能烧完 Claude Max 计划吗？**
A: 能。Claude Max 计划有速率限制，并非真正无限。本技能通过持续运行重度任务并击穿缓存来不断触达速率限制。

**Q: 为什么要击穿 KV 缓存？**
A: 缓存的响应消耗更少的实际计算 Token。如果 API 识别到重复前缀，会使用缓存的 KV 状态而非重新计算。通过注入随机性，我们强制每次都进行全新计算。

**Q: 这安全吗？**
A: 本技能只读取和分析代码——从不修改文件、推送代码或发起外部请求。设计上就是只读的。

---

## 开源协议

MIT
