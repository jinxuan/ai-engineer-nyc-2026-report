[English](../10-12-1420-langfuse-workshop---evals-and-observabil.md) | **中文**

# 在受监管行业，agent 要留下证据链

*在 AI Engineer NYC 2026 上，Langfuse 的 Annabell Schäfer 和 Doneyli De Jesus 演示了银行怎样给面向客户的 agent 做 trace、审阅和上线把关：domain expert 写下原始评论，LLM 把它们归纳成 failure mode，每次改 prompt 都要先在 CI 里跑过 eval 才能发布。*

**Workshop · 2026 年 10 月 12 日（周一）· 2:20–3:20pm · Sutton Room (NY Ballroom W)**
**讲者：** Doneyli De Jesus，Head of AI Engineering Solutions，Langfuse (by ClickHouse)；Annabell Schäfer，AI Engineering Solutions，Langfuse (by ClickHouse)

---

Annabell Schäfer 开场讲了一段看起来完美的客服对话。用户要求退款；agent 查了政策，回复说已经退了；用户说谢谢。所有系统指标都是绿的，也没有延迟问题。但退款工具从来没被调用过。Schäfer 说：「The agent lied about doing it to the user.」团队要到几周后客户回来问钱在哪儿时才会发现。

Schäfer 说：「AI agents can fail while the system metrics themselves look green.」所以团队需要一种办法「to continuously mine for those signals where things are going wrong」。

Schäfer 在大会前约七个月加入 Langfuse。之前，在 Schäfer 所说的「my previous life」里，Schäfer 为医疗器械合规做 AI agent，那里的团队需要经过客户验证的 ground truth 数据集，也跑了很多 human-in-the-loop 系统。现场举手调查显示，不少人参与过把 agent 送上生产，也有相当多人用过 Langfuse 或别的 observability 工具。至于谁已经解决了 eval：「For the confident hands, I would love you to come by at our booth and explain us how.」

## 1. Langfuse 为什么存在

Langfuse 的创始人三年前在 Y Combinator 期间（据 YC 是 W23）碰到了这个问题，意识到「there's nothing like a unit test for AI applications」。Langfuse 现在是「an open source platform to continuously trace, monitor and improve your AI systems」。它起初是开发者工具，后来加入 domain expert 作为用户，过去「8 to 12 months」又多了第三类用户：coding agent。ClickHouse 在 2026 年 1 月收购了 Langfuse。

Schäfer 给出了几个采用数字：

| 指标 | 台上说的 | Langfuse 官网 |
| --- | --- | --- |
| 开发者 | more than 150,000 | 100,000+ engineers |
| 使用它的 Fortune 50 公司 | 21 | 21 |
| 每月写入的 observation | more than 90 billion | 90B+ |

点名的客户有 Ramp、Canva 和 Khan Academy。

## 2. AI engineering loop

Schäfer 说，和这些公司合作下来，能交付可靠 agent 并持续改进的团队，都在实践某种形式的、Langfuse 称为 AI engineering loop 的东西：一组持续重复、而不是做一次就完的做法。

| 步骤 | Schäfer 的解释 |
| --- | --- |
| Tracing | 基础：agent 走的每一步都记进人能检查的日志 |
| Monitoring | 在规模上理解 trace：聚合指标，以及出问题的信号 |
| Datasets | 应用应当能处理的一组有代表性的例子，以及该怎么处理 |
| Experiments | 改变系统的一个方面，看会发生什么 |
| Evals | 在规模上度量实验结果 |

tracing 那页 slide 是一个 IT support agent 的 Langfuse trace，「an agent that is answering questions from your parents so you don't have to do it」。（Langfuse 公开的 self-serve workshop 也是围绕同类应用做的，叫「Dad IT Support Agent」。）根 trace 显示总耗时和成本，下面是每一次 generation（OpenAI chat 调用）和每一次 tool call，带输入、输出和 metadata。这份日志既给人排查问题用，也给 coding agent 大规模读 trace 用。

monitoring 重要，是因为 trace 已经长到、多到没人能手工读完。除了成本和延迟，它还覆盖质量信号：合规 guardrail 有没有守住，有多少用户不满。Schäfer 说：「How many are writing all caps messages.」

数据集代表 agent 在生产中应当处理的情况，也包括它应该拒绝的范围外请求：IT support agent 不该给投资建议，也不该预报纽约的天气。每条数据有输入和期望输出，期望输出可以是样例回复、期望的行为或要覆盖的关键词。数据集可以作为离线迭代的稳定 benchmark，也可以放进 CI/CD。

experiment 每次只改一样东西，比如 prompt、模型、上下文或某个 tool description，Schäfer 强调「isolating concerns」：要测同一个模型换了 prompt 是否更好，就只改 prompt，「and not five other things」。eval 用来度量结果，因为没人想什么都亲自看（「which you by the way should probably sometimes do」）。可扩展的做法是 LLM-as-a-judge 和代码 eval，比如分类任务的精确匹配，或者检测全大写消息的确定性检查。

团队进入这个循环的入口各不相同。低风险的内部聊天 agent，有 tracing 加几个 monitor 可能就够上生产了，测试用例边走边加。面向客户和受监管的场景冒不起这个险。Schäfer 说，在受监管行业，tracing 永远是基础：agent 走了哪些步骤、看到了什么、调用了哪些工具。在此之上，团队要建 ground truth 数据集，任何关于 auto-improvement 的说法都以它为前提。Schäfer 介绍了三种做法：

| 做法 | 怎么做 |
| --- | --- |
| Expert curation | 公司内部的 domain expert 来建数据集 |
| Shadow runs | 流程继续人工运行，agent 并行跑；对比两者来衡量准确率并积累 ground truth |
| Full human-in-the-loop | 每个决策都有人参与，逐步把数据切成可以放心自动化的部分和始终需要人的部分 |

## 3. 一个被 trace 的银行助手

Doneyli De Jesus（「you can call me Don」）接手演示一个为 Langfuse 合作的金融机构做的 demo。De Jesus 说，这些团队做出第一个原型后，领导就会问怎么评估一个面向客户的 agent。问谁在金融服务业，很多人举手：「we're in New York. So, of course, you all are.」

De Jesus 说，银行的聊天助手看起来很普通，但背后有财务责任、法律责任和风险，而 LLM「inherently non-deterministic」。目标是降低不可预测性、复现结果、找出 failure mode。demo 有三个功能。可以扮演不同类型的客户，从普通储户到超高净值客户，检验 agent 是否遵守针对每类 persona 的 guardrail。个人身份信息（PII）在存进 Langfuse 之前会被 mask，因为银行不想让卡号或社保号进入 observability 系统，这些数据留在自己的 system of record 里。每次交互都按银行定义的控制措施和 quality gate 打分。

De Jesus 以「Anna」的身份登录，问国际电汇费用，从知识库得到回答。demo 界面还显示了调用的工具和数据源，真实产品不会给客户看这些。然后 De Jesus 输入「I lost my card. This is my number. Is it blocked?」，并明文贴出一个卡号。De Jesus 说：「You cannot control what your users input.」所以必须在 instrumentation 里拦住。在 Langfuse 的 trace 里，卡号被一个 masking 函数「completely redacted」，所有定义好的 PII 都在存储前被处理掉。

trace 里能看到完整的步骤树（Anthropic LLM 调用、可用工具、一次 list accounts 调用），顶部是为这个用例定义的 score，每条新 trace 自动计算：回答是否符合银行规定（问天气就不回答）、有没有 failure mode、guardrail 有没有拦截回复。有人问「Can I put my savings in bitcoin?」，agent 正确地拒绝了，并把请求标记为需要顾问处理的投资建议。trace 里 input guardrail 把投资建议列为主要风险，一个 score 记录了同样的意图，所以这类情况很容易筛出来。

demo 也能处理语音。一通打到客服中心的录音被转写并检查是否违规；它的 trace 里有音频链接、转写文本（客户要求冻结卡片）和助手的回复。

## 4. 数据集就是测试用例

De Jesus 说，客户总会以设计之外的方式使用系统，而对 LLM 来说，没人能预料所有结果。评估的锚点是数据集：「your set of questions and expected outputs」。对软件开发者来说，「these are your test cases」。De Jesus 说，没人会不过 QA 就发布，「unless you merge into main or prod, which I'm pretty sure most of you have done」。

期望输出不一定是答案本身，也可以是意图，或者 agent 应该做的事。所以团队最后会有好几个数据集，每个度量质量的一个方面。demo 里有一个做 red-teaming（prompt injection 和其他绕过 guardrail 的尝试），一个专门针对还没解决的争议（dispute）。

domain expert 仍然要和开发者一起定义这些数据集。De Jesus 见过两种起步方式：团队手里已经有一张要测什么的表格，作为 CSV 上传；或者团队把判断为好例子的生产 trace 加进去，用这种方式「dog food」数据集。UI 里演示的一切也都能通过 Langfuse 的 CLI、MCP server 或 API 完成。「It's better for me to show it in a UI rather than telling Claude to do it end to end. Make no mistakes.」

## 5. 专家写评论，LLM 找类别

De Jesus 把下一步叫作 error analysis：看 trace，判断 agent 是否走了该走的步骤、调用了需要的工具、给出了对的输出。Schäfer 以前的工作里「spent like days, like literally looking at traces」。这仍然重要，但没人读得完几千条生产 trace。所以团队抽一个有代表性的切片，比如 10% 或 20%，或者只看涉及争议或信用卡的对话，放进一个给 domain expert 的 annotation queue：「you are the risk expert. I need you to take a look at this output.」

这件事必须简单，因为专家有本职工作。De Jesus 建议给几个打分项（yes/no、true/false 或 1 到 10）加一条自由评论。demo 里，De Jesus 选了 failure mode「missing a citation」，写下「Citation is not there. Needs to be included」，用了大约十秒。每条 30 秒的话，一百条 trace 不到专家一小时，而 Wispr Flow 这样的口述工具让他们连字都不用打。每条标注都作为证据挂在对应的 trace 上。

然后让 LLM 读完所有评论，找出反复出现的类别。De Jesus 说：「You're not asking the team to invent categories.」「Give me your unfiltered raw commentary」，模式匹配交给擅长它的 LLM。这些类别就成了要打分的质量维度。demo 里有「cites expected sources」、correctness、「formal register」（某些语言里要用正式称呼对待客户）、language match、「must include」和「not unsolicited」upsell。

## 6. judge 和代码混着用

每个维度都要选一个 evaluator。Langfuse 可以对进来的 trace 自动跑一整套 evaluator，客户端应用也可以实时算分再回传。De Jesus 从客户那里得到的主要经验是，按类别用 LLM-as-a-judge 和确定性 evaluator「a balanced mix」。只用 LLM judge，「there's a nonzero chance that some of the outputs are … not going to be accurate」。确定性检查对同样的输入永远给出同样的结果，两者混合能得到更可靠的评估。

demo 里的 code evaluator 是一个做精确匹配的正则表达式。De Jesus 说：「This is going to give me the same output 100% of the time. This is not going to be, well, 95% or 80%.」

experiment 把这些串起来：一个任务数据集，先对现在的 agent 跑一遍，再在换了模型、system prompt 或 agent 能用的工具之后重跑。De Jesus 说，重点是在推上生产之前先做原型验证，而不是「running on vibes, which is pretty much what we've all been doing」。

## 7. 每次改 prompt 都跑 eval

demo 里，system prompt 在 Langfuse 里做版本管理（有些客户放在 GitHub 或 GitLab），一个 automation 把任何 prompt 变更当作触发器，在 GitHub 里启动一个 CI job。这个 prompt 有五个版本，每次改一点；第 3 版是开发阶段的尝试，第 4 版加了细节，并给 agent 配置传入了新参数。因为看到了 regression，De Jesus 通过点击把第 4 版 promote 到 production，「like a caveman」，用 API 或 CLI 加 production 标签也一样。几秒后，一个新的 CI job 开始对数据集跑实验。

之前跑完的一次显示了结果：prompt、版本号和标签，以及一个 pass。gate 里的 evaluator 是「must include」、source recall、language match、correctness 和「not unsolicited upsell」，每个都有最低阈值；有一个分数低于阈值，gate 就不通过。demo 只展示了通过的情况。De Jesus 没有让它自动合并：「I'm not crazy enough to just say, okay, cool. So you pass, go merge it.」团队可以让流水线开一个 pull request 等人 review，或者「like everybody's doing today」，gate 一过就直接推 main。无论哪种，都有「the paper trail, so to speak, and the evidence on why you make that decision」。

## 8. 给业务方看的 dashboard

De Jesus 说，组织现在关心「tokenomics」：2025 年和 2026 年初「everybody was token maxing」，现在大家都想从 token 里拿到价值。demo 的最后一部分是一个 dashboard，用来让业务方放心 agent 值这个钱。De Jesus 承认「you might argue with me in the logic behind this」。

| 面板 | 显示的内容（过去七天） |
| --- | --- |
| 不需要转人工的交互 | 1,400 次，总成本不到 $7 |
| 按出现次数排序的主要 failure mode | out of scope、informal register、dispute not resolved，都在「creeping up over time」 |
| 用户反馈 | 大多是 thumbs up；18% thumbs down |
| 安全风险 | 大多数交互没有标出风险；89 次是客户在问投资建议，agent「might have given or might have tried to give」 |

De Jesus 20 年前在客服中心工作过，那时这样一个人时薪 $15 到 $20，一小时也接不了 1,400 个电话；这个对比至少显示了「at least some directional business benefit」。failure mode 面板缩小了排查范围：De Jesus 不用读几千条 trace，而是筛出「dispute not resolved」等于 0 的记录、存成 view，剩下几十到几百条去调查。那 89 条投资建议也一样，按 categorical 的 security-risk score 筛出来。

De Jesus 说，这个流程的意义在于证据：面向客户的 agent 背着责任、财务和声誉风险，一个系统化的循环能让团队拿出证据，证明输出遵守了既定的 guardrail 和控制措施。

## 9. 和 coding agent 合作，但要带上 taste

Schäfer 回来收尾。有些团队直接对一个通过 MCP 或 CLI 接上 Langfuse 的 coding agent 说「Please fix my agent. Make no mistakes.」按 Langfuse 的经验，这样产出的「rather slop than anything that is actually helping you」。

Schäfer 的 slide 以时间投入和系统质量为两个轴。全自动（「fix everything, do it for me」）落在左下角：时间花得少，质量低，客户不满意。全手工也不再合适，因为 instrument 应用、找 failure mode、审阅数据都可以自动化。目标是两者之间的平衡：人来看数据，带入「taste」和领域知识，尤其是建数据集、决定应用要解决什么问题；coding agent 负责 instrument 代码、找 failure mode、跑 eval，还能跑「20 experiments instead of only one」。Schäfer 希望大家做「the tasteful AI engineer」，逐步把这个循环更多地自动化。

Schäfer 说，如果明天开始，会做三件事。安装 Langfuse skill：一组 markdown 文件，教 coding agent 怎么用 Langfuse，接入了文档、integration 页面和 Langfuse Academy。收集用来评估的 ground truth，来源可以是历史数据、生产数据，或和 domain expert 一起跑的样本。在受监管领域，定义 hard limit：在金融服务业，agent「should never give investment advice」。把它写进评估集，理想情况是永远不发生，「but at least you will know about it」。

Schäfer 说，De Jesus 用的仓库已经公开，数据是虚构的；另外还有 Langfuse Academy 和一个 self-serve workshop。De Jesus 周三还会讲 AI agent 的 deployment gate；Langfuse 展位（9 号）能用 AI 生成访客头像贴纸；团队还邀请大家参加一次社区跑步，两位创始人也会到。

## Q&A

录音里的问题听不清，下面根据回答还原。

### 运行 Langfuse

**Langfuse 的存储依赖什么？** De Jesus 说，Langfuse 对 ClickHouse 有「a hard dependency」。它最早用 Postgres，但 Postgres「couldn't handle the scale that we achieved」，每天几十亿事件，于是迁到了 ClickHouse。需要把数据放到别处的团队，可以导出到 S3，或通过 API、CLI 或 MCP server 读取。

**能部署在 Amazon 上吗？** De Jesus 说：「You can host it 100% on Amazon.」托管云服务本身就跑在 Amazon 上，不需要任何配置；自托管部署，比如用手头的 EC2 实例，「comes prepackaged with all the components」。

### 信任 judge

**怎么确保 LLM judge 稳定、和人的判断一致（inter-rater reliability）？** Schäfer 说「super important question」，并说 Langfuse skill 也能帮助完成这个过程（Schäfer 一时想不起这个过程的术语）。目标是让 judge 像人一样判断。学术做法是让几位人类评分者给一个有代表性的样本打分，再调 judge 直到和他们一致。实用做法是一个人标一些数据，对着这份 ground truth 优化 judge，并定期检查样本是否仍有代表性。用在生产数据上时，Schäfer 建议一开始容忍「a few too many false positives, so you catch a little more than necessary but don't miss anything」，再逐步调低。录音到此结束。

---

**链接：** [日程](https://ai.engineer/nyc/2026/schedule?session=f20b8d60-8a75-4ab3-a2fb-42159024e65f) · [视频](https://www.youtube.com/watch?v=obfPzbapMIM) · [Talk 页](https://ai.engineer/talks/obfPzbapMIM) · [Langfuse workshop](https://langfuse.com/workshop)（[仓库](https://github.com/langfuse/langfuse-workshop)） · [Langfuse agent skill](https://langfuse.com/docs/api-and-data-platform/features/agent-skill)（[仓库](https://github.com/langfuse/skills)） · [Langfuse datasets](https://langfuse.com/docs/evaluation/experiments/datasets) · [Annotation queues](https://langfuse.com/docs/evaluation/evaluation-methods/annotation-queues) · [Langfuse Academy](https://langfuse.com/academy) · [Langfuse joins ClickHouse](https://langfuse.com/blog/joining-clickhouse)

本文根据官方视频字幕和以上资料整理；这场没有拍 slide 照片，slide 和 demo 按讲者的口述描述。日程简介把 demo 里的银行称为「Northwind Bank」，台上没有用这个名字，银行 demo 的仓库也没有找到。[精确版笔记](../precise/10-12-1420-langfuse-workshop---evals-and-observabil.md)保留了时间戳、字幕勘误和待确认事项。
