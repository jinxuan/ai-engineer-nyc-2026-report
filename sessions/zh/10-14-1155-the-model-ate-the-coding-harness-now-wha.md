[English](../10-14-1155-the-model-ate-the-coding-harness-now-wha.md) | **中文**

# 模型吃掉了 coding harness，但 harness 仍在打开下一片前沿

*在 AI Engineer NYC 2026 上，曾负责 Cursor agent harness、现在做 Grok Bot 的 Jediah Katz 认为：模型变聪明之后，花哨的 coding harness 已经没有必要；但在新类型的工作、基础设施、信任和成本上，harness 仍然重要。*

**Breakout Session · 2026 年 10 月 14 日（周三）· 11:55am–12:13pm · Shannon Room (NY Ballroom E)**
**讲者：** Jediah Katz，Engineer, Agent Harness，SpaceX Super Intelligence（此前在 Cursor）

![标题页：The Model Ate the Coding Harness. Now What?](../../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/01.jpg)
*标题页。*

---

「我 2025 年初加入 Cursor，是在 agent harness 上待得最久的工程师；最近随着 SpaceX 收购 Cursor，我也来到了 SpaceX。」Jediah Katz 说。这场 talk 要回答的问题是：「agent harness 已经死了吗？」（官方发布的视频就用这个问题做标题。）

Katz 接着念了一句听起来像 chatbot 开场白的话：「下面是这份讲稿更自然流畅的版本，读起来像一场连贯的大会演讲，而不是一串零散的要点。」停顿片刻后，Katz 说这是故意的：「只是确认一下，午饭前大家的 slop 探测器还开着。」

## 1. 从 prompt engineer 到 harness engineer

![Prompt Engineer, Context Engineer, Harness Engineer](../../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/02.jpg)
*三个职位名称在幻灯片上依次升起。*

Katz 说，故事要从 2023 年初讲起，「那时 prompt engineer 是最火的头衔」。到 2025 年年中，变成了 context engineer，「也就是用程序实时为模型拼出最好的 context 的人」。而「直到今年年初，业界才定下 agent harness 和 harness engineering 这两个词，用来描述我和团队已经做了一段时间的工作」。

## 2. harness 里有什么

![Agent harness 架构](../../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/04.jpg)
*harness 用两层包住模型，再把它连到 environments、connectors、inference 和 apps。*

「harness 就像模型的眼睛和手。它包在模型外面，引导它，把它和世界连起来。」Katz 说。用户通过 app 接触 harness，harness 把模型连到电脑和 MCP server 这类第三方服务上。

图里模型在中心，外面包着两层：

| 层 | 组件（来自幻灯片） | Katz 的描述 |
| --- | --- | --- |
| Context & Orchestration | Prompts, Tools, Memory, Compaction, Model Routing, Guardrails | 「有点像蛮荒西部。」它决定「什么时候调用哪个模型、模型能看到什么 context」。 |
| Runtime | Loop, State, Transport, Auth, Triggers, Observability | 地基：「核心 agent loop 在这里运行，state 在这里存储，工作才能接着做。」搭这一层「很像传统的软件工程」。 |

harness 外围，幻灯片画了四个连接：Environments、Connectors、Inference 和 Apps。大多数人说「harness」时指的是 context & orchestration 这一层，这也是 Katz 主要工作的地方，包括「一堆你可能不知道的小技巧，我们用它们让 agent 按我们想要的方式行事」。

## 3. harness 的巅峰，然后是「harness 已死」

![Michael Truell 关于在 Cursor 里构建浏览器的推文](../../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/05.jpg)
*Cursor CEO Michael Truell，2026 年 1 月 14 日：用 GPT-5.2 构建的浏览器，连续运行了一周。*

「新年一过，我们差不多到了 harness 的巅峰。」Katz 说。幻灯片上是 Cursor 的 Michael Truell 的一条推文：「我们在 Cursor 里用 GPT-5.2 构建了一个浏览器。它不间断地运行了一周。」推文提到「3M+ lines of code across thousands of files」，用 Rust 从零写了渲染引擎，包括 HTML 解析、CSS cascade、布局、文字排版（text shaping）、绘制和一个自定义 JS VM，并说它「差不多」能用。（Simon Willison 报道 Cursor 那篇文章时，引用的是一百多万行、约一千个文件。）它用的是一个叫 Grind Swarm 的新 long-running harness，Katz 说那是「一个多模型 harness，编排了大量 agent 和大量机器」。「跑起来非常贵。为了这个 demo，Cursor 大概还宕了一两次。」

![Is the harness already dead?](../../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/06.jpg)
*四篇认为 harness 正在缩水或已经消失的文章。*

「和 agent 领域的任何事一样，harness 巅峰才过了几个月，网上就有人开始说 harness 死了。」幻灯片收集了四个例子：

| 来源 | 标题 |
| --- | --- |
| Thariq (@trq212, Anthropic)，7 月 24 日 | "The new rules of context engineering for Claude 5 models" |
| Garry Tan (@garrytan, YC) | "Thin Harness, Fat Skills" |
| Cursor blog，2026 年 4 月 30 日（Stefan Heule 和 Jediah Katz） | "Continually improving our agent harness" |
| Xuan Huang (@Huxpro)，2026 年 9 月 29 日 | "Harness is dead" |

按 Katz 的概括，Thariq 讲的是砍掉 Claude Code 大部分 system prompt，Tan 认为 harness 会淡出，只有 skill 才重要。Katz 也把自己和同事 Stefan Heule 合写的那篇算了进去：文章讲的是从 Cursor 里移除那些用来弥补模型智力短板的 guardrail，比如一个专门负责应用代码修改的小模型（因为当时的 frontier model 做不好这件事），以及针对 coding 的指令，教它「怎么探索代码库、怎么检查自己的工作」。这些文章的共同点：「它们都说，是更聪明的模型杀死了 harness。」

## 4. 证据：在 coding eval 上，简单的 harness 也跟得上

![BASH 加一个 secrets.env 文件](../../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/07.jpg)
*最简单的 harness：一个 shell，加一个装着（假的）API key 的文件。*

Katz 请观众想象「一个非常简单的 harness，模型只能用 shell，外加我所有的 secret。顺便说一句，请不要拍这张幻灯片，这张不能外传」。幻灯片上写着「BASH +」一个 `secrets.env` 文件，「everything the agent needs (please don't commit this)」，里面全是明显造假的 key，有 OpenAI、Anthropic、GitHub、Slack、Stripe、AWS 等等。Katz 问谁觉得这个 harness 在 coding eval 上能和复杂的 harness 打平，「有几只手」举了起来，「但你们大多数人都知道［不会］」。

![两项关于 harness 选择的研究](../../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/08.jpg)
*左：UC Berkeley 在 SWE-bench Lite 上的研究。右：Apple 和 EPFL 在 MLE-bench 上的研究，通过 Pedro Domingos 的推文展示。*

最近有两项研究正好测了这件事。第一项来自 UC Berkeley，「在一系列 harness 上试了七个不同的 frontier model，包括 Pi、Codex 和 Claude Code」，发现「尤其是在更新、更聪明的模型上」，「harness 的选择对 SWE-bench Lite 的分数并没有显著影响」。幻灯片的说明文字是「Harness choice barely moved SWE-Bench-Lite success rate」。从照片里读出的柱状图数据：

| 模型 | Pi | Codex | Claude Code |
| --- | --- | --- | --- |
| Claude Fable 5 | 96.7% | 96.7% | 97.8% |
| Claude Opus 4.8 | 82.2% | 88.9% | 86.7% |
| Claude Sonnet 4.6 | 64.4% | 68.9% | 66.7% |
| Claude Haiku 4.5 | 60.0% | 57.8% | 52.2% |
| GPT-5.6 Sol | 74.4% | 73.3% | 77.8% |
| GPT-5.6 Luna | 53.3% | 55.6% | 55.6% |
| Kimi K3 | 72.2% | 74.4% | （照片里被截掉） |

差距在最上面最小：Claude Fable 5 只差约一个点。Opus 4.8 差到 6.7 个点，Haiku 4.5 差到 7.8 个点，这和 Katz 加的限定「更新、更聪明的模型」对得上。

第二项研究来自 Apple 和 EPFL，「同样发现，专门的 MLE harness 在 MLE-bench 上没有带来大的差别」。幻灯片上是 Pedro Domingos 的推文，把它概括为「Harnesses are useless」，并引用了论文 "How Much of a Harness Does a Strong Agent Need for Autonomous ML Engineering?"（Kirill Brilliantov、Alejandro Hernández-Cano 和 Emmanuel Abbé）。论文里被高亮的一句：在相同时间预算、相同 frontier LLM 下，开源的 state-of-the-art harness「相比单个 session 的 minimal-harness baseline 没有任何优势，说明 backbone 才是性能的主要来源」。

Katz 接受了这个结论：「在今天，花哨的 harness 已经不再增加 coding 智力。我认为这是真的。我知道这挺让人意外。」harness 过去用来弥补「模型原始的 coding 肌肉记忆」短板的那些东西，比如教它探索代码库、「用 slash goal 这类东西」长时间持续工作、跑测试检查自己的工作：「模型已经原生吸收了这些技能。」

## 5. 数据飞轮

![数据飞轮](../../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/15.jpg)
*"The model eats the harness. The harness opens the next frontier."（模型吃掉 harness，harness 打开下一片前沿。）这个飞轮是分几张幻灯片逐步搭起来的。*

为什么会这样？「harness 其实在通过我所说的数据飞轮，让自己过时。」幻灯片上的循环有五步：harness 解锁能力；agent 接手新任务；任务变成训练数据；模型吸收这项能力；harness 删掉脚手架。用 Katz 的话说，harness 让更难的 coding 任务变得可行，更多用户用 agent 去做这些任务，「更多 coding 数据被分享给了实验室」，模型原生学会了这些技能，最后 harness「可以把当初为了让这些任务跑通而做的东西删掉」。

![Why care about the harness?](../../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/11.jpg)
*harness 没死的两个理由。*

「harness 真的死了吗？我说没有，理由有两个。」幻灯片写着：(1)「harness 为模型解锁下一片要攻克的前沿。」(2)「harness 的某些部分永远无法被吸收。」（台上 Katz 说的是模型「无法吸收」这些部分，「尤其是 context 层之外的部分」，没有幻灯片上的「永远」。）

## 6. 理由一：harness 打开下一片前沿

Katz 的第一个例子是 Grok Bot，「我们的自主同事，帮你把工作做完。不只是 coding，而是各种知识工作」。Katz 说它「已经开创了一个新品类」，然后转到它的 harness。Grok Bot 的 harness 不是一次性的对话，而是运行「长期存活的 coordinator bot，由它编排一队 worker agent」，就像「请了一位顶级律师，直接和你打交道、负责策略，但把具体任务分给一组律师助理」。

在 Cursor，随着模型变聪明，团队移除了 prompting 和 guardrail。「但有意思的是，在 Grok Bot 里，我们发现自己又把很多这类东西加了回来，加新的 prompting，加新的 guardrail」，因为在它的新领域里，很多任务模型「还没拿下」。

![Key website flows, Communication, Acting vs asking, Learning over time](../../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/13.jpg)
*Grok Bot harness 目前在攻的四个方向。*

Katz 列了四个。关键网站流程，比如做幻灯片和订行程。沟通：「一个自主同事需要随时告诉你它在做什么，但又不能把你淹没。」行动还是询问：Grok Bot 能做给同事发邮件这类有风险的事，而「你只希望我们按你本来的意思去做」。还有通过 memory 逐渐学会用户的工作方式。对深度使用过 coding agent 的人来说，这些都不新鲜，但「对 Grok Bot 来说，它们重要得多，因为每个人都需要」，而眼下「是 harness 让这些任务变得可行」。

飞轮由此闭合：「当 coding 这样的能力被模型商品化之后，其实是 harness 为模型打开了下一片前沿，让它去攻克，并让飞轮再转一圈。」新前沿可以在 coding 之外，比如 Grok Bot；也可以在软件工程之内、coding 之外：「你越资深，真正写代码的时间就越少。」Katz 说这是「我最希望你们理解的核心思想」。

第二个例子是 software factory，「多个 agent 协作，解决完整的软件开发生命周期」，Katz 说这「离实现还很远」。在内部，有人在频道里报了一个 bug；一个自动化流程做 triage；一个 cloud agent 尝试修复，并用内部工具录下修复前后的视频来证明修好了；修复合并、开始部署后，一个 rollouts 产品会再启动一个 agent 盯着生产环境有没有问题。「你可以把这一切看成一个 meta harness，它在编排很多 agent 一起工作。」

## 7. 理由二：模型吸收不了的部分

第二个理由讲的是 harness 中模型够不到的部分。Katz 举了三个。

![一个人在睡觉，机器人在云上工作](../../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/16.jpg)
*基础设施：主人睡觉时，agent 在云上继续工作。*

**基础设施和 runtime。** 过去一年，agent 做一个典型任务的时长从几分钟变成几小时，再到几天甚至更久。「这根本不可能在某个人的笔记本上跑，笔记本可能会睡眠。如果你真想要一个自主同事，它就必须跑在云上。」

![agent 的三个权限对话框](../../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/17.jpg)
*限定范围的权限：你选定的 Slack 频道、这台 Mac 上的命令、你挑选的仓库里的 pull request。*

**信任与安全。** Bash 加一个 API key 文件能解决 coding 任务，「但你大概不想把所有 API key 放在一个文件里交给模型……我相信你们的安全负责人会有话要说」。幻灯片上是三个权限提示：一个 agent 请求访问 Slack（「在你选定的频道里读取和发帖」），控制一台 Mac（「在这台 Mac 上运行命令」），以及访问 GitHub（「在你挑选的仓库里开 pull request」）。公司需要把权限限定到具体的 agent 和具体的人，而 guardrail 很重要，「对任何不想某天早上醒来发现发生了灾难性操作、或者多了一笔 1 万美元 API 账单的人来说都是如此」。

![关于 Grok Bot 效率和速度的推文](../../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/19.jpg)
*Katz 关于 Grok Bot 用量和速度的帖子，以及用户的反应。*

**效率。** Katz 说，在那两项 harness 几乎不影响分数的研究里，「每个任务的成本差别很大」。Katz 过去一个月一直在优化 Grok Bot 的 harness，调「cache hit rate……token efficiency……model mix」，在「不损失任何质量」的前提下降低成本和延迟。具体数字在幻灯片上，来自 Katz 发过的帖子：

| 日期 | 帖子 | 数字 |
| --- | --- | --- |
| 9 月 5 日 | 改进 routing、caching、动态 context 使用和 token efficiency | 用量更耐用：平均 10%，部分重度用户最高 35% |
| 10 月 1 日 | "Major improvements to make Grok Bot 3x faster" | 回复时间：P50 降 65%，P90 降 72% |

旁边还有两条用户帖子：「WAIT A SECOND. Why is Grok bot so fast today??」和「feel like grok bot is fast AF now」。结论是：「你用 agent 做的工作越多……拥有最高效的 harness 对你就越有价值。」

## 8. 接下来

「这个飞轮在加速，我们现在进入了一个新阶段：产品又一次跑得比模型快。」Katz 说。所以 harness「在变干净之前会先变得更乱」，harness 应该设计成功能容易上线、也容易下线。

![Designing to be deleted](../../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/20.jpg)
*两条设计原则：隐藏内部机制，构建可扩展的 primitive。*

幻灯片给了两条原则。第一，「Show users outcomes, not harness inner workings (Hyrum's law)」（给用户看结果，而不是 harness 的内部机制）。Grok Bot 和同类产品向用户暴露的 harness 越来越少，因为按照 Hyrum's law，「你暴露给用户的任何东西，他们都会开始依赖」。用户能看到的少了，但「要处理的变更管理也少了，你可以迭代得快得多」。第二，「Design extensible primitives for agents」（为 agent 设计可扩展的 primitive）：这类基础构件让你加功能时不会让 agent 的处境变复杂，删功能时也不需要复杂的重构。「这就是好的工程。」

![Designing agent primitives: subscriptions](../../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/21.jpg)
*一个 subscriptions primitive 把多种事件路由给对应的 agent。*

Katz 举的 primitive 例子是 subscriptions。agent 需要在事件发生时被唤醒：一封发给 bot 的邮件、一次 Slack @、一次 PR review、一条 PagerDuty 告警、工作日早上 9 点的定时任务，或者 agent 关心的某个 PR 上 CI 变红。不是为每种事件单独做集成，而是「一个框架、一个 tool」，agent 用它来订阅。幻灯片上，一个 subscriptions 层负责权限、路由到对应的 agent、去重、过滤噪声、唤醒 agent，对外暴露为 `subscribe(trigger)` 和 `wake(event)`。

![Firings by shim](../../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/22.jpg)
*一个统计每个 shim 触发频率的 dashboard，时间跨度约 45 分钟。*

第二个例子是 shim：用确定性的 guardrail 修补模型的不良行为，比如 Grok Bot 回复不够快时，提醒它一句「Hey, user's waiting on you」。「我们其实不想做这些事。它们算是 hack，但我们宁愿现在就修好，也不想……等下一个模型 checkpoint。」每个 shim 都集中跟踪，每次触发都会记录。幻灯片上的 dashboard 按时间堆叠了各个 shim 的触发次数，名字有 `mcp_missing_required_arguments`、`mcp_schema_key_spelling`、`mcp_argument_envelope`、`await_task_id_alias` 和 `legacy_update_state_forwa…`，另外还有 16 个。有一个 bot 盯着这个 dashboard，「每当某个 shim 不再触发，那个模型或 agent 就会提一个 PR，说这个 shim 可以清理掉了」。

![What's next for the harness?](../../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/23.jpg)
*三个预测。*

最后一张幻灯片给了三个预测：(1)「Harnesses will be designed to be deleted.」（harness 会被设计成可以删除的。）(2)「The harness moves up a level: agents are the new building blocks.」（harness 上移一层：agent 是新的基础构件。）过去的 harness「是把一堆 LLM 调用组合起来搭成的」；Grok Bot 和 software factory 则以 agent 为核心构件。(3)「Within 6 months, 90% of agent work runs off the laptop.」（6 个月内，90% 的 agent 工作会离开笔记本运行。）

## 9. 结论

「harness 死了吗？让它更会写代码的那些部分，是的，这就是飞轮在起作用。」Katz 说。但 harness 是 agent 打开下一片工作前沿的途径，而且「harness 带来的信任、效率和基础设施，这些不会消失。所以，昨天的 harness 也许死了，但明天的 agent harness 万岁」。

---

**链接：** [日程](https://ai.engineer/nyc/2026/schedule?session=29083884-87dc-44f8-b412-b494afca8856) · [视频："Is the Agent Harness Dead?"](https://www.youtube.com/watch?v=7Y4sbZgBdnc) · [Talk 页面](https://ai.engineer/talks/7Y4sbZgBdnc) · [Continually improving our agent harness](https://cursor.com/blog/continually-improving-our-agent-harness)（Stefan Heule、Jediah Katz）· [How Much of a Harness Does a Strong Agent Need for Autonomous ML Engineering?](https://machinelearning.apple.com/research/harness-autonomous-ml-engineering)（Apple ML Research）· [HarnessTax 研究](https://arena.ai/blog/coding-agents-harness-tax/)（Arena blog，即 Berkeley 那项研究）· [Cursor cloud agent 文档](https://cursor.com/docs/cloud-agent) · [Cursor is now a part of SpaceX](https://cursor.com/blog/joining-spacex) · [Scaling long-running autonomous coding](https://simonwillison.net/2026/Jan/19/scaling-long-running-autonomous-coding/)（Simon Willison）

本文依据官方视频字幕、作者在现场对同一场 session 的录音、作者拍的 23 张幻灯片照片中的 17 张（其余是飞轮逐步搭建的中间页、一张模糊照片和几张近似重复的照片），以及上面列出的来源。talk 在掌声中结束，没有 Q&A。[精确笔记](../precise/10-14-1155-the-model-ate-the-coding-harness-now-wha.md)（中文）保留了时间戳，并完整记录了幻灯片和各来源之间的出入。
