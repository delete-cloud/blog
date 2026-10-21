---
title: Notes on building my own agent framework
date: 2026-03-29
description: From a Go coding-agent loop to a Python framework stitched together from bub, Tape System, Kapybara and jido.
tags: [agent, llm, python, notes]
---

After weeks of tinkering and research, this weekend I finally settled on the agent framework I wanted. It started as a Go-based, purely coding-focused agent loop, and ended up as a Python implementation that borrows from [bub](https://github.com/bubbuild/bub), [Tape System](https://tape.systems), [Kapybara](https://github.com/BeautyyuYanli/Kapybara) and [jido](https://github.com/agentjido/jido) — my own agent framework stitched out of their ideas. The overall approach borrows from bub too.

## Declarative design

A lot of it is declarative. Extensibility is arguably the most important property of a framework, and declarative design is what buys it. I took bub's "everything is a hook" and, per Opus's suggestion, jido's state_key-based fused design to keep plugins from getting too fine-grained. Whether it works as intended is still unverified.

## Layered memory

The memory part follows kapybara's design (which is also built on tape): layered, with grounding & getter — the former leaning toward external knowledge bases, the latter toward internal context. I haven't fully thought this part through myself yet.

## Lessons from practice

I rediscovered [Context Engineering from the Inside Out](https://blog.yellowday.day/posts/context_engineering_from_the_inside_out/) sitting in my tab bar, and it turned out to be more useful than expected — especially the Failure section. On the point of switching to a better model with higher reasoning effort: when I tested the coding agent loop, gemini 3 flash versus gpt 5.2 or even 5.4, the latter two didn't show a clear advantage. That could be down to my task design or the code, though. I hit a few other pitfalls too, which I'll skip here.

## Some stray thoughts

In polebug's latest WeChat article she wrote about over-indulging in vibe coding, and it resonated. Lately I've had little urge to record my life at all — my expressiveness feels dulled. I need to rebalance; once this framework is done, it's probably time for a break.

Maybe next I should figure out what kind of work and life I actually want.
