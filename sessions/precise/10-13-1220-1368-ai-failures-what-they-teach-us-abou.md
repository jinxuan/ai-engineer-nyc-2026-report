# 1,368 AI Failures：精确版

> Precise notes for this session, compiled from the official video captions where available, the author's room recording, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材整理完，数字已对 slide 和一手资料核对。这场**没有 Q&A**（讲者在视频 19:51 说「We'd be happy to take any questions later」，现场没有开放提问）。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| 视频 | 有。[mMw3arwQsL0](https://www.youtube.com/watch?v=mMw3arwQsL0)，20:02，2026-10-22 上传，标题「1,368 AI Failures and What They Teach About Evals — Vishal Srivastava, New York Life & Tanmay Sah」。自动字幕是正文的主要文本 |
| slide | 文件夹 13 张，**11 张是本场 slide**（IMG_0419–0430，其中 0419/0420/0422 是同一张「13 Dimensions」逐步展开）。**排除 2 张**：IMG_0431 是一张食物照片，IMG_0433 是一张与本场无关的手机截图。开头「AI failures 增长」那张、Tanmay 的「What should we test / How should we test」两张、航空案例和最后一张总结 slide 没有照片 |
| 讲者文章 | [What 1,368 Real-World AI Incidents Teach Us About Evaluation](https://dev.to/dailycontext/what-1368-real-world-ai-incidents-teach-us-about-evaluation-71p)（DEV / Daily Context）。和演讲重叠在 13 维的三大块，但给的是另一组数字（见「出入」） |
| 其他一手资料 | arXiv:2602.18986（slide 上标注的出处，见第 5 节）；arXiv:2606.21015（两位讲者的另一篇论文）；AI Incident Database 第 1368 号事件页 |

## 基本信息

- 时间地点：2026-10-13 12:20–12:38pm，Turing Room (Riverside Ballroom)，Breakout，Evaluations Track。视频正文约 19 分半
- 讲者：
  - Vishal Srivastava，PhD，New York Life Insurance 的 Corporate VP, Lead Data Scientist（大会讲者页）。视频 00:12：「A big part of my research is looking into AI failures, investigating it, and then learning from it」
  - Tanmay Sah，大会页写 AI Researcher。视频 00:21 自我介绍为「a senior quantitative modeler and an AI researcher. This is my second time here」（字幕把姓写成 "Shah"）
- 题目：大会日程是「1,368 AI Failures: What They Teach Us About System Evals」；视频标题是「1,368 AI Failures and What They Teach About Evals」
- 大会简介提到的几样东西**演讲里没有出现**：R-BED（Risk-Based Evaluability Design）这个名字、forthcoming book「Evaluable AI: From Model Evals to System Evals」、「reproducible evaluation pipeline applied to benchmark agent trajectories」的演示、「insufficient evidence」作为评估结果。演讲里的航空案例是讲故事，不是 pipeline 演示

## 1. 数据来源：一个公开的 AI 事故库

- 讲者说（视频 00:34）：「we looked at AI incident database. It's a publicly available AI incident database, and we looked at the AI failures and asked one simple question, what these failures teach us about how … the AI system should be evaluated」
- 讲者说（视频 01:02）：AI failures「have been increasing exponentially」，最近四年（foundation model 时代）「the jump is almost vertical」；企业里的 AI use case 数量「much larger than our capacity to review them each one by one」，所以需要「a structured way to investigate these AI failures」。（这张 slide 没有照片，图上的具体数字【待确认】）
- **哪个事故库**：讲者没说名字。我的理解：是 [AI Incident Database](https://incidentdatabase.ai/)（AIID，Responsible AI Collaborative 维护）。依据：资料 [AIID 第 1368 号事件](https://incidentdatabase.ai/cite/1368/) 是「Malicious OpenClaw Skills Reportedly Delivered AMOS Stealer…」，事件日期 2026-02-01，说明这个库的编号在 2026 年初正好走到 1368，和「1,368 incidents」吻合，像是取了 1–1368 号的快照。但演讲、slide、DEV 文章都没写库名和快照日期，【待确认】
- 资料（arXiv:2604.21412，另一组作者）提到 AIID 在其写作时有 1,460 起事件，可见 1,368 是某个时点的快照，不是现在的总数

## 2. 框架：3 大维度、13 个子维度

**slide「Full AI System Evaluation Across 13 Dimensions」（副标题 SYSTEM-LEVEL AI EVALUATION），倒三角 6 / 4 / 3：**

| 大维度（slide 原文） | 问题 | 子维度 | slide 上每个子维度的问题 |
| --- | --- | --- | --- |
| **Technical Failure**，6 dimensions | Where can the system break? ↓ reaches the real world | Data Foundations | Are the input data quality and resources reasonable? |
| | | System Design | Is the system designed appropriately for the task? |
| | | Predictive Validity | Does the system perform as claimed? |
| | | Fairness | How are outcomes and errors proportional to different gr[oups]? |
| | | Failure Modes | Where, when, how, and for whom does the claim break? |
| | | Explainability | Can behaviour and decisions be explained reasonably? |
| **Deployment Controls**，4 dimensions | Can that failure reach the real world? ↓ if harm occurs | AI Authority | What is the AI allowed to decide or act or inform? |
| | | Human Mediation | Can a human meaningfully intervene? |
| | | Exposure Control | Who is using the AI System, Internal versus External? |
| | | Harm Detection | Can emerging harm be recognised in time? |
| **Business Impact**，3 dimensions | How much does it matter when AI fails? | Financial | What financial consequence could result? |
| | | Legal | What legal or regulatory consequence could result? |
| | | Scale | How large is the impact? |

- slide 左侧竖条写着适用范围：「TRADITIONAL ML · GENAI · RAG · AGENTIC AI」
- 讲者说（视频 01:43–03:16）：三个维度对应生命周期：development stage（data quality、tool architecture、system design 出错）→ 错误传到 deployment；「even if the model or the system has some faulty design issues, but if you have sufficient controls in deployment, those errors can be contained」→ 最后是 business context（reputational、legal、financial risk）。「the model alone or the technical strength of the model is just one part of the system. We have to look at the system holistically.」
- 讲者说（视频 05:00–05:50，录音 00:53–01:47）：AI Authority 是「Is the AI for informing us or it is out there to act on our behalf」；Exposure Control 是内部员工（能看到错误并控制）还是「out there in the wild external customers so that any small issue can really propagate fast」
- 讲者说（视频 06:15）：「we look at this as one framework which has 13 dimensions and three core questions」

## 3. 覆盖率和「多维同时失效」

**slide「AI Failures Map to the Same 13 Dimensions」**：圆环图「**97.7% of 1,368 incidents** map to at least one of the 13 dimensions」，右边列出 13 个子维度编号 1–13，底部标语「AI failures have structure. We can map it.」

- 讲者说（视频 06:27，录音 02:20）：「using these 13 sub-dimensions, almost 98% of AI failures can be covered. Meaning that AI failures are … associated with some of these dimensions, in some cases multiple of these dimensions」；「AI failures have structures. We need to map it」
- 视频简介写「13 sub-areas that cover about 98% of incidents」。**以 slide 的 97.7% 为准**；98% 是口头取整
- 我的理解：97.7% × 1,368 ≈ 1,337，即约 31 起事件没有映射到任何子维度，和下一张直方图上「0」那一栏的高度（约 30）一致

**slide「AI Failures Occur Across Multiple Dimensions Simultaneously」**：直方图，x 轴「Failure dimensions spanned per incident」0–13，y 轴到 150，虚线标「**median 6**」，最高一栏是 7（约 145）。底部标语「The typical AI failure spans nearly 6 dimensions.」

- 讲者说（视频 07:20，录音 03:12）：「on average, there are six failure sub-dimensions that can be present in each of these AI incident failures」。**slide 写的是 median 6，口头说 average**，见「出入」
- 资料（大会简介）：「92% of incidents implicated multiple weaknesses simultaneously」。我的理解：从直方图读，0 和 1 两栏合计约 30 + 80 = 110 起，约占 8%，和「92% 涉及 ≥2 个维度」大致对得上（柱高是目测读数）

## 4. 哪些维度和伤害最相关：按伤害排序，不按频率

**slide「What Fails Most Often Isn't Necessarily What Matters Most」**，副标题「[Ea]ch dot is one of 13 failure dimensions, measured across 1,368 documented incidents (206 with material harm).」

- y 轴是 lift（1×–5×，和伤害的关联倍数）；x 轴「HOW COMMON → % of all incidents with a severe (3) rating · log scale」；颜色区分三大维度
- slide 上直接标注的两个点：**「Human Mediation: rare (0.95%) but 4.8×」**；**「System Design: most common (38%) but ≈1×」**
- 其余点的大致位置（目测读图，不是 slide 上的数字）：Harm Detection 约 2.7×、频率低；Exposure Control 约 2.3×；Fairness、Legal、Predictive Validity 约 1.8–1.9×；AI Authority 约 1.5×；Explainability 约 1.2×；Financial、Scale 约 1×；Data Foundations、Failure Analysis 低于 1×
- slide 上这张图把第 5 个子维度标成「**Failure Analysis**」，其他 slide 都叫「Failure Modes」
- 底部标语：「Prioritize by potential harm, not frequency.」
- 讲者说（视频 07:43–08:57，录音 03:37–04:51）：y 轴是「association to harm when these sub-dimensions are present」，x 轴是频率；左上是 human mediation（「effectiveness of human in the loop」）和 harm detection（「whether we have monitoring control in place」）；scale 和 system design「present in many cases, but they are not associated so much with harm」；「we need to prioritize these sub-dimensions by the potential of making harm, and then allocate appropriate resources … rather than looking at all the failure sub-dimensions equally」

**slide「Deployment Controls Shape Whether Failure Reaches Harm」**，副标题「Overall material harm = fatality, legal action or discrimination · 206 of 1,368 documented incidents」，图题「How much more often each pillar appears when harm occurs — Harmful vs. other incidents · 1× = equally common in both」：

| 大维度 | lift |
| --- | --- |
| Technical Failure | 1.10× |
| **Deployment Controls** | **2.43×** |
| Business Impact | 1.46× |

- 底部标语：「Deployment controls shape whether an error becomes ha[rm]」（照片右侧截断）
- 讲者说（视频 08:59–09:40，录音 04:52–05:34）：「controls that we have in deployment are more critically associated with harm, followed by the business impact, and followed by the technical failure」；「all these sub-dimensions are important, but deployment controls are far more important」（录音转写为 "development controls" / "distorted controls"，应为 deployment）
- 资料（DEV 文章）用的是另一组指标：「Severe monitoring-and-detection failures were associated with a 4.7-times lift in fatal outcomes, while high AI decision authority was associated with a 2.8-times lift」，并说明「These are associations within reported incidents, not causal estimates, and the underlying data is subject to reporting bias」。演讲里没有提「不是因果」和「reporting bias」这两条限定

## 5. Materiality：用 use case 的重要性决定评估深度

**slide「[Ma]teriality Determines Evaluation Depth」**（照片左侧截断）：

- 左栏 ① DESIGN：「[sho]rt heuristics, not fitted to harm data」；Technical Failure 6 inputs × Deployment Controls 4 inputs × Business Impact 3 inputs；「[Each in]put scored 1–3 against written [criteria]; when unsure, rate higher.」；分档：Low ≤4、Medium ≤8、High ≤15、Critical >15
- 右栏 ② TEST：「Applied retrospectively to 1,368 incidents」，Material-harm rate by materiality tier：

| 档位 | 伤害率 | 事件数 |
| --- | --- | --- |
| LOW | 9.1% | 36 of 396 incidents |
| MEDIUM | 15.9% | 87 of 548 incidents |
| HIGH | 17.7% | 66 of 373 incidents |
| CRITICAL | 33.3% | 17 of 51 incidents |

- slide 注：「Harm rises with tier; Critical includes 51 incidents.」底部标语「Materiality sets how deep we evaluate.」
- 我的理解（核对算术）：四档事件数 396 + 548 + 373 + 51 = 1,368；伤害事件 36 + 87 + 66 + 17 = 206，和上一张的「206 with material harm」一致；四个百分比都和分子分母对得上
- **【待确认】打分公式**：slide 写的是三个大维度相乘（「×」），每个 input 1–3 分，阈值 4 / 8 / 15。如果是三块各自的平均分（或最高分）相乘，范围是 1–27，和阈值对得上；如果是 13 个 input 的分数先求和再相乘就对不上。照片左侧截断，看不到完整说明
- 讲者说（视频 09:43–10:27，录音 05:37–06:21）：「we designed a materiality scale … materiality directly rank orders with respect to the harm associated with this AI use cases」，所以可以「allocate more resources to much higher material AI use cases」（录音转写为 "recorders"，应为 rank orders）

## 6. 把风险拆成四个因子（Tanmay）

**slide「Decomposing AI System Level Risk」**：

> P(fail)「The model gets it wrong」(Set by: model quality, inputs) × P(reach | fail)「The error reaches a real decision」(Set by: authority, exposure) × P(miss | reach)「Nobody catches it in time」(Set by: human review, monitoring) × severity「How bad it is when it lands」(Set by: financial, legal, scale) = E[loss | A] expected harm

- 「A = the deployment setup around the model: authority · exposure · human review · monitoring」
- 标语：「Same model, different A, different risk.」
- 出处（slide 底部）：「Source: Srivastava & Sah (2026), arXiv:2602.18986 (preprint), adapted.」
- 资料：arXiv:2602.18986 是 Vishal Srivastava、Tanmay Sah，「Quantifying Automation Risk in High-Automation AI Systems: A Bayesian Framework for Failure Propagation and Optimal Oversight」，2026-02-22 提交（[arXiv](https://arxiv.org/abs/2602.18986)）。摘要里的分解是**三项**：系统失败概率 × 在给定自动化水平下失败变成伤害的条件概率 × 伤害的期望严重度；案例是 2012 年 Knight Capital 的约 4.4 亿美元损失。slide 写「adapted」，把中间一项拆成了 reach 和 miss 两项。摘要没有提 1,368 起事件或 13 维
- 讲者说（视频 10:50–11:40，录音 06:44–07:33）：四个因子是「how often the model fails, whether that failure reaches a real decision, whether it goes uncaught, and how severe the consequences are」；「this same model can create different risk depending on how we deploy it」；「if risk depends on the whole deployment system, our evaluation needs to cover that setup, too」

## 7. Agentic 系统测什么、怎么测：evaluator 也要被治理（Tanmay）

（这两张 slide 没有照片，以下全部来自视频）

- 讲者说（视频 11:43–12:21，录音 07:37–08:15）：传统 ML 已经会「challenge models」：测 assumptions、input data、behavior under stress。扩展到整个部署系统就是问：「does retrieval bring back the right information? Or what happens when a tool fails? Does human review catch mistake? Or does the agent stay within its permission or boundary? And can the system finally recover?」；「the logic stays the same. The boundary of what we test gets bigger.」
- 讲者说（视频 12:28–13:36，录音 08:22–09:30）：slide 右边是传统 ML：在 held-out data 上算 accuracy、AUC ROC，用这些证据支持部署决策，但「we still need to check the data and assumptions behind those metrics」。左边是 agentic system：输入、tool response、action sequence（「or trajectory, or rollout」）的组合太多，只能测选定的场景；有时由另一个 AI agent 或 AI judge 生成测试或判定结果，「but that judge can make mistakes, and the test can miss important failures」。所以要检查「whether the evaluator is reliable, what the evidence covers, and whether it applies to the system we are actually deploying」
- 讲者说（视频 13:32）：「**That's why the evaluator is the part of the system we govern.**」

## 8. 证伪而不是证实：Wason 2-4-6（Vishal）

**slide「[W]hy good evals need to seek failure not confirmation?」**，右上「Peter Wason · UCL · 1960 · Confirmation Bias」（配一张肖像图）：

- 「2 — 4 — 6. What is the rule?」
- STAGE 1 — LIKELY HYPOTHESIS：Ascending even numbers；8–10–12 ✓；20–22–24 ✓；「More confirmation → more confidence」
- ACTUAL RULE：Any three increasing numbers；1–2–3 ✓；3–8–100 ✓
- 「What Peter Wason found：**79%** tested only triples that fit their hunch such as increasing even numbe[rs]」（右侧截断）
- 标语：「Good evals try to falsify, not confirm.」
- 讲者说（视频 13:59–15:19，录音 09:53–11:13）：他在「cognitive neuroscience」领域的研究里看到 Peter Wason，「he's the one who coined this term confirmation bias」；「in this case it 79% of people tested wrong」；「AI systems, as we know, are trained on human-generated data」；「any evaluation … has to falsify, not to confirm」，所以「eval system needs to be failure-driven」
- 资料：Wason 的原始论文是 P. C. Wason, "On the Failure to Eliminate Hypotheses in a Conceptual Task", *Quarterly Journal of Experimental Psychology* 12(3): 129–140, 1960；[维基百科](https://en.wikipedia.org/wiki/Peter_Cathcart_Wason)说他在 UCL 任教，并且「coined the term 'confirmation bias'」。Wason 是认知心理学家，「cognitive neuroscience」是讲者的说法
- **【待确认】79% 的口径**：slide 说 79%「只测了符合自己猜想的三元组」，讲者口头说「79% of people tested wrong」，这是两种不同的说法。二手综述里常见的数字是「约 80% 第一次宣布的规则是错的」（约 20% 第一次就对）。没有读到 1960 年原文，79% 具体对应哪个指标没有核实

## 9. Build–Break–Fix 循环（Vishal）

**slide「How should we operationalize falsification?」**，中心「System Evals」，四步循环：

1. BUILD：Define the System Claim and deployment boundary.
2. BREAK：Seek disproof across failure regions, controls, authority, and workflow.
3. FIX：Intervene at the right layer: model, workflow, scope, authority, or controls.
4. BREAK AGAIN：Verify the repair and search for new failure modes.

- 标语：「Seek disproof, then verify the fix.」
- 讲者说（视频 15:19–16:01，录音 11:13–11:55）：从 system claim 出发，「testing it and trying to break it」，找出 claim 在哪里不成立；fix 有两种：「don't let the AI system run in … the territory where there are failures」，或者加上你有把握的「supporting systems」；反复迭代直到达到需要的置信度。「This is essentially the first principles based approach」
- 讲者说（视频 16:06–16:49，录音 12:00–12:43）：跑完循环，画出可信区域、「uncertain region and high-risk region」，「map out the entire failure region map and route it based on the reliability and materiality」；高风险系统里「make sure that your AI is only being used for … the region that you're confident of」

## 10. 案例：航空公司聊天机器人（Tanmay）

（没有 slide 照片；录音在视频约 17:28 处截止，后半段只有视频）

- 讲者说（视频 17:14–17:38）：一位乘客家里有人去世需要出行，问航空公司的 chatbot 能否买折扣票；chatbot 说可以先买普通票，「claim the discount afterwards within 90 days」；他照做，航空公司后来以政策不允许为由拒绝；「the tribunal held the airline responsible」
- 讲者没有说航空公司的名字。资料：这和 *Moffatt v. Air Canada*, 2024 BCCRT 149（加拿大 BC 省 Civil Resolution Tribunal，2024 年 2 月）的事实一致：Air Canada 网站的 chatbot 告诉乘客可以在 90 天内追溯申请 bereavement fare，Air Canada 拒绝，仲裁庭判航空公司对 chatbot 的说法负责（[CTV News 报道，2024-02-14](https://www.ctvnews.ca/vancouver/article/air-canadas-chatbot-gave-a-bc-man-the-wrong-information-now-the-airline-has-to-pay-for-the-mistake/)）。二手报道的赔偿金额是 650.88 加元，判决原文页面（decisions.civilresolutionbc.ca）抓取被拒，金额【待确认】
- 讲者说（视频 17:41–18:46）：failure chain 是「incorrect advice reaches the customer, the customer acts on it, and the organization faces the consequences」。用 build-break-fix 的做法：先定 requirement「advice must match the approved policy」；用「questions about refunds after travel」去挑战 chatbot；暴露出错误就加「a policy check or human escalation」；再测一次，看这些 control 能否在错误到达客户前拦住。「define the claim, actively challenge it, and use the evidence [to] guide changes and make a deployment decision」

## 11. 总结（Vishal）

（最后一张 slide 没有照片，录音没录到）

- 讲者说（视频 18:49–19:43）：「your evals should be risk-based … rather than frequency-based」；把系统拆成 technical failure、deployment controls、business context；用一个 materiality scale 给 use case 定级（「that could be your company specific scale」）；做 system-level stress testing；「once you are confident about the region, then only make the deployment decision」
- 讲者说（视频 19:36）：「Our final takeaway message is **evaluate the full AI system based on risk, not just one component**.」

## 资料补充：DEV 文章里有、演讲里没讲的内容

（供参考，均为资料，不是讲者现场说的）

- Michigan MiDAS 失业金系统：复核了 40,195 个完全由程序判定的欺诈结论，推翻 85%；22,589 个由程序发起、再转给调查员的案件，推翻 44%。文章的结论是「human involvement alone did not make the problem disappear either」
- 南澳大利亚养老院监测试点：独立评审发现系统产生了超过 12,000 条未被证实为真实事件的警报，最后几个月员工无法回应每一条，至少一条未回应的警报对应一次真实跌倒
- UnitedHealth 集体诉讼：原告指控员工被要求把护理时长控制在算法预测值的 1% 以内；文章注明被告否认、诉讼进行中、没有认定责任
- 文章对 13 维的表述和 slide 不完全相同，见「出入」

## 勘误表（转写 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| Tanmay Shah（视频字幕）/ Tanbeh（录音） | Tanmay Sah | 大会讲者页 |
| Peter Bawson（录音） | Peter Wason | slide |
| 39% of people tested wrong（录音） | 79% | slide、视频字幕 |
| development controls / distorted controls（录音） | deployment controls | slide、视频字幕 |
| human variations（录音） | human mediation | slide |
| fidelity of these sub dimensions（录音） | frequency | 视频字幕 |
| field of sub-dimensions（录音） | failure sub-dimensions | 视频字幕 |
| directly recorders（录音） | directly rank orders | 视频字幕 |
| evanuation（录音） | evaluation | 视频字幕 |
| traditional animal / RRC（录音） | traditional ML / AUC ROC | 视频字幕 |
| distributed system（录音） | agentic system | 视频字幕 |
| roll-off（录音） | rollout | 视频字幕 |
| when a suicide phase（录音） | when a tool fails | 视频字幕 |
| high created system（录音） | high-risk rated system | 视频字幕 |
| at a less chatbot（录音） | an airline's chatbot | 视频字幕 |
| human numbers（录音） | even numbers | slide |
| set of goals（录音） | setup, too | 视频字幕 |

## 出入（来源之间不一致的地方）

1. **覆盖率**：slide 97.7%；讲者说「almost 98%」；视频简介「about 98%」。以 slide 为准
2. **每起事件跨几个维度**：slide 是「median 6」和「spans nearly 6 dimensions」；讲者说「on average, there are six」。均值没有在 slide 上给出
3. **和伤害最相关的指标，演讲和文章用的是两套口径**：演讲（slide）用「material harm = fatality, legal action or discrimination」，206 起，Human Mediation 4.8×、Deployment Controls 整体 2.43×；DEV 文章用「fatal outcomes」，monitoring-and-detection 4.7×、high AI decision authority 2.8×。两组数字不能互相替换。slide 上 AI Authority 对 material harm 的 lift 目测只有约 1.5×
4. **13 维在文章里的说法**：文章的 deployment 部分列的是「excessive authority, weak oversight, **limited reversibility**, or inadequate monitoring」，slide 是 AI Authority、Human Mediation、**Exposure Control**、Harm Detection；文章的 impact 部分列「financial, legal, operational, safety, reputational, or human terms」，slide 只有 Financial、Legal、Scale 三个
5. **第 5 个子维度的名字**：多数 slide 叫 Failure Modes，「What Fails Most Often」那张图标成 Failure Analysis
6. **风险分解的项数**：slide 四项（P(fail) × P(reach|fail) × P(miss|reach) × severity），注明「adapted」；arXiv:2602.18986 摘要是三项
7. **Wason 的 79%**：slide 的口径是「只测了符合猜想的三元组」，讲者的口径是「tested wrong」；二手综述常见的是「约 80% 第一次宣布的规则错误」
8. **大会简介承诺的内容没出现**：R-BED、书名、benchmark agent trajectories 上的 pipeline 演示、「insufficient evidence」结果、简介里的「92%」都没有在演讲中出现（92% 只能从直方图间接推算）
9. **限定语**：DEV 文章明确说这些是「associations within reported incidents, not causal estimates」且有 reporting bias；演讲里把 lift 讲成优先级依据，没有重复这两条限定

## 【待确认】汇总

1. 事故库是不是 AI Incident Database，取的是哪个时点的快照（是否就是第 1–1368 号）
2. 开场「AI failures 增长」那张 slide 的具体数字（没有照片）
3. Materiality 打分公式：三块怎么从 1–3 分的 input 合成，阈值 4 / 8 / 15 对应什么尺度（照片左侧截断）
4. Wason 79% 对应原始论文里的哪个指标
5. 航空案例的赔偿金额（判决原文没有抓到）
