# git worktree：AI 编程时代的并行开发神器 #进行中

## 核心概念

**一个 Git 仓库，多个真实存在的工作目录**

- 只有一个 .git
- 多个工作目录
- 每个目录独立的文件状态、build / node_modules
- 可以 checkout 到不同分支或 commit

## 为什么 AI 时代突然火了？

**我们第一次，真的需要并行写代码了**

今天一个开发者的工作方式正在变成：
- 你在修 bug
- 一个 AI Agent 在改架构
- 另一个 Agent 在补测试

如果大家都挤在同一个 working tree 里，状态必然互相污染。

而 worktree 刚好天然契合：
- 一个 worktree = 一个 Agent 的工作现场
- 不抢、不等、不打断
- 并行但不混乱

## Claude Code 之父 Boris 的建议

同时开启 3–5 个 git worktree，让每个 worktree 都运行独立的 Claude 会话并行工作。

> 不要让 AI 和你排队写代码。并行开发，才是 Claude Code 的正确用法。

## 常用命令

### 创建新的 worktree
```bash
git worktree add -b feature/ai-agent ./feature-ai
```

效果：
- 原目录：你继续干你的活
- 新目录 ./feature-ai：已经 checkout 到 feature/ai-agent

### 查看所有 worktree
```bash
git worktree list
```

### 清理 worktree
```bash
git worktree remove ../feature-ai
```

不会删分支，不会影响历史，只是清理现场。

## worktree vs checkout 切换对比

| 场景 | 传统 checkout | worktree |
|------|--------------|----------|
| 临时 hotfix | stash → checkout → 修 → checkout → unstash | 直接在新目录修 |
| 多任务并行 | 频繁切换，心智负担大 | 多窗口同时开着 |
| AI 协作 | AI 和你排队 | AI 独立环境干活 |

## 原理

- Git 版本数据（.git/objects）是共享的
- 工作区文件是复制出来的一份
- **注意**：Git 不允许「同一个分支多个 worktree」，会导致 HEAD / index 语义混乱

## 关键洞察

> Git 在十年前，就已经为今天准备好了答案。只是直到 AI 编程时代，我们才真正走到该用它的时候。

---

来源：https://mp.weixin.qq.com/s/E5zmFcT3P93A9skOlbjMGA