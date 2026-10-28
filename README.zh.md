[English](README.md) | **中文**

# AI Engineer NYC 2026：参会报告

这是我参加 **[AI Engineer NYC 2026](https://ai.engineer/nyc/2026)**（2026 年 10 月 12–14 日，Sheraton New York Times Square）的笔记。这次会议关注生产环境里的 AI，行业集中在银行、对冲基金、fintech、保险和资产管理。

这个 repo 列出我参加的 38 场 session：谁讲的、讲了什么、我的收获，以及有录像时的视频链接。它是我写参会报告和做分享时的参考资料。

- ⭐ 表示我在现场标星的重点场次。
- **📰 中文版 / Full write-up**（`sessions/zh/` 和 `sessions/`）是每场的完整文章：按顺序覆盖整场 talk 和每张 slide，slide 上的数据整理成表格，有 Q&A 的场次完整收录。**精确版**（`sessions/precise/`，中文）是底层的核对记录，带时间戳、语音转写勘误，以及各来源之间的出入。
- **讲了什么** 依据官方简介，有录像的场次也参考 YouTube 视频描述。**我的收获** 来自我在现场的笔记。
- 录像在陆续上传。没有视频链接的场次，表示截至 **2026-10-27** 检查时频道上还没有。
- 公开的 slide 很少。讲者或官方 talk 页面给了 slide 或 workshop 资料的，列在 **链接** 里；没有列出的就是我没找到。
- 有些 talk 发布视频时改了标题；这里用日程上的标题，视频标题不同时会注明。

## 在哪里看

| 来源 | 链接 |
| --- | --- |
| AI Engineer YouTube 频道（全部视频） | <https://www.youtube.com/@aiDotEngineer/videos> |
| AIE NYC 2026 Complete Playlist | <https://www.youtube.com/playlist?list=PLLKkCXQeuCis> |
| AI Engineer NYC 2026: Online Talks | <https://www.youtube.com/playlist?list=PLVanfCmVOgIg> |
| 第一天直播（10 月 13 日，Main Stage） | <https://www.youtube.com/watch?v=akEqiXFo3GM> |
| 第二天直播（10 月 14 日，Main Stage） | <https://www.youtube.com/watch?v=jiUC6IUMdvc> |
| 官方日程 | <https://ai.engineer/nyc/2026/schedule> |

Main Stage 的直播链接会跳到主办方为该场标注的章节位置。

## 目录

- [场次一览](#场次一览)
- [10 月 12 日（周一）](#10-月-12-日周一) (5 场)
- [10 月 13 日（周二）](#10-月-13-日周二) (19 场)
- [10 月 14 日（周三）](#10-月-14-日周三) (14 场)

## 场次一览

| 日期 | 时间 | 场次 | 讲者 | 视频 |
| --- | --- | --- | --- | --- |
| 10 月 12 日 | 9:00am–11:00am | [Orchestrating Claude Managed Agents](#orchestrating-claude-managed-agents) | CJ Avilla, Harrison Stall | [▶](https://www.youtube.com/watch?v=kiClTcJUnhc) |
| 10 月 12 日 | 11:05am–12:05pm | [Agent = Model + Harness: Build your own Memory-Aware AML AI Agent Harness for Financial Crime Detection](#agent--model--harness-build-your-own-memory-aware-aml-ai-agent-harness-for-financial-crime-detection) | Nacho Martinez | [▶](https://www.youtube.com/watch?v=KtBivPzmOM8) |
| 10 月 12 日 | 1:15pm–2:15pm | [Beyond “Done”: Building Verification Loops for AI Agents](#beyond-done-building-verification-loops-for-ai-agents) | Nick Nisi, Zack Proser | [▶](https://www.youtube.com/watch?v=q4WC4TowkBo) |
| 10 月 12 日 | 2:20pm–3:20pm | [Langfuse Workshop - Evals and Observability in regulated Industries](#langfuse-workshop---evals-and-observability-in-regulated-industries) | Doneyli De Jesus, Annabell Schäfer | [▶](https://www.youtube.com/watch?v=obfPzbapMIM) |
| 10 月 12 日 | 3:30pm–4:30pm | [Unhobbling the Frontier](#unhobbling-the-frontier) | Lance Martin | – |
| 10 月 13 日 | 9:00am–9:18am | [Vault: Making Unstructured Data Work for Investing](#vault-making-unstructured-data-work-for-investing) | Aaron Linsky, Sam Green | [▶](https://www.youtube.com/watch?v=CLve4Nx_RwI) |
| 10 月 13 日 | 9:18am–9:36am | [Anthropic: Accelerating frontier AI in Financial Services.](#anthropic-accelerating-frontier-ai-in-financial-services) | Belinda Neal, Keon Shahab | – |
| 10 月 13 日 | 9:36am–9:54am | [Balancing Acceleration and Safety for AI Agents](#balancing-acceleration-and-safety-for-ai-agents) | John Feminella | [▶](https://www.youtube.com/watch?v=iniryju23Sk) |
| 10 月 13 日 | 9:54am–10:12am | [Does Code Quality Even Matter?](#does-code-quality-even-matter) | Max Kanat-Alexander | [▶](https://www.youtube.com/watch?v=bU5gDVyu_28) |
| 10 月 13 日 | 10:12am–10:30am | [What does an AI engineer do?](#what-does-an-ai-engineer-do) | Aparna Dhinakaran | [▶](https://www.youtube.com/watch?v=ImnZ7s68jw8) |
| 10 月 13 日 | 10:40am–10:58am | [Can Any Model Write Like Me? Building a Benchmark for Creative Writing](#can-any-model-write-like-me-building-a-benchmark-for-creative-writing) | Louis-François Bouchard | [▶](https://www.youtube.com/watch?v=_dTF3NpPzGo) |
| 10 月 13 日 | 11:05am–11:23am | [Write the Answer First: Generating Eval Datasets for Agents With No Benchmark](#write-the-answer-first-generating-eval-datasets-for-agents-with-no-benchmark) | Nathanael Weill | [▶](https://www.youtube.com/watch?v=c_SPW__jF9w) |
| 10 月 13 日 | 11:30am–11:48am | [Your Financial Agent Passed Every Eval. Can You Defend Its Decision?](#your-financial-agent-passed-every-eval-can-you-defend-its-decision) | Anirudh Veeraragavan | – |
| 10 月 13 日 | 11:55am–12:13pm | [Lies, Damned Lies, and Evals: How to trust your agents when the stakes are high](#lies-damned-lies-and-evals-how-to-trust-your-agents-when-the-stakes-are-high) | Stephanie Jarmak | [▶](https://www.youtube.com/watch?v=A3FL_zTQKU0) |
| 10 月 13 日 | 12:20pm–12:38pm | [1,368 AI Failures: What They Teach Us About System Evals](#1368-ai-failures-what-they-teach-us-about-system-evals) | Vishal Srivastava, Tanmay Sah | [▶](https://www.youtube.com/watch?v=mMw3arwQsL0) |
| 10 月 13 日 | 1:40pm–1:58pm | [Proving Alpha: Showcasing AI's Value in Fundamental Investing](#proving-alpha-showcasing-ais-value-in-fundamental-investing) | Hari Kumar | [▶](https://www.youtube.com/watch?v=ZMZlAn6Pyfg) |
| 10 月 13 日 | 2:05pm–2:23pm | [Can AI Understand a Distressed Credit?](#can-ai-understand-a-distressed-credit) | Harneet Kaur | [▶](https://www.youtube.com/watch?v=WOszaC1WWbQ) |
| 10 月 13 日 | 2:30pm–2:48pm | [Why Frontier Models Fail on Billion Dollar Portfolios](#why-frontier-models-fail-on-billion-dollar-portfolios) | Chanyeol Choi, Janie Gu, Jack Haverty, Suyeol Yun | [▶](https://www.youtube.com/watch?v=aVbRSIQO2dE) |
| 10 月 13 日 | 2:55pm–3:13pm | [AI and the Future of Fundamental Investing](#ai-and-the-future-of-fundamental-investing) | Jack Woodruff, Vinoo Ganesh | [▶](https://www.youtube.com/watch?v=akEqiXFo3GM&t=19744s) |
| 10 月 13 日 | 3:20pm–3:38pm | [Your agent fix just broke something else](#your-agent-fix-just-broke-something-else) | Sofia Jakovcevic | [▶](https://www.youtube.com/watch?v=v5tm5EAzsAs) |
| 10 月 13 日 | 3:45pm–4:03pm | [Give the Model a Computer: The One Thing Every AI Agent Framework Agrees On](#give-the-model-a-computer-the-one-thing-every-ai-agent-framework-agrees-on) | Mislav Ivanda | – |
| 10 月 13 日 | 4:30pm–4:48pm | [A Hedge Fund as a Function](#a-hedge-fund-as-a-function) | Frank Long | [▶](https://www.youtube.com/watch?v=akEqiXFo3GM&t=24466s) |
| 10 月 13 日 | 4:48pm–5:06pm | [Frontier Models for Acting in Space and Time](#frontier-models-for-acting-in-space-and-time) | Pim de Witte | [▶](https://www.youtube.com/watch?v=akEqiXFo3GM&t=25339s) |
| 10 月 13 日 | 5:06pm–5:24pm | [Hermes gives you wings: Build on open foundations.](#hermes-gives-you-wings-build-on-open-foundations) | Dillon Rolnick | [▶](https://www.youtube.com/watch?v=akEqiXFo3GM&t=26480s) |
| 10 月 14 日 | 10:40am–10:58am | [The Hidden Costs of a Faster Model](#the-hidden-costs-of-a-faster-model) | Morgan Suo | [▶](https://www.youtube.com/watch?v=YvQ4BVTWfHI) |
| 10 月 14 日 | 11:05am–11:23am | [Building Better Agents Through End-to-End Evals](#building-better-agents-through-end-to-end-evals) | Ryan Stevens | – |
| 10 月 14 日 | 11:30am–11:48am | [We Let Our Financial Planners Write the Agents](#we-let-our-financial-planners-write-the-agents) | Matt Ragone | [▶](https://www.youtube.com/watch?v=gpmze-6ueGU) |
| 10 月 14 日 | 11:55am–12:13pm | [The Model Ate the Coding Harness. Now What?](#the-model-ate-the-coding-harness-now-what) ⭐ | Jediah Katz | [▶](https://www.youtube.com/watch?v=7Y4sbZgBdnc) |
| 10 月 14 日 | 12:20pm–12:38pm | [A/B Testing AI Products Is Weird: What Changes When the Product Is Probabilistic](#ab-testing-ai-products-is-weird-what-changes-when-the-product-is-probabilistic) | Supriya Subudhi | – |
| 10 月 14 日 | 1:40pm–1:58pm | [Building a self-improving software factory](#building-a-self-improving-software-factory) ⭐ | Zach Lloyd | – |
| 10 月 14 日 | 2:05pm–2:23pm | [Scaling Agentic RL Across the Globe](#scaling-agentic-rl-across-the-globe) ⭐ | Jason Mancuso | – |
| 10 月 14 日 | 2:30pm–2:48pm | [Your agent needs a skill to improve its skills](#your-agent-needs-a-skill-to-improve-its-skills) ⭐ | Ben Holmes | – |
| 10 月 14 日 | 2:55pm–3:13pm | [Build your own team of managed agents](#build-your-own-team-of-managed-agents) | Jack Wotherspoon | [▶](https://www.youtube.com/watch?v=Y_p3y6zP5QE) |
| 10 月 14 日 | 3:20pm–3:38pm | [A Roadmap for Evaluating Highly-Subjective Tasks](#a-roadmap-for-evaluating-highly-subjective-tasks) | Cameron Wolfe | – |
| 10 月 14 日 | 3:45pm–4:03pm | [Code Mode: your agent should write code](#code-mode-your-agent-should-write-code) | Mikyo King | [▶](https://www.youtube.com/watch?v=dAoOYFx7ZYw) |
| 10 月 14 日 | 4:30pm–4:48pm | [AI in Practice: An Inside Look at Apollo's Approach](#ai-in-practice-an-inside-look-at-apollos-approach) | Michael Manapat, Jack McDonald | [▶](https://www.youtube.com/watch?v=BOWYujhIQuo) |
| 10 月 14 日 | 4:48pm–5:06pm | [Process Supervision for Agents That Do Long-Horizon Work](#process-supervision-for-agents-that-do-long-horizon-work) ⭐ | Mitchell Troyanovsky | [▶](https://www.youtube.com/watch?v=jiUC6IUMdvc&t=29362s) |
| 10 月 14 日 | 5:06pm–5:24pm | [Everything will be Simulated](#everything-will-be-simulated) | Cristóbal Valenzuela | [▶](https://www.youtube.com/watch?v=jiUC6IUMdvc&t=30551s) |

## 10 月 12 日（周一）

### Orchestrating Claude Managed Agents

**讲者：** CJ Avilla (Member of Technical Staff, Anthropic); Harrison Stall (Member of Technical Staff, Anthropic)  
**时间 / 地点：** 9:00am–11:00am · Sutton Room (NY Ballroom W) · Workshop (1-2 hrs)  
**链接：** **[📰 中文版](sessions/zh/10-12-0900-orchestrating-claude-managed-agents.md)** · **[📰 Full write-up](sessions/10-12-0900-orchestrating-claude-managed-agents.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=c415d432-5b6f-4b4a-9d2c-2e02cfde99db) · [▶ 视频](https://www.youtube.com/watch?v=kiClTcJUnhc) · [精确版](sessions/precise/10-12-0900-orchestrating-claude-managed-agents.md)

**讲了什么。** 两小时的 workshop，讲 Claude Managed Agents 里的动态工作流，以及让它们不越界的管控。录像把问题放在 Unix、OS/360 和 Linux 都撞过的那堵协调之墙上，如今换成了同时运行大量 agent。（视频改名为「Why One AI Agent Isn’t Enough (And How to Run 1,000)」。）

<details><summary>官方介绍（英文原文）</summary>

> Dynamic workflows in Claude Managed Agents, and the controls that keep them in bounds.

</details>

### Agent = Model + Harness: Build your own Memory-Aware AML AI Agent Harness for Financial Crime Detection

**讲者：** Nacho Martinez (AI Developer Advocate, Oracle)  
**时间 / 地点：** 11:05am–12:05pm · Sutton Room (NY Ballroom W) · Workshop (1-2 hrs)  
**链接：** **[📰 中文版](sessions/zh/10-12-1105-agent--model--harness-build-your-own-mem.md)** · **[📰 Full write-up](sessions/10-12-1105-agent--model--harness-build-your-own-mem.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=898cb76b-41f3-47c0-ba31-6a5f8df40117) · [▶ 视频](https://www.youtube.com/watch?v=KtBivPzmOM8) · [Workshop 网站（练习）](https://workshopwaitingroom.com/) · [讲者 GitHub](https://github.com/jasperan) · [精确版](sessions/precise/10-12-1105-agent--model--harness-build-your-own-mem.md)

**讲了什么。** 模型是你改不了的部分；agent 质量的成败在 harness。这个 workshop 在 Oracle AI Database 上用一个 AML 数据集，从基本组件搭起 harness：长期记忆和 schema 扫描、语义检索与混合检索、向量索引的工具箱和 skill 箱，以及有上限的 agent 回合循环。（视频改名为「Memory-Aware Agent Harnesses for Anti-Money Laundering」。）

<details><summary>官方介绍（英文原文）</summary>

> Agent = Model + Harness. The model emits tokens; state, memory, retrieval, tool dispatch, identity, retries, and budgets. All these concepts are harness code — and that is where agent quality is won or lost.
>
> In this hands-on workshop, you build that harness from primitives against Oracle AI Database and a realistic AML dataset: Meridian Bank, 25 branches, ~1,200 transactions over 90 days, and transactions flagged by Anti Money Laundering (AML) rules.
>
> We will go through long-term memory and schema scanning with OAMP, semantic and hybrid RRF retrieval with ONNX in-database embeddings, vector-indexed toolbox and skillboxes, and a bounded agent_turn loop.
>
> After this workshop, you will know everything required to build your own agent harness from scratch, staying faithful to best agent harness and AI practices.
>
> You leave with a readable ~100-line loop you can operate, and a mental model that you can improve for your financial AI agents.

</details>

### Beyond “Done”: Building Verification Loops for AI Agents

**讲者：** Nick Nisi (Engineering Manager, WorkOS); Zack Proser (AI Engineer, WorkOS)  
**时间 / 地点：** 1:15pm–2:15pm · Sutton Room (NY Ballroom W) · Workshop (1-2 hrs)  
**链接：** **[📰 中文版](sessions/zh/10-12-1315-beyond-done-building-verification-loops.md)** · **[📰 Full write-up](sessions/10-12-1315-beyond-done-building-verification-loops.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=edb4f982-0cb8-48ee-8bf7-410255a40cb3) · [▶ 视频](https://www.youtube.com/watch?v=q4WC4TowkBo) · [幻灯片和练习（workshop workbench；返回 401，可能需要登录）](https://talks.workos.cloud/beyonddone) · [精确版](sessions/precise/10-12-1315-beyond-done-building-verification-loops.md)

**讲了什么。** 围绕 AI agent 搭一个验证与修复循环：Playwright 检查浏览器行为，评估器判断收集到的证据是否支撑每条验收标准，循环据此接受结果、要求一次有限的修复，或升级给人。然后故意用坏掉的输出去骗它，测试验证器本身。（视频改名为「Teach Your Agent to Prove It’s Done」。）

<details><summary>官方介绍（英文原文）</summary>

> Delegating work takes more than a capable model and a good prompt. It takes a system that can check results, catch unsupported claims, and decide when to retry or ask for help.
>
> In this hands-on, 60-minute workshop, you’ll build a verification and repair loop around an AI agent. We’ll use Playwright to check browser behavior and Jev to evaluate whether collected evidence supports specific acceptance criteria. Your code will combine those signals to accept work, request a bounded repair, or escalate to a human.
>
> Then we’ll try to fool it. Using deliberately broken outputs and misleading evidence, you’ll test not just the agent, but the verifier itself, and explore the trade-off between catching failures and interrupting people unnecessarily.
>
> Drawing on real projects involving delegated work and agent supervision, this workshop focuses on patterns you can apply with your own stack. You’ll leave with a working verification loop and a practical method for deciding what to automate, what to check, and where human judgment still belongs.

</details>

### Langfuse Workshop - Evals and Observability in regulated Industries

**讲者：** Doneyli De Jesus (Head of AI Engineering Solutions, Langfuse (by ClickHouse)); Annabell Schäfer (AI Engineering Solutions, Langfuse by ClickHouse)  
**时间 / 地点：** 2:20pm–3:20pm · Sutton Room (NY Ballroom W) · Workshop (1-2 hrs)  
**链接：** **[📰 中文版](sessions/zh/10-12-1420-langfuse-workshop---evals-and-observabil.md)** · **[📰 Full write-up](sessions/10-12-1420-langfuse-workshop---evals-and-observabil.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=f20b8d60-8a75-4ab3-a2fb-42159024e65f) · [▶ 视频](https://www.youtube.com/watch?v=obfPzbapMIM) · [Workshop 材料](https://langfuse.com/workshop) · [精确版](sessions/precise/10-12-1420-langfuse-workshop---evals-and-observabil.md)

**讲了什么。** 在受监管行业里打通从 demo 到生产的闭环，以一个银行客服 agent（Northwind Bank）为例：端到端 tracing、拦截并记录高风险请求的 guardrail、在 CI/CD 里基于数据集跑质量和合规 eval，以及一个展示主要失败模式的仪表盘。（视频改名为「Evals for High-Stakes AI Agents」。）

<details><summary>官方介绍（英文原文）</summary>

> Building an AI agent demo is easy. Getting it into production is hard: you need to know what it actually does, catch it when it goes wrong, and keep improving it. In regulated industries you also have to prove all of that. In this hands-on workshop, Annabell Schäfer and Doneyli De Jesus from Langfuse show how to close that loop, using a banking support agent, Northwind Bank, as the example.
>
> You'll see how to:
>
> • Trace every agent interaction end to end
>
> • Use guardrails to catch and block risky requests, and record each block
>
> • Evaluate answers for quality and compliance using datasets as part of your CI/CD
>
> • Spot your agent's top failure modes on a dashboard and estimate the business impact
>
> The session suits both AI leaders and engineers. Bring a laptop if you want to follow along: a seed script loads the demo into your own Langfuse project.

</details>

### Unhobbling the Frontier

**讲者：** Lance Martin (Member of Technical Staff, Anthropic)  
**时间 / 地点：** 3:30pm–4:30pm · Shannon Room (NY Ballroom E) · Workshop (1-2 hrs)  
**链接：** **[📰 中文版](sessions/zh/10-12-1530-unhobbling-the-frontier.md)** · **[📰 Full write-up](sessions/10-12-1530-unhobbling-the-frontier.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=6a4e7cfd-334a-4d3b-94ed-282acc905448) · [讲者博客：Automating eval design and hillclimbing with Claude](https://claude.dev/blog/automating-eval-design-and-hillclimbing/) · [精确版](sessions/precise/10-12-1530-unhobbling-the-frontier.md)  
**录像：** 尚未发布

**讲了什么。** Lance Martin 的论点：很多应用因为一些小的配置失误，落在成本–性能 frontier 的内侧，而不是 frontier 上。五个主题，每个都对应 Claude Code 里 `claude-api` skill 的一条命令：审查 prompt、用好 prompt cache、设计好的 eval、对着 eval 做 hill-climbing、为持续数小时到数天的任务设计 harness。幻灯片部分约 23 分钟，这一小时剩下的时间都是问答。

<details><summary>官方介绍（英文原文）</summary>

> In this workshop, we'll share several simple ways to get the most out of fronter models, including harness design, prompting, and settin g up evaluations for successful hillclimbing.

</details>

**我的收获**

- **Prompt 审查。** 为老模型写的技巧（验证仪式、要求更彻底的强调语）在 frontier 模型上只是浪费 token，互相矛盾的规则现在反而有害：frontier 模型会认真对待两条规则，结果无所适从。升级模型是审查 prompt 的好时机，不要只改 model ID。
- **Prompt cache。** cache 是严格的前缀匹配，哪怕改一个字节都会让它失效。prompt 按变化频率从低到高排列。`/claude-api cost-optimize` 在内部 benchmark 上找到最多约 72% 的成本降幅，性能没有明显变化。
- **好的 eval 设计**（与博客一致）：任务贴近生产；更强的模型或更高的 effort 得分更高；100% 以下留有余量，且这部分余量不是由含糊或无解的任务造成的；多次运行之间方差小。`/claude-api build-eval` 会陪你逐步搭好数据、eval 和 grader，对数据放在哪里没有限制。信任一个 grader 之前，先抽读一些打过分的 transcript。
- **Hill-climbing。** 主要风险是过拟合：benchmark 的特殊之处渗进 harness（例如因为 eval 需要而加了一个 OCR 工具）。`/claude-api hillclimb` 把 eval 分成训练集和测试集，只给分析器看训练集上的失败，每轮只改一处，如果训练集变好而测试集不动就回滚。它不保证收敛；停滞时，原因往往是含糊的案例或糟糕的 grader。
- Hill-climbing 先从文本层面（prompt、skill）开始：改起来便宜，和输出关系紧密。harness 代码的改动大，也更难归因。prompt 也是最容易过拟合的部分。
- **以成本为目标。** 即使 eval 已经饱和，也可以在性能持平的前提下对着更低成本做 hill-climbing。强模型配低 effort 往往比弱模型配高 effort 更便宜也更好；没有 eval 时，先试 Opus 5.5 配低 effort（effort 不是硬性的 token 上限）。博客的例子（44 张客服工单，留出 14 张）：Opus 4.8 高 effort 得分 74.4%，每张工单 4.6¢；Sonnet 5 低 effort 加改进后的 prompt 得分 98.9%，每张 1¢；在留出的工单上是 90.5% 对 78.6%，成本约为五分之一。
- **面向长时间任务的 harness。** 任务现在会跑数小时到数天，所以 Claude Managed Agents 把大脑（harness）和双手（sandbox）分开：session log 是独立的只追加对象，可以恢复；密钥放在一个 Claude 从不直接接触的 vault 里。
- **问答。** 对没有明确答案的 eval 标准（例如 UI），写一份人也能照着打分的 rubric，用二元判断代替 1–10 分制，让多个模型打分（分歧大是坏信号），并多跑几次重复。两两比较很适合含糊的案例。让 Claude 判断一个反复出现的工作流什么时候该变成 skill。避免对抗式采样（只挑某个模型做错的案例）。人的品味和方向仍由人负责；Claude 可以像一个离线的在线评估器那样扫 trace，找出可疑的案例。做 hill-climbing 时，更大的隐患是过拟合，而不是 eval awareness。
- 资料之间的出入：演讲中说 `claude-api` skill 的起始得分是 68%，成本节省约 90%；博客和幻灯片写的是 66.1% 和约五分之一的成本，上文用的是后者。

<details><summary>slide 照片（15 张）</summary>

<a href="slides/10-12-1530-unhobbling-the-frontier/01.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/02.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/03.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/04.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/05.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/06.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/07.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/08.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/09.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/10.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/11.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/12.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/13.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/14.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/15.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/15.jpg" width="260" alt="slide"></a>

</details>

## 10 月 13 日（周二）

### Vault: Making Unstructured Data Work for Investing

**讲者：** Aaron Linsky (Head of Product Engineering, Bridgewater); Sam Green (Team Lead, Applied AI - AIA Labs, Bridgewater)  
**时间 / 地点：** 9:00am–9:18am · Simons Room (Metropolitan East) · Featured Keynote  
**链接：** **[📰 中文版](sessions/zh/10-13-0900-vault-making-unstructured-data-work-for.md)** · **[📰 Full write-up](sessions/10-13-0900-vault-making-unstructured-data-work-for.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=a84ca3a8-cc2f-4b50-a635-fc380f6f04f6) · [▶ 视频](https://www.youtube.com/watch?v=CLve4Nx_RwI) · [▶ 直播 @ 7:22](https://www.youtube.com/watch?v=akEqiXFo3GM&t=442s) · [精确版](sessions/precise/10-13-0900-vault-making-unstructured-data-work-for.md)

**讲了什么。** Bridgewater 建了 Vault：来自 135 个外部来源的 1 亿多份文档，加上内部备忘录和会议记录，经过清洗、权限控制，并按公司的 ontology 打标签。三条经验：人和 agent 对搜索的需求不同；最大的收益来自一个同时服务两者的工具；真正的目标是让 agent 主动把值得投资人关注的内容推到面前。一个手工搭建的 benchmark 把 precision@10 从约 60% 提到了 90% 以上。

<details><summary>官方介绍（英文原文）</summary>

> Model intelligence and persistence have reached a level where we believe applied AI in the enterprise is now about context and connectors. Out of the box frontier intelligence in the investment domain is fundamentally limited by access to information. Bridgewater's investment philosophy is fundamental and systematic. We work to uncover truths about how economies operate, building up that understanding on one of the world's most comprehensive bitemporal structured data stores. But our investors don't work from structured data alone. They consume research, news, transcripts, and internal writing every day. An agent tasked with investment problems needs access to the same information our best human investors have, so we built the Vault: a centralized, high-quality repository of our best external and internal unstructured data, serving humans and agents alike. We'll walk through why we chose to solve this problem ourselves, tracing the Vault from our first instinct, which was to buy it, through expanding it across all of our third-party research and into our internal knowledge, building it up from a point solution into a platform. The Vault already powers PAT, our first generation of analyst agents, used daily by hundreds of Bridgewater investors, and is now enabling our second-generation Researcher.

</details>

**我的收获**

- 和 agent 驱动的投资与研究工作流直接相关。

<details><summary>slide 照片（26 张）</summary>

<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/01.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/02.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/03.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/04.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/05.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/06.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/07.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/08.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/09.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/10.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/11.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/12.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/13.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/14.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/15.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/16.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/17.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/18.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/19.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/19.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/20.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/20.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/21.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/21.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/22.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/22.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/23.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/23.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/24.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/24.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/25.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/25.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/26.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/26.jpg" width="260" alt="slide"></a>

</details>

### Anthropic: Accelerating frontier AI in Financial Services.

**讲者：** Belinda Neal (Managing Director, Financial Services, Anthropic); Keon Shahab (Applied AI Architect, Anthropic)  
**时间 / 地点：** 9:18am–9:36am · Simons Room (Metropolitan East) · Featured Keynote  
**链接：** **[📰 中文版](sessions/zh/10-13-0918-anthropic-accelerating-frontier-ai-in-fi.md)** · **[📰 Full write-up](sessions/10-13-0918-anthropic-accelerating-frontier-ai-in-fi.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=68a1e78b-569e-4314-a56f-f20508123cbe) · [精确版](sessions/precise/10-13-0918-anthropic-accelerating-frontier-ai-in-fi.md)  
**录像：** 直播里没有分章节，也没找到单独的视频

**讲了什么。** Anthropic 金融服务团队提出每家公司都要追上的两条 frontier：智能的 frontier（当下最好的模型能做什么）和采用的 frontier（公司有多少实际工作跑在上面）。从少数几个 agent 扩展到大量 agent，意味着 connector、skill 和管控只建一次。

<details><summary>官方介绍（英文原文）</summary>

> Frontier AI is moving faster than financial services is adopting it, and that gap is the opportunity. In this keynote, Belinda Neal and Keon Shahab of Anthropic set out the two frontiers every firm now has to reach: the frontier of intelligence, what the best model can do today, and the frontier of adoption, how much of the firm's real work runs on it. They show how to go from a handful of agents to many by building connectors, skills and controls once, and what it looks like when a whole industry does the same, with Claude for Financial Advisors as a worked example. You will leave with three questions to take back to your own firm.

</details>

<details><summary>slide 照片（17 张）</summary>

<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/01.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/02.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/03.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/04.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/05.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/06.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/07.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/08.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/09.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/10.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/11.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/12.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/13.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/14.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/15.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/16.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/17.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/17.jpg" width="260" alt="slide"></a>

</details>

### Balancing Acceleration and Safety for AI Agents

**讲者：** John Feminella (SVP Architecture, Two Sigma)  
**时间 / 地点：** 9:36am–9:54am · Simons Room (Metropolitan East) · Featured Keynote  
**链接：** **[📰 中文版](sessions/zh/10-13-0936-balancing-acceleration-and-safety-for-ai.md)** · **[📰 Full write-up](sessions/10-13-0936-balancing-acceleration-and-safety-for-ai.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=73badd10-6f8c-4c95-b5e7-55fc3d7bafc1) · [▶ 视频](https://www.youtube.com/watch?v=iniryju23Sk) · [▶ 直播 @ 23:55](https://www.youtube.com/watch?v=akEqiXFo3GM&t=1435s) · [讲者文章（DEV）](https://dev.to/dailycontext/brakes-let-you-go-fast-balancing-acceleration-and-safety-for-ai-agents-49p9) · [精确版](sessions/precise/10-13-0936-balancing-acceleration-and-safety-for-ai.md)

**讲了什么。** 用 2020 年 Citibank/Revlon 9 亿美元（$900M）付款错误为例，说明如何在不把一切锁死的前提下给 agent 真正的自主权。五个可组合的管控：agent 注册表、按风险分级的审查、跟随 agent 声明用途的工具权限、全公司共享的 eval，以及只把真正要紧的审批交给人的分类器。（视频改名为「What a $900M Bank Error Teaches Us About AI Agents」。）

<details><summary>官方介绍（英文原文）</summary>

> As AI agents grow in utility and power, they also grow in potential risk. For some, this creates a balancing act: too little safety means highly regulated firms are exposed to undue risk; too little innovation means agents cost more to run than they earn.  How do we balance the desire for safe agentic AI usage with the goal of using agents to carry out innovative research and engineering? In this talk, I’ll cover some of the key technical approaches we use and have found highly effective at Two Sigma, and along the way hopefully convince you that there doesn’t need to be a tradeoff between these two properties.

</details>

**我的收获**

- agent 注册表加打分，按风险分层审查。
- 审批疲劳让逐个动作的人工审批行不通；答案是用分类器决定哪些审批要交给人（类似 Claude 的 auto mode）。
- 开放问题：权限是用 allow/deny 列表还是按声明用途来划定；让 agent 自己声明权限会不会让人成为瓶颈；为什么 eval 应该全公司共享而不是各自定制。
- 比喻：汽车有刹车，才能开得快。

<details><summary>slide 照片（24 张）</summary>

<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/01.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/02.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/03.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/04.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/05.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/06.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/07.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/08.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/09.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/10.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/11.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/12.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/13.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/14.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/15.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/16.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/17.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/18.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/19.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/19.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/20.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/20.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/21.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/21.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/22.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/22.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/23.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/23.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/24.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/24.jpg" width="260" alt="slide"></a>

</details>

### Does Code Quality Even Matter?

**讲者：** Max Kanat-Alexander (Principal Software Engineer, Capital One)  
**时间 / 地点：** 9:54am–10:12am · Simons Room (Metropolitan East) · Featured Keynote  
**链接：** **[📰 中文版](sessions/zh/10-13-0954-does-code-quality-even-matter.md)** · **[📰 Full write-up](sessions/10-13-0954-does-code-quality-even-matter.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=5191589c-a70e-4806-99dc-86f7794f46b9) · [▶ 视频](https://www.youtube.com/watch?v=bU5gDVyu_28) · [▶ 直播 @ 43:51](https://www.youtube.com/watch?v=akEqiXFo3GM&t=2631s) · [精确版](sessions/precise/10-13-0954-does-code-quality-even-matter.md)

**讲了什么。** 代码质量为两件事服务：理解一个系统，以及安全地修改它。AI 让一种新软件变得常见：一次性软件，后果小、依赖方少、替换成本几乎为零，这类软件的质量无所谓。而对长期存续的软件，质量比以往任何时候都重要。（视频改名为「Does Code Quality Still Matter? It Depends on This」。）

<details><summary>官方介绍（英文原文）</summary>

> There's so much debate online about "should we read the code?" Some people are yelling, "Everybody should build everything with a dark factory!" Others cry out, "The slop will destroy us all!" What this whole conversation is missing is principles for understanding how we would even make these decisions. Come hear the author of Code Simplicity say what he thinks is true now with AI coding, and see if there's some way to reliably find a path through all these opposing viewpoints.

</details>

**我的收获**

- 质量风险会累积：结构差让系统更难理解，又让未来更容易出问题。
- 长期软件要用很多年，依赖方多，出故障后果严重，需要持续维护。一次性软件正好相反。
- 通过隔离、独立、冗余和限定范围，让软件更「一次性」。
- 让长期软件的严谨变便宜：标准化、嵌在工作流里的工具、更容易的测试（例如 in-memory fake 这样的测试基础设施），lint 规则开到最严，误报变得没那么要紧。
- 这不是非黑即白，长期软件工程也不会消失。

<details><summary>slide 照片（16 张）</summary>

<a href="slides/10-13-0954-does-code-quality-even-matter/01.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/02.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/03.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/04.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/05.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/06.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/07.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/08.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/09.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/10.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/11.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/12.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/13.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/14.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/15.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/16.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/16.jpg" width="260" alt="slide"></a>

</details>

### What does an AI engineer do?

**讲者：** Aparna Dhinakaran (Co-Founder & Chief Product Officer, Arize AI)  
**时间 / 地点：** 10:12am–10:30am · Simons Room (Metropolitan East) · Featured Keynote  
**链接：** **[📰 中文版](sessions/zh/10-13-1012-what-does-an-ai-engineer-do.md)** · **[📰 Full write-up](sessions/10-13-1012-what-does-an-ai-engineer-do.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=b2ed4456-5671-4e8e-83fd-684c012fef8c) · [▶ 视频](https://www.youtube.com/watch?v=ImnZ7s68jw8) · [▶ 直播 @ 1:00:43](https://www.youtube.com/watch?v=akEqiXFo3GM&t=3643s) · [精确版](sessions/precise/10-13-1012-what-does-an-ai-engineer-do.md)

**讲了什么。** 当这个领域从写功能转向运营 agent 工厂，工作就变成搭建闭环：评审非确定性工作的 eval、供 agent 消费的遥测数据，以及据此采取行动的 agent。演示了 Arize 的 Signal：一个常驻 agent，会读 trace、记住过去的问题并提 PR。（视频改名为「What Does an AI Engineer Do Now?」。）

<details><summary>官方介绍（英文原文）</summary>

> Coding agents have effectively eliminated the need for manually writing code, and the current frontier is how to eliminate code review. But where are we going if we look past that? What happens when agents master devops, architecture, scaling, and security? What is an engineer if agents can do all the building? In this look into the next few years, Aparna Dhinakaran argues that the core skill that remains is the definition of “good”: knowing what your users want, and what it looks like when it’s “done”, are the two things agents don’t know and aren’t going to get to. At the same time, more and more AI is going to be involved in software, so software is going to get steadily less deterministic. That presents a dual challenge: you have to get good at defining success, while simultaneously success gets harder to measure. How to tackle those challenges is the subject of this talk.

</details>

<details><summary>slide 照片（12 张）</summary>

<a href="slides/10-13-1012-what-does-an-ai-engineer-do/01.jpg"><img src="slides/10-13-1012-what-does-an-ai-engineer-do/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1012-what-does-an-ai-engineer-do/02.jpg"><img src="slides/10-13-1012-what-does-an-ai-engineer-do/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1012-what-does-an-ai-engineer-do/03.jpg"><img src="slides/10-13-1012-what-does-an-ai-engineer-do/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1012-what-does-an-ai-engineer-do/04.jpg"><img src="slides/10-13-1012-what-does-an-ai-engineer-do/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1012-what-does-an-ai-engineer-do/05.jpg"><img src="slides/10-13-1012-what-does-an-ai-engineer-do/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1012-what-does-an-ai-engineer-do/06.jpg"><img src="slides/10-13-1012-what-does-an-ai-engineer-do/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1012-what-does-an-ai-engineer-do/07.jpg"><img src="slides/10-13-1012-what-does-an-ai-engineer-do/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1012-what-does-an-ai-engineer-do/08.jpg"><img src="slides/10-13-1012-what-does-an-ai-engineer-do/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1012-what-does-an-ai-engineer-do/09.jpg"><img src="slides/10-13-1012-what-does-an-ai-engineer-do/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1012-what-does-an-ai-engineer-do/10.jpg"><img src="slides/10-13-1012-what-does-an-ai-engineer-do/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1012-what-does-an-ai-engineer-do/11.jpg"><img src="slides/10-13-1012-what-does-an-ai-engineer-do/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1012-what-does-an-ai-engineer-do/12.jpg"><img src="slides/10-13-1012-what-does-an-ai-engineer-do/12.jpg" width="260" alt="slide"></a>

</details>

### Can Any Model Write Like Me? Building a Benchmark for Creative Writing

**讲者：** Louis-François Bouchard (CTO & Co-Founder, Towards AI)  
**时间 / 地点：** 10:40am–10:58am · Turing Room (Riverside Ballroom) · Breakouts  
**链接：** **[📰 中文版](sessions/zh/10-13-1040-can-any-model-write-like-me-building-a-b.md)** · **[📰 Full write-up](sessions/10-13-1040-can-any-model-write-like-me-building-a-b.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=736edd83-bb08-49f1-90ed-722b7ed07eb2) · [▶ 视频](https://www.youtube.com/watch?v=_dTF3NpPzGo) · [讲者文章（DEV）](https://dev.to/dailycontext/what-9050-ai-written-scripts-taught-me-about-slop-1g5n) · [精确版](sessions/precise/10-13-1040-can-any-model-write-like-me-building-a-b.md)

**讲了什么。** 哪个模型写的 YouTube 脚本听起来像作者本人？拆解了 AI 写作为什么是「slop」，并展示了一个个人 benchmark 的搭法：10 个脚本、每个模型 5 份草稿、4 个 LLM 评委、九项打分指标，以及 1,000 次人工 A/B 检查。（视频改名为「Why AI Writing Still Reads Like Slop」。）

<details><summary>官方介绍（英文原文）</summary>

> Four months ago, out of frustration with slop, I set out to answer one question: which model writes a YouTube script that sounds like me. Not best in general, not best at writing. Which one passes as my voice, on my channel, by my standards, and isn't cringe to read out loud.
>
> So I vibe coded my own benchmark to answer it.
>
> Benchmarking 148 models took about two weeks, two 20x subscriptions on Claude and Codex, and $80 in API credits. That was never the bottleneck.
>
> The four months went into learning to build a proper benchmark. Deciding what a score means when the thing you are measuring is taste. Writing rubrics that quantify my subjectivity. Working out whether Elo was even the right tool. Finding out the same drafts average 74 out of 100 with one judge and 88 with another. Writing 10 scripts with two human editors and withholding half, so I could tell whether a model is contaminated.
>
> Most of the benchmark uses automated judging, but human judging is crucial for analyzing taste.
>
> So I added human ratings, sat down, and rated the first few hundred pairwise comparisons myself. My evaluation was broken, and only doing the work by hand revealed it.
>
> I fixed it, rated again, and I am releasing the benchmark publicly to get help rating these models.
>
> I will show the architecture, the numbers, what it cost, and the failure modes I would look for first in yours.

</details>

**我的收获**

- Slop 主要是用滥了的词和太泛。
- 每个模型五份草稿，因为有离群值；评委偏爱自家模型。
- 迭代很贵：按幻灯片，改一次 rubric 后整张榜重新评判花了 $2,350，重新生成再评判花了 $3,750。
- 用 AI 写作：不要 one-shot，写作不要用最高 effort，用很长、很细的 skill 和示例给出非常明确的指示，并建自己的评估。

<details><summary>slide 照片（31 张）</summary>

<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/01.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/02.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/03.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/04.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/05.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/06.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/07.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/08.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/09.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/10.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/11.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/12.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/13.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/14.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/15.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/16.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/17.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/18.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/19.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/19.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/20.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/20.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/21.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/21.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/22.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/22.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/23.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/23.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/24.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/24.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/25.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/25.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/26.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/26.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/27.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/27.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/28.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/28.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/29.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/29.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/30.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/30.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/31.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/31.jpg" width="260" alt="slide"></a>

</details>

### Write the Answer First: Generating Eval Datasets for Agents With No Benchmark

**讲者：** Nathanael Weill (Director, Data Science & AI, Air Canada)  
**时间 / 地点：** 11:05am–11:23am · Turing Room (Riverside Ballroom) · Breakouts  
**链接：** **[📰 中文版](sessions/zh/10-13-1105-write-the-answer-first-generating-eval-d.md)** · **[📰 Full write-up](sessions/10-13-1105-write-the-answer-first-generating-eval-d.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=c4ba0138-6f6d-4780-93a1-4d8f0c478339) · [▶ 视频](https://www.youtube.com/watch?v=c_SPW__jF9w) · [精确版](sessions/precise/10-13-1105-write-the-answer-first-generating-eval-d.md)

**讲了什么。** Air Canada 有些 agent 没有公开 benchmark，领域专家又没时间。办法是先写答案：从一个已知答案出发，由它生成输入，再看 agent 能不能找回这个答案。之前一个 RAG 项目从每周一次专家打分的测试，变成每天几十次自动化测试；同样的思路用在了一个供应商合同审查 agent 上。（视频改名为「How to Test AI Agents When No Benchmark Exists」。）

<details><summary>官方介绍（英文原文）</summary>

> Not long ago, nobody knew what a good evaluation strategy for a RAG system looked like. Each team invented its own, and it took months. Then RAGAS arrived. Faithfulness, context relevance, and answer relevance became a playbook you could look up, and the same evaluation now takes a week.
>
> What's the equivalent for a custom agent, like one that reviews contracts, triages invoices, or runs your internal workflows? Nothing. There's no benchmark, no labels, and no playbook, so every team is back to inventing its own and losing the same months.
>
> This talk shares the techniques I used to generate eval datasets for an agent no benchmark covers, along with how to tell when a generated dataset will help you and when it will quietly mislead you. It won't be RAGAS for agents, but it will be enough to get started on Monday.

</details>

**我的收获**

- 在 2024 年的 RAG 项目里，每加一个组件（更好的解析、chunker、re-ranker……）质量都不动，而每次改动都要等一周专家评审。先写答案让测试可以自动化；在 2026 年的合同项目上，反馈周期从 4 周缩到 5 分钟。
- 供应商合同审查没有公开 benchmark，所以测试集只能自己建，并配上一个更长的外层循环。

<details><summary>slide 照片（26 张）</summary>

<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/01.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/02.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/03.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/04.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/05.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/06.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/07.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/08.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/09.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/10.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/11.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/12.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/13.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/14.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/15.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/16.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/17.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/18.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/19.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/19.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/20.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/20.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/21.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/21.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/22.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/22.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/23.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/23.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/24.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/24.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/25.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/25.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/26.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/26.jpg" width="260" alt="slide"></a>

</details>

### Your Financial Agent Passed Every Eval. Can You Defend Its Decision?

**讲者：** Anirudh Veeraragavan (Lead Security Engineer, Fieldguide)  
**时间 / 地点：** 11:30am–11:48am · Turing Room (Riverside Ballroom) · Breakouts  
**链接：** **[📰 中文版](sessions/zh/10-13-1130-your-financial-agent-passed-every-eval-c.md)** · **[📰 Full write-up](sessions/10-13-1130-your-financial-agent-passed-every-eval-c.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=30afe596-abe2-417c-bd5a-313fa4bc54b3) · [精确版](sessions/precise/10-13-1130-your-financial-agent-passed-every-eval-c.md)  
**录像：** 尚未发布

**讲了什么。** 一个金融 agent 可以通过所有 eval，却仍然做出没人能辩护的决定。可辩护需要四项管控：授权、执行控制、证据溯源和评估完整性，并在一条合成的审计决策 trace 上逐一演示。

<details><summary>官方介绍（英文原文）</summary>

> A financial agent can pass every eval and still produce a decision no one can defend. The citation may point to the wrong document. The reasoning may omit the key business rule. The benchmark may treat last year's policy as today's ground truth. A high aggregate score can hide the failure that matters most.
>
> Defensibility requires four distinct controls: authorization, execution control, evidence provenance, and evaluation integrity. Using a synthetic audit decision trace, I'll work through each control: limiting what an agent can do, preserving what it did so contested steps can be reproduced, binding its material claims to the correct sources, and verifying whether its benchmark can be trusted. I'll also show where the agent should stop and a human should decide.
>
> You'll leave with a practical assurance pattern for financial agents whose decisions may be challenged months or years later. When that challenge comes, "the model said so" is not a defensible answer.

</details>

<details><summary>slide 照片（19 张）</summary>

<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/01.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/02.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/03.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/04.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/05.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/06.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/07.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/08.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/09.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/10.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/11.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/12.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/13.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/14.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/15.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/16.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/17.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/18.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/19.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/19.jpg" width="260" alt="slide"></a>

</details>

### Lies, Damned Lies, and Evals: How to trust your agents when the stakes are high

**讲者：** Stephanie Jarmak (AI Engineer, Omni Analytics)  
**时间 / 地点：** 11:55am–12:13pm · Turing Room (Riverside Ballroom) · Breakouts  
**链接：** **[📰 中文版](sessions/zh/10-13-1155-lies-damned-lies-and-evals-how-to-trust.md)** · **[📰 Full write-up](sessions/10-13-1155-lies-damned-lies-and-evals-how-to-trust.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=b1fe8396-6fa8-4574-9775-0dc01f56ce57) · [▶ 视频](https://www.youtube.com/watch?v=A3FL_zTQKU0) · [精确版](sessions/precise/10-13-1155-lies-damned-lies-and-evals-how-to-trust.md)

**讲了什么。** 大多数 agent 评估很容易被误读。只换 harness 就让结果变动 30%；公开 benchmark 用来初筛，不用来做决定。像设计实验一样设计自己的 eval：真实的任务、重复运行、完整的 trace，以及和你真正关心的东西相符的度量。（视频改名为「Don’t Trust Your AI Agents. Test Them Like a Scientist」。）

<details><summary>官方介绍（英文原文）</summary>

> Your coding agent scores 74%. The release decision is due today. Nobody can say what that number has earned the right to decide.
>
> That ambiguity matters everywhere, but especially in regulated industries. Agents can modify transaction systems, access controls, reporting pipelines, and compliance-sensitive infrastructure. When software can affect money, customers, or regulatory obligations, “the benchmark looked good” is not a defensible release argument. Trust has to be supported by evidence that can survive challenge.
>
> An eval score compresses a model, task set, execution environment, evaluator, grader, and one sample from a variable process into a single number. Financial institutions have spent decades developing disciplines for decisions built on imperfect models: define the intended use, validate performance, expose limitations, independently challenge assumptions, and monitor what happens in production. In AI, we often just print the result to one decimal place and put it on a leaderboard.
>
> In this talk, I’ll put one convincing score on trial. The audience will choose between two release candidates, then reconsider that decision as I reveal what the aggregate concealed: a +5.4-point win in my own repository-level eval that shrank to +0.35 after repeated runs; a widely cited coding benchmark whose sponsor stopped reporting it after uncovering flawed tasks and contamination; and model graders that agreed with one another while missing the defects the release gate was supposed to catch.
>
> We’ll rebuild the decision around five checks:
>
> - What decision is this score allowed to change?
>
> - How large is the run-to-run variance?
>
> - Does the task distribution match the real workload?
>
> - Which plausible failures can the evaluator actually detect?
>
> - What are the grader’s true-positive and false-positive rates at the chosen threshold?
>
> You’ll leave with a practical evidence standard for deciding when an agent has earned greater autonomy, and when a precise score is disguising uncertainty that still belongs with a human decision-maker.

</details>

**我的收获**

- 公开 benchmark 与内部评估：终端编程 benchmark 帮不了律所的 agent。
- 在模型、任务和 harness 之间设对照实验。在引用的 Terminal-Bench 子集上，Claude Opus 5 在 OpenHands 下解出 36/63 个任务，在极简的 Pi harness 下只有 19/63，而 GPT-6 Astra 正好相反（31 对 38）：光换 harness 就能翻转模型排名。
- 人工评审的工作量是实实在在的瓶颈。

<details><summary>slide 照片（12 张）</summary>

<a href="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/01.jpg"><img src="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/02.jpg"><img src="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/03.jpg"><img src="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/04.jpg"><img src="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/05.jpg"><img src="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/06.jpg"><img src="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/07.jpg"><img src="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/08.jpg"><img src="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/09.jpg"><img src="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/10.jpg"><img src="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/11.jpg"><img src="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/12.jpg"><img src="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/12.jpg" width="260" alt="slide"></a>

</details>

### 1,368 AI Failures: What They Teach Us About System Evals

**讲者：** Vishal Srivastava (Corporate VP, Lead Data Scientist, New York Life Insurance); Tanmay Sah (AI Researcher)  
**时间 / 地点：** 12:20pm–12:38pm · Turing Room (Riverside Ballroom) · Breakouts  
**链接：** **[📰 中文版](sessions/zh/10-13-1220-1368-ai-failures-what-they-teach-us-abou.md)** · **[📰 Full write-up](sessions/10-13-1220-1368-ai-failures-what-they-teach-us-abou.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=9d72d7ac-a2a2-42cf-b880-5859316d840c) · [▶ 视频](https://www.youtube.com/watch?v=mMw3arwQsL0) · [讲者文章（DEV）](https://dev.to/dailycontext/what-1368-real-world-ai-incidents-teach-us-about-evaluation-71p) · [精确版](sessions/precise/10-13-1220-1368-ai-failures-what-they-teach-us-abou.md)

**讲了什么。** 分析了 1,368 起有记录的 AI 事故，分为三个领域（技术强度、部署管控、业务影响）和 13 个子领域，覆盖约 98% 的事故。人工审查和监控这类部署管控薄弱，与危害关系最密切。还介绍了一个 build-break-fix 循环，目标是证明系统是错的，而不是证明它是对的。（视频改名为「1,368 AI Failures and What They Teach About Evals」。）

<details><summary>官方介绍（英文原文）</summary>

> We analyzed 1,368 documented AI failures across healthcare, financial services, law enforcement, professional services, and consumer technology.
>
> The clearest lesson was not simply that AI models fail.
>
> AI risk lives in the system, not just the model.
>
> In our analysis, 92% of incidents implicated multiple weaknesses simultaneously. The typical documented failure crossed several parts of the deployed system: technical behavior, data, workflows, human oversight, controls, exposure, and downstream consequence.
>
> That creates a fundamental problem for how we evaluate increasingly agentic AI systems.
>
> Most evals still ask some version of one question:
>
> Did the model or agent complete the task correctly?
>
> But task success is not system success.
>
> An agent can reach the expected answer after retrieving unsupported evidence, selecting the wrong tool, passing invalid arguments, crossing an authority boundary, or taking an action that should never have been available to it.
>
> The output can say “pass” while the system tells a different story.
>
> Drawing on the research behind our forthcoming book, Evaluable AI: From Model Evals to System Evals, this talk introduces a different unit of evaluation: the deployed AI system.
>
> We use Risk-Based Evaluability Design (R-BED) to organize system evaluation around three connected questions:
>
> Technical Failure: Where can the system fail?
>
> Deployment Harm: Under what conditions can that failure reach people, processes, or consequential decisions?
>
> Business Impact: If it does, how broadly and severely can the consequence propagate, and what can be recovered?
>
> These questions cannot be collapsed into a single score or task-success metric. A technically weak system may remain contained. A seemingly small technical error may become material because authority, exposure, or controls allow it to propagate. And a system that appears to perform well may still be difficult to trust if its behavior cannot be reconstructed and challenged.
>
> This leads to the concept at the center of the talk: evaluability.
>
> Evaluability is the ability to determine, with evidence, whether a deployed AI system remains within acceptable bounds and continues to serve its intended purpose.
>
> For agentic systems, that means making the trajectory observable: the evidence retrieved, sources used, tool selected, arguments passed, state transitions made, authority exercised, human interventions triggered, and actions taken.
>
> Tanmay Sah and I will demonstrate this through a reproducible evaluation pipeline applied to benchmark agent trajectories. We will show where task-level success can conceal failures elsewhere in the trajectory, what can be evaluated directly from system evidence, where an evaluator or judge is required, and when the correct evaluation result is not pass or fail but insufficient evidence.
>
> We then turn those findings into an engineering discipline:
>
> BUILD: Define the system claim, operating boundary, evidence requirements, and permitted authority.
>
> BREAK: Stress the system, discover failure regions, and trace failures across evidence, execution, tools, workflows, and controls.
>
> FIX: Constrain, redesign, route, monitor, or intervene, then verify that the change actually worked.
>
> The key insight is that meaningful system evals cannot simply be bolted onto an AI application after it has been built.
>
> If the system does not expose the evidence needed to reconstruct what happened, important questions become impossible to answer.
>
> Evaluability has to be designed into the system.
>
> As AI systems become more autonomous and gain access to tools, workflows, data, and consequential actions, the distance between a technical error and a real-world consequence is shrinking.
>
> The next generation of evals therefore cannot stop at asking:
>
> “Did the model get the answer right?”
>
> They must also ask:
>
> Where can the system fail? Can that failure reach the real world? What happens if it does, and did we build enough evidence into the system to know?

</details>

**我的收获**

- 一个 13 个维度的框架，可以当 rubric 用。

<details><summary>slide 照片（13 张）</summary>

<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/01.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/02.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/03.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/04.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/05.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/06.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/07.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/08.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/09.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/10.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/11.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/12.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/13.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/13.jpg" width="260" alt="slide"></a>

</details>

### Proving Alpha: Showcasing AI's Value in Fundamental Investing

**讲者：** Hari Kumar (Head of AI and Data Science, Contour Asset Management)  
**时间 / 地点：** 1:40pm–1:58pm · Turing Room (Riverside Ballroom) · Breakouts  
**链接：** **[📰 中文版](sessions/zh/10-13-1340-proving-alpha-showcasing-ais-value-in-fu.md)** · **[📰 Full write-up](sessions/10-13-1340-proving-alpha-showcasing-ais-value-in-fu.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=ae273f0b-db0f-4638-8ae3-05874bebf9ca) · [▶ 视频](https://www.youtube.com/watch?v=ZMZlAn6Pyfg) · [精确版](sessions/precise/10-13-1340-proving-alpha-showcasing-ais-value-in-fu.md)

**讲了什么。** Contour Asset Management 是一只 60 亿美元（$6B）的基本面多空基金，讨论 AI 投入到底带来什么回报。它对分析师的研究做在线 A/B 测试，比较用与不用 AI 工具，追踪「每条有用洞见的成本」，在市场让旧想法重新变得有意思时把它们翻出来，并把这套方法延伸到交易、运营和合规。（视频改名为「Proving Alpha: How a Hedge Fund Measures AI’s ROI」。）

<details><summary>官方介绍（英文原文）</summary>

> Every investment firm should have AI in their budget. However, far fewer can say what it returned, especially when it comes to a traditional, fundamental equity investing strategy. We are going to showcase the methods we've built to prove how agent workflows measurably contribute to performance when returns are made up of so many different factors.
>
> In a long/short hedge fund, the AI workflows we've developed sit directly in the investment process, rather than alongside it. Collaborating with our investors & researchers to develop tooling together has led to high-value products that get used daily, including those for new idea generation, overnight news synthesis, and earnings preview. These workflows greatly improve the research process, and we are now able to trace their impact back to the book.
>
> We'll discuss how every idea that a workflow produces is logged at the moment of generation, along with its source and reasoning, so it can be scored for utility. From there, we have built systems to track how many ideas reach our PM to become positions, and how those positions perform against given benchmarks. For softer AI-native signals, we grade against the traditional way that same metric would have been produced and measure both time & signal improvements. This has made great strides to solve one of the biggest challenges in AI implementation: understanding the ROI and alpha generated by every tool and decision made via these systems.
>
> We'll walk through this measurement and attribution process, both from a technical lens as well as a product framework. Ranging from the successes to pitfalls, this talk will have you energized and prepared to start measuring the impact of AI in your investment decisions.

</details>

**我的收获**

- 难点：收益受很多因素影响，ROI 很难看清。该问的问题是「你考虑了哪些以前考虑不到的东西？」：新颖的想法、差异化的信息源，以及真正付诸行动的想法。
- 衡量每个标的的覆盖度和耗时，对比 AI 前后；做在线 A/B 比较（例如一个三个月里不在任何 AI 工作流中的标的）。
- agent 可以跟踪笔记；专家访谈和信用卡面板数据是输入的例子。

<details><summary>slide 照片（8 张）</summary>

<a href="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/01.jpg"><img src="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/02.jpg"><img src="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/03.jpg"><img src="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/04.jpg"><img src="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/05.jpg"><img src="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/06.jpg"><img src="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/07.jpg"><img src="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/08.jpg"><img src="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/08.jpg" width="260" alt="slide"></a>

</details>

### Can AI Understand a Distressed Credit?

**讲者：** Harneet Kaur (Founder, Axim Intelligence)  
**时间 / 地点：** 2:05pm–2:23pm · Turing Room (Riverside Ballroom) · Breakouts  
**链接：** **[📰 中文版](sessions/zh/10-13-1405-can-ai-understand-a-distressed-credit.md)** · **[📰 Full write-up](sessions/10-13-1405-can-ai-understand-a-distressed-credit.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=adca6e26-1ddc-4f77-8e35-59bf1d1de81d) · [▶ 视频](https://www.youtube.com/watch?v=WOszaC1WWbQ) · [精确版](sessions/precise/10-13-1405-can-ai-understand-a-distressed-credit.md)

**讲了什么。** 测试 frontier 模型能否对一个正在发生的困境案例（iHeartMedia）做推理。两个顶级模型在破产还是协商重组上分歧很大；把分析拆成逐层核查的几步（提取、推导、判断、预测）后，两者得出了相同的答案，这又引出一个问题：一致是不是就等于正确。

<details><summary>官方介绍（英文原文）</summary>

> Distressed credit and restructuring is one of the most specialized areas of finance. The data is messy, definitions are unclear, and even extracting a single data point can require judgment. In bankruptcy, simple facts (i.e. cash on balance sheet, debt outstanding) can be disputed and depend on context or legal interpretation.
>
> This talk explores what it takes to build AI for a complex, regulated, and judgment-heavy domain. I’ll share my start up (Axim's) approach to "analytical provenance" and "DistressedBench," a new eval framework designed specifically around the reasoning tasks required in distressed investing.

</details>

**我的收获**

- 预测压力或困境没有 ground truth，所以验证是悬而未决的问题。

<details><summary>slide 照片（8 张）</summary>

<a href="slides/10-13-1405-can-ai-understand-a-distressed-credit/01.jpg"><img src="slides/10-13-1405-can-ai-understand-a-distressed-credit/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1405-can-ai-understand-a-distressed-credit/02.jpg"><img src="slides/10-13-1405-can-ai-understand-a-distressed-credit/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1405-can-ai-understand-a-distressed-credit/03.jpg"><img src="slides/10-13-1405-can-ai-understand-a-distressed-credit/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1405-can-ai-understand-a-distressed-credit/04.jpg"><img src="slides/10-13-1405-can-ai-understand-a-distressed-credit/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1405-can-ai-understand-a-distressed-credit/05.jpg"><img src="slides/10-13-1405-can-ai-understand-a-distressed-credit/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1405-can-ai-understand-a-distressed-credit/06.jpg"><img src="slides/10-13-1405-can-ai-understand-a-distressed-credit/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1405-can-ai-understand-a-distressed-credit/07.jpg"><img src="slides/10-13-1405-can-ai-understand-a-distressed-credit/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1405-can-ai-understand-a-distressed-credit/08.jpg"><img src="slides/10-13-1405-can-ai-understand-a-distressed-credit/08.jpg" width="260" alt="slide"></a>

</details>

### Why Frontier Models Fail on Billion Dollar Portfolios

**讲者：** Chanyeol Choi (Co-founder & CEO, LinqAlpha); Janie Gu (Group Product Manager (Kaggle Benchmarks), Google DeepMind); Jack Haverty (Head of Product, LinqAlpha); Suyeol Yun (Head of AI, LinqAlpha)  
**时间 / 地点：** 2:30pm–2:48pm · Turing Room (Riverside Ballroom) · Breakouts  
**链接：** **[📰 中文版](sessions/zh/10-13-1430-why-frontier-models-fail-on-billion-doll.md)** · **[📰 Full write-up](sessions/10-13-1430-why-frontier-models-fail-on-billion-doll.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=e8e53365-3c1f-4c28-bdd0-44c305a05d57) · [▶ 视频](https://www.youtube.com/watch?v=aVbRSIQO2dE) · [精确版](sessions/precise/10-13-1430-why-frontier-models-fail-on-billion-doll.md)

**讲了什么。** Janie Gu（Google DeepMind，Kaggle Benchmarks）解释为什么大多数 AI benchmark 是坏的：不反映真实工作、保持更新成本高、分数无法验证；Kaggle 复现了 65 个行业 benchmark，发现五种反复出现的失败模式。Suyeol Yun（LinqAlpha）介绍与 Google DeepMind 和 Kaggle 共同开发的 Mandate-Bench，评估模型能否像买方分析师一样，从基金授权一路做到投资备忘录：13 个行业共 50 个任务，在其中 10 个上的初步结果里，最好的平均分是 58/100（Claude Code 里的 Claude Opus 5.5）。完整任务集将于 2026 年 11 月在 Kaggle 上发布。（视频改名为「Why Frontier Models Fail as Buy-Side Analysts」；摘要里「>60% from the right harness」这个数字在演讲中没有出现。）

<details><summary>官方介绍（英文原文）</summary>

> Your harness matters as much as your model when it comes to institutional buy-side research. In our benchmarks, moving one model to the right harness improved its score by more than 60%, as much as switching models entirely. Off-the-shelf agents fail because they miss hidden accounting traps, trust stale summary tables, and burn their token budget in degenerate search loops.
>
> We’re launching Mandate-Bench, a benchmark built by LinqAlpha with Google DeepMind’s Kaggle Benchmarks team to test real investment origination: the work an analyst does before a portfolio manager ever sizes a position. We evaluate agents on licensed point-in-time market data across 1,300 US stocks and nearly 5,000 SEC filings, where only 15.8% of frontier model runs meet the bar of an institutional portfolio manager.
>
> In this talk, we will walk through our benchmark findings and break down lessons learned from shipping agents in production to real hedge fund clients with billions in AUM.

</details>

**我的收获**

- 如何搭建和展示一个领域 benchmark 的好范本。

<details><summary>slide 照片（28 张）</summary>

<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/01.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/02.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/03.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/04.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/05.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/06.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/07.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/08.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/09.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/10.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/11.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/12.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/13.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/14.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/15.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/16.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/17.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/18.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/19.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/19.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/20.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/20.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/21.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/21.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/22.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/22.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/23.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/23.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/24.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/24.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/25.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/25.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/26.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/26.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/27.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/27.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/28.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/28.jpg" width="260" alt="slide"></a>

</details>

### AI and the Future of Fundamental Investing

**讲者：** Jack Woodruff (Senior Advisor, Kepler); Vinoo Ganesh (CEO, Kepler)  
**时间 / 地点：** 2:55pm–3:13pm · Simons Room (Metropolitan East) · Stage Talk (18 mins)  
**链接：** **[📰 中文版](sessions/zh/10-13-1455-ai-and-the-future-of-fundamental-investi.md)** · **[📰 Full write-up](sessions/10-13-1455-ai-and-the-future-of-fundamental-investi.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=a244839c-239c-4789-b621-32a3568a218e) · [▶ 直播 @ 5:29:04](https://www.youtube.com/watch?v=akEqiXFo3GM&t=19744s) · [Vinoo Ganesh 的相关演讲：「Do Not Let Models Invent Numbers」](https://www.youtube.com/watch?v=dZgA6btnb_s) · [讲者问答（DEV）](https://dev.to/dailycontext/keplers-vinoo-ganesh-on-building-ai-you-can-trust-3p0f) · [精确版](sessions/precise/10-13-1455-ai-and-the-future-of-fundamental-investi.md)

**讲了什么。** Jack Woodruff（Kepler；曾在 Citadel 管理最大的投资组合之一）与 Vinoo Ganesh（Kepler CEO）的炉边对谈：AI 正在改变基本面投资的哪些方面，在哪里真正省时间，以及哪些事投资人永远不会交给模型。

<details><summary>官方介绍（英文原文）</summary>

> Jack Woodruff ran one of the largest books at Citadel and went on to found Candlestick Capital. Today he's the buyer every finance AI company wants to win, and in this fireside chat with Vinoo Ganesh, CEO of Kepler and former Head of Business Engineering at Citadel, he'll speak candidly about what AI is changing in fundamental investing and what it isn't. Jack will share where AI is saving real time in his process, what he'd never hand over to a model, and why the judgment call still belongs to the investor. They'll also take on the questions most people only ask in private. Will the biggest funds win everything? If everyone has the same models, where does alpha come from? And when does a number have to be right, not just plausible?

</details>

### Your agent fix just broke something else

**讲者：** Sofia Jakovcevic (AI Forward Deployed Engineer, Arize AI)  
**时间 / 地点：** 3:20pm–3:38pm · Turing Room (Riverside Ballroom) · Breakouts  
**链接：** **[📰 中文版](sessions/zh/10-13-1520-your-agent-fix-just-broke-something-else.md)** · **[📰 Full write-up](sessions/10-13-1520-your-agent-fix-just-broke-something-else.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=2a589e49-4ee4-4c53-bfeb-40bc3db16506) · [▶ 视频](https://www.youtube.com/watch?v=v5tm5EAzsAs) · [精确版](sessions/precise/10-13-1520-your-agent-fix-just-broke-something-else.md)

**讲了什么。** 一行 prompt 改动让一个客服 agent 安全性提高 17 个百分点，任务完成率却下降 17 个百分点。agent 在运行时出问题，而不是在代码里；这场分享讲了一个基于生产 trace 搭建的五步回归循环（数据集、评估器、基线实验、对比、CI 关卡），最后讲到会读 trace 并自我修复的 agent。

<details><summary>官方介绍（英文原文）</summary>

> Agent behavior emerges at runtime. So why are we still debugging only at the code level? A prompt edit, model swap, new skill, or tool change can fix exactly what you were looking at while quietly breaking something you weren't. In one example, a one-line policy fix raised an agent's action-safety score while dropping task completion from 0.89 to 0.72. The aggregate outcome looked fine. The regression was hiding underneath it. This session is a hands-on walkthrough of how to turn your agent observability into a continuous regression testing loop. We'll build a regression dataset from real production traces, run a candidate change against it as a full-agent experiment, diagnose competing metrics, and put the workflow into CI so every change is evaluated before it ships. We'll automate the full feedback loop from observe → evaluate → change → test → improve. Observability is becoming more than humans understanding agents. It's becoming the infrastructure agents need to understand and improve themselves.

</details>

**我的收获**

- 汇总指标会掩盖回归；代码改动是局部的，行为改动不是。

<details><summary>slide 照片（15 张）</summary>

<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/01.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/02.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/03.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/04.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/05.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/06.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/07.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/08.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/09.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/10.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/11.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/12.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/13.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/14.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/15.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/15.jpg" width="260" alt="slide"></a>

</details>

### Give the Model a Computer: The One Thing Every AI Agent Framework Agrees On

**讲者：** Mislav Ivanda  
**时间 / 地点：** 3:45pm–4:03pm · Sutton Room (NY Ballroom W) · Expo Session (18 mins)  
**链接：** **[📰 中文版](sessions/zh/10-13-1545-give-the-model-a-computer-the-one-thing.md)** · **[📰 Full write-up](sessions/10-13-1545-give-the-model-a-computer-the-one-thing.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=8d424512-c7f0-483c-9094-ca899d7bb1fd) · [精确版](sessions/precise/10-13-1545-give-the-model-a-computer-the-one-thing.md)  
**录像：** 尚未发布

**讲了什么。** 一年里为 LangChain、CrewAI、Google ADK、Mastra、Pydantic AI 和 OpenAI Agents SDK 做 Daytona 集成的经验表明：这些在别的事上毫无共识的框架，都收敛到同一个原语：给模型一台电脑（一个 sandbox）。

<details><summary>官方介绍（英文原文）</summary>

> Building an agent is easy. Deciding where its code runs is the part that follows you to production.
>
> This talk comes from a year of building Daytona integrations across the agent ecosystem: LangChain, CrewAI, Google ADK, Mastra, Pydantic AI, the OpenAI Agents SDK, and a long tail behind them. Integration after integration, the same pattern kept surfacing. Frameworks that agree on nothing, not even the language, not the mental model, not who controls the flow, all converge on one identical primitive: give the model a computer. We'll trace why.
>
> Every framework ends up facing the same question: where does the model's code actually run? Only three answers exist, and two of them stop holding once real users show up. That's why the past year went the way it did, with frameworks deleting their own code interpreters rather than defending defaults they couldn't. And that's why the computer keeps growing: first a tool call, then the place the agent lives, then a thousand machines at once.
>
> You'll leave with one rule: pick the boundary before the framework. Frameworks are opinions. The boundary is a commitment.

</details>

**我的收获**

- 有一张幻灯片按谁控制流程（AI 决定还是人编排）比较各框架，特别有启发。

<details><summary>slide 照片（17 张）</summary>

<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/01.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/02.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/03.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/04.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/05.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/06.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/07.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/08.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/09.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/10.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/11.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/12.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/13.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/14.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/15.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/16.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/17.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/17.jpg" width="260" alt="slide"></a>

</details>

### A Hedge Fund as a Function

**讲者：** Frank Long (Head of AI, Coatue)  
**时间 / 地点：** 4:30pm–4:48pm · Simons Room (Metropolitan East) · Featured Keynote  
**链接：** **[📰 中文版](sessions/zh/10-13-1630-a-hedge-fund-as-a-function.md)** · **[📰 Full write-up](sessions/10-13-1630-a-hedge-fund-as-a-function.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=9a9dfcd0-459f-43eb-b3f8-004864b4ad2a) · [▶ 直播 @ 6:47:46](https://www.youtube.com/watch?v=akEqiXFo3GM&t=24466s) · [精确版](sessions/precise/10-13-1630-a-hedge-fund-as-a-function.md)

**讲了什么。** Coatue 的 AI 负责人谈把 AI 用于投资的思考框架。

<details><summary>官方介绍（英文原文）</summary>

> Frameworks for Thinking about Applying AI to Investing

</details>

<details><summary>slide 照片（21 张）</summary>

<a href="slides/10-13-1630-a-hedge-fund-as-a-function/01.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/02.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/03.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/04.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/05.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/06.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/07.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/08.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/09.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/10.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/11.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/12.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/13.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/14.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/15.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/16.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/17.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/18.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/19.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/19.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/20.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/20.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/21.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/21.jpg" width="260" alt="slide"></a>

</details>

### Frontier Models for Acting in Space and Time

**讲者：** Pim de Witte (Co-Founder & CEO, General Intuition)  
**时间 / 地点：** 4:48pm–5:06pm · Simons Room (Metropolitan East) · Featured Keynote  
**链接：** **[📰 中文版](sessions/zh/10-13-1648-frontier-models-for-acting-in-space-and.md)** · **[📰 Full write-up](sessions/10-13-1648-frontier-models-for-acting-in-space-and.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=cada670e-c5ea-4977-b3c2-9a68dda26abb) · [▶ 直播 @ 7:02:19](https://www.youtube.com/watch?v=akEqiXFo3GM&t=25339s) · [精确版](sessions/precise/10-13-1648-frontier-models-for-acting-in-space-and.md)

**讲了什么。** Pim de Witte 介绍 General Intuition：一家打造在空间和时间中行动的模型的 frontier 实验室（从 Khosla 和 General Catalyst 融资 6.5 亿美元以上，$650M+）。

<details><summary>官方介绍（英文原文）</summary>

> Pim de Witte is the co-founder and CEO of General Intuition, the frontier lab for acting in space and time. He previously founded Medal, the world’s largest and fastest-growing platform for gaming clips, where millions of gamers share their best moments. Before gaming and AI, he worked with Médecins Sans Frontières and ran the largest Runescape private server at 14. General Intuition recently raised $650M+ from Khosla and General Catalyst.

</details>

<details><summary>slide 照片（2 张）</summary>

<a href="slides/10-13-1648-frontier-models-for-acting-in-space-and/01.jpg"><img src="slides/10-13-1648-frontier-models-for-acting-in-space-and/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1648-frontier-models-for-acting-in-space-and/02.jpg"><img src="slides/10-13-1648-frontier-models-for-acting-in-space-and/02.jpg" width="260" alt="slide"></a>

</details>

### Hermes gives you wings: Build on open foundations.

**讲者：** Dillon Rolnick (CEO, Nous Research)  
**时间 / 地点：** 5:06pm–5:24pm · Simons Room (Metropolitan East) · Featured Keynote  
**链接：** **[📰 中文版](sessions/zh/10-13-1706-hermes-gives-you-wings-build-on-open-fou.md)** · **[📰 Full write-up](sessions/10-13-1706-hermes-gives-you-wings-build-on-open-fou.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=871cd5c8-13f4-4fe9-ac49-c8b25d970add) · [▶ 直播 @ 7:21:20](https://www.youtube.com/watch?v=akEqiXFo3GM&t=26480s) · [精确版](sessions/precise/10-13-1706-hermes-gives-you-wings-build-on-open-fou.md)

**讲了什么。** Nous Research 的 CEO 主张：现代企业需要建立在开放基础（Hermes）之上的开放 agent。

<details><summary>官方介绍（英文原文）</summary>

> Modern businesses require open agents.

</details>

<details><summary>slide 照片（4 张）</summary>

<a href="slides/10-13-1706-hermes-gives-you-wings-build-on-open-fou/01.jpg"><img src="slides/10-13-1706-hermes-gives-you-wings-build-on-open-fou/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1706-hermes-gives-you-wings-build-on-open-fou/02.jpg"><img src="slides/10-13-1706-hermes-gives-you-wings-build-on-open-fou/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1706-hermes-gives-you-wings-build-on-open-fou/03.jpg"><img src="slides/10-13-1706-hermes-gives-you-wings-build-on-open-fou/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1706-hermes-gives-you-wings-build-on-open-fou/04.jpg"><img src="slides/10-13-1706-hermes-gives-you-wings-build-on-open-fou/04.jpg" width="260" alt="slide"></a>

</details>

## 10 月 14 日（周三）

### The Hidden Costs of a Faster Model

**讲者：** Morgan Suo (Head of US Business Development, MiniMax)  
**时间 / 地点：** 10:40am–10:58am · McCarthy Room (Riverside Suite) · Breakout Session (18 mins)  
**链接：** **[📰 中文版](sessions/zh/10-14-1040-the-hidden-costs-of-a-faster-model.md)** · **[📰 Full write-up](sessions/10-14-1040-the-hidden-costs-of-a-faster-model.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=f3445fe7-308b-4e84-8490-fd78a13db4fb) · [▶ 视频](https://www.youtube.com/watch?v=YvQ4BVTWfHI) · [精确版](sessions/precise/10-14-1040-the-hidden-costs-of-a-faster-model.md)

**讲了什么。** 更快的模型只有在整个任务完成得更快时才有用。用一个研究任务来测试 MiniMax 的 M3.1 Flash 内部的各项改动（稀疏注意力、4-bit KV cache、低精度 expert、投机解码、推理预算），每一项都省算力，也都可能悄悄赔上准确率或等待时间。（视频改名为「Is a Faster Model Actually Faster? Test the Whole Job」。）

<details><summary>官方介绍（英文原文）</summary>

> Model upgrades are often sold as more capability per dollar. In production, the harder question is what quietly changed underneath: attention sparsity, 4-bit quantization, speculative decoding, or how we prompt and template the model itself. This talk is a practical tour of those changes and the tests they require. Using a recent model release as a concrete case study, we’ll examine how blockwise 4-bit attention and MoE quantization change scale selection and rounding; how speculative decoding changes serving assumptions; and how reasoning controls can alter the path from API request to prompt construction. Attendees will leave with a good understanding of the hidden costs of change that affect latency, output quality, reproducibility, observability, and rollback for teams shipping AI in latency-sensitive and high-stakes environments.

</details>

**我的收获**

- 信任更快的模型之前先验证输出。

<details><summary>slide 照片（13 张）</summary>

<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/01.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/02.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/03.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/04.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/05.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/06.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/07.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/08.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/09.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/10.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/11.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/12.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/13.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/13.jpg" width="260" alt="slide"></a>

</details>

### Building Better Agents Through End-to-End Evals

**讲者：** Ryan Stevens (Director of Applied Science, Ramp)  
**时间 / 地点：** 11:05am–11:23am · Shannon Room (NY Ballroom E) · Breakout Session (18 mins)  
**链接：** **[📰 中文版](sessions/zh/10-14-1105-building-better-agents-through-end-to-en.md)** · **[📰 Full write-up](sessions/10-14-1105-building-better-agents-through-end-to-en.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=4607dbbe-8f94-4b0e-91ce-257d1cfb5448) · [精确版](sessions/precise/10-14-1105-building-better-agents-through-end-to-en.md)  
**录像：** 尚未发布

**讲了什么。** Ramp 用真实的会计工作流搭了一个 benchmark，端到端测试 frontier 模型、工具、skill、记忆和 harness。一些直觉上的改进反而让系统变差；skill 消融实验找出了拖累性能的 context。

<details><summary>官方介绍（英文原文）</summary>

> More context should make an AI agent better. In our accounting agent, it often did the opposite. At Ramp, we built a benchmark of real-world accounting workflows to test frontier models, tools, skills, memory, and the agent harness end to end. What we found was that some of the most intuitive improvements actually made the system worse. In this talk, I’ll walk through what we learned building and benchmarking a production finance agent: how we used skill ablation to identify context that was hurting performance, reduced one frequently loaded skill from 14k to 5k characters without sacrificing quality, and learned the hard way that agent memory needs to preserve reusable process, not stale facts from the last task. The broader lesson is that production agents are systems problems, not prompt problems. Better models help, but tool design, context management, telemetry, evals, latency, and cost all interact. I’ll share the metrics and development loop we use to decide whether an agent change actually makes the product better. Attendees will leave with a practical framework for evaluating agent-harness changes against quality, reliability, latency, and cost rather than optimizing any one of them in isolation.

</details>

**我的收获**

- 度量从 agent 的目标出发。例子是关账：与 ERP 对账。benchmark 需要干净的数据来做模拟。
- 标注 → benchmark → eval → 可验证的输出，基本上就是对一个生产 agent 做回测。
- Pass@k（pass@1、2、3）是报告可靠性的好方法。
- 在会计里，找到信息或文件并不难；出错的是推理。
- GEPA（根据失败自动调优 skill）值得深入看看。

<details><summary>slide 照片（19 张）</summary>

<a href="slides/10-14-1105-building-better-agents-through-end-to-en/01.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/02.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/03.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/04.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/05.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/06.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/07.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/08.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/09.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/10.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/11.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/12.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/13.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/14.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/15.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/16.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/17.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/18.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/19.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/19.jpg" width="260" alt="slide"></a>

</details>

### We Let Our Financial Planners Write the Agents

**讲者：** Matt Ragone (Financial Planner, Range)  
**时间 / 地点：** 11:30am–11:48am · Turing Room (Riverside Ballroom) · Breakouts  
**链接：** **[📰 中文版](sessions/zh/10-14-1130-we-let-our-financial-planners-write-the.md)** · **[📰 Full write-up](sessions/10-14-1130-we-let-our-financial-planners-write-the.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=9b002687-09cf-42e2-9282-532ed9d0c184) · [▶ 视频](https://www.youtube.com/watch?v=gpmze-6ueGU) · [精确版](sessions/precise/10-14-1130-we-let-our-financial-planners-write-the.md)

**讲了什么。** Range 的理财规划师自己编写和维护每天用的 agent skill，不写代码：六个月后全公司有 24 个 skill，其中 17 个在规划部门。每条建议到达客户之前都由规划师审核。

<details><summary>官方介绍（英文原文）</summary>

> We show how our CFPs built organizational skills to make planning more efficient and effective. Our planners write and maintain the agent skills they use daily, and none of them write code.
>
> We started by using AI as a chat box to polish client messages. The real shift came when work began happening in the background: I can be on a call with one client while a full cash flow or insurance analysis is drafted for another, then review, adjust, and send recommendations afterward. A planner’s bottleneck was never typing speed, but being in only one place at a time.
>
> That requires an agent that works where the work lives and knows how we do planning, not how planning works in general. That’s a skill. To build one: pick a repeatable workflow, walk the agent through one real case while narrating your judgment, package it with the questions it must ask and the limits it must respect, test it on five to ten real cases, then ship it with an owner and a reviewer.
>
> Six months in, we have 24 skills (17 in planning), and adoption spread to sales, product, and marketing without a mandate. I’ll cover what changed when subject matter experts built for their own niches, how we peer-review skills like we review plans, and our open question: how to check quality at scale when the output is judgment.

</details>

**我的收获**

- 采用 AI 不需要工程背景；例子是现金流和保险分析。

<details><summary>slide 照片（13 张）</summary>

<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/01.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/02.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/03.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/04.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/05.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/06.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/07.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/08.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/09.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/10.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/11.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/12.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/13.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/13.jpg" width="260" alt="slide"></a>

</details>

### The Model Ate the Coding Harness. Now What?

**讲者：** Jediah Katz (Engineer, Agent Harness, SpaceX Super Intelligence)  
**时间 / 地点：** 11:55am–12:13pm · Shannon Room (NY Ballroom E) · Breakout Session (18 mins) · ⭐ 标星  
**链接：** **[📰 中文版](sessions/zh/10-14-1155-the-model-ate-the-coding-harness-now-wha.md)** · **[📰 Full write-up](sessions/10-14-1155-the-model-ate-the-coding-harness-now-wha.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=29083884-87dc-44f8-b412-b494afca8856) · [▶ 视频](https://www.youtube.com/watch?v=7Y4sbZgBdnc) · [精确版](sessions/precise/10-14-1155-the-model-ate-the-coding-harness-now-wha.md)

**讲了什么。** Jediah Katz（SpaceXAI；此前是 Cursor 的 agent-harness 工程师）谈更聪明的模型对编程 agent、Grok Bot 和 harness 的未来意味着什么。花哨的 harness 不再给编程能力加分，因为数据飞轮把过去靠打补丁补上的技能教给了模型；harness 更重要的是模型吸收不了的部分：基础设施、信任与安全、效率。（视频改名为「Is the Agent Harness Dead?」。）

<details><summary>官方介绍（英文原文）</summary>

> Jediah Katz discusses how we built Cursor and Grok Bot, and where we think the agent harness is going.

</details>

**我的收获**

- harness 启动数据飞轮，解锁软件工厂。
- Grok Bot 的例子：网站的关键流程、沟通、自己动手还是先问、随时间学习。
- 权限和 API 费用仍然是 harness 要管的事。

<details><summary>slide 照片（23 张）</summary>

<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/01.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/02.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/03.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/04.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/05.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/06.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/07.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/08.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/09.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/10.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/11.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/12.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/13.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/14.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/15.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/16.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/17.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/18.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/19.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/19.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/20.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/20.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/21.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/21.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/22.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/22.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/23.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/23.jpg" width="260" alt="slide"></a>

</details>

### A/B Testing AI Products Is Weird: What Changes When the Product Is Probabilistic

**讲者：** Supriya Subudhi (Staff Product Manager, Bumble)  
**时间 / 地点：** 12:20pm–12:38pm · Turing Room (Riverside Ballroom) · Breakouts  
**链接：** **[📰 中文版](sessions/zh/10-14-1220-ab-testing-ai-products-is-weird-what-cha.md)** · **[📰 Full write-up](sessions/10-14-1220-ab-testing-ai-products-is-weird-what-cha.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=11fef189-97e8-4c8e-87e8-dac43d800846) · [讲者文章（DEV）](https://dev.to/dailycontext/ab-testing-ai-products-is-weird-576h) · [精确版](sessions/precise/10-14-1220-ab-testing-ai-products-is-weird-what-cha.md)  
**录像：** 尚未发布

**讲了什么。** 传统实验假设你能定义一个功能、度量反应、比较结果。AI 的输出会变，用户的反应取决于情境，指标赢了也说不出体验为什么变好。内容来自消费科技和金融科技的实验经验，包括 Bumble 的一个 AI 驱动体验。

<details><summary>官方介绍（英文原文）</summary>

> Traditional product experimentation assumes that we can define a feature, measure a response, and compare outcomes. AI products make that much harder. Outputs can vary, user responses can depend on context, and a metric improvement does not necessarily tell you why the experience improved.
>
> Drawing on experimentation across consumer technology and fintech, as well as the development of an AI-powered product experience at Bumble, I will examine what product teams need to consider when evaluating AI-powered experiences.
>
> We will look at common experimentation failure modes including metric selection, novelty effects, premature conclusions, and misleading behavioural signals, then connect them to the additional complexity introduced when the product experience itself is generated or personalised. I will share a practical approach for defining the product question first, selecting meaningful outcomes, interpreting experiment results, and deciding when an AI feature is ready to scale.

</details>

**我的收获**

- A/B 测试成功只是起点。五个陷阱：(1) 用户要了，然后没用；(2) 平均值的胜利掩盖了长尾；(3) 仪表盘一片绿，账单一片红；(4) 换个模型结果就翻转；(5) AI 监管不断变化。
- 按 token 计价是「仪表盘一片绿，账单一片红」这种矛盾的根源；按时间计费、配足够的吞吐量也许更合适。

<details><summary>slide 照片（8 张）</summary>

<a href="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/01.jpg"><img src="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/02.jpg"><img src="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/03.jpg"><img src="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/04.jpg"><img src="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/05.jpg"><img src="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/06.jpg"><img src="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/07.jpg"><img src="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/08.jpg"><img src="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/08.jpg" width="260" alt="slide"></a>

</details>

### Building a self-improving software factory

**讲者：** Zach Lloyd (Founder & CEO, Warp)  
**时间 / 地点：** 1:40pm–1:58pm · McCarthy Room (Riverside Suite) · Expo Session (18 mins) · ⭐ 标星  
**链接：** **[📰 中文版](sessions/zh/10-14-1340-building-a-self-improving-software-facto.md)** · **[📰 Full write-up](sessions/10-14-1340-building-a-self-improving-software-facto.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=a901d5ea-57c6-433f-9c37-5fe6531adb99) · [精确版](sessions/precise/10-14-1340-building-a-self-improving-software-facto.md)  
**录像：** 尚未发布

**讲了什么。** Warp 认为交互式 agent 无法规模化（成本失控、缺乏治理），团队需要的是软件工厂：自动化开发的闭环系统，基础设施共享，而不是每家公司各造一套。

<details><summary>官方介绍（英文原文）</summary>

> Interactive agents aren’t scaling. Every company is coping with runaway costs, lack of governance, and difficulty automating.
>
> This has led to the need for software factories: a closed-loop system for automating development, with primitives that provably improve throughput and efficiency over time.
>
> Right now, folks are trying to roll their own internal software factories from bare metal, solving the same problems 100 different ways. We need shared, flexible infrastructure that everyone can build upon.
>
> In this talk, we’ll share the core building blocks every company needs to build their own self-improving software factories based on our own experience building Warp Factories:
>
> 1. Cloud infrastructure to centralize agent conversations, skills, and security permissions
>
> 2. Configuration as code for humans and agents to manage that cloud infrastructure
>
> 3. Metrics for humans and agents to track efficiency and code quality, beyond “number of PRs shipped”
>
> 4. Self-improvement loops for agents to review past conversations, score the quality, and propose improvements to the skills driving the factory
>
> You’ll walk away with a clear understanding of how to improve coding agent ROI, and how to deploy a system of your own using Warp Factories.
>
> This session will include both slides and a live demonstration of:
>
> - How to run tasks through a software factory
>
> - How "self-improvement" works, watching agents review past conversations to suggest improvements to skills in a code repository
>
> This will include info about Warp Factories, but is meant to be useful to any team learning about software factories or building their own.

</details>

**我的收获**

- 本地 agent 搬进 sandbox，才能被治理；闭环从一个产品想法开始。
- 工厂基础设施：数据、算力和推理网关（任意模型、任意 harness）。
- 用度量代替感觉：用 benchmark 和分数驱动自我改进。开放问题：数据整理、如何定义正确性、LLM-as-judge。

<details><summary>slide 照片（19 张）</summary>

<a href="slides/10-14-1340-building-a-self-improving-software-facto/01.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/02.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/03.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/04.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/05.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/06.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/07.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/08.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/09.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/10.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/11.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/12.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/13.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/14.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/15.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/16.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/17.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/18.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/19.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/19.jpg" width="260" alt="slide"></a>

</details>

### Scaling Agentic RL Across the Globe

**讲者：** Jason Mancuso (Member of Technical Staff, Research, Modal)  
**时间 / 地点：** 2:05pm–2:23pm · Shannon Room (NY Ballroom E) · Expo Session (18 mins) · ⭐ 标星  
**链接：** **[📰 中文版](sessions/zh/10-14-1405-scaling-agentic-rl-across-the-globe.md)** · **[📰 Full write-up](sessions/10-14-1405-scaling-agentic-rl-across-the-globe.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=e6ccda8d-0613-44f8-b98f-53ba8037b518) · [Stitch（开源 RL 框架）](https://github.com/modal-projects/stitch) · [精确版](sessions/precise/10-14-1405-scaling-agentic-rl-across-the-globe.md)  
**录像：** 尚未发布

**讲了什么。** Modal 支持客户在多云、多地区的集群上扩展 agent RL，并介绍 Stitch：一个开源的、把 RL 训练拆分解耦的框架。

<details><summary>官方介绍（英文原文）</summary>

> The industry is rushing to integrate agents into every surface of every product and service. To do so sustainably, keeping cost, quality, performance, and reliability in check, most organizations have turned to posttraining and deploying open-source models. Reinforcement learning is the most critical component of the posttraining stack. At Modal, we've needed to support our customers in scaling up agent RL across our multi-cloud, multi-region fleet. We'll present Stitch, our open-source framework for disaggregating RL rollouts across clouds, regions, and heterogenous GPU types, as well as a stable recipe for posttraining agentic models on off-policy rollout data.

</details>

**我的收获**

- 为什么要 fine-tune：服务质量、专有数据格式，以及在每美元的任务智能上竞争。
- RL 比 SFT 泛化得更好。有了 agent harness、eval 和生产 trace，就能迭代。
- 配方：GRPO 加 masked importance sampling 加 score centering，以限制漂移。

<details><summary>slide 照片（24 张）</summary>

<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/01.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/02.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/03.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/04.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/05.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/06.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/07.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/08.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/09.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/10.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/11.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/12.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/13.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/14.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/15.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/16.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/17.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/18.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/19.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/19.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/20.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/20.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/21.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/21.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/22.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/22.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/23.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/23.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/24.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/24.jpg" width="260" alt="slide"></a>

</details>

### Your agent needs a skill to improve its skills

**讲者：** Ben Holmes (DevEx Lead, Warp)  
**时间 / 地点：** 2:30pm–2:48pm · Turing Room (Riverside Ballroom) · Breakouts · ⭐ 标星  
**链接：** **[📰 中文版](sessions/zh/10-14-1430-your-agent-needs-a-skill-to-improve-its.md)** · **[📰 Full write-up](sessions/10-14-1430-your-agent-needs-a-skill-to-improve-its.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=0a0cbd67-23fa-4a3d-a8dc-06e3caa3ab2f) · [精确版](sessions/precise/10-14-1430-your-agent-needs-a-skill-to-improve-its.md)  
**录像：** 尚未发布

**讲了什么。** Warp 发现自己的编排 agent 一个月里浪费了数百次工具调用，原因是一个消息传递 skill 里的一条错误指令。展示了如何搭一个反馈循环，评估过去的 agent 对话，找出 skill 本身的问题。

<details><summary>官方介绍（英文原文）</summary>

> We just found out our orchestration agent was wasting hundreds of tool calls this past month. The reason? One bad instruction in our message passing skill.
>
> Peeling back the curtain, there are dozens more examples you probably have in your own agent skills too. As you scale these agents to longer-running workloads, those gaps add up to real token cost.
>
> To solve it, you'll need a feedback loop for agentic self-improvement. I'll show you how to build one: a system that evaluates past agent conversations, finds failure points, and turns those findings into skill improvements you'd want to merge. We'll set up the evaluation loop from scratch, and show how to automate it as part of a software factory.
>
> You'll walk away with the skills you need (pun intended) to close the loop and let your agentic system improve itself.

</details>

<details><summary>slide 照片（20 张）</summary>

<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/01.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/02.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/03.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/04.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/05.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/06.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/07.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/08.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/09.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/10.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/11.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/12.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/13.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/14.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/15.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/16.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/17.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/18.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/19.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/19.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/20.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/20.jpg" width="260" alt="slide"></a>

</details>

### Build your own team of managed agents

**讲者：** Jack Wotherspoon (Member of Technical Staff, Google DeepMind)  
**时间 / 地点：** 2:55pm–3:13pm · Shannon Room (NY Ballroom E) · Expo Session (18 mins)  
**链接：** **[📰 中文版](sessions/zh/10-14-1455-build-your-own-team-of-managed-agents.md)** · **[📰 Full write-up](sessions/10-14-1455-build-your-own-team-of-managed-agents.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=970f672e-f351-46bb-9790-78c6b1baf81d) · [▶ 视频](https://www.youtube.com/watch?v=Y_p3y6zP5QE) · [精确版](sessions/precise/10-14-1455-build-your-own-team-of-managed-agents.md)

**讲了什么。** Gemini Managed Agents 用一次 API 调用就能在云端 sandbox 里运行编程 agent：Interactions API、Antigravity harness、多个 agent 共享的环境、上传的 skill 和 AGENTS.md 文件、定时触发器，以及多 agent 工作流。

<details><summary>官方介绍（英文原文）</summary>

> To do real work, an agent needs its own computer where it can research, write files, and execute code. Giving agents their own sandbox means you can stop babysitting your agents and let them run and verify their own work in a secure environment.
>
> This used to be difficult to set up or do on your own but now it is a single API call away. Managed agents give you a cloud sandbox, a file system, and a full agent loop in a single API call. Build an agent for your daily tasks or build one into your product that you can scale to thousands of customers.
>
> Let’s put it to the test by building a full team of financial agents in just a few minutes!

</details>

**我的收获**

- 多个 agent 可以指向同一个 sandbox。
- 出口代理让密钥不进入 sandbox。

<details><summary>slide 照片（19 张）</summary>

<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/01.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/02.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/03.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/04.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/05.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/06.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/07.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/08.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/09.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/10.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/11.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/12.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/13.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/14.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/15.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/16.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/17.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/18.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/19.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/19.jpg" width="260" alt="slide"></a>

</details>

### A Roadmap for Evaluating Highly-Subjective Tasks

**讲者：** Cameron Wolfe (Staff Research Scientist, Netflix)  
**时间 / 地点：** 3:20pm–3:38pm · Turing Room (Riverside Ballroom) · Breakouts  
**链接：** **[📰 中文版](sessions/zh/10-14-1520-a-roadmap-for-evaluating-highly-subjecti.md)** · **[📰 Full write-up](sessions/10-14-1520-a-roadmap-for-evaluating-highly-subjecti.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=f5c310e9-b0bb-4fa6-bc0d-bda758e1c464) · [幻灯片（公开的 Google Slides）](https://docs.google.com/presentation/d/146TscsJ21LU4kATVS3ES2oC_oZxoCBsTH3soepY25P4/edit?slide=id.g3ec03477a51_0_0#slide=id.g3ec03477a51_0_0) · [精确版](sessions/precise/10-14-1520-a-roadmap-for-evaluating-highly-subjecti.md)  
**录像：** 尚未发布

**讲了什么。** 为主观任务搭建大规模自动 eval 的路线图：协调人工标注、构建和评估 LLM 评委、用生产监控为 eval 提供依据，以及用统计方法降低不确定性。

<details><summary>官方介绍（英文原文）</summary>

> This presentation provides a technical roadmap for building large-scale automatic evaluations for subjective tasks. Specific concepts that will be covered include setting up and coordinating human annotation efforts, building and evaluation LLM judges, using production monitoring to inform model evaluations, and reducing evaluation uncertainty via statistics. By combining these components and monitoring them properly, we can create a scalable evaluation framework that evolves and improves over time as our understanding of a domain deepens.

</details>

**我的收获**

- 从人工评估开始：一小组质量标准、清晰的打分方式和具体的例子。离线评估、人工评估和生产监控要配合使用。

<details><summary>slide 照片（1 张）</summary>

<a href="slides/10-14-1520-a-roadmap-for-evaluating-highly-subjecti/01.jpg"><img src="slides/10-14-1520-a-roadmap-for-evaluating-highly-subjecti/01.jpg" width="260" alt="slide"></a>

</details>

### Code Mode: your agent should write code

**讲者：** Mikyo King (Head of Open Source, Arize AI)  
**时间 / 地点：** 3:45pm–4:03pm · Turing Room (Riverside Ballroom) · Breakouts  
**链接：** **[📰 中文版](sessions/zh/10-14-1545-code-mode-your-agent-should-write-code.md)** · **[📰 Full write-up](sessions/10-14-1545-code-mode-your-agent-should-write-code.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=8413d6ee-ceb1-4095-9f27-49d518f1dd5a) · [▶ 视频](https://www.youtube.com/watch?v=dAoOYFx7ZYw) · [精确版](sessions/precise/10-14-1545-code-mode-your-agent-should-write-code.md)

**讲了什么。** 给 agent 40 个工具，它对每个工具都用得更差；给它一个 sandbox，它就能自己组合出需要的东西。讲了 Arize 如何把 code mode 做进 Phoenix（Bash、Python sandbox、SQL，甚至控制 UI），它的隐性成本（密钥、权限、持久性、双重 sandbox），以及 Harbor eval 显示 token 减少 43%、成本降低 31%。（视频改名为「Stop Giving Your Agent Tools. Give It a Sandbox」。）

<details><summary>官方介绍（英文原文）</summary>

> Give an agent 40 tools and it gets worse at using any of them. Give it a sandbox and a way to write code and it can compose whatever it needs. Recent research has bash-only agents beating typed tool catalogs by 21.8 to 24.5 points on the same benchmark while spending fewer tokens, and the industry is quietly moving in that direction. The shift is really one of abstraction. We used to obsess over prompts; now we optimize skills. We used to design individual tools; now we ship MCP servers and plugins. We've stopped handing agents primitive verbs and started handing them programming. This talk makes the case for code mode while staying realistic about what it costs, and closes with how our open-source observability stack exposes agent traces to coding agents through read-only SQL and code mode, and how we use our own evals and tooling to improve agent outcomes.

</details>

**我的收获**

- 工具调用与 code mode 对比：(1) 工具搜索，(2) 可组合性（循环、分支、串联；参见 Claude agent teams），(3) token 效率，(4) 涌现行为。
- 减少工具调用说得通，不过实际做起来更像是把功能打包。

<details><summary>slide 照片（13 张）</summary>

<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/01.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/02.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/03.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/04.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/05.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/06.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/07.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/08.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/09.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/10.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/11.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/12.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/13.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/13.jpg" width="260" alt="slide"></a>

</details>

### AI in Practice: An Inside Look at Apollo's Approach

**讲者：** Michael Manapat (Co-Founder, Rowspace); Jack McDonald (Principal at Apollo Global Management, COO of Redding Ridge, Apollo Global Management)  
**时间 / 地点：** 4:30pm–4:48pm · Simons Room (Metropolitan East) · Featured Keynote  
**链接：** **[📰 中文版](sessions/zh/10-14-1630-ai-in-practice-an-inside-look-at-apollos.md)** · **[📰 Full write-up](sessions/10-14-1630-ai-in-practice-an-inside-look-at-apollos.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=44e9d04a-e8b4-418c-8a85-02fbb9263dd0) · [▶ 视频](https://www.youtube.com/watch?v=BOWYujhIQuo) · [▶ 直播 @ 7:50:03](https://www.youtube.com/watch?v=jiUC6IUMdvc&t=28203s) · [精确版](sessions/precise/10-14-1630-ai-in-practice-an-inside-look-at-apollos.md)

**讲了什么。** Apollo 的 CLO 业务过去要花 2–3 天在 Excel 里手工搭一个模拟组合；用了 Rowspace 只需几分钟。这是一场客户联合分享，讲如何把 AI 用在分散的信贷数据上。（视频改名为「AI Agents Cut 3-Day Portfolio Builds to Minutes」。）

<details><summary>官方介绍（英文原文）</summary>

> Rowspace customer co-presenting keynote

</details>

**我的收获**

- 案例 1：CLO 的组合构建，追踪资金流向。几天的手工 Excel 变成几分钟，而且是实时的。
- 案例 2：一个 warehouse 采购流程，原本靠手工，最后以一份书面总结收尾。
- 供应商的角色类似 forward-deployed engineer：在分散的数据之上理解工作流。

### Process Supervision for Agents That Do Long-Horizon Work

**讲者：** Mitchell Troyanovsky (Co-Founder, Basis)  
**时间 / 地点：** 4:48pm–5:06pm · Simons Room (Metropolitan East) · Featured Keynote · ⭐ 标星  
**链接：** **[📰 中文版](sessions/zh/10-14-1648-process-supervision-for-agents-that-do-l.md)** · **[📰 Full write-up](sessions/10-14-1648-process-supervision-for-agents-that-do-l.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=1cbc625f-7f07-49bb-9c5c-57c95f07ba77) · [▶ 直播 @ 8:09:22](https://www.youtube.com/watch?v=jiUC6IUMdvc&t=29362s) · [Basis behavior spec](https://www.agentbehavior.dev/) · [精确版](sessions/precise/10-14-1648-process-supervision-for-agents-that-do-l.md)

**讲了什么。** Basis 为会计师打造 agent。长周期 agent 可能经由不可靠的步骤得出正确结果，比如一个税务结论依据的是预训练知识或某篇博客，而不是一手资料。评估长周期工作，需要同时有关于结果和背后决策的证据。

<details><summary>官方介绍（英文原文）</summary>

> When an agent takes on hours of research, data extraction, tool use, and judgment, it may make hundreds of decisions before producing a final result. That result can be correct even when the steps behind it are unreliable. Evaluating long-horizon work requires evidence about both the result and the decisions that produced it.
>
> Basis builds AI agents for accountants and encounters this problem across complex accounting work. An agent may reach the correct tax conclusion while relying on pretraining or a blog instead of checking the tax code and regulations. An outcome eval marks the answer correct, even though the research would not meet the standard an accountant expects.
>
> Basis developed behavior specs to evaluate those decisions across the full trajectory. Each spec defines how an agent should behave when a specific situation occurs. A judge reviews the trajectory and determines whether that situation arose and whether the agent followed the expected behavior. Basis partnered with Braintrust to publish the approach as an open standard for teams building long-horizon agents.
>
> In this session, Mitch Troyanovsky will explain why outcome evals become expensive and slow to iterate on for long-horizon work, how Basis chooses the few behaviors worth measuring, and how behavior failures guide changes to prompts, tools, or runtime context. He will also discuss how teams can evaluate behavior across production traffic without turning the spec into an unmanageable list of every rule an agent might follow.

</details>

**我的收获**

- 只看结果的 eval 不够：两次运行结局一样，过程却可能不同。形成判断不只关乎准确率，也关乎时间之类的因素。
- 过程监督：评判步骤，而不只是答案。「运行时交易数据」这个类比很贴切，因为 agent 的 context 是实时变化的。
- Basis 的 behavior spec 记录了构建者们达成一致的观点（agentbehavior.dev）。

<details><summary>slide 照片（16 张）</summary>

<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/01.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/02.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/03.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/04.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/05.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/06.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/07.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/08.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/09.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/10.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/11.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/12.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/13.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/14.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/15.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/16.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/16.jpg" width="260" alt="slide"></a>

</details>

### Everything will be Simulated

**讲者：** Cristóbal Valenzuela (Co-Founder and Co-CEO, Runway)  
**时间 / 地点：** 5:06pm–5:24pm · Simons Room (Metropolitan East) · Featured Keynote  
**链接：** **[📰 中文版](sessions/zh/10-14-1706-everything-will-be-simulated.md)** · **[📰 Full write-up](sessions/10-14-1706-everything-will-be-simulated.md)** · [日程](https://ai.engineer/nyc/2026/schedule?session=af8422e8-d5f8-4dbd-a918-6360f5bfba80) · [▶ 直播 @ 8:29:11](https://www.youtube.com/watch?v=jiUC6IUMdvc&t=30551s) · [精确版](sessions/precise/10-14-1706-everything-will-be-simulated.md)

**讲了什么。** Runway 联合创始人谈世界模型已经成熟到能实时模拟真实环境，以及这为机器人、数字界面和科学带来了什么。

<details><summary>官方介绍（英文原文）</summary>

> AI models have spent years learning to generate text, images, and video. The next frontier of AI development focuses on models that understand space, motion, and the physical dynamics of the real world. Cristóbal Valenzuela, Co-CEO and Co-Founder of Runway, will discuss world models mature enough to simulate real environments in real time, and what this progress opens up for robotics, digital interfaces, and broader scientific discovery.

</details>

**我的收获**

- 「world interface model」这个想法很有意思：比如它可以直接展示一辆车的内部，不需要任何代码。
