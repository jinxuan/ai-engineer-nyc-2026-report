[English](../10-13-0900-vault-making-unstructured-data-work-for.md) | **中文**

# Bridgewater 的 Vault：agent 好不好用，取决于背后的数据和 connector

*在 AI Engineer NYC 2026 上，Bridgewater 的 Sam Green 和 Aaron Linsky 讲了公司怎么建起 Vault：一个超过 1 亿篇文档、同时为人类投资者和 agent 调过的资料库。两人认为真正的回报在于 agent 不再等着被提问，而是主动把投资者引向值得关注的地方。*

**Featured Keynote（Main Stage）· 2026 年 10 月 13 日（周二）· 9:00–9:18am · Simons Room (Metropolitan East)**
**讲者：** Sam Green，Team Lead, Applied AI（AIA Labs），Bridgewater Associates；Aaron Linsky，Head of Product Engineering，Bridgewater Associates

---

当天 Main Stage 的第一场 keynote 来自 Bridgewater Associates。Sam Green 把 Vault 介绍为「我们在 Bridgewater 搭建的数据平台，把 unstructured data 变成投资流程里的 first-class asset」，然后列出建设过程中的三条教训。

![The Vault turns unstructured data into a first-class asset](../../slides/10-13-0900-vault-making-unstructured-data-work-for/02.jpg)
*开场先亮出三条教训。*

第一，人和 agent 对数据平台的需求很不一样，「所以要分别为这两种需求来建」。第二，一个工具能同时满足两组需求时，影响最大。第三，「the real prize」（真正的大奖）是让 agent 成为「投资流程里的 proactive participant」，而不是「给人用的 reactive tool」。

## 1. 五十年的结构化数据

Green 从 Bridgewater 的历史讲起。50 年来，公司一直在追求「对市场和经济如何运作的全世界最好的理解」，并把它变成「一套系统化的编码，可以拿来长期 stress test 我们的逻辑」。最初是「黄色便签本上的一组 if-then 规则」，后来演变成以人为中心、持续复利的过程。最近公司在建 AIA，「the artificial investment associate，把机器放到这个持续过程的中心」。

![Fifty years of compounding](../../slides/10-13-0900-vault-making-unstructured-data-work-for/03.jpg)
*从标着「The 1980 bond system」的黄色便签本，到以人为中心的学习流程，再到 AIA 的 reasoning engine。*

slide 把这套理念压成三步：跟踪世界上正在发生什么；找出公司已编码的理解还没覆盖的东西；改进并复利。

![From tabular data to unstructured data](../../slides/10-13-0900-vault-making-unstructured-data-work-for/04.jpg)
*2023 年以前是结构化数据上的时间序列模型；2023 年以来是语言模型处理非结构化内容。*

在大部分历史里，工作就是在结构化数据上建时间序列模型。这些数据要精心构建，才能「安全地用于研究流程，并对齐内部 ontology 和 world model」。语言模型打开了大得多的数据世界：新闻、研究报告、transcripts、政策文件，以及「以前只飘在空气里」的内部交流，比如内部 memo、会议记录和公司内部的辩论。

Green 说，这些数据也需要同样的处理：「仔细清洗、治理，准备好既能用于今天的 point-in-time 研究问题，也能用于长期的系统化研究」。看了一圈数据供应商和平台，「没有一家能给我们所需的全部控制权」。（官方简介还提到团队最初想直接买，台上没讲这一段。）于是 Bridgewater 自建了 Vault。现在它有超过 1 亿篇文档，每天从 135 个外部来源新增几千篇，再加上 Bridgewater 的内部数据。每篇文档都对齐公司的 ontology，标注国家、概念、市场，「最重要的是文档讨论的时间段」。

## 2. 同一个问题，两个截然不同的答案

为了说明这件事的分量，Green 向一个 enterprise chat client 问了一个「相当基础、有代表性的投资问题」：「AI 基础设施投资怎样影响韩元？市场参与者预期这会持续吗？」讲者没说是哪家客户端、哪个模型，slide 只写「same question, same LLM」。

![Standard tool: out of date, under-sourced](../../slides/10-13-0900-vault-making-unstructured-data-work-for/05.jpg)
*走开放网络的那次搜了 15 个网站。*

![The standard tool builds its answer on one broker note](../../slides/10-13-0900-vault-making-unstructured-data-work-for/06.jpg)
*答案靠的是一篇 broker note。*

agent 上网找到 15 个看起来相关的来源，但回答只用了一两个，末尾还承认没找到足够材料来确认市场共识。更糟的是，它最依赖的那篇 note「早在 2026 年 7 月就发了」，而提问是在 10 月初。「在 AI 时代、在投资领域，……三个月是非常长的时间，」Green 说。

![The Vault: searching with time as part of the query](../../slides/10-13-0900-vault-making-unstructured-data-work-for/07.jpg)
*接上 Vault 后，同一个模型把范围缩到 10 月 1–8 日，跑了三次查询，引用了七个来源。*

接上 Vault 后，同一个查询的表现完全不同。连接本身提示 agent 按特定时间窗口找，也就是 10 月 1 日到 8 日。三次有针对性的查询返回 52 篇候选文档，最终答案引用了七个可信来源，而不是一两个。「要让 agent 发挥作用，需要好的数据和对的 connectors，」Green 说完，把话筒交给 Linsky。

## 3. 为两种读者做搜索

「搜索很难，」Linsky 开口说，「如果要为两种不同的消费者设计，就更难了。」

![Search is hard](../../slides/10-13-0900-vault-making-unstructured-data-work-for/08.jpg)
*「And harder when designing for two very different consumers.」*

![Two very different consumers](../../slides/10-13-0900-vault-making-unstructured-data-work-for/09.jpg)
*人每分钟读约 300 词，agent 读 30,000 多词。*

| | 人 | Agent |
| --- | --- | --- |
| 敏感于 | Precision | Recall |
| 需要 | High signal-to-noise ratio | Performant data access |
| 规模 | 小 | 巨大 |
| 消化速度 | 慢 | 非常快 |
| 阅读速度 | ~300 words per minute | 30,000+ words per minute |

人需要 precision，因为能读的就那么多：「读得快的人每分钟大约 300 词，我自己慢得多。」agent「对 precision 没那么敏感。数据在那儿，它们就能找到」，而且读得快大约一百倍。

![Five dimensions](../../slides/10-13-0900-vault-making-unstructured-data-work-for/10.jpg)
*Recall、precision、filtering、entitlements 和 scale。*

Linsky 列了五个重要维度（recall、precision、filtering、entitlements、scale），重点讲其中三个：给人的 precision、给 agent 的 scale，以及两者都需要的 filtering。

## 4. Precision：先把抱怨做成 benchmark

Vault 灌满之后（「我们数据和法务团队的一次壮举」），recall 不再是紧迫问题。头号抱怨变成了「Vault search is bad」（Vault 的搜索很烂）。「作为产品负责人，听到这话很难受，但我的回应是：那就做个 benchmark，」Linsky 说。

![#1 Complaint: "Vault search is bad"](../../slides/10-13-0900-vault-making-unstructured-data-work-for/11.jpg)
*约 200 个人工标注的例子，以及由此变得可衡量的五项改进。*

团队收集问题，人工找出最佳文档，也标出那些看起来有用其实没用的。约 200 个例子，不到一周做完：「不好玩，也不光鲜，但非常有用。」关键指标是 precision at 10，即前 10 个结果里有多少是对的文档。起点约 60%：「大家说得对，确实很烂。」

有了 benchmark，每项改进都能衡量。slide 列了五项：修复文档 ingestion 和解析问题、调整 Reciprocal Rank Fusion 权重、加 reranker、query expansion、agentic search。Linsky 说，做完之后「到了 90-95%」。（台上 Linsky 说的是「recall at 10 or precision at 10」；前面定义的指标是 precision at 10，视频说明写的是「over 90%」。演讲没有拆分每项改进各贡献多少。）

## 5. Filtering：在 pre-filtering 和 post-filtering 之间

![We want the most relevant documents](../../slides/10-13-0900-vault-making-unstructured-data-work-for/12.jpg)
*典型过滤条件：截至某日期、某个来源（如 U.S. Federal Reserve）、某种货币。*

生产 traces 显示，几乎每个查询都带条件：截至某个日期、来自某个来源、关于某个资产。「这些都需要高级过滤，我们第一个大的性能问题就出在这里。」

![Postfiltering v Prefiltering](../../slides/10-13-0900-vault-making-unstructured-data-work-for/13.jpg)
*Post-filtering：先搜整个索引，再按 metadata 过滤靠前的结果。*

Linsky 讲了向量数据库的两种经典过滤方式。Post-filtering 先用向量索引，再按 metadata 过滤靠前的结果，速度快，但「recall 可能很低，因为全局最相关的结果未必符合你的过滤条件」。Pre-filtering 先过滤，再对每个匹配项算距离，recall 很好，但可能很慢。Bridgewater 需要介于两者之间的方案，最后换成了 turbopuffer。它的「native filtering」「只看包含匹配项的最近几个聚类，只给这些打分」，结果「像 post filtering 一样快，recall 又接近 pre-filtering」。turbopuffer 自己介绍这项技术的文章描述的是同一设计：attribute index 和向量索引的聚类挂钩，query planner 只访问含匹配文档的最近聚类。

## 6. Scale：agent 按秒查询

![Need to serve at Agent Scale](../../slides/10-13-0900-vault-making-unstructured-data-work-for/15.jpg)
*人每小时 100 次查询，agent 每秒 100 次以上。*

Linsky 说，人的规模「相当平常」：300 个用户，每小时约 100 次查询，读返回的内容要花几分钟。agent 高出几个数量级，「轻松每秒 100 次以上，agent swarm 轻松上千」，而且几秒就读完文档。turbopuffer「也轻松扛住了」。

Linsky 跳过了 entitlements，说当天晚些时候会讲。（日程上 Linsky 在 11:55am 还有一场「Can Your AI Analyst Keep a Secret?」，和 turbopuffer 合讲如何在检索层执行权限。）

## 7. 同样的习惯，换个新家

![Vault covered all five dimensions](../../slides/10-13-0900-vault-making-unstructured-data-work-for/17.jpg)
*「It unlocked new workflows. It didn't change how people work.」*

Vault 覆盖了全部五个维度，也解锁了新工作流，「但没有改变研究员的工作方式。他们还是不会每天早上来 Vault」。这是 Linsky 的第二条教训：「工具再厉害，也不能直接扔到投资者桌上就指望他们用。我们得把他们的老习惯迁移过来。」

![Same habits, new home → new habits](../../slides/10-13-0900-vault-making-unstructured-data-work-for/19.jpg)
*每个邮件习惯在 Vault 里都有对应，再往上叠新能力。*

很多研究员活在邮件里。研究报告直接从 broker 发来，他们订阅列表，用规则把一切过滤进文件夹。「它管用，大家熟悉，而且知道自己没漏掉什么。」Vault 把每个习惯都重建了一遍：

| 在邮件里 | 在 Vault 里 | 由此打开的新习惯 |
| --- | --- | --- |
| Research received by email | Searchable in the Vault | Ask questions across all of it |
| Subscribed lists | Saved searches | Dynamic feeds by topic |
| Folders | Saved collections | Hand feeds and collections to agents |

「还是同样的习惯，只是换了个新家，」Linsky 说。这些到位之后，用户可以跨全部数据提问、关注按主题的 feed、把集合交给自己的 agent，「那时我们才明确有了 product market fit」。

![Wherever the work happens](../../slides/10-13-0900-vault-making-unstructured-data-work-for/20.jpg)
*给人和给 agent 的入口。*

接着用户希望到处都能用 Vault。给人的有 chat、手机 app、Slack 和 IDE；给 agent 的有 API 和 Python client、MCP server，还有一个带 skill 的命令行工具供 coding agent 使用，「因为 agent 爱 CLI」。

## 8. Vault 撑起了什么

![What agent access makes possible](../../slides/10-13-0900-vault-making-unstructured-data-work-for/21.jpg)
*Human accelerants、agent sandboxes 和系统化决策，都从 Vault 取数据。*

| 分组 | 系统 | slide 上的说明 |
| --- | --- | --- |
| Human accelerants | Pocket Analyst | 「Our first AI analyst. Hours of research in minutes, using the Vault.」 |
| | Researcher | 「The next generation of our Pocket Analyst capable of performing long time horizon financial research projects around our entire research circle.」 |
| Agent sandboxes | Custom RL models | 「Models we train with reinforcement learning to make better use of the Vault.」 |
| Systematic decision making | Forecaster | 「AI-generated macro forecasts.」 |
| | Investment signals | 「Systematic strategies that turn documents into structured signals, sourcing every document through the Vault.」 |

Linsky 说，Vault 帮公司「更快地绕着研究循环转」。Pocket Analyst 是 Bridgewater 第一个 AI analyst，依靠 Vault，「几分钟、几美元就能做完几小时的研究」。Researcher 把同样的思路「拉长到几天甚至几周」。自己训练的模型和 foundation model 一起对 Vault 数据做推理，供给系统化决策：Forecaster 生成「AI 生成的宏观相关预测」，investment signals 把「非结构化数据变成结构化、可交易的策略」。（slide 写明自定义模型用的是 reinforcement learning；Linsky 口头只说训练了自定义模型。）

## 9. 从 reactive 到 proactive：Investor Workbench

Green 回到台上讲第三条教训。真正的大奖，是把 agent 从回答人类提问的资源，变成「主动参与者，把高价值的专业人员引向最需要他们注意的地方」。

![Building the Investor Workbench](../../slides/10-13-0900-vault-making-unstructured-data-work-for/22.jpg)
*Reactive 是被问才答，proactive 是引导注意力。界面上写着「What am I missing?」*

Bridgewater 的做法是 investor workbench：「agent 一直在线，跟踪对这位投资者重要的事，把最需要关注的领域呈现给他们。」系统提出一个主题，附上几条信息，由投资者决定深挖还是忽略。

![Summary and evidence, ready for action](../../slides/10-13-0900-vault-making-unstructured-data-work-for/23.jpg)
*主题卡片「AI impact on currencies」，带 agent summary 和 activity feed。*

![Internal and external evidence highlighted](../../slides/10-13-0900-vault-making-unstructured-data-work-for/24.jpg)
*同一张卡片，证据栏被高亮。*

示例卡片回到了开场的问题：AI 对货币的影响。卡片上有一段 agentic summary 供快速浏览，还有「agent 所依据的确切证据，让他们自己判断这条建议从何而来」，并能直接跳进 Linsky 介绍的研究工具。

![The Vault captures the human + agent collaboration](../../slides/10-13-0900-vault-making-unstructured-data-work-for/25.jpg)
*一份 agentic analysis，标题「Understanding AI's Impact on FX Markets」。*

投资者和 agent 一起深挖时，「两者协作的结果会自动存回 Vault」。

## 10. 复利

![The Vault enables compounding](../../slides/10-13-0900-vault-making-unstructured-data-work-for/26.jpg)
*收集对的信息，把综合结论贡献回语料库。*

「Vault 让复利成为可能，」Green 说，因为它把对的内外部信息放到投资者和 agent 手边，「并自动捕获他们产出的综合结论，放回语料库，供下一轮使用」。Green 总结说，让 agent 起作用需要的是「对的 context，以及对你所在世界正在发生什么的正确理解」。最后 Green 给听众留了一个问题：「Does your AI know what's going on in the world? And does it remember what your firm has learned about it?」（你的 AI 知道世界上正在发生什么吗？它记得你的公司学到了什么吗？）「我们也在教我们的 AI。」

---

**链接：** [日程](https://ai.engineer/nyc/2026/schedule?session=a84ca3a8-cc2f-4b50-a635-fc380f6f04f6) · [视频](https://www.youtube.com/watch?v=CLve4Nx_RwI) · [直播（第一天，从 7:24 起）](https://www.youtube.com/watch?v=akEqiXFo3GM&t=444s) · [Native filtering for high-recall vector search](https://turbopuffer.com/blog/native-filtering)（turbopuffer）· [How Bridgewater's AIA Labs Built PAT](https://www.bridgewater.com/aia-labs/how-bridgewaters-aia-labs-built-pat-the-ai-pocket-analyst-tool) · [Can Your AI Analyst Keep a Secret?](https://ai.engineer/nyc/2026/schedule?session=88f71c9f-f0a9-4f42-96c0-0e1492bad1c2)（Linsky 当天稍晚的场次）

本文依据官方视频字幕、第一天直播、slide 照片和以上资料整理。这场演讲没有 Q&A。[精确版笔记](../precise/10-13-0900-vault-making-unstructured-data-work-for.md)（中文）保留了时间戳，以及演讲、slide 和各处介绍之间出入的完整记录。
