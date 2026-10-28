# Langfuse Workshop - Evals and Observability in regulated Industries：精确版

> Precise notes for this session, compiled from the official video captions and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材整理完，视频字幕逐段过完。没有 slide 照片，slide 和 demo 画面只能从讲者的口述推断。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | 没有照片 |
| 视频 | [obfPzbapMIM](https://www.youtube.com/watch?v=obfPzbapMIM)，改名「Evals for High-Stakes AI Agents — Doneyli De Jesus & Annabell Schäfer, Langfuse」，53:32，上传 2026-10-18。YouTube 自动字幕。章节：00:00 Why evals are hard / 05:01 The AI engineering loop / 12:47 Demo: a banking assistant with tracing and PII masking / 20:35 Building datasets / 25:18 Expert review and failure modes / 30:07 Choosing evaluators / 35:00 Running evals automatically on every prompt change / 39:44 Dashboards for stakeholders / 45:58 Working with your coding agent / 52:13 Q&A |
| 官方 talk 页 | [ai.engineer/talks/obfPzbapMIM](https://ai.engineer/talks/obfPzbapMIM)：From the talk：[Langfuse agent skill](https://langfuse.com/docs/api-and-data-platform/features/agent-skill)、[Langfuse workshop](https://langfuse.com/workshop)；Further reading：[Langfuse datasets](https://langfuse.com/docs/evaluation/experiments/datasets)、[Annotation queues](https://langfuse.com/docs/evaluation/evaluation-methods/annotation-queues) |
| 其他一手资料 | [langfuse.com](https://langfuse.com/) 首页数字；[Langfuse joins ClickHouse](https://langfuse.com/blog/joining-clickhouse)；[YC 公司页](https://www.ycombinator.com/companies/langfuse)；workshop 仓库 [github.com/langfuse/langfuse-workshop](https://github.com/langfuse/langfuse-workshop) |

## 基本信息

- 时间地点：2026-10-12（周一）2:20–3:20pm，Sutton Room (NY Ballroom W)，Workshop (1-2 hrs)（资料：日程，[session 链接](https://ai.engineer/nyc/2026/schedule?session=f20b8d60-8a75-4ab3-a2fb-42159024e65f)）
- 原题「Langfuse Workshop - Evals and Observability in regulated Industries」，视频改名「Evals for High-Stakes AI Agents」
- 讲者：Doneyli De Jesus（日程：Head of AI Engineering Solutions, Langfuse (by ClickHouse)）；Annabell Schäfer（日程：AI Engineering Solutions, Langfuse by ClickHouse）
- 讲者说（视频 00:12，Schäfer）：自我介绍是 Langfuse 的「AI solutions engineer」，同事 Doneyli「leading AI solution architecture」，和最大的客户一起把可靠的 agent 送上生产。她「in my previous life」在一家为医疗器械合规做 AI agent 的公司，那里要有客户验证过的 ground truth 数据集，跑很多 human-in-the-loop 系统，当时大量时间花在 Langfuse 里；约七个月前加入 Langfuse
- 资料：日程 bio 说 Schäfer 之前「built AI agents for MedTech compliance」
- 官方简介（日程）：以银行客服 agent「Northwind Bank」为例：端到端 trace 每次交互；用 guardrail 拦截高风险请求并记录每次拦截；用数据集在 CI/CD 里评估答案的质量和合规；在 dashboard 上看到 agent 的主要 failure mode 并估算业务影响。可以带电脑跟做：一个 seed script 会把 demo 装进你自己的 Langfuse 项目
- 我的理解：视频里讲者没有说出「Northwind Bank」这个名字；seed script 也没有在视频里出现

## 1. 为什么 eval 难

- 讲者说（视频 01:09，Schäfer）：现场举手：很多人参与过把 agent 送上生产；不少人用过 Langfuse 或别的 observability/eval 工具；问谁「solved evals」，「For the confident hands, I would love you to come by at our booth and explain us how」
- 讲者说（视频 02:09）：「AI agents can fail while the system metrics themselves look green」。例子（slide 上的图）：用户问能不能退款，agent 查了政策说「sure I issued your refund」，状态全绿、没有延迟问题；但 refund 工具从没被调用，「the agent lied about doing it to the user」，几周后用户回来说没收到钱才会知道。所以要持续挖掘出错信号
- 讲者说（视频 03:14）：Langfuse 创始人三年前在 Y Combinator 时发现「there's nothing like a unit test for AI applications」，做了 Langfuse，「an open source platform to continuously trace monitor and improve your AI systems」。起初是开发者工具，后来目标用户加上 domain expert，过去「8 to 12 months」还多了一类用户：coding agent
- 资料：Langfuse 是 YC W23，2022 年创立，创始人 Marc Klingen、Maximilian Deichmann、Clemens Rawert；2026 年 1 月被 ClickHouse 收购（[YC](https://www.ycombinator.com/companies/langfuse)、[Langfuse 博客](https://langfuse.com/blog/joining-clickhouse)）
- 讲者说（视频 03:54）：「more than 150,000 uh developers globally」；「21 of the Fortune 50」；每月「more than 90 billion observations」；客户举了 Ramp、Canva、Khan Academy 和一个听不清的名字（字幕「SUP」，【待确认】）
- 资料：langfuse.com 首页写「Used by 21 of the Fortune 50」「90B+ observations per month」「100,000+ engineers build on Langfuse」（[langfuse.com](https://langfuse.com/)）。开发者数和讲者说的 150,000 不一致，见出入

## 2. AI engineering loop

- 讲者说（视频 04:25，Schäfer）：和客户合作三年，能可靠交付并持续改进 agent 的团队都在做某种形式的「AI engineering loop」，一组系统化改进的实践：
  - **Tracing**（视频 04:50）：基础。把 agent 的每一步记成日志，最好人也能检查。slide 上是一个 Langfuse trace 截图，一个 IT support agent，「an agent that is answering questions from your parents so you don't have to do it」：根 trace 有总耗时和成本，下面拆成每一步，generation（OpenAI chat 调用）和 tool call（前面有个小工具图标），屏幕更宽的话能看到输入输出和 metadata。用来追踪成本、延迟、token，也给 coding agent 规模化地看
  - **Monitoring**（视频 06:50）：规模上人看不过来，要聚合指标、找出错信号：成本延迟之外还有质量信号，比如合规 guardrail 是否守住、有多少用户不满，「How many are writing all caps messages」
  - **Datasets**（视频 07:37）：生产中想让 agent 处理的情况的代表集合，包括应当被正确拒绝的范围外请求（IT support agent 不该给投资建议或回答纽约天气）。每条有输入和期望输出：样例回复、期望行为或要覆盖的关键词。可作为离线迭代的稳定 benchmark，也可放进 CI/CD
  - **Experiments**（视频 08:29）：改变设置的一个方面看会怎样（prompt、模型、上下文、tool description），「ideally isolating concerns」：只改 prompt，别同时改五样
  - **Evals**（视频 09:04）：要能度量实验结果；自己看（「which you by the way should should probably sometimes do」）之外，要可扩展的方式：LLM-as-a-judge，或代码 eval（分类任务的精确匹配、行为的确定性检查、检测全大写）
- 资料：Langfuse 公开 workshop（[langfuse.com/workshop](https://langfuse.com/workshop)）用一个 TypeScript 示例「Dad IT Support Agent」（agent 叫 Specs，帮老爸解决 iPhone 问题），分 00–08 九步：Setup、Base App、Tracing、Prompt Management、Monitoring、Dataset、Experiments、Evaluation、Wrap-up。我的理解：讲者 tracing slide 上的 IT support agent 截图就来自这个示例
- 讲者说（视频 09:40，Schäfer）：入口因场景而异。低风险的内部聊天 agent 可以先只有 tracing 加几个 monitor（看哪里用户沮丧），边走边加测试用例，直接上生产。面向客户、尤其是受监管行业的，不想冒险。受监管行业里看到几种做法，基础永远是 tracing（每一步、输入输出、调用了哪些工具），在此之上建 ground truth 数据集（「you probably all heard about auto improvement … all of this definitely needs the foundation of a ground truth data set」）：
  - expert curation：和公司内的 domain expert 一起建
  - shadow runs：流程继续人工做，agent 并行跑，对比 agent 会怎么处理，以此衡量准确率并积累 ground truth
  - 完全 human-in-the-loop，逐步把数据切成有把握自动化的部分和始终需要人的部分（她上一家公司就这么做）

## 3. Demo：银行助手、tracing 和 PII masking

- 讲者说（视频 11:56，De Jesus）：「you can call me Don」。这个例子是给几家金融机构做的：他们做出第一个原型后，领导或团队会问怎么评估这类面向客户的 agentic 方案。现场问金融从业者（「we're in New York. So, of course, you all are」）和医疗等其他受监管行业
- 讲者说（视频 12:51）：一个典型的银行助手。背后对银行很重要：金融责任、法律责任、风险，而 LLM「inherently non-deterministic」，要降低不可预测性、尽量复现结果、找出 failure mode
- 讲者说（视频 13:48）：功能一：可以扮演不同客户（普通客户到超高净值客户），检验 agent 是否遵守针对不同 persona 的 guardrail
- 讲者说（视频 14:22）：功能二：开启 PII 识别，被认定为 PII 的数据在写入 Langfuse 之前自动 mask。金融机构不想把信用卡号、社保号存进 observability 系统，要留在自己的 system of record 里
- 讲者说（视频 15:04）：功能三：检查 agent 是否遵守控制措施、怎么打分、定义的 quality gate 是否执行
- 讲者说（视频 15:28）：以「Anna」身份问国际电汇费用，从知识库返回答案；demo 里特意把调用的工具、数据源和 agent 配置细节显示在界面上（真实产品不会给客户看）
- 讲者说（视频 16:04）：输入「I lost my card. This is my number. Is it blocked?」并明文贴出卡号；用户输入无法控制，所以要在 instrumentation 时拦住。在 Langfuse 里打开 trace，卡号「completely redacted」，因为有一个 masking 函数，定义好的 PII 都会被立即 mask，不以明文存储
- 讲者说（视频 16:57）：trace 里能看到用户 prompt 和完整步骤树：Anthropic LLM 调用、可用工具、list tools、list accounts 等，以及输出；顶部是为这个用例定义的所有 score，每条新 trace 进来时自动计算：回复是否符合银行合规（客户问天气就不答）、failure mode、guardrail 是否拦截了回答
- 讲者说（视频 18:16）：例子：「can I put my savings in bitcoin」，银行不该回答；agent 正确处理，同时标记为 investment advice，要转给顾问。trace 里 input guardrail 显示主要风险是 investment advice，score 里客户意图也是 investment advice，可以据此筛选
- 讲者说（视频 19:14）：还支持多模态：客户打电话到客服中心，处理这通电话，speech-to-text 后评估有无违规；trace 里能看到客户在问冻结卡，能看到音频链接、转写和助手的回复

## 4. 建数据集

- 讲者说（视频 20:47，De Jesus）：客户不可预测，会以设计之外的方式用系统，也无法预料 LLM 方案上线后所有结果。评估标准的锚点是数据集：「your set of questions and expected outputs」。对软件开发者来说「these are your test cases」，没人会不过 QA 就发版，「unless you merge into main or prod, which I'm pretty sure most of you have done it」
- 讲者说（视频 21:48）：expected output 不一定是 agent 的答案，也可以是意图、应有行为。所以通常会有多个数据集，各测一类东西。demo 里一个是 red teaming（prompt injection 等绕过 guardrail 的攻击），一个专门针对「which cases have been disputed and where the … dispute has not been solved」
- 讲者说（视频 23:04）：这里仍需要 domain expert 和开发团队一起定义。两种建 golden dataset 的方式：团队已有一张要测的东西的表格，在 Langfuse 里新建数据集上传 CSV；或从生产 trace 里选出好的例子加进数据集（「dog food」），前提是认定它们是未来输出应有的样子
- 讲者说（视频 23:46）：演示的一切都能通过 CLI、MCP（字幕 NCP）或 API 完成，「it's better for me to show it in a UI rather than telling claude to do it end to end. Make no mistakes.」

## 5. 专家审阅和 failure mode

- 讲者说（视频 24:51，De Jesus）：error analysis：看 trace，理解 agent 是否走了该走的步骤、调用了该调用的工具、给了想要的输出。Schäfer 以前作为 Langfuse 用户「spent like days like literally looking at traces」
- 讲者说（视频 25:45）：生产里没法看几千条，所以挑一个有代表性的样本（比如 10% 或 20%，或只看涉及 dispute、信用卡的），给专家建一个 annotation queue：「you are the risk expert. I need you to take a look at this output」
- 讲者说（视频 26:45）：要让专家省事，他们有本职工作：给几个打分项（yes/no、true/false、1 到 10）加一条评论。演示：failure mode 选「missing a citation」，评论「Citation is not there. Needs to be included」，用了约 10 秒。一百条每条 30 秒，「That is what 50 minutes. No, no, not even. Right. I'm I'm I'm bad at math today.」（我的理解：100 × 30 秒 = 50 分钟，他的第一个答案是对的）。「give me an hour of your time」；现在可以用 Wispr Flow（字幕 whisper flow）口述
- 讲者说（视频 28:09）：标为完成后，人工标注作为证据跟着 trace。第二步用 LLM 从专家的全部评论里找出模式和类别：不要让专家自己发明类别，「give me your unfiltered raw commentary」，LLM 擅长模式匹配和从文本提取信息
- 讲者说（视频 29:02）：得到类别后定义质量维度（「we just call it scores right but I like to come up with fancy words」），demo 里有：cites expected sources、correctness、formal register（某些语言要用更正式的称呼）、language match、must include、not unsolicited（upsell）等

## 6. 选 evaluator

- 讲者说（视频 30:13，De Jesus）：再决定每个维度怎么打分、用什么 evaluator。可以在 Langfuse 里建 evaluator 库，trace 进来自动算分；也可以在客户端应用里实时算分再回传
- 讲者说（视频 31:05）：经验：LLM-as-a-judge 和确定性 evaluator 按类别「a balance mix」。全靠 LLM judge，「there's a nonzero chance that some of the outputs are … not going to be accurate」；加上同样输入永远同样输出的确定性检查，得到「a blended score」
- 讲者说（视频 31:57）：新建 evaluator 时可选 LLM-as-a-judge 或 code evaluator。code evaluator「It's just a regex. This is doing an exact match」，「always going to be 100% the same output for the same input」
- 讲者说（视频 33:05）：实验锚定在数据集上；问「What happens if I change the model? … the system prompt? … give it access to new tools」，在上生产前试，而不是「running on Vibes」

## 7. 每次改 prompt 自动跑 eval

- 讲者说（视频 34:38，De Jesus）：prompt 在 Langfuse 里做版本管理（「we don't even charge for this」），也有客户在 GitHub/GitLab 里管。把 prompt 变更当作触发器：已配置一个 automation，任何 prompt 变更都会在 GitHub 里启动一个 CI job（「Hopefully … I'm not getting the unicorn from from GitHub」）
- 讲者说（视频 35:45）：这个 prompt 有五个版本，v1 到 v4 逐步改动（v3 是开发用的尝试，v4 加了细节并给 agent 配置传了不同参数）。因为看到一些 regression，把 v4 promote 到 production（「like a caveman like clicking through」，也可以用 API 或 CLI 加 production 标签）。几秒后 GitHub 上出现新的 CI job，跑实验和评估
- 讲者说（视频 37:38）：展示之前跑完的一次：列出 prompt、版本、label，结果 passed；evaluator 有 must include、source recall、language match、correctness、not unsolicited upsell；每个 score 可以设最低阈值，不过线就不算 pass
- 讲者说（视频 38:20）：「I'm not crazy enough to just say, okay, cool. So you pass, go merge it」，现在只是放着；完全可以端到端自动开 PR 等人 review，或者「like everybody's doing today」直接推 main。至少有证据和「paper trail」

## 8. 给业务方看的 dashboard

- 讲者说（视频 39:12，De Jesus）：组织越来越关注「tokconomics」：「2025 in beginning 2026 everybody was token maxing now everybody is like token valuing or whatever they're calling it now」。用 dashboard 让领导和业务方确信 agent 带来业务价值（字幕 39:12–39:29 有一段缺失）
- 讲者说（视频 40:29）：dashboard 指标，「you might argue with me in the logic behind this」：
  - 过去 7 天有 1,400 次交互不需要转人工；成本「less than $7」。他「20 years ago」做过客服中心员工，那样一个人时薪 $15–20，一小时也接不了 1,400 个电话，「at least some directional business benefit」
  - failure mode 按出现次数排序，前三是 out of scope、informal register、dispute not resolved，且「creeping up over time」。在 tracing 里筛 dispute not resolved = 0 并存成 view，把几千条缩到几十、几百条做定向调查
  - 用户 thumbs up / thumbs down 作为 score 展示，大多是 thumbs up，thumbs down 占 18%
  - security risk 构成：大多数交互没有风险；过去 7 天有 89 次客户在问投资建议、agent「might have given or might have tried to give investment advice」，可以按 categorical score（intent / security risk = investment advice）筛出具体例子
- 讲者说（视频 45:12）：总结：面向客户的方案有责任、财务和声誉风险，不想把不准确或幻觉的信息放到客户面前；按系统化流程，就能拿出证据证明 agent 的输出遵守了 guardrail 和控制措施

## 9. 和 coding agent 一起干

- 讲者说（视频 45:59，Schäfer）：有些团队的做法是「Please fix my agent. Make no mistakes.」即使 agent 通过 MCP 或 CLI 接上了 Langfuse 数据，「this is something that we just see producing rather slop」
- slide（据讲者描述）：两个轴，时间投入（多/少）和系统质量（低/高）。全自动「fix everything, do it for me」落在左下角：不用花时间，但质量低、客户不满意；也不该全手工（自己 instrument、自己找所有 failure mode、自己看全部数据），这些可以自动化。要带进你的「taste」和 domain expert 来建数据集、定义应用要解决的问题和方向。理想区域是平衡点：人看数据、带入 taste 和领域知识；coding agent 做能自动化的事（instrument、找 failure mode、跑 eval、「maybe making also 20 experiments instead of only one」）。「try to be the the tasteful AI engineer」，逐步把这个循环更多地自动化
- 讲者说（视频 48:12）：如果明天开始：
  1. 装 Langfuse skill：一组 markdown 文件，帮 coding agent 理解怎么用 Langfuse、落实最佳实践；接了文档、所有 integration 页面和 Langfuse academy（数据集、instrument、写 evaluator 的最佳实践）
  2. 收集 ground truth：历史数据、生产数据或和 domain expert 一起跑的样本
  3. 受监管领域要定义 hard limit：比如金融服务「it should never give investment advice」，写进评估集，最好永不发生，「but at least you will know about it」
- 资料：Langfuse agent skill 开源在 [github.com/langfuse/skills](https://github.com/langfuse/skills)，可用 `npx skills add langfuse/skills --skill "langfuse"` 安装；文档列出的参考文件有 `cli.md`、`instrumentation.md`、`prompt-migration.md`，覆盖 instrumentation、prompt 迁移、数据集等（[文档](https://langfuse.com/docs/api-and-data-platform/features/agent-skill)）
- 讲者说（视频 49:23）：资源：De Jesus 用的仓库「is also available online」，数据是虚构的；Langfuse academy；一个 self-serve workshop
- 资料：self-serve workshop 即 [langfuse.com/workshop](https://langfuse.com/workshop)；academy 在 [langfuse.com/academy](https://langfuse.com/academy)。银行 demo 仓库的地址没有找到，【待确认】
- 讲者说（视频 50:06）：De Jesus 周三讲「building deployment gates for AI agents」；Langfuse booth 9；booth 上可以拍脸生成 AI 贴纸；有一次社区跑步，两位创始人在场
- 资料：日程里 De Jesus 的「Building Deployment Gates for AI Agents in Financial Services」在 2026-10-14（周三）2:55–3:13pm，McCarthy Room (Riverside Suite)

## Q&A 精确版

问题在视频里听不清，以下从回答还原。

1. **（视频 51:14）Langfuse 的存储依赖什么，能不能换？**（从回答推断）De Jesus：Langfuse 对存储「has a hard dependency」在 ClickHouse 上：产品最初用 Postgres，「Postgress couldn't handle the scale that we achieved and like billions of billions of events per day」，于是迁到 ClickHouse。追问需求是什么：数据可以导出到 S3，也可以用 API、CLI 或 MCP 取用
   - 资料：Langfuse v3 把核心数据层迁到 ClickHouse（[Langfuse joins ClickHouse](https://langfuse.com/blog/joining-clickhouse) 的报道背景）
2. **（视频 51:52）追问：「I want to host it on [Amazon]」** De Jesus：「you can host it 100% on Amazon」；云服务是全托管 SaaS，「it runs on Amazon」；也有自托管部署模式，比如用手头的 EC2 实例，「it comes prepackaged with all the components」
3. **（视频 52:18）LLM-as-a-judge 的一致性 / inter-rater reliability 怎么保证？** Schäfer：「super important question」。Langfuse skill 里也覆盖这部分（她一时想不起这个过程的术语，【待确认】）：让 LLM 像人一样判断。学术做法：自己给一个有代表性的样本打分，最好几个人类评分者，再优化 LLM judge 使它和人一致。实用做法：一个人标一些数据作为 ground truth，对着它优化，定期检查样本是否仍有代表性；用在生产数据上时，开始先容忍「a few too many false positives so you catch a little more than necessary but don't miss anything」，再逐步调低。视频在这里结束

## 勘误表（YouTube 自动字幕 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| Lancuse / Langfus / Lenfuse / Lenfus / length use / langu / lenuse / language skill | Langfuse | 日程 |
| Annabelle | Annabell | 日程 |
| Donelli / Delli | Doneyli | 日程 |
| way combinator | Y Combinator | 上下文、YC 公司页 |
| SUP | 【待确认】（客户名听不清） | — |
| lock | log | 上下文 |
| open AAI | OpenAI | 上下文 |
| NCP | MCP | 上下文 |
| whisper flow | Wispr Flow | 我的理解 |
| tokconomics | tokenomics | 上下文 |
| crown to data set | ground truth data set | 上下文 |
| interator reliability | inter-rater reliability | 上下文 |
| raiders | raters | 上下文 |
| click house / Clickhouse / clickout | ClickHouse | 通用写法 |
| Postgress | Postgres | 通用写法 |
| emails are great | evals are great | 上下文 |
| asking the the the theme to invent categories | asking the team to invent categories | 上下文 |
| autoscope / foremost registered | out of scope / formal (informal) register | 上下文 |

## 出入

1. **开发者数**：讲者说「more than 150,000 developers」；langfuse.com 首页写「100,000+ engineers」。21 of the Fortune 50 和 90B+ observations per month 与首页一致
2. **讲者职位**：Schäfer 台上说「AI solutions engineer」，日程写 AI Engineering Solutions，官方 talk 页写 Growth Engineer；De Jesus 被介绍为「leading AI solution architecture」，日程写 Head of AI Engineering Solutions
3. **Northwind Bank 和 seed script**：日程简介说用 Northwind Bank 为例并有 seed script 装进自己的项目；视频里没说出这个名字，也没演示 seed script；公开的 Langfuse workshop 是 Dad IT Support Agent 示例
4. **CI「blocks a prompt change when scores drop」**：视频简介这么写；演示里 CI 显示的是一次 passed，讲者口头说可以设阈值不过线就不算 pass，没有演示被拦下的情况
5. **evaluator 名单两处不同**：29:47 的维度是 cites expected sources、correctness、formal register、language match、must include、not unsolicited；38:00 CI 结果里是 must include、source recall、language match、correctness、not unsolicited upsell
6. **dashboard 前三 failure mode**：讲者先说「out of scope, informal register, and dispute not resolved」，后面复述时字幕成了「autoscope, the foremost registered」，维度名本身叫 formal register（我的理解：failure mode 指的是没用正式称呼）
7. **视频章节**：章节把 Q&A 放在 52:13，实际第一个回答从 51:14 开始

## 【待确认】清单

1. 银行 demo 仓库的地址（讲者说已公开），以及是否叫 Northwind Bank
2. 客户名单里字幕作「SUP」的那家公司
3. 150,000 developers 的出处（官网是 100,000+）
4. Schäfer 在最后一题想不起来的术语（我的理解：可能是 judge alignment 或 calibration）
5. Langfuse skill 是否真的覆盖 judge 和人工标注对齐（skill 文档页没提）
6. Q&A 原问题（音频听不清）
7. 39:12–39:29 字幕缺失的内容
