[English](../10-13-1340-proving-alpha-showcasing-ais-value-in-fu.md) | **中文**

# 一家对冲基金想在 P&L 显现之前，先量出 AI 改变了什么

*在 AI Engineer NYC 2026 上，Contour Asset Management 的 Hari Kumar 讲了一家小型基本面基金怎样给 AI 的价值算一个数：对分析师的研究做实时 A/B 测试，一个「cost per useful insight」指标，以及老老实实地统计修工具花掉的时间。Kumar 有意没有给出任何结果。*

**Breakout · 2026 年 10 月 13 日（周二）· 1:40–1:58pm · Turing Room (Riverside Ballroom)**
**讲者：** Hari Kumar，Contour Asset Management AI 与数据科学负责人（此前在 Meta 和 BCG 做数据科学）

---

![Measuring the impact of AI in fundamental investing](../../slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/01.jpg)
*标题页：「Measuring the impact of AI in fundamental investing.」*

Hari Kumar 幻灯片上的标题和日程上的不一样：「Measuring the impact of AI in fundamental investing」，副标题是「What we are trying to capture, and where are are heading」（原文如此）。Kumar 介绍 Contour Asset Management 是「一家在纽约的对冲基金」，「成立了十年多一点」，Kumar 自己是「大概过去一年」加入的。按 Kumar 的描述，「我们是一家 fundamental long short 机构，大概 15、20 个人，60 亿 AUM，主要投 TMT」，也就是科技、媒体和电信。（人数和资产规模来自演讲和视频简介，没有找到独立来源。）

Kumar 的任务是展示「公司各个环节的影响，主要从研究流程开始」。难的是解释清楚：「我们在 AI 上花了这么多钱和时间……到底得到了什么？既有 P&L 角度的，那个还要等，也有更偏运营、偏『我们是不是得到了好点子』的角度。」

Kumar 一开始就打了预防针：「有意讲得模糊一点，更多讲运营和流程层面」，技术细节留到会后再聊。整场演讲没有任何量化结果。

## 1. 如果没有 AI，会怎样？

![What would have happened without AI?](../../slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/02.jpg)
*基金 A/B 测试背后的三个问题，脚注是「No clean control group. No easy counterfactual.」*

那页「根本问题」把反事实分成三部分：

| | 领域 | 问题 |
| --- | --- | --- |
| 01 | 投资决策 | 我们会不会发现同样的点子，或得出同样的结论？ |
| 02 | 研究获取 | 手工找这些信息要花多久？ |
| 03 | 结果 | 好处是判断更好、执行更快，还是两者都有？ |

脚注承认了难点：「No clean control group. No easy counterfactual.」（没有干净的对照组，也没有简单的反事实。）

基金大约有十个分析师，各自负责自己的覆盖范围；Kumar 说，在小基金里是「一个人覆盖 30 到 40 只股票」。问题在于，「在 AI、更 agentic 的世界里」，分析师「用 AI 工具和不用 AI 工具，会不会得出同类结论」。过去几个月，基金一直在「做实时 A/B 测试：记录下分析师在 pre-AI 世界里会怎么做整套流程，以及随着我们推出更多 workflow 和工具，他们现在怎么做」。

Kumar 讲了测试的一种形式。针对单只股票，在一个三个月的周期里，「从一次财报到下一次财报」，分析师先在不用任何 LLM 的情况下记录流程、数据来源和结论。然后随着工具到位，和 portfolio manager 一起把同样的流程再跑一遍。工具里有一个辅助专家访谈，还有一个 agent 能「发放一份由 agent 而不是人来做的消费者调查」。专家访谈的对比是：一边是 agent 主持访谈、分析师整理笔记，另一边是 agent 整理笔记、分析师只下最终结论。最后要问的是「手工找这些信息要多久，和……用更 agentic 的方式相比」。Kumar 没有给出这些测试的任何数字。

## 2. 衡量流程，而不只是 P&L

很多工作是产生点子，而投资论点成熟的速度不一样：有些「能在短期内被验证……至少在 3 个月窗口内」，另一些「要 6 个月或一年才会体现出来」。Kumar 说，这就是为什么「这类事情的难点在于你没法马上展示 ROI，更多是从流程角度来看」：工具有没有改进消费者调查的流程、更新模型的流程，以及「我们是真的改变了一个人的工作方式，还是只是给他的流程加了更多 thrust」。（字幕写的是「thrust」；现场录音听到的是「thrash」。）

## 3. 思考得更好，而不只是干得更快

![Better thinking, not just faster work](../../slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/03.jpg)
*研究 workflow 那页，以及要问分析师的问题。*

整套幻灯片的第一个编号章节是研究 workflow，标题是「Measure the incremental insight: better thinking, not just faster work」（衡量增量洞见：思考得更好，而不只是干得更快）。在「How to Better Challenge a Thesis」下面，目标是「找出分析师原本不会用到的来源」，use case 包括专家访谈和消费者调查的笔记、产生点子，以及用另类数据挖「second/third-order connections」（二阶、三阶关联）。侧边栏给出要问分析师的问题：「What did you consider that you wouldn't have before?」（有什么是你以前不会考虑、现在考虑了的？）以及要求「追踪新颖的点子、差异化的来源，以及被付诸行动的点子」。页脚被一位观众的头挡住了一部分，开头是「The value is additional signal: not simply…」

Kumar 说，基本面投资是要提出一个论点，但又不能「被一开始提出的论点锚定得太死」；数据、专家和其他洞见应该解除这些锚定，「我认为在 AI 工具的影响衡量上，这里大有可为」。基金的点子生成 agent 会按股票、按分析师给已有的点子加料：拿到来自另类数据或专家的几个数据点后，agent 补充结论，并估计这个点子会在几个月内见效还是要更久。

Kumar 讲了这些 agent 要解决的一个具体问题。分析师在一个点子上花了几天，PM 不感兴趣，或者被别的工作挤掉了，于是搁置，直到几个月以后，那时「也许你已经错过了关键的价格窗口」。所以有个 agent 在「追踪你记过的所有笔记，哪怕你只在一个点子上花了一天，把它和市场对照」，再判断值不值得重新提醒，「不只是提醒你，还有 PM 和交易员」。「时机其实是最重要的，」Kumar 说，所以 agent 还会考虑「什么时候该建仓、平仓」。

## 4. 把另类数据整合起来

Kumar 说，基金在新数据集上花了很多钱，比如「卫星测绘或者非常细的信用卡数据」，但「只有把它们整合到一起才有用」。例子是 Yipit（YipitData），它的信用卡 panel 数据「原本是孤立的，但现在有了 Yipit 这类数据的 MCP 和 API 访问，你可以把它喂给我们的 agent，这基本就是我们做的事」。agent 读专家访谈的记录和分析师的笔记，叠加信用卡 panel 或消费数据，看两者能不能互相印证。

## 5. 花掉的时间，以及「cost per useful insight」

时间是 ROI 的另一个难点。钱花在工具、模型更新和 AI 上，但更重要的是「分析师一天的时间花在了什么上」。基金为此建了指标：「分析师每天产生了哪些洞见，在上面花了多少时间，也就是追踪每只股票上花的时间，以及最终的 P&L 结果是什么。」

Kumar 说，投资「在 P&L 上有一个非常非常明显的成功指标」，但它来得慢，「尤其是更偏基本面的机构，你不是按天交易」，回报「预期要在几个月的时间跨度上体现」。所以基金追踪「像 cost per useful insight 这样的东西，这是我们自己想出来的指标」。一天里产生的每个点子都记下来，基金会跟踪「这些点子最后落到了哪里？哪些是有意思的？想出这样一个点子用了哪些工具？」Kumar 指出这衡量的「不只是 AI 支出，还有整体的技术投入」。Kumar 没有定义这个指标的分子和分母，没说什么算「useful」，也没给出任何数值。

## 6. 研究台之外

![Bring the same discipline to the rest of the firm](../../slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/06.jpg)
*全公司那一节：交易、运营、财务与分析。*

幻灯片的第三节「Beyond the research desk」（研究台之外）把同样的做法用到公司其他部门：

| | 职能 | 幻灯片上列的内容 |
| --- | --- | --- |
| 01 | Trading | 订单历史、执行时机和历史回看 |
| 02 | Operations | 重复性工作、异常情况和流程可靠性 |
| 03 | Finance / Analytics | 查询、对账和理解内部历史 |

页脚被挡住一部分，开头是「As workflows become repeatable, more ou…」

在交易上，基金衡量建仓和平仓的时机，以及仓位在各行业之间、同一行业几只股票之间怎么分布，由 AI agent 追踪这些股票。交易员的时间「不应该只花在执行订单上，也应该用来产生点子，把点子喂给我们的分析师和助理」。在运营上，很多 workflow 在 Bloomberg 或订单管理系统里，每家供应商都有自己的 AI 工具，很难串起来。基金正在把运营和合规流程，比如订单对账、追踪供应商信息，整合进「一个专门的工具」，取代外部供应商「零敲碎打的方案」，Kumar 说这是「我们一个相当大的优先事项」。Kumar 承认这部分「不一定切中这次演讲的主题，即真正衡量 ROI 的方法」；目的是把交易和运营人员解放出来，去做基金的主要目标：「产生新点子、理解市场、关注时机」。

## 7. 同样的数据支出，不同的用处

![Same data spend. Different utility.](../../slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/05.jpg)
*数据获取那页：手工使用订阅和通过 agent 使用订阅的对比，以及「new vendor math」。*

数据获取那页（幻灯片里是第 02 节，但 Kumar 快结束时才讲）对比了两种使用订阅的方式：

| | 流程 |
| --- | --- |
| Before | 订阅 → 手工搜索 → 提取 → 研究 |
| With MCP / APIs | 订阅 → agent 调用 → 可复用的研究上下文 |

在「The new vendor math」（新的供应商算账方式）下面，列出了应该改为衡量的东西：utilization、calls、citations、adoption 和 cost per useful insight。页脚是：「Programmable access becomes a competitive requirement.」（可编程访问成为竞争的必要条件。）

Kumar 说，基金的数据支出要么是一组分开的工具（「数据用 AlphaSense……另类数据用 Yipit」），要么是一个平台（「买 Rogo 会员，或者投 Kensho 这样的」）。Contour 在内部自建：把分析师的工具合进一个平台，试着「把现有的支出用得更上一层」。以前分析师在 AlphaSense 的网页界面里搜，再把结果手工搬进模型。「Rogo 这类工具会再往前走一步，让所有数据直接进到你的模型里，还有一个平台让你和 Rogo 的 agent 聊天。」Contour 保留了 AlphaSense 和 Yipit，但把它们接到自己的平台上，这个平台「从我们分析师的习惯里学习，并且针对每个分析师和他们的模型模式专门编程」。问题是「怎样提高你的订阅的价值？」随着供应商推出 MCP server 和 API，「目标应该是持续学习」。

## 8. 从聊天工具到运营层

![From chat tool to operating layer](../../slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/07.jpg)
*基金眼中 AI 的走向。*

「direction of travel」（走向）那页列了三个转变：

| 从 | 到 |
| --- | --- |
| 独立的聊天 | 嵌入 workflow 的 agent |
| 手工检索 | 可编程的数据访问 |
| 一次性的回答 | 可重复、可审计的流程 |

页脚写明差异化来自「数据获取、workflow 设计和人的判断」。Kumar 没有逐行讲；口头版本出现在结尾：「随着 AI 普及，尤其是在很多这种小型基本面机构里，有一个统一的层、替各个分析师做很多不同的任务，会非常非常有价值。」

## 9. 一本老实账

![Measuring ROI based on a variety of factors](../../slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/08.jpg)
*收益和真实成本，标注着「a work in progress」。*

最后一页内容页「An honest accounting」（一本老实账）标着还在进行中，把收益（「New signal. Better decisions. Expanded research capacity.」，即新信号、更好的决策、更大的研究产能）和真实成本（「Model errors. Broken connectors. Verification. Rework.」，即模型错误、坏掉的 connector、核验、返工）摆在一起。结尾写着：「Measure usage alongside quality, outcomes, and failure rates. Additionally explore longer-term tracking of investment ideas.」（衡量使用量的同时也衡量质量、结果和失败率；另外探索对投资点子的长期追踪。）

Kumar 说，给非常懂技术的投资人引入新工具，意味着总有东西在坏，会出现「模型调用出错，或者 connector 每隔几天就断一次，然后修它就是你的活」。ROI 必须算上「花在修 bug 上的时间，那些技术那边自己修不了的 bug」，以及「说到花在，比如说，有价值的洞见上的时间，还要算上为了搞懂这些工具怎么用而被占掉的时间」。Kumar 说这部分时间在下降，「但这是我们还在考虑的一个指标」。

Kumar 最后请有问题的人会后联系；这场没有 Q&A。

---

**链接：** [日程](https://ai.engineer/nyc/2026/schedule?session=ae273f0b-db0f-4638-8ae3-05874bebf9ca) · [视频](https://www.youtube.com/watch?v=ZMZlAn6Pyfg) · [Rogo (Kleiner Perkins)](https://kleinerperkins.com/perspectives/rogo-the-ai-platform-for-global-finance)

本文依据官方视频字幕、作者的现场录音和幻灯片照片写成。session 简介承诺的内容比演讲讲到的多：简介提到要把记录下的点子一路追踪到仓位及其相对基准的表现，还提到隔夜新闻汇总和财报预览工具，这些台上都没讲。录音结束时没有 Q&A。[精确笔记](../precise/10-13-1340-proving-alpha-showcasing-ais-value-in-fu.md)（中文）保留了时间戳，并完整记录了演讲和幻灯片之间的出入。
