---
title: 工具 · 让 Claude 控制浏览器
date: 2026-03-27
---

# 工具 · 让 Claude 控制浏览器

> [!abstract] 一句话概括
> 给 Claude Code 装上完整的联网和浏览器自动化能力，让它能像真人一样操作浏览器。
>
> **GitHub**: https://github.com/eze-is/web-access

---

## 为什么需要它？

> [!warning] Claude Code 原生工具的局限
> - **WebSearch**：只能搜索
> - **WebFetch**：只能获取静态页面
> - **痛点**：无法处理需要登录、需要交互的网站

---

## 核心架构

### 五层联网通道

| 工具 | 用途 | 强度 |
|:----:|:----:|:----:|
| WebSearch | 搜索信息 | ⭐ |
| WebFetch | 获取网页 | ⭐⭐ |
| curl | HTTP 请求 | ⭐⭐ |
| Jina | 内容提取 | ⭐⭐⭐ |
| **CDP** | 浏览器操作 | ⭐⭐⭐⭐⭐ |

web-access 会根据场景**自动选择**最合适的工具。

---

## CDP 浏览器操作

> [!tip] 最硬核的能力
> 直连你日常使用的 Chrome，**天然携带登录态**。

### 能做什么

- ✅ 操作已登录的网站（GitHub、小红书、Gmail...）
- ✅ 处理动态页面（JS 渲染、SPA）
- ✅ 交互操作（点击、输入、滚动、截图）
- ✅ 视频截帧分析
- ✅ 文件上传

### 配置步骤

1. **环境要求**：Node.js 22+
2. **开启 Chrome 远程调试**：
   - 地址栏输入 `chrome://inspect/#remote-debugging`
   - 勾选 "Allow remote debugging for this browser instance"
3. **检查环境**：
   ```bash
   bash ~/.claude/skills/web-access/scripts/check-deps.sh
   ```

---

## API 速查表

**代理地址**：`http://localhost:3456`

| 操作 | 命令 |
|------|------|
| 列出 tab | `curl -s http://localhost:3456/targets` |
| 新建 tab | `curl -s "http://localhost:3456/new?url=URL"` |
| 执行 JS | `curl -s -X POST "http://localhost:3456/eval?target=ID" -d 'code'` |
| 点击 | `curl -s -X POST "http://localhost:3456/click?target=ID" -d 'selector'` |
| 真实点击 | `curl -s -X POST "http://localhost:3456/clickAt?target=ID" -d 'selector'` |
| 上传文件 | `curl -s -X POST "http://localhost:3456/setFiles?target=ID" -d '{"selector":"...","files":["..."]}'` |
| 截图 | `curl -s "http://localhost:3456/screenshot?target=ID&file=path"` |
| 滚动 | `curl -s "http://localhost:3456/scroll?target=ID&direction=bottom"` |
| 关闭 tab | `curl -s "http://localhost:3456/close?target=ID"` |

---

## 三种点击方式

| 方式 | 原理 | 适用场景 |
|:----:|------|---------|
| `/click` | JS el.click() | 大部分常规场景 |
| `/clickAt` | CDP 真实鼠标事件 | 对点击校验严格的网站 |
| `/setFiles` | 直接设置文件路径 | 文件上传 |

---

## 实战踩坑

> [!bug] 中文内容乱码
> 通过 `/eval` 传输中文时，bash/curl 编码会破坏字符。
>
> **解决方案**：用 Base64 编码
> ```bash
> # 生成 Base64
> node -e "console.log(Buffer.from('中文', 'utf8').toString('base64'))"
>
> # 浏览器中解码
> const content = new TextDecoder('utf-8').decode(
>   new Uint8Array(atob(base64).split('').map(c => c.charCodeAt(0)))
> );
> document.execCommand('insertText', false, content);
> ```

> [!bug] 登录态丢失
> 直接打开子站 URL 可能丢失登录态。
>
> **解决方案**：从已登录的主站页面点击链接进入，而不是直接导航。

---

## 设计哲学

> [!quote] Skill = 哲学 + 技术事实，不是操作手册
>
> 不要替 AI 推理，而是讲清楚 tradeoff 让 AI 自己选择。
