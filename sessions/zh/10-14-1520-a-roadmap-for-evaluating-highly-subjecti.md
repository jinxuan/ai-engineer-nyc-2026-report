[English](../10-14-1520-a-roadmap-for-evaluating-highly-subjecti.md) | **中文**

# 评估主观任务，每一步都在和噪声作战

*在 AI Engineer NYC 2026 上，Netflix 的 Cameron Wolfe 为没有标准答案的任务给出了一份三段式 eval 路线图：human evaluation 定义质量，LLM judge 作为经过校验的代理，production monitoring 作为最终检验；全程用统计方法区分真实的改进和噪声。*

**Breakout · 2026 年 10 月 14 日（周三）· 3:20–3:38pm · Turing Room (Riverside Ballroom) · Evaluations Track**
**讲者：** Cameron R. Wolfe，Netflix Staff Research Scientist（Deep (Learning) Focus newsletter 作者）

![Talk Slides and Information](../../slides/10-14-1520-a-roadmap-for-evaluating-highly-subjecti/01.jpg)
*talk 的第二张幻灯片指向 cameronrwolfe.me/links，那里有完整幻灯片和所有参考资料。*

---

「我们要过的信息很多。」Cameron Wolfe 开场说，「算是一个概览。」第二张幻灯片放了一个二维码和链接，指向完整幻灯片和 Wolfe 写的长文，想看细节的人可以去看。

Wolfe 讲的是创意写作、deep research 这类没有唯一正确答案的任务该怎么做 eval。这场 17 分钟的 talk 搭了一个可重复的三段式框架，又加了第四步「重复」，并且反复回到同一个主题：噪声。

作者只拍到了这一张幻灯片。下文的幻灯片图片来自 Wolfe 公开的幻灯片，talk 是逐页按它讲的。

## 1. 为什么 eval 难，主观任务的 eval 更难

Wolfe 说，eval 很难，证据随处可见：「大家一直在聊 benchmaxxing。几乎每天都有新的 eval 发布，eval 也在不断修订。」开场幻灯片收集了一些例子，包括一张标题为「The Benchmark Landscape Is Exploding」的图表，以及 OpenAI 解释 SWE-bench Verified 为什么「no longer measures frontier coding capabilities」（不再能衡量前沿编码能力）的文章。

幻灯片给了三个原因。**范围**：模型和 agent 能力很广，覆盖各种任务和领域。**成本**：benchmark 包含大量测试用例，有的运行时间长，有的很贵。**噪声**：「（多个来源的）non-determinism 让 eval 过程充满变数。」幻灯片把「噪声」框了起来，标注「This talk!」。Wolfe 说，噪声「让人很难从结果里得出可靠的结论」。

为了说明噪声从哪里来，Wolfe 拆解了一次 eval trial 的结构。eval suite 是一组任务。每个任务交给模型加 harness，幻灯片把这部分标为「non-deterministic」，它产出一条 trajectory 和一个结果。然后 grader 给出 pass 或 fail。成本让任务集只能很小（幻灯片写着「Small to limit cost」）；Wolfe 在台上说，最近有些 benchmark「跑一次要花 5 万、10 万美元」。小任务集加上 non-deterministic 的 agent，结果本来就很难靠得住。

Wolfe 指出，最近大部分 eval 工作集中在可验证任务上，这类任务的 grader 是确定性的：「字符串匹配、符号验证、可执行的测试用例，等等」。

![可验证性的光谱](../img/10-14-1520-a-roadmap-for-evaluating-highly-subjecti/p11.jpg)
*来自 Cameron Wolfe 公开幻灯片：真实任务分布在从不可验证到可验证的光谱上，附一个由 LLM judge 打分的创意写作例子。*

但真实任务分布在一条光谱上。幻灯片把它们从不可验证排到可验证：创意写作、聊天、deep research、职场任务、软件工程、指令遵循、竞赛编程和数学。在数学那一端，模型对应用题的答案可以用字符串匹配和标准答案比对。在另一端，幻灯片的例子让模型「写一段简短、有画面感的文字，描写日出时分一个安静的小镇」，由一个 LLM judge 从原创性、情感冲击力和连贯性三方面按 1 到 5 分打分，给出「Score: 4/5」。Wolfe 说，在这一端，「pass/fail 的定义极其主观、开放，连人都很难评」。

评估这类任务要用 LLM judge 或 rubric，也就是用「另一个 non-deterministic、基于 LLM 的组件」替代确定性的 grader。结果是在一个本来就难的问题上又叠加了更多噪声和主观性。Wolfe 用 Netflix 电影《Bird Box》（2018）里角色蒙着眼的剧照来形容这种感觉：「我们对自己的 agent 做得好不好完全是瞎的。」

## 2. 三段式框架

![三段式框架](../img/10-14-1520-a-roadmap-for-evaluating-highly-subjecti/p14.jpg)
*来自 Cameron Wolfe 公开幻灯片：production monitoring 反馈给 human evaluation，human evaluation 再反馈给 offline evaluation。*

目标这张幻灯片列出了「一个可重复的三段式主观任务 eval 框架」、每一段的最佳实践，以及一个优先事项：「Reduce noise and draw reliable conclusions」（减少噪声，得出可靠结论）。图里三个框 Offline Evaluation、Human Evaluation 和 Production Monitoring 连在一起，箭头从 production 指回 offline。Wolfe 说这些实践「既适用于主观任务，也适用于一般的 eval」，talk 会一路收集「可以用来减少噪声的技巧」。幻灯片按依赖顺序讲这三段：先 human evaluation，再 offline evaluation，最后 production monitoring。

## 3. 第一步：human evaluation

human evaluation 是一个任务「定义、衡量和改进质量的 North Star」。它需要一个清晰的质量定义，Wolfe 说这通常来自和「少数几位领域专家」合作写质量规范。幻灯片列出规范需要的三样东西：一小组质量标准、一种明确的评分方式（「某种二元或 Likert 评分」），以及细节和具体例子。目标是给评分人尽可能多的信息，「把这个人工评估过程的主观性降到最低」。

### 校准（calibration）

再好的规范也不能盲信。下一张幻灯片写着：「Don't trust the quality… calibrate the evaluation process.」（别轻信质量……校准评估过程。）具体做法是：多个人评同一份样本，测量他们的一致程度，在他们分歧的地方更新规范。「你就是在和人聊，看看我的规范里是不是有什么问题，让人不清楚该怎么评。」Wolfe 说。目标是规范清晰、一致性高；演讲备注补充说，一致性要长期监控。幻灯片的阅读清单列了两个常用的一致性指标 Fleiss' kappa 和 Krippendorff's alpha，Wolfe 没有口头提到。做到位之后，就得到一个校准过的人工评估流程。

### 需要多少数据：power analysis

要得出可靠的结论，评估者还得正确地采样数据，包括数量和类型。数量上，Wolfe 用的是统计方法。「大致思路是，power analysis 让我们估算需要往 eval 里放多少数据，才能对两个模型之间的性能差异得出有意义的结论。」这个估算取决于三件事：你想检测的 effect size、eval 结果的噪声水平（「可以用 resampling 估计」），以及你想要的显著性水平。

![power analysis 公式](../img/10-14-1520-a-roadmap-for-evaluating-highly-subjecti/p20.jpg)
*来自 Cameron Wolfe 公开幻灯片：比较两个模型的样本量公式，各部分都有标注。*

幻灯片展示了这个公式。Wolfe 自己讨论这个话题的文章「Applying Statistics to LLM Evaluations」把它归于 Evan Miller 的论文「Adding Error Bars to Evals」：

n = (z<sub>α/2</sub> + z<sub>β</sub>)² · (ω² + σ²<sub>A</sub>/K<sub>A</sub> + σ²<sub>B</sub>/K<sub>B</sub>) / δ²

其中 ω² = Var(x<sub>A</sub>) + Var(x<sub>B</sub>) − 2Cov(x<sub>A</sub>, x<sub>B</sub>)。

Wolfe 在台上只点了三个输入，没有逐项讲公式。结合 Wolfe 文章里的定义来读，它回答的是一个问题：如果模型 A 的真实平均分和模型 B 差了某个量，需要多少道 eval 题（n）才能可靠地看出这个差距？每一项都有直白的含义：

| 符号 | 幻灯片标注 | 含义 |
| --- | --- | --- |
| δ | Minimum Detectable Effect | 你在乎的最小真实平均分差距，比如 5 个百分点。Wolfe 的文章写的是「我们想检测的最小真实均值差」。 |
| z<sub>α/2</sub>, z<sub>β</sub> | Percentiles of Standard Normal Distribution | 两个置信度设置换算成的数字。α 是误报率（把两个一样的模型判为不同），常取 0.05，对应 z<sub>α/2</sub> ≈ 1.96。power 即 1 − β，是抓到真实差距的概率，常取 0.8，对应 z<sub>β</sub> ≈ 0.84。 |
| ω² | （红框标出） | A 和 B 的分差在不同题目之间波动多大。减去 2Cov 意味着：当两个模型觉得同样的题目难或容易时，逐题比较的噪声更小。 |
| σ²<sub>A</sub>, σ²<sub>B</sub> | Expected per-question variance | 同一个模型对同一道题答多次时得分的抖动，再对所有题目取平均。 |
| K<sub>A</sub>, K<sub>B</sub> | Number of Resamples for Models A and B | 每个模型对每道题答几次。 |

从公式能读出三点。第一，n 随 1/δ² 增长：按 Wolfe 文章的说法，「要检测一半大小的性能差距，需要 4 倍的样本」。第二，结果越嘈杂（括号里的方差），需要的题目越多。第三，对每道题多采样几次（提高 K）只能缩小同题多次运行之间的抖动，也就是 σ²/K 这几项；对 ω²，即题与题之间的差异，毫无作用，那只能靠更多题目来平均掉。文章还说，「评估任何模型时，都可以用 resampling 降低分数方差」。

举个例子，数字是本文为说明而选的，不是 talk 里的：取 α = 0.05、power 0.8，(1.96 + 0.84)² ≈ 7.84。如果 ω² = 0.1，采样次数足够多让 σ²/K 项可以忽略，要检测的差距 δ = 0.05，那么 n ≈ 7.84 × 0.1 / 0.0025 ≈ 314 道题。要检测两倍大的差距 δ = 0.10，大约 78 道题就够了。

### 需要什么样的数据

数据类型上，Wolfe 总结了 benchmark 报告通常强调的几点，幻灯片列了五个特性：多样、真实、有难度、有区分度、准确。真实是指数据应该「准确反映 agent 在生产环境里要解决的任务类型」。难度和区分度（数据能不能把强模型和弱模型分开）通常靠 profiling 来衡量：「就是采样一些 rollout，看模型实际能解决哪些问题、解决不了哪些。」准确性来自「模型过滤和人工过滤相结合」。幻灯片强调这些原则「applicable to all forms of evaluation!」（适用于所有形式的 eval！），在 offline 那一步还会再出现。

结果是一个可靠的人工评估流程。它的局限，下一张幻灯片写道，是「每个新模型或实验都做人工评估，不可行、不现实」。

## 4. 第二步：offline（自动）evaluation

如果每个新 agent 或模型都需要人工审核，「它很快就会变成瓶颈，我们就没法迭代了」，Wolfe 说。办法是用「一个高效（*但不完美！*）的人工验证代理」：LLM judge，或者基于 rubric 的 judge。

两者的区别在于具体程度。LLM judge 拿到的是高层次的质量规范，和人工评分人拿到的差不多，对每个任务用同一套规范。rubric 则是「针对具体任务的清单，每一项都单独用 LLM judge 验证」。

![DR Tulu 里基于 rubric 的 judge](../img/10-14-1520-a-roadmap-for-evaluating-highly-subjecti/p27.jpg)
*来自 Cameron Wolfe 公开幻灯片：DR Tulu 论文里不断演化的 rubric，用来「像人一样」验证答案质量。*

Wolfe 的例子来自 DR Tulu 论文，它的方法是用 rubric 训练一个 deep research 模型，这些 rubric「在训练过程中和 policy model 共同演化」。图里的问题是「How did Netflix manage to successfully adapt One Hundred Years of Solitude, a notoriously difficult book to bring to the screen?」（Netflix 是怎么成功改编《百年孤独》这本出了名难拍的书的？）Wolfe 喜欢这个例子，因为任务非常主观，而且讲的是 Netflix。rubric 不给宽泛的规范，而是列出 judge 必须在答案里逐一独立检查的具体要点，「让评估客观得多」。幻灯片上的红框总结了这个思路：「Use automatic metrics to verify solution quality (similarly to a human)」（用自动指标来验证答案质量，就像人一样）。

### meta-evaluation：judge 准不准？

这些指标仍然只是代理，「所以它们不会完美」。下一步是 meta-evaluation，「确保你的 judge 是准确的！」

![用 golden sample 做 meta-evaluation](../img/10-14-1520-a-roadmap-for-evaluating-highly-subjecti/p31.jpg)
*来自 Cameron Wolfe 公开幻灯片：用 human evaluation 产出的 golden sample 衡量 judge 和人的一致程度。*

human evaluation 会产出「golden」数据，也就是评分人对其质量很有把握的样本。然后用相关性指标或二元准确率，测量 judge 或 rubric 和这些样本的一致程度。meta-evaluation 还可以走得更远。Wolfe 介绍了 Netflix 的一个项目，Netflix Tech Blog 上的文章「Evaluating Netflix Show Synopses with LLM-as-a-Judge」写过它：这个项目把 judge 的分数和弃剧率等播放指标关联起来，「如果你的 LLM judge 说某个东西不好，那很可能有人试着看了之后会弃剧」。（Wolfe 在台上说 judge 的输出和这些指标「causally correlated」；演讲备注描述的是把 judge 输出和播放指标做相关分析。）

一致性要多高才够？幻灯片说「Depends on the task」（取决于任务）。任务越主观，谁都能达到的一致性就越低。幻灯片给的答案是：「Human agreement is an informative baseline」（人与人之间的一致性是一个有参考价值的基线）。测出人和人之间的一致程度，目标是让 judge 和人的一致程度差不多。

### benchmark 和统计

有了确认可用的 verifier，就可以围绕它们搭 benchmark：像前面那样正确采样数据，用 LLM judge 或 rubric 给输出打分，并且持续修订 benchmark。还剩一个问题。在 human evaluation 里，唯一 non-deterministic 的部分是被测模型；而在 offline 环境里，「我们有一个 non-deterministic 的模型，和一个 non-deterministic 的 judge 模型」。所以 Wolfe 说，「把各种统计方法统统用上」。

![用统计方法处理噪声](../img/10-14-1520-a-roadmap-for-evaluating-highly-subjecti/p37.jpg)
*来自 Cameron Wolfe 公开幻灯片：针对 LLM judge 打分的 benchmark 的三种统计工具。*

幻灯片列了三种：用 power analysis 确定 benchmark 规模，每个结果都报告 error bar，用 resampling（「从你的 LLM 或 judge 多采几次样」）降低方差。Wolfe 的文章解释了 error bar：它基于 standard error，「也就是样本均值的标准差」，95% 区间大约是左右各 1.96 个 standard error。文章还提醒，因为两个模型的区间重叠就断定它们没有差别，是「过于保守」的。文章建议逐题比较模型，因为它们在同一道题上的得分往往同涨同跌，配对比较能抵消这部分共有的噪声。另外，题目很少时（文章给的数字是少于 100 道），基于中心极限定理的区间会低估不确定性。

下一张幻灯片是一个文字等式：「Eval Results + Confidence Signals = Meaningful Capability Improvements」（eval 结果 + 置信信号 = 有意义的能力提升）。两者都要报告，并且努力收窄区间，这样「如果某个模型的某项分数更高，那是因为模型确实更好，而不是因为噪声」。

之后 offline evaluation 就可以跑很多很多遍，幻灯片上写的是「×1000」，用来测试大量想法。最好的候选再回到 human evaluation 确认。Wolfe 说，到这一步，团队可能会问是不是可以部署了。「答案可以是可以，但如果此时就说可以，我们其实跳过了 eval 过程里非常重要的一环。」

## 5. 第三步：production monitoring

缺的那一环就是 production monitoring：把 agent 在生产环境里的使用情况当作又一次 eval。幻灯片把生产使用指标分为两类：显式的，比如点赞点踩和用户偏好反馈（「你在 chatbot 里会经常看到」，Wolfe 说）；隐式的，比如 session 时长和放弃率。这些是「agent 在生产中被使用时的质量信号」。

这些指标通过和当前版本做 A/B test 来测量。Wolfe 把它描述为「软」部署：「不是直接把模型推给所有人，而是先用 A/B test 在生产里检验这个模型的质量，确认一切正常，再全量上线。」

![production monitoring 为其他步骤提供依据](../img/10-14-1520-a-roadmap-for-evaluating-highly-subjecti/p48.jpg)
*来自 Cameron Wolfe 公开幻灯片：生产数据回流到 offline 和 human evaluation。*

全量上线之后监控还要继续，寻找「任何令人担忧的性能趋势」和 agent 失败的案例。幻灯片上的红色箭头从 Production Monitoring 分别指回另外两个框，下面一行字：「Ground your evaluations with data from the real world!」（用真实世界的数据给你的 eval 打底！）演讲备注列出了要找的东西：失败、困难样本、新用例和分布外任务。这些案例会被送回流程的其他部分。

## 6. 第四步：重复

「Evaluation is a moving target」（eval 是一个移动的靶子），幻灯片写道。每个组件都可以随时间改进：新的或更好的质量标准，对人和 judge 更好的校准，更好的数据以避免饱和，比如把生产中的错误样本加进 offline eval。或者，最后一行写着，「Repeat the entire process in a new domain!」（在一个新领域把整个流程再走一遍！），搭一套全新的 eval。

## 7. 要点

![路线图总结](../img/10-14-1520-a-roadmap-for-evaluating-highly-subjecti/p52.jpg)
*来自 Cameron Wolfe 公开幻灯片：三段各自的角色。*

总结幻灯片重述了论点。「Evaluation is already hard, *subjective tasks are even noisier*.」（eval 本来就难，*主观任务噪声更大*。）在路线图里，human evaluation 是「质量的 north star，提供一个人们认同的清晰质量定义」；offline evaluation 用「校准过的 LLM judge 或 rubric 作为代理，扩大实验规模」；production monitoring 是「监控关键生产指标并做 A/B test，作进一步评估」。三段都要「fight noise at every step!」（每一步都和噪声作战！），用 power analysis、置信区间和 resampling 降低方差，给出清晰的置信信号。

「我们对 eval 结果要非常怀疑，要尽量用统计方法。」Wolfe 说。最后一张要点幻灯片上的完整顺序是「Offline → Human → A/B test → Deploy」：跑大量 offline eval 看哪些可能有效，把最好的交给 human evaluation 获得「保真度更高的视角」，在生产中做 A/B test「先试试水」，然后部署，并持续监控失败，反馈到前面的步骤里。

最后一张内容幻灯片是一份阅读清单，覆盖标注规范和一致性指标、LLM judge 的搭建和 Netflix 的 judge 工作、基于 rubric 的 judge、LLM eval 的统计方法，以及搭 benchmark 和评估 agent 的指南。「我做了这个索引，基本上我们讲到的每样东西都链到了延伸阅读。」Wolfe 说。「非常感谢，希望对大家有帮助。」

---

**链接：** [Schedule](https://ai.engineer/nyc/2026/schedule?session=f5c310e9-b0bb-4fa6-bc0d-bda758e1c464) · [Slides（公开的 Google Slides）](https://docs.google.com/presentation/d/146TscsJ21LU4kATVS3ES2oC_oZxoCBsTH3soepY25P4/edit) · [Applying Statistics to LLM Evaluations](https://cameronrwolfe.substack.com/p/stats-llm-evals)（Cameron R. Wolfe）· [Adding Error Bars to Evals](https://arxiv.org/abs/2411.00640)（Evan Miller）· [DR Tulu: Reinforcement Learning with Evolving Rubrics for Deep Research](https://arxiv.org/abs/2511.19399) · [Evaluating Netflix Show Synopses with LLM-as-a-Judge](https://netflixtechblog.com/evaluating-netflix-show-synopses-with-llm-as-a-judge-6269251e6f28) · [Why SWE-bench Verified no longer measures frontier coding capabilities](https://openai.com/index/why-we-no-longer-evaluate-swe-bench-verified/) · [A statistical approach to model evals](https://www.anthropic.com/research/statistical-approach-to-model-evals)（Anthropic）

截至写作时，这场 talk 还没有公开录像。本文依据作者的现场录音、一张幻灯片照片、Cameron Wolfe 公开的幻灯片和演讲备注，以及上面列出的资料；录音结束时没有 Q&A。[精确笔记](../precise/10-14-1520-a-roadmap-for-evaluating-highly-subjecti.md)（中文）保留了时间戳和转写更正。
