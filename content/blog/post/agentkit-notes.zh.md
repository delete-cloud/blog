---
title: 记一次自己的 Agent 框架实现历程
date: 2026-03-29
description: 从 Go 的 coding agent loop 到 Python 实现，缝了 bub、Tape System、Kapybara、jido 之后定型的一套 agent framework。
tags: [agent, llm, python, 随笔]
---

我的 agent 项目在经历了几周的折腾与调研后，这周末终于把自己想要的 agent 框架定好了。从最初的基于 Go 的、纯编程相关的 coding agent loop，到现在用 Python 实现，借鉴了 [bub](https://github.com/bubbuild/bub)、[Tape System](https://tape.systems)、[Kapybara](https://github.com/BeautyyuYanli/Kapybara)、[jido](https://github.com/agentjido/jido) 等项目之后，最终缝出了一个我自己的 agent framework。框架的整体思路其实也是借鉴的 bub。

## 声明式设计

这次很多地方用到了声明式的设计。可扩展性我觉得可以说是框架最重要的一个点，声明式保证了这一点：参考了 bub 的 everything is hook，opus 给出了 jido 的基于 state_key 的融合式设计，来保证 plugin 的粒度不会太细。不过具体实现效果还没验证。

## 记忆分层

记忆部分参考了 kapybara 的设计（kapybara 同样是以 tape 为基础的），分层实现：grounding & getter。前者偏向于外部知识库，后者偏向于内部情景。这部分设计我自己也还没完全想清楚。

## 实践部分的经验

无意间翻到了在标签栏里躺了一段时间的 [Context Engineering from the Inside Out](https://blog.yellowday.day/posts/context_engineering_from_the_inside_out/)，没想到挺受用的，尤其是 Failed 部分：换用更好模型和更高的推理等级这点——我之前测试 coding agent loop 的时候，对比 gemini 3 flash 和 gpt 5.2 甚至 gpt 5.4，后两者可能并没有什么优势，当然也可能是任务设计或者代码的问题。另外还有一些别的踩坑经验，就不一一展开了。

## 一点杂感

看了 polebug 姐最新的公众号文章里关于过度沉迷 vibe coding 的部分，自己也感同身受。最近感觉没什么记录生活的欲望了，甚至感觉表达力都下降了，需要重新调整一下，把这个 framework 实现完应该就可以歇一阵子了。

接下来或许该想一下自己内心到底真的想要什么样的工作与生活。
