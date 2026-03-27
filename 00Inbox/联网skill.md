# 这个 skills 太硬核了，刚开源就斩获 1.7K Star！Agent 联网能力拉满！

原创 开源星探 开源星探

 _2026年3月27日 09:25_ _湖北_ 2人

最近在 AI 开发者圈子里，有一个话题被反复讨论：如何让 AI 助手真正"上网"？

不是简单的搜索一下，而是能够像真人一样操作浏览器、登录账号、完成复杂的交互任务。

Claude Code 其实早就内置了 WebSearch 和 WebFetch 工具，但用过的人都知道，这两个工具局限性太大了。

它们只能做简单的网页获取和搜索，完全无法处理需要登录态、需要复杂交互的场景。

就在大家都在为这些问题头疼的时候，一个叫 **web-access** 的 skill 横空出世，直接把 Claude Code 的联网和浏览器能力拉满了。

![](https://mmbiz.qpic.cn/mmbiz_png/rfBHhQhezUhK77EZvBFM7AK7OtFSf29DG3nE96ydL61zdMDQYnrpMr7PicwaDDAD2ldFCiaEDHGIROJbcr9RicfypyPaLI5yHdaiakiaY6X5e4Ts/640?wx_fmt=png&from=appmsg)

#### 项目简介

**web-access** 是由开发者 `一泽 Eze` 开源的一个 Claude Code skill，它的核心理念就是：**给 Claude Code 装上完整的联网和浏览器自动化能力**。

![](https://mmbiz.qpic.cn/mmbiz_png/rfBHhQhezUgGLSbO9bzicdnjNmic3DHvAJuQ1Tat7RDePmyyFzGhGJnISiaE1mM2niaFOmlZBBnJuANlj5CtsNibOThwicj8NwaYcFDPic97lysxbI/640?wx_fmt=png&from=appmsg)

这个 skill 不仅仅是简单地把几个工具拼在一起，而是从底层架构上重新设计了联网策略、浏览器操作和经验积累机制。

它解决了 Claude Code 原本存在的三大短板：

- • 缺少智能的联网工具调度策略
    
- • 没有真正的浏览器自动化能力
    
- • 无法积累和复用站点操作经验
    

有了 web-access 之后，Claude Code 终于可以像真人一样在互联网上自由穿梭了。

#### 核心能力

1、联网工具自动选择

web-access 最大的特点之一就是它的智能工具选择机制。它内置了五层联网通道：

- • **WebSearch**：用于搜索信息
    
- • **WebFetch**：用于获取网页内容
    
- • **curl**：用于简单的 HTTP 请求
    
- • **Jina**：用于高效的网页内容提取
    
- • **CDP**：用于完整的浏览器操作
    

五层通道，按需组合。最厉害的是，web-access 会根据具体场景**自主判断**使用哪个工具，甚至可以任意组合多个工具来完成复杂任务。

> 比如搜索某个产品的最新信息时，它会先用 WebSearch 找到相关链接，再用 WebFetch 或 Jina 获取详细内容，最后如果需要登录态操作，再切换到 CDP 模式。

2、CDP Proxy 浏览器操作

这绝对是 web-access 最硬核的功能。

通过 Chrome DevTools Protocol（CDP），web-access 可以**直连你日常使用的 Chrome 浏览器**。

- • **天然携带登录态**：你在 GitHub、🍠、Gmail、X 上登录的账号，web-access 都能直接使用
    
- • **支持动态页面**：JavaScript 渲染的页面、单页应用（SPA）都能完美处理
    
- • **支持交互操作**：点击、输入、滚动、截图都不在话下
    
- • **支持视频截帧**：可以对视频的任意时间点进行截帧分析
    

再也不用费劲心思去处理 cookie、token 这些登录态问题了，直接用你自己的 Chrome 浏览器就行！

3、三种点击方式

web-access 提供了三种不同的点击方式，每种都有特定的适用场景：

- • **/click**：使用 JavaScript 进行点击，适合大部分常规场景
    
- • **/clickAt**：使用 CDP 真实鼠标事件进行点击，适合那些对点击方式有严格校验的网站
    
- • **/setFiles**：专门用于文件上传操作，可以直接指定本地文件路径
    

这三种方式覆盖了几乎所有网页交互的需求，无论是简单的按钮点击还是复杂的文件上传，都能轻松搞定。

4、并行分治：多任务同时处理，效率翻倍

当你有多个目标需要同时处理时，web-access 会采用**并行分治**的策略：

- • 分发多个子 Agent 并行执行任务
    
- • 所有子 Agent 共享同一个 CDP Proxy
    
- • 每个子 Agent 使用独立的 tab，互不干扰
    

> 比如你想同时调研 5 个产品的官网，web-access 可以同时打开 5 个 tab，让 5 个子 Agent 分别去获取信息，最后再汇总成对比摘要。这种效率提升是单线程操作无法比拟的。

5、站点经验积累，越用越聪明

web-access 最有远见的设计就是它的**站点经验积累**机制：

- • 按域名存储操作经验
    
- • 记录 URL 模式、平台特征、已知陷阱
    
- • 跨 session 复用这些经验
    

6、媒体提取，图片视频一网打尽

web-access 还内置了强大的媒体提取能力：

- • 从 DOM 中直接提取图片和视频的 URL
    
- • 对视频的任意时间点进行截帧分析
    
- • 支持各种常见的媒体格式
    

无论是需要下载页面上的图片，还是需要分析视频中的某个画面，web-access 都能轻松完成。

#### 快速上手

安装方式

web-access 提供了两种安装方式，都非常简单：

方式一：让 Claude 自动安装（推荐）  
直接对 Claude 说："帮我安装这个 skill：https://github.com/eze-is/web-access"

方式二：手动安装

`git clone https://github.com/eze-is/web-access ~/.claude/skills/web-access`

CDP 模式配置（可选但强烈推荐）

如果你想使用完整的浏览器操作能力，需要配置 CDP 模式：

1. 1. **环境要求**：Node.js 22+
    
2. 2. **开启 Chrome 远程调试**：
    

- • 在 Chrome 地址栏打开 `chrome://inspect/#remote-debugging`
    
- • 勾选 "Allow remote debugging for this browser instance"
    
- • 可能需要重启浏览器
    

4. 3. **运行环境检查**：
    

`bash ~/.claude/skills/web-access/scripts/check-deps.sh`

1. 4. **启动 CDP Proxy**：
    

`node ~/.claude/skills/web-access/scripts/cdp-proxy.mjs &`

使用示例

安装完成后，你就可以直接让 Claude 执行各种联网任务了，web-access 会自动接管：

**简单搜索**："帮我搜索一下 AI 编程助手的最新进展"

**读取网页**："读一下这个页面：https://github.com/eze-is/web-access"

**多任务并行**："同时调研这 5 个产品的官网，给我对比摘要"

CDP Proxy API 参考

如果你想直接使用 CDP Proxy 的 API，这里有一些常用的示例：

`# 新建 tab   curl -s "http://localhost:3456/new?url=https://example.com"      # 执行 JS   curl -s -X POST "http://localhost:3456/eval?target=ID" -d 'document.title'      # JS 点击   curl -s -X POST "http://localhost:3456/click?target=ID" -d 'button.submit'      # 真实鼠标点击   curl -s -X POST "http://localhost:3456/clickAt?target=ID" -d '.upload-btn'      # 文件上传   curl -s -X POST "http://localhost:3456/setFiles?target=ID" \     -d '{"selector":"input[type=file]","files":["/path/to/file.png"]}'      # 截图   curl -s "http://localhost:3456/screenshot?target=ID&file=/tmp/shot.png"      # 滚动   curl -s "http://localhost:3456/scroll?target=ID&direction=bottom"      # 关闭 tab   curl -s "http://localhost:3456/close?target=ID"`

#### 设计哲学

web-access 的作者一泽 Eze 在项目中提到了一个非常有意思的设计哲学：**Skill = 哲学 + 技术事实，不是操作手册**。

这个理念的核心是：不要替 AI 推理，而是讲清楚 tradeoff 让 AI 自己选择。

web-access 不是给 AI 一本操作手册，告诉它"第一步做什么，第二步做什么"，而是给它提供一套工具和原则，让它根据具体情况自己做出决策。

这种设计方式让 web-access 具有了极高的通用性和上限。它不是为某个特定场景设计的，而是为所有联网和浏览器操作场景设计的。

#### 写在最后

web-access 的出现，彻底改变了 Claude Code 的能力边界。它不仅仅是一个工具，更是一个让 AI 真正"融入"互联网的桥梁。

这种设计理念让 Skill 更灵活、更智能，也更能适应复杂多变的实际场景。

这个 skill 的潜力巨大，随着版本的不断更新，它的能力还会越来越强。

如果你是一个深度使用 AI 编程助手的用户，可以去试试 web-access，一定会给你带来惊喜。

GitHub：

> https://github.com/eze-is/web-access

![](http://mmbiz.qpic.cn/mmbiz_png/NjA8gwicXyeI2VxoiawibkJicFf5vrWbd9LiaDXHrRCwEg9o0Ez33xZGP8q7HNLmB9lZFPLOGhk3zNo1J8r1BibM283w/300?wx_fmt=png&wxfrom=19)

**开源星探**

专注于分享GitHub上优质、有趣、实用的开源项目、工具及学习资源，为互联网行业爱好者提供优质的科技技术资讯。

633篇原创内容

公众号

  

  

  

![](https://mmbiz.qpic.cn/mmbiz_gif/NjA8gwicXyeKqAjyn8A3ob9xT4DDY8DB3JCvIaM6JKWXFsgCxznXicJhpRYJ5MIPb9xvgGA4WYhPagIKorlScib0Q/640?wx_fmt=gif)

  

  

  

如果本文对您有帮助，也请帮忙点个 赞👍 + 在看 哈！❤️