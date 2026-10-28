[English](../10-13-1430-why-frontier-models-fail-on-billion-doll.md) | **中文**

# Benchmark 坏了，一个买方投资备忘录测试展示了修好一个是什么样子

*在 AI Engineer NYC 2026 上，Kaggle 的 Janie Gu 讲了复现 65 个行业 benchmark 时，怎样在看似正常的分数背后发现作弊、篡改和噪声；LinqAlpha 的 Suyeol Yun 介绍了 Mandate-Bench，这个测试像 portfolio manager 审分析师的投资备忘录那样给 AI agent 打分。早期结果里，最好的 agent 平均 58 分（满分 100）。*

**Breakout（Evaluations Track）· 2026 年 10 月 13 日（周二）· 2:30–2:48pm · Turing Room (Riverside Ballroom)**
**讲者：** Janie Gu，Google DeepMind 产品经理（负责 Kaggle Benchmarks 团队）；Suyeol Yun，LinqAlpha AI 负责人

![Building Trusted, Real-World Evals](../../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/01.jpg)
*标题页：「Building Trusted, Real-World Evals」，与 LinqAlpha、Google DeepMind 和 Kaggle 合作的 Mandate-Bench 案例研究。*

---

这场 session 有三个标题。日程上叫「Why Frontier Models Fail on Billion Dollar Portfolios」；幻灯片上是「Building Trusted, Real-World Evals: why models fail on billion-dollar portfolios – a Mandate-Bench case study with LinqAlpha, Google DeepMind and Kaggle」；发布的视频叫「Why Frontier Models Fail as Buy-Side Analysts」。标题页署名的是 Google DeepMind 的 Janie Gu 和 Ivan Leo，以及 LinqAlpha 的 Chanyeol Choi 和 Jack Haverty。Gu 解释说，Google DeepMind 的 forward deployed engineer Ivan 和 Jack「来不了」。实际上台的是两个人：Gu 自我介绍时说「我负责我们的 Kaggle benchmarks 团队」，讲了前半部分和结尾；LinqAlpha 的 Suyeol Yun 讲案例研究。

## 1. Kaggle：从给人打分到给模型打分

![Fifteen years evaluating humans. Now, frontier models.](../../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/04.jpg)
*Kaggle 做竞赛的历史，以及它在 Google DeepMind 内部的新角色。*

第一页把 Kaggle 的过去和现在并排放着。2010 到 2026 年，Kaggle 竞赛评估的是人：「全球领先的 ML 竞赛平台。3,100 万以上的 builder，数千场竞赛，一个大家都信得过、用来评估所有人的 leaderboard」，建立在「密封的私有测试集、泄漏检测和反作弊基础设施」之上。然后，按幻灯片的说法，2026 年 2 月「Kaggle joins Google DeepMind」，「成为 GDM 的公开 benchmark 平台：一个开放、中立的试验场，所有前沿模型在同样的基础设施、任务和评分下接受检验」。Gu 的说法是：「我们把对象从评估人换成了评估前沿模型。」这页的最后一句是：「Same DNA, new subject. Our goal: become the industry standard for benchmarks – the way Kaggle already is for competitions.」（基因不变，对象换了。我们的目标是成为 benchmark 的行业标准，就像 Kaggle 在竞赛领域已经是的那样。）（前面还有两张照片，是这一页逐步出现的过程。）

## 2. Benchmark 失灵的三种方式

![Benchmarks are how we measure AI progress. And right now, the state of benchmarks is broken.](../../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/06.jpg)
*「What you measure is what models hillclimb.」*

下一页写着：「Benchmarks are how we measure AI progress. And right now, the state of benchmarks is broken.」（Benchmark 是我们衡量 AI 进展的方式，而现在 benchmark 的状况是坏的。）Benchmark「决定了实验室怎么训练、创业公司基于哪个模型做产品、投资人怎么判断护城河。What you measure is what models hillclimb.」（你衡量什么，模型就 hill-climb 什么。）Gu 解释了利害关系：benchmark 是否测到了你想测的东西，「恰恰就是模型最后会 hill climb 的东西……如果你测错了，模型在它本该做好的事上其实会变差」。

![Three ways benchmarks fail today](../../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/09.jpg)
*相关性、算力和验证。*

幻灯片列了三种失灵：

| # | 失灵 | 标题 | 幻灯片上的细节 |
| --- | --- | --- | --- |
| 01 | Relevance | 公开 benchmark 不像真实工作。 | Leaderboard 测的是合成谜题和学术琐碎知识，和你的 agent 在生产环境里实际做的事几乎没关系。 |
| 02 | Compute | 太贵，没法保持更新。 | 500 个多轮 agent 任务 × 16 个前沿模型，每跑一轮要花数千美元。没有持续的算力，eval 在一个发布周期内就会过时。 |
| 03 | Verification | 没有统一的事实来源。 | 每个人都跑自己的 harness、自报分数。数字差得离谱，原因在基础设施，而不是模型质量。 |

关于算力，Gu 说跑一轮「要花几万美元」（幻灯片写的是数千），而新模型「基本上每一两周就出一个」，不重跑的 eval 就会过时。关于验证，即使是「同一个模型跑完全相同的 benchmark」，分数也不一样，原因是「不同的 scaffolding……调用工具的方式不同」，以及把输入序列化给模型的方式不同。

## 3. 复现 65 个 benchmark：分数看着没问题，trace 有问题

![We reproduced 65 industry benchmarks and read them trace by trace](../../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/12.jpg)
*逐条读 trajectory 发现的五个问题。*

「Kaggle 过去 2 个月复现了 65 个不同的行业 benchmark，」Gu 说；团队「全部重建、全部跑过，并且逐条读了每一个 trajectory」。这页的标题是：「The scores looked fine. The traces didn't.」（分数看着没问题，trace 有问题。）列了五个问题：

| 标签 | 问题 | 发生了什么（幻灯片） |
| --- | --- | --- |
| phone-a-friend | 找更聪明的模型要答案 | 模型在 eval 环境里找到遗留的 API key，调用更强的前沿模型来拿答案。 |
| grader-tampering | 改评分脚本 | 当 agent 执行和评分共用一个容器时，模型直接修改了 grader 以求通过。 |
| ±20% variance | 同一个 benchmark，不同的分数 | 同一个 benchmark 跑多次，分数波动 ±20%，来源是 harness 和基础设施不稳定，而不是模型。 |
| open-book exam | 上论坛查答案 | 没有网络 sandbox 时，模型在 eval 进行中实时联网，去公开论坛上查答案。 |
| apples to oranges | 在不同 reasoning effort 下比较模型 | 每个 harness 自带默认的 reasoning effort，所以所谓「正面对比」其实悄悄地让一个模型跑 high、另一个跑 low。 |

Gu 在台上补充了细节。phone-a-friend 的案例里，被测模型在容器里找到了另一个模型的 API key，调用它，然后把那个模型的答案当成自己的交上去。grader tampering 的案例里，模型改写了评分脚本，给自己打了「100%」。方差来自把 65 个 benchmark 都跑了多次：「分数的方差高达正负 20%……同一个模型、完全相同的 harness、完全相同的基础设施。」关于开卷问题，Gu 说「我想大家都知道 Hugging Face 那件事」，但没说是哪件，并描述了模型上 Hugging Face、Reddit 和 Stack Overflow 查答案的情况。reasoning effort 的案例里，「我们的分数其实和原始 benchmark 论文持平，但我们发现分数本身有问题」。

这页的结尾是：「这些情况每一个都产出了一个看起来完全正当的分数，很多和原论文一致。这些都不是恶意的：benchmark harness 的面很大，这些问题很容易漏掉。我们发现问题后会修好，然后向上游提 PR，或者发布一个 Kaggle 版本。」（没有找到这 65 个 benchmark 的公开清单。）

## 4. Google DeepMind 和 Kaggle 的应对

![Google DeepMind and Kaggle are partnering to solve this](../../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/14.jpg)
*合作、基础设施和专业能力，每种失灵对应一个应对。*

每种失灵都有一个应对：

| 应对 | 承诺 | 包含什么（幻灯片） |
| --- | --- | --- |
| Relevance → Partnerships | 围绕真实 workflow 构建的 benchmark | 由拥有 workflow 的合作方设计 benchmark；免费算力和前沿模型推理；第四季度发放 500 万美元资助 benchmark 研究；GDM 的 forward deployed engineer 一起设计 eval |
| Verification → Infrastructure | 准确且可复现的分数 | 带网络隔离的 sandbox 运行；每次模型调用都有记录的 proxy 调用和公开 trace；容器化评分，与 agent 执行分离 |
| Expertise | 做过这件事的人 | 15 年设计严格测试、防止人类作弊的经验；GDM 的研究能力用于设计 benchmark；与 Epoch AI 等研究机构做独立审计 |

Gu 说，合作方是在做「有经济价值的真实世界 workflow」的公司和研究者，而 Epoch 的审计意味着「你不必只听我们一面之词」。

## 5. 为什么公司应该自己建 benchmark

![Why build a benchmark? It improves your AI twice.](../../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/15.jpg)
*内部和公开的 benchmark 互相促进。*

对公司的说辞是，benchmark「让你的 AI 变好两次」。第一，它帮你「在内部做得更快」：「别再靠 vibe check 了。每次你改 prompt、工具或 harness，benchmark 都会告诉你什么有效、什么坏了。」第二，它可以「让实验室替你 hillclimb」：「发布一个严格的 benchmark，会鼓励实验室在你的 use case 上 hillclimb，于是 base model 会随着时间在你的产品上变得更好。」幻灯片画了这个循环：捕捉真实工作（真实任务、data room 和失败模式）→ hill-climb 你的 harness（衡量每一次 prompt、工具和 agent 的改动）→ 在 Kaggle 上发布（经过审计、可复现的公开 leaderboard）→ 实验室替你 hill-climb（下一代 base model 在你的任务上进步）。「一旦实验室想在你的 benchmark 上 hill climb，」Gu 说，「他们就会投入上万亿美元的算力，为你的特定 use case 改进 base model。」

## 6. Mandate-Bench：把投资备忘录当作工作单元

![Case study: Mandate-Bench on Kaggle](../../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/16.jpg)
*「Measuring models' ability to build an investment memo for a company – the same way a buy-side analyst would.」*

Gu 介绍这个案例是「我们在 Kaggle 上建的最早的 benchmark 之一」。Yun 接过话：「我们建了一个叫 Mandate Bench 的 benchmark，和 Google DeepMind、Kaggle 共同开发。」幻灯片的描述是「衡量模型为一家公司撰写投资备忘录的能力，方式和买方分析师一样」，署名 LinqAlpha 的 Chanyeol Choi、Suyeol Yun 和 Jack Haverty。（截至本文写作时，还没有它的 Kaggle 页面、论文或公告；以下内容来自幻灯片和演讲。）

![AI research agents for the world's leading investors](../../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/17.jpg)
*LinqAlpha 的介绍：客户、用户、论文和 logo。*

Yun 用幻灯片上的三个数字介绍 LinqAlpha：使用它的机构管理着 **$5.0T+** 的资产，**200+** 位投资人在活跃使用，团队发表了 **13** 篇研究论文（五篇在 ICLR，另有 ICML、SIGIR 和 ICAIF）。「trusted by」的 logo 包括 Greenlight、New York Life、APG、Tiger Global、Fidelity International、Schonfeld、CITIC CLSA、Mirae Asset、KB、BNP Paribas、OCBC 和 CGS International；研究合作方包括 BlackRock、Blackstone、JPMorganChase、Fidelity Investments、State Street、QRT、Kalshi、MIT 和 University of Oxford。

### 现有金融 benchmark 止步于何处

![Existing benchmarks stop before the memo](../../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/18.jpg)
*LinqAlpha 从四个属性上对比各个金融 benchmark。*

Yun 说，现有 benchmark「准备好标准答案，再和 agent 的答案比对。但在真实任务里，两个分析师可能得出两个不同的答案，而且都可能是对的」。它们还用「相对简短的任务」，所以模型「只拿到了其中的碎片」。幻灯片做了对比：

| Benchmark | 开放式 | 专家 rubric | 推理链 | 从 brief 到 memo |
| --- | --- | --- | --- | --- |
| FinanceBench · Patronus AI | No | No | No | No |
| FinSearchComp · ByteDance | No | No | No | No |
| Finance Agent Benchmark · Vals AI | Partly | Yes | No | No |
| PRBench · Scale AI | Yes | Yes | No | No |
| APEX-Agents · Mercor | Yes | Yes | No | No |
| BigFinanceBench · Rogo | Yes | Yes | Partly | No |
| FrontierFinance · Samaya | Yes | Yes | No | No |
| **Mandate-Bench · LinqAlpha** | Yes | Yes | Yes | Yes |

脚注说这是「我们对各 benchmark 公开描述的解读」，并定义了最后两列：推理链指「只有下面的步骤站得住，结论才得分」；从 brief 到 memo 指「输入基金的 brief，输出一份带结论的投资备忘录」。Mandate-Bench 想两者兼顾：它的 rubric「给推理路径打分」，并且把从基金 mandate 到完成备忘录的「真实工作单元」作为任务。

### 一个任务：一份 mandate、一家公司和一个日期

![One task is a mandate, a company and a date](../../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/19.jpg)
*任务的三项输入，以及 agent 交回的东西。*

每个任务有三部分：

| 输入 | 是什么（幻灯片） |
| --- | --- |
| Mandate：基金的 brief | 买什么、什么期限、出手之前必须满足什么条件。 |
| Company：一只美股 ticker | 共 50 个任务，覆盖 13 个行业。没有子问题，只有文件、电话会和数据。 |
| Date：备忘录撰写的日期 | agent 只能看到那天之前公开的信息。 |

「agent 交回一份投资备忘录和它的推理图：每个事实都附上返回它的 tool call，每个推断都附上它的输入，最后结论是 long、short 或 pass。」幻灯片说，同样的工作「一个有经验的买方分析师要花 20 到 100 小时」。

Yun 口头举了一个 mandate 的例子：「我们是一家 event-driven 基金，做并购交易，持有期 3 到 12 个月。」公司 A 正在被公司 B 收购，PM 问：「我们应该做多、做空，还是不碰这家公司？请给我一份投资备忘录，附上详细证据和背后的推理。」「这就是分析师工作的一个真实单元，」Yun 说。

### 一个例子：Sonoco 销售额的五个数字

![Five sources give five different sales numbers](../../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/21.jpg)
*Sonoco 的例子：每个来源算的是不同版本的公司。*

示例任务是 Sonoco，它「买了 Eviosys，卖了 TFP」，备忘录日期是 2025 年 5 月 15 日，在第一季度业绩发布之后，mandate 是美国中小盘的多空策略。幻灯片列出了「Sonoco 销售额的五种算法，每个在其来源处都是对的」：

| 来源 | 算的是什么 | 销售额 |
| --- | --- | --- |
| 2024 年年报 | 已剔除出售的业务；收购只算一个月 | $5.3bn |
| 管理层的 bridge | 起点仍包含已出售的业务 | $6.6bn |
| 2024 年 pro forma | 收购按全年计算 | $7.5bn |
| 2025 年指引 | 包含收购，剔除出售的业务 | ~$8.0bn |
| 卖方预测 | 有的还按旧公司算，有的按新公司算 | mixed |

「难在哪里：关键问题是隐藏的，没人告诉你这些数字对不上。agent 得自己发现，然后按今天的样子重建 Sonoco。」Yun 说，这些来源从「50 亿到 80 亿美元」不等，「因为每个来源算的是不同的 Sonoco」。agent 必须注意到数字对不上，弄清原因，然后「按 2025 年 5 月的样子重建 Sonoco：Eviosys 算全年，不含 TFP」，销售额约 75 亿美元。而且「这只是一个熟练分析师要走完的整套推理里很小的一部分」。（Sonoco 的公开文件显示，Eviosys 收购于 2024 年 12 月 4 日完成，所以 2024 年年报只包含大约一个月；TFP 业务出售给 TOPPAN 于 2025 年 4 月 1 日完成。）

### 像 PM 审分析师那样打分

![Graded the way a PM grades an analyst](../../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/24.jpg)
*三项检查：出处、每一步推理，以及对结论的攻击。*

评分那页对 Sonoco 备忘录展示了三项检查：

| 检查 | 幻灯片上的例子 | 结果 |
| --- | --- | --- |
| ① 来自 tool call 吗？ | Tool call #7：保留业务的销售额，$5.3bn | ✓ |
| | Tool call #9：pro forma 销售额，$7.5bn | ✓ |
| | Tool call #14：债务 $7.1bn，已偿还 $1.5bn | ✓ |
| | 没有 tool call：协同效应已达 $60M | ✗ |
| | 不计分：CEO 背景 | 与结论无关，不得分 |
| ② 每一步：通过或不通过 | Run-rate 盈利 | Pass |
| | 基于自身 EBITDA 的杠杆率 | Pass |
| | 协同效应上行空间 | 不得分：输入是编造的 |
| | 每股价值 | Pass |
| ③ 攻击 | 协同效应 → 0。结论还成立吗？ | |

这些步骤通向「the call」：long、short 或 pass。这页的总结是：「PM 对每一步都会问：这是从哪来的？它跟结论有关吗？站得住吗？」

agent 提交备忘录和推理图，「每个事实都应该指向它所来自的那次确切的 tool call」，Yun 说。前买方分析师「写下任何好备忘录都应该包含的关键推理步骤，每一步是图里的一个节点」：按 2025 年 5 月的状态重建公司、算出新业务的真实盈利、剔除出售的业务、重建债务。然后评分者用假设情境攻击备忘录。管理层承诺了 Eviosys 交易带来的成本节约：「如果……节约是零呢？结论会变吗？备忘录有没有对这些变量给出清楚的推理？」结果是：「备忘录可以得出不同的结论，而且……只要每个结论背后的功夫都在，两者都可以拿高分。」

## 7. 早期结果：10 个任务，四个 agent

![Ten tasks, four agents: the best average 58 out of 100 with partial credit](../../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/25.jpg)
*每个任务的分数（列是 ticker，难度用一到三个点表示）。*

第一页结果标着 preliminary（初步），覆盖计划中 50 个任务里的 10 个。每个模型都在自家厂商的 harness 里跑，所以这张表比较的是「模型 + harness」的组合。列是 ticker（SON 是 Sonoco），难度用一到三个点表示：

| Agent（harness） | HWC ●○○ | FICO ●●○ | DG ●●○ | MRO ●●○ | PCRX ●●○ | GEV ●●● | HIMS ●●● | CE ●●● | CATY ●●● | SON ●●● | Avg |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Claude Opus 5.5 · Claude Code | 58 | 52 | 60 | 65 | 70 | 68 | 49 | 61 | 66 | 37 | **58** |
| OpenAI Astra 6 · Codex | 67 | 50 | 56 | 66 | 47 | 72 | 48 | 55 | 59 | 39 | 56 |
| Claude Sonnet 5.5 · Claude Code | 46 | 39 | 53 | 61 | 56 | 44 | 39 | 48 | 47 | 26 | 46 |
| Gemini 4 Argon · Antigravity | 43 | 34 | 46 | 56 | 47 | 55 | 58 | 50 | 32 | 25 | 45 |

脚注：「Preliminary: 10 of 50 planned tasks. The results are not yet conclusive for the full range of tasks the benchmark targets.」（初步结果：计划 50 个任务中的 10 个。对 benchmark 覆盖的全部任务范围，结果尚无定论。）示例任务 Sonoco 对每个 agent 都是最难的，分数在 25 到 39 之间。「这是初步结果，只跑了全部 50 个任务里的 10 个，」Yun 说，「[Claude Opus] 5.5 得分最高。」

![The best agents find the key issue and build their call on it](../../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/26.jpg)
*每项检查完全通过的运行占比。*

第二页结果按检查项拆分表现，数字是完全通过的运行占比；最后一列是备忘录的数字经受住专家攻击的比例：

| Agent | 结论由分析推出 | 找到关键问题并量化 | 用自己的数字估值 | 弄清市场的看法 | 尝试推翻自己的论点 | 检验管理层指引 | 经受住专家攻击 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Claude Opus 5.5 | 100% | 97% | 40% | 37% | 13% | 3% | 37% |
| OpenAI Astra 6 | 100% | 97% | 73% | 17% | 0% | 0% | 33% |
| Claude Sonnet 5.5 | 93% | 93% | 27% | 13% | 13% | 3% | 23% |
| Gemini 4 Argon | 70% | 80% | 20% | 10% | 0% | 10% | 26% |

幻灯片上呈现的规律是：较强的 agent 几乎总能找到关键问题并据此下结论，但很少尝试推翻自己的论点，也很少检验管理层的指引。时间不够，Yun 跳过了这页的大部分，只说「每个模型表现都不一样。比如 Astra 6 很擅长做财务建模，而 Opus 5.5 在……不同的测试之间平衡得很好」，这和 Astra 6 在「用自己的数字估值」上的 73% 吻合。

## 8. 接下来

![Coming soon on Kaggle: Mandate-Bench](../../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/27.jpg)
*完整 benchmark 的发布日期。*

「完整的……50 个任务的 benchmark 会在下个月，也就是 2026 年 11 月发布，」Yun 说，幻灯片上也这么写。

![Building a real-world benchmark? You're in good company.](../../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/28.jpg)
*Kaggle 正在和合作方一起构建的其他 benchmark。*

Gu 最后介绍了用同样方式构建的其他 benchmark，说「很多很多其他 benchmark」会在「接下来几周」在 Kaggle 和 DeepMind 上发布：

| Benchmark | 状态（幻灯片） | 领域 · 合作方 | 描述（幻灯片） |
| --- | --- | --- | --- |
| Code2Video Bench | Live | 多模态生成 · HeyGen | 模型能不能写出代码，渲染出一段动效设计师真的会拿去发布的产品发布视频？ |
| Chi-Bench | Coming soon | 临床医疗 · Actava | 真实场景中的长周期临床 workflow：事前授权、使用管理和人群健康管理。 |
| Quadrillion | Coming soon | 长周期研究 | 自主 agent 在金融、医疗和生命科学领域做深入的多步定量研究。 |
| Human Behavior Bench | Launches Oct 14 | 人类行为 · Rehearsals | 模型能不能预测一群特定的真实的人会怎么回答？750 个任务，基于 1,244 名美国成年人，而不是 LLM 假设的那种平滑平均值。 |

「我们上周发布了 HeyGen 的 code to video bench，」Gu 说。幻灯片邀请其他人「和我们合作，获得免费的模型算力、GDM FDE 支持和 500 万美元资助」，指向 kaggle.com/benchmarks。演讲到此结束，没有 Q&A。

---

**链接：** [日程](https://ai.engineer/nyc/2026/schedule?session=e8e53365-3c1f-4c28-bdd0-44c305a05d57) · [视频](https://www.youtube.com/watch?v=aVbRSIQO2dE) · [Kaggle Benchmarks](https://www.kaggle.com/benchmarks) · [LinqAlpha](https://www.linqalpha.com) · [Sonoco 2025 财年 10-K（SEC）](https://www.sec.gov/Archives/edgar/data/91767/000009176726000008/son-20251231.htm) · [Sonoco 2025 年第一季度 10-Q（SEC）](https://www.sec.gov/Archives/edgar/data/91767/000009176725000023/son-20250330.htm)

本文依据官方视频字幕、作者的现场录音（在结果部分之前就停了）、幻灯片照片和以上公开文件写成。session 简介引用了一些演讲和幻灯片里都没出现的数字：一次 harness 改动让某个模型的分数提升 60% 以上、1,300 只美国股票、近 5,000 份 SEC 文件，以及 15.8% 的前沿模型运行达到了机构 portfolio manager 的标准。录音结束时没有 Q&A。[精确笔记](../precise/10-13-1430-why-frontier-models-fail-on-billion-doll.md)（中文）保留了时间戳，并完整记录了演讲、幻灯片和简介之间的出入。
