# 项目 · Claude Code 工具链

> Claude Code 生态的核心工具合集

---

## 核心工具

| 工具 | 用途 | 强度 |
|------|------|:----:|
| [[工具 · 让 Claude 控制浏览器]] | 浏览器自动化、联网操作 | ⭐⭐⭐⭐⭐ |
| [[工具 · Claude Code Hooks 完全指南]] | 自动化触发器、流程编排 | ⭐⭐⭐⭐ |
| [[工具 · claude-context-mode 上下文优化插件]] | 上下文管理、节省 token | ⭐⭐⭐ |

---

## 工具协作关系

```
用户指令
    ↓
Hooks（自动触发）→ context-mode（优化上下文）
    ↓
web-access（执行联网操作）
    ↓
输出结果
```

---

## 适用场景

### 日常开发
- 自动格式化代码（Hooks）
- 自动同步到 Git（Hooks）
- 查阅在线文档（web-access）

### 复杂任务
- 需要登录的网站操作（web-access CDP）
- 大量日志分析（context-mode）
- 多步骤自动化（Hooks + web-access）

### 调试排查
- 日志检索（context-mode）
- 网页抓取分析（web-access）
- 自动运行测试（Hooks）

---

## 快速配置

1. **先配 Hooks**：设置自动化规则
2. **再加 web-access**：获得联网能力
3. **按需装 context-mode**：遇到上下文瓶颈时

---

## 相关资源

- [web-access GitHub](https://github.com/eze-is/web-access)
- [context-mode GitHub](https://github.com/mksglu/claude-context-mode)
- [Claude Code 官方文档](https://docs.anthropic.com/claude-code)