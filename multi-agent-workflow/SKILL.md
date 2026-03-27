# 多 Agent 协作模式

> 一个 AI 做所有事不如专业分工。
> 就像公司有不同岗位，AI 也可以角色分工。

---

## 核心原则

```
任务 → 拆分成角色 → 每个角色专注一件事 → 流水线或协作
```

## 常见模式

### 模式 1：流水线

```
输入 → [Agent A] → [Agent B] → [Agent C] → 输出

例：翻译工作流
原文 → [翻译 Agent] → 初稿 → [审校 Agent] → 终稿
```

### 模式 2：审核循环

```
[生成 Agent] ←→ [审核 Agent]
     ↓              ↓
   输出 ←── 反馈 ←──┘

例：代码工作流
[写代码] → [代码审查] → 反馈 → [修改] → [再审查] → 通过
```

### 模式 3：专家委员会

```
        ┌─ [专家 A] ─┐
任务 ──┼─ [专家 B] ──┼─→ 综合 → 输出
        └─ [专家 C] ─┘

例：决策
[技术专家] + [产品专家] + [用户专家] → 综合建议
```

---

## 实战案例：翻译工作流

### 角色定义

```
Alice (翻译):
- 专注：准确翻译
- 输出：译文初稿

Bob (审校):
- 专注：质量把控
- 输出：修改建议 + 评分

Carol (终审):
- 专注：最终决策
- 输出：发布/打回
```

### 流程

```
1. Alice 翻译
2. Bob 审校，给出评分和建议
3. 评分 ≥ 85 → Carol 发布
   评分 < 85 → Alice 根据建议修改 → 回到步骤 2
```

### 质量门禁

```markdown
## Bob 的评分标准

| 维度 | 权重 | 说明 |
|------|------|------|
| 准确性 | 40% | 意思是否正确 |
| 流畅性 | 30% | 读起来是否自然 |
| 术语 | 20% | 专业词汇是否一致 |
| 格式 | 10% | 排版是否正确 |

通过标准: 总分 ≥ 85
```

---

## 实战案例：Tech News 工作流

```
[信息采集] ──┐
[信息采集] ──┼─→ [Alex 撰稿] → [Bob 审核] → [Alice 翻译] → [发布]
[信息采集] ──┘
```

### 角色

```
信息采集:
- 各来源并行抓取
- 输出: 原始素材

Alex (撰稿):
- 筛选、组织、深度分析
- 输出: 英文日报

Bob (审核):
- 事实核查、格式检查
- 输出: 通过/修改建议

Alice (翻译):
- 英译中
- 输出: 中文日报
```

---

## 设计原则

### 1. 单一职责

```
❌ 坏: 一个 Agent 又翻译又审校又排版
✅ 好: 翻译 Agent + 审校 Agent + 排版 Agent
```

### 2. 明确接口

```
每个 Agent 的输入输出要清晰定义：

翻译 Agent:
  输入: { source_text, source_lang, target_lang }
  输出: { translated_text, confidence, notes }
```

### 3. 可重试

```
如果某个 Agent 失败，应该能单独重跑，不影响其他 Agent
```

### 4. 质量门禁

```
每个阶段有明确的通过标准，不达标就打回
```

---

## 实现方式

### 方式 1：Prompt 角色扮演

```
一个 AI，不同的 system prompt：

翻译时:
  "你是专业翻译 Alice，只负责翻译，不要审校"

审校时:
  "你是严格的审校 Bob，找出所有问题"
```

### 方式 2：独立 Agent

```
真正的多个 AI 实例，通过文件或 API 传递结果：

agent_alice.py → output.json → agent_bob.py → review.json
```

### 方式 3：Sub-agent 架构

```
主 Agent 调度，子 Agent 执行：

Main Agent:
  1. spawn(TranslateAgent, text)
  2. result = await translate_result
  3. spawn(ReviewAgent, result)
  4. ...
```

---

## 一句话总结

> **多 Agent = 分工协作。每个 Agent 专注一件事，通过清晰的接口传递结果，用质量门禁保证输出。**

---

## 参考

- [Building effective agents](https://www.anthropic.com/research/building-effective-agents)
- OpenClaw sub-agent 架构
