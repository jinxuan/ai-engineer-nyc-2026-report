# A Hedge Fund as a Function：精确版

> Precise notes for this session, compiled from the official livestream captions, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：直播字幕逐段读完；21 张照片逐张核对 banner，并用原图放大读了代码和图上的小字。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | 作者拍的 21 张（IMG_0511–0531 = `01.jpg`–`21.jpg`）。20 张是直播画面的手机翻拍，banner 都是「Keynote · Frank Long / Head of AI · COATUE」，全部属于这场；`20.jpg`（IMG_0530）是在会场里拍的大屏和台上讲者，不是 slide。重复较多：01/02 封面、04/05 同一页、17/18 和 16 基本相同、19/21 同一页 |
| 视频 | 没有单独剪辑的视频。Day 1 官方直播 [akEqiXFo3GM](https://www.youtube.com/watch?v=akEqiXFo3GM&t=24466s)，章节「6:47:46 A Hedge Fund as a Function — Frank Long (Coatue)」，到 7:02:19 下一场。主持人 Alex Lieberman 在 6:47:21 介绍。下文时间都是直播时间 |
| 字幕 | YouTube 自动字幕，ASR 错误较多（公司名被转成 KOTU、COU、CO2，见勘误表）。**有五处约 10 秒的字幕空白**（6:50:53–6:51:03、6:55:52–6:56:02、6:56:53–6:57:05、6:59:24–6:59:36、7:00:34–7:00:46），原始 vtt 里也是空的，那几段讲者说了什么无从得知 |
| 其他一手资料 | 没有找到讲者或 Coatue 公开发布的相关文章或 slide |

## 基本信息

- 时间地点：2026-10-13（周二）4:30–4:48pm，Simons Room (Metropolitan East)，Featured Keynote，Main Stage（资料：[日程](https://ai.engineer/nyc/2026/schedule?session=9a9dfcd0-459f-43eb-b3f8-004864b4ad2a)）。直播 6:47:46 开讲；直播时间是从直播开头算起的时长，中间跳过了休息时段，不能直接换算成钟点
- 讲者：Frank Long，Head of AI，Coatue（日程、banner）
- 官方简介（日程）只有一句：「Frameworks for Thinking about Applying AI to Investing」
- 题目：日程「A Hedge Fund as a Function」；slide 封面和页脚「An Investment Fund as a Function」；讲者口头「a hedge fund as a function」
- slide 19/21 结尾页：「X @fyxlong　LinkedIn linkedin.com/in/franklong1」
- 讲者说（6:47:47）：「I work at Coatue, which is a technology investment firm across public and private markets.」「last year when I joined Coatue, the bet that I wanted to make was I think that agents will have an even bigger impact in investing than they did at coding.」

## 1. 开场：押注 agent 对投资的影响会大过对 coding

- 讲者说（6:47:53）：这个行业要「take high conviction bets on big themes and watch those play out」，「honestly a career is the same way」
- 讲者说（6:48:15）：「I know that's a big statement. We're all here because of the … transformative impact that AI has had in coding, but I really believe it strongly.」今天讲 Coatue 用来实现这种影响的 framework

## 2. 把基金看成一个函数

- slide 01/02：封面「COATUE · 01 / COVER · An Investment Fund as a Function」
- slide 03：

```
def fund(c_external, c_internal):        | Takes external and internal context
    return selection, size, timing       | People own the investment judgment
```

- 讲者说（6:48:29）：「you can think about a hedge fund as a function that takes in two parameters, context external and context internal, and spits out one tuple: selection, size, timing. What stock, how much, when.」
- 讲者说（6:48:49）：这是在解一个「compression problem」：怎样把所有信息压缩成这个简单的 tuple；「the product that we provide is essentially a time series of this tuple that constructs a portfolio」
- 我的理解：slide 03 右栏红字「People own the investment judgment」在 return 那一行，讲者在这里没有口头展开，后面 decide 一行的红字「Make the decision: a person」把它说得更明白

## 3. 人在屏幕上能看到的，模型都应该能用 API 看到

- slide 04/05：「Everything a human can see in pixels, a model can see via API」，左边「A PERSON LOOKS AT」一个显示器线框（标题、折线图、表格）
- slide 06（同一页加上右侧两个框，从原图放大读）：

| 框 | 调用 | 返回 |
| --- | --- | --- |
| c_external | `news.search("Samsung", lang="ko")` | `[{title, source, time}, …]` |
| c_external | `prices.history("005930.KS")` | `[{date, close}, …]` |
| c_external | `filings.get("005930.KS", "Q3")` | `{revenue, op_margin, …}` |
| c_internal | `positions.current()` | `[{ticker, weight, cost}, …]` |
| c_internal | `liquidity.requirements()` | `{cash_floor, redemptions}` |
| c_internal | `case_studies.search("Samsung")` | `[{memo, author, date}, …]` |

- 资料：005930.KS 是三星电子在韩国交易所的代码（[Yahoo Finance](https://finance.yahoo.com/quote/005930.KS/)）
- 讲者说（6:49:14）：「every single thing that a human can see in pixels on the screen, an agent in your building should be able to see via API」。比喻：「imagine you brought the smartest people ever, the greatest investors alive … but you gave them a computer without login to 75% of your services. It's almost immaterial how smart the system is if it doesn't have the tools to actually action that.」
- 讲者说（6:49:44）：external context：新闻、价格、filings，「not via dragging and dropping, copy and pasting, like directly via API」
- 讲者说（6:50:00）：internal context「is just as important」：「your own firm situation」，比如当前持仓、对某些股票的流动性和风险要求、过去的 case studies，或者和某个行业、股票、管理层打交道的历史
- 讲者说（6:50:22）：这是地基，「no credit to me, Coatue has been an amazing leader on this for over a decade. It's both technological and cultural.」有很强的数据科学团队、流程和系统；文化上「super super rigorous」：「For decades, we filed every single note about every single trade, about every single decision we made or didn't make, about what went well and what didn't go well.」
- 出入：讲者先说「over a decade」，紧接着说「For decades」
- 字幕空白 6:50:53–6:51:03；之后接「… a context window and get my magic numbers and go home, right? It can't be that easy」。我的理解：空白里大概是「那是不是把所有 context 塞进 context window 就行了」之类的设问，【待确认】

## 4. 拆到每一块都能检查

- slide 07–09「Break it down until each piece can be checked」，逐行加出来，最终版：

```
def fund(c_external, c_internal):                  | Takes external and internal context
    info = gather(c_external, c_internal)          | Gather information
    view = analyze(info)                           | Analyze it
    selection, size, timing = decide(view)         | Make the decision: a person
    return selection, size, timing                 | People own the investment judgment
```

  （decide 那行的函数名和说明是红字）
- 讲者说（6:51:14）：第一步 gather，把内外部 context 收进来，过滤相关和不相关的。例子：「I pulled some news articles about the NBA that probably isn't material to stocks, but oh well, this is actually related to some local permitting that might actually impact data centers. Like there's a lot of nuance in this gather step.」（字幕作「MBA」，按后文「Korean NBA fans」改）
- 讲者说（6:51:41）：然后 analyze，形成观点：「what that means for the markets, public and private」；再把观点变成决定，「A decision object has these three parameters」
- 讲者说（6:52:04）：每一步都有输入和输出，下一步的输入是上一步函数的输出；「as an engineer, this is a way for me to conceptualize the problem in a way that makes a lot of sense. But even this, it really can't be as simple as writing three functions.」

## 5. 案例：解读韩国媒体的新闻

- 讲者说（6:52:36）：看第一步里「a tiny subpiece of a tiny subpiece of just gathering information」：解读新闻。「news from Asia and Korean media outlets has been super important this year because semis [and] AI have been a huge huge theme in the market」，这是 AI 很可能有影响的地方：「reading Korean language news better than our completely non-Korean staff」
- slide 10–12「Interpreting news from Korean media outlets」（面包屑 fund › gather），逐行加出来，最终版（中间一栏是 runner 标签）：

| 行 | 代码 | runner | 说明 |
| --- | --- | --- | --- |
| 01 | `def gather(question, korean_news):` | | One research question, one source |
| 02 | `articles = collect(korean_news)` | CODE | Originals, with source and time |
| 03 | `relevant = filter(articles, question)` | LLM | Against the question, with reasons |
| 04 | `notes = translate_extract(relevant)` | LLM | Sourced observations, in English |
| 05 | `check(notes, against=articles)` | CODE | Every quote matches its source |
| 06 | `return update` | PERSON | Accepts, revises or rejects it |

- 讲者说（6:53:17）：collect：「this could be an MCP. This could be a CLI. License the data feeds. Clean the data feeds.」光这一步就有很多自由度：怎么切块给 LLM、图片按图片拉还是转成文字，「I almost think about those knobs as like hyperparameters in a traditional machine learning system … something for me to tune and optimize」
- 讲者说（6:54:00）：filter：「there's probably Korean NBA fans」，很多内容和组合里重要的名字、主题、股票无关，要过滤掉
- 讲者说（6:54:22）：「translate slash extract slash should I summarize? Should I extract this into some sort of structured form?」怎样把信息暴露给模型有很多实验方式，「one of the reasons this is hard is it keeps changing」
- 讲者说（6:54:39）：check：拿内部笔记、内部观点、过去的谈话、会议录音交叉核对，「the idea of pulling information from disparate sources and cross referencing them, that's super powerful」；最后 update，可能更新观点
- 讲者说（6:55:00）：右边那栏「code, LLM, LLM, code, person」，「there's many, many ways to run these functions」

## 6. 每一行都需要一个 runner

- slide 13「You need a runner」：四种 runner，上方各配一个例子：

| Runner | 例子 | 图标 |
| --- | --- | --- |
| CODE（黑色方块） | `if pe < 15: keep()` | 闪电、盾牌、美元 |
| ML（绿色菱形） | `model.fit(x, y)` | 散点拟合曲线 |
| LLM（灰色圆） | `summarize(transcript)` | 大脑 |
| PERSON（红色圆） | `meet(ceo)` | 显微镜、两人对话 |

- 讲者说（6:55:13）：「I think of each line as having a runner」。code：「It's fast. It's cheap. It's reliable.」比如「if the price to earnings ratio is less than 15, I should just use deterministic code for that」
- 讲者说（6:55:36）：第二种是统计、经典机器学习，比如给某个模式拟合一条回归线，「you can model much more complex relationships than just pure deterministic code」
- 字幕空白 6:55:52–6:56:02（「And for even more complex relation…」之后），接「that's the state of the art, right? It's probably totally impossible to build a bunch of if statements that can summarize better than a modern LLM.」我的理解：空白里在讲第三种 LLM runner，对应 slide 的 `summarize(transcript)`
- 讲者说（6:56:13）：人也是重要的 runner。比如见公司管理层：「I like the new [Unitree?] robots, but I probably don't want to send an LLM or an AI agent in to do that. I probably don't want to send an if statement into that meeting.」因为判断力和在现实世界里周旋的能力，很多时候「a person is the best runner for a given piece of the function」（「new unit robots」原文不清，【待确认】）
- 讲者说（6:56:42）：「There is no glory in automating things for the sake of automating things. The goal is not to take as many things and make them LLM as possible for us. The goal is …」——这里进入字幕空白 6:56:53–6:57:05，目标是什么没有听到，【待确认】

## 7. 一个函数，很多种接法

- slide 14「gather() for the Korean news · one function, many ways to wire it」：一张数据流图。输入 korean_news、dart_filings、call_transcripts，以及 c_internal 下的 our_notes；节点 collect() → dedupe() / rank() → filter() / translate() → extract_quotes() / extract_figures() / entity_link() → check_sources() / compare_to_last_view() / key_stock_drivers() / compare_to_holdings() → propose() → review()。两条红色虚线回路：check_sources() 回到 extract_quotes()，标「QUOTE DOESN'T MATCH」；review() 回到 propose()，标「REVISE」；还有一条从 review() 回到 our_notes 的虚线
- 我的理解：dart_filings 大概指韩国金融监督院的电子公示系统 DART 上的公告，讲者没有口头提到，【待确认】
- 讲者说（6:57:05）：真实的函数「probably not just a linear statement」，有各种分支。也许先 collect 再 translate，主要目标是 extract quotes 并对照原文核实「that these are actually things that people said」；也许先 dedupe、filter，再提取外部数字；也许先传入一份笔记，找确认性的数据点；「or maybe I'm trying a red team. There's a million different ways to configure this. So yet again more hyperparameters within the control flow」
- slide 15（同一张图，一条路径标红）：底部三种接法：「1 READ THE PRESS, CHECK THE QUOTES」「2 FROM THE NUMBERS TO KEY STOCK DRIVERS」「3 RANK FIRST, COMPARE TO OUR LAST VIEW」，第 3 条高亮：korean_news → collect → rank → translate → extract_quotes → compare_to_last_view → propose → review
- 讲者说（6:57:53）：「So many ways to think about just this very simple problem of pulling, filtering, summarizing, aggregating the news for LLMs.」

## 8. 每个节点都有最合适的 runner

- slide 16（以及几乎相同的 17、18）：同一张图，节点按 runner 换了形状和颜色（对照 slide 13 的图例，我的理解）：
  - 黑色方块（code）：collect()、dedupe()、extract_figures()、check_sources()、compare_to_holdings()
  - 绿色六边形（ML）：rank()、entity_link()、key_stock_drivers()
  - 灰色椭圆（LLM）：filter()、translate()、extract_quotes()、compare_to_last_view()、propose()
  - 红色（person）：review()
- 讲者说（6:58:02）：「each of these boxes probably has an optimal runner」，比如 collect 是 deterministic code：「I'm pulling last 24 hours for a given source」；（6:58:19–6:58:23 短暂空白）「… decision models, maybe statistical methods, a combination」
- 讲者说（6:58:29）：「this probably understates the complexity. The reality is most things are multi-runner」：code 加 LLM 加统计方法，或者人和 LLM 一起；「it's probably combinatorics between my four buckets」
- 讲者说（6:58:47）：LLM 这一类里还能再调：大、中、小模型；effort「xhigh, high, medium」；选 harness；「which skills load with the harness」；「which hooks load with the skills」。「I have an absolutely enormous amount of hyperparameters within my search space. And this is just for this one tiny problem … I'm just filtering the news.」
- 讲者说（6:59:16）：拆问题的方法是看「what can I actually compartmentalize in a way that we can understand and evaluate」——接字幕空白 6:59:24–6:59:36，之后：「Oh, no. That actually shouldn't be a model at all. That should be an if statement. Or, oh man, LLMs are really, really bad at translating that to portfolio impact. It should really be a human on that step, but they're using a deterministic system to help them search and pull the information.」
- 讲者说（6:59:51）：「if you don't have a north star to hill climb against, you're just kind of … you're just fidgeting around.」
- 我的理解：空白前后连起来看，讲者的意思是先把每一块拆到能评估，评估之后才能发现某一块该换 runner；「north star」指可以 hill-climb 的评估目标，但具体怎么评估讲者没有展开

## 9. 真正的图有多大

- 讲者说（7:00:01）：「I cannot convey enough that this is just one tiny thing we do.」每个数据源都不止三步变换；「So the actual graph of code at funds like Coatue, it actually probably looks — oh, not like this. This is my screen saver.」
- slide 19/21：结尾页，一张巨大的扇形图，左边上千个输入节点（各种形状和颜色），一层层汇聚到右边一个节点和一个红点；上方是讲者的 X 和 LinkedIn
- 讲者说（7:00:23）：「The actual graph probably looks something like this, right? It's probably massively complex. In each of those nodes, we just talked about at least 10 hyperparameters with at least 20 different valid values for them.」
- 我的理解：按讲者的数字，每个节点至少 20^10 ≈ 10 万亿种配置，讲者没有算出来
- 字幕空白 7:00:34–7:00:46，之后：「… also absorb culturally, organizationally. That is the great challenge of applying AI [to] investing.」空白里的前半句【待确认】

## 10. 结尾：把世界的信息变成一致决策的状态机

- 讲者说（7:00:56）：「beyond investing, what we're really talking about is creating a state machine that can take information from the world and understand it well enough to make coherent decisions」，用途远不止投资机构：「in policy, in nonprofits, in drug discovery, in corporate strategy」
- 讲者说（7:01:18）：在这种场合最兴奋的是和其他领域、问题却「congeal on the same problem space」的人交流；「I'll hang out in the hallway afterwards. Here are my socials. And thank you everyone for your time.」

## Q&A 精确版

没有 Q&A。讲者 7:01:33 致谢，掌声后主持人在 7:01:53 直接介绍下一场 Pim de Witte（General Intuition）。

## 勘误表（YouTube 字幕 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| KOTU / COU / CO2 / COTU | Coatue | 日程、banner、slide |
| MBA（6:51:27） | NBA | 讲者后文「Korean NBA fans」 |
| tupole | tuple | 上下文 |
| prices to earnings ratio | price-to-earnings ratio | slide `if pe < 15` |
| LM | LLM | 上下文 |
| ddup | dedupe | slide `dedupe()` |
| polling, filtering | pulling, filtering | 上下文 |
| combinotaurics | combinatorics | 上下文 |
| X high, high, medium | xhigh, high, medium（effort 档位） | 上下文 |
| code and funds like CO2 | code at funds like Coatue | 上下文 |
| new unit robots | 可能是 Unitree robots，【待确认】 | 上下文 |

## 出入

1. **题目**：日程「A Hedge Fund as a Function」，slide「An Investment Fund as a Function」，讲者口头「a hedge fund」，介绍 Coatue 时说的是「technology investment firm across public and private markets」
2. **「over a decade」与「for decades」**：讲者先说 Coatue 在数据基础上领先「over a decade」，下一句说「For decades, we filed every single note」
3. **slide 12 和口述的顺序**：slide 上是 collect → filter → translate_extract → check → update；口述时讲者把 check 说成「checking our own internal notes … against these things」（和内部笔记交叉核对），slide 上 check 的说明是「Every quote matches its source」（核对引文和原文）。我的理解：slide 14 里两者分成了 check_sources() 和 compare_to_last_view() / compare_to_holdings() 两类节点
4. **runner 标签**：讲者口述右栏是「code, LLM, LLM, code, person」，和 slide 12 一致

## 【待确认】清单

1. 五处字幕空白里讲者说的内容，尤其是 6:56:53 处「The goal is …」之后的目标是什么
2. 「new unit robots」是否指 Unitree
3. dart_filings 是否指韩国 DART 公告
4. 讲者说「last year when I joined Coatue」的具体加入时间（没有找到 Coatue 的官方公告）
