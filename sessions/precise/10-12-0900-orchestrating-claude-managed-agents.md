# Orchestrating Claude Managed Agents：精确版

> Precise notes for this session, compiled from the official video captions and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in the video), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：官方视频字幕逐段整理完，产品事实和 Anthropic 文档、release notes 对过。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | 没有照片。slide 内容只能从讲者口述（「on slide」「on screen」「in our comparison chart here」）和视频描述还原 |
| 视频 | [kiClTcJUnhc](https://www.youtube.com/watch?v=kiClTcJUnhc)，改名「Why One AI Agent Isn't Enough (And How to Run 1,000) - CJ Avilla & Harrison Stall, Anthropic」，1:41:43，上传 2026-10-16。字幕是 YouTube 自动字幕。视频描述带章节（见下），整场两段 Q&A 都在视频里 |
| 官方 talk 页 | [ai.engineer/talks/kiClTcJUnhc](https://ai.engineer/talks/kiClTcJUnhc)：列了两个相关 talk 作为资源（「Claude for long-horizon tasks」；Jim Clark, Docker「How Many Credentials Should Your AI Agent Have? Zero.」），没有 slide 或代码链接 |
| 会后报道 | Latent.Space 的 [How Anthropic's Dynamic Workflows Coordinate Claude Agents](https://latent.space/p/claude-dynamic-workflows)（原链接 swyx.substack.com/p/claude-dynamic-workflows 跳转到这里），署名 Richard MacManus，2026-10-12。只作交叉核对 |
| 一手文档 | Claude Platform 文档：[Multiagent orchestration](https://platform.claude.com/docs/en/managed-agents/multiagent-orchestration)、[Workflow runs](https://platform.claude.com/docs/en/managed-agents/workflow-runs)、[Release notes](https://platform.claude.com/docs/en/release-notes/overview)、[API credits for Max and Team plans](https://platform.claude.com/docs/en/about-claude/api-credits-for-subscribers)；Claude Code 文档 [Dynamic workflows](https://code.claude.com/docs/en/workflows) |

视频章节（资料：YouTube 描述）：0:00 Unix vs. OS/360 · 6:00 Meet CJ & Harrison · 7:00 Introducing Flows · 9:05 Why single agents hit a wall · 14:00 How dynamic workflows work · 17:25 Demo: stock watchlist scanner · 23:20 From console to production · 32:15 Q&A round 1 · 1:04:45 Bug Hunter · 1:07:15 Explainer Studio · 1:12:15 Laziness, self-bias & goal drift · 1:15:20 Agent safety at scale · 1:20:05 Declare, contain, detect · 1:21:15 Separating the brain from the hands · 1:27:50 Credential vaults & locking down tools · 1:31:45 Wrap-up + Q&A round 2

## 基本信息

- 时间地点：2026-10-12（周一）9:00–11:00am，Sutton Room (NY Ballroom W)，Workshop (1-2 hrs)（资料：[日程](https://ai.engineer/nyc/2026/schedule?session=c415d432-5b6f-4b4a-9d2c-2e02cfde99db)）
- 原题「Orchestrating Claude Managed Agents」；官方简介：「Dynamic workflows in Claude Managed Agents, and the controls that keep them in bounds.」
- 讲者：CJ Avilla、Harrison Stall，都是 Anthropic 的 Member of Technical Staff（日程、讲者说 06:00）
- 分工（我的理解，按字幕的换人标记和自称推断）：CJ 讲开场历史、从 console 到生产和分阶段调资源、Bug Hunter、Explainer Studio、单 agent 的三个毛病、budget / interrupt / 审查 agent / auto mode、收尾；Harrison 讲 flows 介绍和 console demo、安全六项、declare/contain/detect、brain 与 hands、vault 和工具/网络收紧。Q&A 两人轮流答，个别回答说话人不确定

## 1. 开场：Unix、OS/360 和「Linus doesn't scale」

- 讲者说（CJ，视频 00:12）：1969 年 Ken Thompson 在 Bell Labs（「just about 30 miles from here」），妻子带一岁的儿子去加州探亲一个月，他把项目切开，每周做一样：kernel、editor、assembler，再加「one other thing」，月底有了 Unix。重点是「conceptual integrity」：「one person can hold the entire project in their head and execute the entire project end to end」
- 我的理解：Thompson 本人的说法通常是 OS、shell、editor、assembler 各一周（三周）；CJ 记不起的第四样应是 shell。没找一手出处核对，【待确认】
- 讲者说（视频 01:05）：同一时期 IBM 做 System/360 和 OS/360，以前每次升级硬件就要重写软件；IBM「bet the company」，估算约 5,000 man-years，「throw as many as 1,000 engineers」。Fred Brooks 把教训写成 The Mythical Man-Month
- 讲者说（视频 02:15）：书里的一个结论是「surgical team」：一个 chief 周围约 10 个专家（language lawyer、sidekick、co-pilot……），surgeon 一人保持 conceptual integrity；「we also want to take that same sort of structure and scale it up much further」
- 讲者说（视频 02:50）：Linux 7.2 不久前发布，开发周期九周，「contributions from 2,600 something different developers across 300 different companies」
- 资料：LWN 统计 7.2 有 2,652 位开发者提交了代码（创纪录），7.2 于 2026-08-17 发布（二手汇总，[LWN](https://lwn.net/Articles/1088776/)）。「300 家公司」和「九周」没核到，【待确认】
- 讲者说（视频 03:16）：1998 年 kernel 邮件列表上 Linus「kind of having a bit of a breakdown」，收到「I've emailed you three times with my patch」之类的邮件，说要去度假；第二天 Larry McVoy 发帖「The problem is that Linus doesn't scale」，并推销版本控制（「pushing this commercial product」）；约四年后（2002）Linus 承认「I don't scale」，从 star 结构变成有「layers of trust」的网络，contributions 交给 trusted maintainers 审，只有例外情况上升，Linus 仍是最终 commit 到 main 的人
- 资料：Larry McVoy 1998-09-30 在 linux-kernel 发帖「A solution for growing pains」，原句「The problem is that Linus doesn't scale.」（[LKML 存档](https://lkml.rescloud.iu.edu/9809.3/0957.html)）；他推的是后来的 BitKeeper（商业软件）
- slide（讲者口述，视频 05:06）：左边 star 结构「like 36 different actors」都直接向一个人贡献；右边分层结构「at most four people at each level」
- 讲者说（视频 03:43）：类比今天：「a lot of us who are orchestrating and managing lots of agents now might start to feel this feeling of the being the bottleneck」，坐在 GitHub 里批一堆 agent 提的 PR
- 讲者说（视频 05:22）：AI Engineer San Francisco 上「Tarek and Gary」说「We have Fable … it's time to be unreasonable」，Gary 说「it's time to boil the ocean」。我的理解：指 Anthropic 的 Thariq 和 YC 的 Garry Tan；具体是哪场 talk、原话【待确认】

## 2. Flows：Claude 自己编排多 agent 拓扑

- 讲者说（Harrison，视频 06:56）：「on Friday」在 Claude Managed Agents（「shipped back in April」）里发布了 flows，「the next version of multi-agent orchestration, where you can let Claude fully orchestrate the entirety of its own multi-agent topology」
- 资料：release notes：Claude Managed Agents 2026-04-08 beta 发布；2026-10-09（周五）「You can now let a Claude Managed Agents agent use dynamic workflows, in beta with the `managed-agents-2026-04-01` beta header」（[release notes](https://platform.claude.com/docs/en/release-notes/overview)）。文档里的正式名字是 **dynamic workflows** / workflow run，讲者口头一直叫 flows
- 讲者说（视频 07:37）：很多人熟悉 Claude Code 里的 dynamic workflows：说「use ultra code」，Claude 就起一张 agent 网，底下是「a really interesting JavaScript engine」，支持 map-reduce 式活动；不用自己画「classify-and-act or fan out and synthesize adversarial verification style」拓扑，Claude 靠训练和自身能力知道怎么编排
- 资料：Claude Code 文档：workflow 是 Claude 写的 JavaScript 脚本，在 prompt 里写关键词 `ultracode` 或说「use a workflow」触发；本地默认最多 16 个并发 agent，每个 run 最多 1,000 个 agent（[Claude Code workflows](https://code.claude.com/docs/en/workflows)）
- 讲者说（视频 09:01）：「hand waved numbers here」：single agent 跑约一小时就「keels over」；coordinator + roster（Managed Agents 的第一版多 agent，一个 coordinator 下一层 agent）能跑久一点；dynamic workflows 支持「arbitrarily complex nested sets of agents operating all in parallel」。slide 上有对比（具体数字只说是示意）
- 讲者说（视频 10:00）：从用户那里听到的共同痛点：自己手搓多 agent harness；处理 container restart（「what if one of your agents out of a hundred dies … two hours into the task」）；一个 agent 管「60 as an example agents」，像 Linus 一样不 scale；不知道怎么看清整张 agent 图的状态
- 讲者说（视频 11:32）：Opus 4.5 之后那个 meme：人们半开着笔记本走路怕 Claude Code 断掉，在公园里一直按「approve approve approve」；「that's just not going to scale for our ambitions」
- 讲者说（视频 12:20）：现场举手，约一半人听说过 Claude Managed Agents。它是让 Anthropic 管理 agent 生命周期的 API：定义 agent（skills、tools、MCP servers 等），在 session 里实例化，状态化的 API 资源加 durable event log 实时监控
- 讲者说（视频 13:15）：「back in I think it was May」发布了「multi-agent V1」：要预先定义每个 sub agent，还要监听每个 agent 的 thread；像操作系统的进程和线程，共享状态要靠互发消息，「wasn't the most trivial thing to set up」
- 资料：release notes 2026-05-06：Multiagent orchestration 和 Outcomes 进入 beta。和讲者说的五月一致
- 讲者说（视频 14:05）：flows 的工作方式：lead agent 先做计划（例：「migrate my million line mono repo from Python to Rust」），然后自动 fan out 到它自己起停的 agents，分多个 phase 完成「discrete, verifiable units of work」，不用你管 agent 之间的连接。例：phase 1 读文件（一堆 explorer agents），phase 2 分析……
- 讲者说（视频 15:36）：运行时可以看单一 event stream 里所有 agent 在做什么，也能逐个 agent 看；开跑前给 session 设 budget；不满意方向可以通过 API interrupt
- 讲者说（视频 16:18）：和 Claude Code 的区别：这是 API surface，可以嵌进自己的应用、内部流程、CI；slide 上有对比表，CJ 强调常见用法是按 schedule 跑、监听 webhook，例如 GitHub 上的 bug hunter workflow
- 资料：文档：`multiagent` 块用 `multiagent_20261001` 类型，subagents 和 workflows 默认都开；只有 primary thread 的 agent 能启动 run，「runs don't nest」；一个 run 生命周期内最多 1,000 个 agent，同时最多 64 个 thread（API 不保证），默认 run 寿命 24 小时（[Workflow runs](https://platform.claude.com/docs/en/managed-agents/workflow-runs)）

## 3. Demo：股票 watchlist scanner（console）

- 讲者说（Harrison，视频 17:55）：Claude Console 左侧「Managed agents」下拉菜单 → Quick start，模板列表最底下有「dynamic workflow」标签；「because I recognize there is a … bunch of finance oriented folks in the room」，选「stock watchlist scanner」模板
- 讲者说（视频 18:52）：模板 config 建一个 agent：模型 Claude Opus 5.5，类型「multi-agent 2026-10-01」（即 `multiagent_20261001`），加一段定义 persona 的 system prompt；点「use this template」就在后台建 agent 资源
- 讲者说（视频 19:25）：agent 需要一个 environment，「a computer essentially for Claude to operate on」；「for the last … 10 or 12 months … giving Claude access to a computer makes it way way way more capable」。他用早上建好的「stock watchlist scanner env」，点 start a session
- 讲者说（视频 20:09）：输入 Apple、Nvidia、Amazon、JPM、XOM、COST，回车开 session。Sessions 面板里：agent（「the person at the top of the pyramid, the CEO」）、environment、实时 run；「the session thread … the single context window, the single instantiation of an agent」并行起来
- 讲者说（视频 22:17）：打开早先跑完的一次：「in this case it was only like six concurrent agents or so at each step」，Claude 按任务复杂度增减 agent 数

## 4. 从 console 到生产：分阶段投入算力

- 讲者说（CJ，视频 23:28）：console 用来原型；满意后集成进自己的应用：比如每个工作日开盘前 5 分钟按 schedule 跑 scanner，或从 Slack 触发
- slide（代码，讲者口述，视频 24:17）：创建 agent 时传 multiagent config；给 lead agent 的第一个 event 写「run a workflow」，「that's all you need to do to get flows up and running」
- 讲者说（视频 24:46）：默认 Claude 临时生成需要的 sub-agents；也可以在 agent config 里传预配置 subagents（例：专看 Yahoo Finance、新闻源、SEC EDGAR 的）；agents 可以用 custom skills、MCP servers、在 environment 里跑的 custom tools；environment 可以自带 sandbox，「self-hosted environment in your own infra」
- 资料：文档：预定义 agent 写在 `workflows.predefined_agents`（最多 20 个）；self-hosted sandboxes 2026-05-19 发布
- 讲者说（视频 26:11）：单 agent 做 scanner 会逐个看 ticker；flows 把列表拆开，每个 ticker 交给一个 scan agent，「an isolated context where all it cares about is its one ticker」，不受其他 ticker 和搜索结果影响；然后 review 阶段也是独立 context
- slide（对比图，讲者口述，视频 27:23）：同一个 scanner 用单 agent 和 flows 各跑一次：右下 flows「costs much more」，中间一栏「much more effective at finding all of the details」。具体数字没念，【待确认】
- 讲者说（视频 28:10）：可以让 Claude 在不同 phase 投不同资源：scan 阶段用 Haiku（「a really fast, efficient model」），每个 ticker 两个 Haiku agent；review 阶段用 Fable，「two or three agents per ticker」；「expand and contract based on how much you want to invest in that point in the process」；重要任务建议配一套完整 eval
- 讲者说（视频 29:43）：模板也打包进了 Claude Code 和 Claude API skill：周日晚之后装的 Claude Code 可以跑 `/claude-api managed-agents-onboard watchlist-scanner`（字幕「Claude API managed agents on board with watch list scanner」），用另一个 CLI「Ant CLI」驱动 Claude API；CJ 自己常用 Claude Code 而不是 console 建 managed agent
- 讲者说（视频 30:46）：试下来，初始 prompt 越开放越好：「use a workflow one ticker per agent … This is what I'm kind of looking for」，让 Claude 先搭 flow 再微调
- 讲者说（视频 31:25）：屏幕上的 QR code 送约 $50 API credits；本周新宣布 Max 和 Team 订阅附带 API credits：「Max 5X, you'll get like $100 a month」，「Teams is up to 500」
- 资料：文档：Max 5x 每月 $100、Max 20x $200、Team 共享池最多 $500；覆盖 Claude API 和 Managed Agents，不覆盖 Claude Code；release notes 2026-10-07 公布（[API credits](https://platform.claude.com/docs/en/about-claude/api-credits-for-subscribers)）

## 5. Bug Hunter（第一轮 Q&A 之后）

- 讲者说（CJ，视频 1:03:46）：上一届 AIE 的一个主题是 loops：「How can we close these loops」让系统更自主。Bug Hunter 是 Anthropic 自己在用的 flow，模板也在 Claude Code 里
- 讲者说（视频 1:04:14）：流程：提 PR → Bug Hunter flow 把代码切成 slices，每个 slice 交给独立 context 的 agent 列「this might be a problem」→ 第二阶段 reviewer 对同一 slice 表态同意或不同意 → 一个阶段挑出可能真实的候选 → **prover phase**：agent 下载 repo、尝试复现 bug → 输出报告；实际使用时是 PR 上的 inline comment，并标严重程度。人类 reviewer 的价值是知道 PR 没提到的「distant corner of the code base」，agent 可以把这类问题找出来
- 讲者说（视频 1:06:58）：启动命令 `/claude-api managed-agents-onboard bug-hunter`（字幕「Cloud API scale manage agents on board Bug Hunter」）
- 资料（二手）：AlphaSignal 报道同一模板和命令，并引 Anthropic 的合成测试：116,000 行代码植入 70 个 bug，单 agent 三次找到 14/15/27 个，dynamic workflow 三次都找到 66 个（[AlphaSignal, 2026-10-09](https://alphasignal.ai/news/anthropic-s-claude-now-orchestrates-1-000-agents-finding-66-of-70-bugs)）。讲者没提这组数字，Anthropic 原文没找到，【待确认】

## 6. Explainer Studio：用 flow 做动画视频

- 讲者说（CJ，视频 1:07:09）：举手问谁关注 @ClaudeDevs，「not enough of you」。新产品发布配的动画讲解视频很多是 flow 生成的：research（查清细节）→ plan、scout、fact-check → design brief → production（逐场景制作、动画）；实际还有音效、配音、脚本等阶段
- 讲者说（视频 1:08:20）：CJ 帮着管 Twitter 账号，「4 years ago … would have taken like … 20 hours」，现在是「one very small prompt」。命令 `/claude-api managed-agents-onboard explainer-video-maker`（字幕「Explainer Video Maker」，模板名【待确认】）
- 演示（视频 1:09:02）：放了一段讲 dynamic workflows 的生成视频：「what if the task has too many pieces for one agent?」例子是 300 份合同里找 change of control 条款；画面讲 session threads、怎么用 multiagent 参数开启、要设 budget、budget 可追加。旁白是 CJ 自己的声音
- 现场（视频 1:10:28）：投票看「how a toilet works」还是自行车，toilet 小胜；视频带 Claude 品牌 lockup 和 Anthropic 字体。CJ 推荐用 Claude Design（claude.ai/design 或桌面端）做品牌设计系统，再交给 flow
- 讲者说（视频 1:10:15）：YouTube 的 Claude 频道有 Claude FM 之类的直播（「nice autumnal kind of like vibe music」）

## 7. 单 agent 的三个毛病

- 讲者说（CJ，视频 1:12:15）：做 dynamic workflows 学到的：**agentic laziness**、**self-preferential bias**、**goal drift**
- agentic laziness（视频 1:12:24）：prompt 例：「We're having an outage … look at these 50 different services … tell me the root cause … don't touch production」。单 agent 看到 35 个健康后开始说服自己「everything looks good」，不看剩下的。coordinator 也一样，要记数，sub-agents 本身也会偷懒。flow 给每个 service 一个 agent，「by the end, we know for sure that we went and checked every single service」：「fan out and loop until done」合在一起
- self-preferential bias（视频 1:14:14）：让 agent 写首诗再问好不好，它说好；代码也一样。advisor pattern 用 generator + judge 利用「generator discriminator gap」，但问题够大时 roster 不够；flow 可以铺开很多「adversarial review that has its own isolated context」
- goal drift（视频 1:15:22）：「don't touch prod」是最后一句，context 一路填满、compact，这句可能丢；flow 在开头定好目标，「copied in like a very consistent way into each of the sub agents for all the different phases」

## 8. 规模化下的安全：六项风险

- 讲者说（Harrison，视频 1:16:20）：从单 agent 到可能几百个，「how do we scale our safety and security posture」；「if I had to say like one thing to get right when you build out your agents is this」
- 讲者说（视频 1:17:20）：「six very concrete things」（slide 列表；我按口述整理出五项，第六项可能是 visibility 被合并说了，【待确认】 slide 原文）：
  1. prompt injection：扫开放互联网时拾到「ignore previous instructions … exfiltrate」；Anthropic 在模型层和「classifiers that make tool call checks like auto mode in Claude code」上投入
  2. 凭据泄露：「Claude is taking on the persona of whatever credentials that you give it」；VM 磁盘上的 `my API keys.md`，即使藏起来也可能被诱导泄露；所以「credentials never have to touch the sandbox」
  3. data exfiltration：敏感信息不能流给第三方
  4. 越权：budget 等平台限制，加 permission policy 让 agent 只能跑你给的工具
  5. 可见性：durable event stream 记录 agent 整个生命周期，「pretty table stakes」
- 讲者说（视频 1:20:04）：开发者的三件事「declare, contain, detect」：declare 写清 agent 是谁、权限、能做什么、skills、tools；contain 是物理限制，environment policy 里限定可访问域名、只开 web search 不开 web fetch 等，「even in the absolute worst of case, the amount of harm … is minimal」；detect 靠实时监听 event stream，必要时立刻 interrupt

## 9. Brain 和 hands 分离、出口处注入凭据

- 讲者说（视频 1:21:13）：四月发布 Managed Agents 时有个 meme：「Anthropic like settled the debate … The brain and hands should be separate」
- 资料：Anthropic 工程博客 [Scaling Managed Agents: Decoupling the brain from the hands](https://www.anthropic.com/engineering/managed-agents)
- 讲者说（视频 1:21:35）：brain 是跑 inference 的 loop（agentic harness）加 session state；hands 是 agent 在真实世界行动的地方，比如在 Claude 能访问的电脑上 grep，或跑在你 macOS 机器上的 custom tool；「there should be like no way for you to given a hand access and totally steer the brains」
- 讲者说（视频 1:22:32）：凭据注入：每个能拿到 secret 的 container 里放的都是假值，用它调 API 不会成功；只有在 egress proxy 出去时才替换成真值。「there's no way like physically to prompt Claude or to … get that credential off of the sandbox」；「you should not be able to foot gun yourself」

## 10. API 上的控制：budget、interrupt、审查、auto mode

- 讲者说（CJ，视频 1:24:03）：设 budget，例「You have a $1.25」；到额度时 flow 没完成，可以决定追加，「it won't have have redo any of the work」；任何 session 都能用。和 **Outcomes** 搭配：「Here is the outcome that I want and the rubric that I expect, and here is a budget」
- 讲者说（视频 1:25:03）：slide 上第二段代码：发 `user.interrupt` 事件「will stop the flow」，然后可以重新 prompt。建议练习：把今天跑的 flow 设一个低于预期的 budget（比如一美元），跑、加额度、看它继续
- 资料：文档：budget 在创建 session 时设（`max_list_cost`，单位为美分字符串）；到额度时所有 open run 暂停，session 以 `budget_reached` 进入 idle，提高或去掉 budget 后恢复（release notes 2026-08-07 发布 budget）。**`user.interrupt` 只停 agent 当前 turn，「It ends no run」**；要停 run 得发 `user.message` 让 agent 停（[Workflow runs](https://platform.claude.com/docs/en/managed-agents/workflow-runs)）。见出入 2
- 讲者说（视频 1:25:42）：可以起一个审查用的 managed agent：用 API 拉 durable log 的全部 transcript，「tell me if my agents did anything that was unexpected or accessed anything that was they weren't supposed to」
- 讲者说（视频 1:26:42）：「easiest lowest-hanging fruit」：把 permission policy 从 always ask 改成 **auto mode**。理由：人在公园里狂按 approve，「like 97% of the time we're going to click approve as humans」；Claude 更有耐心，逐个判断工具调用是否安全或有破坏性
- 资料：Claude 博客「Auto mode is now the default in Claude Code for Pro, Max, and Team plans」（2026-08-07）：「users approve 97% of permission prompts in Claude Code」；受控研究里人工审查拦下 13.6% 的危险命令，auto mode 拦下 89%（[claude.com/blog](https://claude.com/blog/auto-mode-default-in-claude-code)）。Managed Agents 的 permission policy 2026-09-10 加入 `auto`（release notes）

## 11. Vault、最小工具集、网络白名单

- 讲者说（Harrison，视频 1:27:50）：例子是通过环境变量用 Twilio API。naive 做法：上传带 Twilio key 的 `.env`，agent 在 Python 脚本里直接调，问题是明文。做法：把 key 存成 vault credential（environment variable 类型）；`injection_location` 指定出口时 secret 出现在哪里（bearer auth header 还是请求体）；networking 里的 allowed hosts 规定这个凭据只能替换给哪个域名，「we don't want like our Twilio API key to ever go out to … mydomain.com」；创建 session 时传 vault，平台在 egress 层把占位符换成真值
- 资料：release notes：2026-06-09 vault 支持 environment variable credentials；2026-06-30 加 `injection_location` 设置
- 讲者说（视频 1:30:02）：给 agent 最小工具集：agent toolset 里的 web fetch、web search 可以 `enabled: false`；MCP 工具事先不知道会返回什么，「what if … an attacker added like, you know, my exfiltration tool」，所以按工具逐个开启
- 讲者说（视频 1:31:01）：environment 配置里限制网络：只允许 `api.twilio.com`，调别的网站「would like physically fail at the … firewall layer」
- 资料：文档：`limited` 网络的 environment 用 `allowed_hosts`；agent 声明的 MCP server 不在 allowed_hosts 里时创建 session 返回 400（[Multiagent orchestration](https://platform.claude.com/docs/en/managed-agents/multiagent-orchestration)）

## 12. 收尾

- 讲者说（CJ，视频 1:31:46）：「we're … in many cases trying to like reinvent the wheel of these organizational learnings that we had … in the '60s and '70s」；「start from a template, shape it … to be your own. Tell us what you find」

## Q&A 精确版

两轮，第一轮在 demo 后（视频 32:15–1:03:30），第二轮在结尾（1:32:38–1:41:32）。中间 Explainer 演示时有一个插话问题。提问几乎都听不清，下面的问题按讲者复述整理。回答人按字幕换人标记推断，不确定的写「讲者」。

**Q1 [32:18] 在 Claude Code 里跑 `/claude-api` skill，是在我机器上跑还是在 Managed Agents 里跑？**
CJ：Claude Code 按你的配置在本地或云端跑，和 Managed Agents 是两套系统；它从 Claude Code 的环境发 API 调用去创建 agent、session 等资源。

**Q2 [32:57] 和 plugins 怎么合并？能不能把一个 workflow（例：研究潜在收购）当 plugin 分发？**
Harrison：plugin 包含多种资源，其中 skills 等组件「should just drop into the API surface already today」（API 有 `resources` 字段和 skills 相关字段）。把 workflow 当 plugin 分发「not today」，不确定以后会不会支持；可以把 workflow 运行时生成的 prompt 拿出来放进 Claude Code，「it's a text file … probably be pretty pretty close」。

**Q3 [34:15] 能不能让部分 agent 跑在 Mac 或其他非托管环境？**
用 **custom tools**：模型通过它「kick out to you」，你在本地 macOS 或任何机器上执行，再把结果返回模型。

**Q4 [34:56] 怎么随时间优化 flow，少烧 token？**
Harrison：这是通用的 hill climb 问题；Claude Code 里有 hill climb skill 可以优化 prompt；flow 本身的调优，「cop-out answer」是把完整 transcripts 交给 Claude，让它找做多了或做少了的地方；原生集成「going to be top of mind」。CJ 补充 **Dreams / Dreaming**：给 Managed Agents 一组 session，让它「crystallize and … improve the memory store」，跑十次 flow 后做一次。Harrison：「I basically described hand rolling your own dreaming」。
资料：release notes 2026-05-06：Dreams 研究预览，「reads an existing memory store alongside past session transcripts and produces a reorganized output memory store」。

**Q5 [37:10] 能不能在 flow 运行时可视化多 agent 拓扑？**
Harrison：console 里可以；每个 sub-agent 有自己的 session thread，所有按 thread 的信息都能通过 API 拿到，可以自己做 UI。CJ 演示：看全部 threads、钻进单个 sub-agent、看事件（每个 tool call、结果、sub-agent 交回 lead agent），例如 thread message received 事件里能看到 lead agent 传了什么 payload；UI 还会改进。Harrison：「the dashboard itself」建在 public API 上，「anything that you see here, you could totally hand roll yourself」。

**Q6 [38:59] 模型知道自己的 budget 吗？**
Harrison：现在不知道，budget 主要是防 runaway 的 guardrail；「if you tell it the budget, Claude can get a little bit anxious」，以后可能改善。CJ：比如到 $5 会收到「budget reached」事件，你可以在运行中加钱，flow 从停下的地方继续，「any work that you've already accomplished is not thrown away」。

**Q7 [39:56] 和 Claude Tag 怎么整合？**
Harrison：Claude Tag 是把信息连到 Slack 里的 Slack bot，「a slightly orthogonal product surface」，近期大概不会直连；想自己做类似 Tag 的东西可以用 flows。CJ：会继续让各产品「seams start to like disappear」，例如 Cowork 和 chat 合并了一些，Claude Code 能从很多地方用；「Cloud Tag is like a huge daily driver for us」。

**Q8 [41:21] sub-agents 之间能互相通信吗？**
Harrison：API 上没有「sub-agent A message sub-agent B」的直接方式；它们「all share a file system」，可以用文件传话；能看到的只有 agent message sent / received 事件。
资料：文档：「All agents share the same sandbox, filesystem, and vault credentials, but each agent runs in its own session thread」。

**Q9 [42:05] flow 能不能根据发现的问题自己改、自己 hill climb prompt？**
Harrison：用 hill climb skill 和 dreaming 让它从过去学；有 **deployments**（按 schedule 跑 session），但没有按 schedule 自我改进的机制，可以用这些组件搭。CJ：hill climb 是 Claude API skill 的子命令 `/claude-api hillclimb`，「taking … Andrej Karpathy's auto research and sort of baking it into a skill」；例子：Haiku 发布时（「a couple weeks ago … might have been last week」）他做了个和孩子玩 charades 的工具（webcam + Haiku 猜动作），跑 hill climb 时 Claude 生成了一个 artifact 让他录自己划船、打网球的片段，迭代出「we need this many frames per second」之类的 prompt 改动；Claude 可能会给、也可以要求它给一个人工交互界面。
资料（二手）：Claude Haiku 5.5 约 2026-10-07 发布（多家报道，Anthropic 原页未核），和「last week」相符，【待确认】具体版本。Scheduled deployments 2026-06-09 发布（release notes）。

**Q10 [44:44] Navier-Stokes 之类科学发现突然集中出现，是用这种东西做的吗？**
Harrison：「maybe in our … demos our stock ticker is not ambitious enough」；flows 是「a way … to throw a test time compute at hard problems」；内部研究 harness 的细节不能说，作为 API 是让公众也能做这类事的第一步。CJ 补充：旧的 coordinator + roster 有 sub-agent 数量限制，flows「I think it's 1,000 agents」，lead agent 还可以同时或接连跑多个 flow。他周五起了一个 1,000 agents 的 flow 做操作系统，到现在「already been working for like 85 hours」，「three or four flows in」，GitHub repo 里约 1,000 个 commit，没做完。prompt 很简单：「Ken Thompson made Unix in a week. Can you try and make an operating system that's Unix compatible with a Python interpreter? Use flows … feel free to be brave and use up to 1,000 agents」，并给了一个 GitHub repo。
资料：文档：每个 run 最多 1,000 个 agent、默认寿命 24 小时，和「85 小时、三四个 flow」不矛盾（我的理解：每个 flow 是一个 run）。

**Q11 [47:41] 加 sub-agent 有没有边际递减，像组织加人有协调成本？**
Harrison：好问题，没有数字；这个研究领域很新，以后每个 sub-agent 的「capability curve」可能可计算。CJ：同样 100 个 agent，「it's going to do better with flows because there's some organization and isolation between those contexts with gating and these different phases」。

**Q12 [48:49] 怎么防无限循环或级联错误？**
Harrison：「yes, sort of」：用 budget 防 runaway；Claude「pretty good at detecting subagents that are misbehaving」，内部 eval 里能自动发现并切掉；另外用 dreaming、hill climbing 从过去的 run 学。

**Q13 [49:39] 能不能按条件（例如没有变化）只跑 flow 的一部分，免得白烧 token？**
Harrison：平台上没有，除了告诉 Claude 条件执行；「I would probably like layer something else on top of flows yourself within your application layer」；单 agent 30 秒能做完的事不适合 flows，「it's more for … ambitious, complex tasks」。

**Q14 [50:38] agents 之间的 context 隔离（每个 agent 的世界观）？**
Harrison：每个 agent 在自己的 session thread，「physically in the database like separate context windows」，默认不会通过文件系统之类泄露 context；每个 agent 有自己的 system prompt（自带或让 Claude 写），可以强制分开的 persona；thread message sends / receives 能看到 agent 间通信，防止「middle management agent」乱来。

**Q15 [51:37] 共享文件系统能不能物理隔开？用 hooks 控制访问呢？**
Harrison：现在只能靠 prompt 限定目录；没有目录级权限层，「that seems pretty useful … maybe we'll take that back」。追问 hooks：Managed Agents 里对应的是 **tool permissions**：读文件时按配置向你的系统请求许可，你检查是哪个 agent 读 `/mount/foobar`，不想给就 deny。

**Q16 [52:59] 凭据管理？**
Harrison：「you're teeing me up」，下半场整段讲 agentic safety（见第 8–11 节）。

**Q17 [53:17] flow 里能不能加入人工反馈？**
用 custom tools：给 Claude 一个「request human feedback」工具，需要时调用，你在工具结果里回复。

**Q18 [53:37] 造操作系统那个例子，如果中途挂了，怎么看中间状态、怎么交互？**
CJ：prompt 里要了「a team of docs writers」，「use Sonnet 5.5. I love the … documentation that comes out of that」，每个 flow 结束时写文档，直接看 GitHub repo；也可以 interrupt（「you're going the wrong direction. Please stop right here」），或让 Claude 持续更新报告文件。Harrison：写到 outputs 目录的文件可以通过「V1 files」API 下载，持久保存；或者像 CJ 一样往 GitHub commit。

**Q19 [55:11] 跑到一半从外部得到可能改变方向的新信息，难道烧掉两天 token 重来？**
Harrison：interrupt session，补充信息（「lemma X is actually probably useful for you」）；primary agent 掌握 flow 的完整状态，能从停下处起一个新 flow，结合新信息和历史。不支持的是在原计划中间插一步、重跑后续步骤。

**Q20 [56:07] 游戏开发者想用 flows 做游戏内 telemetry？**
Harrison：有丰富的 durable event stream，可以实时流式拿或用静态 API 列出。追问是否 multimodal：Managed Agents 支持图片输入，但扔游戏截图进去让它实时行动「would be a pretty bad user experience because it would just take too long」；提问者补充用于调试崩溃截图，「Totally feasible」。

**Q21 [57:53] 怎么在组织里共享模板化的 agents？**
Harrison：有 Terraform provider 可以用来创建 agents；console 有导出功能；没有一键分享按钮或「first-class like sharing mechanism」。CJ：最常见是在上面做产品界面，例如「very popular project management tool」的 Kanban 卡片拖到下一列就触发一个 managed agent session 或 flow。
资料：release notes 2026-09-03：`ant` CLI 1.30.0 加了 `ant apply`，从 repo 里的文件创建、更新 agents 等资源。Terraform provider 未在 release notes 里找到，【待确认】。

**Q22 [59:51] 能否按交给 agent 的工作类型（而不是 system prompt）调整行为？**
Harrison：先说用 system prompt，然后改口：每个 session 有 **effort** 旋钮，还能按 session 覆盖部分字段（session-level agent configuration overrides）。

**Q23 [1:00:51] Claude 怎么知道该用多大规模？**
Harrison：「evals and lower defaults」：股票 ticker 不该起 1,000 个 agent；Navier-Stokes 那种要你明确推它加大并行度。（Latent.Space 也引了这句，归给 Stall）

**Q24 [1:01:39] 用 markdown 文件既看 agent 在做什么、又定义 agent，是对的做法吗？**
Harrison：「It depends」；API 功能很多；「if you throw in your API key to a markdown file … that's definitely bad. Do not do that」；只是一次性让 Claude 读个 markdown 干点事，Managed Agents 不是合适的抽象。CJ 补充 progressive disclosure：主文档是索引（kernel 在这、assembler 在这、editor 在这），sub agent 只读和自己相关的，省 token。

**Q25 [1:09:59] （Explainer 演示中插话）能直接发到 YouTube 吗？**
「It does not deploy to YouTube, no.」「We should, though.」

**Q26 [1:32:38] 结果不好验证（比如视频看品味），给了反馈怎么在 flow 里迭代？**
CJ：先在 Claude Code 里迭代流程（用 dynamic workflows 或直接手做第一个视频），让 Claude 把做法编成 skill，再在 Managed Agents 的 flow 里用；前期可能要在组织里调研现有流程怎么做；然后用 hill climb skill 改 prompt，可以人来驱动：每次生成两个，你选喜欢的，它据此改 prompt。

**Q27 [1:33:59] 什么时候用 ultracode，什么时候用 Managed Agents flows？**
讲者：ultracode 是你坐在笔记本前在 terminal 里交互用 Claude Code；flows 是要按固定节奏重复、通过 API 接进任何产品。「we actually took the Ultra Code … abstraction or methodology, and generalized it into what flows is on the Claude managed agents API」；ultracode 是一次性的，flows 是「level this up a layer, and reuse them, and … run them on whatever trigger schedule I want」。

**Q28 [1:35:03] 平台上发生恶意行为，责任怎么算？**
讲者：「unfortunately, I am not a lawyer」；像任何平台型业务，平台和用户有直接关系；「defense-in-depth」，每一层都要尽力。

**Q29 [1:35:40] 内部用 durable event stream 做过什么？**
Harrison：有人做了竞技 Catan 模拟，用事件解释每个玩家为什么这么走，用了 custom tools。CJ：最容易想到的是自建 UI：带 tool call 展开的聊天界面，tool call 可以带 generative UI 画图；金融场景里 watchlist 发现有意思的东西时渲染图表、加技术分析。

**Q30 [1:37:29] 自托管 sandbox 和 Anthropic 原生环境有兼容差异吗？**
Harrison：有些功能在 self-hosted sandbox 上不完全支持，但 flows「totally supported」；一般尽量全环境支持，只是创建 session 时传的 environment 类型不同。

**Q31 [1:37:56] Bug Hunter 遇到多 repo 依赖图怎么办？**
讲者：「it's kind of up to Claude」：给了 GitHub repo 权限，它们都挂进 container；有 Bazel 那种清晰依赖树就用工具查，没有就搜符号。追问每次 PR review 是否要重新挂载：是，像 CI job 每次 checkout。

**Q32 [1:39:01] 预先建好依赖图能帮 flow 优化吗？**
「totally」：作为文件上传进 session，告诉 Claude 文件路径。

**Q33 [1:39:24] 多 agent 拓扑能嵌套几层？**
「it's just one layer of nesting」，不支持任意复杂的 mega fan out；以后可能，但要先把安全「really locked in」。
资料：文档：subagent 委派一层深；workflow run 不嵌套，只有 primary thread 能启动 run。

**Q34 [1:39:46] 数据在 VPN 后面、只有本机能访问，怎么给托管 agent？**
Harrison：用 **MCP tunnels**，「private MCP access to the agents on platform using Cloudflare tunnels today」，不暴露到公网，连接直接从你的私有 MCP server 出发。
资料：release notes 2026-05-19：MCP tunnels 研究预览，「connect to MCP servers in your private network」。Cloudflare tunnels 这个实现细节没在文档里核到，【待确认】。

**Q35 [1:40:53] Anthropic 内部怎么用？**
「We're very like Claude tag pilled」：Claude Tag 自主做很多事，例如看 Jira 过去 24 小时的动态，主动建议改 ticket。追问定时：「if you just say at Claude like every every day at 6:00 a.m. like do this … it would work out of the box」。视频在掌声处结束（1:41:36）。

## 勘误表（YouTube 字幕 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| Cloud Code / Cloud Console / Cloud Manager agents / cloud manage agents | Claude Code / Claude Console / Claude Managed Agents | 上下文、文档 |
| Claude managed agents（字幕常小写） | Claude Managed Agents | 文档 |
| ultra code / Ultra Code | ultracode | Claude Code 文档 |
| multi-agent 2026-10-01 | `multiagent_20261001` | 文档 |
| Claude API managed agents on board / Cloud API scale manage agents on board | `/claude-api managed-agents-onboard` | AlphaSignal 报道的命令写法 |
| hill climb scale | hill climb skill（`/claude-api hillclimb`） | 上下文 |
| Tarek and Gary | Thariq and Garry（Tan） | 我的理解，【待确认】 |
| CD was talking about | CJ was talking about | 上下文 |
| Jay's probably about to | CJ's probably about to | 上下文 |
| Nav your Stokes | Navier-Stokes | 上下文 |
| loop intel done | loop until done | 上下文 |
| Ant CLI | `ant` CLI | release notes |
| {slash}mount{slash}fubar | /mount/foobar | 上下文 |
| a bearer off token | a bearer auth token | 上下文 |
| allowed hosting | allowed hosts | 文档 `allowed_hosts` |
| Basil build | Bazel build | 上下文 |
| marked out files | markdown files | 上下文 |
| Cloud Devs | ClaudeDevs（X 账号） | AlphaSignal、Latent.Space |
| Cloud Tag | Claude Tag | 上下文 |
| Claude AI {slash} design | claude.ai/design | 上下文 |

## 出入

1. **flows 的名字**：讲者一直叫 flows；文档和 release notes 叫 dynamic workflows / workflow runs，Latent.Space 也写讲者「called them flows」
2. **interrupt 能不能停 flow**：CJ 说发 `user.interrupt`「will stop the flow」；文档写 `user.interrupt`「stops the agent's turn. It ends no run」，要停 run 得发 `user.message` 让 agent 停。Harrison 在 Q19 的说法（interrupt 后补信息、起新 flow）和文档不冲突
3. **Linux 7.2 的数字**：口头「2,600 something … developers across 300 different companies」；LWN 统计 2,652 位开发者；公司数和「九周」没核到
4. **Unix 的「一个月」**：讲者说一个月四周、各做一样（kernel、editor、assembler 和想不起的第四样）；常见说法是三周、OS / shell / editor / assembler，未查一手出处
5. **swyx 报道**：委托方说是 swyx 写的；链接跳到 Latent.Space，署名 Richard MacManus。报道没有 1,000 agents、85 小时等数字，也没讲 vault 和 auto mode
6. **Dreams 的归属**：Latent.Space 写是 Avilla 建议用 Dreaming，和字幕的换人标记一致
7. **97% 的出处**：讲者说「some public research」；Claude 博客 2026-08-07 的说法是 97%（早期报道有 93% 的版本）
8. **单 agent 的极限**：讲者自称「hand waved numbers」（一小时 keels over），不是测量值
9. **workflow 当 plugin 分发**：Q2 回答 Managed Agents 的 workflow「not today」；Claude Code 本地的 workflow 已经可以放进 plugin 分发（Claude Code 文档）。两者是不同产品，不算矛盾

## 【待确认】清单

1. slide 上 single agent vs flows 对比图的具体成本和召回数字
2. 安全六项的 slide 原文（口述只能分出五项）
3. Explainer Studio 的模板名（字幕「Explainer Video Maker」）
4. 「Tarek and Gary」在 AI Engineer SF 的具体 talk 和原话
5. Linux 7.2 的公司数和开发周期
6. Anthropic「66 of 70 bugs」测试的一手出处（只有二手报道）
7. Terraform provider 是否官方存在
8. MCP tunnels 是否基于 Cloudflare tunnels
9. Q9 里 Haiku 的版本（推测 Haiku 5.5）
