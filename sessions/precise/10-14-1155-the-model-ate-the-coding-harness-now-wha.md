# The Model Ate the Coding Harness. Now What?：精确版

> Precise notes for this session, compiled from the official video captions where available, the author's room recording, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材整理完，视频和现场录音逐段对照完。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | 作者拍的 23 张（IMG_0623–0646，缺 0631），全部属于这场。数字都从照片上读过（含放大裁切） |
| 视频 | [7Y4sbZgBdnc](https://www.youtube.com/watch?v=7Y4sbZgBdnc)，改名「Is the Agent Harness Dead? — Jediah Katz, SpaceXAI」，18:14，上传 2026-10-19。字幕 `subs/7Y4sbZgBdnc.txt`。**和现场是同一场**：措辞逐句一致；视频时间 ≈ 录音时间 + 12 秒（视频 00:12 = 录音 00:00） |
| 官方 talk 页 | [ai.engineer/talks/7Y4sbZgBdnc](https://ai.engineer/talks/7Y4sbZgBdnc)：列了两个资源，[Cursor cloud agent 文档](https://cursor.com/docs/cloud-agent) 和 [Cursor is now a part of SpaceX](https://cursor.com/blog/joining-spacex)（页面标注 2026-08-14） |
| 其他一手资料 | [Continually improving our agent harness](https://cursor.com/blog/continually-improving-our-agent-harness)（Stefan Heule、Jediah Katz）；Apple 研究页 [How Much of a Harness Does a Strong Agent Need for Autonomous ML Engineering?](https://machinelearning.apple.com/research/harness-autonomous-ml-engineering)；HarnessTax（UC Berkeley + Arena，[Arena 博客](https://arena.ai/blog/coding-agents-harness-tax/)，正文没抓到，数字靠 slide 和二手报道） |

## 基本信息

- 时间地点：2026-10-14 11:55am–12:13pm，Shannon Room (NY Ballroom E)，Breakout Session (18 mins)，Engineering Track（资料：日程）
- 原题「The Model Ate the Coding Harness. Now What?」，视频改名「Is the Agent Harness Dead?」
- 讲者：Jediah Katz。slide 标题页「Agent Harness, SpaceX Super Intelligence · AI Engineer NYC October 2026」；YouTube 写 SpaceXAI；日程写「Engineer, Agent Harness, SpaceX Super Intelligence」
- 讲者说（视频 00:12）：「I joined Cursor early in 2025 as the longest tenured engineer working on our agent harness and recently I came along to SpaceX as part of the acquisition of Cursor.」
- 资料：日程 bio：Cursor 早期工程师，做 agent harness 的产品、平台和 eval；之前在 Figma 搭 AI evals 平台，在宾大设计并教授 CIS1921（constraint optimization）
- 官方简介：「Jediah Katz discusses how we built Cursor and Grok Bot, and where we think the agent harness is going.」
- 开场小插曲（视频 00:55）：他念了一句「Here's a more natural and flowing version of that script which reads like a cohesive conference talk」，然后说是故意的，「making sure everyone's still got their slop detectors on before lunch」（字幕作 soft detectors）

## 1. 从 prompt engineer 到 harness engineer

- slide：三个头衔斜向排开 Prompt Engineer → Context Engineer → Harness Engineer
- 讲者说（视频 01:09）：2023 年初 prompt engineer 最热；到 2025 年中变成 context engineer，「someone who programmatically puts the best context for the model together on the fly」；「only at the beginning of this year did the industry land on the term agent harness and harness engineering」

## 2. harness 里有什么：runtime 和 context & orchestration 两层

- slide（架构图）：中间 Agent Harness，内层「Context & Orchestration：Prompts · Tools · Memory · Compaction · Model Routing · Guardrails」包着 Models；下层「Runtime：Loop · State · Transport · Auth · Triggers · Observability」；外部连接 Environments（上）、Connectors（下）、Inference（左）、Apps（右）
- 讲者说（视频 01:43）：「The harness is like the eyes and the hands for the model. It wraps around the model, steers it, and connects it to the world.」
- 讲者说（视频 02:10）：runtime「controls where the core agent loop runs and how state is stored so that work can resume」，像传统软件工程；context and orchestration「are kind of like the wild west … control which models are called when and what context they see」，他主要做这一层

## 3. Peak harness，以及「harness 已死」

- slide（Michael Truell 推文，@mntruell，5:12 PM · Jan 14, 2026 · 6.4M Views）：「We built a browser with GPT-5.2 in Cursor. It ran uninterrupted for one week. It's 3M+ lines of code across thousands of files. The rendering engine is from-scratch in Rust with HTML parsing, CSS cascade, layout, text shaping, paint, and a custom JS VM.」「It "kind of" works!」
- 讲者说（视频 02:59）：「In the new year, we kind of hit peak harness」；这个浏览器用的是新的 long-running harness「Grind Swarm」，「a multi-model harness that orchestrated a lot of agents and a lot of machines」，很贵，「it actually took down Cursor maybe once or twice to get this demo」
- 资料：Cursor 的 Wilson Lin 写了 Scaling long-running autonomous coding，项目叫 FastRender；Simon Willison 的报道里任务管理器叫「grind-swarm」，峰值约 2,000 个 agent 并发、跑了约一周（[Simon Willison, 2026-01-19](https://simonwillison.net/2026/Jan/19/scaling-long-running-autonomous-coding/)、[2026-01-23](https://simonwillison.net/2026/Jan/23/fastrender/)）。Cursor 原文没打开，【待确认】
- slide「Is the harness already dead?」四张截图：
  - Thariq（@trq212，Jul 24）「The new rules of context engineering for Claude 5 models」
  - Garry Tan（@garrytan）「Thin Harness, Fat Skills」
  - Cursor 博客「Continually improving our agent harness」（Apr 30, 2026，Stefan Heule & Jediah Katz，11 min read）
  - Xuan Huang · 黄玄（@Huxpro）「Harness is dead」（9:03 PM · Sep 29, 2026 · 13.7K Views）
- 讲者说（视频 03:40）：「Tariq from Anthropic talked about how they had killed much of the system prompt from Claude Code. Garry Tan from YC thought that the harness would fade away and only skills would matter.」他和同事 Stefan 也写过删掉 Cursor 里的 guardrails：以前用一个小模型专门 apply code edits，因为 frontier model 不擅长；还有「how to explore the codebase and how to check your work」这类 coding 专用指令
- 讲者说（视频 04:20）：「if all these people are saying the harness is dead, they were saying that smarter models are what killed it」
- 资料：Cursor 博客原文存在（[cursor.com/blog/continually-improving-our-agent-harness](https://cursor.com/blog/continually-improving-our-agent-harness)），作者和 slide 一致；Thariq 和 Garry Tan 原文没找到，【待确认】

## 4. 证据：简单 harness 在 coding eval 上不输

- slide：「BASH +」一个 `secrets.env`（「everything the agent needs (please don't commit this)」），里面是一排明显是假的 key（`OPENAI_API_KEY=sk-proj-FAKE…`、`ANTHROPIC_API_KEY=sk-ant-FAKE…`、GitHub、Slack、Notion、Stripe、AWS 等）
- 讲者说（视频 04:31）：「a very simple harness where the model only has access to the shell and to all of my secrets. Please don't screenshot this slide by the way. This one's off limits.」现场举手：认为这个简单 harness 能和复杂的一样好的人「A few hands, but most of you know [no]」
- slide 左「UC Berkeley — Harness choice barely moved SWE-Bench-Lite success rate」，柱状图（Success rate %，每个模型三根柱：Pi / Codex / Claude Code，从照片读数）：

| 模型 | Pi | Codex | Claude Code |
| --- | --- | --- | --- |
| Claude Fable 5 | 96.7% | 96.7% | 97.8% |
| Claude Opus 4.8 | 82.2% | 88.9% | 86.7% |
| Claude Sonnet 4.6 | 64.4% | 68.9% | 66.7% |
| Claude Haiku 4.5 | 60.0% | 57.8% | 52.2% |
| GPT-5.6 Sol | 74.4% | 73.3% | 77.8% |
| GPT-5.6 Luna | 53.3% | 55.6% | 55.6% |
| Kimi K3 | 72.2% | 74.4% | （照片截断）【待确认】 |

- slide 右「Apple & EPFL — Dedicated MLE harnesses did not improve MLE-Bench success rate over a minimal one.」配图是 Pedro Domingos（@pmddomingos）的推文「Tl;DR: Harnesses are useless.」（7:12 AM · Oct 8, 2026 · 89.4K Views），引用论文「How Much of a Harness Does a Strong Agent Need for Autonomous ML Engineering?」Kirill Brilliantov、Alejandro Hernández-Cano、Emmanuel Abbé（EPFL、Apple），arXiv cs.AI，30 Sep 2026。高亮句：「under an equal time budget and the same frontier LLM, open-source state-of-the-art harnesses provide no advantages over a single session of a minimal-harness baseline, pointing to the backbone as the primary driver for performance」
- 讲者说（视频 05:05）：Berkeley「tried seven different frontier models on a series of harnesses including Pi, Codex, and Claude Code … particularly on the newer smarter models … the choice of harness did not actually make a difference significantly in the score on SWE-bench Lite」；Apple 和 EPFL「found similarly that a dedicated MLE harness did not make a big difference on MLE-bench」
- 资料：Apple 研究页确认论文题目、作者，结论「the effort spent elaborating hand-crafted harnesses around strong models yields poor returns for current MLE benchmarks」（[Apple ML Research](https://machinelearning.apple.com/research/harness-autonomous-ml-engineering)）
- 资料（二手）：Berkeley 那项研究叫 **HarnessTax**（UC Berkeley 和 Arena，2026-09-16）：7 个模型 × 3 个 harness（Claude Code、Codex CLI、Pi），SWE-bench Lite 和 Terminal-Bench 2.0，每组 30 题 × 3 次；Fable 5 在 Claude Code 里成本约为 Pi 的两倍（$1.33 vs $0.67）；按几何平均，SWE-bench Lite 上 Claude Code 约为 Pi 的 2.0 倍、Codex CLI 的 1.6 倍。来源 [Arena 博客](https://arena.ai/blog/coding-agents-harness-tax/)（正文没抓到）及 [digitalapplied 报道](https://www.digitalapplied.com/blog/coding-agent-harness-cost-harnesstax-same-model-5x)，论文原文【待确认】
- 讲者说（视频 05:45）：「A fancy harness in this day and age no longer adds coding intelligence. I think that is true.」以前在 harness 里补的东西：探索代码库、长时间坚持工作（「things like slash goal」）、跑测试检查自己，「The model has natively absorbed these skills.」

## 5. Data flywheel：harness 让自己过时

- slide（Data flywheel 环）：Harness unlocks capabilities → Agents take on new tasks → Tasks become training data → Model absorbs the ability → Harness deletes the scaffolding →（回到起点）。最后一版标题「The model eats the harness. The harness opens the next frontier.」，起点那段箭头标黄
- 讲者说（视频 06:20）：「the harness is actually making itself obsolete through what I call the data flywheel」：harness 把更难的 coding 任务带进可达范围 → 更多用户用 agent 做这些任务 → 「more coding data is shared with the labs」→ 模型原生学会 → harness 可以删掉当初为此加的东西
- 讲者说（视频 07:12）：「Is the harness really dead? I say no, and there's two reasons why.」
- slide「Why care about the harness?」：1「The harness unlocks the next frontier for the model to solve.」2「Parts of the harness can never be absorbed.」

## 6. 理由一：harness 打开下一个前沿（Grok Bot、software factory）

- 讲者说（视频 07:39）：Grok Bot「is our autonomous colleague who gets work done for you. Not just coding, but all kinds of knowledge work」，「It's already created a new category」
- 讲者说（视频 08:05）：Grok Bot 的 harness 不是基于一次性对话，而是「long-lived coordinator bots that orchestrate a fleet of worker agents」。比喻：请一位高级律师，他直接对接你、负责策略，把活分给一队 paralegal
- 讲者说（视频 08:34）：在 Cursor 是随着模型变强一路删 prompt、删 guardrail；「curiously in Grok Bot, we've actually found ourselves putting back a lot of these things, adding new prompting, adding new guardrails」，因为新领域里很多任务模型还没做好
- slide：「Key website flows / Communication / Acting vs asking / Learning over time」
- 讲者说（视频 09:06）：key website flows 如做 slide deck、订旅行；communication：让你知道它在做什么「without overwhelming you」；acting vs asking：比如给同事发邮件，「you only want us to do it in the way that you intended」；learning over time：通过 memories 学习你的工作方式
- 讲者说（视频 09:36）：这些任务在 coding agent 里都不新，「but for Grok Bot, they're much more important because everybody is needing them」；现在是 harness 让它们可行。回到飞轮：「when a capability like coding gets commoditized by the model, it's actually the harness which unlocks a new frontier for the model to then solve and kicks off that flywheel again」
- 讲者说（视频 10:44）：「This is the core idea that I would like you to understand, this flywheel and how the model and the harness together drive it.」
- 讲者说（视频 10:58）：software factory，多个 agent 协作覆盖整个软件开发生命周期，「far from a solved vision」。内部例子：有人在频道里报 bug → automation 自动 triage → 启动 cloud agent 修，用内部工具录 before/after 视频证明修好 → 合并部署时 Rollouts 产品再启动一个 agent 盯生产环境。「almost like a meta harness, which is orchestrating many agents together」

## 7. 理由二：模型吸收不了的部分（infra、trust、efficiency）

- 讲者说（视频 12:16）：**infrastructure / runtime**：过去一年多，agent 处理一个典型任务的时长从几分钟到几小时、几天甚至更长，「there's just no way that this can work on one person's laptop, which might go to sleep. It has to be running in the cloud」
- slide：一张插画，云上一群小机器人在工作，旁边一个人在床上睡觉（Zzz）
- 讲者说（视频 12:43）：**trust & security**：bash 加一个装满 API key 的文件能解 coding 任务，但「your head of security would have something to say」；需要按 agent、按人收窄的权限；guardrails 是给「anyone who doesn't want to wake up one morning to a really catastrophic action or to a $10,000 API fee」
- slide：三个 macOS 风格的授权弹窗：「Agent would like to access Slack. It will be able to read and post in the channels you choose.」「Agent would like to control this Mac. It will be able to run commands on this Mac. You can turn this off at any time in Settings.」「Agent would like to access GitHub. It will be able to open pull requests in the repositories you pick.」
- 讲者说（视频 13:21）：**efficiency**：前面两项研究里 harness 对分数影响不大，「both of those studies actually found that there was a significant difference in cost per task」。他过去一个月专注优化 Grok Bot harness 的效率：cache hit rate、token efficiency、model mix，「bring down the costs and the speed … without any hit to quality」；「the more and more that you use agents to do your work … the more valuable it will be for you to have the most efficient harness」
- slide（推文截图，数字从照片读）：
  - Jediah Katz（Sep 5）：「The @bot team has been poring over the harness the past 2 weeks and we've found many opportunities to improve our routing, caching, dynamic context usage, and overall token efficiency. Your usage should go much further now. On average 10%, and up to 35% more effective usage for certain heavy users.」引用 Grok Bot（@bot，Sep 5）「We've reset usage limits for all Grok Bot users.」
  - Jediah Katz（Oct 1）：「Over the past week and a half we've made major improvements to make Grok @bot 3x faster.」配图「Grok Bot is fast now」：Time to reply **P50 ↓65%，P90 ↓72%**
  - mr fundman（@mrfundman，Sep 27, 2026，6.9M Views）「WAIT A SECOND. Why is Grok bot so fast today??」；Nate Esparza（Oct 1）「feel like grok bot is fast AF now」

## 8. 接下来的 harness：三个预测

- slide「Designing to be deleted」：「Show users outcomes, not harness inner workings (Hyrum's law)」；「Design extensible primitives for agents」
- 讲者说（视频 14:27）：「that flywheel is speeding up, and right now we've entered a new age where the product is once again moving faster than the model」，所以 harness「is going to get more messy before it gets cleaned up」，要设计得容易 ship 和 unship。Grok Bot 等产品越来越少暴露 harness 内部给用户；Hyrum's Law：「anything that you expose to users, they will come to depend on」；代价是可见性降低，好处是「less change management … you can iterate much faster」
- slide「Designing agent primitives」：左边五种事件 Email to the bot / Slack mention / PR review / PagerDuty alert / Weekday 9am → 中间 **Subscriptions**（Permissions · Route to the right agent · Dedupe · Filter out noise · Wake the agent）↔ agent：`subscribe(trigger)`、`wake(event)`
- 讲者说（视频 15:43）：primitive 例子 subscriptions：agent 需要被事件唤醒（Slack 消息、PR 的 CI 变红、某个时间点），「rather than having separate integrations for each of these things, we have one framework and one tool that … the agent can use to subscribe to an event」
- slide「Firings by shim」：一张随时间（22:45–23:30）的堆叠柱状图，图例里能读到的 shim：`await_none_task_id`、`await_task_id_alias`、`await_terminal_path_alias`、`copy_to_box_computer_pa…`、`dynamic_tools_stringified_argume…`、`legacy_update_state_forwa…`、`mcp_argument_envelope`、`mcp_missing_required_arguments`、`mcp_schema_key_spelling`，另有「+16」
- 讲者说（视频 16:12）：确定性 guardrail 用来补模型的坏行为，比如 Grok Bot 回复用户不够快时推它一下「Hey, user's waiting on you」。「we'd rather not do these things. They're kind of like hacks, but we would rather fix them now than … wait for the next model checkpoint」。这些叫 **shims**，集中登记，每次触发都记日志；有一个 bot 盯 dashboard，「whenever a shim stops firing, that model or that agent will put up a PR and say this shim can be cleaned up」
- slide「What's next for the harness?」：1「Harnesses will be designed to be deleted.」2「The harness moves up a level: agents are the new building blocks.」3「Within 6 months, 90% of agent work runs off the laptop.」
- 讲者说（视频 16:58）：上一代 harness「built from composing a bunch of LLM calls」，新的（Grok Bot、software factory）「are using agents as the core building block」；「I predict that within 6 months, 90% of agent work is going to run off the laptop」

## 9. 结论

- 讲者说（视频 17:36）：「is the harness dead? The parts that made it smarter at coding, yes, that is the flywheel at work.」但 harness 是 agent 打开下一个前沿的方式，trust、efficiency、infrastructure「those things are not going anywhere」。「yesterday's harness may be dead, but long live the agent harness of tomorrow.」

## Q&A 精确版

没有 Q&A。现场录音在「thank you」和掌声后结束（录音 18:00 起），视频也在 18:08 掌声处结束。18 分钟的 breakout 没有留提问时间。

## 勘误表（Whisper / YouTube 字幕 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| Jedidiah（视频字幕） | Jediah | slide、日程 |
| asian harness | agent harness | 上下文 |
| state of storage that work can resume（录音） | state is stored so that work can resume | 视频字幕 |
| multimodal harness（视频字幕） | multi-model harness | 录音转写、上下文（编排多个模型和 agent） |
| Grind Swarm | grind-swarm（FastRender 项目的任务管理器） | Simon Willison 报道 |
| Cloud Code / SweetBench light / MLEBench | Claude Code / SWE-bench Lite / MLE-bench | slide |
| Tariq | Thariq（@trq212） | slide |
| Gary Tan | Garry Tan | slide |
| soft detectors / slop detectors | slop detectors | 录音转写、上下文 |
| Glockbot / GrockBot / Crock-Bot / RockBot / Grok Bots | Grok Bot | slide |
| five-wheel / final at work（录音） | flywheel at work | 视频字幕 |
| shin（录音） | shim | slide「Firings by shim」 |
| Hiram's law | Hyrum's law | slide |
| cash hit rate | cache hit rate | 上下文 |
| you can't iterate much faster（录音） | you can iterate much faster | 视频字幕 |
| far from a solid vision（录音） | far from a solved vision | 视频字幕 |

## 出入

1. **「两层」还是更多**：讲者说把 harness 分成 runtime 和 context & orchestration 两层；slide 架构图还画了 Environments、Connectors、Inference、Apps 四个外部连接，讲者只口头提到 apps、computers、MCPs
2. **浏览器 demo 的数字**：slide 上 Michael Truell 推文是「3M+ lines of code across thousands of files」、一周不间断；Simon Willison 转述 Wilson Lin 的博客是「over 1 million lines of code across 1,000 files」（二手，Cursor 原文没核对，【待确认】哪个是最终数字）
3. **「harness 不影响分数」的强度**：讲者说「did not actually make a difference significantly」，slide 写「barely moved」。从照片读的柱状图里，Claude Haiku 4.5 是 Pi 60.0% vs Claude Code 52.2%（差 7.8 点），Opus 4.8 是 82.2% vs 88.9%（差 6.7 点），差距在较弱/较旧的模型上更大；讲者口头也限定了「particularly on the newer smarter models」。二手报道说 SWE-bench Lite 上 harness 效应约 ±2 点，和照片读数不一致，【待确认】（我的理解：可能指的是平均值）
4. **slide「can never be absorbed」vs 口头「can't absorb」**：slide 用了「never」，口头没有
5. **效率提升的数字**：讲者只说「significantly … without any hit to quality」；具体数字只在 slide 的推文里：用量平均多 10%、重度用户最多 35%；速度 3x，P50 回复时间 −65%、P90 −72%
6. **讲者单位的叫法**：slide 和日程是「SpaceX Super Intelligence」，YouTube 是「SpaceXAI」，官方 talk 页的资源链接仍是 cursor.com

## 【待确认】清单

1. 柱状图里 Kimi K3 的 Claude Code 数值（照片截断），以及是否正好 7 个模型（照片里能看到 7 个）
2. HarnessTax 论文原文（作者、arXiv 号、±2 点的说法），Arena 博客正文没抓到
3. Thariq「The new rules of context engineering for Claude 5 models」原文和日期年份（slide 只写 Jul 24）
4. Garry Tan「Thin Harness, Fat Skills」原文
5. 浏览器 demo 的代码行数（3M+ vs 1M+）
6. Grind Swarm 的官方写法（Cursor 原文未核对）
7. 「slash goal」指的是哪个产品的 `/goal` 命令（字幕「things like slash goal」，没有展开）
