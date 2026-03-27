# web-access Skill 使用指南

> 给 Claude Code 装上完整的联网和浏览器自动化能力
> 
> GitHub: https://github.com/eze-is/web-access

---

## 解决的问题

Claude Code 原生工具的局限：
- **WebSearch**：只能搜索
- **WebFetch**：只能获取静态页面
- **痛点**：无法处理需要登录、需要交互的网站

---

## 五层联网通道

| 工具 | 用途 | 特点 |
|-----|------|-----|
| WebSearch | 搜索信息 | 快速发现 |
| WebFetch | 获取网页 | 简单页面 |
| curl | HTTP 请求 | 底层控制 |
| Jina | 内容提取 | 高效抓取正文 |
| **CDP** | 浏览器操作 | 最强，能登录能交互 |

---

## CDP 核心能力

直连日常使用的 Chrome，**天然携带登录态**。

- 支持动态页面（JS 渲染、SPA）
- 支持交互操作（点击、输入、滚动、截图）
- 支持视频截帧分析
- 支持文件上传

### 配置步骤

1. **环境要求**：Node.js 22+
2. **开启 Chrome 远程调试**：
   - 地址栏打开 `chrome://inspect/#remote-debugging`
   - 勾选 "Allow remote debugging for this browser instance"
3. **检查环境**：
   ```bash
   bash ~/.claude/skills/web-access/scripts/check-deps.sh
   ```

---

## CDP Proxy API

代理地址：`http://localhost:3456`

### 常用命令

```bash
# 列出所有 tab
curl -s http://localhost:3456/targets

# 新建 tab
curl -s "http://localhost:3456/new?url=https://example.com"

# 执行 JS
curl -s -X POST "http://localhost:3456/eval?target=ID" -d 'document.title'

# 点击（JS 层）
curl -s -X POST "http://localhost:3456/click?target=ID" -d 'button.submit'

# 点击（真实鼠标事件）
curl -s -X POST "http://localhost:3456/clickAt?target=ID" -d '.upload-btn'

# 文件上传
curl -s -X POST "http://localhost:3456/setFiles?target=ID"   -d '{"selector":"input[type=file]","files":["/path/to/file.png"]}'

# 截图
curl -s "http://localhost:3456/screenshot?target=ID&file=/tmp/shot.png"

# 滚动
curl -s "http://localhost:3456/scroll?target=ID&direction=bottom"

# 关闭 tab
curl -s "http://localhost:3456/close?target=ID"
```

---

## 三种点击方式

| 方式 | 原理 | 适用场景 |
|-----|------|---------|
| `/click` | JS el.click() | 大部分常规场景 |
| `/clickAt` | CDP 真实鼠标事件 | 对点击校验严格的网站 |
| `/setFiles` | 直接设置文件路径 | 文件上传 |

---

## 实战经验

### 中文内容写入
通过 `/eval` 传输中文时，bash/curl 编码会破坏字符。

**解决方案**：用 Base64 编码
```bash
# 1. 生成 Base64
node -e "console.log(Buffer.from('中文内容', 'utf8').toString('base64'))"

# 2. 在浏览器中解码
const content = new TextDecoder('utf-8').decode(new Uint8Array(atob(base64).split('').map(c => c.charCodeAt(0))));
document.execCommand('insertText', false, content);
```

### 登录态问题
有些网站的登录态需要从主站跳转才能带到子站，直接打开子站 URL 会丢失登录态。

**解决方案**：从已登录页面点击链接进入，而不是直接导航。

---

## 设计哲学

**Skill = 哲学 + 技术事实，不是操作手册**

不要替 AI 推理，而是讲清楚 tradeoff 让 AI 自己选择。

---

## 相关链接

- [[Claude Code]]
- [[Obsidian CLI]]

#已完成