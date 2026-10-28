# Building Better Agents Through End-to-End Evals：精确版

> Precise notes for this session, compiled from the official video captions where available, the author's room recording, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材整理完，数字逐个对过 slide 和 Ramp 博客。**这场没有 Q&A**（录音在讲者致谢后结束）。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | 作者拍的 19 张（IMG_0590–0609，缺 0593），全部是本场（页眉都是「AIRFLOW @ RAMP」），没有混入别场的照片。IMG_0594 是倒着拍的，已旋转读取。照片分辨率 1600×1200，图表上的小字（坐标刻度）读不清，具体柱高只能估读 |
| 视频 | 没有 |
| 讲者博客 | Ramp Builders「Stack Benchmarking」：https://builders.ramp.com/post/stack-benchmarking 。页面抓取结果里没有作者和日期（prep 记为 2026-06-03，【待确认】）。覆盖 skill ablation、memory、模型对比，数字和 slide 有几处不同（见「出入」） |
| 其他一手资料 | Mercor「Introducing new APEX-Accounting benchmark, built with Ramp」：https://www.mercor.com/apex/newsletter/introducing-apex-accounting-built-with-ramp/ （页面标 Updated On: Aug 2, 2026）；GEPA 论文 arXiv:2507.19457 |

## 基本信息

- 时间地点：2026-10-14 11:05–11:23am，Shannon Room (NY Ballroom E)，Breakout Session (18 mins)，Engineering Track
- 讲者：Ryan Stevens，Ramp Director of Applied Science（官方 bio：带 Applied Scientists 和 Analytics Engineers，做 Risk Management 和 Growth 的 ML；之前在 Meta Ads Core ML 做 Data Scientist；NYU 经济学博士）
- 讲者说 [04:39]：「My training is as an MLE」，后半句转写成「Before I came at RAMP, I was the first MLE at RAMP」，自相矛盾，【待确认】原话（可能是「When I came to Ramp, I was the first MLE at Ramp」）
- slide 页眉「AIRFLOW @ RAMP」的含义（团队名？内部项目名？）【待确认】，录音里没解释
- 官方简介要点：会计 agent 里「More context should make an AI agent better… it often did the opposite」；skill ablation；把一个常加载的 skill 从 14k 压到 5k 字符；memory 要存「reusable process, not stale facts」；「production agents are systems problems, not prompt problems」
- 讲的产品是 Ramp Stack（给会计事务所的 AI 系统；资料：Ramp 博客 https://ramp.com/blog/ramp-stack-launch ，搜索结果显示日期为 2026-06-03，正文没打开，【待确认】）
- 结构（slide IMG_0597）：01 What is your agent's goal? / 02 Designing your evals / 03 Getting value from your evals

## 01 先弄清 agent 要做的 job

- slide「Biggest mistake people make is agentifying a process they don't understand」（meme：「I have no idea how to do accounting / But I built an agent for it」）。录音里没有这段的讲解（录音开头之前）
- slide「For a job, map out the tasks and what success is」：JOB = Close the books；TASKS 三个，每个带 success 定义：
  - Reconcile cash — Success: Adjusted bank and ledger balances agree.
  - Record accruals — Success: Unpaid expenses are recorded in the correct period.
  - Explain variances — Success: Material changes are identified and supported by evidence.
- 讲者说 [00:00]：「the first task in building an agent for a process is breaking down the job, the tasks, and what success looks like」；这张图是三个类别，「There's gonna be a fourth that you're gonna see a little later」
- slide「We map the exact process a human follows, then translate it to an agent」：QuickBooks（AP general ledger）→ Excel（reconciliation worksheet：Bill 101 $1,000/$1,000 Match；102 $2,000/$2,000 Match；103 Missing/$500 → $500 gap；「2 matched · 1 difference」）← Ramp（Bills）；下方「With an agent — Automates the same reconciliation」
- 讲者说 [00:33]–[01:42]：和 bookkeeper、会计事务所坐下来，弄清一个任务的完整端到端流程。reconciliation 就是左边 ERP 里的 accounts payable ledger 和右边付账单的系统（Ramp）数字对上；会计在 Excel 里逐行拉两边数据，看是否 tie，找原因，再让它们 tie。「pretty easy to agentify this process if you understand exactly what it is」

## 02 设计 eval

### benchmark 的四类任务

- slide「We wanted to build a benchmark, so we designed four task categories」（照片倒置，已旋转）：

| Category | Definition（slide 原文） | Tasks |
| --- | --- | --- |
| Reconciliation | Tie the two sources together, identify differences, and explain or correct the breaks. | 61 |
| Data Entry | Post or update transactions, journal entries, vendor bills, and invoices. | 26 |
| Variance Analysis | Compare actuals against budget, prior periods, or expectations and explain the drivers. | 28 |
| Schedules & Accruals | Build or update supporting schedules, calculate accruals, and carry the right balances into the close. | 45 |
| Overall | — | **160** |

- 讲者说 [01:42]：「I'm going to interchangeably use terms like benchmarks, evals, verifiability because frankly I think it's a soup of language」
- 资料：Mercor 的 APEX-Accounting 正好是「160 tasks across 10 simulated companies」，built with Ramp；Ramp 博客写的是「237 tasks」「3,469 grading criteria」、8 个 world。两个数字的关系见「出入」

### 有用户之前靠合成数据

- slide「Building your evals (benchmark) is different depending on the product life cycle」：Before users：Synthetic business data（Bills · payments · ledger）→ Database（Simulated state）↔ Tools ↔ Agent；With users：Real customer data → Database（Customer state）↔ Tools ↔ Agent。下一张同图把左半边（数据 + Database）圈红，标注「**Data is the bottleneck to evals**」
- 讲者说 [02:25]：「remember all agents are doing is they're calling tools that hit a database, that's it」；没有用户就需要 simulation 和合成数据，「I would highly recommend anytime you're building an agent for an area where you don't have enough users you should build synthetic data」
- 讲者说 [02:57]：benchmark 是和 **Mercor** 合作建的（转写为 "Mercore"），「They were a great partner for this. You don't have to build it with a company but simulation is pretty important」。有用户之后的阶段这场不讲，「I only have 18 minutes」
- 讲者说 [03:04]：「data is the bottleneck to evals… to evaluate you have to have clean data」

### 合成企业

- slide「We built synthetic businesses with payments, bills, and ERP data」：Company Profile 示例 **Velo Goods Co.**，「a DTC lifestyle and home goods brand selling through three channels: Shopify (direct-to-consumer storefront), Amazon FBA, and wholesale to specialty retailers. ~$4M annual revenue, 12 employees… Single-entity LLC on QuickBooks Online with Gusto for payroll. Uses a third-party logistics provider (3PL)…」；右侧表格列出 Chart of Accounts（~60 accounts）、Customer list、Vendor list（25–30 vendors）、Invoices (AR)（12 months of wholesale invoices, Net 30 terms）、Bills (AP)，Medium 列为 QBO（表格小字只能部分读清）
- 讲者说 [03:35]：「They're a [DTC] lifestyle company… They have a calendar year. They want to do just close at the end of the month. They're multi-channel」；为它建了 ERP、customer list、vendor list、AP 等，「everything that would be needed for a synthetic business in order to do good offline evaluation」

### eval 就是 backtest

- slide「Lots of words for same concept: we want to backtest agents and measure success」（Spider-Man 互指 meme：EVALS / BENCHMARKS / LABELS / VERIFIABLE OUTPUTS）
- 讲者说 [04:39]–[05:43]：evals、verifiability、benchmarks「are just trying to find labels」；「really, you're just looking to run back tests… against the same set of data day over day over day」。他的说法：coding agent 的处境好，「git by nature allows you to replay things in the past state」；production agent 的问题是「How do you capture state over time so you can replay?」

### Task → World → Trajectory → Score

- slide「For each business, we designed a series of accounting tasks, rubric and LLM as a judge」：
  - Task：「Reconcile our AP payables and bills on Ramp for Q4 2025.」
  - World：Synthetic ERP Data（AP ledger, AP aging, Credits / adjustments）+ Synthetic Business Data（Ramp bills, Ramp payments, Vendor invoices）
  - Trajectory：1 Read bills + AP data → 2 Match bills + payments → 3 Investigate differences → FINAL OUTPUT: AP reconciliation report
  - Score（FINAL OUTPUT ONLY）：67%，4 of 6 criteria met；criteria：Q4 AP balance correct ✓、Bills matched ✓、Payments matched ✓、Duplicates flagged ✗、Timing gaps explained ✗、Exceptions listed ✓
- 讲者说 [05:43]：这些叫 **worlds**，「It's a term that Mercor came up with, and I quite like it」；world 也可以是 production 数据的 snapshot
- 讲者说 [06:17]：只评最终输出，每个 task 「anywhere from 10 to 20 criteria」；**不评 trajectory**（有没有调对工具、用对 skill、路径是否高效）——「when you're initially building your agent you just want it to be right, to be cheap and to be fast; how it gets there is a little bit less important」
- 资料：Mercor 页面写 rubric 平均每题 13.7 条；Ramp 博客 3,469 条 / 237 题 ≈ 14.6 条（我的理解，自己算的），都落在讲者说的 10–20 之间
- 讲者说 [07:23]：「the process to build this whole thing was **four to six months** before launching this agent」；在 frontier model 没有专门优化过的领域（「slightly orthogonal to what they're optimized against」），这种投入是常态；coding agent「out of the box」大概能用

### 指标：Accuracy 和 Pass@k

- slide「Two complementary ways to measure performance」：
  - Accuracy — 「How many criteria are correct?」 4 of 6 → 67%，「Partial credit counts」
  - Pass@k — 「Did any attempt get everything right?」 Attempt 1: 4/6 Incomplete，Attempt 2: 5/6 Incomplete，Attempt 3: 6/6 Fully correct → Pass@1 = 0，Pass@2 = 0，Pass@3 = 1；「One fully correct attempt is enough (k = 3)」
- 讲者说 [08:28]：「Pass@k is an all or nothing metric」；会计这类出错代价高的领域，「Pass@k might be the thing you're optimizing for… if an accountant can't trust that one of the steps is wrong then they may not use your agent」（转写为 "PASATK"）
- 资料（Ramp 博客）的定义：Pass@1 单次全对；Pass@5「at least one of five independent attempts fully satisfies all criteria」；**Pass^5「all five independent attempts fully satisfy all criteria」，是更严格的一致性指标**；默认每题跑 5 次。讲者现场没讲 Pass^k，但模型对比图里有 Pass^5（见下一节、「出入」）

## 03 用 eval 获取价值

### 选模型

- slide「First obvious use case is to decide which models to use」：图「Model comparison by metric」，副标题「95% CIs from world-bootstrap · raw models run without Stack skills or memory」。四组指标 Accuracy、Pass@1、Pass@5、Pass^5；七个系列：Ramp Stack、Opus 4.7、Opus 4.6、GPT 5.5、GPT 5.4、Gemini 3.5 Flash、Gemini 3.1 Pro。估读（刻度 0/15/30/45/60/75%，【待确认】精确值）：Accuracy 一组大多在 45–65%，Ramp Stack 最高；Pass@1 在约 5–22%；Pass@5 约 12–40%；Pass^5 都在 15% 以下，Gemini 两个和 GPT 5.4 接近 0
- 讲者说 [09:05]：「this is from a blog post we wrote when Stack launched. At the time, the Opus models were the highest performing. That's still what we see when we rerun these benchmarks. Five-five is the best model.」「accuracy is very high. Pass@1 is very low. These aren't saturated yet」；对 Ramp 是好事，因为「harness optimizations allow us to get extra pass@」
- 资料（Ramp 博客）：Ramp Stack 跑在 **GPT 5.5** 上，带 skills 和 memory；Stack 比其他 agent accuracy 高 4%、Pass@1 高 3%；GPT 5.4 → 5.5 accuracy 差近 19pp；Opus 4.6 和 4.7「little to no difference」；Gemini 落后，3.5 Flash 明显不如 3.1 Pro；单题耗时 GPT 5.4 约 4 分钟最快，Gemini 3.5 Flash 略超 20 分钟最慢，其余约 10 分钟
- 「Five-five」指 GPT 5.5 还是 Opus 5.5，录音听不出，【待确认】（图上没有 Opus 5.5）

### 失败原因分析

- slide「We dig deeper into why models are failing to spot optimizations」，三个模型的失败次数（y 轴 Failures）：

| 失败类型 | Muse-Spark-1.1 (xHigh) | Claude-Fable-5 (Max) | GPT-5.6-Sol (Max+ Pro) |
| --- | --- | --- | --- |
| Reasoning failure | 13 | 19 | 21 |
| Information-gathering failure | 5 | 3 | 2 |
| Instruction-following and prompt understanding failure | 3 | 1 | 4 |
| Planning and reflection failure | 1 | 1 | 1 |

- 讲者说 [10:10]：会计场景主要是 reasoning failure，「these harnesses are pretty good at actually gathering the right amount of data… accounting is not that difficult of a gathering exercise」；「the models, they just ain't that smart, and we've got some room to run here」；reflection failure「I would actually call it other」
- 讲者说 [11:07]：「[Muse Spark] actually did really well in this」（转写为 "new spark"）
- 资料：Mercor APEX-Accounting 页面的三个头部模型正是这三个：Claude Fable 5 56.4%、Muse Spark 1.1 52.6%、GPT-5.6 Sol 51.5%（Mean % Criteria Passed, k=3）；58% 的题没有任何模型在任何一次全对。这张失败分析图很可能来自 APEX-Accounting 的设定（我的理解，【待确认】）
- 注意这张图的模型（Muse Spark 1.1、Fable 5、GPT-5.6 Sol）和上一张模型对比图（Opus 4.6/4.7、GPT 5.4/5.5、Gemini）不是同一批，后者是 Stack 发布时的旧数据（讲者说「from a blog post we wrote when Stack launched」）

### 没用的实验（含 GEPA）

- slide「We ran a lot of experiments, here's what didn't work」：

| Experiment | Change tested | Result |
| --- | --- | --- |
| Model selection | Gemini, GPT-5.4, GPT-5.5; Claude Opus vs. Sonnet | Benchmark quantified model performance gaps |
| Auto-tuned skills (GEPA) | Automated skill edits with autoresearch | **Training gains did not generalize** |
| High reasoning effort | High vs. low effort across model combinations | Same accuracy; higher cost |
| Minimal skill bundle | PDF + Excel only vs. SLIM 4-skill baseline | Below the 4-skill baseline |
| Conservative rewrites | Shorter, more cautious finance skills (xsafe) | No gain over standard skills |

- 讲者说 [11:07]：「you'll see papers of like these awesome things that they did… and they don't show you the 30 things that failed」
- 讲者说 [11:38]（GEPA 的「side quest」，转写为 "jeepa auto tuning auto researching skills"）：「no good. It might be our agent, but I think the idea of hill climbing with evals on text space is not the first thing I would do here. Intuitively, it's difficult to understand why it would work, i.e. how you can get direction on a gradient. We did not see success here. We've still not seen success.」
- 讲者说 [11:38]–[12:23]：「there's a lot of noise on auto-research. The Karpathy famous tweet is about tuning ML models, which obviously it has a gradient, it has a direction, it's also a finite space it's searching over」；「maybe don't think that auto-research is going to solve your bad agent problem」
- **资料：GEPA 是什么**。论文「GEPA: Reflective Prompt Evolution Can Outperform Reinforcement Learning」，Lakshya A. Agrawal 等，arXiv:2507.19457（https://arxiv.org/abs/2507.19457 ），ICLR 2026 有页面（https://iclr.cc/virtual/2026/poster/10009493 ），代码 https://github.com/gepa-ai/gepa 。GEPA = Genetic-Pareto：对 compound AI system 采样 trajectory（推理、tool call、tool 输出），用自然语言反思诊断问题、提出并测试 prompt 修改；维护一个 Pareto front（保留在各个样本上表现最好的候选，而不是只演化全局最优）以避免局部最优。论文 v2 说六个任务上平均比 GRPO 高 6%、最多 20%，rollout 少最多 35 倍（v1 是四个任务、平均高 10%；两版数字不同）。它也是 DSPy 里的一个 optimizer（二手资料，未核对 DSPy 文档，【待确认】）
- 资料：Karpathy 的 autoresearch 是 github.com/karpathy/autoresearch（2026 年 3 月初发布；agent 改单个 `train.py`、跑实验、只保留有改进的修改；二手资料，没打开 repo 本身）

### 有用的实验

- slide「Here's some experiments that did work」：

| Experiment | Change tested | Result |
| --- | --- | --- |
| Skill ablation | Trim 8 skills to a 4-skill SLIM bundle | +2.8pp mean score vs. no skills · 6 worlds |
| Trajectory-distilled memory | Distill prior runs into memory; remove period-specific amounts | +5.7pp mean score vs. skills alone · 34 tasks |
| Excel skill compression | Shrink Excel skill by 61%; preserve useful instructions | ≈ 0pp quality change, **$30k/month saved** |

- 讲者说 [12:23]：只讲前两个（skill ablation、trajectory distilled memories）。**Excel skill 压缩和 $30k/month 录音里没讲**，只在 slide 上

### Skill ablation

- slide「Compare skill versus a no-skill baseline, exact same logic as feature ablation for ML」：「Average skill impact」，x 轴「Improvement relative to no-skill baseline (%)」：Skill A +0.55%、B +0.47%、C −0.04%、D −0.72%、E −0.76%、F −1.33%、G −1.67%
- 讲者说 [12:23]：类比 ML 里 leave-one-out 的 feature ablation，「trying to assess the incrementality of a given feature on model performance」；他们只对 skills 做了，tools 更难但也能做；目标是「trim the number of skills, saving context」
- 讲者说 [13:29]：「**I think I've reversed these actually.** I think the negatives are like, it's good with the skill and the positives is bad with the skill. Regardless… there's definitely differences」；结果成为「maybe we should delete these skills, compress these skills」的候选集
- 资料（Ramp 博客）：「Only Skill A and Skill B were positive on average」「Skill C was essentially neutral」「The rest were negative on average, and none was uniformly best across worlds」——**与图的正负方向一致，讲者现场「画反了」的担心与博客不符**（见「出入」）。博客：bookkeeping spreadsheet skill 从 14k 压到 5k 字符（−64%）；单 world 配对验证原 skill +3.7pp、瘦身版 +3.3pp，整个 slim bundle +5.4pp；六个标准 world 上 slim bundle 比 no-skills +2.3pp

### Trajectory-distilled memory / Roll Forward Worlds

- slide「People don't test memory systems, we designed a unique way to do that → Roll Forward Worlds」：
  - This month → What the agent remembers → Next month
  - Construction company：Match records（Differences explained，Learn how this business works）→ CUSTOMER RULES：How to count income on unfinished jobs；Which job each cost belongs to（Needed to solve the next period）→ New month. Same business. Use its rules on new records; Calculate the new month's numbers
  - Restaurant group：Match records → CUSTOMER RULES：How sales match bank deposits；Which store each sale belongs to → New month. Same business…
  - 底部：「**Remember how the business works. Use fresh numbers.** Last month's amounts are not this month's answers.」
  - 右图「Memory on vs. off」：Memory off 56% → Memory on 61.5%，+5.5pp
- 讲者说 [14:10]：「people don't really test their memory systems」；做法：同一任务跑两个时期，第一期后写 memory，第二期要么直接注入 memory（「if you wanna cheat a little bit」），要么让 agent 搜索 memory。customer 的隐含规则没写明，但能从 success criteria 推出来，「you have to learn these rules in order to be successful at this task」
- 讲者说 [15:16]：「there was a ton of alpha here. Like the types of memories that were written for tasks really depended on the prompts, and then success was also dependent on those prompts. We saw a very large gain. This was actually one of the biggest gains we saw」
- 资料（Ramp 博客）：roll-forward world 上「memory improved mean score from 56.0% to 61.5%」；正例：workers' compensation audit estimate 无 memory 0%、有 memory 100%；**反例**：Apex January AIA billing 任务「the no-memory agent scored 75%, while the memory-enabled agent fell to 25%」，因为 memory 里是过时、过细的步骤，「the memo smuggled in the old task's fact pattern」；「The goal is not "always write more memory."」。官方简介里的「memory needs to preserve reusable process, not stale facts」对应这个反例，**录音里没讲这个反例**

### 线上：定时回归测试

- slide「Once it's in production, internal eval platform runs on a schedule to catch regressions」（平台截图）：
  1. Choose cases, trials, and model：Case tags（Select all tags (20)）；Trials per case 3（「Each selected case runs 3 times」）；Model: Stack default；Reasoning effort: Model default；Subagent model: Stack default；Subagent reasoning effort: Model default；按钮「Run 206 matching cases」；case 示例 `abk_excel_organization`（a payroll sheet that subtotals in place…）、`abk_pdf_bank_statement_to_excel`（a scanned-style PDF bank statement…），tag `excel`、`pdf`、`canonical_benchmark`
  2. Set the recurring schedule：`stack-scheduled-evals`，「At 02:00 AM, only on Monday and Thursday」（New York time），tag 含 `reconciliation`、`excel`、`mercor_world_2_2`（小字，【待确认】）
  3. Catch regressions automatically：Run benchmark（Same cases + starting worlds）→ Compare with baseline（Accuracy + Pass@k）→ Review regressions（Inspect failed tasks + traces）
- 讲者说 [15:49]：「The thing that I think people don't do enough of is what I call regression testing. **I'm not a big believer in CI gating based off of agentic evals.** As someone that's seen a lot of non-determinism, a lot of 80%, 70% passage, CI gating is not usually the best idea」；内部平台托管 eval，别人可以往里加；case 打 tag 便于聚类；定时跑；「a background agent that then looks at failures, regressions」，再发 Slack：「it looks like something's regressed. Go investigate it. Here's what I think broke.」

## 总结

- slide「What are the takeaways」：
  - Breakdown a job into the discrete jobs to be done with clear success criteria
  - Build realistic data environments and tasks for offline evaluation
  - Focus on extracting as much value from these offline efforts as possible
  - Figure out how to make it so that offline evaluations are consistently used to judge agent performance
- 讲者说 [17:01]：「I'm at time」，复述四条后致谢

## Q&A 精确版

没有。录音在 [17:47]「Thank you」后结束，没有提问环节的声音；18 分钟 breakout 讲满了（讲者说「I'm at time」）。

## 勘误表（Whisper 转写 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| RAMP | Ramp | 上下文 |
| Mercore | Mercor | 资料（Mercor APEX-Accounting 页面） |
| Scamers and accruals | Schedules and Accruals | slide |
| DVC lifestyle company | DTC lifestyle (and home goods) brand | slide |
| PASATK / pass at k / Passat one | Pass@k / Pass@1 | slide |
| new spark | Muse Spark（Muse-Spark-1.1） | slide |
| jeepa | GEPA | slide |
| auto-research / auto researching | autoresearch（Karpathy） | slide、资料 |
| Karpathy famous tweet | Karpathy 的 autoresearch | 上下文 |
| MLE | MLE（machine learning engineer），保留 | 上下文 |
| in MLE by training | an MLE by training | 上下文 |
| Five-five | GPT 5.5 或 Opus 5.5，【待确认】 | — |

## 出入

1. **任务数：160 vs 237**。slide 四类合计 160（61+26+28+45），和 Mercor APEX-Accounting「160 tasks across 10 simulated companies」一致；Ramp 博客写「237 tasks」「eight worlds」、3,469 条 criteria。可能是两个版本（Mercor 公开版 vs Ramp 内部版），没有资料明确说明二者关系，【待确认】。Ramp 发布文说「independent benchmark of 200+ tasks」（搜索摘要，未打开原文）
2. **Skill ablation 的增益：+2.8pp（slide）vs +2.3pp（博客）**，都是「slim bundle vs no-skills baseline，六个标准 world」。以哪个为准【待确认】，可能 slide 是重跑后的新数字
3. **Memory 增益：+5.7pp（「did work」表，34 tasks，vs skills alone）vs +5.5pp（Memory on/off 图和博客，56.0% → 61.5%）**。两处 slide 自己就不一致，可能是不同切片（34 题 vs roll-forward world），【待确认】
4. **Excel / spreadsheet skill 压缩：61%（slide）vs 64%（博客，14k → 5k 字符；官方简介也是 14k → 5k）**。14k→5k 算下来约 64%。$30k/month 只在 slide 上，博客没有成本数字
5. **Skill ablation 图的正负方向**：讲者现场说「I think I've reversed these」；博客说只有 A、B 平均为正、C 中性、其余为负，和图上的方向一致。以图和博客为准（我的理解）
6. **Pass@k 和「出错代价高」**：讲者说高风险领域可能要优化 Pass@k；按博客定义，Pass@k 是「k 次里至少一次全对」，衡量「每次都对」的是 Pass^k（图里有 Pass^5，讲者没讲）。只记录定义差异，不判断讲者本意
7. **memory 的反例**：官方简介和博客都强调 memory 会带入过期事实（AIA billing 75% → 25%），录音里只讲了正向的大增益

## 【待确认】汇总

1. 讲者「first MLE at Ramp」那句的原话（[04:39]）
2. slide 页眉「AIRFLOW @ RAMP」指什么
3. Ramp Stack 发布日期 2026-06-03（来自 prep 和搜索摘要）和「Stack Benchmarking」博客的作者、日期
4. 「Five-five is the best model」指 GPT 5.5 还是 Opus 5.5
5. 模型对比图各柱的精确值（照片只能估读）
6. 失败分析图是否来自 APEX-Accounting 的设定
7. 160 题（slide/Mercor）和 237 题（Ramp 博客）的关系
8. +2.8pp vs +2.3pp 以哪个为准
9. +5.7pp（34 tasks）vs +5.5pp 的切片差异
10. 定时任务 tag 小字（`mercor_world_2_2` 等）
11. GEPA 是否作为 DSPy optimizer 发布（只看到二手资料）
