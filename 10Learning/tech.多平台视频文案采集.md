# 多平台视频文案采集方案

## 背景
知识库需要增强多平台内容采集能力，特别是抖音、小红书等短视频平台。

## 当前进度
- [x] 抖音基础信息抓取测试成功（标题、描述、章节、数据统计）
- [ ] 完整口播文案提取 - 需要进一步研究

## 找到的开源方案

### 推荐方案
| 项目 | 方式 | 链接 |
|-----|------|------|
| BiliNote | AI 笔记生成，直接输出 Markdown | [GitHub](https://github.com/JefferyHcool/BiliNote) |
| AsrTools | 剪映官方接口，识别准确率高 | [知乎介绍](https://zhuanlan.zhihu.com/p/1890425137087612618) |
| douyin-text-extractor | Node.js 库，专门提取抖音文案 | [GitHub](https://github.com/wjllance/douyin-text-extractor) |
| Douyin_TikTok_Download_API | 多平台爬虫 API | [GitHub](https://github.com/Evil0ctal/Douyin_TikTok_Download_API) |
| stt | 本地 fast-whisper，离线运行 | [GitHub](https://github.com/jianchang512/stt) |

## 下一步
- [ ] 尝试 BiliNote（最省事，输出直接是 Markdown）
- [ ] 如果效果不好，试用 AsrTools（准确率更高）
- [ ] 长期方案：写脚本实现「甩链接→自动入库」

## 测试案例
- 抖音链接：https://v.douyin.com/h4AXzZ_20yQ/
- 测试结果：基础信息抓取成功，口播文案需要专门工具

#进行中