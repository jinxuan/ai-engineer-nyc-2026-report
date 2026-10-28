[English](../10-13-1220-1368-ai-failures-what-they-teach-us-abou.md) | **中文**

# 按风险评估整个 AI 系统，而不是按出错频率

*在 AI Engineer NYC 2026 上，Vishal Srivastava 和 Tanmay Sah 把 1,368 起公开报道的 AI 事故映射到 13 个失败维度上，提出的观点是：决定一次失败会不会变成实际伤害的，多数时候不是模型本身，而是模型周围的控制措施。*

**Breakout（Evaluations Track）· 2026 年 10 月 13 日（周二）· 12:20–12:38pm · Turing Room (Riverside Ballroom)**
**讲者：** Vishal Srivastava, PhD，New York Life Insurance 企业副总裁兼首席数据科学家；Tanmay Sah，AI 研究员

---

Vishal Srivastava 把这项研究说成一种习惯：「我的研究很大一部分是看 AI failures，调查它们，然后从中学习。」Tanmay Sah 介绍自己是「senior quantitative modeler，也是 AI 研究员」。

演讲从一个公开数据集讲起。「我们看的是 AI incident database，一个公开的 AI 事故数据库，」Srivastava 说，「我们看了这些 AI failures，然后问了一个简单的问题：这些失败告诉我们，AI 系统……应该怎么评估。」讲者没有说出数据库的名字，也没说快照日期。（公开的 AI Incident Database 编号在 2026 年初到了第 1,368 号，和标题对得上，但演讲里没提。）

Srivastava 说，AI 失败「一直在指数级增长」，而过去四年，也就是 foundation model 时代，「几乎是垂直往上跳」。（开场那张图没有照片，所以这里不复述它的数字。）在公司内部，AI use case 的数量「远超过我们逐个 review 的能力」，所以需要「一种结构化的方式来调查这些 AI failures」。

## 1. 一个框架：3 个问题、13 个维度

![Full AI System Evaluation Across 13 Dimensions](../../slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/03.jpg)
*框架画成一个倒三角：六个技术维度、四个部署控制、三个业务影响。*

框架那页标题是「Full AI System Evaluation Across 13 Dimensions」，画了一个倒三角，三层分别是 6、4、3 个维度。左边一条竖栏写着它适用于传统 ML、生成式 AI、RAG 和 agentic AI。每一层问一个问题，13 个子维度各自再问一个：

| 支柱 | 问题 | 子维度 | 幻灯片上的问题 |
| --- | --- | --- | --- |
| **Technical Failure**（6） | 系统会在哪里出问题？ | Data Foundations | 输入数据的质量和资源是否合理？ |
| | | System Design | 系统设计是否适合这个任务？ |
| | | Predictive Validity | 系统表现是否和宣称的一致？ |
| | | Fairness | 结果和错误在不同群体之间是否成比例？ |
| | | Failure Modes | 宣称的能力在哪里、何时、怎样、对谁失效？ |
| | | Explainability | 行为和决策能否得到合理解释？ |
| **Deployment Controls**（4） | 这个失败能不能传到现实世界？ | AI Authority | AI 被允许决定、执行还是只提供信息？ |
| | | Human Mediation | 人能不能有效介入？ |
| | | Exposure Control | 谁在用这个 AI 系统，内部还是外部？ |
| | | Harm Detection | 正在出现的伤害能否及时被发现？ |
| **Business Impact**（3） | AI 出错时影响有多大？ | Financial | 可能带来什么财务后果？ |
| | | Legal | 可能带来什么法律或监管后果？ |
| | | Scale | 影响范围有多大？ |

Srivastava 按生命周期把这几层讲了一遍。开发阶段犯的错，无论在数据质量、工具架构还是系统设计上，都会流进部署阶段。到了部署阶段，「即使模型或系统有设计缺陷，只要部署时有足够的控制，这些错误也能被拦住。」剩下的部分落到业务场景里，变成声誉、法律或财务风险。「单看模型，或者模型的技术能力，只是系统的一部分。我们得把系统当成整体来看。」

部署层有两个维度讲得多一些。AI Authority 问的是：「这个 AI 是给我们提供信息的，还是要代表我们去行动？」Exposure Control 区分内部员工和「外部客户」：内部员工能看到错误并把它控制住，而面对外部客户，「任何小问题都可能传得很快」。Srivastava 的总结是「一个框架，13 个维度，三个核心问题」。（前面还有两张照片，是同一页逐层出现的过程。）

## 2. 几乎所有失败都能归进去，而且大多同时落在好几个维度

![AI Failures Map to the Same 13 Dimensions](../../slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/04.jpg)
*1,368 起事故里有 97.7% 至少能映射到 13 个维度中的一个。*

下一页拿事故来检验框架。一张环形图显示 **1,368 起事故中有 97.7%** 至少能映射到 13 个维度之一，配的一句话是「AI failures have structure. We can map it.」（AI 失败是有结构的，我们能把它画出来。）Srivastava 在台上约成「将近 98%」，并补充说这些失败和这些维度相关，「有些情况下同时和好几个维度相关」。按这个数字算，大约有 31 起事故一个维度都对不上。

![AI Failures Occur Across Multiple Dimensions Simultaneously](../../slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/05.jpg)
*每起事故涉及 13 个维度中的几个；中位数是 6。*

一张直方图统计了每起事故跨了几个维度，从 0 到 13。最高的柱子在 7（约 145 起），一条虚线标出中位数 6，图注写着「The typical AI failure spans nearly 6 dimensions」。Srivastava 说每起事故「平均有六个失败子维度」；幻灯片给的是中位数，不是平均数。session 简介里说过「92% 的事故同时牵涉多个弱点」，这个数字台上没讲，不过直方图上 0 和 1 两根柱子加起来大约占 8%，和它一致。

## 3. 最常出错的地方，不一定是最要紧的地方

![What Fails Most Often Isn't Necessarily What Matters Most](../../slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/06.jpg)
*每个点是一个维度：横轴是它有多常见（对数坐标），纵轴是它和伤害的关联有多强。*

这张散点图是整场演讲的核心。每个点是 13 个维度中的一个，基于 1,368 起事故计算，其中 206 起造成了实质伤害。横轴是这个维度被评为 severe 的频率，用对数坐标；纵轴是 lift，即发生伤害时这个维度出现得多出几倍，从 1× 到 5×。幻灯片上标注了两个点：

| 维度 | 频率（severe 评级） | 对伤害的 lift |
| --- | --- | --- |
| Human Mediation | 罕见：0.95% 的事故 | 4.8× |
| System Design | 最常见：38% 的事故 | 约 1× |

其他点没有标注。目测的话，Harm Detection 约 2.7×，Exposure Control 约 2.3×，Fairness、Legal 和 Predictive Validity 在 1.8–1.9× 左右，AI Authority 约 1.5×，Financial、Scale、Data Foundations 和第五个技术维度接近或低于 1×。（这张图把那个维度叫「Failure Analysis」，其他页叫「Failure Modes」。）

Srivastava 讲了左上角：human mediation，即「human in the loop 的有效性」，以及 harm detection，即「我们有没有监控措施」。Scale 和 system design「在很多案例里都出现，但和伤害的关联没那么强」。结论在幻灯片上写作「Prioritize by potential harm, not frequency」（按潜在伤害排优先级，而不是按频率）：「我们需要按造成伤害的可能性给这些子维度排优先级，然后分配相应的资源……而不是把所有失败子维度一视同仁。」

![Deployment Controls Shape Whether Failure Reaches Harm](../../slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/07.jpg)
*每个支柱在有伤害的事故里相对其余事故的 lift。*

在三个支柱的层面做同样的比较时，实质伤害定义为「死亡、法律诉讼或歧视」，1,368 起里有 206 起。柱状图显示每个支柱在有伤害的事故里比在其余事故里多出现几倍，1× 表示一样常见：

| 支柱 | 在有伤害事故中的 lift |
| --- | --- |
| Technical Failure | 1.10× |
| **Deployment Controls** | **2.43×** |
| Business Impact | 1.46× |

「部署时的控制措施和伤害的关联最关键，其次是业务影响，然后才是技术失败，」Srivastava 说，「这些子维度都重要，但 deployment controls 要重要得多。」

讲者在 DEV 上发的配套文章给的是另一组指标，比较对象是致死结果而不是实质伤害：监控和检测严重失败的 lift 是 4.7×，AI 决策权限高的 lift 是 2.8×。文章还写明这些是「已报道事故内部的关联，不是因果估计」，会受报道偏差影响；台上没有重复这条说明。

## 4. 重要性决定评估要做多深

![Materiality Determines Evaluation Depth](../../slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/08.jpg)
*先设计好一个 materiality 评分，再回头用事故数据检验。*

如果伤害比频率更重要，组织就需要知道哪些 use case 可能造成伤害。这页分两半。设计那一半描述了一组简短的启发式规则，「没有拿伤害数据去拟合」：六个技术输入、四个部署控制输入、三个业务影响输入，相乘组合，每个输入按书面标准打 1–3 分，「拿不准时打高分」。分档是 Low（≤4）、Medium（≤8）、High（≤15）和 Critical（>15）。照片左边被截掉了，所以看不到每个支柱内部的输入在相乘之前具体怎么合并。

检验那一半把这个评分回头套到 1,368 起事故上：

| Materiality 档位 | 实质伤害率 | 事故数 |
| --- | --- | --- |
| Low | 9.1% | 36 of 396 |
| Medium | 15.9% | 87 of 548 |
| High | 17.7% | 66 of 373 |
| Critical | 33.3% | 17 of 51 |

各档加起来正好是 1,368 起事故、206 起有伤害，和前几页一致。伤害率逐档上升，Critical 虽然只有 51 起，伤害率却是 Low 的三倍多。「Materiality 和这些 AI use case 的伤害直接成排序关系，」Srivastava 说，所以团队可以「把更多资源分配给 materiality 高得多的 AI use case」。这页的标语是：「Materiality sets how deep we evaluate.」（重要性决定评估做多深。）

## 5. 把风险拆成四个概率的乘积

![Decomposing AI System Level Risk](../../slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/09.jpg)
*预期伤害是四个因子的乘积，其中三个取决于模型周围的部署方式。*

Tanmay Sah 接着讲了一个公式。部署方式 A 下的预期损失是四个因子的乘积：

| 因子 | 含义 | 由什么决定 |
| --- | --- | --- |
| P(fail) | 模型出错 | 模型质量、输入 |
| P(reach \| fail) | 错误传到了真实决策 | 权限、暴露面 |
| P(miss \| reach) | 没人及时发现 | 人工审核、监控 |
| Severity | 落地后有多严重 | 财务、法律、规模 |

这里的 A 是「模型周围的部署方式：authority · exposure · human review · monitoring」。幻灯片注明改编自讲者自己的预印本 arXiv:2602.18986；那篇论文的摘要用的是三个因子（失败、在给定自动化程度下失败变成伤害、严重程度），案例是 2012 年 Knight Capital 的损失，幻灯片把中间那个因子拆成了两个。

Sah 把四个因子念作「模型多常出错、这个错误会不会传到真实决策、会不会没人发现、后果有多严重」。只有第一个属于模型本身。「同一个模型，部署方式不同，风险也不同，」Sah 说；幻灯片上写的是「Same model, different A, different risk.」（同一个模型，不同的 A，不同的风险。）所以：「如果风险取决于整个部署系统，我们的评估也得覆盖这套部署。」

## 6. 在 agentic 系统里，评估者本身也需要被治理

Sah 接着对比了测什么和怎么测。（这两页没有照片。）传统的 model risk 工作已经会通过检验假设、输入数据和压力下的行为来「challenge」模型。把这套做法扩展到整个部署好的系统，就要问：「retrieval 有没有找回正确的信息？工具出错时会怎样？人工审核能不能抓到错误？agent 有没有守在自己的权限或边界内？系统最终能不能恢复？」用 Sah 的话说，「逻辑不变，只是测试的边界变大了。」

至于怎么测，传统 ML 在 held-out 数据上算 accuracy 或 AUC-ROC，把它当作部署决策的依据，不过「我们仍然需要检查这些指标背后的数据和假设」。agentic 系统的输入、工具响应和动作序列（「也叫 trajectory 或 rollout」）组合太多，覆盖不全，只能测选出来的场景，而且常常由另一个 agent 或 AI judge 来生成测试或打分，「但这个 judge 也会犯错，测试也可能漏掉重要的失败」。所以团队必须检查「评估者是否可靠、证据覆盖了什么、它是否适用于我们实际部署的系统」。Sah 的结论是：「这就是为什么 evaluator 是系统里需要我们去治理的那一部分。」

## 7. 好的 eval 要去证伪：2-4-6 任务

![Why good evals need to seek failure not confirmation](../../slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/10.jpg)
*Peter Wason 1960 年的规则发现任务，用来论证 eval 应该以找失败为导向。*

Srivastava 回来讲了一个经典实验，来自 1960 年 UCL 的 Peter Wason，Srivastava 称 Wason 是「提出 confirmation bias 这个词的人」。参与者看到 2–4–6，要猜出规则。最可能的假设是「递增的偶数」，用 8–10–12 和 20–22–24 去试都能印证：「more confirmation → more confidence」（印证越多，越有信心）。实际规则是任意三个递增的数，试一下 1–2–3 或 3–8–100 就能发现。按幻灯片的说法，79% 的参与者「只测试了符合自己猜想的三元组」。（Srivastava 在台上说的是「79% 的人测错了」，这是另一个指标；这个 79% 对应 Wason 论文里的哪个数字，没有核实。）

和 AI 的联系是：「我们知道，AI 系统是用人类产生的数据训练的」，而评估「必须去证伪，而不是去印证」，所以 eval 系统「需要以失败为导向」。这页的标语是：「Good evals try to falsify, not confirm.」（好的 eval 去证伪，而不是去印证。）

## 8. 构建、打破、修复、再打破

![How should we operationalize falsification?](../../slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/11.jpg)
*围绕「System Evals」的四步循环。*

下一页的循环有四步：

| 步骤 | 含义 |
| --- | --- |
| Build | 定义系统的宣称能力和部署边界。 |
| Break | 在失败区域、控制、权限和工作流上寻找反证。 |
| Fix | 在正确的层面干预：模型、工作流、范围、权限或控制。 |
| Break again | 验证修复，并寻找新的失败模式。 |

Srivastava 说，从系统宣称能做什么出发，想办法打破它，找到宣称在哪里失效。修复有两种形式：「不让 AI 系统在……有失败的区域里运行」，或者加上团队有把握的「支撑系统」。反复迭代，直到达到所需的信心。「这基本上就是 first principles 的做法。」产出是一张地图，标出可信、「不确定」和「高风险」的区域，再「根据可靠性和 materiality」分流；对高风险系统，「确保你的 AI 只用在……你有把握的区域」。

## 9. 一个案例：航空公司的 chatbot

Sah 用一个案例收尾方法部分（没有幻灯片照片）。一位家人去世的乘客问航空公司的 chatbot 能不能买优惠票。chatbot 说先买普通票，「90 天内再申请折扣」。乘客照做了；航空公司以自己的政策为由拒绝，而「仲裁庭判航空公司担责」。Sah 没有说出航空公司的名字；事实和 *Moffatt v. Air Canada*（2024 BCCRT 149）吻合，这个案子由 British Columbia 的 Civil Resolution Tribunal 在 2024 年 2 月判决。

失败链条是：「错误的建议传到客户那里，客户照着做了，组织承担后果。」用 build–break–fix 的做法，会先定下要求「建议必须和批准的政策一致」，用「旅行之后的退款问题」去挑战 chatbot，在出错的地方加上「政策检查或转人工」，然后再测，看这些控制能不能在错误到达客户之前拦住它。「定义宣称，主动去挑战它，用证据指导改动并做出部署决策。」

## 10. 要点

Srivastava 做了总结（结尾那页没拍到）。eval「应该基于风险……而不是基于频率」。把系统拆成技术失败、部署控制和业务场景；用 materiality 量表给 use case 打分（「可以是你们公司自己的量表」）；在系统层面做压力测试；并且「只有在你对这个区域有把握之后，才做部署决策」。最后一句是：「Evaluate the full AI system based on risk, not just one component.」（按风险评估整个 AI 系统，而不只是其中一个组件。）讲者表示「之后可以回答问题」；这场没有 Q&A。

---

**链接：** [日程](https://ai.engineer/nyc/2026/schedule?session=9d72d7ac-a2a2-42cf-b880-5859316d840c) · [视频](https://www.youtube.com/watch?v=mMw3arwQsL0) · [What 1,368 Real-World AI Incidents Teach Us About Evaluation](https://dev.to/dailycontext/what-1368-real-world-ai-incidents-teach-us-about-evaluation-71p)（DEV / Daily Context）· [Srivastava & Sah, arXiv:2602.18986](https://arxiv.org/abs/2602.18986) · [AI Incident Database](https://incidentdatabase.ai/) · [CTV News 关于 Moffatt v. Air Canada 的报道](https://www.ctvnews.ca/vancouver/article/air-canadas-chatbot-gave-a-bc-man-the-wrong-information-now-the-airline-has-to-pay-for-the-mistake/)

本文依据官方视频字幕、作者的现场录音、幻灯片照片和以上来源写成。录音结束时没有 Q&A。[精确笔记](../precise/10-13-1220-1368-ai-failures-what-they-teach-us-abou.md)（中文）保留了时间戳，并完整记录了演讲、幻灯片和文章之间的出入。
