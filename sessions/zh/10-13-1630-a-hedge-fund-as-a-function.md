[English](../10-13-1630-a-hedge-fund-as-a-function.md) | **中文**

# Coatue 的 Frank Long：把基金写成一个函数，再给每一行挑一个 runner

*在 AI Engineer NYC 2026 上，Coatue 的 AI 负责人 Frank Long 把一家投资机构描述成一个函数：输入外部和内部的 context，输出买什么、买多少、什么时候买。Long 认为，真正的工作是把它拆成可以检查的小块，再决定每一块由代码、统计模型、LLM 还是人来跑。*

**Featured Keynote · 2026 年 10 月 13 日（周二）· 4:30–4:48pm · Simons Room (Metropolitan East)，Main Stage**
**讲者：** Frank Long，Head of AI，Coatue

![Frank Long 在 Main Stage 上](../../slides/10-13-1630-a-hedge-fund-as-a-function/20.jpg)

---

Frank Long 介绍 Coatue 是「一家横跨一级和二级市场的科技投资公司」，在这个行业里，「你真正需要的是……能对大主题下高信念的注，然后看它们兑现」。Long 说职业生涯也是一样，而 Long 加入这家公司时押的注是：「agent 对投资的影响会比对 coding 还大。我知道这话说得很大。」这场演讲讲的是 Coatue 为此用的 framework。

## 1. 基金就是一个函数

![def fund(c_external, c_internal)](../../slides/10-13-1630-a-hedge-fund-as-a-function/03.jpg)
*两行代码写成的基金。封面标题是「An Investment Fund as a Function」。*

「从高处看，你可以把一家对冲基金想成一个函数：输入两个参数，外部 context 和内部 context，吐出一个 tuple：selection、size、timing。买哪只股票，买多少，什么时候。」slide 把它写成代码 `def fund(c_external, c_internal): return selection, size, timing`，页边注着「Takes external and internal context」（输入外部和内部 context），以及一行红字「People own the investment judgment」（投资判断归人）。

Long 称之为一个压缩问题：怎样把世界上所有的信息变成一个简单的 tuple。「我们提供的产品，本质上就是这个 tuple 的一条时间序列，由它构成一个投资组合。」

## 2. 人在屏幕上看到的，模型都该能通过 API 看到

![Everything a human can see in pixels, a model can see via API](../../slides/10-13-1630-a-hedge-fund-as-a-function/06.jpg)
*分析师在屏幕上看的东西，对应到外部和内部 context 的 API 调用。*

地基是一条简单的原则：「一个人在屏幕上能以像素看到的每一样东西，你公司里的 agent 都应该能通过 API 看到。」Long 打了个比方：「想象你请来了史上最聪明的人、在世最伟大的投资者，把他们请进公司，却给了他们一台登不进 75% 服务的电脑。系统再聪明，如果没有工具去真正采取行动，也几乎无关紧要。」

slide 一边是显示着图表和表格的显示器，一边是 agent 取而代之会发的调用，例子用的是三星电子（韩国代码 005930.KS）：

| Context | 调用 | 返回 |
| --- | --- | --- |
| External | `news.search("Samsung", lang="ko")` | title, source, time |
| External | `prices.history("005930.KS")` | date, close |
| External | `filings.get("005930.KS", "Q3")` | revenue, operating margin, … |
| Internal | `positions.current()` | ticker, weight, cost |
| Internal | `liquidity.requirements()` | cash floor, redemptions |
| Internal | `case_studies.search("Samsung")` | memo, author, date |

外部 context（新闻、价格、filings）应该「不是靠拖拽、复制粘贴，而是直接通过 API」拿到。内部 context「同样重要」：公司自己的处境，比如当前持仓、对某些股票的流动性和风险要求，以及过去的 case study，或者和某个行业、某只股票、某个管理层打交道的历史。

Long 把这块地基归功于公司：「这不是我的功劳，Coatue 在这方面领先了十多年。它既是技术上的，也是文化上的。」除了数据科学团队和系统，文化上也「非常非常严谨」：「几十年来，我们把每一笔交易、每一个做了或没做的决定、哪些顺利哪些不顺，都记成了笔记。」

（直播字幕在这里以及之后的四处各有约十秒的空白，所以记录里少了几句。）字幕恢复时，Long 正在否定这样一种想法：把所有东西塞进「一个 context window，拿到我的神奇数字就下班……不可能这么简单」。

## 3. 拆到每一块都能检查

![Break it down until each piece can be checked](../../slides/10-13-1630-a-hedge-fund-as-a-function/09.jpg)
*fund 函数拆成 gather、analyze 和 decide。*

下一页把函数一行行加出来：

| 代码行 | slide 上的注 |
| --- | --- |
| `def fund(c_external, c_internal):` | Takes external and internal context |
| `info = gather(c_external, c_internal)` | Gather information |
| `view = analyze(info)` | Analyze it |
| `selection, size, timing = decide(view)` | Make the decision: a person（做决定的是人） |
| `return selection, size, timing` | People own the investment judgment |

gather 意味着过滤出相关的东西。「我拉到几篇讲 NBA 的新闻，大概和股票无关；可是等等，这篇其实涉及某个地方的审批许可，可能会影响数据中心。gather 这一步里有很多微妙之处。」analyze 形成观点，判断「这对一级和二级市场意味着什么」，decide 把观点变成那个三元 tuple。每一步的输出就是下一步的输入，Long 说对工程师来说这是很自然的思考方式。「但就算这样，也不可能只是写三个函数那么简单。」

## 4. 案例：韩国新闻

为了展示全部复杂度，Long 放大到「仅仅是收集信息这一步里的一个小环节的一个小环节」：解读新闻。「今年来自亚洲和韩国媒体的新闻非常重要，因为半导体和 AI 一直是市场上的超大主题，」Long 说；而「比我们完全不懂韩语的员工更好地」读韩语新闻，看起来正是一个好用例。

![Interpreting news from Korean media outlets](../../slides/10-13-1630-a-hedge-fund-as-a-function/12.jpg)
*针对一个来源的 gather 步骤，每一行都标了 runner。*

| 代码行 | Runner | slide 上的注 |
| --- | --- | --- |
| `def gather(question, korean_news):` | | One research question, one source（一个研究问题，一个来源） |
| `articles = collect(korean_news)` | Code | Originals, with source and time（原文，带来源和时间） |
| `relevant = filter(articles, question)` | LLM | Against the question, with reasons（对照问题筛选，附理由） |
| `notes = translate_extract(relevant)` | LLM | Sourced observations, in English（带出处的英文要点） |
| `check(notes, against=articles)` | Code | Every quote matches its source（每条引文都和原文对得上） |
| `return update` | Person | Accepts, revises or rejects it（接受、修改或否决） |

光是 collect 就有很多自由度：「可以是一个 MCP，可以是一个 CLI，买数据源的授权，清洗数据源。」文本怎么切块喂给模型，图片按图片传还是转成文字：「我几乎把这些旋钮看作传统机器学习系统里的 hyperparameter……都是我要去调、去优化的东西。」filter 把和投资组合无关的东西去掉（「大概有韩国的 NBA 球迷」）。接着是翻译、提取、摘要还是转成结构化形式的问题，「怎样把信息暴露给模型，有太多可以实验的方式」，而「难的原因之一是它一直在变」。check 把结果和其他来源交叉核对；台上 Long 提到的是内部笔记、内部观点、过去的谈话和会议录音，并说把分散来源的信息拉到一起交叉核对「非常强大」。（slide 上这一行的注更窄：每条引文都和原文对得上。）最后由人决定是否更新公司的观点。

## 5. 每一行都需要一个 runner

![You need a runner](../../slides/10-13-1630-a-hedge-fund-as-a-function/13.jpg)
*四种 runner，各配一个例子。*

「我把每一行都看作有一个 runner，」Long 说。slide 上列了四种：

| Runner | slide 上的例子 | Long 给的理由 |
| --- | --- | --- |
| Code | `if pe < 15: keep()` | 「快，便宜，可靠。」 |
| ML（统计、经典机器学习） | `model.fit(x, y)` | 能刻画「比纯确定性代码复杂得多的关系」 |
| LLM | `summarize(transcript)` | 「用一堆 if 语句写出比现代 LLM 更好的摘要，大概完全不可能。」 |
| Person | `meet(ceo)` | 判断力，以及「在现实世界里周旋的能力」 |

市盈率筛选该交给确定性代码；给某个模式拟合回归线归统计。至于去见一家公司的管理层，「我大概不想派一个 LLM 或 AI agent 去，也不想派一个 if 语句进那场会」。很多情况下，「人才是函数某一块最好的 runner」。

「为了自动化而自动化，没有任何荣耀可言，」Long 说。「我们的目标不是把尽可能多的东西变成 LLM。」接下来说目标是什么的那句话，正好落在字幕空白里。

## 6. 一个函数，很多种接法

![gather() for the Korean news: one function, many ways to wire it](../../slides/10-13-1630-a-hedge-fund-as-a-function/14.jpg)
*三个来源和内部笔记，经过一张步骤图，流向提议和审核。*

真实的版本「大概不只是一串线性语句」。slide 把 `gather()` 画成一张图。标着 `korean_news`、`dart_filings` 和 `call_transcripts` 的三个来源进入 `collect()`，再经 `dedupe()`、`rank()`、`filter()`、`translate()` 分支到 `extract_quotes()`、`extract_figures()` 和 `entity_link()`，再到 `check_sources()`、`compare_to_last_view()`、`key_stock_drivers()` 和 `compare_to_holdings()`，公司自己的笔记也接进来，最后到 `propose()` 和 `review()`。两条虚线回路：引文和原文对不上时打回重做，审核人要求修改时打回重做。

Long 列了几种接法：翻译后提取引文再核对，「确保这些真的是有人说过的话」；或者去重、过滤、提取数字；或者从一份现成的笔记出发，去找能印证它的数据点；「也可能我在做 red team。配置方式有一百万种。所以控制流里又多了一批 hyperparameter。」

![标出一条接法](../../slides/10-13-1630-a-hedge-fund-as-a-function/15.jpg)
*三种有名字的接法，第三种用红线标出。*

slide 给其中三种起了名字：「Read the press, check the quotes」（读报道，核引文）；「From the numbers to key stock drivers」（从数字到关键股价驱动因素）；以及高亮的「Rank first, compare to our last view」（先排序，再和我们上次的观点比）：从韩国新闻经 collect、rank、translate、extract quotes、compare to last view，到 propose 和 review。

## 7. 搜索空间

![按 runner 着色的同一张图](../../slides/10-13-1630-a-hedge-fund-as-a-function/16.jpg)
*每个节点按 runner 重新着色，沿用 runner 那页的图例。*

「这些方框里的每一个，大概都有一个最合适的 runner。」collect 是确定性代码：「我拉某个来源最近 24 小时的内容。」按 runner 那页的形状和颜色，这张图标出的分配是：

| Runner | 节点 |
| --- | --- |
| Code | collect, dedupe, extract_figures, check_sources, compare_to_holdings |
| ML | rank, entity_link, key_stock_drivers |
| LLM | filter, translate, extract_quotes, compare_to_last_view, propose |
| Person | review |

Long 说就连这样「大概也低估了复杂度」。「现实是大多数东西都是 multi-runner」：代码加 LLM 加统计方法，或者人和 LLM 一起。「大概是我这四个桶之间的排列组合。」光 LLM 这一个桶里就还有更多选择：大、中、小模型；xhigh、high、medium 这些 effort 档位；用哪个 harness；harness 加载哪些 skill；skill 加载哪些 hook。「我的搜索空间里有多到离谱的 hyperparameter。而这只是这一个小问题……我只是在过滤新闻。」

出路是把问题拆成「我们能理解、能评估」的小块。又一处字幕空白之后，Long 讲了这样做会发现什么：「哦不，这一步根本不该用模型，应该是个 if 语句。或者，天哪，LLM 在把这件事翻译成对投资组合的影响上真的非常差，这一步真该由人来做」，再由一个确定性系统帮这个人检索、拉取信息。「如果你没有一个可以 hill-climb 的北极星，你就只是在……瞎折腾。」

## 8. 真正的那张图

「我怎么强调都不够：这只是我们做的一件小事，」Long 说。每个数据源要经过的变换都不止三步。「所以像 Coatue 这样的基金，真实的代码图大概长这样——哦，不是这个，这是我的屏保。」

![完整的图和 Long 的联系方式](../../slides/10-13-1630-a-hedge-fund-as-a-function/19.jpg)
*成千上万个输入节点汇聚到一个决定上。*

最后一页是一张扇形图：成千上万个各种形状和颜色的节点，一层层收拢到一个点，上方是 Long 的 X 账号（@fyxlong）和 LinkedIn 地址。「真实的图大概就是这样，对吧？极其复杂。我们刚才说过，每个节点至少有 10 个 hyperparameter，每个至少有 20 个有效取值。」最后一处字幕空白之后，Long 讲到公司还必须「在文化上、组织上消化」的东西。「这就是把 AI 用到投资上的最大挑战。」

## 9. 投资之外

「在投资之外，我们真正在谈的，是造一台状态机：从世界上拿进信息，理解得足够好，做出一致的决定，」Long 说，用途包括政策、非营利、药物研发和企业战略。Long 说在这种场合最兴奋的，是和其他领域的人聊，他们的问题最终「汇聚到同一个问题空间」；Long 邀请大家会后在走廊里接着聊，然后致谢。

---

**链接：** [Schedule](https://ai.engineer/nyc/2026/schedule?session=9a9dfcd0-459f-43eb-b3f8-004864b4ad2a) · [Day 1 直播 6:47:46 处](https://www.youtube.com/watch?v=akEqiXFo3GM&t=24466s)

截至撰写时，这场 keynote 没有单独发布的视频。本文依据 Day 1 官方直播字幕和作者翻拍的直播 slide 照片，外加一张会场照片。字幕有五处各约十秒的空白，所以少了几句话。日程上的题目是「A Hedge Fund as a Function」，slide 上是「An Investment Fund as a Function」。这场演讲没有 Q&A。[精确笔记](../precise/10-13-1630-a-hedge-fund-as-a-function.md)（中文）保留了直播时间戳、字幕勘误和待确认的问题。
