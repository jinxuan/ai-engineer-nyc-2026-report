# Proving Alpha：精确版

> Precise notes for this session, compiled from the official video captions where available, the author's room recording, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材整理完。这场**没有 Q&A**（讲者在视频 18:13 说「if anyone has any questions after the talk, would love to connect」）。**整场没有给出任何量化结果**（讲者在视频 02:12 说「intentionally trying to keep this a little bit vague」）。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| 视频 | 有。[ZMZlAn6Pyfg](https://www.youtube.com/watch?v=ZMZlAn6Pyfg)，18:24，2026-10-24 上传，标题「Proving Alpha: How a Hedge Fund Measures AI's ROI - Hari Kumar, Contour Asset Management」。自动字幕是正文的主要文本 |
| slide | 8 张（IMG_0434–0441），**全部是本场**；IMG_0438 和 IMG_0437 是同一张（Subscriptions）。大约是 deck 的 01–07 页加结尾页 |
| 其他资料 | 没有讲者文章或 repo。ai.engineer/talks/ZMZlAn6Pyfg 返回 404 |

## 基本信息

- 时间地点：2026-10-13 1:40–1:58pm，Turing Room (Riverside Ballroom)，Breakout
- 讲者：Hari Kumar，Contour Asset Management 的 Head of AI and Data Science（大会讲者页）。讲者页简介：之前在 Meta 数据科学团队（推荐算法、增长预测），再之前在 BCG；UPenn 的 CS 硕士（M.S.E）和金融学士
- slide 封面：「CONTOUR / AI & INVESTING」，标题「**Measuring the impact of AI in fundamental investing**」，副标题「What we are trying to capture, and where are are heading.」（slide 原文如此），「OCTOBER 2026」，「HARI KUMAR, CONTOUR ASSET MANAGEMENT」
- 题目：大会日程「Proving Alpha: Showcasing AI's Value in Fundamental Investing」；slide 封面和视频标题都和日程不同
- 讲者说（视频 00:12–01:05）：「I am the head of AI at Contour Asset Management, which is a hedge fund based in the city. It's been around for a little over a decade now」，他自己来了「the past year-ish」；「we're a fundamental long short shop, around like 15 20 people, **6 billion AUM**. And mostly invest in the TMT space」
- 视频简介：「a $6B fundamental long/short fund focused on tech, media and telecom」
- 资料（公开信息有限）：Contour 是纽约的 TMT long/short 基金；能找到的公开 AUM 是 2015–2016 年前后的约 14–15 亿美元（[Business Insider 转载，联合创始人 Seth Wunder 离开时](https://de.finance.yahoo.com/nachrichten/cofounder-1-4-billion-tech-201248913.html)）。**60 亿 AUM 和 15–20 人只有讲者口述和视频简介，没有找到一手来源，【待确认】**

## 1. 问题：AI 花了钱，到底换来什么

- 讲者说（视频 01:06–02:02）：他的 mandate 是「how can I show impact across all areas of the firm, largely starting with this research process」；难点是说清楚「we're spending this amount of money and time on AI spend … What are we actually gaining from it? Both from a P&L standpoint, which is to come, but also just from a more operational and are we getting good ideas perspective」
- 讲者说（视频 02:10）：「intentionally trying to keep this a little bit vague and more in terms of the operational and process parts of it」，技术细节留到会后

## 2. 研究流程的 live A/B test

**slide「THE FUNDAMENTAL QUESTION — What would have happened without AI? Actually mapping out A/B Tests」**：

| | 维度 | 问题 |
| --- | --- | --- |
| 01 | INVESTMENT DECISIONS | Would we have found the same idea or reached the same conclusion? |
| 02 | RESEARCH ACCESS | How long would sourcing the information have taken manually? |
| 03 | OUTCOMES | Is the benefit better judgment, faster execution, or both? |

- slide 页脚：「No clean control group. No easy counterfactual.」
- 讲者说（视频 02:26–02:58）：约 10 个 analyst，各管一块 coverage；小型基金里「one person that covers 30 to 40 names」
- 讲者说（视频 03:01–03:34）：「in a AI and more agentic world … would you have come to the same type of conclusion with the AI tools or without the AI tools」；过去几个月的做法是「creating live AB tests by recording and documenting their entire process that they would have done in a pre-AI world and how they're doing it as we roll out more workflows and tools」
- 讲者说（视频 03:35–04:36）：做法举例：**针对某一个 name，一个三个月周期（「from one earnings report to the following one」）**，先记录不用任何 AI 工具（「if you're not using any LLMs」）时的流程、找到的数据源和结论；再引入工具后，和 PM 一起跑同样的流程。工具例子：帮做 expert call interview 的工具，以及「field a consumer survey that's done by an agent instead of a human」
- 讲者说（视频 04:38–05:09）：expert call 的对比：agent 跑 expert call、你自己综合笔记，对比 agent 替你综合笔记、你最后下结论；本质是「how long would it have taken to source some of this information manually versus … in a more agentic manner」
- 讲者没有给出这个 A/B 测试的任何结果数字（时间、结论差异、P&L）

## 3. 衡量过程，而不只是 P&L

- 讲者说（视频 05:11–05:40）：很多工作是 idea generation，想看新的 thesis「can they be proven out in a short-term scale, which is a little less of what we do, but at least in like the 3-month window, or are they things that take 6 months or a year to manifest」
- 讲者说（视频 05:43–06:14）：「the challenge with something like this is you can't show ROI immediately. It's more so from a process-based manner」：是否改进了 consumer survey 的流程、更新模型的流程；问的是「are we actually changing the way someone's working or we just adding more thrust to their process」（字幕 "thrust"，录音转写 "thrash"，原词【待确认】）

## 4. Idea 生成、挑战 thesis、找回被遗忘的 idea

**slide「MEASURE THE INCREMENTAL INSIGHT — Better thinking, not just faster work.」**（页眉「01 / RESEARCH WORKFLOWS」）：

- How to Better Challenge a Thesis：「Goal here is to surface sources an analyst would not otherwise use.」
- Use Cases：Expert Calls and Consumer Survey Notes；Idea Generation；Alternative Data → Second/Third-Order Connections
- 右栏 THE QUESTION TO ASK：「**"What did you consider that you wouldn't have before?"**」；「Track novel ideas, differentiated sources, and ideas acted upon.」
- 页脚：「The value is additional signal: not simply …」（被前排观众挡住，后半句【待确认】）
- 讲者说（视频 06:24–06:44）：fundamental investing 的要点是想出 thesis，但「not be so anchored to a thesis that you come up with initially」，要相信数据、专家和其他 insight 来去掉这些 anchor，「this is where I think that there's a lot of impact measurement when it comes to AI tooling」
- 讲者说（视频 06:52–07:33）：「idea generation agents」按 name、按 analyst 把已有的 idea 加几层：你从 alternative data 或 expert 得到了几个数据点，agent 再补充结论，以及这个 idea 会在几个月内兑现还是需要更久（字幕原文是「manual idea generation agents」，"manual" 是否误识别【待确认】）
- 讲者说（视频 07:37–08:54）：一个大问题是：idea 做了几天，PM 不感兴趣或被别的事挤掉，就丢了，几个月后才回来，「maybe you miss like a key price window」。所以有 agent「tracking through all of the notes that you've been taking, even if you only worked on an idea for a day, tracking it against the market」，判断现在是否值得「flash … to not just you, but the PM and the trader」；「the timing is actually the most important thing」，agent 也要考虑「when you should enter exit a position」

## 5. 把 alternative data 放到一起

- 讲者说（视频 08:58–09:59）：基金在新数据集上花很多钱，比如「satellite mapping or really granular credit card data」，「they're only helpful when they're all brought together」。例子：「something like a Yipit, which provides credit card panel data, that lives in isolation, but now with MCP and API access for things like Yipit, you can feed it into our agents, which is kind of what we've done」；agent 同时读 expert call 和笔记 transcript，再叠加 credit card panel 或 spend data，看两者如何相互印证
- 我的理解：「Yipit」指 YipitData（另类数据供应商）。字幕写成 "Yipit" / "Gipit"，录音转写为 "GIFIT"

## 6. 时间维度和「cost per useful insight」

- 讲者说（视频 10:05–10:52）：衡量 ROI 的另一个难点是时间：钱花在工具、模型更新、AI 上，但更重要的是「what is an analyst spending their day on」。他们做了指标：「these are the insights that have been generated by an analyst per day. What was the time spent on that? So, tracking time spent on each name. And what were the eventual P&L outcomes」
- 讲者说（视频 10:52–11:16）：投资的好处是有「a very very obvious success metric when it comes to P&L」，但兑现慢，「especially from a more fundamental shop, where you're not trading by the day … expected to materialize over months-long horizons」
- 讲者说（视频 11:16–11:40）：「tracking things like a **cost per useful insight, which is the metric that we've come up with**」：每天产生的 idea 都被记录，追踪「Where do those ideas eventually land? What were the interesting ones? And what were the tools that were taken to come up with an idea like that?」
- 讲者说（视频 11:44）：这部分衡量的是「not just AI spend, but general tech investment」
- 讲者**没有给出这个指标的定义公式**（分子分母、「useful」怎么判定），也没给数值。【待确认】

## 7. 推到研究以外：交易、运营、财务

**slide「BEYOND THE RESEARCH DESK — Bring the same discipline to the rest of the firm.」**（页眉「03 / FIRMWIDE IMPACT」）：

| | 部门 | slide 说明 |
| --- | --- | --- |
| 01 | Trading | Order history, execution timing, and historical lookbacks |
| 02 | Operations | Repetitive work, exceptions, and process reliability |
| 03 | Finance / Analytics | Query, reconcile, and understand internal history |

- 页脚：「As workflows become repeatable, more ou…」（被挡住，后半句【待确认】）
- 讲者说（视频 12:15–12:50）：trading 方面在衡量 entry 和 exit 时间、仓位在不同行业和同一行业多个 name 间的分布，让 AI agent 跟踪这些 name；trader 的时间「should not just be spent on executing orders. It should also be spent on bringing idea generation, feeding ideas to our analysts and associates」
- 讲者说（视频 12:51–14:10）：ops 方面，很多 workflow「set within Bloomberg or set within your … order keeping agent」，各家都有 AI 工具但很难整合；他们在把 ops 和 compliance 的流程合到「one specific tool」，不再依赖外部供应商的「piecemeal solutions」（例：order reconciliation、track vendor info）；「bring those all into one is … a pretty big priority for us」
- 讲者说（视频 14:12–14:32）：这一块「not necessarily to the point of this talk, a way to really capture ROI」，而是让 trading 和 ops 的人把技能用到基金的主目标上：「create new ideas, understand the market, and focus on timing」

## 8. 同样的数据订阅，不同的用法

**slide「THE ECONOMICS OF SUBSCRIPTIONS — Same data spend. Different utility.」**（页眉「02 / DATA ACCESS」，页码 04）：

- BEFORE：Subscription → manual search → extraction → research
- WITH MCP / APIS：Subscription → agent calls → reusable research context
- THE NEW VENDOR MATH：**Utilization / calls / citations / adoption / cost per useful insight**
- 页脚：「Programmable access becomes a competitive requirement.」
- 讲者说（视频 14:35–15:32）：基金的数据支出要么是分散的工具（「an AlphaSense for … data, a Yipit for alt data」），要么是整体方案（「buying a Rogo membership, or investing in like Kensho」）；他们在内部做的是把 analyst 用的工具合到一个平台，并且「up level the spend that we have right now」
- 讲者说（视频 15:34–16:20）：例子：AlphaSense（「a big … call plus data provider」），以前 analyst 在 AlphaSense 网页里查，再搬进自己的模型；「Things like Rogo will kind of take that one step further by allowing you to have all that data within your model itself, and you can have a platform that you chat with Rogo agents for」；他们的做法是继续用 AlphaSense 或 Yipit，但接进自己的内部平台，平台「learns from our analysts habits and is programmed specifically for each individual analyst and their model pattern」；「how can you increase the value of your subscription?」供应商都在出 MCP 和 API，「the goal should be to learn continuously」
- 资料：Rogo 是面向投行、PE、资管的金融 AI 平台（[Kleiner Perkins 介绍](https://kleinerperkins.com/perspectives/rogo-the-ai-platform-for-global-finance)；[OpenAI 客户案例](https://openai.com/index/rogo)）。AlphaSense、Kensho（S&P Global 旗下）、YipitData 没有单独核对
- 我的理解：这张 slide 的拍摄时间（录音约 10:49）落在讲者讲 cost per useful insight 的时候，而他口头讲 subscriptions 是在视频 14:35 之后，翻页和讲述的顺序不完全同步

## 9. 方向：从聊天工具到 operating layer

**slide「THE DIRECTION OF TRAVEL — From chat tool to operating layer.」**（页眉「WHERE THIS IS GOING」）：

| 从 | 到 |
| --- | --- |
| Standalone chat | Agents embedded in workflows |
| Manual retrieval | Programmable data access |
| One-off answers | Repeatable, auditable processes |

- 页脚：「The differentiator: data access, workflow design, and human judgment.」
- 讲者没有逐条讲这张 slide。对应的口头结论在视频 17:47–18:09：「as AI adoption increases, especially at a lot of these smaller fundamental shops, having **one unified layer that's doing a lot of different tasks across analysts** is going to be really, really valuable」

## 10. ROI 里的隐性成本：修工具的时间

**slide「[A]N HONEST ACCOUNTING — Measuring ROI based on a variety of factors」**（右上角「A WORK IN PROG[RESS]」）：

- THE UPSIDE：New signal. Better decisions. Expanded research capacity.
- THE REAL COSTS：**Model errors. Broken connectors. Verification. Rework.**
- 底部：「Measure usage alongside quality, outcomes, and failure rates. Additionally explore longer-term tracking of investment ideas.」
- 讲者说（视频 16:40–17:45）：给很 tech-forward 的投资人引入新工具，总有坏的时候，「where model calls break or the connector breaks down every couple days, and then it's your job to fix it」；ROI 里要算上「the time that [is] being taken away trying to fix bugs, that someone from the tech side cannot fix itself」；「when it comes to time spent for, let's say, valuable insight, what is the time detracted to understand how these tools work as well」；这部分时间在下降，「but it's a metric that we're still considering」

## Q&A

无。视频和录音都在讲者致谢后结束（视频 18:15，录音约 18:03）。

## 勘误表（转写 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| Harry（视频字幕） | Hari | 大会讲者页 |
| TNT space（录音） | TMT space | 视频字幕、视频简介 |
| 30 to 40 meetings（录音） | 30 to 40 names | 视频字幕 |
| AI more authentic world（录音） | AI and more agentic world | 视频字幕 |
| live ae tests（录音） | live A/B tests | slide |
| earning support（录音） | earnings report | 视频字幕 |
| LMS（录音） | LLMs | 视频字幕 |
| Heist Fund（录音） | hedge fund | 视频字幕 |
| involving rpm（录音） | involving our PM | 视频字幕 |
| new pcc's（录音） | new theses | 视频字幕 |
| GIFIT / Gipit / Yipit | Yipit（YipitData） | 上下文 |
| NCP / MTP | MCP | slide（WITH MCP / APIS） |
| training（录音，"bring that into training"） | trading | 视频字幕、slide |
| Cooper（录音） | Bloomberg | 视频字幕 |
| YPX（录音） | Yipit | 视频字幕 |
| robo-member share / heavy net（录音）；Roqoo membership（字幕） | Rogo membership / Kensho | 视频字幕、上下文 |
| Rola agents（录音） | Rogo agents | 视频字幕 |
| alpha-sensory event tool（录音） | AlphaSense or Yipit tool | 视频字幕 |
| off-waddle（录音） | up-level | 视频字幕 |
| a picture breaks down … a few documents（录音） | the connector breaks down every couple days, and then it's your job to fix it | 视频字幕、slide（Broken connectors） |
| so consuming（录音） | still considering | 视频字幕 |

## 出入（来源之间不一致的地方）

1. **大会简介承诺的内容，演讲没有讲**：简介说「every idea that a workflow produces is logged at the moment of generation, along with its source and reasoning」，并追踪「how many ideas reach our PM to become positions, and how those positions perform against given benchmarks」，以及对「softer AI-native signals」和传统方法对比「time & signal improvements」，还说会「trace their impact back to the book」。演讲里只讲了 idea 和时间的追踪思路，**没有讲 idea → position → 相对 benchmark 表现的归因，也没有任何数字**。讲者开场就说要刻意讲得模糊
2. **简介点名的三个工具没有出现**：简介提到「new idea generation, overnight news synthesis, and earnings preview」三个每天在用的产品，演讲里只讲到 idea generation；overnight news synthesis 和 earnings preview 没提
3. **题目**：日程「Proving Alpha: Showcasing AI's Value in Fundamental Investing」，slide 封面「Measuring the impact of AI in fundamental investing」，视频「Proving Alpha: How a Hedge Fund Measures AI's ROI」
4. **AUM**：讲者口述和视频简介是 60 亿美元；能找到的公开数字是 2015–2016 年前后约 14–15 亿美元，没有找到当前的一手数字
5. **slide 顺序和讲述顺序**：slide 页码是 01 Research workflows → 02 Data access（Subscriptions）→ 03 Firmwide；口头顺序是研究 → cost per useful insight → 全公司 → 订阅 → 隐性成本。拍照时间显示 Subscriptions 那张在讲 cost per useful insight 时就已经出现（我的理解）

## 【待确认】汇总

1. 60 亿 AUM、15–20 人的一手来源
2. 「cost per useful insight」的定义（分子分母、什么算 useful）
3. slide「MEASURE THE INCREMENTAL INSIGHT」页脚「The value is additional signal: not simply …」的后半句
4. slide「BEYOND THE RESEARCH DESK」页脚「As workflows become repeatable, more ou…」的后半句
5. 视频 06:14「adding more thrust」还是「thrash」；视频 06:52「manual idea generation agents」里的 "manual"
