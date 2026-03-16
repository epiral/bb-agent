# bb-agent

I want to build an agent that my sister can use.

我想做一个我姐姐都能用的 agent。

She's not a developer. She doesn't know what an API is. But she knows what she wants: "help me find good restaurants people are talking about on Xiaohongshu and Douban." That should just work.

她不是开发者，不知道 API 是什么。但她知道自己想要什么："帮我看看小红书和豆瓣上大家在聊哪些好吃的餐厅。"这件事应该直接就能跑。

She can't use Opus or GPT-5. Not just because of the cost — some models she simply has no way to access. But she can run an open-source model. If the agent only works with frontier models, it doesn't work for her. It doesn't work for most people.

她用不了 Opus 或 GPT-5，不只是因为贵，有些模型她根本就没办法用。但她可以跑开源模型。如果 agent 只能跟前沿模型配合，那对她来说就是不能用。对大部分人来说都是不能用。

**AI Agent for everyone. Every model. Every language. Every website.**

**AI Agent 平权。任何模型，任何语言，任何网站。**

---

## The problem / 问题

Today's AI agents have a class divide:

今天的 AI Agent 存在一个阶级分化：

- **Frontier models** (Opus, GPT-5) can write code to interact with websites — reverse-engineer APIs, handle auth, parse responses. They work, but not everyone can access them.
- **Open-source models** (Qwen, GLM, Llama, DeepSeek) are smart enough to reason and plan, but they can't reliably write complex scraping code on the fly. So they're locked out of the real web.

- **前沿模型**（Opus、GPT-5）能写代码跟网站交互，逆向 API、处理认证、解析响应。能用，但不是每个人都能用到。
- **开源模型**（通义千问、智谱、Llama、DeepSeek）足够聪明，能推理能规划，但没法现场写出复杂的爬虫代码。所以它们被挡在了真实互联网之外。

**This is not a model problem. It's an interface problem.**

**这不是模型的问题，是接口的问题。**

## The solution / 解法

bb-agent is built on [bb-browser](https://github.com/epiral/bb-browser) site adapters. [95 adapters](https://github.com/epiral/bb-sites) have already done the hard work — reverse-engineering APIs, handling cookies, parsing responses, all packaged into one-line CLI commands that return structured JSON.

bb-agent 基于 [bb-browser](https://github.com/epiral/bb-browser) 的 site adapter 构建。[95 个 adapter](https://github.com/epiral/bb-sites) 已经做完了难的部分，逆向 API、处理 cookie、解析响应，全部封装成一行 CLI 命令，返回结构化 JSON。

```
# No code needed. Just CLI. Any model can do this.
# 不需要写代码，就是 CLI。任何模型都会。

bb-browser site twitter/search "AI agent"          → JSON
bb-browser site twitter/following elonmusk         → JSON
bb-browser site xiaohongshu/search "旅行攻略"       → JSON
bb-browser site bilibili/search "深度学习"          → JSON
bb-browser site youtube/search "machine learning"  → JSON
```

The intelligence requirement drops from writing code to calling commands:

对模型的智力要求从写代码降到了调命令：

```
Before / 之前:
  Agent needs Opus → (write code to reverse-engineer Twitter API) → maybe works

After / 之后:
  Agent needs any model → bb-browser site twitter/search "query" → always works
```

**Use frontier models to BUILD the adapters. Use any model to RUN them.**

**用前沿模型来造 adapter，用任何模型来跑任务。**

## What it can do / 能做什么

95 adapters across 35+ platforms. Twitter, YouTube, Reddit, Bilibili, Xiaohongshu, Douban, Zhihu, Weibo, HackerNews, GitHub, and more.

95 个 adapter 覆盖 35+ 平台。推特、YouTube、Reddit、B站、小红书、豆瓣、知乎、微博、HackerNews、GitHub 等。

- **Cross-platform workflows** — search Twitter, check Xiaohongshu, read Reddit, all in one task
- **Social graph analysis** — follow chains, find key people, map relationships
- **Content monitoring** — track keywords across platforms
- **Research automation** — collect and synthesize from multiple sources

- **跨平台工作流** — 搜推特、查小红书、读 Reddit，一个任务搞定
- **社交图谱分析** — 追踪关注链，找关键人物，画关系图
- **内容监控** — 跨平台关键词追踪
- **调研自动化** — 多源信息收集与综合

## How it works / 原理

```
bb-agent (any LLM: Qwen, GLM, DeepSeek, Llama, GPT, Claude...)
    │
    │  "search Twitter for AI agents, then check who the top authors follow"
    │
    ├── bb-browser site twitter/search "AI agent"
    ├── bb-browser site twitter/user {top_author}
    ├── bb-browser site twitter/following {top_author}
    │         ...just CLI commands, structured JSON in/out
    │
    ▼
  Results → LLM reasoning → next action → repeat
```

The browser is already logged in. bb-browser runs in your Chrome with your cookies, your sessions, your auth. No API keys needed. The adapter runs JS in the page context — it IS the page.

浏览器已经登录了。bb-browser 跑在你的 Chrome 里，用你的 cookie、你的会话、你的登录态。不需要 API key。adapter 在页面上下文里跑 JS，它就是页面本身。

## Status / 状态

Early stage. Building in public.

早期阶段，公开构建中。

## Related / 相关项目

- [bb-browser](https://github.com/epiral/bb-browser) — The browser automation CLI that powers everything
- [bb-sites](https://github.com/epiral/bb-sites) — Community-maintained site adapters (95 adapters, 35+ platforms)

## License

MIT
