# Building a self-improving software factory：精确版

> Precise notes for this session, compiled from the official video captions where available, the author's room recording, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材整理完。**录音只有前 6 分半（slide 部分），demo 和之后全部没录到**，demo 部分只能靠照片。没有 Q&A 录音。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | 作者拍的 19 张（IMG_0655–0674，缺 0665），全部是本场（Warp 的 slide 和 Warp Factories 产品界面），没有混入别场。拍照时间跨度约 16 分钟，**IMG_0664「Let's look at a demo」之后的 10 张是 demo 的唯一记录**。IMG_0671 是斜拍的 Benchmarks 列表 |
| 视频 | 本场没有。背景：同一讲者 9 月的「Software Engineering Is Becoming Factory Engineering — Zach Lloyd, Warp」（AI Engineer 频道，2026-09-27 上传，20:36，https://www.youtube.com/watch?v=tUPPVhBBcoM ），是另一场活动，只作背景，没有看内容 |
| 一手资料 | Warp 博客「The Factory Stack」（Zach Lloyd，2026-09-05）https://www.warp.dev/blog/the-factory-stack ；「The missing feedback loop for software factories」（Varoon Kodithala，2026-08-26）https://www.warp.dev/blog/engineering-self-improving-software-factories 。Warp 博客列表里还有「Using LLM-as-a-judge scoring to measure your software factory」（2026-09-18）、「Terraform for agents: how we define our software factory in code」（2026-09-30），只看到标题没读正文 |

## 基本信息

- 时间地点：2026-10-14 1:40–1:58pm，McCarthy Room (Riverside Suite)，Expo Session (18 mins)
- 讲者：Zach Lloyd，Warp Founder & CEO（官方日程）。prep 记他之前负责 Google Docs/Sheets，本次没有另行核对，【待确认】
- 讲的产品：Warp Factories（Warp 的 cloud software factory 产品；demo 用的是 `platform.staging.warp.dev`，一个叫 **wilson** 的 factory）
- 官方简介：「Interactive agents aren't scaling. Every company is coping with runaway costs, lack of governance, and difficulty automating.」四个 building blocks：1 Cloud infrastructure to centralize agent conversations, skills, and security permissions；2 Configuration as code；3 Metrics「beyond "number of PRs shipped"」；4 Self-improvement loops「for agents to review past conversations, score the quality, and propose improvements to the skills driving the factory」。简介预告了 demo：跑任务、看 agent 回看对话并对 skill 提改进

## 1. 从 interactive 到 automated development

- slide「The evolution of agentic development.」：一条曲线 Chat → Autocomplete → Interactive Prompting（标 TODAY）→ Automated development（标 NEXT）。这张是第 3 页，前两页没拍到
- 讲者说 [00:00]：工程师在笔记本上「interactively prompting agents to build features and fix bugs」；多数公司想去的是「something that is more like automated development」，以「the most possible business value at the lowest possible cost in a way that is governed and secure」部署 agent；interactive agent 的限制：「you're bottlenecked by a person」，每人一套配置导致成本难控、难治理
- slide「More AI usage != more business value.」：1 Ad hoc interactive usage, no automation；2 No systematic way to optimize cost and quality；3 No central layer for governance and control。右侧三条新闻标题截图：
  - Forbes「Is The Cult Of 'Tokenmaxxing' Just Another Fad Or The New Normal?」
  - VB「The AI governance mirage: Why 72% of enterprises don't have the control and security they think they do」
  - Forbes「Uber Burns Its 2026 AI Budget In Four Months On Claude Code」
- 资料（Uber 一条）：多家媒体报道 Uber 2026 年的 AI coding 预算四个月用完，CTO Praveen Neppalli Naga 对 The Information 这么说；Fortune 2026-05-26 报道 COO 质疑这笔投入的回报（https://www.fortune.com/2026/05/26/uber-coo-ai-spending-tokens-claude-code/ ，搜索摘要）。Forbes 原文没找到，【待确认】。72% 那条 VB 文章没有核对

## 2. Local agent 难治理 → 放进云端 sandbox

- slide「Local agents are **ungovernable agents**.」
- 讲者说 [00:54]：本地 agent 的安全问题：一旦被攻破，「it basically has the ability to take over whatever the user who signed in on their laptop is」；也难治理：「everyone is configuring their own MCPs, their own skills, their own rules, and if you can't see what's going on, you can't monitor it」
- 讲者说 [01:27]：「the solution to both these problems is to sandbox your agents and move them to the cloud」，新兴的产品类别叫 **cloud software factory**（「this term is kind of overloaded」）

## 3. Cloud software factory 的闭环

- slide（环形图）中心「A cloud software factory」：New product idea or bug reported → Triage → Spec → Implement → Verify → Ship → Monitor → 回到起点；三个人工节点：Reviewing the spec（Spec 后）、Reviewing the code（Implement 后）、Reviewing the product（Verify/Ship 之间）
- 讲者说 [01:47]：「every company is going to set up a loop in the cloud where a combination of humans and agents do work… humans are responsible for putting work in; agents will do different parts of the software life cycle」：[triage] agent 和人一起写 spec、做 implementation、做 computer use verification（转写为 "Friage"）；企业要「define all the rules for how humans and agents interact, and then get to a spot where you can really, really measure and improve over time」
- 讲者说 [02:25]：「getting this concept right is going to be the key driver to how your organization ships software」

## 4. Factory 是基础设施：open、adaptable、defined in code

- slide「Software factories should be built on an infrastructure stack that is **open, adaptable, and defined in code**.」
- 讲者说 [02:25]–[03:32]：「At Warp, our belief is that you should think of this as infrastructure」，不是一个产品或某个团队的东西：
  - open：支持不同模型，「whether it's open-weight or frontier」，支持不同 harness，「the best models and harnesses are constantly changing」
  - adaptable：「the factory should work with your existing infrastructure」
  - defined in code：严格定义「what are the workflows, what are the skills, what are the [MCPs], where do these run」；放进版本控制后「you get the ability to test different versions of your factory… for coding agents to update your factory」
- 资料（「The Factory Stack」）原文：「Software factories should be built on an infrastructure stack that is open, **composable** and defined in code.」；open =「works with any model, agent and hosting configuration」；composable =「you can adopt part or all of the stack」；defined in code =「factory state is versioned, testable, and revertible」，「It's Terraform for factories.」（slide 用 adaptable，博客用 composable，见「出入」）

## 5. AI sovereignty：your data / compute / inference

- slide「Your data」→ 下一张：「Your data / Your compute / Your inference gateway / Any model, any harness」
- 讲者说 [03:32]：「AI sovereignty matters」：拥有进出 factory 的所有数据；agent 跑在自己的基础设施和 compute 上，「This is what most larger enterprises that we work with want」；自己提供 inference；「any model, any harness. So really preserve optionality」
- 资料（「The Factory Stack」）：没有用「AI sovereignty」这个词；相关表述是 data 层「store all data from it on your own infrastructure」「No external provider should get to train on this data」，inference 层说多个模型来源能对冲「risks of just having one model source (e.g. regulation, etc.)」

## 6. Measurement > Vibes

- slide「Measurement > Vibes」，下方三项：「Scoring vs. Self-improvement vs. Benchmarking」
- 讲者说 [04:25]：Twitter 上每天都有人说哪个模型最好（「someone would say like [Pi] is the best or someone would say Grok」，【待确认】"pie" 指什么），也有 SWE-bench、Terminal-Bench 这类 benchmark，但要去的方向是「being measurement first on your own data」：
  - score：「eval how your coding agents are doing」
  - 把分数喂给能改进 factory 的 agent（self-improvement）
  - 自建 benchmark：「take all of your real coding agent tasks and try them against different models and measure how it goes」

## 7. 两种工程师

- slide「Two types of engineers will use your factory:」Product Engineer / Factory Engineer
- 讲者说 [05:22]：product engineering 仍然做功能、修 bug，只是「through the factory」；第二种是 **factory engineering**，「either your platform team or just your general engineers will have to get good at… making the factory better: improving the skills, setting the context, changing the model mix」
- 资料：讲者 9 月那场演讲的标题就是「Software Engineering Is Becoming Factory Engineering」（背景）

## 8. Demo（无录音，只有照片）

录音在 [06:17]「I'm going to show what some of this looks like in practice」之后结束。以下全部是 **slide**（产品界面照片），讲者当时怎么讲的没有记录。

- slide「Let's look at a demo →」
- **Agents 页**（factory `wilson`）：顶部「Configuration is read-only here. Make changes in `warpdotdev/factory-dev` at `main:v1`.」五个 agent：
  - wilson Foreman — 「Orchestrates the factory workflow and dispatches each gated step.」
  - code-review — 「Reviews factory pull requests and routes findings to rework or human resolution.」
  - implementation — 「Implements, validates, and updates code changes.」
  - spec — 「Writes and …」（被挡）
  - triage — 「Triages requests and establishes task state.」
  - 左栏：New Chat、Inbox、Runs、MCPs and apps、Secrets、Integrations、Users；Factories › wilson：Dashboard、Agents、Automations、Scorers、Self-improvement（31）、Benchmarks、Factory definition、Settings
- **foreman 详情页**：「Managed in GitHub」，同样指向 `warpdotdev/factory-dev` at `main:v1`；标签 Prompt / Automations / Settings；Agent resources 里有 Slack (Warp managed)、figma、sentry、staging_maa_debugger，以及 secret `WILSON_SENTRY_AUTH_TOKEN`
- **Scorer「Redundant Tests」**：「Whether the implementation agent's final delivered test diff includes redundant, low-signal, or trivial tests — judging the test artifact, not whether a human later asked to delete it.」All-time scorer cost **$526.24**；Scorer trend（last 30 days，Pass rate）；一条 run 记录：Classification「No surplus tests」，Run time 3min 22sec，Cost $0.26
- **Self-improvement 页**：Approval **13/51，25%**（Approved / Rejected / P[ending]），「Changes grouped by PR or review opened time.」Inbox（Needs review）里的提案标题：
  - Keep Go module files tidy after dependency changes（「A Go dependency update left obsolete go.sum entries and required a review-driven cleanup after CI failed. WARP.md …」）
  - Report only findings and unexamined dimensions in reviews
  - Handle CI failure follow-ups without a plan-only skill
  - Make design snapshot lookup and pre-write locate step satisfiable
  - Trust the du[…]ility judgment（被挡）
  - Read mat[…]menting（被挡）
- **Benchmarks 列表**：Review / Spec / Triage / Foreman Default Model Selection、warp-bench-smoke-test、warp-bench-1.0、warp-bench-1.0-mini、REV-2396 = Team settings…、WarpBench 0.2、UI Computer-Use Impl…、Mini WarpBench…、WarpBench 0.1 等
- **WarpBench 0.1 › 9/2 at 10:45 PM**：散点图 Correctness vs Average cost。「Lowest cost option: glm 5.3 flash, $1.99/trial」；悬停点 **gpt-5.6-sol (high)：Average cost $6.23，Correctness 94.8%**；glm 5.3 flash 约 93.5%；gemini 3.7 flash 约 90.5%；claude opus 5 (hi…) 约 92.5%、成本在 $10 以上（后三个是估读，【待确认】）；底部警告「Some plotted … Use Rescore … to retry unscored tasks」，另有「grok 4…」和「28/29 tasks scored」（被挡）
- **Factories Stack**（Figma 页面「The-Software-Factory-Stack---Zach-Lloyd」）：

| 层 | 组件（slide 原文） |
| --- | --- |
| 06 ACCESS | VIA Factory MCP（Any coding agent）· Comms Platform（Slack · Teams）· Task Tracker（Jira · Linear）· Code Forge（GitHub · GitLab）· Any Device（Web / mobile）· Programmatic（API / SDK） |
| 05 ORCHESTRATION | Native Orchestration（Triage / Spec / Impl / Review / Verify）· Multi-agent（Warp · Claude Code · Codex）· Human in the loop（Live steering + handoff）· Automations（trigger / cron） |
| 04 IMPROVEMENT | Metrics（Autonomy · cost · cycle time）· Scorers（Evaluate completed runs）· Benchmarks（Compare models / skills per task set）· Self-improvement（Automatic observer loop） |
| 03 INFERENCE | Warp-hosted inference（Multi-model · frontier + open-weight）· Bring your own inference（Bedrock · Vertex）· Custom model routing（LLM routing rules · self-improv…） |
| 02 COMPUTE | Sandbox（Docker image · Kubernetes · Any remote env）· Agent runner（Mac / Linux / Windows · configurable disk/ram/cpu）· Comp…/Video（右侧被裁） |
| 01 DATA & CONTEXT | Runs + transcripts（Agent traces）· Agent memory（Vector store）· Agent artifacts（PRs · reports · attachments）· MCPs / Internal knowledge · Acces…/RBAC（被裁） |
| 00 FACTORIES-AS-CODE | Factory.Yaml · Agent Definitions · Automation Definitions · Launch Config (Runne…) |

- slide 结尾：「TRY WARP FACTORIES TODAY」+ 二维码
- 资料（「The Factory Stack」）的七层和 slide 一致（博客没编号，名称相同）：00 Factories-as-code（agent config、runners、repos、automations、integrations、scorers/evals，「all defined in a factory.yaml file」）、01 Data and context（含 audit logs and telemetry、versioned skills、skill/plugin marketplaces）、02 Compute（remote dev environments、computer and browser use for testing、Linux/Mac/Windows）、03 Inference（frontier + open-weight、multiple harnesses、model labs / neo-clouds / hyperscalers / self-hosted、tunable routing）、04 Improvement（cost per PR、cycle times、scorers、replay、benchmarking、self-improvement agents）、05 Orchestration（control plane、subagents、live steering、cloud-to-local handoff）、06 Access
- 资料（「The missing feedback loop for software factories」，Varoon Kodithala，2026-08-26）对 demo 里 scorer 和 self-improvement 的说明：
  - scorer 和 factory 其他部分一样用文件定义，指定要评哪个 agent、0–1 分的 labels、pass threshold（例 0.8；efficiency scorer 用 1.0）、sample rate（**默认 25%**）；run 结束后由一个 cloud agent 按 rubric 给对话打分，返回 pass/fail 和理由。内置 scorer 有 code quality、efficiency、task/procedure compliance、verbosity
  - self-improvement：定时 automation「every few hours」收集新的 scorer 失败，批量交给 self-improvement agent，它在 transcripts 里找根因，对 skills 和配置提 diff（Warp-managed factory 是 review branch，GitHub-managed 是 PR）；要求「make redrafts, not additions」
  - 数字：内部 factory「over 300 PRs shipped in the past week」；一个 efficiency 反例是 agent 为改一行 CSS 用了 275 次 computer-use 操作；几天内 merge 了十多个 self-improvement PR
  - 原话：「Every nudge you give your agents, every review you leave, should flow into concrete improvements to your software factory.」

## Q&A 精确版

没有录音。录音在 [06:33] 结束，demo 之后是否有 Q&A 不知道，【待确认】。

## 勘误表（Whisper 转写 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| hard to cover | hard to govern | 上下文 |
| Friage | triage（agent） | slide（Triage） |
| OpenWade | open-weight | slide（frontier + open-weight） |
| MTPs | MCPs | 上下文、slide |
| sweet bench / terminal bench | SWE-bench / Terminal-Bench | 上下文 |
| two agents (feed those scores and two agents) | to agents | 上下文 |
| our own product work factories | our own product, Warp Factories | slide |
| pie | 【待确认】（可能是 Pi 这个 agent harness，或某个模型名） | — |
| a product or a team-made | 【待确认】 | — |

## 出入

1. **「adaptable」vs「composable」**：slide 和讲者都说「open, adaptable, and defined in code」，讲者 9 月发的「The Factory Stack」写的是「open, composable and defined in code」。含义不同：讲者口述的 adaptable 是「work with your existing infrastructure」，博客的 composable 是「adopt part or all of the stack」。以 slide 为本场原话
2. **「AI sovereignty」** 是现场用词，「The Factory Stack」博客里没有这个词，相关意思分散在 data 和 inference 两层
3. **demo 数字只有照片一个来源**：self-improvement 批准率 13/51（25%）、scorer 累计成本 $526.24、WarpBench 0.1 上 gpt-5.6-sol (high) 94.8% / $6.23、glm 5.3 flash $1.99/trial，都是 staging 环境截图，没有博客或文档佐证，也不知道讲者现场怎么解读这些数字
4. **简介说 metrics 要「beyond "number of PRs shipped"」**，而 self-improvement 博客用的内部规模指标正是「over 300 PRs shipped in the past week」。只记录，不判断

## 【待确认】汇总

1. Zach Lloyd 之前负责 Google Docs/Sheets（只在 prep 里，未核对）
2. Forbes「Uber Burns Its 2026 AI Budget…」原文；VB 72% 那篇未核对
3. 「pie is the best」里的 pie 指什么
4. 「a product or a team-made」原话
5. WarpBench 0.1 图上 glm 5.3 flash、gemini 3.7 flash、claude opus 5 的估读值
6. demo 之后是否有 Q&A
