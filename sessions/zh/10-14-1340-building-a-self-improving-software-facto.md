[English](../10-14-1340-building-a-self-improving-software-facto.md) | **中文**

# coding agent 应该待在一座会给自己打分、自我改进的云端工厂里

*在 AI Engineer NYC 2026 上，Warp 的 Zach Lloyd 主张，公司应该把 coding agent 从工程师的笔记本上搬到一座「cloud software factory」（云端软件工厂）里：用代码定义，用公司自己的数据衡量，由 agent 来改进。随后 Zach Lloyd 演示了 Warp 自己的版本。*

**Expo Session · 2026 年 10 月 14 日（周三）· 1:40–1:58pm · McCarthy Room (Riverside Suite)**
**讲者：** Zach Lloyd，Founder & CEO，Warp

---

Zach Lloyd 说，今天大多数工程师是以交互的方式使用 coding agent：在自己的笔记本上，也许同时开好几个，「交互式地给 agent 下 prompt，让它们做功能、修 bug」。而 Lloyd 接触的大多数公司想要走到的地方，「是更接近自动化开发的东西」：部署 agent，「以尽可能低的成本拿到尽可能多的业务价值，同时做到可治理、安全」。

session 简介列出了实现这一点的四个基础构件：把 agent 对话、skill 和安全权限集中管理的云基础设施；configuration as code；「超越『发布了多少个 PR』」的指标；以及自我改进循环，「让 agent 回顾过去的对话、给质量打分，并对驱动工厂的 skill 提出改进」。Lloyd 用大约六分半钟的幻灯片讲了这套论点，然后切到 Warp Factories 的现场演示，这是 Warp 为此做的产品。

## 1. 从交互式开发到自动化开发

![The evolution of agentic development](../../slides/10-14-1340-building-a-self-improving-software-facto/01.jpg)
*「The evolution of agentic development」：chat、autocomplete、interactive prompting（今天）、automated development（下一步）。*

拍到的第一张幻灯片画了一条上升曲线，穿过四个阶段：chat、autocomplete、interactive prompting（标着「today」），以及 automated development（标着「next」）。Lloyd 说，交互式 agent 会碰到上限：「瓶颈在于得有一个人在运行」它们；而且「如果每个人都有自己的一套配置，成本很难控制」，也很难治理。

![More AI usage != more business value](../../slides/10-14-1340-building-a-self-improving-software-facto/02.jpg)
*今天 agent 用法的三个问题，旁边是三条新闻标题。*

下一张幻灯片是「More AI usage != more business value」（AI 用得更多 ≠ 业务价值更多），列了三个问题：临时的交互式使用，没有自动化；没有系统化的方法去优化成本和质量；没有一个集中的治理和控制层。旁边是三条新闻标题：

| 媒体（按幻灯片所示） | 标题 |
| --- | --- |
| Forbes | "Is The Cult Of 'Tokenmaxxing' Just Another Fad Or The New Normal?" |
| VB | "The AI governance mirage: Why 72% of enterprises don't have the control and security they think they do" |
| Forbes | "Uber Burns Its 2026 AI Budget In Four Months On Claude Code" |

Uber 的事被广泛报道过：Uber CTO Praveen Neppalli Naga 告诉 The Information，公司 2026 年的 AI coding 预算四个月就用完了；Fortune 后来报道，Uber 的 COO 对这笔支出的回报提出了质疑。Forbes 那篇文章本身和 VB 文章里的 72% 这个数字，本文没有核实。

## 2. 本地 agent 无法治理

![Local agents are ungovernable agents](../../slides/10-14-1340-building-a-self-improving-software-facto/03.jpg)
*「Local agents are ungovernable agents.」（本地 agent 就是无法治理的 agent。）*

Lloyd 指出了在笔记本上运行 agent 的两个问题。第一是安全：如果本地 agent 被攻破，「它基本上就能接管在这台笔记本上登录的那个用户的一切，这可不是好事」。第二是治理：「每个人都在配置自己的 MCP、自己的 skill、自己的规则，如果你看不到发生了什么，你就没法监控。」

「这两个问题的解法，都是把你的 agent 放进 sandbox，搬到云上。」Lloyd 说。为此正在出现的产品类别，Warp 称之为 cloud software factory，Lloyd 也承认这个词「有点被用滥了」。

## 3. 工厂的循环

![A cloud software factory](../../slides/10-14-1340-building-a-self-improving-software-facto/04.jpg)
*把工厂画成一个循环，有三个由人审查的节点。*

幻灯片把工厂画成一个循环：一个新的产品想法或 bug 报告，然后是 triage、spec、implement、verify、ship 和 monitor，再回到起点。循环上有三个人工检查点：审查 spec、审查代码、审查产品。

按 Lloyd 的描述，「每家公司都会在云上搭一个循环，由人和 agent 一起干活」。人往里投放工作；agent 负责软件生命周期的一部分。一个 triage agent 和人一起写 spec，agent 负责实现，还做「computer use 验证」。Lloyd 说，企业会想要「定义人和 agent 怎么互动的所有规则，然后做到能够真正、真正地随时间衡量和改进」，因为「把这个概念做对，会成为你的组织怎么交付软件的关键驱动力」。

## 4. 开放、可适配、用代码定义的基础设施

![Open, adaptable, and defined in code](../../slides/10-14-1340-building-a-self-improving-software-facto/05.jpg)
*「Software factories should be built on an infrastructure stack that is open, adaptable, and defined in code.」（软件工厂应该建立在开放、可适配、用代码定义的基础设施栈之上。）*

「在 Warp，我们相信你应该把它当成基础设施来看。」Lloyd 说：不是单个产品，而是「一个你可以在上面构建、可以扩展的基础设施栈」。幻灯片列了三个特性，Lloyd 逐一解释。

开放，指工厂支持不同的模型，「不管是 open-weight 还是 frontier」，也支持不同的 harness，因为「最好的模型和 harness 一直在变」。可适配，指「工厂应该能和你现有的基础设施一起工作」。用代码定义，指工厂有一份严格的定义，涵盖「有哪些 workflow、有哪些 skill、有哪些 MCP、它们在哪里运行」。一旦这份定义进了版本控制，「你就能测试工厂的不同版本，也能让 coding agent 来更新你的工厂」。

Lloyd 9 月的博客文章「The Factory Stack」里，第二个词略有不同：「open, composable and defined in code」，其中 composable（可组合）指公司可以采用这个栈的一部分或全部。文章把用代码定义的那部分称为「Terraform for factories」。幻灯片和 Lloyd 在台上说的都是 adaptable。

## 5. AI 主权

![Your data, your compute, your inference gateway](../../slides/10-14-1340-building-a-self-improving-software-facto/07.jpg)
*四种所有权，一行一行地出现（第一步只显示了「Your data」）。*

「我从企业那里听到的另一件事是，AI 主权很重要。」Lloyd 说。幻灯片逐步出现四行：your data、your compute、your inference gateway，以及 any model, any harness。公司应该拥有「所有进去的数据和所有出来的数据」，在自己的基础设施和算力上运行 agent（「和我们合作的大多数大企业要的就是这个」），自己提供 inference，并支持任何模型和 harness，以「真正保留选择的余地」。Lloyd 说，软件工厂「对你的业务非常关键」，公司应该掌握它的核心组成部分。（「AI sovereignty」这个说法没有出现在「The Factory Stack」里，那篇文章在 data 层和 inference 层下讲了类似的观点，包括不应该让任何外部提供商用工厂的数据做训练。）

## 6. 衡量胜过感觉

![Measurement > Vibes](../../slides/10-14-1340-building-a-self-improving-software-facto/08.jpg)
*「Measurement > Vibes」：scoring、self-improvement 和 benchmarking。*

Lloyd 说，Twitter 上哪天都有人宣称某个模型最好，也有 SWE-bench 和 Terminal-Bench 这样的公开 benchmark。但在开发越来越自动化的时候，目标是「在你自己的数据上以衡量为先」。幻灯片列了工厂应该具备的三种能力。Scoring，指能够「对你的 coding agent 表现如何做 eval」。Self-improvement，指把这些分数交给能改进工厂的 agent。Benchmarking，指构建你自己的 benchmark：「把你所有真实的 coding agent 任务拿出来，在不同模型上试一遍，衡量效果。」

## 7. 两类工程师

![Two types of engineers will use your factory](../../slides/10-14-1340-building-a-self-improving-software-facto/09.jpg)
*产品工程师和工厂工程师。*

最后一张讲观点的幻灯片把工程师分成两种角色。产品工程师仍然为客户做功能、修 bug，但他们是「通过工厂」来做，把工作投放进去。第二种，Lloyd 称之为 factory engineering（工厂工程）：「要么是你的平台团队，要么就是你的普通工程师，必须擅长」把工厂变得更好，「改进 skill、设置 context、调整 model mix」。（Lloyd 9 月在另一场 AI Engineer 活动上的 talk 题目就是「Software Engineering Is Becoming Factory Engineering」。）

## 8. 演示：Warp Factories

![Let's look at a demo](../../slides/10-14-1340-building-a-self-improving-software-facto/10.jpg)
*切到演示。*

「我来展示一下这些在实践中是什么样子，」Lloyd 说，「以我们自己的产品 Warp Factories 为例。」作者的现场录音到这里就结束了，所以 Lloyd 对演示的讲解没有录下来。以下内容根据屏幕照片还原，并借助 Warp 自己的博客文章解释这些功能是什么。Lloyd 怎么解读屏幕上的数字，无从得知。

### agent

![wilson 工厂的 Agents 页面](../../slides/10-14-1340-building-a-self-improving-software-facto/11.jpg)
*一个名为 wilson 的工厂的 Agents 页面，在 Warp 的 staging 环境上。*

演示跑在 Warp 的 staging 站点上，工厂名叫 `wilson`。Agents 页面上有一条横幅：「Configuration is read-only here. Make changes in `warpdotdev/factory-dev` at `main:v1`」，意思是工厂的定义放在一个 GitHub 仓库里。页面列了五个 agent，和第 3 节循环里的各个阶段对应：

| Agent | 屏幕上的描述 |
| --- | --- |
| wilson Foreman | "Orchestrates the factory workflow and dispatches each gated step." |
| code-review | "Reviews factory pull requests and routes findings to rework or human resolution." |
| implementation | "Implements, validates, and updates code changes." |
| spec | 部分被遮住（"Writes and …"） |
| triage | "Triages requests and establishes task state." |

侧边栏显示了产品的结构：顶层是 New Chat、Inbox、Runs、MCPs and apps、Secrets、Integrations 和 Users；工厂下面是 Dashboard、Agents、Automations、Scorers、Self-improvement（计数为 31）、Benchmarks、Factory definition 和 Settings。

![foreman agent 的详情页](../../slides/10-14-1340-building-a-self-improving-software-facto/12.jpg)
*foreman agent，「Managed in GitHub」。*

foreman 的详情页标着「Managed in GitHub」，指向同一个仓库和分支，有 Prompt、Automations 和 Settings 三个 tab。它的 agent 资源包括 Slack（Warp managed）、Figma、Sentry、一个 staging debugger，以及一个名为 `WILSON_SENTRY_AUTH_TOKEN` 的 secret。

### 一个 scorer

![Redundant Tests scorer](../../slides/10-14-1340-building-a-self-improving-software-facto/13.jpg)
*一个判断 implementation agent 有没有写低价值测试的 scorer。*

下一个屏幕是一个叫「Redundant Tests」的 scorer，描述是判断「implementation agent 最终交付的测试 diff 里有没有冗余、信号弱或琐碎的测试——评判的是测试产物本身，而不是之后有没有人要求删掉它」。页面显示这个 scorer 累计成本 $526.24，以及 30 天的通过率趋势。其中一次打分的运行被归为「No surplus tests」，运行时长 3 分 22 秒，成本 $0.26。

Warp 的文章「The missing feedback loop for software factories」（Varoon Kodithala，2026 年 8 月）解释了 scorer 的工作方式。和工厂的其他部分一样，每个 scorer 定义在一个文件里，写明它评估哪个 agent、从 0 到 1 打分的 label、通过阈值（比如 0.8）和采样率（默认 25%）。一次运行结束后，一个 cloud agent 按 rubric 给对话打分，返回 pass 或 fail 以及理由。内置的 scorer 覆盖代码质量、效率、任务和流程遵循情况，以及啰嗦程度。文章里效率不及格的例子是：一个 agent 为了改一行 CSS，用了 275 次 computer-use 操作。

### 自我改进

![Self-improvement 页面](../../slides/10-14-1340-building-a-self-improving-software-facto/14.jpg)
*对工厂提出的修改，通过率 13/51。*

Self-improvement 页面显示通过率是 51 个里通过 13 个，即 25%，修改「按 PR 或 review 开启时间分组」，还有一个待审查提案的 inbox。提案标题都是对 agent 的 skill 和指令的修改：

| 提案（按屏幕所示） |
| --- |
| Keep Go module files tidy after dependency changes ("A Go dependency update left obsolete go.sum entries and required a review-driven cleanup after CI failed…") |
| Report only findings and unexamined dimensions in reviews |
| Handle CI failure follow-ups without a plan-only skill |
| Make design snapshot lookup and pre-write locate step satisfiable |
| 另外两条，部分被遮住（"Trust the du…ility judgment"、"Read mat…menting"） |

按同一篇 Warp 文章的说法，有一个 automation「每隔几小时」运行一次，收集新的 scorer 失败，分批交给一个 self-improvement agent。这个 agent 在 transcript 里找根因，然后对 skill 和配置提出 diff：对 Warp 托管的工厂是一个 review 分支，对 GitHub 管理的工厂是一个 pull request，并且要求「make redrafts, not additions」（重写，而不是追加）。文章说，Warp 内部的工厂一周交付了「300 多个 PR」，几天内合并了十几个 self-improvement PR。文章的总结是：「你给 agent 的每一次提醒、留下的每一条 review，都应该转化为对你的软件工厂的具体改进。」

### benchmark

![Benchmarks 列表](../../slides/10-14-1340-building-a-self-improving-software-facto/15.jpg)
*为工厂定义的 benchmark。*

![斜着拍的 Benchmarks 列表](../../slides/10-14-1340-building-a-self-improving-software-facto/16.jpg)
*同一个列表，显示了更多条目。*

Benchmarks 页面列出了用工厂自己的工作构建的任务集：针对 review、spec、triage 和 foreman agent 的 default-model-selection benchmark；warp-bench-smoke-test、warp-bench-1.0 和 warp-bench-1.0-mini；WarpBench 0.1 和 0.2；一个以工单命名的 benchmark（REV-2396，关于团队设置）；一个 UI computer-use implementation 任务集；还有一个「Mini WarpBench」。

![WarpBench 0.1 结果](../../slides/10-14-1340-building-a-self-improving-software-facto/17.jpg)
*WarpBench 0.1 上几个模型的正确率与每次试验平均成本。*

WarpBench 0.1 的一次运行，日期是 9 月 2 日 10:45 PM，画的是正确率对每次试验平均成本。屏幕上标出了两个数值；下表其余数值是从图上读出来的，只是近似值：

| 模型 | 正确率 | 每次试验平均成本 |
| --- | --- | --- |
| gpt-5.6-sol (high) | 94.8%（有标注） | $6.23（有标注） |
| glm 5.3 flash | 约 93.5%（从图上读出） | $1.99，标为「lowest cost option」 |
| claude opus 5 (high) | 约 92.5%（从图上读出） | 高于 $10（从图上读出） |
| gemini 3.7 flash | 约 90.5%（从图上读出） | 看不清 |

底部有一条警告说部分结果没有打分，建议用 Rescore 重试；显示 29 个任务里有 28 个已打分。这个页面就是第 6 节里「构建你自己的 benchmark」这个想法的一个实例：Warp 自己的任务，在几个模型上跑，成本和正确率并排放在一起。

### 技术栈

![The Factories Stack](../../slides/10-14-1340-building-a-self-improving-software-facto/18.jpg)
*软件工厂栈的七层，从 factories-as-code 到 access。*

Lloyd 随后展示了完整技术栈的示意图（一个标题为「The Software Factory Stack」的 Figma 页面），和「The Factory Stack」文章里描述的七层一致：

| 层 | 幻灯片上的组件 |
| --- | --- |
| 06 Access | Factory MCP（任意 coding agent）· 沟通平台（Slack、Teams）· 任务跟踪（Jira、Linear）· 代码托管（GitHub、GitLab）· 任意设备（web、mobile）· 程序化接入（API、SDK） |
| 05 Orchestration | 原生编排（triage、spec、implement、review、verify）· 多 agent（Warp、Claude Code、Codex）· human in the loop（实时引导和交接）· automations（trigger、cron） |
| 04 Improvement | Metrics（autonomy、cost、cycle time）· scorers（评估已完成的运行）· benchmarks（按任务集比较模型或 skill）· self-improvement（自动 observer 循环） |
| 03 Inference | Warp 托管的 inference（多模型，frontier 和 open-weight）· bring your own inference（Bedrock、Vertex）· 自定义模型路由（LLM routing 规则） |
| 02 Compute | Sandbox（Docker image、Kubernetes、任意远程环境）· agent runner（Mac、Linux、Windows；可配置磁盘、RAM、CPU）· 第三项被截掉 |
| 01 Data & context | Runs 和 transcripts（agent trace）· agent memory（vector store）· agent artifacts（PR、报告、附件）· MCP 和内部知识 · access/RBAC（被截掉） |
| 00 Factories-as-code | factory.yaml · agent definitions · automation definitions · launch config |

在博客的版本里，最底层定义 agent 配置、runner、仓库、automation、集成和 scorer，「全部定义在一个 factory.yaml 文件里」；improvement 层还加了每个 PR 的成本、replay 和 self-improvement agent。

![Try Warp Factories today](../../slides/10-14-1340-building-a-self-improving-software-facto/19.jpg)
*最后一张幻灯片。*

talk 以一张写着「Try Warp Factories today」、带二维码的幻灯片结束。

---

**链接：** [日程](https://ai.engineer/nyc/2026/schedule?session=a901d5ea-57c6-433f-9c37-5fe6531adb99) · [The Factory Stack](https://www.warp.dev/blog/the-factory-stack)（Zach Lloyd）· [The missing feedback loop for software factories](https://www.warp.dev/blog/engineering-self-improving-software-factories)（Varoon Kodithala）· [Software Engineering Is Becoming Factory Engineering](https://www.youtube.com/watch?v=tUPPVhBBcoM)（Lloyd 9 月的 talk，仅作背景）· [Fortune 关于 Uber AI 支出的报道](https://www.fortune.com/2026/05/26/uber-coo-ai-spending-tokens-claude-code/)

截至写作时，这场 session 还没有发布视频。本文依据作者的现场录音（只覆盖前六分半钟，即幻灯片部分，到演示开始为止）、幻灯片和屏幕照片，以及上面列出的 Warp 博客文章。演示以及之后的内容，包括有没有 Q&A，都没有录下来。[精确笔记](../precise/10-14-1340-building-a-self-improving-software-facto.md)（中文）保留了时间戳，以及未经核实的细节的完整清单。
