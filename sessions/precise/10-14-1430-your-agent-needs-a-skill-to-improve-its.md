# Your agent needs a skill to improve its skills：精确版

> Precise notes for this session, compiled from the official video captions where available, the author's room recording, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | 作者拍的 20 张（IMG_0701–0720）。不是传统 slide：前 14 张是 tldraw 画布（浏览器标签「AIE NYC talk - your agent needs a skill to improve its skills」），后几张是 Warp 终端/编辑器里的演示和生成的 HTML 报告。**没有外来照片**：第一张 IMG_0701 就是本场开场「Hi, I'm Ben DevEx Engineering Lead at Warp」 |
| 视频 | 没有本场视频。YouTube 上 Warp 八月的 "LLM Knowledge Bases" 是另一场活动，没用作来源 |
| 一手资料 | [Skill Doctor 页面](https://www.warp.dev/skill-doctor)；[skill-doctor 的 SKILL.md](https://github.com/warpdotdev/common-skills/blob/main/.agents/skills/skill-doctor/SKILL.md)（`warpdotdev/common-skills` 仓库）。演示画面里的路径 `~/Projects/common-skills/.agents/skills/skill-doctor/scorers/efficiency.md` 和这个仓库对得上 |

## 基本信息

- 时间地点：2026-10-14 2:30–2:48pm，Turing Room (Riverside Ballroom)，Breakouts，Evaluations Track
- 讲者：Ben Holmes，Warp，DevEx Lead（session.json）。白板开场写的是「DevEx Engineering Lead at Warp」；官方 bio 还写他是 Astro（astro.build）核心维护者、做 Warp Factories
- 官方简介要点：「We just found out our orchestration agent was wasting hundreds of tool calls this past month. The reason? One bad instruction in our message passing skill.」要讲一个 self-improvement 的反馈环：评估过去的 agent 对话、找到失败点、把发现变成值得 merge 的 skill 改动，并自动化成 software factory 的一部分
- **注意**：简介里「orchestration agent / message passing skill / 几百次 tool call」的例子，录音里没有出现；现场讲的是另一个例子（测试环境里找端口号），见第 4、6 节
- 演示时屏幕右上角时间：2:34pm、2:37pm、2:39pm、2:42pm、2:44pm（slide 上的系统时钟），和日程吻合

## 1. 开场：software factory 和两个循环

- slide（IMG_0701）：「Hi, I'm Ben DevEx Engineering Lead at Warp. Today we're talking about self-improving agents.」
- 讲者说 [00:00]：主题是「building self-improving agents. Agents that look through their past work and figure out how to improve their own setup」。他做 Warp Factories，「open and flexible infrastructure to help you all build software factories of your own」；今天不讲 software factory 全貌，只讲其中一块：让 agent 回看它们做过的工作，提出改进
- slide（IMG_0702、0703）「**Inner loop – improve the product**」：New work assigned → Triage → Spec writing → Implementation → （一格被挡住，【待确认】）→ Code review → Ship → Monitor usage → 回到 New work assigned
- 讲者说 [00:38]–[01:35]：software factory 就是自动化软件开发生命周期，可以看成流水线，也可以看成一个轮子：从 task list、board、Slack 消息开始，agent 依次做研究/triage、和人一起写 spec、实现、用 computer use 截图或录屏、几轮 adversarial code review，然后上线、监控
- slide（IMG_0704、0705）「**Outer loop – improve the factory**」：外圈红色虚线，内圈是「Core factory loop」。外圈四步：**Review agent conversations → Score quality → Isolate failures → Propose improvements**，「Propose improvements」旁边标着闹钟「**Every 24 hr**」。右侧「Scoring rubrics」：efficiency ✓、code quality ✓、test coverage ✗。左侧一个 diff 卡片：「test-writing/SKILL.md +57 −20 … **+Coverage pyramid** When to add unit vs. integration tests…」。底部一个红字框被人头挡住，只看到「…ations wrote …ests」（【待确认】，可能是「N conversations wrote redundant tests」）
- 讲者说 [01:35]–[02:41]：更有意思的是围绕这个轮子的「meta engineering」，他们叫 **factory engineering**：「putting in engineering work to improve the system that is driving the agents themselves」。可优化的东西包括 skills、代码架构、静态分析（他提到 SonarQube 等本次会议赞助商）；今天的循环只针对 **agent skills and configuration**
- 讲者说 [02:41]：循环是：agent 回看过去的对话 → scoring agents 打分，「think of this like an LLM as a judge, reviewing against rubrics that you define」，指标比如 code quality、test coverage → 汇总大量对话后找出失败及原因 → 「maybe an agent wakes up on a cron to suggest improvements」→ 继续评估，看改动效果

## 2. 第一步：把所有 agent 对话集中到一处

- slide（IMG_0706、0707）：「Review agent conversations」旁写着 **Local agents**：`~/.claude/**jsonl`、`~/.codex/*.jsonl`、Warp conversation storage；另一处写着需要的三样东西：**Transcript / Tool calls / MCP calls / Outputs (code)**
- 讲者说 [03:16]：本地 agent（Claude Code、Codex、Pi 等）的对话存在本机，Claude 有 `.claude` 文件夹，Codex 有 `.codex` 文件夹，优化「single player setup」够用了。他见过团队建一个 S3 bucket，用自动化把每个人的 `.claude` 文件夹打包上传，「just so we have everyone's conversations in one place」
- 讲者说 [03:59]–[04:33]：用 software factory 的话这件事「kind of comes for free」：agent 跑在共享的机器上，而不是每个人的笔记本；可能是长期运行的实例（转写为 "OpenBlog"，【待确认】），也可能是每次运行起一个隔离 sandbox。Warp 的 factory 是「isolated Docker sandboxes for each agent」。所有 agent trace 都在同一个地方
- 讲者说 [04:33]–[05:44]：不只要对话 transcript，还要元数据：tool calls、文件系统调用、MCP 调用；还要 outputs：对话里的代码 diff，或者 pin 住 commit 看最终的 pull request。三样：「the transcript of what was said, … the tool calls …, and then you have the output」

## 3. 第二步：打分（LLM-as-judge + rubric）

- slide（IMG_0708–0711）：「Score quality」下面画了三个 scorer：**Redundant tests / Verbosity / Efficiency**
- 讲者说 [05:44]：**举手调查一**：「How many people in here feel like their agents write too many tests?」（转写为 "two-minute tests"）。他说现场「about like 40% of people」举手，「On our team, it was about 100%」
- 讲者说 [06:16]：是否过度写测试取决于模型和指令；各团队对 end-to-end / integration / unit 测试的取舍也不同。可以设一个 scorer（他说的名字听不清，slide 上是「Redundant tests」），评估每次对话里 agent 是否写了太多测试
- 讲者说 [06:47]：**举手调查二**：「how many people feel that their agents write too many comments?」——「There's about everyone」。他不点名是哪个模型。code review 现在是最大的瓶颈，agent 怎么总结改动、在代码里留什么注释，会影响 review 速度。所以有 **Verbosity** 指标：agent 跟你说话或写代码时是否过于啰嗦
- 讲者说 [07:20]–[08:30]：第三个是 **efficiency**，看 tool calls 和 MCP calls。agent 跑得越久、越没人盯着（比如在 cloud sandbox 里），越容易漏掉失败的 tool call：「these agents are very persistent and will work around whatever roadblock that they run into, and they won't tell you about it」
- **讲者说的真实例子** [07:58]–[08:30]：他们的测试 harness 里，skills 带着很多错误指令，agent 绕过去了、从没告诉他们，靠反复尝试端口号摸清环境，最终做成。表面上只是「the agent takes like two minutes longer than maybe we expect」，底下有很多可改进之处

## 4. scorer 长什么样：`efficiency.md`

- 讲者说 [08:30]：scorer「It's just markdown」，在他们的设置里是「a specialized skill definition」，定义要用 LLM 打分的东西
- slide（IMG_0712，Warp 里打开的 `~/Projects/common-skills/.agents/skills/skill-doctor/scorers/efficiency.md`）：
  - `name: Efficiency`
  - `description: "Whether the agent worked directly toward its result, or wasted effort on redundant steps, avoidable rework, or unnecessary back-and-forth."`
  - labels（照片可读部分，右侧被截断）：

| value | score | description（照片可读部分） |
| --- | --- | --- |
| `highly_efficient` | 1 | "The agent took a direct path: nothing re-read or re-run, independent steps batched, no wo…" |
| `mostly_efficient` | 0.8 | "The agent slipped once or twice: a duplicated read, an early retry, or a small correction — … on cost." |
| `mostly_inefficient` | 0.4 | "The agent wasted effort repeatedly, or caused a round of rework an earlier check would have prevented." |
| `highly_in[efficient]` | 0.2 | "…the same defect reworked…"（大部分被挡住） |

- 讲者说 [09:00]–[09:35]：label 定义了这个标准下好和差是什么样，像高中写五段式作文时老师给的 rubric
- 讲者说 [09:35]：pass/fail 的线可以自己定，「In our case, maybe the bound is halfway through this list. So if it's a 0.4 or 0.2, that's kind of a failing grade」，以上算 pass
- 资料（SKILL.md）：「Only conversations with raw scores below 0.5 count as failed, and only those feed skill suggestions」（WebFetch 摘要的转述）；聚合时原始均值按 `curve(score) = 0.5 + 0.5 * score` 拉伸
- slide（IMG_0713、0714）efficiency.md 下半部分：要求按「what a com[petent] engineer with the same tools would have needed」来打分，而不是按 agent 手头能做到的；「A mistake that looks unavoidable in context still counts if better tooling, a skill, or a check would have prevented it — name that cause in the reason.」下面的 bullets 是「common sources of waste, not an exhaustive checklist」。**Assess:** 列表：
  - **Rework from mistakes.** Work redone because the agent got it wrong the first time: a test or build failure a local check would have caught, edits to the wrong file, a misread requirement later reverted.
  - **Cost to the human.** Repeated correction or steering from the user is the most expensive waste. A question asked up front is cheap; the same question asked after building the wrong thing is not.
  - **Information gathering.** Re-reading, re-running, or re-searching for something already found; reading a large file end to end when a targeted search would answer it.
  - **Routine-step overhead.** A roundabout way of doing something that's a standard, repeated part of this agent's job … Weight this beyond its one-run cost: the same avoidable overhead recurs on every future conversation until a skill or rule fixes the pattern.
  - **Batching.** Independent reads, searches, or workstreams run serially across turns instead of together.
  - **Flailing.**（后半被挡住）
  - **Verification timi[ng]**（后半被挡住）
- 讲者说 [10:06]：rubric 写的内容「what you would expect」，比如 information gathering、routine step overhead；「You should keep this as succinct as possible and just build it up as you go」，可以从几条 bullet 起步，自己测

## 5. 第三步：逐条对话打分，筛出失败

- slide（IMG_0715、0716）：一个会话文件 `update-auth-service.jsonl` 分发给三个 scorer：`redundant-test.md`、`verbosity.md`、`efficiency.md`；下方 **Sessions**（`add-chat-mvp.jsonl` 等）和 **Grades**（PASS / FAIL）
- slide（IMG_0717）完整流程：Review agent conversations → Score quality（Redundant tests / Verbosity / Efficiency）→ Isolate failures（`add-chat-mvp.jsonl` FAIL、`update-auth-service.jsonl`、`add-sentry-webhook.jsonl`，旁边 FAIL / PASS）→ Propose improvements
- 讲者说 [10:23]–[12:02]：假设有一个 auth service 的全栈实现对话，PR 已 merge，现在让 agent 去打分。「one agent run per conversation」；可以随机抽样，也可以全跑。示例打分：
  - test coverage：写了很多 end-to-end tests（转写为 "native tests"，【待确认】），而他们的规范说应优先写另一种测试（转写为 "independent test"，【待确认】是不是 integration test），没遵守指令 → fail
  - verbosity：注释不多、容易看懂 → pass
  - efficiency：在测试环境里花大量时间摸索怎么测这个改动，其实应该事先讨论 → 低于线
- 讲者说 [12:02]：通常的触发是 **PR merge**：merge 后起一个 agent 按各项标准给那段对话打分；随时间积累数据，每条记录包含各项的 pass/fail，以及一个 agent 写的「why one of those failures or passes happened」，这成为下一个 agent 决定改什么的 context
- 讲者说 [12:36]–[13:00]：他们的设置：对话一结束就收进来；cron 每 24 小时对过去 24 小时的对话抽样（「maybe there's been like 200」），只留失败的，把失败报告交给 self-improvement agent：「these were the 50 conversations that failed. Here are the notes … Go ahead and find commonalities」，并给它 skills 仓库和代码库的访问权限。例子就是白板上那个 diff：给测试相关 skill 加一个「coverage pyramid」（200、50 是举例的数字，不是实测）
- 资料（SKILL.md）：默认最多抽 **12** 个 session，回看 45 天；超过 50 条 transcript 时并行分批，每批约 20；每个 scorer 记录 label、数值分和 1–3 句理由

## 6. 演示：Skill Doctor

- 讲者说 [13:30]：「this is going to be the easiest one that you all will get to try. So it's called Skill Doctor」，在 Warp Factories 的页面能找到；它在单人设置上跑，爬你所有的 agent 对话（Claude、Codex 等），做一次性的评估
- 讲者说 [14:00]：这是在 Codex sessions 上跑的样例，「it grades in batches for a little bit of token efficiency」，然后 self-improvement agent 看分数出报告
- slide（IMG_0720）「**Agent Skill Report**」，`# skill-doctor`，「Generated October 14, 2026 at 4:09 PM UTC · harness: codex」：
  - 总评 **B**，OVERALL **83**
  - efficiency **73**、code quality **75**、procedure compliance **80**、verbosity **100**、skill coverage **100**
  - **12** conversations scored、**19** skills installed、**16** skills used
  - Findings 部分被人头挡住，能看到「Agents push to the wrong PR …」「… the agent pushed to origin instead of the … contributor's branch without asking」「Skills that … changelog …」「… in 3 sessions that added tests, and … less test and rewrite changelog entries」（片段，【待确认】完整内容）
- 核对（资料 + 我的计算）：SKILL.md 的总分权重是 `overall = 0.25 * efficiency + 0.25 * code_quality + 0.2 * procedure_compliance + 0.15 * verbosity + 0.15 * skill_coverage`。代入 slide 上的分数：0.25×73 + 0.25×75 + 0.2×80 + 0.15×100 + 0.15×100 = **83**，和报告一致（也确认了 procedure compliance 是 80 而不是照片上像 88 的那个数）
- 资料：skill coverage 不是 rubric 打分，是「a computed fraction of sessions where an installed skill was detected」；code quality 只在有代码改动的对话上打分（SKILL.md）
- 讲者说 [14:31]：「this is one shot」，更适合定期跑，「as a first go, you can just see what you can optimize right now」
- 讲者说 [14:31]–[16:03] 演示里 self-improvement agent 提的两个改动：
  1. **end-to-end testing skill**：agent 在测试机上总是到处找可用端口、不了解环境；改动是让 agent 有更好的找端口指令，「instead of just banging against the wall until it got something right」，省下 tool calls
  2. **changelog skill**：他总是要重写 changelog 的措辞，于是 skill 改成更 prescriptive 的语言规定
  - 「these were things that I could have gone and found myself」，但端口那条，因为他不盯着 agent 的过程（通常折叠在 toggle 后面或跑在云上），自动发现很有用
- 资料（[warp.dev/skill-doctor](https://www.warp.dev/skill-doctor)）：支持 Claude Code、Codex、Warp；用 `/skill-doctor` 运行；「Subagents score these conversations against tested rubrics for efficiency, code quality, and skill coverage」；页面上的样例报告是另一个项目（client-app，B / 82），不是现场这份
- 资料（SKILL.md）：「Everything runs locally. Never upload transcripts, session files, or any excerpt of them anywhere.」改动先写到 `proposed/<skill-name>/SKILL.md` 并生成 `diff -u`，「Do not modify the user's real skill files in this step」；最后渲染成一个自包含的 `report.html`

## 7. 持续运行：分数仪表盘

- 讲者说 [16:03]：最后想展示怎么持续跑，但演示出了状况（「Oh, shoot … No!」），他改为口头描述：一组按时间画的分数曲线
- 讲者说 [16:03]–[16:36]：他对比了 memory 系统：memory 也是让 agent 回顾对话，但「it's very invisible what the agent actually decided or what takeaways that it's got」；打分的好处是可以有一个仪表盘
- 讲者说 [16:36]–[17:37]：比如 efficiency 随时间的曲线，平均「50–60%」；把你对 agent 设置做的改动标在曲线上，就能看到拐点：「oh there was an inflection point here, this is when I merged the skill」，「I actually know that when I merge that skill that it suggested something real happened」
- 录音到 [17:46]「By having scores in the loop with self-improvement, you can get」截止

## Q&A 精确版

没有录到。录音在讲者结语中途截止（[17:46]，约 2:47pm），之后有没有 Q&A，【待确认】。

## 勘误表（Whisper 转写 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| Ben Holtz | Ben Holmes | session.json |
| DevX engineering lead | DevEx Engineering Lead | slide IMG_0701 |
| as part of the staff | 【待确认】（可能是 "as part of the process"） | 听不清 |
| suggesting programs | suggesting improvements【待确认】 | 上下文 |
| sonar cube | SonarQube | 上下文 |
| PlogCode / Plog / Plog folder | Claude Code / Claude / `.claude` folder | slide（`~/.claude/**jsonl`） |
| dot clock holders | `.claude` folders | 上下文 |
| Codex folder | `.codex` folder | slide |
| Pi | Pi（一个 coding agent）【待确认】 | 上下文 |
| OpenBlog | 【待确认】（某个托管 agent 的长期运行实例） | 听不清 |
| Warps Factory | Warp Factories | 上下文、资料 |
| Cloud Runners | cloud runners | 上下文 |
| codeps | code diffs | 上下文 |
| three countries | three components / three things | 上下文 |
| two-minute tests | too many tests | 上下文 |
| version of test | redundant tests（scorer 名） | slide |
| the engine communicates | the agent communicates | 上下文 |
| Velocity | Verbosity | slide |
| a handle on language | 【待确认】 | 听不清 |
| impines | 【待确认】 | 听不清 |
| financing of the agent | efficiency of the agent | slide |
| test harvest | test harness | 上下文 |
| paint a bunch of port numbers | ping / try a bunch of port numbers | 上下文、[15:04] |
| port / ports（指 scorer） | score / scorers | 上下文 |
| a point of view of blue | 【待确认】（大概是 "a pass/fail line"） | 听不清 |
| Apple Base 4 | 【待确认】（大概是 "once the rubric is set up"） | 听不清 |
| circle sessions | Codex sessions【待确认】 | 上下文 |
| in-off service | auth service | slide（`update-auth-service.jsonl`） |
| native tests | end-to-end tests【待确认】 | 上下文 |
| independent test | integration test【待确认】 | slide「When to add unit vs. integration tests」 |
| a self-improvement | a self-improvement agent | 上下文 |
| our protection changes | 【待确认】 | 听不清 |
| WarpFactors | Warp Factories | 资料 |
| blog, codexpire | Claude, Codex, …【待确认】 | 上下文 |
| codec sessions | Codex sessions | slide（`harness: codex`） |
| skill maker | skill（具体名【待确认】） | 上下文 |
| fight for port numbers | hunt for port numbers | 上下文 |
| end-to-end testing skill | end-to-end testing skill | — |
| board | port | 上下文 |
| strangers | traces【待确认】 | 上下文 |
| a memory system in their minds | a memory system in their mind【待确认】 | 听不清 |
| sorting | scoring | 上下文 |

## 出入（来源之间不一致的地方）

1. **简介里的例子 vs 现场的例子**：官方简介说「orchestration agent was wasting hundreds of tool calls … One bad instruction in our message passing skill」；录音里没有这个例子，讲的是测试 harness 里 skills 指令错误、agent 反复试端口号（[07:58]、[15:04]）。可能是同一类问题的不同实例，【待确认】
2. **scorer 名单**：白板和讲者讲的是 Redundant tests / Verbosity / Efficiency（外圈图里是 efficiency / code quality / test coverage）；SKILL.md 和现场报告里的 scorer 是 efficiency、code quality、procedure compliance、verbosity、skill coverage，**没有 "redundant tests"**。我的理解：redundant tests 是讲解用的例子（或他们内部 factory 的 scorer），不是 skill-doctor 的公开 scorer
3. **pass/fail 线**：讲者说「maybe the bound is halfway」，0.4、0.2 算 fail；SKILL.md 写的是原始分低于 0.5 算失败。两者一致
4. **抽样数量**：讲者举例「past 24 hours … maybe 200」「50 conversations that failed」是示意数字；skill-doctor 默认最多 12 个 session，现场报告也是 12。前者讲的是团队 factory 的自动循环，后者是单人一次性运行，不矛盾
5. **分数口径**：讲者说 efficiency「50–60% on average」（[17:07]，口头描述的假想仪表盘）；现场报告 efficiency 73。不同场景，不矛盾；另外报告上的分数可能已经过 `0.5 + 0.5 * score` 拉伸（我的理解，【待确认】报告展示的是否是拉伸后的值）

## 【待确认】清单

1. 内圈 Inner loop 被手机挡住的一格（Implementation 和 Code review 之间）
2. 外圈图底部红字框的完整文字（「…ations wrote …ests」）
3. 示例里 agent 写了哪种测试、规范要求哪种（"native tests" / "independent test"）
4. "OpenBlog" 指什么托管平台
5. "Pi" 是否指 Pi coding agent
6. 报告 Findings 的完整内容
7. 简介里 message passing skill 的例子现场是否讲过
8. 报告分数是否为拉伸后的值
9. 演示出状况时原本要展示的仪表盘
10. 有没有 Q&A
11. 勘误表里标【待确认】的若干听不清的短语（as part of the staff、a handle on language、impines、a point of view of blue、Apple Base 4、our protection changes、skill maker、strangers）
