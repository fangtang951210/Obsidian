# Claude Code Hooks 完全指南

Hooks 是 Claude Code 的「自动触发器」——设定规则后，让重复操作自动化运行。

## 核心原理

**一句话**：Hook 命令的 stdout 输出会自动注入到 Claude 的上下文中。

```
事件触发 → 执行命令 → 输出注入上下文 → Claude 自动看到
```

## Hook 类型一览

| Hook | 触发时机 | 典型用途 |
|------|---------|---------|
| **SessionStart** | 会话开始 | 加载项目状态、待办事项 |
| **SessionEnd** | 会话结束 | 清理、日志记录 |
| **PreToolUse** | 工具执行前 | 拦截危险命令、审批操作 |
| **PostToolUse** | 工具执行后 | 自动格式化、自动同步 |
| **PermissionRequest** | 权限请求时 | 自动批准安全操作 |
| **UserPromptSubmit** | 用户发消息时 | 注入上下文、验证提示词 |
| **Stop** | Claude 说完成 | 检查是否真完成 |
| **SubagentStop** | 子代理完成 | 检查子代理输出质量 |
| **PreCompact** | 压缩上下文前 | 备份对话记录 |
| **Notification** | 发送通知时 | 自定义通知处理 |

---

## 配置位置

| 文件 | 作用范围 |
|------|---------|
| `.claude/settings.json` | 当前项目 |
| `~/.claude/settings.json` | 全局所有项目 |

**项目级优先**：项目配置会覆盖全局配置。

---

## 实用场景与示例

### 1️⃣ 会话开始自动加载项目状态

**场景**：每次打开 Claude Code，不用手动说明项目情况

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "startup",
        "hooks": [
          {
            "type": "command",
            "command": "echo '📊 项目状态' && git status --short && echo '' && cat TODO.md 2>/dev/null"
          }
        ]
      }
    ]
  }
}
```

**效果**：Claude 启动时自动看到 git 状态和待办事项。

---

### 2️⃣ 文件保存后自动格式化

**场景**：Claude 写完代码，自动跑 Prettier/ESLint

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "prettier --write \"$CLAUDE_PROJECT_DIR\"/**/*.ts"
          }
        ]
      }
    ]
  }
}
```

**效果**：每次 Write/Edit 后自动格式化代码。

---

### 3️⃣ 自动同步到 Git

**场景**：笔记或代码自动备份到 GitHub

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "cd \"$CLAUDE_PROJECT_DIR\" && git add -A && git diff --cached --quiet || (git commit -m '📦 自动同步' && git push)",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

---

### 4️⃣ 拦截危险命令

**场景**：防止 Claude 执行 `rm -rf` 等危险操作

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "echo '$CLAUDE_TOOL_INPUT' | grep -q 'rm -rf' && echo '{\"permissionDecision\": \"deny\", \"permissionDecisionReason\": \"禁止执行 rm -rf 命令\"}' && exit 2 || exit 0"
          }
        ]
      }
    ]
  }
}
```

---

### 5️⃣ 自动批准测试命令

**场景**：`npm test` 等安全命令不用每次点确认

```json
{
  "hooks": {
    "PermissionRequest": [
      {
        "matcher": "Bash(npm test*)",
        "hooks": [
          {
            "type": "command",
            "command": "echo '{\"permissionDecision\": \"allow\"}'"
          }
        ]
      }
    ]
  }
}
```

---

### 6️⃣ 用户发消息时注入上下文

**场景**：每次对话自动带上 Sprint 目标、最近的错误日志

```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "echo '{\"hookSpecificOutput\": {\"additionalContext\": \"当前 Sprint: 完成用户认证模块\"}}'"
          }
        ]
      }
    ]
  }
}
```

---

### 7️⃣ 完成后自动检查

**场景**：Claude 说完成了，自动验证测试是否通过

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "npm test 2>&1 | tail -5"
          }
        ]
      }
    ]
  }
}
```

---

## Matcher 语法

| 写法 | 匹配规则 |
|------|---------|
| `"Write"` | 精确匹配 Write 工具 |
| `"Write\|Edit"` | 匹配 Write 或 Edit |
| `"Bash"` | 匹配所有 Bash 命令 |
| `"Bash(npm *)"` | 匹配 npm 开头的命令 |
| `"*"` 或 `""` | 匹配所有 |

---

## 环境变量

| 变量 | 说明 |
|------|------|
| `$CLAUDE_PROJECT_DIR` | 当前项目根目录 |
| `$CLAUDE_TOOL_INPUT` | 工具输入参数（PreToolUse/PostToolUse） |

---

## 输出控制

### 简单模式（退出码）

- **exit 0**：成功，stdout 注入上下文
- **exit 2**：阻断操作，stderr 反馈给 Claude
- **其他**：非阻断错误

### JSON 模式（高级控制）

```json
{
  "permissionDecision": "allow|deny|ask",
  "permissionDecisionReason": "原因说明",
  "hookSpecificOutput": {
    "additionalContext": "注入的上下文"
  }
}
```

---

## 调试技巧

1. **查看已注册 hook**：运行 `/hooks` 命令
2. **详细日志**：`claude --debug` 查看执行过程
3. **先手动测试**：命令先在终端跑一遍再写入配置
4. **注意转义**：JSON 内双引号要转义 `\"`

---

## 注意事项

- Hook 配置在会话启动时加载快照，修改后需**新会话**才生效
- 超时默认 60 秒，可 `timeout` 参数调整
- 多个 hook 并行执行
- 安全风险：Hook 执行任意 shell 命令，配置前确认命令安全

---

## 官方文档

https://docs.anthropic.com/en/docs/claude-code/hooks

---

> 写于 2026-03-29
