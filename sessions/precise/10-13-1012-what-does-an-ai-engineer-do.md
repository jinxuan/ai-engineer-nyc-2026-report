# What does an AI engineer do?：精确版

> Precise notes for this session, compiled from the official video captions, the Day 1 livestream captions, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：单独视频字幕逐句读完，和直播切片对过；12 张照片逐张核对 banner，并用原图放大读了小字。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | 作者拍的 12 张（`01.jpg`–`12.jpg`），都是直播画面的手机翻拍，banner 都是「What does an AI engineer do? · Aparna Dhinakaran / Co-Founder & CPO · arize」，全部属于这场。原始文件夹里按时间分进来的头三张 banner 是上一场 Max Kanat-Alexander 的，已归到那场。`06.jpg` 被 YouTube 播放器的控件（暂停键、进度条）挡住了左下角一部分。讲者讲「then and now」（视频 02:18）和「build, eval, ship, monitor, improve」循环（视频 03:13）时的 slide 没有照片；后面几页右上角有 Build / Eval / Ship / Monitor / Improve 五个标签，高亮当前阶段 |
| 视频 | [ImnZ7s68jw8](https://www.youtube.com/watch?v=ImnZ7s68jw8)，改名「What Does an AI Engineer Do Now? - Aparna Dhinakaran, Arize AI」，15:27，上传 2026-10-25。字幕是主要文本。描述带章节（见下） |
| 直播 | Day 1 官方直播 [akEqiXFo3GM](https://www.youtube.com/watch?v=akEqiXFo3GM&t=3616s)，章节「1:00:43 What Does an AI Engineer Do? — Aparna Dhinakaran (Arize AI)」，下一章节 1:28:41。主持人介绍在直播 1:00:16，讲者开口 1:00:44，讲完 1:15:52；之后是场间休息的音乐（字幕只有「Heat」），1:16:01 有一句换场广播。**视频时间 + 1:00:32 ≈ 直播时间**。直播和视频内容一致 |
| 官方 talk 页 | `ai.engineer/talks/ImnZ7s68jw8` 返回 404（2026-10-27） |
| 其他一手资料 | Arize 文档 [Signal](https://arize.com/docs/ax/observe/signal)、[Align evals to human feedback](https://arize.com/docs/ax/evaluate/align-evals-to-human-feedback)；Arize 博客 [From Signal to PR](https://arize.com/blog/from-signal-to-pr/)（2026 年 7 月，作者含 Aparna Dhinakaran） |

视频章节（资料：YouTube 描述）：00:00 Intro · 00:38 Karpathy's funnel: who's running thousands of agents · 01:45 Rising abstraction, from code to agent factories · 02:23 Then vs now: build the loop, not the feature · 03:31 Signal: an always-on agent for your traces · 04:45 Agents are now the main consumers of telemetry · 05:19 What triggers an always-on agent · 06:01 Inside a run: context, sandbox, proxy · 07:11 What persists and what gets torn down · 08:13 Designing durable agents · 09:14 Model and harness agnostic · 10:21 Writing evals, and evaluating the evaluators · 12:48 Trace everything, including your coding agents · 14:17 Closing the gap between labs and businesses

## 基本信息

- 时间地点：2026-10-13（周二）10:12–10:30am，Simons Room (Metropolitan East)，Featured Keynote，Main Stage（资料：[日程](https://ai.engineer/nyc/2026/schedule?session=b2ed4456-5671-4e8e-83fd-684c012fef8c)）
- 讲者：Aparna Dhinakaran，Co-Founder & Chief Product Officer，Arize AI（日程；banner 写「Co-Founder & CPO」）
- 直播 1:00:16 主持人介绍：「As AI takes on more of the building, what does it mean to be an engineer? Here to explore why defining success may become the essential skill, please welcome …」
- 官方简介（日程）：coding agent 已经基本不需要手写代码，当前前沿是消除 code review；agent 掌握 devops、架构、扩展和安全以后，工程师还剩什么？讲者认为剩下的核心技能是定义「good」：知道用户要什么、「done」是什么样子；同时软件里 AI 越来越多、越来越不确定，于是既要擅长定义成功，成功又越来越难衡量
- 我的理解：台上的内容比简介窄，大部分时间在讲 Arize 怎么搭 Signal 这个 always-on agent；「定义什么是 good」只在 eval 一节出现（视频 11:35）

## 1. 开场：Karpathy 的漏斗

- 讲者说（视频 00:12）：在 AI Engineer New York，重新问一次「what does an AI engineer do?」，「every year we have an idea of what an AI engineer does and that definition kind of changes」
- slide 01「1000x Agent Engineer」，左边四层漏斗（从照片读）：

| 人数 | slide 文字 |
| --- | --- |
| ~6B | (~75%) have barely touched an LLM |
| ~1-2B | (~20%) casual, free-tier users |
| ~20M | (~0.2%) professionals in math and code, where an agent finishes weeks of work from a prompt |
| ~5K | (~0.00006%) with internal frontier access, running swarms of thousands of agents |

- slide 01 右边：Andrej Karpathy 推文截图（@karpathy · 20h）：「Dusting off this tweet from April because this gap in shared understanding of LLM capability is "widening". It's now less "two groups of people speaking past each other" and more a sharp funnel.」「Napkin math somewhere around 6B people (~75% of the population) have barely come …」；引用他 Apr 9 的帖子「Judging by my tl there is a growing gap in understanding of AI capability.」下面是 Aparna Dhinakaran（@aparnadhinak）的回复：「I think there is another disconnect happening where labs are tackling a subset of problem types (those with data to solve and testable) and feeling the awesome power. Will the pace of labs show up in business? Big gap from possible & shipped based on problem types.」
- 讲者说（视频 00:33）：Karpathy「just tweeted something this weekend」，讲 frontier 上跑着成千上万 agent、「swarms of always-on agents」的人和其他人之间越来越大的差距；6B+ 碰过 LLM，1–2B 是免费用户，20M 专业人士把几周的活压到几天，「those at the frontier labs … It's like 5,000 users」在同时跑成千上万个 agent
- 讲者说（视频 01:28）：每个人看到这张图的第一个问题是「where am I on this?」，他画出了一条路线：接下来几年怎样成为那 5,000 人之一
- 资料（二手）：Karpathy 这条帖子发于 2026-10-11（周日），复活 2026-04-09 的帖子；四层的标签和百分比与 slide 一致，Karpathy 自称「napkin math」（[报道](https://pasqualepillitteri.it/en/news/23106/karpathy-llm-capability-gap-widening)）。原帖没有直接打开，【待确认】原文措辞
- 出入：Karpathy 原帖第三层按报道是「frontier agents, math & code」，slide 写成「professionals in math and code, where an agent finishes weeks of work from a prompt」；第四层报道是「internal frontier systems」，slide 加了「running swarms of thousands of agents」。讲者口头说第三层是把「weeks down to days」

## 2. 抽象层越来越高

- slide 02「How we got here」（纵轴 AI capabilities，五级台阶）：

| 时期 | 阶段 | 说明 |
| --- | --- | --- |
| 1970s | High-level languages | Frameworks and tests |
| 2022–24 | Chat assistants | Copy code in and out by hand |
| 2025 | Local agents | You approve each step |
| 2025–26 | Cloud agents | Work in the background |
| Now | Factory of agents | Thousands of agents running for hours |

- 讲者说（视频 01:45）：「this has been happening I'd say for a couple of decades now where the abstraction layer has gotten higher and higher」，最近几年更快：从高级语言到复制粘贴代码，到 local agents、cloud agents，「the expectation now is how do all of us start running a team of agents or factories of agents」

## 3. Then vs now：造 loop，不是造 feature

- 讲者说（视频 02:18）：AI engineer 的「then and now」（这页没有照片）：「your job's no longer to just write the feature, write an AI feature, it's about how do you build the loop to write that feature」；不再是读 diff 做 code review，而是搭系统和 agent 来做 code review，写 eval 来审那些 non-deterministic 的任务；不再只是设置 metrics 和 telemetry，而是让 agent 去读 observability 数据、找模式
- 讲者说（视频 03:13）：「this loop … the build, the eval, ship, you monitor, you improve it」，AI engineer 现在负责的「not just for doing each of those steps, but how do you build the loop that … actually does each of those steps」
- slide（后面几页右上角）：Build · Eval · Ship · Monitor · Improve 五个标签

## 4. Signal：读 trace 的 always-on agent

- slide 03「We built Signal to run this loop」：左边三块「Finds issues — In your traces」「Shows the evidence — The runs behind each issue」「Links a fix — A pull request」；右边是 Signal Issues 界面截图（Issues Found 圆环图、Issues Over Time 柱状图、Detected Issues 列表）。截图里第一条 issue 大致是「ROUTER-CHAT eval-analysis sessions accumulate 64–70K token prompts, causing 80–100s RunIterations」（照片小字，【待确认】准确措辞）
- 讲者说（视频 03:31）：「we built signal to actually run this loop for AI engineers」：读所有 trace、识别 telemetry 里的模式，「it can actually put up a fix or a PR if it has access to the repo」；「signal's an always-on agent」
- slide 04「Signal starts with reading traces」：左边一张 trace 表（像是银行客服 agent：「Show me all transactions over $500 this month」「Apply for a personal loan of $10,000」等），三组被标「Signal」；右边三条 issue，都标 High：「Says an action is done without calling a tool」「States balances it never looked up」「Pre-approves without checking the profile」
- 讲者说（视频 04:02）：按可配置的量和频率读一批 trace，目标是从 telemetry 里识别 issue；要记住以前见过的 issue：以前怀疑过但没记下的，数据多了以后可能变成 critical
- 资料：Arize 文档：Signal 是「a built-in worker that scans a tracing project's traces on a schedule」，把反复出现的失败归成 issue，每个 issue 有 overview、trace evidence 和 suggested fix；Enterprise 可接 GitHub repo，让 Signal 开 PR，Free 和 Pro 只有 issue detection（[Signal 文档](https://arize.com/docs/ax/observe/signal)）。博客：「continuously reviews production traces, surfaces ranked issues with evidence and proposed fixes」，「The engineer still makes the call」（[From Signal to PR](https://arize.com/blog/from-signal-to-pr/)）

## 5. telemetry 的读者变成 agent

- slide 05「The future of telemetry」三阶段：Phase 1 Application → Observability → Human dev，「The human is the only reader.」；Phase 2 → Human dev + agent，「Human reads, prompts an agent to write.」；Phase 3 → Agents，「The agent reads it directly.」
- 讲者说（视频 04:46）：「the future of who's actually consuming telemetry data has really changed」，不再是人看，也不只是人看了再交给 agent，而是「how do you set up your telemetry data so that agents are the primary consumers of that data」；observability 数据也成了唤醒 always-on agent 的 trigger

## 6. 什么唤醒 agent，一次 run 里有什么

- 讲者说（视频 05:19）：建 always-on agent 工厂，第一件事是什么让 agent 醒来。Signal 可按固定频率跑（「every 3 hours, 6 hours, go pull the last … X amount of traces」），也可由事件触发（Slack 消息、用户投诉、metric-based trigger）
- 资料：Arize 文档：开启 Signal 会建一个默认每 6 小时跑一次的 automation，频率可从每 3 小时调到每月；也可以用 `ag` CLI 手动触发（[Signal 文档](https://arize.com/docs/ax/observe/signal)）。和讲者说的一致
- slide 06「Anatomy of an agent run」（左下被播放器控件挡住）：左列 control plane：Trigger（Cron, event, click）→ Admit（Quotas, caps, flags）→ 第三块被挡（能读出「…ob / Pro… keys」，我的理解：可能是「Create job / Prompt, model, keys」，【待确认】）；中间「Sandbox (isolated, ephemeral)」由上到下：Task prompt（What to do this run）、Run context（Memory files, tool servers）、Workspace（Repo clone, scratch dir）、Instructions（System prompt, permissions）、一层被挡（能读出「Scope… keys」）、Base image（Runtime, CLIs, skills）；图例「Sandbox layers (bottom = most stable)」；右列 Results（Via tool call）→ Traces（Every turn）→ Teardown（Pod reclaimed）
- 讲者说（视频 06:01）：agent 启动后，拉进 trace 的全部 context、第三方数据（APM logs、APM metrics）、repo 权限、需要的 key，都放进一个 sandbox，每次唤醒都新建；「there's a proxy, the only way to get information in and out of the sandbox is by this single proxy」；sandbox 里发生的一切都被 trace，产出的 issue 存进 memory

## 7. 每次 run 之后留下什么、扔掉什么

- slide 07「What happens after each run」：Run ends（The agent finishes）分两支：Kept, on purpose：Output（The answer or change it made）、Traces（Every turn）、Memory（Files, not chat history）、New skills（Saved as needed）；Thrown away：Sandbox（Torn down after the run）、Credentials（Short-lived, revoked）、Workspace（Repo clone, scratch files）、Leftovers（Caught by an idle sweep）
- 讲者说（视频 07:11）：找到的 issue 存进 memory，「so that the next time an agent gets spun up, it doesn't have to start from ground zero」；agent 本身被 trace（调了什么、是否开了 PR）；run 中学到的 skill「can also get saved to the actual repo」。扔掉的：sandbox 拆掉，credentials 移除，repo 权限移除

## 8. Durable agent

- slide 08「How to keep the agent durable」：左框「Sandbox and harness fails」：Step 1、Step 2、✕ Step 3；「resume」箭头到右框「New sandbox and harness starts」：Step 3、Step 4、Step 5；下方「Saved state, outside the sandbox — Each finished step, the conversation, memory, and which step is next」；箭头标「Saved as each step finishes」「Loaded by the new sandbox」
- 讲者说（视频 08:13）：sandbox 或 harness 挂了，「it shouldn't have to start everything from scratch in the middle of the run. It should actually just be able to pick up from the step that it failed」；状态存在 sandbox 外面，可以起新的 sandbox、新的 harness 接着跑

## 9. Model and harness agnostic

- slide 09「Build model and harness agnostic」：Eval set（Scores every pair）→ Stays put：Knowledge in files（Skills, memory, instructions）、Tools over MCP（Same tools for any harness）、Controls（Sandboxes, permissions, review）→ Swap the harness（Harness A / Harness B，各有 Eval 和 Cost 条）、Swap the model（Model A / Model B）。图例 Evals / Stays put / Swappable
- 讲者说（视频 09:14）：「today signal supports not just Claude Code, which is what we started with, but it also supports Cursor and Codex」，目标是支持主要的 harness，包括开源的；也支持多个模型，比如用 Claude Code 时可选 Anthropic 家族里任一模型。好处：评估 agent 质量；控制成本：「can we reduce the cost and actually still maintain the rigor … and maintain the quality of issues」
- 资料：Arize 文档：Signal 默认跑在 Claude Code 上；Enterprise 可选 Claude Code、Codex 或 Cursor，并配相应的 AI provider（[Signal 文档](https://arize.com/docs/ax/observe/signal)）

## 10. 写 eval，再评估 evaluator

- 讲者说（视频 10:21）：「an AI engineer really needs to kind of learn how to write evals」。Signal 每次找到 issue 开 PR，就有 eval 在 agent 上面跑来测这个 PR；eval 可以很简单：一个 dataset、一个 prompt，测输出防回归
- slide 10「From assertions to evals」（Eval 标签高亮）：

| Dataset（问题 / 被测 agent 的回答） | Output（judge 给的标签和理由） |
| --- | --- |
| Cancel my subscription / Done. It ends on Nov 1. | correct：Matches the cancellation policy. |
| Refund for order 4412? / Refunds take 90 days. | incorrect：Policy says 5 to 7 days, not 90. |
| What is your return policy? / 30 days, free returns. | correct：Matches the return policy. |
| Change my address / Updated to 12 Main St. | correct：Address updated as asked. |

  中间 Eval prompt：「You are grading a support agent. Given the question and the answer, reply with a label and a reason. correct: matches our policy / incorrect: it does not」
- 讲者说（视频 10:55）：eval 还要「aligned with human judgment」，「this is actually probably where an AI engineer should be spending a lot of their time」：和 agent 一起看 trace，找常见错误
- 讲者说（视频 11:18）：「I was hearing Lance yesterday talk about the Claude skill to build evals. We have a skill with the Arize evaluator to actually help you build and write evals as well as align those evals with the human judgment.」「this is probably where the shift to defining what is good becomes really, really important for an AI engineer」
- 资料：「Lance yesterday」指前一天（10-12）Anthropic 的 Lance Martin 的 workshop，讲了 `claude-api` skill 的 `build-eval` 命令（见本仓库 [Unhobbling the Frontier](../10-12-1530-unhobbling-the-frontier.md)）。Arize 文档：「Use the Arize skills plugin with the arize-align-evaluator skill in your coding agent」，报告 accuracy、confusion matrix、per-label precision and recall（[Align evals to human feedback](https://arize.com/docs/ax/evaluate/align-evals-to-human-feedback)）。我的理解：讲者说的「a skill with the Arize evaluator」大概就是这个 arize-align-evaluator，讲者没说名字，【待确认】
- slide 11「You evaluate the evaluator」：

| Case | Human | Judge | 一致 |
| --- | --- | --- | --- |
| Cancel my plan | correct | correct | ✓ |
| Refund for #4412? | incorrect | incorrect | ✓ |
| Return policy? | correct | correct | ✓ |
| Change my address | correct | incorrect | ✗ |
| Close my account | incorrect | incorrect | ✓ |

  右边「Judge-human agreement 80% · n = 5 cases」
- 讲者说（视频 11:45）：很多团队的问题是「how do I trust the evaluator?」，「how do I trust that it's as good as my judgment?」；Arize 自己的做法是「we actually evaluate the evaluators」：建 golden truth 数据集，跑 eval，和人的结果比对、对齐，「make sure that the judge actually agrees with the human … before we actually ship out a new evaluator」；这些 eval 可以 offline 也可以 online 用
- 我的理解：80% 是 5 个示例里 4 个一致的演示数字，不是实际系统的指标

## 11. Trace 一切，包括 coding agent；online eval

- 讲者说（视频 12:47）：agent 建好、always-on、上线前跑 eval；上线后「the goal is to actually trace everything」，「not just tracing the agents, but also tracing the coding agents behind the agents that built this」：可以 trace Claude Code、Cursor、Codex，看清「what happened … in the factory of agents」
- slide 12「Online evaluation」（Monitor 标签高亮）：01 Live traces from every agent run · 02 Evals score every span（高亮）· 03 Failures cluster into patterns · 04 Patterns become eval cases · 05 Back into the factory as a fix；右边是带 Span Evaluations 列（correct / incorrect）的 trace 表
- 讲者说（视频 13:31）：offline evaluation 用来测新改动，上线后变成在 telemetry 上跑的 online evals；这些结果成为新数据，被拉回给 agent 读，找 trace 里的问题；系统就是「collect data, eval that data, run agents on top of it so that you can find problems and figure out where the gaps are」

## 12. 结尾：弥合实验室和企业之间的差距

- 讲者说（视频 14:18）：「there's a pretty big gap between what's happening in the labs today and what's happening with a lot of businesses」，部分原因是「the data inside of businesses are a lot more messy than what is very constrained and testable inside of a lab」；「AI engineers are kind of the role to close that gap」：trace 一切，跑 eval，搭一个让 agent 能从 telemetry 改进的系统，「you're no longer just building individual features, but you're actually building the system to improve your products」
- 我的理解：这呼应 slide 01 上讲者自己对 Karpathy 的回复（labs 只解「those with data to solve and testable」的问题）
- 讲者说（视频 15:13）：展台可以看 Signal，「thank you so much for listening to us」

## Q&A 精确版

没有 Q&A。视频在 15:20 掌声后结束；直播里讲者 1:15:52 说完，随后是音乐和换场广播（1:16:01「The current program has finished its run …」），直到 1:28:41 下一章节。

## 勘误表（YouTube 字幕 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| Aparna Dina Kuran（直播主持人介绍） | Aparna Dhinakaran | 日程、banner |
| Arise AI / arise | Arize AI | 日程、banner |
| Karpathi（直播） | Karpathy | slide |
| signal | Signal（产品名） | slide、Arize 文档 |
| Cloud Code / cloud code | Claude Code | 上下文、Arize 文档 |
| codecs（直播） | Codex | 视频字幕、Arize 文档 |
| the cloud skill（直播） | the Claude skill | 视频字幕 |
| e-vals | evals | 上下文 |
| keep this agent configure durable | keep this agent durable | slide 08 标题 |

## 出入

1. **标题**：日程和 banner 是「What does an AI engineer do?」，YouTube 改名「What Does an AI Engineer Do Now?」
2. **简介和台上内容**：日程简介讲「definition of good」、软件越来越不确定带来的双重挑战；台上大部分是 Signal 的架构，「defining what is good」只在 eval 一节提了一句
3. **Karpathy 漏斗的措辞**：slide 和二手报道转述的原帖措辞不同（见第 1 节）；讲者口头说第三层把「weeks」压到「days」，slide 写「an agent finishes weeks of work from a prompt」
4. **抽象层上升的时间跨度**：讲者说「a couple of decades」，slide 02 从 1970s 开始
5. **Signal 能否开 PR**：讲者说「if it has access to the repo」就能开 PR；文档说开 PR 只限 Enterprise，Free/Pro 只有 issue detection。博客又说 Signal 在 Free 和 Pro 上可用，full Managed Agents（含 repo access）对 Enterprise 是 beta
6. **harness 支持**：讲者说现在支持 Claude Code、Cursor、Codex；文档说默认 Claude Code，三选一只在 Enterprise

## 【待确认】清单

1. Karpathy 2026-10-11 原帖的完整措辞（只看到 slide 截图和二手报道）
2. slide 06 被播放器挡住的两格（control plane 第三格、sandbox 第五层）的完整文字
3. slide 03 截图里那条 issue 的准确措辞
4. 讲者说的「a skill with the Arize evaluator」是否就是 arize-align-evaluator
