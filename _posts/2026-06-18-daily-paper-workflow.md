---
title: 'AI论文阅读工作流分享'
date: 2026-06-18
permalink: /posts/2026/06/daily-paper-workflow/
author_profile: false
tags:
  - research workflow
  - Zotero
  - Obsidian
  - Codex
  - paper reading
---

AI写作和AutoResearch的高速发展让论文数量井喷，我发现我越来越及时track当日的感兴趣论文，之前我vibe coding了一个网站用于收集当日 Arxiv感兴趣方向/感兴趣作者/Huggingface 推荐论文，每日的感兴趣方向的论文在600篇左右，这个数量即使我只看摘要扫一遍也需要很长时间，所以我慢慢迭代出了当前这版工作流，用AI来快速过滤出当日论文里的感兴趣方向的论文，并且自动添加到Zotero里面去；最后生成一个汇总markdown放到obsdian里面去，里面包含快速打开zotero论文的超链接，以及Zotero的LLM-For-Zotero的AI总结。

如果在扫这个摘要的时候对这篇论文感兴趣我会点击summarize，zotero后台会自动调用codex帮我生成详细的论文介绍。个人觉得想要真正理解一篇论文还是不能只看论文总结，而是需要看具体的论文，进行自己的思考；论文主题千奇百怪，同一个工作也有很多不同写法，具体对于每一篇论文的价值判断还是得依靠读者自己的学术taste，所以在论文阅读的过程中，我更多是和AI进行论文的内容的讨论，而不是直接让他告诉我他的想法。

具体工作流现在大概长这样：

1. Paper Easy 先读取当天的 arXiv Daily Papers，以及前一天的 Hugging Face Daily Papers。（因为自动化是每天早上八点开始抓取，所以当日的huggingface daily paper还没有生成）
2. Codex 根据我关心的主题筛选论文，比如世界模型、具身智能和多模态大语言模型。
3. 命中的论文会被同步进 Zotero，并放到 `Arxiv-Daily-Papers/YYYY-MM-DD` 这样的日期 collection 下，同时尽量补齐 PDF。
4. 最终的每日简报写入 Obsidian vault，位置是 `论文笔记/DailyPapers`。
5. 简报里每篇感兴趣论文都会带上 Zotero 跳转链接，以及几个 “LLM for Zotero” 的按需生成链接，比如 `Summarize`、`Methodology`、`Experiments`。同时为了方便后续扩展，也可以自己添加prompt.txt，在每日生成论文简报的时候会自动生成对应超链接。

我比较喜欢这个设计的一点是：它没有把 LLM 用在所有地方。LLM 只负责最适合它的部分，也就是语义筛选和按需阅读；而格式、归档、去重、断点续传、Zotero 写入、Obsidian 输出这些确定性的事情都交给脚本来做。我还是觉得对应确定性的事情应该交由脚本去做，而不是AI（虽然现在很多人希望端到端），这样生成质量更加可控，也会更有效率一点。

## 为什么不自动总结所有论文

一开始我也试过让自动化把每天命中的论文都生成一遍阅读笔记。但很快就会发现这件事有点浪费（如果对全部感兴趣的论文进行Codex总结，一天能大概筛选出60篇目标方向的论文，对其全部进行总结会花掉Codex 20x 订阅的周用量的3%）：有些论文只是当天扫一眼标题和摘要就够了，真正值得细读的其实只占一小部分。

所以现在我改成了“入口优先”的方式。每日 Markdown 里会列出论文、摘要、PDF、Zotero 链接，以及几个按需动作。比如我在 Obsidian 里点一下 `Summarize`，macOS 会通过一个本地 URL scheme 唤起脚本，只为这一篇论文生成一个 Zotero note。点 `Methodology` 就只生成方法部分的笔记。这样阅读动作更像是从简报里自然延伸出去，而不是每天批量生产一堆自己未必会看的总结。

## 每天最终看到的东西

最终的 Obsidian 简报分成两块：

- **感兴趣论文**：只放与当前研究方向明确相关的论文，并按主题归类。每篇论文包括中英文标题、中英文摘要、论文链接、PDF 链接、Zotero 链接和按需 note 链接。
- **Hugging Face Daily Papers**：完整列出前一天 Hugging Face Daily Papers，方便快速浏览社区当天关注的论文。

## 复现需要的资源

这套流程里用到的东西主要是下面这些：

- [Paper Easy Codex Plugin](https://github.com/YanxingLiu/paper-easy-codex-plugin)：我用来读取 arXiv Daily Papers 和 Hugging Face Daily Papers 的 Codex 插件。
- [Codex Obsidian Plugin](https://github.com/YanxingLiu/codex-obsidian)：我使用的 Obsidian 相关 Codex 插件 fork，用来把最终简报稳定写入 vault。
- [llm-for-zotero](https://github.com/yilewang/llm-for-zotero)：Zotero 里的 LLM 插件，用来读取论文全文并生成 note。
- [Zotero](https://www.zotero.org/)：论文归档、PDF 管理和阅读笔记的核心位置，Zotero插件用的codex app里openai开发的。
- [Obsidian](https://obsidian.md/)：每日论文简报的入口，也作为长期知识库的一部分。

我自己的自动化脚本按流程拆成了几步：准备输入 JSON、同步 Zotero 和 PDF、安装/处理按需 note 的 URL scheme、生成 Obsidian Markdown、按需生成 Zotero note。这个拆分的好处是每一步都可以单独验证，出了问题也比较容易定位。

## 一点感受

对我来说，这套流程最重要的不是“让 AI 帮我读完所有论文”，而是希望能以一个流畅的过程阅读完当日的论文。以前我会在网页、Zotero、Obsidian、PDF 阅读器之间来回切换；现在早上先看一份简报，觉得哪篇值得读，就点进 Zotero 或者按需生成一份 note。

它更像是一个研究入口，而不是一个替代阅读的机器。自动化负责把材料摆好，LLM 负责在我需要的时候展开某一篇论文，最后判断和取舍还是留给自己。

## 最终效果

最后展示一下当前成品的效果：Obsidian 中的每日论文简报、Zotero 中按日期归档的论文 collection，以及从简报点击 prompt 后在 Zotero 里按需生成的结构化阅读 note。

![AI 论文阅读工作流最终效果](/images/daily-paper-workflow-showcase.png)
