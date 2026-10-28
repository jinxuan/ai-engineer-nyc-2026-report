[English](../10-13-0954-does-code-quality-even-matter.md) | **中文**

# 代码质量要不要讲究，看软件有多 disposable；而 rigor 可以变便宜

*在 AI Engineer NYC 2026 上，Capital One 的 Max Kanat-Alexander 认为，代码质量存在的唯一目的，是让人理解软件、安全地修改软件。所以对 AI 正在让它变得遍地都是的一次性程序来说，代码质量不再重要；durable 系统仍然需要它，而且可以花更少的代价得到它。*

**Featured Keynote · 2026 年 10 月 13 日（周二）· 9:54–10:12am · Simons Room (Metropolitan East)，Main Stage**
**讲者：** Max Kanat-Alexander，Principal Software Engineer，Capital One

---

Max Kanat-Alexander 用反话开场。「今天我想挑一个大家都没什么看法、一点争议都没有的话题，」Kanat-Alexander 说，又补了一句：社交媒体上每个观点都「really deeply nuanced」（极其细腻），「也许事情比 yes 或 no 稍微复杂那么一点」。

![Absolutely no, absolutely yes](../../slides/10-13-0954-does-code-quality-even-matter/01.jpg)
*网上争论的两个阵营。*

Kanat-Alexander 在 Capital One 负责 AI 辅助软件开发，之前是 Bugzilla 项目的 Chief Architect、Google Code Health 的技术负责人；主持人介绍 Kanat-Alexander 时说的是《Code Simplicity》一书的作者。这场 talk 想替换掉每个 senior engineer 最爱的答案。「It depends，」Kanat-Alexander 说，「但说实话这个答案没多大用，因为到底取决于什么？」出发点是：代码质量当初为什么重要。「我看到很多人说，嗯，因为它让我感觉好。这理由不够。」

## 1. 一切取决于理解

![Understanding and executing on that understanding](../../slides/10-13-0954-does-code-quality-even-matter/02.jpg)
*决定任何软件结果的两个因素。*

Kanat-Alexander 说，用软件想做成的任何事，都「depends on two factors and two factors alone」（只取决于两个因素）：理解，以及基于理解去执行；目标可以是「对齐一个模型、扩展一个系统、推动采用」。debugger、observability、eval 乃至测试，存在都是为了提高其中之一。「我们在代码质量领域做过的一切，都只是为了这两件事。」它和 correctness 或别的什么都无关。

![Behavior and how to correctly modify it](../../slides/10-13-0954-does-code-quality-even-matter/03.jpg)
*软件工程师最需要的两种理解。*

对工程师来说，最重要的两种理解是：系统的实际行为（「不是文档说它的行为是什么」），以及怎样正确、安全地修改这个行为。

## 2. 为什么结构重要

![Hello World to Hi World](../../slides/10-13-0954-does-code-quality-even-matter/04.jpg)
*在很小的系统里，理解就是知道它做什么。*

很小的程序没什么可理解的：它打印 hello world，你想让它说 hi world，就改一下。真实的系统不一样。

![Chromium 代码库全景图](../../slides/10-13-0954-does-code-quality-even-matter/05.jpg)
*「A real map of the code base of Chromium.」*

下一张 slide 是一幅密密麻麻的可视化，Kanat-Alexander 说这是「Chromium 代码库的真实地图，Chromium 是 Chrome 背后的开源项目」。组件之间的交互方式那么多，它有「essentially infinite possible behaviors」（几乎无穷多种可能的行为）。「我没法用语言向你描述 Chromium 整个系统的行为。」（这幅图的出处没有说明。）

![Chromium 的 URL parser](../../slides/10-13-0954-does-code-quality-even-matter/06.jpg)
*解析 `https://example.com:443/articles?id=7#summary` 的调用树。*

工程师能描述的是组件。这张 slide 画出 Chromium 的 URL parser 怎样拆一个 URL：`ParseStandardUrl` 调用 `DoExtractScheme` 和 `DoParseAfterSpecialScheme`，后者再调用 `DoParseAuthority`（它又调用 `ParseUserInfo` 和 `ParseServerInfo`）、`FindNextAuthorityTerminator` 和 `ParsePath`（它又调用 `FindQueryAndRefParts`）。这些函数名和 Chromium 的 `url_parse.cc` 一致。「这就是代码存在的全部理由，」Kanat-Alexander 说，「为了结构。」

![Python 和汇编里的 Hello World](../../slides/10-13-0954-does-code-quality-even-matter/07.jpg)
*输出一样，结构不同。*

slide 上两个程序在 Linux 上都打印 Hello World：一行 `print("Hello World")`，和一段装寄存器、做系统调用的 x86-64 汇编。前者「has the identity and structure of Hello World」（有 Hello World 的身份和结构）；后者只是「一串没有特定身份的指令」。到了几百万、几十亿行，「就难懂得多了」。Kanat-Alexander 说，readability、testability 听上去像是人的关切，「但真正一直最重要的，是系统有某种可以理解的结构」。

## 3. 每次修改的两类风险

![两类风险](../../slides/10-13-0954-does-code-quality-even-matter/08.jpg)
*每次修改都同时带着这两类。*

结构最要紧的时候是修改系统，而每次修改都有两类风险。第一类是引入问题，而问题不只是 bug：项目拖期、做出来不是用户想要的、预算超支导致失败，都算。第二类是让以后的修改更难，也就是让未来出问题的可能性变大。

![两类风险都来自理解](../../slides/10-13-0954-does-code-quality-even-matter/09.jpg)
*同一张列表加上了根因：难以理解，以及让它更难理解。*

两者都归结到理解：第一类来自系统本身难懂，第二类来自把系统改得更难懂。「我读过的每一份事故 postmortem，都能归结到这两个因素，」Kanat-Alexander 说：生产配置太难懂，有人写了没法懂的东西，系统的 emergent properties（涌现特性）没人能理解。第二类又反过来喂第一类，形成「a vicious cycle that compounds in a non-linear fashion」（非线性复利的恶性循环），直到系统走到「每次修改都会带出某种问题」的地步，比如那些「一亿、五亿行的大型主机代码库」。

Kanat-Alexander 承认，未来的模型也许会有「处理复杂度的无限能力」，但这不是今天的话题，因为模型眼下正在做另一件有意思的事。

## 4. Disposable software

模型正在创造「一个新的软件类别」，Kanat-Alexander 说。「这个类别一直存在，但它马上会比任何其他形式的软件常见十万倍。那就是 disposable software。」

![四个因素](../../slides/10-13-0954-does-code-quality-even-matter/10.jpg)
*失败的后果、依赖它的人和系统、替换成本、维护成本。*

设想一个软件：失败的后果很小甚至没有，几乎没有人、也几乎没有别的系统依赖它，change management 几乎不存在，「几小时或几天」就能整个替换掉，维护几乎不花钱。「在那样的世界里，代码质量不重要。」因为它要防的那些复利风险根本不存在。

## 5. Durable software 不会消失

![Durable 和 disposable software](../../slides/10-13-0954-does-code-quality-even-matter/11.jpg)
*两类软件并排对比。*

| Durable software | Disposable software |
| --- | --- |
| Lives for years（存活多年） | Replaceable in hours（几小时就能替换） |
| Depended on by millions（数百万人依赖） | Depended on by a few, or many who don't care if it changes（少数人依赖，或依赖的人不在乎它变） |
| Major consequences of failure（失败后果严重） | If it doesn't work, I'll just make my own（不好用我就自己做一个） |
| Requires continuous maintenance（需要持续维护） | Maintenance is unimportant or nearly free（维护不重要或几乎免费） |

传统上，业界一直在造 durable software，它的失败可能是「business catastrophic」（对业务是灾难性的）（台上 Kanat-Alexander 说的是「数百万或数千人依赖」）。「世界上大多数软件工程师，大部分时间都在维护这些巨大的 durable 系统。」Kanat-Alexander 预计这会继续下去，并说这正是网上讨论跑偏的地方：durable 系统大概仍会是世界上最重要的系统，而且大概会更多。「但数量最多的系统会是 disposable software。」

一个 disposable 工具不好用，「你会给每个人 agent，他们会自己做一个」。「不喜欢系统自带的计算器？自己做一个。要给孩子办派对？就为这一场派对写个派对策划软件。用完扔掉。代码质量不重要，没人在乎。」

## 6. 按 disposability 决定 rigor

![从 less rigor 到 more rigor](../../slides/10-13-0954-does-code-quality-even-matter/12.jpg)
*一条光谱：一端更 disposable、rigor 更少，另一端更 durable、rigor 更多。*

「In the future, we will apply engineering rigor to the degree of disposability of the software.」（将来，我们会按软件的 disposable 程度来施加工程 rigor。）Kanat-Alexander 说。在 disposable 一端，「我们的工程 rigor 实践一样都不重要」；在 durable 一端，几十年积累下来的所有实践「都非常重要」。

![光谱上的四个因素](../../slides/10-13-0954-does-code-quality-even-matter/13.jpg)
*consequences、cost of replacement、dependencies 和 cost of maintenance 决定系统落在线上的哪里。*

决定系统落在这条线上哪里的是四个因素：consequences、cost of replacement、dependencies、cost of maintenance。全低就是 disposable，全高就是 durable。Kanat-Alexander 凭经验加了一句提醒：「你应该往 durable 那边偏。你应该假设这几项都比你估计的高。」

## 7. 让软件更 disposable

团队还可以做一件新的事：「You can make software more disposable.」（你可以让软件更 disposable。）（这页 slide 没有照片，下面按口头内容整理。）Kanat-Alexander 讲了四个手段。

**隔离环境。** 一个只能接触你自己数据的部署环境：「没有客户数据，没有个人身份信息。」

**强制独立。** 「这个环境里的系统，不允许接受其他生产系统的 inbound API 调用」，于是部署到那里的东西都是 disposable 的。

**冗余。** 保留「一个又老又笨的备用系统」。大家可以 vibe code 自己更顺手的前端，因为那个「随手 vibe code 出来的小破玩意」坏了的时候，durable 系统还在。「它肯定会坏，对吧？没关系。坏了也无所谓。」

**限制范围。** 只给一个人用的计算器；只管一场派对的策划工具。

Kanat-Alexander 把这称为「a whole new discipline of software engineering」（软件工程的一个全新分支）：不是提高 durable 系统的 rigor，而是鼓励、允许更多的 disposability。

## 8. 降低 rigor 的成本

![Durable software engineering is not going away](../../slides/10-13-0954-does-code-quality-even-matter/14.jpg)
*Kanat-Alexander 再次强调的一点。*

「这是我今天在社交媒体上看到的最大的误解，」Kanat-Alexander 说。「我相信世界上 durable software 的绝对数量会增加，因为 AI coding 工具让做出更多 durable software 越来越容易。」disposable 和 durable 不是非黑即白；沿着光谱变化的，是投入 rigor 的努力有多少，「既包括眼前的努力，也包括长期的努力」。

![有无更好地基时的工程投入](../../slides/10-13-0954-does-code-quality-even-matter/15.jpg)
*没有更好的地基，越往 durable 一端工程投入涨得越陡；有了，就涨得平缓。*

这张图的纵轴是工程投入，横轴从 disposable 到 durable：标着「without better foundations」的曲线陡峭上升，标着「with better foundations」的平缓上升。Kanat-Alexander 说，最该关心的是「reducing the cost of rigor」（降低 rigor 的成本）。rigor 越便宜，系统落在光谱哪里就越无所谓，猜错了也越容易补救：「把东西做成 durable 的代价越低，你改主意或者判断错就越容易，因为你对这个估计不可能永远百分之百正确。」这需要另一套实践，「它们本身不是工程 rigor」，而是「关于怎样降低工程 rigor 成本的」实践。

计时器上「大概还剩 5 分钟」，Kanat-Alexander 讲了四个（这页同样没有照片）：

**Standardization（标准化）。** 「我们做标准化，是因为它消除了可能性。」系统里没有 PHP，就没人需要去搞清楚 PHP 的 linter、实践和安全漏洞。框架和 RPC 协议也一样：「没错，你仍然可以让 agent 把你所有的工具都写出来，但你每次都得重来一遍。」

**In-workflow tooling（嵌在工作流里的工具）。** 「自己就会发生的工具。程序员不用选择去运行它，也不用理解它怎么运行。」它必须可靠、false positive 少、要快；这样它也能用在 disposable software 上。「如果它免费又快，谁会在乎？」

**让测试更容易。** 不是怎样把测试做得更好，而是「怎样让谁都不用去想怎样把测试做得更好」，让每个测试都又快又确定，不需要人人都是专家。Kanat-Alexander 当 test owner 时一直有个梦想：每个团队都提供自己服务的 in-memory fake 版本。「你猜怎么着？coding agent 能做这个。所以你现在就可以做了，对吧？但你得选择去做，并让所有人都能用。」

**Maximum strictness（最严格）。** 把所有 lint 规则开到最大，「永远不妥协」。和标准化一样，它省掉了大量讨论；只要 false positive 少、后果不大，偶尔 lint 判错也可以接受。反面是在百万行代码库里很晚才打开 linter，然后面对「我要改的 100,000 行」。

Kanat-Alexander 说，这些只是「可以用在另一条轴上的一整块软件工程学科」里的几个方面。

## 9. 答案

![Does code quality even matter?](../../slides/10-13-0954-does-code-quality-even-matter/16.jpg)
*最后一页回答了三个问题。*

有了这个 framework，「it depends」就能变成 yes 或 no。对 disposable software，代码质量重要吗？不重要。对 durable software？重要。还有第三个「更有意思」的问题：它一定很贵吗？「不，」Kanat-Alexander 说，然后结束了演讲。

---

**链接：** [Schedule](https://ai.engineer/nyc/2026/schedule?session=5191589c-a70e-4806-99dc-86f7794f46b9) · [视频：「Does Code Quality Still Matter? It Depends on This」](https://www.youtube.com/watch?v=bU5gDVyu_28) · [Day 1 直播 43:16 处](https://www.youtube.com/watch?v=akEqiXFo3GM&t=2596s) · [Chromium url_parse.cc](https://chromium.googlesource.com/chromium/src/+/refs/heads/main/url/third_party/mozilla/url_parse.cc) · [Code Simplicity](https://www.codesimplicity.com/)

本文依据官方视频字幕（和 Day 1 直播对照过）以及作者翻拍的直播 slide 照片。YouTube 视频改名为「Does Code Quality Still Matter?」，日程和 slide 上是「Even」。这场演讲没有 Q&A。[精确笔记](../precise/10-13-0954-does-code-quality-even-matter.md)（中文）保留了时间戳和字幕勘误。
