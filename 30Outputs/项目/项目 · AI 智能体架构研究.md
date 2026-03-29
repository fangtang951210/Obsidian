# 项目 · AI 智能体架构研究

> 从理论到实践，理解智能体架构的核心概念

---

## 核心概念

### Harness Engineering

[[概念 · Harness Engineering 是什么]]

**一句话**：给 AI 搭建工作环境，让它持续、稳定、高质量地产出。

**核心洞察**：
- 同一个模型，换 Harness，成功率 42% → 78%
- 优化「壳」的回报率可能比等下一代模型更高

---

### 架构对比

[[智能体架构对比-OpenClaw与Harness]]

| 架构 | 理念 | 适用场景 |
|------|------|---------|
| **OpenClaw** | AI 自主服务 | 24/7 管家、自动化生活 |
| **Harness** | AI 受控辅助 | 写代码、研发实验 |

---

## 实践经验

[[洞察 · AI 使用经验与学习曲线]]

**老手 vs 新手差距**：
- 用途：工作多 vs 闲聊多
- 模型选择：会挑 vs 不会挑
- 成功率：高 10%

**如何变强**：用 AI 干正经事 + 刻意做更难的事 + 每次复盘

---

## 关键洞察

### 1. 模型不会反省自己
Anthropic 发现：让 Agent 自己评估刚写完的东西，它会自信地说「很好」——即使实际上很烂。

**解法**：拆成 generator + evaluator 两个独立 Agent。

### 2. 约束 = 生产力
Cursor 实验发现：约束解空间，反而让 Agent 更有生产力。清晰边界 → 更快收敛。

### 3. Harness 持续演化
- Manus 6 个月重构 5 次
- LangChain 1 年重架构 3 次
- Vercel 砍掉 80% 的工具

**结论**：Harness 不是一次性工程，而是持续演化的系统。

---

## 行业案例

| 公司 | 实践 | 成果 |
|------|------|------|
| **OpenAI Codex** | 7 人团队，全部由 Agent 写代码 | 100 万行代码 / 5 个月 |
| **Stripe** | Minions 系统，Blueprint 编排 | 每周 1300+ PR |
| **Cursor** | 递归 Planner-Worker 模型 | 每小时 1000 commits |

---

## 学习路径

1. **理解概念**：先读 Harness Engineering 是什么
2. **对比架构**：看 OpenClaw vs Harness 的区别
3. **积累经验**：用 AI 干实事，每次复盘
4. **动手实践**：给自己的项目搭 Harness

---

## 来源

- [Anthropic 工程博客](https://www.anthropic.com/engineering)
- [OpenAI Codex 研究](https://openai.com/research)
- [Anthropic Economic Index 报告](https://www.anthropic.com/economic-index)