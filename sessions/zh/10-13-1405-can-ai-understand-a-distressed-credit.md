[English](../10-13-1405-can-ai-understand-a-distressed-credit.md) | **中文**

# 两个前沿模型对一笔困境债务看法相反，直到把推理拆成结构化的步骤

*在 AI Engineer NYC 2026 上，Axim Intelligence 创始人 Harneet Kaur 展示了两个前沿模型对 iHeartMedia 债务给出相反的前景判断，把分析拆成可检查的几层之后两者趋于一致，然后追问：两个模型意见一致，是不是就等于判断正确。*

**Breakout · 2026 年 10 月 13 日（周二）· 2:05–2:23pm · Turing Room (Riverside Ballroom)**
**讲者：** Harneet Kaur，Axim Intelligence 创始人兼 CEO（此前在 Morgan Stanley，做过困境债务投资人）

---

Harneet Kaur 用一句话介绍 Axim Intelligence：「我们在为 distressed credit 投资做 intelligence layer。」Kaur「在金融业干了 10 年」，最后几年在一家困境私募和对冲基金「做 distressed credit 和 special situations 投资」。演讲要回答的问题是：模型能不能把一家陷入财务困境的公司理解到足以判断接下来会发生什么的程度。

## 1. 什么是 distressed credit

![What is a distressed credit?](../../slides/10-13-1405-can-ai-understand-a-distressed-credit/01.jpg)
*一笔债务从正常履约滑向困境的过程，以及三种可能的结局。*

「Credit 就是一家公司的债券或贷款，」Kaur 说，「Coca-Cola 的债券就是一笔 credit。」当公司面临两件事的风险时，这笔债务就处于 stressed 状态：「钱用光」，以及「还不上债」。

概览页画了一条下行曲线，穿过三个阶段，然后分成三种结局：

| 阶段 | 幻灯片描述 |
| --- | --- |
| 01 Performing | 债务正常还本付息。 |
| 02 Deterioration | 流动性收紧；covenant 受到考验。 |
| 03 Distress | 无法再融资，或流动性出问题。 |

| 结局 | 幻灯片描述 |
| --- | --- |
| Negotiated exchange | 庭外债务置换 |
| Liability management exercise (LME) | Up-tier 或 drop-down 操作 |
| Bankruptcy | 申请庭内 Chapter 11 |

Kaur 说，信用市场每年都有一部分处于 stress 或 distress，原因可能是周期性的（建筑或地产下行）、经营性的（餐饮公司的食品召回），也可能是长期性的冲击：「如果你今天是一家软件公司，可能正面临 AI 的冲击。」而困境不止一种结局。「distress 是什么意思？其实没那么简单。破产只是困境的一种结局」；其他还有 liability management exercise，或者「庭外重组、协商置换」。

## 2. 两个老世界：统计模型和人类分析师

![Distressed analysis has historically lived in 2 worlds](../../slides/10-13-1405-can-ai-understand-a-distressed-credit/02.jpg)
*一边是统计违约模型，一边是信贷经理，各有短板。*

下一页把这个领域的历史一分为二：

| | Machine learning models | Credit managers |
| --- | --- | --- |
| 任务 | 预测恶化 / 违约 | 分析信贷投资 |
| 工具 | Altman Z-Score；Chava–Jarrow；现代 ML | 承销审查；观察名单；投资委员会 |
| 短板 | 统计风险上强，上下文上弱。 | 上下文丰富，但高度依赖人工、难以规模化。 |

「过去，困境问题在工程上相当二元，」Kaur 说：违约或不违约。「今天就复杂多了。」10 年或 20 年前管用的评分，「最有名的是 Altman Z-score」，对于一个「有时按月、按季度、按年变化」的局面来说太静态了。市场也变得更不透明：「private credit 等领域的兴起，带来了定价的缺失。」（Z-score 出自 Edward Altman 1968 年发表在 *The Journal of Finance* 上的论文；Chava–Jarrow 指 Sudheer Chava 和 Robert Jarrow 2004 年发表在 *Review of Finance* 上的破产预测模型。）

另一个世界是定性的：读财报、理解债权人那一方、读信贷文件。「这个过程非常依赖人工，很难规模化。」

## 3. 问题：理解一笔 credit 意味着什么？

![Can AI understand a distressed credit?](../../slides/10-13-1405-can-ai-understand-a-distressed-credit/03.jpg)
*投资人关于 iHeartMedia 信贷状况会问的十个问题。*

Kaur 创办 Axim 时的问题是：「AI 在理解一笔困境……债务、以及它可能走的路径上，能走多远。还有，能不能把一个其实相当复杂的流程系统化。」这页把 iHeartMedia 的 logo 放在十个问题中间：资本结构是什么？哪些在结构上优先？LME 之后变了什么？有多少流动性？ABL（asset-based loan，资产抵押贷款）的到期怎样影响局面？回收率会是多少？下一个事件是什么？和上个季度相比变了什么？近期的关键催化剂是什么？今天和 2018 年相比如何？

Kaur 把这类问题分成财务的（流动性、资本结构）、法律的（「债务在结构上的优先级是什么」）和经营的（收入会涨还是跌）。像「Rogo、Harvey 或者其他一些大的 AI 公司」会逐题给模型做 benchmark，比如「债务的哪一部分在结构上优先？」，Kaur 称之为「the big finance bench」。

Kaur 的看法不同：「对 credit，尤其是 distressed credit 的真正理解，是你能不能预测接下来会发生什么。」这意味着要有「一个关于它未来走哪条路的概率分布」。

## 4. 测试案例：iHeartMedia，以及两个意见相左的模型

![There's no "ground truth" for predicting stress or distress](../../slides/10-13-1405-can-ai-understand-a-distressed-credit/04.jpg)
*两个模型对 iHeartMedia 到 2029 年走势的概率判断，ground truth 一栏是空的。*

Kaur 选了 iHeartMedia，「一家 2018 年申请过破产的公司……该折腾的都折腾过了。2024 年它又做了……一次 stressed transaction。所以这是一个进行中的局面……没有 ground truth。」（公开文件显示，iHeartMedia 2018 年 3 月申请 Chapter 11，2019 年 5 月脱离破产；2024 年 12 月完成一次置换，持有约 48 亿美元、占现有债务 92.2% 的债权人换成了新票据和定期贷款，大部分到期日延后三年。）

「我们把它扔给最聪明的模型试试，」Kaur 说，「我用了 Max Intelligence 档的 Astra 6，还有 Opus 5.5。」拿到「结构化的数据上下文」之后，两个模型对 iHeartMedia 到 2029 年 12 月 31 日的走势看法不一：

| 路径 | Astra 6 | Opus 5.5 | Ground truth |
| --- | --- | --- | --- |
| 协商重组 | **35%** | 20% | ? |
| 再融资 | 30% | 15% | ? |
| 破产 | 25% | **62%** | ? |
| 激进的非按比例 LME | 10% | 3% | ? |
| 合计 | 100% | 100% | ? |

Astra 6 认为最可能的结局是协商重组；Opus 5.5 认为是破产。「你怎么判断哪个对哪个错？判断不了，因为我不知道真实概率是多少，」Kaur 说，「我对这些事有自己最好的判断。我会说我偏向 Astra 6，但大家都是人嘛。」Kaur 没有说明结构化上下文里包含什么，也没说两个模型拿到的 prompt 是否完全相同。

## 5. DistressedBench：逐层检查推理

![DistressedBench: eval framework for distressed credit analysis](../../slides/10-13-1405-can-ai-understand-a-distressed-credit/07.jpg)
*四层，每层先评估再进入下一层：extraction、reconciliation/derivation、judgment、prediction。*

最终答案没法打分，但通往答案的步骤可以。「我怎样至少改进对它们过程的评估，」Kaur 问，这样「我就能看……我是否同意那些不同的假设」。结果就是「这样一种 benchmark……我们对每一层做 benchmark」。幻灯片把它叫做 DistressedBench，列了四步：Extraction → Reconciliation/Derivation → Judgment → Prediction。

模型在每一层都可能出错。Kaur 说，extraction「对上市公司来说……基本解决了」，但「对私募贷款，还有……遇到 Excel 文件和 PDF 的时候，还是可能有挑战」。derivation 建立在它上面：「extraction 是你拿到了 EBITDA……derivation 是，好，现在有了 EBITDA 和债务，就能算出杠杆率。」judgment 问的是：「现在有了杠杆率，这是不是一笔 distressed credit？」

这时 Kaur 的电脑没电了（「我想我的电脑挂了」），演讲停了大约一分钟。恢复后，Kaur 解释说，等模型走到 prediction 这一步时，「你已经确保了数据点是对的，确保了……reconciliation 这一层是对的，确保了有某种合理的判断」。这和人类分析师的工作方式一样：「如果你在一个错误的数据点上做判断，那不会是个好判断。」Kaur 没说 DistressedBench 有多少道题、谁来标注、每一层怎么打分；Axim 的网站上它标着「coming soon」。

## 6. 结果：两个模型趋于一致

结果那页没拍到。Kaur 报告说，「一旦我们真正建立了结构化推理流程……两个模型对这家公司未来概率的判断都趋于一致了」。不再是「Opus 说 62% 破产、Astra 说 25% 破产，现在两个模型都说破产的概率其实只有四分之一，也就是 25%，而且两个模型都同意协商重组是更可能的路径」。Kaur 没有给出另外三条路径收敛后的概率。收敛后的破产概率正好等于 Astra 6 最初的估计，也就是 Kaur 说自己偏向的那个模型。

## 7. 意见一致不等于判断正确

Kaur 得出两个结论。第一，在「不可验证的问题上，给它们和人一样的结构化、有组织的上下文……这些模型有没有预测优势？答案只能是没有」。用 Kaur 的话说，「实际上，连 agent 也是人，对吧？某种程度上是这样，我就不展开这个争议了。」第二，「一个结构化的推理流程，可以让两个模型……或者说不同的智能，收敛到对未来基本相同的看法上」。

「但那是正确的路径吗？我们不知道。」Kaur 说，进行中的局面是「最真实的检验」，因为「否则任何历史分析都会带来某种 look-ahead bias（前视偏差）」。让人感兴趣的正是收敛本身：「如果你能系统地建立一个流程，真的收敛到相同的概率结果上，那其实就是市场上的价格。」

![Learn more about Axim Intelligence](../../slides/10-13-1405-can-ai-understand-a-distressed-credit/08.jpg)
*结尾页：Axim 做什么、Kaur 的背景，以及创始人、团队和顾问工作过的公司 logo。*

结尾页介绍 Axim 在做「the intelligence layer for stressed & distressed credit」，并在「experience across founder, team & advisors」（创始人、团队和顾问的经历）下列出 Goldman Sachs、Morgan Stanley、Meta 和 Glean。Kaur 以「我就讲这些，谢谢」结束；没有 Q&A。

---

**链接：** [日程](https://ai.engineer/nyc/2026/schedule?session=adca6e26-1ddc-4f77-8e35-59bf1d1de81d) · [视频](https://www.youtube.com/watch?v=WOszaC1WWbQ) · [Axim Intelligence](https://aximintelligence.com) · [iHeartMedia 2024 年置换完成公告（SEC 8-K 附件）](https://www.sec.gov/Archives/edgar/data/1400891/000119312524283675/d854532dex991.htm) · [iHeartMedia 2026 年第二季度 10-Q（SEC）](https://www.sec.gov/Archives/edgar/data/0001400891/000162828026055252/ihrt-20260630.htm) · [Chava & Jarrow (2004)](https://ideas.repec.org/a/oup/revfin/v8y2004i4p537-569..html)

本文依据官方视频字幕、作者的现场录音、幻灯片照片和以上公开文件写成。session 简介承诺会讨论「analytical provenance」（分析溯源），这个词台上没有出现。录音结束时没有 Q&A。[精确笔记](../precise/10-13-1405-can-ai-understand-a-distressed-credit.md)（中文）保留了时间戳，并更详细地记录了 iHeartMedia 公开的资本结构信息。
