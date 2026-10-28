# Agent = Model + Harness: Build your own Memory-Aware AML AI Agent Harness for Financial Crime Detection：精确版

> Precise notes for this session, compiled from the official video captions and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in the video), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：官方视频字幕逐段整理完，课程、模型、workshop 站点等事实核过；讲者对 context、缓存和 token 数的几处说法没有找到一手依据，已标出。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | 没有照片。slide 内容只能从讲者口述和视频描述还原 |
| 视频 | [KtBivPzmOM8](https://www.youtube.com/watch?v=KtBivPzmOM8)，改名「Memory-Aware Agent Harnesses for Anti-Money Laundering — Nacho Martinez, Oracle」，51:48，上传 2026-10-17。字幕是 YouTube 自动字幕。讲解约 30 分钟，之后是 notebook 演示和问答，最后讲者让大家自己做 workshop |
| 官方 talk 页 | [ai.engineer/talks/KtBivPzmOM8](https://ai.engineer/talks/KtBivPzmOM8)：资源列了 workshop 站点 [workshopwaitingroom.com](https://workshopwaitingroom.com/)（标在 29:29）和两个相关 talk（「Gateways are All You Need」「Respect The Process」） |
| workshop 站点 | [workshopwaitingroom.com](https://workshopwaitingroom.com/)：标题「Total Recall · New York」，页脚「Agent Memory & Harness Engineering · New York · October 2026」；填 GitHub handle 后会被加进 `jasperan-org/financial-fraud-agent-harness-workshop-lightweight`，在 Codespaces 里跑。repo 本身需要邀请，没看到内容 |

视频章节（资料：YouTube 描述）：0:00 Workshop setup · 2:07 Intro · 2:57 Agenda · 4:57 The use case · 5:57 The agent stack · 8:31 Agentic systems = automation + autonomy · 9:16 What is an AI agent? · 11:06 Why the harness matters more than the model · 13:21 Files vs. databases · 15:56 Agent memory · 19:01 Context summarization vs. compaction · 19:46 The agent loop · 20:51 Toolbox & skillbox patterns · 22:30 Skill distillation · 24:15 Continual learning · 26:10 Instruction hierarchy · 27:05 Workshop UI walkthrough · 33:35 Q&A: Episodic memory & conflicts · 34:35 Q&A: Skills, caching & context budgets · 40:49 Q&A: Keep your memory layer simple · 43:29 Q&A: Pre-built SQL vs. dynamic queries · 46:14 Hybrid search · 47:39 Build small custom harnesses

## 基本信息

- 时间地点：2026-10-12（周一）11:05am–12:05pm，Sutton Room (NY Ballroom W)，Workshop (1-2 hrs)（资料：[日程](https://ai.engineer/nyc/2026/schedule?session=898cb76b-41f3-47c0-ba31-6a5f8df40117)）
- 原题「Agent = Model + Harness: Build your own Memory-Aware AML AI Agent Harness for Financial Crime Detection」，视频改名「Memory-Aware Agent Harnesses for Anti-Money Laundering」
- 讲者：Nacho Martinez，AI Developer Advocate，Oracle（日程）；自我介绍「an AI developer advocate and a data scientist by heart」（视频 02:09）。GitHub [jasperan](https://github.com/jasperan)（YouTube 描述）
- 官方简介（资料：日程）：「Agent = Model + Harness. The model emits tokens; state, memory, retrieval, tool dispatch, identity, retries, and budgets. All these concepts are harness code — and that is where agent quality is won or lost.」workshop 用 Oracle AI Database 和一个 AML 数据集：「Meridian Bank, 25 branches, ~1,200 transactions over 90 days」，内容包括「long-term memory and schema scanning with OAMP, semantic and hybrid RRF retrieval with ONNX in-database embeddings, vector-indexed toolbox and skillboxes, and a bounded agent_turn loop」，承诺「a readable ~100-line loop」
- 讲者说（视频 01:35）：同事 Richmond 在场巡台答疑。我的理解：应是 Oracle 的 Richmond Alake（和 Nacho 合教 DeepLearning.AI 的 Agent Memory 课），【待确认】
- 我的理解：简介里的 OAMP、ONNX in-database embeddings、Meridian Bank 数据集，讲者在视频里都没提到名字

## 1. Workshop 设置

- 讲者说（视频 00:38）：在网站上用 GitHub handle 报名，自动收到 repo 邀请；workshop 跑在 GitHub Codespaces；「I paid too much money for that domain. So, use it」（workshopwaitingroom.com）
- 讲者说（视频 29:47）：Codespaces 费用他付，「just be careful with … the hardware that you deploy because it's expensive」；结尾（视频 51:17）说回酒店、回家也能继续开 codespace，他继续付费；Oracle 展位开到周三晚上

## 2. 讲者和课程

- 讲者说（视频 02:31）：「we re recently released a course with Andrew Ng and deep learning AI about building adaptive AI agents」，免费、约一小时，讲模型如何「adapt over time, learn over time」
- 资料：Oracle 开发者博客：DeepLearning.AI 短课 Building Adaptive AI Agents，讲师 Nacho Martinez 和 Casius Lee，讲 continual / adaptive learning，用 Oracle AI Database 26ai（[Oracle blog](https://blogs.oracle.com/developers/we-just-launched-a-new-course-on-how-to-build-adaptive-ai-agents)，[课程页](https://www.deeplearning.ai/short-courses/building-adaptive-ai-agents/)）。Andrew Ng 和这门课的直接关系没找到（他在 Oracle 和 DeepLearning.AI 早先的 Agent Memory 课发布稿里有引语），【待确认】

## 3. 场景：中型银行的反洗钱 agent

- 讲者说（视频 03:37）：听说来的人很多在 Wall Street 工作，所以把 workshop 改成银行场景
- 讲者说（视频 03:52）：前提：现在常听说「open AAI is launching 10,000 agents or 5,000 agents」去解决几百年未解的数学题，这些 agent 一直自主跑到结论、中间没有人；但 context window 有限，LLM 本身没有记忆，这些要我们自己实现；「we don't need to focus so much on the frontier models … let's work on the harness」。OpenAI 具体是哪个项目、数字多少，没核到，【待确认】
- 讲者说（视频 04:57）：中型银行有很多可疑交易，分成五类；例：一笔 100 万美元进账，然后连续三天在 ATM 每天取 5 万美元；agent 要自主判断或至少标记，让银行里的人少花时间

## 4. Agent stack 和应用形态

- slide（讲者口述，视频 05:54）：agent stack 自上而下：application layer（界面、chatbot、手机应用）、model（「the reasoning core」，「we don't get any say」）、data、infrastructure、compute。今天聚焦 **data** 层，因为「everything that we can manipulate and work around is on this part」
- 讲者说（视频 06:49）：data 层里有 agent loop（「arguably one of the most important parts」，驱动一切、可定制）、gateway 和 MCP（连外部数据源）、memory layer、context engineering（自动 summarize / compact / shrink 工作 context）、tools 和 skills
- slide（视频 07:41）：四种应用形态：LLM chatbots、RAG（「a glorified vector search」）、LLM-driven workflows、AI agents。Claude Code、Codex、Pi、OpenClaw、Hermes Agent 等是 3 和 4 的组合：workflow 提供 automation，agent 提供 autonomy，合起来叫 agentic systems
- 讲者说（视频 08:46）：工程师天天用这些系统，但自己做 AI 应用时很少按这种方式设计

## 5. 什么是 agent，什么是 harness

- 讲者说（视频 09:17）：AI agent 是「a computational entity」：LLM 提供推理（像大脑），数据库提供记忆，工具提供行动（像四肢），输入是感知（像眼睛），context 像注意力一样有限（「we can't watch seven movies at the same time」）
- 讲者说（视频 10:27）：reasoning 由 frontier labs 提供，我们控制不了；能控制的是 memory、tool use、perception，「and this is the harness itself」：「all the scaffolding that you need to do to build … the tool use the memory and the perception」
- 讲者说（视频 11:12）：harness「I think is the most interesting topic in AI nowadays」：换个配置，同一个 frontier 模型就变好或变差，「it's been proven that by just changing the harness the same model performs better or worse for specific tasks」（没引具体研究）
- slide（视频 11:46）：harness 的组成，「seven different or eight different components if you consider continue learning」：model、storage、encoding & retrieval（「how do I make the distance between two vectors」）、memory（短期 / 长期还是分区）、semantic layer、agent loop、context engineering（+ continual learning）
- 讲者说（视频 12:23）：semantic layer 的例子：「I'm a speaker and you are listening you are not singing right now … because it's not appropriate」：没明说但大家默认的上下文

## 6. Files 还是 databases

- 讲者说（视频 13:19）：有人主张一切放文件，有人是「database maximalists」
- 现场（视频 13:51）：slide 上是多个 agent 写同一个文件，问现在的 workaround 是什么；观众答「work trees」
- 讲者说（视频 14:20）：worktree 是 workaround，因为文件并发写有一致性问题（谁先写、读到旧版本怎么办）；「the work tree … workaround has been already solved by databases 30 years ago」，ACID（atomic、consistent、isolated、durable）写入就不会有这个问题；文件适合存大量非结构化数据，「We don't have to go into only one of them」

## 7. 记忆：短期和长期

- 讲者说（视频 15:58）：agent memory 是让 agent 使用、精炼、在合适时候召回信息的机制；分类由工程师决定
- slide（视频 16:35）：**short-term memory**：context window 本身，对话结束就消失；**long-term memory** 分三类：**episodic**（对话、tool logs、以往对话摘要）、**procedural**（skills、workflows，即一串 skills）、**semantic**（你是谁、偏好、喜欢的框架）
- 讲者说（视频 17:59）：不能都塞进短期记忆：有限，而且「the bigger the attention matrix is … the worse it performs. And this happens for every model」；从 0 token 开始比已经有 256,000 token 时解得准得多；「We're at a million because of the KV cache」；超过一定长度「the performance goes horribly」，「the smaller the context window is the better it will solve a problem」
- 我的理解：长 context 性能下降有多项研究（常被叫作 context rot），但「every model」「every type of problem」是讲者的概括；「limit is a million because of the KV cache」的因果说法没找到依据，【待确认】
- slide（视频 19:01）：两种办法：**context summarization**（让模型写摘要，从摘要开始下一段对话）；**compaction**（把对话分块存进数据库或文件、给 ID，agent 需要时按 ID 加 offset 取回，「kind of like reading from a file」）

## 8. Agent loop

- 讲者说（视频 19:44）：agent loop 是「the driver」：context assembly（从 skills、tools、以往对话、文件、数据库动态拉进 context）→ reasoning（调 OpenAI 或别的模型 API）→ 决定动作，循环
- 讲者说（视频 20:24）：workshop 的 loop 有「maximum patience」：最多 8 次迭代，每次最多 60 秒推理，「because I don't want to run out of openi credits」；完全可配置
- 资料：日程简介写的是「a bounded agent_turn loop」

## 9. Toolbox / skillbox、skill distillation

- 讲者说（视频 20:53）：他装了约一百个 skills，每个「like 3,000 5,000 tokens」，全放进 context 很快超限。办法：给 tools 和 skills 做 vector embeddings，agent loop 需要时做向量相似度搜索，拉出最相关的，用完再卸掉
- 讲者说（视频 22:12）：tools 和 skills「for some cases more than 50% of the context window」。未核，【待确认】
- 讲者说（视频 22:30）：**skill distillation**：从以往对话和运行里提炼东西，最简单是写一个 SKILL.md 放回 skillbox；「Hermes agent, one of the innovators in this sense」；例：前端配色偏好做成 skill，下次提到「coloring style」就因为 embedding 被自动选中
- 讲者说（视频 24:00）：**dynamic skill discovery**：agent loop 组装 context 时拉对的 skills 和 tools

## 10. 冻结的模型怎么变好：continual learning

- 讲者说（视频 24:13）：「how does a frozen model get better」：frontier 模型权重控制不了，但 data 层可以
- slide（视频 24:57）：三类：**parametric memory**（改权重，例如 LoRA、微调）、**representational memory**（embeddings 等）、**non-parametric memory**（今天讲的一切）。workshop 只做第三类，前两类在他的课里

## 11. Instruction hierarchy

- 讲者说（视频 25:59）：chat completions 请求里有「an inherent instruction hierarchy」：system prompt 最重要，user 请求排第三；tools 和 skills 在最下面；不能都塞进 system prompt，否则就没有层级
- slide（Q&A 中再展示，视频 36:39）：一个请求的组成：system prompt（例 10K 或 5K token）、developer（应用指令）、user query（「negligible like 100 tokens thousand tokens」）、tool outputs 和 retrieved docs（「80% of the request like 35 to 100,000 tokens」）
- 我的理解：讲者第二次说的四层顺序（system、developer、user、tools/skills）没有在两处完全一致；第一次说 user 是第三，第二次说 skills「have the least weight out of the four」

## 12. Workshop 界面和 notebook

- 讲者说（视频 27:05）：UI 设想是 AML 分析员和一个 24/7 后台运行的自主 agent 互动：「It doesn't sleep」，通过 harness 连数据库，自主做决定，但「It's not ruining people's lives」，不会直接封账户，而是把判断打印出来给人复核
- 演示（视频 29:23）：Firefox 里打开；codespace 自动部署一个应用：不断生成新交易（「of course fake data」），有的正常、有的有问题；agent 是「a grog model running on OCI」，标记可疑交易；能可视化 agent 的 context window、看数据库里的客户等数据、看 agent 实时分析（走 toolbox 里的工具、在 vector store 建的知识空间里搜索、调用很多工具、生成 suspicious activity reports），以及最近 30 个事件
- 讲者说（视频 31:51）：workshop 主体是 notebook「student」，里面有几个 to-do；第一步用「Grok model 4.3 … running on … Oracle」并连数据库（演示时 kernel 要重启）
- 资料：OCI release notes：xAI Grok 4.3 于 2026-05-01 上线 OCI Generative AI，模型名 `xai.grok-4.3`（[OCI release note](https://oracle.hydrogen.sagittarius.connect.product.adaptavist.com/iaas/releasenotes/generative-ai/xAI-grok-4-3.htm)，这是 Oracle 文档的镜像地址，官方 docs.oracle.com 原页没打开）
- 讲者说（视频 28:55）：还有一个 GitHub repo 汇集他们的其他 workshops、notebooks（slide 上，名字没念）
- 讲者说（视频 38:53）：notebook 里 context assembly 的默认是每次检索「three」个 tools 和三个 skills（分别在两个 vector store 里，k = 3）；不用 reranker，但可以在相似度搜索后加 embedding model + reranker，从三个里挑最相关的，因为任务可能一个 skill 就够

## Q&A 精确版

问答穿插在演示中（视频 33:06–50:12），多数提问听不清，按讲者复述或回答内容整理。

**Q1 [33:12] repo 在哪？**
在 workshopwaitingroom.com，或扫 QR code，通过 GitHub API 自动给权限。

**Q2 [33:36] episodic memory 会不会和以前的对话混淆？**
看你给它多少权重，这也是讲 instruction hierarchy 的原因；记忆层架构自己定：episodic memory 很重要时，「you might actually want to vectorize that rather than just save it as raw text」；workshop 里没有冲突，「because we have a clear separation on the memory … components」。

**Q3 [34:32] skills 能放进 system prompt 吗？**
可以，但在 instruction hierarchy 里 skills「have the least weight out of the four」；把它当「a plug and play place」。他和 Richmond 在试：预先计算第三方「skill hubs and tool hubs」（例如一组 MCP）的 embeddings，让 agent 对它们做向量搜索。tools 和 skills 附在每次 chat completions 请求末尾，「typically is like 80% of the whole request」；之所以没被多收五倍钱，是因为 OpenAI 等有「a semantic cache」，和上次请求相同的 80% 不重算，否则「four or five times more expensive」。
资料：OpenAI 文档写的是前缀精确匹配：「Cache reuse requires the entire rendered prefix to match」（[OpenAI prompt caching](https://developers.openai.com/api/docs/guides/prompt-caching)）。见出入 2。

**Q4 [37:20] skillbox 和 toolbox 里东西那么多，context 预算怎么管？**
context window 可以想成一个 n 维矩阵，起点不是 0，「closer to like 10,000」。他读过一篇比较各 harness 首次请求放多少 token 的文章：「Pi … the most minimalistic and the most efficient … about 6,000 tokens」，其他 harness「more like 30 40,000 tokens」。做法是让 agent loop 控制一切：每次请求先由模型判断要做什么，再组装 context，检索最相关的三个 tools、三个 skills，可选 reranker（见第 12 节）。
资料：各方测量差别很大：有文章说 Pi 的 system prompt 加工具定义不到 1,000 token，Claude Code 约 1 万到 2.4 万；一次单句测试是 Pi 1,358 对 Claude Code 31,012（[Daniel Vaughan 比较](https://codex.danielvaughan.com/2026/04/19/system-prompts-compared-codex-gemini-claude-code/) 等二手测量）。讲者说的那篇没找到，「6,000」【待确认】。

**Q5 [40:49] 和记忆层交互最有效的方式是什么？（问题部分听不清）**
「keep it simple」：别建太多抽象；先只做短期和长期两个结构；agent 觉得短期记忆里某样值得留，就「promote it to the long-term memory」，先存成原始文本；再向量化，在 loop 的 observe 和 reason 之间加一步「recall long-term memory」，向量距离够近才放进 context，太远就丢弃。

**Q6 [42:55] MCP 有什么用？（问题听不清）**
这个 workshop 没用 MCP；在数据库场景，MCP 解决的是让 agent 写 SQL 时不幻觉：SQL 有「like 15 engines」（Postgres、Oracle Database……），方言细节会让查询失败；所以预先写好、验证过的查询，通过 MCP 给 agent 用，而不是每次动态生成。

**Q7 [44:21] 为什么把 skills 做成 embeddings，而不是放文件里搜？**
放文件里只能用正则 / 关键词（grep）匹配，文件变大后效率下降，「not exponentially, quadratically, let's say」；预计算的 embedding（「384 minimum to a 1,000 plus dimensions」）捕捉了语义，便于 semantic search。再进一步是 hybrid search：semantic + keyword，用 **RRF**；notebook 里有一张图，两种检索结果之间「this gap that you see at the middle」就是 hybrid 比单独任一种更相关的部分；多一点计算，检索质量更好。
我的理解：grep 随文件规模的复杂度说法（「quadratically」）没有依据，一般是线性，【待确认】讲者原意。

**Q8 [47:30] 应该从头写自己的 harness 吗？（问题部分听不清）**
不期待大家去造新的 Claude Code；举手问谁喜欢 Pi，「less than I expected」。观点：为具体问题做小的 custom harness；例：顾问可以给金融客户一个 harness，给别的客户另一个；解 Navier-Stokes 那类数学题的 agent 没人在环，需要默认自动 compaction / summarization 和「infinite or near infinite patience」，而这个 workshop 的 loop 只有 8 步。「I would start from scratch because that way you can … strip 90% of what you don't need」；一切都围绕 chat completions API，Anthropic 和 OpenAI 各有标准，把和模型通信当作 harness 的一个 plugin，「the smaller the everything is, the better it is」。

（视频 50:14）讲者让大家自己开 codespace 做 notebook，他下台巡场；「I thought like 15 minutes or 20 minutes went by, but one hour went by」。视频 51:40 结束。

## 勘误表（YouTube 字幕 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| workshopwaiting room.com / workshopwingroom.com / workshop waiting room.com | workshopwaitingroom.com | 站点、talk 页 |
| code face / code spaces | Codespaces | 上下文 |
| Andrew Ang | Andrew Ng | 上下文 |
| deep learning AI | DeepLearning.AI | 课程页 |
| open AAI / openi | OpenAI | 上下文 |
| cloud code / clot / codeex / codec | Claude Code / Claude / Codex | 上下文 |
| spy | Pi | 上下文（同一句列举 harness；Q8 讲者说「a show of hands for who likes pi」） |
| open claw / hermes agent | OpenClaw / Hermes Agent | 上下文 |
| grog model / rock model 4.3 | Grok 4.3 | OCI release notes |
| cloud Oracle | Oracle Cloud (OCI) | 上下文 |
| skill MD | SKILL.md | 上下文 |
| GP | grep | 上下文 |
| posgres / VSQL | Postgres / （不确定，可能是 MySQL 或 SQLite） | 上下文，【待确认】 |
| navir stocks | Navier-Stokes | 上下文 |
| premputation / premp compute | precomputation / precompute | 上下文 |
| the advertise limit | the advertised limit | 上下文 |

## 出入

1. **简介 vs 讲解**：日程简介写 Oracle AI Database、OAMP、ONNX in-database embeddings、Meridian Bank 数据集、「~100-line loop」；视频讲解没提这些名字，只说连数据库、vector store、Grok 4.3 on OCI。讲解里提到「openi credits」，而演示用的是 OCI 上的 Grok，两者可能都用过，【待确认】
2. **「semantic cache」**：讲者说 OpenAI 等用 semantic cache，请求有 80% 相同就不重算；OpenAI 文档的 prompt caching 是前缀精确匹配，不是语义相似
3. **Pi 的首个请求 token 数**：讲者说约 6,000，其他 harness 3–4 万；公开测量里 Pi 的 system prompt 和工具定义多在 1,000 以下，数字差别大，依赖版本和装了哪些工具
4. **instruction hierarchy 的层数**：第一次讲 system 最重要、user 第三；Q&A 里说 skills 是「four」层里权重最低。没有给出依据文献
5. **课程**：讲者说和 Andrew Ng、DeepLearning.AI 做的「building adaptive AI agents」；Oracle 博客确认课程和 DeepLearning.AI 合作、讲师是 Nacho Martinez 和 Casius Lee，Andrew Ng 的角色未见
6. **时长**：日程是 1 小时 workshop，视频 51:48；讲解加问答占满，大家动手做 notebook 的部分不在视频里

## 【待确认】清单

1. OpenAI「10,000 agents」解数学题指的是什么
2. 「limit is a million because of the KV cache」的依据
3. 「tools and skills 占 context 50% 以上 / 请求的 80%」的依据
4. Pi 首次请求约 6,000 token 的出处（讲者说读过一篇比较文章）
5. Andrew Ng 和 Building Adaptive AI Agents 课的关系
6. 同事 Richmond 是否为 Richmond Alake
7. slide 上 GitHub repo（其他 workshops 合集）的名字
8. 字幕「VSQL」指哪种 SQL 引擎
9. grep「quadratically」变慢的说法
