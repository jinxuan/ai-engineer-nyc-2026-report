[English](../10-13-0918-anthropic-accelerating-frontier-ai-in-fi.md) | **中文**

# Anthropic 给金融业的方案：agent 的地基只建一次

*在 AI Engineer NYC 2026 上，Anthropic 的 Belinda Neal 和 Keon Shahab 对金融机构说，agent 的瓶颈已经不在模型，而在于每个 agent 都要手工搭。两人的 slide 给出了一个五层的「agent foundation」，并以 Claude for Financial Advisors 作为全行业的例子。*

**Featured Keynote（Main Stage）· 2026 年 10 月 13 日（周二）· 9:18–9:36am · Simons Room (Metropolitan East)**
**讲者：** Belinda Neal，Managing Director, Financial Services，Anthropic；Keon Shahab，Applied AI Architect，Anthropic

---

**关于素材的说明。** 这场演讲没有任何录音可用。它在第一天直播里播出过（作者的照片拍的就是直播画面，带观众人数角标），但发布的直播录像跳过了它：主持人在 6:53 介绍了两位 Anthropic 讲者，接下来出现的却是 Bridgewater 那场。截至本文写作时也没有单独视频。因此本文只依据 slide 照片和官方简介，记录的是 slide 上写了什么，而不是讲者说了什么，也无法知道有没有 Q&A。

![Who you are hearing from](../../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/01.jpg)
*讲者介绍页：Neal 在 Goldman Sachs 待了 19 年以上，Shahab 在 Databricks 待了五年。*

开场 slide 介绍了两位讲者。Belinda Neal 是 Anthropic 金融服务的 Managing Director，此前在 Goldman Sachs 工作 19 年以上，做过工程、Global Markets 和 Equity Research。Keon Shahab 是 Applied AI Architect，此前在 Databricks 做了五年金融服务方向的 Lead Solutions Architect。从照片里的讲者小窗看，开头和结尾由 Neal 讲，五层架构由 Shahab 讲。

官方简介定下了论点：前沿 AI「进步得比金融服务业采用得快，这个差距就是机会」。简介说每家公司都有两个前沿要到达：「the frontier of intelligence（智能前沿），即最好的模型今天能做什么；以及 the frontier of adoption（采用前沿），即公司有多少真实工作跑在它上面」。

## 1. 三个前沿

![Taking the frontier to the industry](../../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/02.jpg)
*演讲结构：智能、采用，以及把同一套地基用到整个行业。*

| # | 前沿 | 标题 | slide 上的说明 |
| --- | --- | --- | --- |
| 1 | The frontier of intelligence | Applying intelligence | What the best model can do today. An alpha opportunity. |
| 2 | The frontier of adoption | Scaling agents | Building the agent foundation and the operating model. A scale opportunity. |
| 3 | Applied to financial services | Compounding impact | The same foundation, built once for all of wealth. An industry opportunity. |

slide 把这场演讲概括为「如何在这三个方面抓住前沿机会」。

## 2. 智能前沿

![Frontier AI changes what's possible](../../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/03.jpg)
*coding、cyber 和 finance 三条示意曲线，各自在不同时间越过能力阈值。*

一张图用三条示意性、时间平移过的曲线，表示 coding、cyber 和 finance 从早期应用一路升向「autonomous fleets of agents」（自主运行的 agent 群）。按脚注，coding 随 Claude Opus 4.5 越过阈值（标在 Nov '25），cyber 随 Claude Mythos Preview 越过阈值（标在 Apr '26；Anthropic 在 2026 年 4 月随 Project Glasswing 公布了这个模型，没有公开发布）。finance 目前标为「early agents」。图上有一段引语，署名 Citadel 的 Atte Lahtiranta：「我们的投资专业人员活在数据和分析模型里。Claude for Excel 在那里和他们会合，让他们搭建、测试和信任自己工作的速度出现 step-change。」脚注说出处是 Anthropic 金融服务 keynote「The Briefing」。（Anthropic 2026 年 5 月金融服务发布的报道里，Lahtiranta 的引语措辞不同；slide 上这一版找不到原始出处。）

![The Claude model family](../../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/04.jpg)
*从「faster and lower cost」到「more capable」的四个模型，各配一条客户引语。*

模型家族那页把当前每个 Claude 模型对应到金融工作和一条客户引语：

| 模型 | 定位 | slide 上的用例 | slide 上的客户引语 |
| --- | --- | --- | --- |
| Haiku 5.5 | Fastest and lowest cost | Classification and routing; field extraction, chat | AlphaSense：「Ask in Document is one of our big sources of spend, doing about 8M calls a week in production.」 |
| Sonnet 5.5 | For everyday work at volume | Analysis and reporting; filings at volume | Balyasny：「Claude Sonnet 5.5 scored ahead of Sonnet 5 and used about 121k tokens per answer, where Sonnet 5 used 497k.」 |
| Opus 5.5 | The daily driver for complex finance work | Decks and merger models; credit agreement redlines; day-to-day agentic coding | Rogo：「At its lowest effort setting, Claude Opus 5.5 beat Opus 5 at high effort on our BigFinance Bench with about 60% fewer output tokens.」 |
| Fable 5.1 | For the hardest work | Quant and trading research; the hardest coding problems; long unattended runs | Millennium：「A particular piece of code had an extremely rare crash, about one in a million runs, that nobody on our team had explained in four to five years. … Claude Fable 5.1 was the first to find it.」 |

![Smarter, cheaper and safer with every release](../../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/05.jpg)
*能力、成本和 prompt injection 的数字，脚注写着这些数字还有待确认。*

第三页 slide 论证模型「每次发布都更聪明、更便宜、更安全」：

| 论点 | 主数字 | slide 上的细项 |
| --- | --- | --- |
| Intelligence is going up | 16+ hrs of work a model can finish on its own | In 2023: minutes |
| Cost per task is going down | 40% less to run: Opus 5.5 against Opus 5 | Haiku 5.5: 75% less; Sonnet 5.5: up to 30% less; Fable 5.1: 25% less |
| And it is getting safer | 1.0% of prompt-injection attacks succeed on Fable 5.1 | Fable 5, the release before: 6.5%; other frontier models: 9% to 53% |

脚注说能力数字来自 METR 的 task-length 测量，成本来自 Anthropic 的发布页（每个模型和它所替代的模型比），安全数字来自 Gray Swan indirect prompt injection benchmark（15 次尝试内的攻击成功率）。脚注最后一句是：「Figures to confirm before the talk.」（演讲前待确认的数字。）这些数字没能全部核实。一个汇总厂商自报结果的公开 benchmark 网站列出 Fable 5.1 在 Gray Swan 测试上为 1.0%。有一篇报道说 Opus 5.5 的 token 单价比 Opus 5 低 20%；slide 上的 40% 是 cost per task，口径不同。

## 3. 采用前沿：从少数 agent 到很多 agent

![Agents work. Now scale them.](../../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/06.jpg)
*四个阶段，从 2023 年的 chatbot 到「many agents, one foundation」。*

采用那页画了四个阶段：

| 时间 | 阶段 | slide 上的说明 | slide 上的例子 |
| --- | --- | --- | --- |
| 2023：公司做 chatbot | Claude assists | 「Ask it things. Marvel a bit. Move on.」 | 一个关于差旅政策的泛泛回答 |
| 2024：公司跑试点 | Claude collaborates | 「RAG, a knowledge base, a pilot or three.」 | 「Found it in the 2024 travel policy (p. 4): international trips need director approval.」 |
| 2025 至今：少数 agent，手工搭建 | Claude works | 「Agents doing real work.」（「We are here」） | 「Done. I fixed the billing bug across 12 files, added tests, and opened a PR for your review.」 |
| 下一步：很多 agent，一套地基 | Claude operates | 「Fleets of agents run whole processes.」 | |

结论是：「The model is no longer the constraint. Building every agent by hand is.」（瓶颈不再是模型，而是每个 agent 都手工搭。）「下一步：怎样建 agent foundation。」

![The agent foundation: five layers, built once](../../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/07.jpg)
*两层管 agent 能做什么，三层管你能不能信它，底座是模型。*

这套地基在模型之上有五层，每层回答一个问题：

| 分组 | 层 | 问题 |
| --- | --- | --- |
| 让你能信任它 | 5. Traces and evals | 怎么知道它还在正常工作？ |
| | 4. Sandbox and approval | 它行动前谁来签字？ |
| | 3. Identity and permissions | 它以谁的身份行动？ |
| 它能做什么 | 2. Skills | 它懂你的业务和流程吗？ |
| | 1. Connectors | 它能接触到你的系统和数据吗？ |
| 底座 | The model | 它能干这活吗？「大多数团队从这里开始。」 |

接下来每一层的 slide 都标着「illustrative example」（示意例子）。

### Layer 1：connectors

![Layer 1: Connectors](../../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/08.jpg)
*五个 agent 和五个系统通过一层 MCP 相连。*

connectors 那页画了五个 agent（trade breaks、KYC review、credit memo、onboarding、surveillance）通过一层 MCP 连到五个系统（orders、market data、payments、documents、CRM）。论点就是一道算术：100 个 agent 直接连 50 个系统要 5,000 个集成，「一个系统一变，每个 agent 都坏」。走 connector 只要 150 个（100 个 agent 加 50 个系统），「一个系统一变，只修一个 connector」。slide 的总结是：「每个系统只接一次，在一个地方管控，任何 agent 都能用。」

### Layer 2：skills

![Layer 2: Skills](../../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/09.jpg)
*一个调查 trade break 的 skill 文件，归运营部门所有，像代码一样做版本管理。*

skills 的例子是一个文件 `break-investigation/SKILL.md`，写的是「本公司如何调查一笔 trade break」，共三步：核对交易两边；找出不一致的字段；「提出修复方案，永远不要自己执行。」文件标签是：归 Operations 所有、像代码一样评审、版本 14。「任何需要这套方法的 agent 都能拿去用，」slide 写道。「方法写下来一次，模型就成了专家。」

### Layer 3：identity and permissions

![Layer 3: Identity and permissions](../../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/10.jpg)
*角色为 ops-analyst 的 trade-break investigator：读允许，CRM 拒绝，写只能提议。*

同一个 agent 的身份是 `trade-break-investigator`，角色是 `ops-analyst`，权限按范围划定：

| 操作 | 资源 | 结果 |
| --- | --- | --- |
| read | Orders | Allowed |
| read | Market data | Allowed |
| read | CRM | Denied |
| write | Orders | Propose only |

「每次调用都按角色检查，并记录日志。」原则是：「No agent is a super-user. Each one is scoped to its job.」（没有哪个 agent 是超级用户，每个都只限于自己的工作。）

### Layer 4：sandbox and approval

![Layer 4: Sandbox and approval](../../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/12.jpg)
*提议、dry run、人工批准、执行，由 hook 而不是模型来强制。*

审批流程分四步：agent 提议（一项 booking 修改）；以 dry run 方式暂存，不动任何真实数据；由一个人批准（运营负责人）；按批准的原样执行。强制执行不靠模型：「skill 负责询问，hook 负责强制。这段代码在每个动作之前运行，模型无法跳过。」slide 的总结是：「在指定的人批准之前，没有任何真实的东西会动。」

第 5 层 traces and evals 只出现在总览页和小结页上，没有拍到它单独的一页。

![A strong foundation lets you move fast](../../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/13.jpg)
*第 3 到第 5 层回答了风险委员会最先问的三个问题。*

这一部分以监管角度收尾。「风险委员会要求的那些控制，正是让团队不必从头开始就能上线下一个 agent 的那些控制。」风险委员会最先问的三个问题对应第 3、4、5 层：它以谁的身份行动，它行动前谁签字，怎么知道它还在正常工作。「Regulated does not mean slow. Every new agent inherits all three.」（受监管不等于慢。每个新 agent 都继承这三层。）

## 4. 用到整个行业：Claude for Financial Advisors

![Claude for Financial Advisors: the orchestrator across the stack](../../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/14.jpg)
*Claude 位于理财顾问各种工具的中心，connectors、skills 和 approval 分别是第 1、2、4 层。*

第三个前沿把同一套地基用到整个行业。slide 把 Claude 放在理财顾问各个系统的中心：CRM、托管方、组合与报告、规划、另类投资、会议记录、遗产与文档、市场数据。slide 说顾问花在客户身上的时间不到 20%，「其余都耗在 10 多个互不相连的工具上」，引用的是 Kitces Research。（Anthropic 的产品文章引用同一研究，说法是大约六分之一的时间。）五层里有三层对应到产品上：connectors（「合作方工具负责计算：来自你信任的系统的确定性数字」）、skills（「Claude 负责编排：跨工具端到端跑完每个工作流」）、approval（「顾问负责批准：未经签字，什么都不保存、不发送」）。产品于 2026 年 9 月 14 日在 Future Proof 大会上发布。Anthropic 自己的介绍和审批层一致：CRM 更新、客户消息草稿等操作由 Claude 暂存，等顾问批准，受监管的活动「仍须经过人工审查和批准」。

![Layer 1: the industry connects once](../../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/15.jpg)
*按类别分组的 connector 合作方。*

| 类别 | slide 上的 logo |
| --- | --- |
| Custody & investments | BlackRock, Charles Schwab, iCapital, Vanguard |
| Portfolio, planning & reporting | Addepar, Envestnet, Orion, SS&C Black Diamond, wealth.com |
| Data & research | FactSet, Morningstar, S&P Global, Snowflake |
| CRM & client meetings | HubSpot, Redtail, Salesforce, Wealthbox, Zocks |
| Core software | Box, Docusign, Dropbox, Google Workspace, Microsoft, Slack, Zoom |

照片模糊，有几个 logo 是按轮廓认出来的。Anthropic 当前的合作方名单和 slide 有重叠但不完全一样；之后又加了 CAIS、Franklin Templeton、Invesco、MSCI 等。

![Layer 2: eight workflows give time back](../../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/16.jpg)
*产品自带的八个 skill。*

Claude for Financial Advisors 自带的八个 skill 是：pre-meeting prep、meeting follow-up、rebalance review、alternatives brief、estate and tax brief、prospect intake、advisor onboarding、compliance review。「可以直接用，也可以按你们公司的情况改。」Anthropic 的产品页列的是同样八个，名字稍长一些。

## 5. 带回去的三个问题

![Take the frontier to your firm](../../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/17.jpg)
*一个工作流，一套地基，一个 connector。*

最后一页把三个前沿变成了官方简介里承诺的「带回自己公司的三个问题」：

| 前沿 | 要做的 | 问题 |
| --- | --- | --- |
| 智能 | One workflow | 你要把前沿模型用到哪个用例上？ |
| 采用 | One foundation | 你怎样建 agent foundation，从少数扩展到很多？ |
| 用到金融服务 | One connector | 你怎样发布 connectors 和 skills，让你的用户在上面搭建？ |

结尾一句是：「How will you take on the frontier opportunity?」（你打算怎样抓住前沿机会？）

---

**链接：** [日程](https://ai.engineer/nyc/2026/schedule?session=68a1e78b-569e-4314-a56f-f20508123cbe) · [直播（第一天；主持人在 6:53 的介绍，演讲本身不在录像里）](https://www.youtube.com/watch?v=akEqiXFo3GM&t=413s) · [Claude for Financial Advisors](https://claude.com/resources/articles/claude-for-financial-advisors)（Anthropic）· [Future Proof 报道](https://investmentnews.com/practice-management/anthropics-big-deal-claude-for-financial-advisors-feels-the-love-future-proof/268189)（InvestmentNews）· [Gray Swan IPI 结果](https://benchlm.ai/benchmarks/gray-swan-ipi)（BenchLM）

这场演讲没有录音，因此本文只依据 slide 照片，加上官方简介和以上资料；反映的是 slide 上展示的内容，而不是台上说了什么，也不知道有没有 Q&A。[精确版笔记](../precise/10-13-0918-anthropic-accelerating-frontier-ai-in-fi.md)（中文）列出了所有未能核实的数字。
