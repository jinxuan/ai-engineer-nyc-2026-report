# Vault: Making Unstructured Data Work for Investing：精确版

> Precise notes for this session, compiled from the official video captions, the official livestream, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材整理完，单独视频和直播逐段对照完。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | 作者拍的 26 张（01–26），全部是这场：直播画面的手机翻拍，下方横幅「Vault: When Fifty Years of Structured Data Isn't Enough · Aaron Linsky · Sam Green · BRIDGEWATER」（01、02 是开场标题页，横幅还没出现）。slide 右下角页码是「NN / 23」，全片 23 页，有几页没拍到。01/02、14/15、16/17、18/19 是同一页的逐步展开 |
| 视频 | [CLve4Nx_RwI](https://www.youtube.com/watch?v=CLve4Nx_RwI)，改名「How Bridgewater Turned 100 Million Documents Into an Edge - Sam Green & Aaron Linsky, Bridgewater」，16:44，上传 2026-10-24。字幕是主要文本。说明里有章节表（见下） |
| 直播 | 第一天直播 [akEqiXFo3GM](https://www.youtube.com/watch?v=akEqiXFo3GM&t=444s)，这场在直播 7:22–23:55。直播时间 ≈ 视频时间 + 7:15（视频 09:09「60%」= 直播 16:24）。和视频措辞逐句一致；视频只剪掉了开头「Just waiting for our slides. There we go.」（直播 7:24） |
| 官方 talk 页 | ai.engineer/talks/CLve4Nx_RwI 返回 404 |
| 其他一手资料 | turbopuffer 博客 [Native filtering for high-recall vector search](https://turbopuffer.com/blog/native-filtering)（Bojan Serafimov，2025-01-21）；Bridgewater 页面 [How Bridgewater's AIA Labs Built PAT](https://www.bridgewater.com/aia-labs/how-bridgewaters-aia-labs-built-pat-the-ai-pocket-analyst-tool)；同日下午 Linsky 的另一场 [Can Your AI Analyst Keep a Secret?](https://ai.engineer/nyc/2026/schedule?session=88f71c9f-f0a9-4f42-96c0-0e1492bad1c2)（日程） |

视频章节（资料：YouTube 说明）：00:00 Intro: three lessons from building the Vault · 01:11 Bridgewater's 50-year research process · 02:35 Why unstructured data needed the same rigor · 03:25 What's in the Vault · 04:14 Open web vs the Vault: a side-by-side · 06:53 Search for humans vs search for agents · 08:19 Benchmarking precision: "Vault search is bad" · 09:32 Fast filtering at scale · 11:35 Getting investors to change their habits · 13:02 What the Vault enables: analysts, forecasts, signals · 14:05 Proactive agents: the investor workbench · 15:44 How the Vault compounds what the firm learns

## 基本信息

- 时间地点：2026-10-13（周二）9:00–9:18am，Simons Room (Metropolitan East)，Main Stage，Featured Keynote（资料：[日程](https://ai.engineer/nyc/2026/schedule?session=a84ca3a8-cc2f-4b50-a635-fc380f6f04f6)）。当天 Main Stage 第一场
- 原题「Vault: Making Unstructured Data Work for Investing」；直播横幅「Vault: When Fifty Years of Structured Data Isn't Enough」；视频改名「How Bridgewater Turned 100 Million Documents Into an Edge」
- 讲者：Sam Green，Team Lead, Applied AI（横幅：「Team Lead, Applied AI · AIA Labs」）；Aaron Linsky，Head of Product Engineering，均为 Bridgewater Associates（slide 页眉、横幅、日程）
- 分工（讲者说 + slide 照片里的讲者小窗）：Green 讲开场、背景和对比例子（视频 00:12–06:53），Linsky 讲搜索和产品（06:54–14:01），Green 收尾讲 Investor Workbench（14:05–16:27）
- 官方简介要点（资料：日程）：前沿模型在投资领域「fundamentally limited by access to information」；Bridgewater 有「one of the world's most comprehensive bitemporal structured data stores」；Vault 是「a centralized, high-quality repository of our best external and internal unstructured data, serving humans and agents alike」；第一反应是买，后来自己建，从点状方案变成平台；Vault 支撑第一代 analyst agent PAT（「used daily by hundreds of Bridgewater investors」），并在支撑第二代 Researcher
- 主持人介绍（直播 6:29）：「two amazing folks from Bridgewater who are in the weeds of production AI every single day」
- 每页 slide 底部有免责声明：「For illustrative purposes only. Please review the "Important Disclosures and Other Information" located at the end of this presentation.」

## 1. 开场：三条教训

- slide（照片 02）「The Vault turns unstructured data into a first-class asset.」三条：1「Humans and agents have different data platform needs」2「The greatest impact occurs when these different needs can be met by a single tool」3「The real prize is to shift agents from reactive to proactive」
- 讲者说（Green，视频 00:12）：Vault 是「the data platform that we've built at Bridgewater to turn unstructured data into a first-class asset in our investment process」。第一条：人和 agent 对数据平台的需求很不同，「you have to build for those different needs separately」；第二条：一个工具同时满足两组需求时影响最大；第三条：「the real prize」是让 agent 成为投资流程里「a proactive participant」，而不是给人用的「reactive tool」

## 2. Bridgewater 的五十年研究流程

- slide（照片 03）「FIFTY YEARS OF COMPOUNDING · Pursuing the best understanding of how markets and economies work.」三张图：黄色便签纸「The 1980 bond system」→ 圆环「Learning process」（中心「Human at the Center」）→「AIA's reasoning engine」。下方三步：1「Track what is going on in the world.」2「Find what our encoded understanding has not yet captured.」3「Improve and compound.」
- 讲者说（视频 01:17）：过去 50 年 Bridgewater 一直在追求「the world's best understanding of how markets and economies work」，并把它变成「a systematic encoding that we can use to stress test our logic over time」；最初是「a set of if-then rules on a yellow legal pad」，演变成以人为中心的持续复利过程；最近在做 AIA，「the artificial investment associate, which puts a machine at the center of this continuous process」
- 我的理解：slide 上的便签纸标注是 1980 年的债券系统；讲者口头没提年份

## 3. 从表格数据到非结构化数据

- slide（照片 04）「From tabular data to unstructured data.」左：「BEFORE 2023 · TABULAR DATA：Time series models on structured data. Built to align to our ontology and world model」；右：「SINCE 2023 · UNSTRUCTURED DATA：Language models with unstructured content. External: News, research, transcripts, policy memos. Internal: Live debate, chat conversations, memos」
- 讲者说（视频 02:14）：结构化数据要「carefully built so that it could be safe for use in our research process and aligned to our internal ontology and world model」；语言模型带来更广的数据：新闻、研究、transcripts、政策文件，以及内部「previously just in the ether」的东西（内部 memo、会议记录、内部辩论）
- 讲者说（视频 02:56）：这些数据需要同样的处理：「carefully cleaned, governed, and made ready for use in both point-in-time research questions today and also our systematic study over time」；看了数据供应商和数据平台，「there was nothing that provided us with all of the control that we needed」，所以自建 Vault
- 讲者说（视频 03:41）：Vault 现在有「more than 100 million documents」，每天新增几千篇，来自「135 external sources」，加上 Bridgewater 内部所有数据；每篇都对齐内部 ontology，富化标注包括 countries、concepts、markets，「and most importantly, time period discussed in the document」
- 出入：官方简介说第一反应是买（「our first instinct, which was to buy it」），讲者在台上只说市场上没有满足控制需求的产品，没讲买的尝试

## 4. 同一个问题：开放网络 vs Vault

- slide（照片 05）「SAME QUESTION, SAME LLM · Standard tool: out of date, under-sourced」：问题「How is AI infrastructure investment impacting the Korean Won? Do market participants expect this to continue?」，界面显示「Searching 15 websites」
- slide（照片 06）同标题，两条：1「Searches the open internet and reads several sites.」2「Builds its answer on one broker note.」右侧是回答截图，有红圈标注
- slide（照片 07）「SAME QUESTION, SAME LLM · The Vault: Searching with Time as Part of the Query」三条：1「Reasons that it needs recent data: October 1 to 8.」2「Searches the Vault: three targeted queries, 52 items.」3「Grounds its answer in 7 trusted sources.」
- 讲者说（视频 04:17）：把一个「fairly basic representative investment question」交给一个 enterprise chat client；agent 上网找到 15 个看起来相关的来源，但回答只用了一两个；回答末尾 agent 自己承认没找到足够文档来确认市场共识
- 讲者说（视频 05:28）：它主要依据的一篇 note「was published way back in July of 2026」，而提问是「just at the end of last week in early October」；「In the age of AI and in investing, … three months is a massive amount of time」（口头先说 four 后改 three）
- 讲者说（视频 05:54）：同一问题接上 Vault：连接 Vault 会提示 agent 按时间段找，于是聚焦 10 月 1–8 日；找到 52 篇候选文档；最后用了 7 篇而不是一两篇，agent「more confident」能回答问题
- 讲者说（视频 06:37）：「it takes great data and the right connectors to make agents impactful」
- 我的理解：讲者没说 enterprise chat client 是哪家产品、用的是哪个模型；slide 只写「same LLM」

## 5. 搜索为两种读者设计

- slide（照片 08）「Search is hard. And harder when designing for two very different consumers.」
- slide（照片 09）「Two very different consumers」：

  | | HUMANS | AGENTS |
  | --- | --- | --- |
  | 敏感于 | Sensitive to Precision | Sensitive to Recall |
  | | High signal-to-noise ratio | Performant data access |
  | 规模 | Small scale | Massive scale |
  | 速度 | Slow to consume | Very quick to consume |
  | 阅读速度 | ~300 words per minute | 30,000+ words per minute |

- 讲者说（Linsky，视频 07:20）：人需要 precision 和高信噪比，「Even a fast reader manages about 300 words a minute, and personally, I'm much slower」；agent「far less sensitive to precision. If the data is there, they'll find it」，交给它的东西都会被 attend，读得快约一百倍，每分钟 30,000 词
- slide（照片 10）「FIVE DIMENSIONS」：Recall · Precision · Filtering · Entitlements · Scale
- 讲者说（视频 08:02）：有的对人重要，有的对 agent 更重要，很多两者都要；Linsky 展开三个：precision（for people）、scale（for agents）、filtering（for both）

## 6. Precision：先做 benchmark

- slide（照片 11）「PRECISION · FOR PEOPLE · #1 Complaint: "Vault search is bad"」；「RESPONSE · Let's benchmark it」：「Collect questions, manually find the best docs, including marking ones that were bad · About 200 in total」；右框「Search WAS bad · Enabled us to measure our improvements:」Fixed Doc ingest/parsing issues · Reciprocal Rank Fusion weights · Reranker · Query expansion · Agentic Search
- 讲者说（视频 08:21）：Vault 灌满之后（「a heroic effort by our data and legal teams」）recall 不再是紧迫问题，但头号抱怨是「Vault search is bad」；「my response was let's benchmark it」。人工收集问题、找最佳文档，包括「ones that looked helpful but weren't」，约 200 个例子，「it took us less than a week. It was not fun or very glamorous, but it was very helpful」
- 讲者说（视频 09:01）：关键指标是 precision at 10（前 10 个结果里有几个是对的文档），起点约 60%，「the people were right, it was bad」；做完到 Agentic Search 后「up in the 90-95% … recall at 10 or precision at 10」
- 出入：讲者口头在 recall@10 和 precision@10 之间改口，前文定义的指标是 precision@10；视频说明写「precision at 10 from about 60% to over 90%」。slide 列的五项改进各自贡献多少没有给数字

## 7. Filtering：pre-filtering、post-filtering 和 turbopuffer 的 native filtering

- slide（照片 12）「FILTERING · FOR BOTH · We want the most relevant documents」，示例过滤条件：「as of July 16, 2026」「U.S. Federal Reserve」「KRW」「USD」；「That requires advanced filtering」
- slide（照片 13）「Postfiltering v Prefiltering」：拍到的是 postfiltering 一侧，「Search the whole index, then filter the top n results by metadata.」「Diagram is illustrative」（prefiltering 一侧没拍到）
- 讲者说（视频 09:35）：看生产 traces，几乎总带条件：某个日期为止、某个来源、某个资产，「all of that requires advanced filtering, and that's where we ran into our first major performance issue」
- 讲者说（视频 10:02）：经典两种做法：post filtering 先用向量索引再按 metadata 过滤 top 结果，快，但「recall could be pretty low because the globally relevant results might not match your filters」；pre-filtering 先过滤再对每个匹配算距离，recall 很好但可能很慢
- 讲者说（视频 10:33）：于是试用并最终切到 turbopuffer，它的「native filtering」「looks only in the nearest clusters that contain matches and then scores just those」，结果是「fast like post filtering, but with close to the same recall as pre-filtering」
- 资料：turbopuffer 博客 [Native filtering for high-recall vector search](https://turbopuffer.com/blog/native-filtering)（Bojan Serafimov，2025-01-21）：基于聚类的向量索引（受 SPFresh 启发），attribute index 和聚类挂钩，query planner 先找含匹配文档的最近聚类，再只评估这些聚类里的候选；博客示意表：post-filter recall 0%/20ms，pre-filter 100%/10s，目标 90%/25ms。和讲者描述一致

## 8. Scale

- slide（照片 14、15）「SCALE · FOR AGENTS · Need to serve at Agent Scale」：HUMAN SCALE「100 Queries per Hour · Minutes to read Docs」；AGENT SCALE「100+ Queries per Second · Docs read in seconds」
- 讲者说（视频 10:56）：人的规模「pretty pedestrian」，每小时 100 次查询，「We have 300 users」；agent 高几个数量级，「Easily 100 plus queries a second, and agent swarms can hit thousands easily」；turbopuffer「handled that easily, too」
- slide（照片 16、17）「Vault covered all five dimensions」（五项打勾）；照片 17 加了「It unlocked new workflows. It didn't change how people work.」
- 讲者说（视频 11:28）：entitlements 没展开，「I'll be talking about that later today」
- 资料：日程里 Linsky 同日 11:55am 在 Shannon Room 有另一场 [Can Your AI Analyst Keep a Secret?](https://ai.engineer/nyc/2026/schedule?session=88f71c9f-f0a9-4f42-96c0-0e1492bad1c2)，和 turbopuffer 合讲 Vault 在检索层做行级权限
- 出入：讲者说「300 users」；官方简介说 PAT「used daily by hundreds of Bridgewater investors」，两者不矛盾

## 9. 迁移习惯：Same habits, new home

- 讲者说（视频 11:36）：Vault 解锁了新工作流，「But it didn't change how our researchers worked. They still weren't coming to Vault every morning」；第二条教训：「no matter how amazing the tools are, we can't just drop them on our investors' desks and expect them to be used. We need to migrate their old habits」
- slide（照片 18、19）「Same habits, new home → new habits」：

  | Port：IN EMAIL | → IN THE VAULT | Evolve |
  | --- | --- | --- |
  | Research received by email | Searchable in the Vault | Ask questions across all of it |
  | Subscribed lists | Saved searches | Dynamic feeds by topic |
  | Folders | Saved collections | Hand feeds and collections to agents |

- 讲者说（视频 12:02）：很多人活在邮件里：研究报告直接从 broker 来、订阅了列表、规则把东西过滤进文件夹，「It worked. It was familiar. And they knew that they weren't missing anything」；Vault 里一一对应，「It's the same habits, just a new home」；之后才能打开新习惯：跨全部数据提问、按主题的动态 feed、把集合交给 agent，「that's when we clearly had product market fit」
- slide（照片 20）「Wherever the work happens. Ramps to expand their tools, from the browser to the agents they direct.」PEOPLE：Browser · Chat · Phone · Slack · IDE；AGENTS：API and Python client · Command-line tool and skill for coding agents · MCP server
- 讲者说（视频 12:47）：放进 chat、做了手机 app、Slack、IDE，也都开放给 agent，「including a CLI because agents love CLIs」

## 10. Vault 支撑什么

- slide（照片 21）「What agent access makes possible.」：

  | 分组 | 项目 | slide 说明 |
  | --- | --- | --- |
  | HUMAN ACCELERANTS | Pocket Analyst | Our first AI analyst. Hours of research in minutes, using the Vault. |
  | | Researcher | The next generation of our Pocket Analyst capable of performing long time horizon financial research projects around our entire research circle. |
  | AGENT SANDBOXES | Custom RL models | Models we train with reinforcement learning to make better use of the Vault. |
  | SYSTEMATIC DECISION MAKING | Forecaster | AI-generated macro forecasts. |
  | | Investment signals | Systematic strategies that turn documents into structured signals, sourcing every document through the Vault. |

- 讲者说（视频 13:09）：首先帮他们「move around that research circle faster」；pocket analyst 能「do hours of research in just a few minutes for a few dollars」；researcher 是「the same idea stretched across days or weeks」；训练自定义模型更聪明地推理 Vault 数据，和其他 foundation model 一起喂给系统化决策；forecaster「produces AI-generated macro-relevant predictions」；还从 Vault 建了 investment signals，「turning unstructured data into structured, tradable strategies」
- 资料：Bridgewater 页面（LangChain Interrupt 2026 演讲的整理，2026-05-19 录制，讲者 Brendan McManus、Michael Ran、Santi Weight）称 PAT 为「AIA Pocket Analyst Tool」，「enables investors to complete hours of exploratory research in minutes」
- 出入：slide 上的「Custom RL models」明确写 reinforcement learning；讲者口头只说「train custom models」。「for a few dollars」只在口头

## 11. 从 reactive 到 proactive：Investor Workbench

- slide（照片 22）「Reactive to proactive · Building the Investor Workbench」：REACTIVE「Answers when asked」→ PROACTIVE「Steers attention」；右侧界面截图标题「What am I missing?」
- 讲者说（Green，视频 14:07）：「the real prize」是把 agent 从回答问题的被动资源，变成「proactive participants helping to steer your high-value professionals towards the most important areas for their attention」；做法是 investor workbench，「agent is always on tracking what's important to that investor」
- slide（照片 23、24）「Summary and evidence, ready for action.」界面：研究主题「AI impact on currencies」，左「Agent Summary」，右「Activity Feed」；照片 24 高亮右栏「Internal and external evidence」
- 讲者说（视频 14:45）：提出一个主题和几条信息，让投资者决定深挖还是忽略；卡片里有「an agentic summary」和 agent 依据的「exact evidence」；深挖时可直接连到 Linsky 讲的分析工具的研究产出
- slide（照片 25）「THE WORKBENCH · AGENTIC ANALYSIS · The Vault captures the human + agent collaboration」，截图「Understanding AI's Impact on FX Markets」
- 讲者说（视频 15:29）：和 agent 一起深挖时，「the results of their collaboration are automatically captured back in the vault」

## 12. 结尾：Vault 让复利发生

- slide（照片 26）「The Vault enables compounding.」Gather：「The right external and internal information, for humans and agents.」Learn：「Synthesis is contributed back to the corpus.」右图「Reasoning Engine」圆环（Perception、Hypothesis、Investigation、Synthesis、Learning，中间 Unstructured Data、Structured Data、Compound Memory 等）
- 讲者说（视频 15:44）：「The vault enables compounding because it provides the right evidence, the right … internal and external information at the fingertips of our investors and agents, and it automatically captures the synthesis that they generate and puts it back in the corpus for the next turn」
- 讲者说（视频 16:06）：「what it takes to make an agent work is the right context and the right understanding of what's going on in your world」；「does your AI know what's going on in the world? And does it remember what your firm has learned about it? Because we are teaching ours too.」

## Q&A 精确版

没有 Q&A。视频在 16:27「Thank you」和掌声处结束；直播在 23:44 掌声后放音乐，下一段直接是 Two Sigma 那场的开场介绍（23:57）。

## 勘误表（YouTube 字幕 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| Erin Lensky（视频） | Aaron Linsky | slide、日程 |
| Bridgewwater（直播） | Bridgewater | slide |
| AiA / ya（直播） | AIA (artificial investment associate) | slide「AIA's reasoning engine」、横幅「AIA Labs」 |
| Turbo Puffer / turbuffer | turbopuffer | 官方写法 |
| four three months | three months（讲者口误后改口） | 上下文 |
| extra internal and external（视频）/ ex internal（直播） | external and internal（口误） | 照片 26 |

## 出入

1. **指标名**：定义的是 precision@10（60% → 90–95%），讲者结尾说「recall at 10 or precision at 10」；视频说明写「over 90%」
2. **agent 敏感于什么**：slide 写 agents「Sensitive to Recall」；讲者说 agent「far less sensitive to precision」，两种说法方向一致
3. **买还是建**：官方简介说最初想买，台上没提
4. **Custom RL models**：slide 写明强化学习，口头只说训练自定义模型
5. **标题**：日程、直播横幅、视频标题三个都不同

## 【待确认】清单

1. 开放网络例子用的是哪个 enterprise chat client 和模型（讲者没说）
2. 照片 13 里 prefiltering 一侧的内容（没拍到），以及没拍到的几页
3. 五项搜索改进各自对 precision@10 的贡献
4. 「135 external sources」「more than 100 million documents」只有讲者口述和视频说明，没有独立来源
