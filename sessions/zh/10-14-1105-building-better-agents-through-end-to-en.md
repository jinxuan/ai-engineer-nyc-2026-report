[English](../10-14-1105-building-better-agents-through-end-to-en.md) | **中文**

# Ramp 在会计 eval 上花了几个月，回报来自 memory 和 skill ablation，而不是 auto-tuning

*在 AI Engineer NYC 2026 上，Ramp 的 Ryan Stevens 讲了公司怎样为 Ramp Stack agent 搭了一个由虚构会计业务组成的 benchmark，以及在它上面跑的实验里，哪些有用、哪些没用。*

**Breakout Session · 2026 年 10 月 14 日（周三）· 11:05–11:23am · Shannon Room (NY Ballroom E)**
**讲者：** Ryan Stevens，Director of Applied Science，Ramp

---

Ryan Stevens 带领 Ramp 的应用科学家和分析工程师。这场 talk 讲的是 Ramp Stack——Ramp 给会计师事务所做的 AI 系统——以及它背后的 evaluation 工作。session 的官方介绍这样提出问题："More context should make an AI agent better… it often did the opposite."（更多的上下文本该让 AI agent 更好……结果往往相反。）

每张幻灯片都有一个小页眉，写着 "AIRFLOW @ RAMP"；talk 没有解释它。整份幻灯片分三部分："What is your agent's goal?"、"Designing your evals" 和 "Getting value from your evals"。

## 1. 先弄懂这份工作，再造 agent

![Biggest mistake people make is agentifying a process they don't understand](../../slides/10-14-1105-building-better-agents-through-end-to-en/01.jpg)
*开场页：一张 meme，写着 "I have no idea how to do accounting / But I built an agent for it."*

第一张幻灯片点出了 Stevens 所说的最大错误："agentifying a process they don't understand."（把一个自己都不懂的流程做成 agent。）（作者的录音从这张幻灯片之后才开始，所以 Stevens 是怎么引入它的没有录到。）

![For a job, map out the tasks and what success is](../../slides/10-14-1105-building-better-agents-through-end-to-en/02.jpg)
*一份工作，三项任务，每项都有自己的成功定义。*

「为一个流程造 agent，第一件事是拆解这份工作、它的任务，以及成功是什么样子，」Stevens 说。对一个记账员来说，工作是结账（close the books）。幻灯片把它拆成三项任务：

| 任务 | 成功意味着 |
| --- | --- |
| Reconcile cash | 调整后的银行余额和账簿余额一致。 |
| Record accruals | 未付费用记在正确的期间里。 |
| Explain variances | 重大变动被识别出来，并有证据支持。 |

这些成了 Ramp benchmark 里的类别，Stevens 说后面还会出现第四个。

![We map the exact process a human follows, then translate it to an agent](../../slides/10-14-1105-building-better-agents-through-end-to-en/03.jpg)
*一次对账：一边是 QuickBooks，一边是 Ramp，中间是一张 Excel 工作表。*

任务和成功标准定下来之后，团队「和记账员坐下来聊，和会计师事务所坐下来聊」，弄清每项任务完整的端到端流程。幻灯片举的例子是对账。一边是 ERP（这里是 QuickBooks），里面有应付账款（accounts payable）账簿，也就是公司要付的账单；另一边是付这些账单的系统（Ramp）。两边的数字必须一致。「如果你不知道 accounts payable 这个词，我几年前也不知道，」Stevens 说。会计把两边逐行拉进 Excel，检查对不对得上，弄清为什么对不上，然后把它们对上。幻灯片上的工作表有三张账单：

| 账单 | QuickBooks | Ramp | 状态 |
| --- | --- | --- | --- |
| 101 | $1,000 | $1,000 | Match |
| 102 | $2,000 | $2,000 | Match |
| 103 | Missing | $500 | $500 gap |

下面一个标着 "With an agent" 的框把同样的对账自动化了。「如果你确切地知道这个流程是什么，把它 agent 化就挺容易的，」Stevens 说。

## 2. 设计 eval

![Agenda: 02 Designing your evals](../../slides/10-14-1105-building-better-agents-through-end-to-en/07.jpg)
*标出 talk 第二部分的议程页。*

Stevens 说 talk 里会把「benchmark、eval、verifiability」混着用，「因为说实话，我觉得这是一锅术语大杂烩」。

### 四类任务

![We wanted to build a benchmark, so we designed four task categories](../../slides/10-14-1105-building-better-agents-through-end-to-en/04.jpg)
*benchmark 的四个类别和任务数（照片是倒着拍的）。*

| 类别 | 定义（来自幻灯片） | 任务数 |
| --- | --- | --- |
| Reconciliation | 把两个来源对上，找出差异，解释或修正对不上的地方。 | 61 |
| Data Entry | 录入或更新交易、日记账分录、供应商账单和发票。 | 26 |
| Variance Analysis | 把实际数和预算、往期或预期比较，解释驱动因素。 | 28 |
| Schedules & Accruals | 建立或更新辅助明细表，计算应计项，把正确的余额带进结账。 | 45 |
| **Overall** | | **160** |

Stevens 说，variance analysis 讲的是「数字为什么会动」。对 schedules and accruals，Stevens 只说了一句它是「会计做的一件复杂的事」，没有展开。

160 这个总数和 Mercor 公开的 APEX-Accounting benchmark 一致，那份 benchmark 描述为「10 家模拟公司中的 160 个任务」，是和 Ramp 一起做的。Ramp 自己的博客文章 "Stack Benchmarking" 描述的是 8 个 world 中的 237 个任务、3,469 条评分标准。没有任何来源解释这两套之间是什么关系。

### 有用户之前，用合成数据

![Building your evals is different depending on the product life cycle](../../slides/10-14-1105-building-better-agents-through-end-to-en/06.jpg)
*有用户之前和之后。红框标出数据那一侧："Data is the bottleneck to evals."*

幻灯片画了两条 pipeline。有用户之前：合成的业务数据（账单、付款、账簿）灌进一个保存模拟状态的数据库，tool 读这个库，agent 调用 tool。有用户之后：真实客户数据灌进一个保存客户状态的数据库，tool 和 agent 不变。这张幻灯片的第二个版本把数据和数据库那一半圈了出来，标注 "Data is the bottleneck to evals."（数据是 eval 的瓶颈。）

「记住，agent 做的全部事情就是调用访问数据库的 tool，就这样，」Stevens 说。没有用户时，「你需要模拟，你需要合成数据。只要你在一个用户不够多的领域造 agent，我强烈建议你造合成数据」。Ramp 是和 Mercor 一起搭的 benchmark：「他们是这件事上很棒的合作伙伴。你不一定要和某家公司一起做，但模拟挺重要的。」有用户之后的阶段不在这次范围内：「我只有 18 分钟。」关于瓶颈，Stevens 说这是「一句显而易见的话」，但「要做评估，你得有干净的数据」。

### 合成的业务

![We built synthetic businesses with payments, bills, and ERP data](../../slides/10-14-1105-building-better-agents-through-end-to-en/08.jpg)
*一家合成企业的公司档案，以及为它构建的文档。*

幻灯片上的示例企业是 Velo Goods Co.，描述为一家 DTC 生活方式和家居用品品牌，通过 Shopify、Amazon FBA 销售，并批发给专业零售商，年收入约 400 万美元，12 名员工，单一实体 LLC，用 QuickBooks Online 记账，Gusto 管工资，外加一家第三方物流商。档案旁边的表格列出了在 QuickBooks Online 里为它构建的文档，包括科目表（约 60 个科目）、客户和供应商名单（25 到 30 家供应商）、12 个月的 Net 30 账期批发发票，以及账单。「他们用自然年。他们只想在月底结账。他们是多渠道的，」Stevens 说。团队构建了「一家合成企业为了做好离线评估所需要的一切」。

### Eval 就是回测

![Lots of words for same concept: we want to backtest agents and measure success](../../slides/10-14-1105-building-better-agents-through-end-to-en/09.jpg)
*蜘蛛侠互指 meme，"Evals"、"Benchmarks"、"Labels" 和 "Verifiable outputs" 互相指着。*

Stevens 说「我是 MLE（machine learning engineer）出身」，认为这些词汇掩盖了一个简单的目标。eval、verifiability 和 benchmark「都只是在找 label……说到底，你就是想跑回测」，让 agent「在同一组数据上一天又一天地跑，找到优化它的办法」。

按 Stevens 的说法，coding agent 在这点上占便宜，「因为 git 天生就能让你重放」：过去的状态总是被记录下来。生产环境里的 agent 提出了一个更难的问题：「你怎么随时间记录状态，好让你能重放？」上线之前，团队自己构建这个状态，并把它固定住。

### 任务、world、trajectory、分数

![For each business, we designed a series of accounting tasks, rubric and LLM as a judge](../../slides/10-14-1105-building-better-agents-through-end-to-en/10.jpg)
*一个 eval case 从任务到分数，只对最终输出评分。*

幻灯片追踪了一个 case：

| 阶段 | 内容 |
| --- | --- |
| Task | 「对一下我们 Q4 2025 在 Ramp 上的 AP 应付款和账单。」 |
| World | 合成的 ERP 数据（AP 账簿、AP 账龄、贷项和调整）加上合成的业务数据（Ramp 账单、Ramp 付款、供应商发票） |
| Trajectory | 读取账单和 AP 数据 → 匹配账单和付款 → 调查差异 → 最终输出：一份 AP 对账报告 |
| Score（只看最终输出） | 67%，6 条标准满足 4 条 |

六条标准是：Q4 AP 余额正确、账单已匹配、付款已匹配、例外已列出（满足），以及标出重复项、解释时间差（未满足）。

这些数据环境叫 world，「这是 Mercor 起的名字，我挺喜欢」。一个 world 也可以是生产数据的一份快照。每个任务有「10 到 20 条标准」，由 LLM judge 按 rubric 评分。（Mercor 的页面给出的是平均每个任务 13.7 条标准。）Ramp 有意不对 trajectory 评分，也就是不看用没用对 tool 或 skill、路径是否高效：「刚开始造 agent 的时候，你只想要它对、便宜、快；它怎么到达那里没那么重要。」

Stevens 说，搭这一切花了「这个 agent 上线前四到六个月」，对那些「和前沿模型的优化目标有点不沾边」的领域来说，这样的投入很典型。相比之下，一个 coding agent「大概……开箱就能用」。

### Accuracy 和 Pass@k

![Two complementary ways to measure performance](../../slides/10-14-1105-building-better-agents-through-end-to-en/11.jpg)
*Accuracy 给部分分；Pass@k 问的是有没有哪次尝试全对。*

幻灯片把两个指标并排放着。Accuracy 问的是 "How many criteria are correct?"（有几条标准是对的？）：6 条对 4 条就是 67%，「部分分也算」。Pass@k 问的是 "Did any attempt get everything right?"（有没有哪次尝试全对？）在幻灯片的例子里，三次尝试分别得 4/6、5/6 和 6/6，所以 Pass@1 = 0，Pass@2 = 0，Pass@3 = 1："One fully correct attempt is enough (k = 3)."（一次完全正确的尝试就够了（k = 3）。）

「Pass@k 是一个全有或全无的指标，」Stevens 说。在出错代价高的领域，比如会计，「Pass@k 可能才是你要优化的东西……如果会计没法相信其中某一步是不是错的，他们可能就不会用你的 agent」。（Ramp 的博客文章还定义了一个更严格的指标 Pass^5，要求五次独立尝试全部满足每一条标准。它出现在下一张图里，但台上没有讨论。）

## 3. 从 eval 里拿到价值

### 选模型

![First obvious use case is to decide which models to use](../../slides/10-14-1105-building-better-agents-through-end-to-en/12.jpg)
*在 Accuracy、Pass@1、Pass@5 和 Pass^5 上比较模型，裸模型运行，不带 Stack 的 skill 或 memory。*

第一个用途是选模型。图表比较了 Ramp Stack 和 Opus 4.7、Opus 4.6、GPT 5.5、GPT 5.4、Gemini 3.5 Flash、Gemini 3.1 Pro，带有对 world 做 bootstrap 得到的 95% 置信区间。照片上读不出精确的柱高；大致上，accuracy 大多在 45% 到 65% 之间，Ramp Stack 最高；Pass@1 在约 5% 到 22% 之间；Pass@5 在约 12% 到 40% 之间；Pass^5 每个模型都低于 15%。

「这是 Stack 发布时我们写的一篇博客里的，」Stevens 说，「当时 Opus 系列是表现最好的。我们重跑这些 benchmark 时看到的仍然是这样。」Stevens 还说「five-five 是最好的模型」，录音里没说清指的是 GPT 5.5 还是 Opus 5.5。（Ramp 的博客文章说 Stack 跑在 GPT 5.5 上，带 skill 和 memory，在 accuracy 上比其他 agent 高 4%，在 Pass@1 上高 3%，而且 GPT 5.5 的准确率比 GPT 5.4 高将近 19 个点。）关键点是「accuracy 很高，Pass@1 很低。这些还没饱和」，这给 harness 方面的工作留出了提高 Pass@k 的空间。

### 模型为什么失败

![We dig deeper into why models are failing to spot optimizations](../../slides/10-14-1105-building-better-agents-through-end-to-en/13.jpg)
*三个前沿模型按类型统计的失败次数。*

| 失败类型 | Muse-Spark-1.1 (xHigh) | Claude-Fable-5 (Max) | GPT-5.6-Sol (Max+ Pro) |
| --- | --- | --- | --- |
| 推理 | 13 | 19 | 21 |
| 信息收集 | 5 | 3 | 2 |
| 指令遵循和 prompt 理解 | 3 | 1 | 4 |
| 规划和反思 | 1 | 1 | 1 |

在会计上，失败大多是推理失败。「这些 harness 其实挺擅长收集适量的数据，」Stevens 说；应付账款是「我能给一个五岁小孩讲明白的东西」。结论是："The models, they just ain't that smart, and we've got some room to run here."（这些模型就是没那么聪明，我们这里还有提升空间。）Stevens 说 Muse Spark「在这上面其实表现得很好」。（这三个模型和上一张图是不同的一组，上一张图来自发布时期的博客文章。这三个是 Mercor 的 APEX-Accounting 排行榜上的前三名，但幻灯片没说这项分析是不是来自那个 benchmark。）

### 没用的

![We ran a lot of experiments, here's what didn't work](../../slides/10-14-1105-building-better-agents-through-end-to-en/14.jpg)
*五个没起作用的实验。*

「你会看到论文里写他们做的那些很棒的事……而他们不会给你看失败的 30 件事，」Stevens 说。

| 实验 | 测试的改动 | 结果 |
| --- | --- | --- |
| Model selection | Gemini、GPT-5.4、GPT-5.5；Claude Opus vs. Sonnet | benchmark 量化了模型之间的性能差距 |
| Auto-tuned skills (GEPA) | 用 autoresearch 自动修改 skill | 训练集上的提升没有泛化 |
| High reasoning effort | 在不同模型组合上比较 high 和 low effort | 准确率相同；成本更高 |
| Minimal skill bundle | 只用 PDF + Excel vs. SLIM 4-skill baseline | 低于 4-skill baseline |
| Conservative rewrites | 更短、更谨慎的财务 skill（"xsafe"） | 比标准 skill 没有提升 |

在 GEPA 那一行，也就是自动调 skill 上，Stevens 走了一段「小支线」。GEPA（Genetic-Pareto）是 2025 年一篇论文里的 prompt optimizer，它采样系统的 trajectory，用自然语言反思，然后提出 prompt 修改。对 Ramp 来说它「不行。可能是我们 agent 的问题，但我觉得在文本空间上用 eval 做 hill climbing，不是我在这里会先做的事。直觉上很难理解它为什么会有效，也就是你怎么在一个梯度上拿到方向。我们在这上面没看到成功。到现在也还没看到」。

Stevens 想特别提一下，是因为「auto-research 上有很多噪音」。Andrej Karpathy 的 autoresearch 讲的是调 ML 模型，「那显然有梯度，有方向，搜索的也是一个有限空间」。教训是："Maybe don't think that auto-research is going to solve your bad agent problem."（也许别指望 auto-research 能解决你那个糟糕 agent 的问题。）

### 有用的

![Here's some experiments that did work](../../slides/10-14-1105-building-better-agents-through-end-to-en/15.jpg)
*三个起了作用的实验。*

| 实验 | 测试的改动 | 结果 |
| --- | --- | --- |
| Skill ablation | 把 8 个 skill 精简成 4-skill 的 SLIM 包 | 相比不用 skill，平均分 +2.8pp，6 个 world |
| Trajectory-distilled memory | 把过往运行蒸馏成 memory；去掉特定期间的金额 | 相比只用 skill，平均分 +5.7pp，34 个任务 |
| Excel skill compression | Excel skill 缩小 61%；保留有用的指令 | 质量变化 ≈0pp，每月节省 $30k |

Stevens 只讲了前两行。Excel 压缩那一行和它每月 30,000 美元的节省出现在幻灯片上，但 talk 里没提。（session 介绍和 Ramp 的博客说这次压缩是从 14k 到 5k 字符，约 64%，幻灯片上是 61%。博客给出的精简包相比不用 skill 的提升是 +2.3pp，幻灯片上是 +2.8pp。）

### Skill ablation

![Compare skill versus a no-skill baseline](../../slides/10-14-1105-building-better-agents-through-end-to-en/16.jpg)
*每个 skill 相对于不用 skill 的 baseline 的平均影响。*

Stevens 借用了机器学习里 feature ablation 的思路：去掉一个 feature，或者把 feature 加回来，以测量「某个 feature 对模型性能的增量」。Ramp 对 skill 这么做了；对 tool 更难，但也可以。目标是「精简 skill 数量，省 context」。图表：

| Skill | 相对于不用 skill 的 baseline 的提升 |
| --- | --- |
| A | +0.55% |
| B | +0.47% |
| C | −0.04% |
| D | −0.72% |
| E | −0.76% |
| F | −1.33% |
| G | −1.67% |

在台上，Stevens 说：「我想我其实把这些弄反了。我觉得负的那些是说，有这个 skill 是好的，正的是说有这个 skill 是坏的。」Ramp 的博客文章按图上画的方向解读：只有 skill A 和 B 平均为正，C 基本中性，其余都是负的。不管哪种读法，Stevens 说，「肯定有差别」，这些结果成了「也许我们该删掉这些 skill、压缩这些 skill」的候选名单。建议是："Skill ablation, you should do it. Tool ablation, you can also do."（Skill ablation 你应该做。Tool ablation 也可以做。）

### 用 roll-forward world 测 memory

![People don't test memory systems, we designed a unique way to do that: Roll Forward Worlds](../../slides/10-14-1105-building-better-agents-through-end-to-en/17.jpg)
*两家示例企业把学到的规则带进下个月；开启 memory 得 61.5%，关闭得 56%。*

「大家其实不怎么测自己的 memory 系统，」Stevens 说；他们压缩信息、存下来、再读回去。Ramp 的做法叫 roll-forward world，在两个期间上跑同一个任务。第一个期间结束后，agent 写下一条 memory；在第二个期间，要么把 memory 直接注入（「如果你想稍微作点弊」），要么让 agent 自己去搜它的 memory。

幻灯片给了两个例子。一家建筑公司在第一个月匹配记录，学到客户规则：怎么计算未完工项目的收入，每笔成本属于哪个项目。一家餐饮集团学到销售额怎么和银行存款对上，每笔销售属于哪家门店。下一个月，同一家企业、新的记录，agent 用这些规则算出新的数字。幻灯片的格言是："Remember how the business works. Use fresh numbers. Last month's amounts are not this month's answers."（记住这家企业怎么运作。用新的数字。上个月的金额不是这个月的答案。）这些规则没有写在任何地方，但能从成功标准里推断出来：「你必须学会这些规则，才能把这个任务做成功。」

柱状图显示 memory 关闭为 56%，开启为 61.5%，提升 5.5 个点。（「有用的」那张表给的是 34 个任务上 +5.7pp；两者可能是不同的切片。）「这里有大量的 alpha，」Stevens 说。写出来的 memory 很大程度上取决于 memory prompt，成功率也是。「这其实是我们看到的最大提升之一。」

talk 只讲了提升。Ramp 的博客文章还报告了一个反例：在一个计费任务上，不带 memory 的 agent 得了 75%，而开启 memory 的 agent 掉到了 25%，因为 memory 带过来了旧任务的具体事实。session 介绍里那句 memory 应该保存「reusable process, not stale facts」（可复用的流程，而不是过时的事实），指的就是这个。

### 生产环境里的回归测试

![Once it's in production, internal eval platform runs on a schedule to catch regressions](../../slides/10-14-1105-building-better-agents-through-end-to-en/18.jpg)
*Ramp 的内部 eval 平台：选 case、设定时、捕捉回归。*

「我觉得大家做得不够的，是我所说的回归测试，」Stevens 说，「我不太相信基于 agentic eval 的 CI gating。作为一个见过很多非确定性、很多 80%、70% 通过率的人，CI gating 通常不是最好的主意。」

Ramp 的做法是把 eval 放在一个其他人也能往里加东西的内部平台上。截图展示了三步。第一，选 case、trial 和模型：按 tag 筛选 case（20 个 tag，例如 `excel`、`pdf` 和 `canonical_benchmark`），每个 case 跑三次 trial，用 Stack 的默认模型和推理 effort，还有一个 "Run 206 matching cases" 按钮。第二，设定周期计划：一个叫 `stack-scheduled-evals` 的 job，每周一和周四纽约时间凌晨 2:00 运行。第三，自动捕捉回归：在同样的 case 和起始 world 上跑 benchmark，把 accuracy 和 Pass@k 和 baseline 对比，审查失败的任务和 trace。一个后台 agent 审查失败并发到 Slack：「看起来有东西退化了。去查一下。这是我认为坏掉的地方。」

## 4. 要点

![What are the takeaways](../../slides/10-14-1105-building-better-agents-through-end-to-en/19.jpg)
*收尾页上的四点。*

「我到时间了，」Stevens 说，然后念了最后一张幻灯片上的四点：把一份工作拆成若干个有清晰成功标准的离散任务；为离线评估构建真实的数据环境和任务；从这些离线工作里尽可能多地榨取价值；确保持续用离线评估来判断 agent 的表现。

---

**链接：** [日程](https://ai.engineer/nyc/2026/schedule?session=4607dbbe-8f94-4b0e-91ce-257d1cfb5448) · [Stack Benchmarking](https://builders.ramp.com/post/stack-benchmarking)（Ramp Builders）· [Ramp Stack launch](https://ramp.com/blog/ramp-stack-launch) · [Introducing APEX-Accounting, built with Ramp](https://www.mercor.com/apex/newsletter/introducing-apex-accounting-built-with-ramp/)（Mercor）· [GEPA: Reflective Prompt Evolution Can Outperform Reinforcement Learning](https://arxiv.org/abs/2507.19457) · [GEPA 代码](https://github.com/gepa-ai/gepa) · [karpathy/autoresearch](https://github.com/karpathy/autoresearch)

截至写作时，这场 session 还没有公开视频。本文依据作者的现场录音、作者拍的 19 张幻灯片照片中的 18 张（省略了一张几乎重复的）和上面列出的来源写成。录音以 Stevens 的致谢结束，没有 Q&A。[精确笔记](../precise/10-14-1105-building-better-agents-through-end-to-en.md)（中文）保留了时间戳，以及 talk、幻灯片和 Ramp 博客之间所有分歧的完整记录。
