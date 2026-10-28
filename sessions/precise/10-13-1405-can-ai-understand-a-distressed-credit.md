# Can AI Understand a Distressed Credit：精确版

> Precise notes for this session, compiled from the official video captions where available, the author's room recording, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材整理完，iHeartMedia 的公开事实已对 SEC 文件和新闻核对。这场**没有 Q&A**（视频 14:05 讲者说「that's all I had. Thank you」后结束）。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| 视频 | 有。[WOszaC1WWbQ](https://www.youtube.com/watch?v=WOszaC1WWbQ)，14:14，2026-10-24 上传，标题「Can AI Understand a Distressed Credit? - Harneet Kaur, Axim Intelligence」。自动字幕是正文的主要文本 |
| slide | 文件夹 8 张，**7 张是本场 slide**（IMG_0442–0445、0447–0449；0447 和 0448 是同一张 DistressedBench）。**排除 1 张**：IMG_0446 是一张笔记本电脑屏幕和键盘的近照，不是 slide。**「两个模型收敛」那张结果 slide 没有照片**（讲到那里时讲者电脑断电，恢复后作者没拍） |
| 其他资料 | 讲者公司官网 [aximintelligence.com](https://aximintelligence.com)；iHeartMedia 的 SEC 文件和新闻稿 |

## 基本信息

- 时间地点：2026-10-13 2:05–2:23pm，Turing Room (Riverside Ballroom)，Breakout。视频正文约 14 分钟
- 讲者：Harneet Kaur，Axim Intelligence 的 Founder & CEO。讲者说（视频 00:22）：「we're building the intelligence layer for distressed credit investing」
- 背景：讲者说（视频 13:48）：「I spent 10 years in finance. Um, the last few as a distressed credit and special situations investor at a … distressed PE / hedge fund」。slide 最后一页：「Who we are: Founder & CEO – 10 years across equities, distressed & special situations investor」，下方「EXPERIENCE ACROSS FOUNDER, TEAM & ADVISORS」列了 Goldman Sachs、Morgan Stanley、Meta、Glean 的 logo（是团队和顾问的合计，不全是讲者本人）。大会讲者页：Morgan Stanley，NYU Stern 金融与经济学士，Wharton MBA
- 资料（[Axim 官网](https://aximintelligence.com)）：产品是 Axim Credit Analyst（private beta）；「analytical provenance」是官网上的卖点；DistressedBench 标为「Coming soon」；创始人「nearly a decade in finance」，其中五年 public equities、四年 distressed credit 和 special situations
- 大会简介里的「analytical provenance」这个词**演讲中没有出现**；DistressedBench 只出现在 slide 标题上，讲者口头没有说出这个名字

## 1. 什么是 distressed credit

**slide「01 / OVERVIEW — What is a distressed credit?」**：一条下滑曲线分三段，再分叉成三种结局：

| 阶段 | slide 说明 |
| --- | --- |
| 01 PERFORMING | Debt is serviced. |
| 02 DETERIORATION | Liquidity tightens; covenants are tested. |
| 03 DISTRESS | Unable to refinance debt or liquidity challenged. |

| 结局 | slide 说明 |
| --- | --- |
| 01 Negotiated Exchange | Out-of-court exchange of debt |
| 02 Liability Management Exercise | Up-tier or drop-down exercise |
| 03 Bankruptcy | Files for in-court chapter 11 |

- 讲者说（视频 00:55–01:28）：「credit is just a bond or loan … of a corporation. The bond of Coca-Cola is a credit」；stress 是指公司有潜在风险会导致「one, run out of money, two, not be able to pay back their debt」
- 讲者说（视频 01:30–01:58）：每年都有一部分信用市场处于 stress 或 distress，原因可以是 cyclical（建筑、房地产下行）、operational（餐饮企业的食品召回）、secular disruption（「if you're a software business today, and maybe seeing some AI disruption」）
- 讲者说（视频 02:07–02:28）：「What does distress mean? It's actually not straightforward. Bankruptcy is one outcome of distress」，还可以是 liability management exercise，或「out-of-court restructuring, negotiated exchange」

## 2. 两个旧世界：统计模型和人工分析

**slide「Distressed analysis has historically lived in 2 worlds.」**：

| | 01 / MACHINE LEARNING MODELS | 03 / CREDIT MANAGERS（slide 编号如此） |
| --- | --- | --- |
| 做什么 | Predict deterioration / default | Analyze credit investments |
| 内容 | Altman Z-Score；Chava–Jarrow；Modern ML | Underwriting；Watchlists；Investment committee |
| LIMITATION | Strong on statistical risk. Weak on context. | Rich context. Highly manual and hard to scale. |

- 讲者说（视频 02:37–02:52）：「Historically, the engineering question for distress was pretty binary」：预测违约或不违约；「Today, that's become a lot more complex」
- 讲者说（视频 02:52–03:31）：传统 ML 方法跟不上；10–20 年前很重要的打分，「the big one is the Altman Z-score」，对一个「changes, you know, sometimes monthly, quarterly, yearly」的局面显得太静态；市场也更不透明：「the rise of private credit and some of these other areas have caused, you know, a lack of pricing」
- 讲者说（视频 03:32–04:02）：另一边是定性分析：读财报、看 lender 那边的情况、读 credit docs，「This process is super manual, very hard to scale」
- 资料：Altman Z-score 出自 Edward Altman 1968 年发表在 *The Journal of Finance* 的论文；Chava–Jarrow 是 Sudheer Chava、Robert A. Jarrow,「Bankruptcy Prediction with Industry Effects」, *Review of Finance* 8(4): 537–569, 2004（[RePEc](https://ideas.repec.org/a/oup/revfin/v8y2004i4p537-569..html)）。Altman 论文的卷期没有单独核对

## 3. 问题：AI 能「理解」一个 distressed credit 吗

**slide「03 / THE QUESTION — Can AI understand a distressed credit?」**：中间是 iHeartMedia 的 logo 和「CREDIT SITUATION」，周围 10 个问题：

- What is the capital structure? / What is structurally senior?
- What changed post-LME? / How much liquidity is there?
- How does the ABL maturity affect the situation? / What would recoveries be?
- What is the next event? / What changed since last quarter?
- What are the key near-term catalysts? / How does today compare to 2018?

- 讲者说（视频 04:07–04:27）：创办 Axim 时想的问题是「how far can AI get at understanding a distressed … credit and the type of paths you can take. Um and can you systematize what is effectively a pretty complex process」
- 讲者说（视频 04:31–05:14）：投资人要回答的问题分三类：financial（liquidity、capital structure）、legal（「what's the structural priority of the debt」）、operational（收入明天会涨还是跌）
- 讲者说（视频 05:14–05:44）：市场上「if you're following Rogo or Harvey or some of these, you know, big AI companies」，做法是对这些问题逐个 benchmark（例：「what part of the debt is structurally senior?」），「call it the big finance bench」
- 讲者说（视频 05:47–06:09）：她的看法不同：「**the true understanding, right, of credit, and particularly distressed credit, is can you predict what happens next?** … have a probability distribution of what path this takes in the future」
- 我的理解：「the big finance bench」听起来是泛指，不确定是否指某个具体 benchmark（例如 FinanceBench），【待确认】

## 4. 案例：iHeartMedia，两个前沿模型给出相反的判断

**slide「04 / THE CHALLENGE — There's no "ground truth" for predicting stress or distress.」**，表头「**Path through December 31, 2029**」：

| 路径 | Astra 6 | Opus 5.5 | Ground Truth |
| --- | --- | --- | --- |
| Negotiated restructuring | **35%**（高亮） | 20% | ? |
| Refinancing | 30% | 15% | ? |
| Bankruptcy | 25% | **62%**（高亮） | ? |
| Aggressive non-pro-rata LME | 10% | 3% | ? |
| Total | 100% | 100% | ? |

- 讲者说（视频 06:14–06:47）：「I chose iHeartMedia, which is a company that filed for a bankruptcy in 2018 … it's been through the rigmarole. Um it also went and did … another stressed transaction back in 2024. So, this is a live situation … there's no ground truth」
- 讲者说（视频 06:49–07:29）：「let's throw it into the smartest, you know, models. I did Astra 6 on Max Intelligence um and Opus 5.5」；「they disagreed … given a structure[d] context of the data」；negotiated restructuring 是 Astra 的首选，bankruptcy 是 Opus 的首选
- 讲者说（视频 07:31–07:58）：「how do you say which is right or wrong? You can't cuz I don't know what the probabilities are」；「I have my best judgment on things. I I would say **I lean towards Astra 6**, but you know, we're all human」
- **模型名**：我的理解：「Astra 6」是 OpenAI 2026 年 9 月发布的 GPT-6 Astra（[Vercel AI Gateway 模型页](https://vercel.com/ai-gateway/models/gpt-6-astra)，以及当时的新闻报道，没有打开 OpenAI 英文原页核对）；「on Max Intelligence」可能指最高一档 reasoning effort（Artificial Analysis 上有「GPT-6 Astra (max)」这个变体）。讲者给的「structured context」具体是什么材料、两个模型是否用了同样的 prompt，没有说。【待确认】

### iHeartMedia 的公开事实（资料）

| 事件 | 日期 | 内容 | 来源 |
| --- | --- | --- | --- |
| Chapter 11 申请 | 2018-03-14 | iHeartMedia、iHeartCommunications 及部分境内子公司在 U.S. Bankruptcy Court for the Southern District of Texas（Houston Division）申请 Chapter 11；Clear Channel Outdoor 不在其中。公司自己的公告称要处理「more than $20 billion in debt」 | [iHeartMedia 10-K XBRL 附注（SEC）](https://sec.gov/Archives/edgar/data/1400891/000140089122000012/R8.htm)；新闻报道 |
| 走出破产 | 2019-05-01 | 重整计划生效，同时完成和 Clear Channel Outdoor 的分拆。行业报道称债务从约 161 亿美元降到约 57.5 亿美元（和「200 亿以上」的口径不同，没有核对差异原因） | 同上 SEC 附注；[RadioInsight](https://radioinsight.com/headlines/176503/iheartmedia-emerges-from-chapter-11-bankruptcy-protection/) |
| 2024 年债务交换（讲者说的「another stressed transaction back in 2024」） | 2024-12-23 宣布完成（交换要约 2024-12-18 截止） | 约 48 亿美元、即现有债务本金的 **92.2%** 参与交换；新工具包括 2029 年到期的 first lien term loans、9.125% first lien notes due 2029、7.750% due 2030、7.000% due 2031、10.875% second lien notes due 2030；「extended the vast majority of its debt maturities by three years」；总债务减少「over $440 million」 | [8-K 附件 99.1「iHeartMedia Completes Comprehensive Exchange Transactions」（SEC）](https://www.sec.gov/Archives/edgar/data/1400891/000119312524283675/d854532dex991.htm) |
| ABL（slide 问题之一） | 截至 2026-06-30 | 4.5 亿美元 senior secured asset-based revolving credit facility（2022-05-17 签订，2024-11-06 修订），债务表列为「due 2027」；borrowing base 4.329 亿美元，已提取 1.25 亿美元；现金约 1.744 亿美元，总可用流动性 4.572 亿美元；总债务约 50.43 亿美元。公司称「expects to be able to meet its obligations as they become due over the coming year」 | [2026 Q2 10-Q（SEC）](https://www.sec.gov/Archives/edgar/data/0001400891/000162828026055252/ihrt-20260630.htm) |
| ABL 展期 | 2026-08-07（据搜索摘要） | 搜索结果摘要称 Q2 10-Q 披露了一项 2026-08-07 的 Second Amendment，把 ABL 到期日从 2027-05-17 延到 2029-01-30，承诺额仍为 4.5 亿美元。**我抓取 10-Q 正文时没有看到这段文字**（只在 XBRL 标签里有 2026-08-07 的期后事项），【待确认】 | 同上 |

- 我的理解：slide 上「Path through December 31, 2029」的时间窗口，正好覆盖 2024 年交换后新 term loan 和部分 first lien notes 的 2029 年到期日；讲者没有解释为什么选这个日期
- 讲者没有在演讲中讲 iHeartMedia 的具体数字（债务规模、杠杆、到期日），slide 上也只有问题，没有答案

## 5. DistressedBench：把推理拆成四层，逐层检查

**slide「05 / EVALUATION FRAMEWORK — DistressedBench: eval framework for distressed credit analysis.」**：

> 01 Extraction → 02 Reconciliation / Derivation → 03 Judgment → 04 Prediction

- 讲者说（视频 08:01–08:22）：第二步是「how do I improve at least an assessment of their process … I can look at … whether I agree or disagree with the different assumptions」，于是做了「this sort of benchmark … we benchmark each layer」
- 讲者说（视频 08:27–09:05）：每一层 LLM 都可能出错。Extraction：「with public companies it's … pretty solved. I would say with private loans and … when you have like Excel documents and PDFs, that can … still be a challenge」。Derivation：「extraction would be you got EBITDA. … derivation would be, okay, now you have EBITDA and you have debt, now you can have leverage」。Judgment：「now you have the leverage. Is this a distressed credit or not?」
- （视频 09:10–10:15：讲者电脑断电，现场中断约 1 分钟，「I think my computer died」，恢复后继续）
- 讲者说（视频 10:22–11:05）：每一层都能检查，到 prediction 层时「you've assured the data points are good, you've assured that … the reconciliation layer is good, you've assured that there's some sort of reasonable judgment」；这也是人类分析师的思路：「**If you build a judgment on a bad data point, that's not going to be a good judgment**」
- 讲者没有说 DistressedBench 有多少题、谁标注、每层怎么打分。资料：官网标为「Coming soon」

## 6. 结果：结构化推理后两个模型收敛

（这张 slide 没有照片，以下来自视频）

- 讲者说（视频 11:15–12:02）：「once we actually created the structured reasoning process, … both the models actually converge in terms of what they believed the future probabilities of the business are going to be」；「now instead of saying **62% bankruptcy with Opus and 25% bankruptcy with Astra**, now both these models are saying there's actually only **a quarter or 25% chance of bankruptcy** and both models agreed that **the negotiated restructuring path is actually the more likely path**」
- 收敛后其他三条路径的概率、两个模型各自的完整分布，讲者没有说，【待确认】
- 我的理解（只是复述数字关系）：收敛后的 bankruptcy 概率 25% 和 Astra 6 原先的数字相同，也就是 Opus 5.5 向 Astra 6 靠拢；讲者自己此前说她倾向 Astra 6

## 7. 结论：一致不等于正确

- 讲者说（视频 12:07–12:38）：第一条：「given non-verifiable questions, right? With the same structured context, organized context as a human … do these models have a predictive edge? And you have to say **no**」，因为「in effect, even agents are human, right? In in some sense of the word and I won't get into the controversy there」
- 讲者说（视频 12:38–12:59）：第二条：「a structured reasoning process can get the two models … or different intelligences to converge on effectively a same view of the future」
- 讲者说（视频 13:00–13:11）：「but is that the correct path? We don't know, right?」；用 live situation 是「the truest measure」，因为「otherwise any sort of historical analysis would create some sort of **look ahead bias**」
- 讲者说（视频 13:13–13:34）：「the convergence is pretty interesting, too, right? Because if you can systematically build a process that actually converges on the same probabilistic outcomes. **That is effectively what is a price in a market.**」
- 视频简介的概括：「which raises a new question: is agreement the same as being right?」

## Q&A

无。视频和录音都在讲者介绍公司、展示二维码并致谢后结束（视频 14:05）。

## 勘误表（转写 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| Axon Intelligence / Axon / Axium | Axim Intelligence | slide、大会讲者页 |
| Harini（视频 13:44） | Harneet | 大会讲者页 |
| ultimate z-score（录音） | Altman Z-score | slide |
| Pro Go（字幕）/ robo（录音） | Rogo | 上下文 |
| harvey（录音） | Harvey | 上下文 |
| i heart media | iHeartMedia | slide |
| asterisk on max intelligence / astra（录音） | Astra 6 on Max Intelligence | slide（Astra 6）、视频字幕 |
| either job（录音） | EBITDA | 视频字幕 |
| distract credit（录音） | distressed credit | 视频字幕 |
| a lot of pricing（录音） | a lack of pricing | 视频字幕 |
| reading the credit card（录音） | read the credit docs | 视频字幕 |
| secular destruction（录音） | secular disruption | 视频字幕 |
| changes 10 times monthly（录音） | changes sometimes monthly | 视频字幕 |
| Daddy. That sucks.（录音 08:19 段） | 电脑断电时的现场话语，非正文 | 视频字幕 |
| distraught card / distraughts pe（录音） | distressed credit / distressed PE | 视频字幕 |
| the cure code（录音） | the QR code | slide |

## 出入（来源之间不一致的地方）

1. **大会简介和演讲内容**：简介说会分享「analytical provenance」和 DistressedBench；演讲口头没有出现 analytical provenance，DistressedBench 只是 slide 标题，没有讲题量、标注或打分方式
2. **「structured context」前后两次的区别**：第一次两个模型就已经拿到「given a structure[d] context of the data」（视频 07:14）；收敛发生在「structured reasoning process」之后。两次输入具体差在哪里（是只多了分层检查，还是人工修正了中间结果），讲者没有讲清楚，【待确认】
3. **收敛结果没有 slide**：「两个模型都给 25% bankruptcy、都认为 negotiated restructuring 最可能」只有口述，作者没有拍到对应 slide
4. **讲者背景年限**：讲者口述和 slide 是「10 years in finance」；官网是「nearly a decade」，其中五年 public equities、四年 distressed。两者不冲突，但 slide 写「10 years across equities, distressed & special situations」
5. **iHeart 2018 年债务口径**：公司公告称「more than $20 billion」，行业报道称重整前约 161 亿美元，口径没有核对

## 【待确认】汇总

1. 「Astra 6 on Max Intelligence」是否就是 GPT-6 Astra 的最高 reasoning effort（OpenAI 原页没有打开）
2. 给模型的「structured context」是什么，第一次和「structured reasoning」之后的输入差别
3. 收敛后完整的概率分布（除 bankruptcy 25% 以外）
4. 「the big finance bench」是否指某个具体 benchmark
5. iHeartMedia ABL 在 2026-08-07 展期到 2029-01-30（只见于搜索摘要）
