# 增量验证：边做边查，不要最后才发现错

> 做完 100 步再检查，错误可能在第 3 步。
> 每做一步就验证，问题立刻发现。

---

## 核心原则

```
做一步 → 验证一步 → 通过 → 下一步
                  ↓
               失败 → 修复 → 重新验证
```

## 为什么不能最后才验证？

### 问题：错误累积

```
第 1 步: 正确
第 2 步: 正确
第 3 步: 小错误 ← 没发现
第 4 步: 基于第 3 步，继续
...
第 50 步: 结果完全错误

现在要找错误在哪？
要从第 1 步开始重新检查...
```

### 解决：每步验证

```
第 1 步 → ✅ 验证通过
第 2 步 → ✅ 验证通过
第 3 步 → ❌ 验证失败 ← 立刻发现！
       → 修复
       → ✅ 验证通过
第 4 步 → 继续...
```

---

## 验证方法

### 1. 单元测试

```python
# 每个函数都有对应测试
def calculate_attention(Q, K, V):
    ...

def test_calculate_attention():
    Q = torch.randn(2, 4, 8)
    K = torch.randn(2, 4, 8)
    V = torch.randn(2, 4, 8)
    result = calculate_attention(Q, K, V)
    assert result.shape == (2, 4, 8)
    assert not torch.isnan(result).any()
```

### 2. 已知值检验

```python
# 代入简单值，手算结果
def test_known_values():
    # 当 Q=K=V=单位矩阵时，输出应该是...
    Q = K = V = torch.eye(4)
    result = calculate_attention(Q, K, V)
    expected = ...  # 手算的结果
    assert torch.allclose(result, expected)
```

### 3. 不变量检查

```python
# 某些性质应该始终成立
def test_invariants():
    result = calculate_attention(Q, K, V)
    
    # 注意力权重应该加起来等于 1
    weights = get_attention_weights(Q, K)
    assert torch.allclose(weights.sum(dim=-1), torch.ones(...))
    
    # 输出维度应该和 V 一致
    assert result.shape == V.shape
```

### 4. 边界测试

```python
def test_edge_cases():
    # 空输入
    # 单个元素
    # 非常大的值
    # 非常小的值
    ...
```

---

## 实战流程

### 复杂计算任务

```
任务: 实现 Transformer 的 Attention

步骤 1: 实现 Q = XW_q
  → 测试: 输出形状正确？梯度能传播？
  → ✅ 通过

步骤 2: 实现 K = XW_k
  → 测试: 同上
  → ✅ 通过

步骤 3: 实现 scores = QK^T
  → 测试: 形状是 (seq, seq)？值在合理范围？
  → ✅ 通过

步骤 4: 实现 scaled_scores = scores / sqrt(d_k)
  → 测试: 值的方差是否接近 1？
  → ❌ 失败！忘了开根号
  → 修复
  → ✅ 通过

步骤 5: 实现 weights = softmax(scaled_scores)
  → 测试: 每行加起来等于 1？
  → ✅ 通过

...
```

### 文章写作任务

```
任务: 写一篇技术文章

步骤 1: 写大纲
  → 检查: 逻辑顺序对吗？覆盖了要点吗？
  → ✅ 通过

步骤 2: 写第一章
  → 检查: 和大纲一致吗？术语准确吗？
  → ✅ 通过

步骤 3: 写第二章
  → 检查: 和第一章衔接吗？有没有重复？
  → ❌ 有些内容和第一章重复了
  → 修复
  → ✅ 通过

...
```

---

## Checkpoint 机制

### 为什么需要 Checkpoint？

```
做了 20 步，第 21 步出错了。
如果没有保存前 20 步的结果，可能要全部重来。
```

### 怎么做？

```
每完成一个重要步骤：
1. 保存当前状态到文件
2. 记录已完成的步骤
3. 出错时可以从最近的 checkpoint 恢复
```

### 示例

```python
# checkpoint.json
{
  "completed_steps": ["step1", "step2", "step3"],
  "current_step": "step4",
  "intermediate_results": {
    "step1_output": "path/to/step1_result.pkl",
    "step2_output": "path/to/step2_result.pkl",
    "step3_output": "path/to/step3_result.pkl"
  }
}
```

---

## 验证 Checklist

每一步验证时问：

```markdown
- [ ] 输出格式/形状正确？
- [ ] 数值在合理范围？
- [ ] 代入简单值结果对？
- [ ] 边界情况处理了？
- [ ] 和前一步的输出对接正确？
- [ ] 没有硬编码的魔法数字？
```

---

## 一句话总结

> **每做一步就验证一步。小步快跑，错误立刻发现。不要等到最后才发现第 3 步就错了。**

---

## 参考

- 测试驱动开发 (TDD)
- [Vibe Physics](https://www.anthropic.com/research/vibe-physics) — 逐步验证每个计算
