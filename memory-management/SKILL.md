# AI Agent 记忆管理

> AI 每次醒来都是"失忆"的。文件就是你的记忆。

---

## 核心原则

```
短期记忆 = 当前对话上下文（会消失）
长期记忆 = 文件系统（持久化）
```

## 记忆层次

```
workspace/
├── MEMORY.md              # 🧠 长期记忆（精华）
├── memory/
│   ├── 2026-03-27.md      # 📅 每日记录（原始）
│   ├── 2026-03-26.md
│   └── ...
├── AGENTS.md              # 📋 行为规则
└── TOOLS.md               # 🔧 工具配置
```

### 每日记录 vs 长期记忆

| 每日记录 | 长期记忆 |
|----------|----------|
| 原始日志 | 精华提炼 |
| 流水账 | 重要决策 |
| 会过期 | 持续更新 |
| 详细 | 简洁 |

---

## 写入规则

### 什么该记？

✅ **记：**
- 重要决策和原因
- 用户偏好
- 项目状态
- 学到的教训
- 关键日期和 deadline
- 常用的 ID、链接、配置

❌ **不记：**
- 敏感信息（密码、token）
- 可以随时查到的信息
- 临时性的内容

### 怎么记？

```markdown
## 2026-03-27

### 项目进展
- LLM Fundamentals Day 4 完成
- 新增位置编码附录

### 决策
- 采用树状结构组织复杂任务（来自 Vibe Physics）

### 待办
- [ ] Day 5: GPT vs BERT
```

---

## 读取规则

### 每次 session 开始：

```
1. 读 AGENTS.md（行为规则）
2. 读 MEMORY.md（长期记忆）
3. 读 memory/今天.md + memory/昨天.md（近期上下文）
```

### 回答历史问题时：

```
1. 先搜索 memory/ 目录
2. 找到相关文件再读取
3. 找不到就说"让我查一下..."
```

---

## 维护规则

### 每日：
- 记录当天重要事项到 `memory/YYYY-MM-DD.md`

### 每周：
- 回顾本周日记
- 提炼重要内容到 `MEMORY.md`
- 删除过时信息

### 原则：
- **MEMORY.md 不超过 500 行**（太长就该拆分或精简）
- **日记保留 30 天**（更早的可以归档或删除）

---

## 常见模式

### 项目追踪

```markdown
## 进行中的项目

### LLM Fundamentals
- 状态: Day 4/60
- Repo: github.com/xxx/llm-fundamentals
- 下一步: Day 5 GPT vs BERT
```

### 用户偏好

```markdown
## Elar 的偏好

- 时区: Asia/Singapore
- 沟通风格: 简洁直接
- 技术栈: Python, TypeScript
```

### 学到的教训

```markdown
## 教训

### 2026-03-27: 树状结构
- 来源: Vibe Physics
- 学到: 复杂任务拆成小模块，每个模块独立验证
- 应用: 已更新到 AGENTS.md
```

---

## 一句话总结

> **"Mental notes"不存在。想记住就写到文件。文件是 AI 的唯一长期记忆。**

---

## 参考

- OpenClaw workspace 最佳实践
- [Building effective agents](https://www.anthropic.com/research/building-effective-agents)
