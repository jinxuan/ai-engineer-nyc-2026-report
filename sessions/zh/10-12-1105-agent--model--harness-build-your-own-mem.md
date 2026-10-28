[English](../10-12-1105-agent--model--harness-build-your-own-mem.md) | **中文**

# 模型改不了，那就造 harness：从记忆层搭起一个反洗钱 agent

*在 AI Engineer NYC 2026 上，Oracle 的 Nacho Martinez 认为，agent 的质量取决于冻结模型外面那层 harness（记忆、context 管理、tools 和 skills 的检索、loop），并带大家走了一遍 workshop：为一个标记可疑银行交易的自主 agent 搭这样一个 harness。*

**Workshop · 2026 年 10 月 12 日（周一）· 11:05am–12:05pm · Sutton Room (NY Ballroom W)**
**讲者：** Nacho Martinez，AI Developer Advocate，Oracle

---

session 简介用一句话交代了前提：「Agent = Model + Harness. The model emits tokens; state, memory, retrieval, tool dispatch, identity, retries, and budgets. All these concepts are harness code — and that is where agent quality is won or lost.」（模型只负责吐 token；状态、记忆、检索、工具调度、身份、重试、预算都是 harness 代码，agent 的质量就在这里分出高下。）

Nacho Martinez 自称「AI developer advocate，骨子里是个 data scientist」，手上有一小时，原计划讲 10 到 15 分钟，然后让大家自己做练习。在 workshopwaitingroom.com 登记 GitHub handle 的人会被加进 workshop repo，可以在 GitHub Codespaces 里跑，费用由 Martinez 出（「这个域名我花了太多钱，所以用起来」）。结果讲解加提问几乎占满了整个小时。「我以为才过了 15 或 20 分钟，结果一小时过去了。」

Martinez 提到最近一门免费的 DeepLearning.AI 短课 Building Adaptive AI Agents，讲模型怎么「随时间适应、随时间学习」，是这次 workshop 涉及的内容之一。（Oracle 的发布文章列出的讲师是 Martinez 和 Casius Lee；Martinez 把这门课归于 Andrew Ng 和 DeepLearning.AI。）

## 1. 场景：一家不睡觉的银行

听说很多参会者在 Wall Street 工作，Martinez 把 workshop 围绕一家银行来设计。出发点是 frontier labs 的新闻：派出成千上万个 agent 去攻克几百年没解决的数学问题，中间没有人参与。Martinez 说，agent 自己做不到：「context window 是有限的」，而记忆「是大语言模型本身没有的东西」。这些部分得工程师来造。「我们不用那么关注 frontier 模型……来做 harness 吧，那才是有意思的部分。」

场景是一家中型银行，可疑交易分成五类。Martinez 的例子：一笔 100 万美元的入账，接着连续三天每天在 ATM 取 5 万美元。agent 要对这类交易做判断，至少要标记出来，让银行员工在每一笔上少花时间。session 简介补充了数据集的细节：一家虚构的 Meridian Bank，25 家分行，90 天里大约 1,200 笔交易，跑在 Oracle AI Database 上。

## 2. Agent stack，以及工程师能控制哪一层

Martinez 描述的 stack 有五层：应用层（聊天界面、手机 app）、模型（「推理核心」，「我们说了不算」）、数据层、基础设施和算力。workshop 聚焦数据层，因为「我们能操纵、能动手的东西都在这一层」。

数据层里有 agent loop（「可以说是最重要的部分之一」）、连接外部数据源的 gateway 和 MCP、memory layer、对工作 context 做 summarize、compact、压缩的 context engineering，以及 tools 和 skills。

Martinez 说，AI 应用有四种形态：LLM chatbot、RAG（「基本上就是一个美化了的 vector search」）、LLM 驱动的 workflow，以及 AI agent。Claude Code、Codex、Pi、OpenClaw、Hermes Agent 这类工具是后两者的组合：workflow 提供 automation，agent 提供 autonomy，合在一起叫 agentic system。Martinez 说，工程师每天都在用这些系统，自己做应用时却很少这样设计。

## 3. agent 是什么，harness 是什么

Martinez 把 agent 比作人。LLM 提供推理，像大脑。数据库提供记忆。工具是它在操作系统上行动的四肢。输入是它的感官。它的 context 像注意力一样有限：「我们没法同时看七部电影。」

推理来自 frontier labs。记忆、tool use 和感知是工程师能控制的，「这就是 harness 本身」：「为搭出……tool use、记忆和感知所需要的全部脚手架」。Martinez 称之为「当下 AI 里最有意思的话题」，因为换一种配置，同一个 frontier 模型在某项任务上就会变好或变差。

harness 有「七个，如果算上 continual learning 是八个组件」，每个都是一个决策：

| 组件 | Martinez 描述的决策 |
| --- | --- |
| Model | 调用哪个模型 |
| Storage | 数据放在哪 |
| Encoding and retrieval | 怎么度量向量之间的距离，才能取回对的数据 |
| Memory | 短期加长期，还是别的分区方式 |
| Semantic layer | 双方没说出口、但都默认的东西 |
| Agent loop | 什么在自动、反复地驱动 agent |
| Context engineering | 什么进入、什么离开 context window |
| Continual learning | 系统怎么随时间变好 |

讲 semantic layer 时，Martinez 拿现场举例：「我是讲者，你们在听。你们现在没在唱歌。为什么不唱歌？因为不合适。」

## 4. 文件还是数据库

关于「一切放文件」和「database maximalists」之争，Martinez 展示了多个 agent 同时写一个文件的情形，问现场现在的 workaround 是什么。一位参会者答：worktrees。Martinez 说，文件在并发写上有天生的问题：哪次写在先，文件改动前已经读过的人怎么办。「worktree……这个 workaround，数据库 30 年前就解决了」：有了 atomic、consistent、isolated、durable（ACID）的写入，冲突就不会出现。文件适合存大量非结构化数据，数据库适合别的活。「不必只选其中一个。」

## 5. 记忆：短期、episodic、procedural、semantic

Martinez 说，agent memory 指让 agent 使用信息、精炼信息、「在该用的时候恰当地回忆起来」的那些机制，分哪几类由工程师决定。workshop 用的是这种划分：

| 记忆 | 存什么 |
| --- | --- |
| Short-term | context window 本身；对话结束就消失 |
| Long-term: episodic | 发生过什么：对话、tool logs、以往对话的摘要 |
| Long-term: procedural | 怎么做事：skills，以及由一串 skills 组成的 workflows |
| Long-term: semantic | 你是谁、你的偏好、你最喜欢的框架 |

Martinez 认为不能把一切都放进短期记忆，因为它有限，而且「attention matrix 越大……表现越差」。同一个问题，从 0 token 开始解，比 context 里已经有 256,000 token 时「准确得多」，超过某个点，「性能会烂得一塌糊涂」。（这和广泛报道的长 context 性能下降一致，但 Martinez 说它对每个模型、每类问题都成立，没有引用具体研究。）

让 context 保持小有两种办法。context summarization：让模型写一份简短摘要，下一段对话从摘要开始。compaction：把对话分块存进数据库或文件，给一个 ID，agent 以后需要时按 ID 加 offset 取回，「有点像读文件」。

## 6. Agent loop

agent loop 是「司机」。context assembly 把 skills、tools、以往对话、文件和数据库记录拉进来。模型通过一次 API 调用推理，决定做什么。然后 loop 重复，「想的话可以自动、无限地跑」。Martinez 的 workshop loop 设了「maximum patience」：最多 8 次迭代，每次最多 60 秒推理，部分原因是保护 Martinez 自己的 API credits；这些都可以配置。简介里把它叫作「a bounded agent_turn loop」。

在 workshop 的 notebook 里，context assembly 每次请求检索最相关的三个 tools 和三个 skills（对两个 vector store 各做 k = 3 检索）。Martinez 没用 reranker，但建议加一个 embedding model 加 reranker，从三个里挑出最好的一个，因为「任务可能一个就能解决」。

## 7. Toolbox 和 skillbox 模式，以及 skill distillation

Martinez 装了大约一百个 skills，每个「大概 3,000、5,000 token」。全部加载，对话还没开始 context 就满了。workshop 的办法是把 tools 和 skills 做成向量，loop 需要时按相似度只检索最相关的几个，用完再卸掉。Martinez 估计，在某些情况下 tools 和 skills 会占掉「context window 的 50% 以上」。

skill distillation 把以往的对话和运行沉淀成可复用的东西。最简单的形式是写一个 SKILL.md 放进同一个 skillbox。Martinez 称 Hermes Agent 是这方面「的创新者之一」。比如一个记录了前端配色偏好的 skill，下次用户提到「coloring style」时就会因为 embedding 匹配被自动选中。合起来就是 dynamic skill discovery：loop 组装 context 时拉进对的 skills 和 tools。

## 8. 冻结的模型怎么变好

「冻结的模型怎么变好？」工程师改不了 frontier 模型的权重，但能控制数据层。Martinez 把 continual learning 分成三类：

| 类别 | 原理 |
| --- | --- |
| Parametric memory | 改模型权重，例如 low-rank adaptation 或 fine-tuning |
| Representational memory | 通过 embeddings 起作用，例如为问题换一个更好的 embedding model |
| Non-parametric memory | 上面讲的一切：记忆、检索、skills、context |

workshop 只讲了第三类；课程三类都讲。

## 9. Instruction hierarchy

Martinez 说，每个 chat-completions 请求都带着「一个内在的 instruction hierarchy」。system prompt 权重最高，用户请求排第三。附在最下面的 tools 和 skills 权重最低。把一切塞进 system prompt，层级就被压平了。之后展示的一张 slide 拆解了一个典型请求：

| 部分 | Martinez 给的典型大小 |
| --- | --- |
| System prompt | 5K–10K tokens |
| Developer（应用指令） | — |
| User query | 「可以忽略」，100 到 1,000 tokens |
| Tool outputs 和 retrieved documents | 「请求的 80%」，35,000–100,000 tokens |

## 10. Workshop

workshop 的界面设想的是一位反洗钱分析员和一个全天候在后台运行的 agent 协作（「它不睡觉」），agent 通过 harness 连着数据库。它自主做判断，但不直接执行：「它不会毁掉别人的生活」去冻结账户，而是把判断记下来给人复核，在调查阶段省时间。

demo 里，部署好的应用不断生成假交易，有的正常，有的有问题。一个跑在 Oracle Cloud Infrastructure 上的 Grok 4.3 模型判断哪些要加进可疑名单。界面上能看到 agent 的 context window、数据库里的客户和其他数据、一段实时分析（agent 调用 toolbox 里的工具、在 vector store 里检索知识、生成 suspicious activity reports），以及银行最近 30 个事件。练习本身是一个带 to-do 的「student」notebook，第一步是连上模型和数据库。

## Q&A

提问发生在演示过程中；录音里大多听不清，下面按 Martinez 的复述或回答整理。

### 记忆设计

**episodic memory 会不会和以前的对话混在一起？** 看你给它多少权重，这也是 instruction hierarchy 重要的原因。episodic memory 很重要的时候，「你可能会想把它向量化，而不是只存成原始文本」。workshop 里不会冲突，「因为我们在架构上把各个记忆……组件分得很清楚」。

**跟记忆层打交道，最有效的方式是什么？** 「保持简单。」先只做短期和长期两种记忆。context window 里某样东西看起来值得留，就「把它提升到长期记忆」，先存成原始文本。之后再向量化，在 loop 的 observe 和 reason 之间加一步「recall long-term memory」：和查询在向量空间里足够近的记忆进入 context，远的丢掉。

### Skills、tools 和 context 预算

**skills 能放进 system prompt 吗？** 能，但在层级的「四层里 skills 权重最低」，Martinez 把它们当作「一个即插即用的地方」。Martinez 和同事 Richmond 在试验为第三方的「skill hubs 和 tool hubs」（比如一组 MCP servers）预先算好 embeddings，让 agent 去检索。Martinez 说，tools 和 skills 通常占每个请求的 80% 左右。按 Martinez 的说法，请求之所以没贵上「四五倍」，是因为各家提供商有「semantic cache」，和上一次请求重复的内容不再重算。（OpenAI 文档描述的 prompt caching 是前缀精确匹配，「整个渲染后的前缀」都要一致，而不是语义相似。）

**toolbox 和 skillbox 里东西这么多，context 预算怎么管？** context window 从来不是从零开始，而是「更接近 10,000」。Martinez 引了一份比较各 harness 首个请求发多少内容的文章：Pi「最极简、最高效」，大约 6,000 token；其他的「更像 3 万、4 万」。（独立测量的结果因版本和安装的工具差别很大，有几份把 Pi 的 system prompt 加工具定义算在 1,000 token 以下；Martinez 引的那份比较没有找到。）workshop 的做法是让 agent loop 控制一切：模型先判断请求需要什么，loop 再用最相关的三个 tools 和 skills 组装 context。

**为什么把 skills 做成 embeddings，而不是放在文件里？** 文件只能用正则或关键词搜，文件越大越慢。预先算好的 embedding「最少 384 维，到 1,000 多维」，能抓住关键词搜索漏掉的含义。两者结合，也就是用 reciprocal rank fusion（RRF）的 hybrid search，效果更好。notebook 里有一张图，标出关键词结果和语义结果之间的那一带，在那里 hybrid 检索比单用哪一种都好：「多一点算力，但检索质量会更好。」

### 数据库和自定义 harness

**MCP 解决什么问题？** 这个 workshop 没用 MCP。Martinez 说，在数据库场景里，MCP 可以防止 agent 瞎编 SQL：有「大概 15 种引擎」，方言细节一多，生成的查询经常跑不通。把写好、验证过的查询通过 MCP 提供给 agent，就能避免。

**该自己造 harness 吗？** Martinez 并不指望谁去造一个新的 Claude Code；现场举手喜欢 Pi 的人「比我预想的少」。重点是为具体问题造小的 custom harness。顾问可以给金融客户造一个，给数据或约束不同的客户再造一个。一个在没人参与的情况下攻数学难题的 agent，需要自动 compaction 和「无限或接近无限的 patience」，和 workshop 那个 8 步的 loop 不一样。「我会从零开始，因为这样你……能砍掉 90% 用不着的东西。」一切都围绕 chat-completions API 构建，所以把和模型的连接当作 harness 的一个 plugin 来看，就很清楚「最后……一切越小越好」。

随后 Martinez 让大家去各自的 codespace，下台巡场帮忙。Martinez 表示会继续为 codespace 付费，大家之后可以接着做，并请大家去 Oracle 展位，展位开到周三晚上。

---

**链接：** [日程](https://ai.engineer/nyc/2026/schedule?session=898cb76b-41f3-47c0-ba31-6a5f8df40117) · [视频](https://www.youtube.com/watch?v=KtBivPzmOM8) · [Talk 页面](https://ai.engineer/talks/KtBivPzmOM8) · [Workshop 登记](https://workshopwaitingroom.com/) · [Building Adaptive AI Agents (DeepLearning.AI)](https://www.deeplearning.ai/short-courses/building-adaptive-ai-agents/) · [Oracle 的课程发布文章](https://blogs.oracle.com/developers/we-just-launched-a-new-course-on-how-to-build-adaptive-ai-agents) · [OpenAI prompt caching 文档](https://developers.openai.com/api/docs/guides/prompt-caching)

本文依据官方视频字幕、session 简介和上面列出的资料写成；这场没有 slide 照片，slide 和演示内容按 Martinez 的口述描述。视频在大家开始动手练习时结束。[精确版笔记](../precise/10-12-1105-agent--model--harness-build-your-own-mem.md)保留了时间戳和没能核实的说法。
