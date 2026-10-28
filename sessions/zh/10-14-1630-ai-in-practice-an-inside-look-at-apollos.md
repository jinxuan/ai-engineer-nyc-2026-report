[English](../10-14-1630-ai-in-practice-an-inside-look-at-apollos.md) | **中文**

# Apollo 旗下 CLO 业务：三天的组合搭建如今只要几分钟

*在 AI Engineer NYC 2026 上，Apollo 旗下 Redding Ridge 的 Jack McDonald 向 Rowspace 联合创始人 Michael Manapat 讲述：在公司自己核对过的数据上运行的 AI agent，怎样把 portfolio construction 从几天缩短到几分钟；以及信任为什么来自工程师每天和前台坐在一起。*

**Featured Keynote · 2026 年 10 月 14 日（周三）· 4:30–4:48pm · Simons Room (Metropolitan East)，Main Stage**
**讲者：** Michael Manapat，Co-Founder，Rowspace；Jack McDonald，Principal，Apollo Global Management，兼 Redding Ridge COO

---

这一场是对谈，不是演讲：Michael Manapat 提问，Jack McDonald 回答，没有放 slide。官方简介写的是「Rowspace customer co-presenting keynote」（Rowspace 客户联合登台的 keynote）。

Manapat 介绍 Rowspace 是「一个 agentic platform，用 AI 帮助 Apollo 这样的金融服务伙伴处理杂乱、分散的数据」。公司用 agent「做数据工程的活」，在许多系统之间核对数据，「让 portfolio manager、交易员等人更快、更准确地做决策」。Rowspace 在 2026 年 2 月公开亮相，融资 5000 万美元，由 Sequoia 和 Emergence Capital 领投；据 Fortune 报道，Manapat 曾在 Stripe 和 Notion 担任高级技术职务，平台在客户自己的云系统里处理数据。

McDonald 自我介绍是 Apollo 的 principal、Redding Ridge 的 COO，Redding Ridge 是「我们的 structured credit 和 CLO 管理人，captive to Apollo」，是「完全独立的实体、一家运营公司，管理约 500 亿美元资产」，主要是 CLO（collateralized loan obligation，担保贷款凭证）。McDonald 2020 年加入 Apollo，在 Redding Ridge 之前待过 financial institutions group 和 asset-backed group。（Redding Ridge 自己的招聘页写的是截至 2026 年 3 月 31 日管理资产约 439 亿美元。）

## 1. AI 什么时候变成 mission-critical

Manapat 用 Rowspace 在金融机构里普遍看到的一个落差来开场：「AI 作为锦上添花，和 AI 真正成为业务的 mission critical，之间有相当大的距离。」Manapat 说，在 Apollo，AI 正在真正改变公司的运作方式。

McDonald 讲了起点。Redding Ridge 管很多基金实体，每天从各项投资进来「成千上万个数据点」，管着 McDonald 所说的「thousand name loan pools」，还有跨多个业务、每天都要评估的信用敞口。「CLO 管理非常活跃，在 broadly syndicated loan 市场上交易」，所以信息必须随手可得。目标是让数据「准确、一致、格式能用」。

难点在于这些信息是私有的。和公开股票、公开固收不同，它们「非常分散，散落在各种文件里」。所以 Redding Ridge 请 Rowspace「进来，真正和我们的团队坐在一起」，学会实际的计算方法，弄清「什么才是真正的 golden source of truth」，再不断改进 AI，让它理解并复现这些工作。

## 2. Portfolio construction：从几天到几分钟

Manapat 请 McDonald 举一个具体例子，说说过去要一个团队忙一周甚至更久的「fire drill」。McDonald 举了两个。

第一个是 portfolio construction。CLO「非常看重各项测试」「非常 rules based」，有一套具体的 waterfall 决定钱往哪流，这些都写在「很深的 indenture 文件和好几份不同的法律协议」里。PM 搭一个 mock portfolio，要读这些文件、建 Excel 模型、凑齐「200 到 400 个单一信用名字」。用 Rowspace 之前，这要「在手工 Excel 里花两到三天」。现在系统从文档库、交易和合规系统、实时报价里拉取实时信息，「原本要花几天的组合」只要「几分钟」。

工具不会把活全干完。McDonald 说，它把组合做到「80%、90% 完成」，PM 再用自己的判断打磨「最后那 10%，让它成为自己的组合」。

## 3. 每周的 trade reconciliation

第二个例子是 warehouse 采购的 trade reconciliation。Redding Ridge 用银行融资，随时可以在固收市场买入贷款。以前每周有个团队核对本周买了什么、卖了什么：「真的是进到银行账户里看对账单、看交易到底在哪结算」，然后给管理层发一封汇总邮件。这要「两三个人每周花好几个小时」。

现在同一份报告在 Rowspace 里重建成了可视化。它「一分钟内就能重新生成」，全部自动化；团队的工作是每天做 QA。McDonald 说结果是「一个更像例行公事的流程」。

| 工作流 | 以前 | 现在 |
| --- | --- | --- |
| 200–400 个信用名字的 mock portfolio | 手工 Excel 2–3 天 | 几分钟做到 80–90%；PM 完成最后 10% |
| 每周 trade reconciliation | 2–3 人每周好几个小时 | 约一分钟重新生成报告；每天 QA |

## 4. 为什么不直接用 Claude 或 ChatGPT？

Manapat 指出，Redding Ridge 团队做的是交易和组合管理决策，对准确性的要求远高于读一段 chatbot 回答。Manapat 把两个问题放在一起问：「为什么不能直接把这些东西接进 Claude 或 ChatGPT 看看效果？」以及，一个团队要怎样才能信任一个 AI 产品到可以「下单交易、搭建组合」的程度？

McDonald 说，通用模型「非常有用，但用的都是通用信息……没有你自己的实际数据」。它们适合做研究、找新想法、快速了解一个产品或一项投资。Redding Ridge 做 AI 的思路是「拥有自己的数据，在上面搭一个我们确知为真的平台和技术」。

McDonald 说，信任来自 Rowspace 的工作方式，这和其他科技供应商不同：「他们是 boots on the ground。Michael 每天都和我们的团队坐在一起。」原因在于「一个数据可能来自 20 个不同的来源，而只有一个是对的」。Rowspace 团队和 PM、财务人员一起弄清计算方法、哪个数据正确、哪些来源应该被覆盖。因为结果经过 Redding Ridge 自己的人验证，数据又来自 trade order management system、performance system、Bloomberg 和各种 pricing index，团队可以依赖它。McDonald 说，公司里其他地方的系统仍然很分散，还要手工搬数据，「而 Rowspace 现在把这些全都替我们自动化了」。

## 5. Apollo 为什么早早就「AI-pilled」

Manapat 说，「甚至在去年年初，Claude Code 还没火起来、Cursor 还停留在 tab completion 时代的时候」，Apollo 就是 Rowspace 接触过的最「AI-pilled」的机构之一，既体现在它对 AI 时代如何投资的看法上，也体现在运营上：它很早就说「必须用 AI native 的方式重建公司，必须要平台，不要单点方案」。这是从哪来的？

McDonald 归结为文化。Apollo 以作为「另类资产管理领域的领导者」自居，以「非常创新」为傲。管理层强调提升团队技能、保持灵活。「我们意识到 AI 时代已经来了，我们需要适应它、采用它，而不是……否定它，」McDonald 说，把新技术带进工作流，让人腾出精力去思考，替代手工流程。McDonald 形容 Apollo 是「一家非常创新、非常扁平、文化开放的公司」。接着说：「你问题的第二部分我想不起来了，抱歉。」Manapat 便继续往下问。

## 6. 金融业还有多早？

Manapat 说，软件工程作为一门学科已经彻底变了，法律和客服可能也快了。金融处在什么位置：是「AI 已经来了、已经带来改变」，还是连「第一局的下半局」都还没到？

「我仍然认为还非常早，」McDonald 说，至少在资产管理、投行和机构金融领域是这样。大银行有庞大的中后台，提升空间很大；Apollo 自己也「极其复杂」。现在的价值在建模型、快速了解一家公司、汇总研究，但「至少就我们今天的位置看，这些附加值较低，而且会演变」，某些职能将来可能「需要的初级支持会变少」。Redding Ridge 目前的重点是让例行任务更高效，这能腾出精力，「因为我们要做、要改进的事几乎无穷无尽」。Manapat 问题的后半部分，即改变金融需要什么，没有得到单独的回答。

## 7. 商品，还是竞争优势？

Manapat 摆出两种可能。一种是 AI 加快任务、省下运营开支，但不给任何公司带来优势，就像没有哪家公司因为用 Excel 而领先。另一种是 AI「实质性地带来竞争优势，或者帮助巩固 Apollo 在结构设计和创新交易上的领先」。

「如果适应得好、用得对，它会让你更有竞争力，」McDonald 说。对活跃交易市场里的公司，成败取决于流程和信息：知道可用的流动性、价格在哪、任一时刻值多少。这在公开市场已经存在；「在私募市场，它还在演变」，Apollo 一直在推动这件事。长期看，拥有更好的、团队确知准确的信息的公司会领先，因为「我们最不想做的就是……基于错误信息做决定」，错误会一路传到公司和投资人那里。

## 8. 接下来要实现什么

Manapat 说，问 Apollo 五年后的样子不公平：「我连自己团队下周用 Codex 还是 Claude Code 都预测不了。」所以 Manapat 改问 McDonald 个人希望看到什么。

McDonald 说了两点。第一，对每一条发给投资人的信息都有十足把握，不用再花几小时手工核对；Redding Ridge 有「400 多家全球投资人」。第二，更「tech enabled 的思维方式」。团队不缺这个，但技术一直是「事后才想到的东西」。例行工作自动化之后，人们可以去问「这个流程为什么存在、今天怎么运作、怎样才能真正改进」，以及要不要引入新技术。McDonald 说，理想状态是这成为 Redding Ridge 员工处理问题时的第一反应。

## 9. 给做金融 AI 的人的建议

Manapat 的最后一个问题来自 Manapat 和 Redding Ridge 合作的体会：这些工作「复杂和有创造性得难以想象」。做金融 AI 的人应该记住什么？

「我们是极其复杂的组织，」McDonald 说。看起来「新奇、奇怪、格格不入」的东西，「很可能确实有它存在的理由」。所以「花时间、真正下到一线，和你要为之做工具的团队或前台专业人员坐在一起……这些时间花得值」。McDonald 认为 Rowspace 的产品「领先于我们的路线图和预期」，靠的正是这一点：「Michael 和团队几乎每天都花大量时间和我们的团队坐在一起。」

Manapat 最后打了个 Manapat 自称「不害臊的广告」：Rowspace 和 Apollo 的技术团队都在招想把 AI 用到金融上的人。

---

**链接：** [日程](https://ai.engineer/nyc/2026/schedule?session=44e9d04a-e8b4-418c-8a85-02fbb9263dd0) · [视频](https://www.youtube.com/watch?v=BOWYujhIQuo)（「AI Agents Cut 3-Day Portfolio Builds to Minutes」）· [Day 2 直播 7:50:03 处](https://www.youtube.com/watch?v=jiUC6IUMdvc&t=28203s) · [Fortune 关于 Rowspace 亮相的报道](https://fortune.com/2026/02/25/rowspace-sequoia-ai-financial-platform-private-equity-investment-venture-capital/) · [Redding Ridge 招聘页](https://job-boards.greenhouse.io/reddingridge/jobs/4994805008)

本文依据官方视频字幕，并和 Day 2 直播字幕对照过。台上没有放 slide，作者也没有拍照。对谈结束时没有观众提问。[精确版笔记](../precise/10-14-1630-ai-in-practice-an-inside-look-at-apollos.md)（中文）保留了时间戳和字幕勘误。
