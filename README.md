# Public Skills

可复用的 AI Agent 技能和指南，适用于 OpenClaw、Claude Code、Codex 等。

## Skills

| 技能 | 描述 | 来源 |
|------|------|------|
| [tree-structure](./tree-structure/) | 用树状结构处理复杂任务 | Vibe Physics |
| [cross-validation](./cross-validation/) | 用多个 AI 互相检查 | Vibe Physics |
| [memory-management](./memory-management/) | AI Agent 的记忆组织 | OpenClaw 实践 |
| [multi-agent-workflow](./multi-agent-workflow/) | 多 Agent 协作模式 | Anthropic |
| [incremental-verification](./incremental-verification/) | 边做边查，增量验证 | TDD + Vibe Physics |

## 使用方式

1. **直接复制** — 把相关 SKILL.md 放入你的项目
2. **作为参考** — 提取原则写入你的 AGENTS.md
3. **组合使用** — 这些 skill 可以叠加

## 核心原则总结

```
🌳 树状结构     — 大任务拆小模块
🔄 交叉验证     — 不要让同一个 AI 既做题又判卷
🧠 记忆管理     — 文件是唯一的长期记忆
👥 多 Agent     — 分工协作，各司其职
✅ 增量验证     — 每做一步就验证一步
```

## 贡献

欢迎 PR！好的 skill 应该：
- 解决一个具体问题
- 有清晰的原则和示例
- 可独立复用

## 灵感来源

- [Vibe Physics](https://www.anthropic.com/research/vibe-physics)
- [Long-running Claude](https://www.anthropic.com/research/long-running-claude)
- [Building effective agents](https://www.anthropic.com/research/building-effective-agents)

---

*Maintained by Elar & Arae*
