[English](../10-14-1430-your-agent-needs-a-skill-to-improve-its.md) | **中文**

# 给每段 agent 对话打分，再让 agent 去修 skill

*在 AI Engineer NYC 2026 上，Warp 的 Ben Holmes 讲了一个循环：agent 按 Markdown 写的 rubric 给过去的 coding agent 对话打分，收集失败的对话，再针对出问题的 skill 提出修改。Holmes 用 Warp 的 Skill Doctor 做了演示。*

**Breakout，Evaluations track · 2026 年 10 月 14 日（周三）· 2:30–2:48pm · Turing Room (Riverside Ballroom)**
**讲者：** Ben Holmes，Warp DevEx Lead

![开场白板](../../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/01.jpg)
*整场 talk 都在一块 tldraw 白板上讲：「大家好，我是 Ben，Warp 的 DevEx Engineering Lead。今天聊 self-improving agents。」*

---

Ben Holmes 没有用幻灯片，而是在一块手绘的 tldraw 画布上讲，中间切到 Warp 终端做演示。Holmes 说，主题是「构建 self-improving agents：agent 回看自己过去的工作，想办法改进自己的配置」。

Holmes 负责 Warp Factories，Holmes 的说法是「开放、灵活的基础设施，帮大家搭自己的 software factory」。这场 talk 没有展开整体图景，只讲其中一块：让 agent 回顾自己在这类系统里做过的工作，提出改进建议，随着时间推移越做越好。

session 简介开头举的例子和 Holmes 台上讲的不一样：「我们刚发现，上个月我们的 orchestration agent 浪费了几百次 tool call。原因？message passing skill 里的一条坏指令。」录音里没有出现这个故事；talk 用的是一个相关的例子，agent 到处找端口号，下文会讲到。

## 1. 两个循环：改进产品，改进工厂

![内循环：改进产品](../../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/03.jpg)
*内循环：工作从分配开始，经过 triage、spec、实现、review、上线，到监控。*

Holmes 说，software factory 指的是把软件开发生命周期自动化。有人把它画成一条 agent 相互交接的流水线；Holmes 画的是一个轮子，标着「Inner loop – improve the product」：分配新工作、triage、写 spec、实现、code review、上线、监控使用情况，再回到新工作。（实现和 code review 之间有一个框在每张照片里都被挡住了，标签不明。）工作来自任务列表、看板或 Slack 消息。agent 做调研或 triage，也许和人一起写 spec，实现改动，用 computer use 截图或录屏以便检查结果，再跑「几轮对抗式的 code review」，然后上线、监控。

「但这东西更有意思的部分，是围绕它的那一整套 meta engineering，让这个轮子尽可能高效地转起来。」Holmes 说。Warp 把它叫做 **factory engineering**：「投入工程工作，去改进驱动 agent 本身的那个系统。」这可以是改进驱动 agent 的 skill，也可以是改进代码架构、跑静态分析，让 agent 在代码库里改对地方（Holmes 提到了 SonarQube 和其他在做这件事的赞助商）。这场 talk 的循环只针对一件事：agent 的 skill 和配置。

![外循环：改进工厂](../../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/04.jpg)
*外循环包在工厂核心循环外面：review、打分、分离失败、提出改进。*

第二张图「Outer loop – improve the factory」用一个红色虚线圈把工厂的核心循环围起来。四步分别是：review agent 对话、给质量打分、分离失败、提出改进，最后一步旁边画着闹钟，写着「Every 24 hr」。「score quality」旁边是一个 scoring rubric 的框，efficiency 和 code quality 打了勾，test coverage 被划掉。「propose improvements」旁边是一张 `test-writing/SKILL.md` 的 diff 卡片（+57 −20），新增了一节「Coverage pyramid」，讲「什么时候加 unit test，什么时候加 integration test」。底部有一条红色备注，部分被挡住了。

Holmes 沿着这个循环走了一遍。agent 回看自己过去的对话。打分的 agent 评判对话进行得怎么样：「可以把它想成 LLM as a judge，按你自己定义的 rubric 来 review」，指标比如 code quality 或 test coverage。团队的大量对话都打完分之后，「你就能找出所有失败，弄清楚失败的原因，然后也许有个 agent 被 cron 唤醒，来提出改进建议」。之后继续 eval，看改动效果如何。talk 剩下的部分逐段展开。

## 2. 把所有对话集中到一处

![agent 对话存在哪里](../../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/06.jpg)
*本地 agent 把 transcript 存在 `~/.claude`、`~/.codex` 和 Warp 的对话存储里。*

第一步是把所有 agent 对话放到一处，让一个 agent 能对团队做的所有事进行推理。Claude Code、Codex、Pi 这类本地 agent 把 session 存在各自的机器上；白板上列了 `~/.claude/**jsonl`、`~/.codex/*.jsonl` 和 Warp 的对话存储。这些足够优化「你的单人配置」。Holmes 接触过一些团队，专门建了一个 S3 bucket，再写个自动化把每个人的 `.claude` 文件夹打包上传，「就为了把所有人的对话放在一处」。

Holmes 说，有了 software factory，集中化「基本是白送的」，因为 agent 跑在共享机器上，而不是个人笔记本上：要么是一台长期运行的计算实例，要么是每次运行都新起的隔离 sandbox。在 Warp 的工厂配置里，每个 agent 跑在一个隔离的 Docker sandbox 里。只要所有任务都经过同一个云端 runner，所有 agent trace 自然就落在同一个地方。

![trace 需要包含什么](../../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/07.jpg)
*要记录三样东西：transcript、tool 和 MCP 调用、代码产出。*

只有对话文本是不够的。trace 还应该包含元数据：tool call、文件系统调用、MCP 调用，「所有把 agent 和外部世界连起来的东西」。还应该包含产出：对话里的代码 diff，或者如果你记下了 commit，就是实际的 pull request。Holmes 归纳为三样：「说了什么的 transcript」、tool call，「然后是产出」。

## 3. 用 LLM judge 给质量打分

![三个示例 scorer](../../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/10.jpg)
*「score quality」下的三个 scorer：冗余测试、啰嗦程度、效率。*

数据就位之后，「就可以让一个 agent 开始给质量打分了」。Holmes 画了三个示例 scorer，每个都先让台下举手。

**冗余测试。**「在座有多少人觉得自己的 agent 写的测试太多了？」Holmes 说大约 40% 的人举了手；「我们团队大概是 100%。」这取决于模型和指令，各团队对 end-to-end、integration、unit 测试的看法也不一样。scorer 可以评估 agent 在每段对话里写多余测试的频率。

**啰嗦（verbosity）。**「有多少人觉得自己的 agent 写的注释太多？」「差不多所有人。」Holmes 没点名是哪个模型。这件事重要，是因为「code review 是现在最大的瓶颈」：agent 怎么总结自己的改动、在代码里留什么注释，都会影响改动能不能被快速 review。verbosity scorer 检查 agent 跟你说话或写代码时是不是过于啰嗦。

**效率。**第三个看 tool call 和 MCP call。agent 在没有人参与的情况下跑得越久，比如跑在没人盯着对话的云端 sandbox 里，失败的 tool call 就越容易被漏掉。「这些 agent 非常执着，遇到什么障碍都会绕过去，而且不会告诉你。它们只会告诉你成功了。」Holmes 举了 Warp 自己的例子：在他们的 test harness 里，agent 的 skill 带着很多糟糕的指令，但 agent 不声不响地绕过去了，一个端口号接一个端口号地试，摸清环境，直到成功。「我们最多知道的是，哦，agent 好像比预期多花了两分钟。但底下其实有很多可以改进的地方。」

## 4. scorer 长什么样

「就是 markdown，你们大概也猜到了。」Holmes 说。在 Warp 的配置里，scorer「像一个专门的 skill 定义」，规定 LLM 该给什么打分。Holmes 打开了 Warp `common-skills` 仓库里 `skill-doctor` skill 中的 `efficiency.md`。

![efficiency.md：描述和标签](../../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/12.jpg)
*efficiency scorer 的开头：一行描述和四个带标签的等级。*

文件的描述是：「agent 是否直奔结果，还是把力气浪费在多余的步骤、本可避免的返工或不必要的来回上。」接着定义了四个标签（描述以照片里能看清的部分为准）：

| 标签 | 分数 | 描述 |
| --- | --- | --- |
| `highly_efficient` | 1 | 「agent 走了直路：没有重复读或重复跑，独立步骤批量执行……」 |
| `mostly_efficient` | 0.8 | 「agent 失误了一两次：一次重复读取、一次过早重试，或一个小修正……」 |
| `mostly_inefficient` | 0.4 | 「agent 反复浪费力气，或造成了一轮本可以靠更早的检查避免的返工。」 |
| `highly_inefficient` | 0.2 | 大部分被挡住；提到「同一个缺陷反复返工」 |

Holmes 说，这些标签定义了在这项标准上什么算好、什么算差，就像高中老师发给学生的五段式作文 rubric。及格线由你来定：「在我们这里，分界线大概在这个列表的一半。所以 0.4 或 0.2 就算不及格。那段对话里出了问题。」skill 公开的 `SKILL.md` 与此一致：只有原始分低于 0.5 的对话算失败，也只有这些对话会进入 skill 改进建议。

![efficiency.md：评估什么](../../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/14.jpg)
*scorer 的评判基准是：一个手里有同样工具的称职工程师需要做多少。*

![efficiency.md：Assess 列表](../../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/13.jpg)
*常见的浪费来源列表。*

标签下面，rubric 要求 judge 以「一个手里有同样工具的称职工程师需要做多少」为基准打分，而不是以 agent 实际做到的程度为基准。「一个在当时情境下看似无法避免的错误，如果更好的工具、一个 skill 或一项检查能防止它，仍然要算——在理由里写明这个原因。」后面的条目是「常见的浪费来源，不是一份穷尽的清单」：

| 浪费来源 | rubric 原文（节选） |
| --- | --- |
| 犯错导致的返工 | agent 第一次没做对而重做的工作：本地检查就能发现的测试或构建失败、改错了文件、误读需求后来又回滚 |
| 人的成本 | 用户反复纠正或引导是代价最高的浪费；事先问一个问题很便宜，做错之后再问同一个问题就不便宜了 |
| 信息收集 | 重复读、重复跑、重复搜已经找到的东西；用定向搜索就能回答的问题却把大文件从头读到尾 |
| 例行步骤的额外开销 | 用绕远的方式完成 agent 工作里标准、重复的部分；权重高于它单次运行的成本，因为在 skill 或规则修好之前，它会在以后的每段对话里重复出现 |
| 批量执行 | 彼此独立的读取、搜索或工作流跨轮次串行执行，而不是一起执行 |
| Flailing（瞎折腾）；验证时机 | 照片里部分被挡住 |

Holmes 对写 rubric 的建议：「尽量写得简洁，边用边补充。」一份 rubric 可以从几条 bullet 开始，自己先测试。

## 5. 给每段对话打分，再把失败挑出来

![一个 session 分发给三个 scorer](../../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/16.jpg)
*一个 session 文件发给每个 scorer，结果记为 pass 或 fail。*

下一张图把一个 session 文件 `update-auth-service.jsonl` 发给三个 scorer 文件（`redundant-test.md`、`verbosity.md`、`efficiency.md`），下方是一堆 session，旁边一个成绩框写着 PASS 和 FAIL。

Holmes 假设的场景：一个 agent 做了一个 auth service 的全栈实现，PR 已经合并，现在由一个 agent 评估这段对话进行得怎么样，「每段对话跑一次 agent」。可以随机抽样打分，也可以每段对话都打。逐个 scorer 看下来，grader 可能发现：测试方面，agent 写了很多 end-to-end 测试，而团队规范偏好另一种，所以因为没有遵循指令而 fail。verbosity 方面，注释很少、容易看懂，所以 pass。效率方面，agent 花了大量时间摸索测试环境，想弄清怎么测这个改动，而这本该事先讨论清楚，所以落在线下。

Holmes 说，通常的触发点是 PR 合并：这时由一个 agent 按所有标准 review 这段对话。时间长了，每条记录都积累起每项标准的 pass 或 fail，外加 agent 对「为什么失败或通过」的解释，这些会成为负责决定改什么的那个 agent 的 context。

![完整的外循环，一步一步](../../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/17.jpg)
*session 被打分，失败被分离出来，改进被提出。*

白板上的完整链条从 review 对话开始，经过打分、分离失败（`add-chat-mvp.jsonl`、`add-sentry-webhook.jsonl` 等 session 被标为 fail 或 pass），到提出改进。在 Warp 的配置里，对话一发生就被收集。一个 cron job 每 24 小时对过去一天的对话抽样（「也许有 200 段」），筛出失败的，把失败报告交给一个 self-improvement agent，指令大致是「这是失败的 50 段对话。这是每段对话上留下的备注。去找共性」，同时给它 skill 仓库和代码库的访问权限。（200 和 50 只是举例，不是实测数字。）外循环图上那张 coverage pyramid 的 diff，就是这种循环产出的改动。

## 6. 演示：Skill Doctor

「这会是你们最容易上手试的一个。」Holmes 说，接着介绍了 **Skill Doctor**，可以从 Warp 官网获取。它是一个面向单人配置的 skill：爬取你所有的 agent 对话，不管来自 Claude、Codex 还是其他工具，做一次性的 eval。

![在 Warp 里运行 /skill-doctor](../../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/19.jpg)
*一个 orchestrator agent 和三个 scorer subagent 在 review Codex session。*

运行时的照片显示命令是 `/skill-doctor Review the codex sessions for hubble.md stored in …/Documents/hubble-md-codex-sessions`，有一个 orchestrator 和三个 scorer agent（scorer-a、scorer-b、scorer-c）的标签页。orchestrator 读了 skill 支持的 harness 列表，列出了 session 文件。Holmes 说 Skill Doctor「分批打分，省一点 token」，之后一个 self-improvement agent 读取分数并写报告。

![Agent Skill Report](../../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/20.jpg)
*示例运行的成绩单：B，总分 83。*

报告标题是「Agent Skill Report」，生成于 2026 年 10 月 14 日 4:09 PM UTC，harness 为「codex」，总评 B：

| 指标 | 分数 |
| --- | --- |
| Efficiency | 73 |
| Code quality | 75 |
| Procedure compliance | 80 |
| Verbosity | 100 |
| Skill coverage | 100 |
| **Overall** | **83 (B)** |
| Conversations scored | 12 |
| Skills installed / used | 19 / 16 |

总分按 skill 的 `SKILL.md` 里的权重计算：efficiency 和 code quality 各 25%，procedure compliance 20%，verbosity 和 skill coverage 各 15%，算下来是 83。按同一份文件，skill coverage 不是 rubric 分数，而是检测到使用了已安装 skill 的 session 占比；code quality 只对改了代码的对话打分。照片里 findings 部分大多被挡住；能看清的片段提到 agent 推错了 PR（没问就推到 origin，而不是贡献者的分支），还提到一个 changelog skill。

「这是一次性的。」Holmes 说；定期跑更好，但「第一次跑，你就能看看现在能优化什么」。在这个示例里，self-improvement agent 读了 scoring agent 对较弱指标的备注，提出了两处修改。第一处对应前面那个例子：agent 不了解环境是怎么搭的，一直在测试机上找空闲端口，所以它建议修改 end-to-end testing skill，给出更好的找端口指令，「而不是一直撞墙，直到碰对为止」，省下 tool call。第二处来自 Holmes 总是重写 agent 写的 changelog 条目；它用更明确的规定性措辞重写了 changelog skill，让这种来回不再发生。

「这些事我自己去找也能找到。」Holmes 说，但端口问题能被自动发现尤其有用，因为 agent 的过程通常折叠在一个开关后面，或者跑在云端，没人仔细看。

Warp 的 Skill Doctor 页面说它支持 Claude Code、Codex 和 Warp，以 `/skill-doctor` 运行，用 subagent「按经过测试的 rubric，从 efficiency、code quality 和 skill coverage 几方面给这些对话打分」。`SKILL.md` 补充说：一切都在本地运行（「Never upload transcripts, session files, or any excerpt of them anywhere」，即不要把 transcript、session 文件或其中任何片段上传到任何地方）；默认从过去 45 天里最多抽 12 个 session；建议的修改写到 `proposed/<skill-name>/SKILL.md` 并附 diff，而不是直接改用户真实的 skill 文件；所有内容渲染成一个自包含的 `report.html`。

## 7. 持续运行：分数看板

Holmes 本想最后演示怎么持续运行这个循环，但演示失败了（「Oh, shoot … No!」），于是 Holmes 改为口头描述：一组随时间变化的分数图表。

Holmes 拿它和 memory 系统做了对比。memory 也让 agent 回看对话，但「agent 到底做了什么决定、得出了什么结论，是很不透明的」。打分则给你一个看板。把 efficiency 分数随时间画出来，比如平均在 50–60%，再标出你对 agent 配置做的改动；然后就能看到「这里有个拐点，就是我合并那个 skill 的时候」。Holmes 说，不只是 agent 在改进自己，「我还真的知道，合并它建议的那个 skill 之后，另一边确实发生了变化」。

录音在结束语中途断了：「By having scores in the loop with self-improvement, you can get…」（把分数放进 self-improvement 的循环里，你就能……）

---

**链接：** [Schedule](https://ai.engineer/nyc/2026/schedule?session=0a0cbd67-23fa-4a3d-a8dc-06e3caa3ab2f) · [Skill Doctor](https://www.warp.dev/skill-doctor)（Warp）· [skill-doctor SKILL.md](https://github.com/warpdotdev/common-skills/blob/main/.agents/skills/skill-doctor/SKILL.md)（`warpdotdev/common-skills`）

截至写作时，这场 session 还没有公开视频。本文依据作者的现场录音（在结束语处、约 17¾ 分钟时中断）、白板和演示的照片，以及上面的 Skill Doctor 页面和 `SKILL.md`。录音结束时没有 Q&A；之后是否有 Q&A 不得而知。[精确笔记](../precise/10-14-1430-your-agent-needs-a-skill-to-improve-its.md)（中文）保留了时间戳和完整的未核实细节列表。
