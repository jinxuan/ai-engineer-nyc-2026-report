# A Roadmap for Evaluating Highly-Subjective Tasks：精确版

> Precise notes for this session, compiled from the official video captions where available, the author's room recording, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材整理完。作者只拍了 1 张照片，**slide 来源是讲者公开的 Google Slides 导出**（PDF 56 页，含逐步出现的动画页；txt 导出带讲者备注）。正文约 17 分钟，**没有 Q&A**，[17:30] 之后是会后闲聊。统计部分（power analysis 公式）第 3 节单独做了白话说明。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide（作者照片） | 1 张 IMG_0742，是本场第 2 页「Talk Slides and Information」+ 二维码 + `cameronrwolfe.me/links`。**没有外来照片** |
| slide（公开 deck） | [Google Slides](https://docs.google.com/presentation/d/146TscsJ21LU4kATVS3ES2oC_oZxoCBsTH3soepY25P4/edit)，导出 `/export/pdf`（56 页）和 `/export/txt`（含讲者备注）。下文「slide p.N」指导出 PDF 的页码 |
| 视频 | 没有 |
| 一手资料 | deck 里链接的资料：[Applying Statistics to LLM Evaluations](https://cameronrwolfe.substack.com/p/stats-llm-evals)（Cameron R. Wolfe，2026-03-09，power analysis 公式的讲解）；[Adding Error Bars to Evals](https://arxiv.org/abs/2411.00640)（Evan Miller，2024-11-01，公式出处）；[DR Tulu](https://arxiv.org/abs/2511.19399)（rubric 例子）；[Evaluating Netflix Show Synopses with LLM-as-a-Judge](https://netflixtechblog.com/evaluating-netflix-show-synopses-with-llm-as-a-judge-6269251e6f28)（Netflix 博客原文 403 打不开，只读到二手摘要） |

## 基本信息

- 时间地点：2026-10-14 3:20–3:38pm，Turing Room (Riverside Ballroom)，Breakouts，Evaluations Track
- 讲者：Cameron R. Wolfe，Netflix Staff Research Scientist（Globalization 团队，做多模态 LLM），Deep (Learning) Focus newsletter 作者，Rice 大学 CS 博士（session.json bio）
- 官方简介要点：为主观任务搭大规模自动评测的技术路线图：组织人工标注、构建和评估 LLM judge、用 production monitoring 指导模型评测、「reducing evaluation uncertainty via statistics」
- slide p.2 / 作者照片：所有资料链接在 `cameronrwolfe.me/links`
- 讲者说 [00:00]：「It's supposed to be kind of like an overview」，细节扫码看 slide 和他写的文章

## 1. 动机：评测为什么难，主观任务为什么更难

- slide p.3：「Evals are difficult…」，配了 benchmaxxing 的访谈截图、「The Benchmark Landscape Is Exploding」图，以及 OpenAI 的「Why SWE-bench Verified no longer measures frontier coding capabilities」（链接 [openai.com](https://openai.com/index/why-we-no-longer-evaluate-swe-bench-verified/)）
- slide p.4–5：难的三个原因：**Scope**（能力覆盖面广）、**Noise**（「Non-determinism (from several sources) makes the evaluation process variable」）、**Cost**（测试用例多、耗时或昂贵）。p.5 把 Noise 框起来：「This talk!」
- 讲者说 [00:34]：大家都在说 benchmaxxing，新的 eval 天天出、天天改；今天重点讲 noise：「it just makes it really difficult to draw reliable conclusions from the results」
- slide p.6–8「Structure of an evaluation trial」：Evaluation suite（一组 task）→ Model + harness（标「Non-deterministic」）跑出 trajectory 和 outcome → Grader 给 pass/fail。task 数量标「Small to limit cost」。p.8：近期评测工作大多是 **verifiable tasks**，grader 是确定性的：「String match, symbolic verification, executable test cases, and more…」
- 讲者说 [01:06]–[01:41]：因为成本，「some of these recent benchmarks will cost like, you know, $50,000, $100,000 to run」，所以 task 集合通常不大；再加上 agent 非确定性，评测结果本来就难依赖（这个金额 slide 上没有，只有讲者说）
- slide p.9–11：任务按可验证性排成一条光谱，从 Non-Verifiable 到 Verifiable：Creative Writing、Chat、Deep Research、Workplace Tasks、SWE、Instruction Following、Competitive Programming、Math。p.10 举数学题例子（Model Output 经 String Match 对上 Ground Truth）；p.11 举创意写作例子（「Write a short, evocative paragraph about a quiet town at sunrise」，由 LLM Judge 按 originality、emotional impact、coherence 给 1–5 分，输出「Score: 4/5」）
- 讲者说 [02:14]–[02:49]：一端是数学，可以确定性验证；另一端是创意写作，「the definition of pass/fail is extremely subjective and open-ended and it's hard even for humans to evaluate」
- slide p.12：「These tasks introduce more noise and subjectivity in the evaluation process」。讲者备注：「Replace deterministic component with non-deterministic verifier」
- 讲者说 [02:49]：要用 LLM judge 或 rubric，等于把原本确定性的 grader 换成又一个非确定性的 LLM 组件，噪声和主观性更多
- slide p.13：电影 *Bird Box*（Netflix，2018）剧照，备注「Drowning in noise / Unsure if the agent performs well or not」

## 2. 总框架：三个部分

- slide p.14「High-Level Goals」：Repeatable three-part framework for subjective evaluation；Outline best practices for each component；**Reduce noise and draw reliable conclusions**。图：**Offline Evaluation ← Human Evaluation ← Production Monitoring**
- 讲者说 [03:20]：「a repeatable three-part framework for evaluating super-subjective tasks. This will consist of offline evaluation, human evaluation, production monitoring」（转写为 "viewing evaluation, instruction monitoring"），每部分给最佳实践，「which applies both [to] the subjective task and just [general] evaluation」，沿途讲降噪和提高统计置信度的技巧

## 3. Step One：Human Evaluation（含统计部分的白话说明）

**3.1 质量定义和标注规范**

- slide p.15–16：「Our North Star for defining, measuring, and improving quality」；「Requires a clear definition of quality!」；「Partner with SMEs → clear **quality guidelines** to minimize subjectivity」，三个勾：
  - ✅ Small set of quality criteria
  - ✅ Define how to rate them（备注：binary / likert）
  - ✅ Details & specific examples
- 讲者说 [03:50]–[04:54]：先和少数领域专家（subject matter experts）一起写 quality guidelines：定义一小组标准、定义怎么打分（「some type of binary or [Likert] score」）、给大量细节和具体例子，「so that we can minimize the subjectivity of this human evaluation process」

**3.2 校准（calibration）**

- slide p.17–18：「Don't trust the quality… **calibrate** the evaluation process」：Multiple evaluations per sample → measure agreement → update guidelines；Aim for clear quality guidelines with high agreement。讲者备注还有一条：「Continue to monitor agreement over time → ensure results are meaningful」
- 讲者说 [04:54]：写得再好也不能假设人工评测可靠。具体做法：同一个样本让多个人评，看他们是否一致，再根据分歧改规范；「you're literally just talking with humans and trying to see … is there some issue in my guideline that makes it unclear」
- slide p.55「Further Reading」列的校准指标：[Fleiss' kappa](https://en.wikipedia.org/wiki/Fleiss%27s_kappa)、[Krippendorff's alpha](https://en.wikipedia.org/wiki/Krippendorff%27s_alpha)（讲者没口头提）

**3.3 采多少数据：power analysis（统计部分）**

- slide p.19：「Sample data properly to draw reliable conclusions: **Amount of data**: power analysis → setup & volume for confident eval results」
- slide p.20 列出三个输入：⏩ Effect size we want to detect；⏩ Level of noise in eval results；⏩ Desired significance level。并给出公式（逐字抄自 slide）：

  n = (z_{α/2} + z_β)² · (ω² + σ²_A / K_A + σ²_B / K_B) / δ²

  其中 ω² = Var(x_A) + Var(x_B) − 2Cov(x_A, x_B)；σ²_A = E[σ²_{A,i}]，σ²_B = E[σ²_{B,i}]。slide 上的标注：(z_{α/2} + z_β) 是「Percentiles of Standard Normal Distribution」；K_A、K_B 是「Number of Resamples for Models A and B」；δ 是「Minimum Detectable Effect」
- 讲者说 [05:42]–[06:17]：「power analysis allows us to estimate the volume of data that we need to put into the evaluation process to make a meaningful conclusion about the difference in performance between two different models」；估计时看 effect size、eval 结果的噪声水平（「you can estimate this with resampling」）、和想要的显著性水平。**讲者没有逐项讲公式**，只讲了这三个因素
- 资料（公式出处）：Cameron 的文章 [Applying Statistics to LLM Evaluations](https://cameronrwolfe.substack.com/p/stats-llm-evals)（2026-03-09）说公式「provided in」Evan Miller 的 [Adding Error Bars to Evals: A Statistical Approach to Language Model Evaluations](https://arxiv.org/abs/2411.00640)（2024-11-01，文章称之为 Anthropic 的论文）
- **白话说明**（符号含义来自 Cameron 文章，标「资料」；解释和例子是我的理解）：
  - 要回答的问题：模型 A 和模型 B 的平均分如果真的差 δ，评测集至少要多少道题（n），才能可靠地看出来？
  - **δ（最小可检测差距）**：你在乎的最小分差，比如 5 个百分点。资料：「the smallest true difference in mean that we want to detect」
  - **α（显著性水平）**：误报率，即两个模型其实一样、却判断成有差别的概率，常用 0.05。**1 − β（power）**：真有差别时能检测出来的概率，常用 0.8。z_{α/2}、z_β 是把这两个概率换算成标准正态分布上的位置（α = 0.05 时 z_{α/2} ≈ 1.96；power 0.8 时 z_β ≈ 0.84）
  - **ω²**：同一道题上「A 的分 − B 的分」的方差，代表「题和题之间」的差异。资料：代码里定义为 Var(x_A − x_B)。减去 2Cov 那一项的意思是：如果两个模型在同样的题上一起难、一起易（正相关），配对比较的噪声就小
  - **σ²_A、σ²_B**：同一道题让模型重复答几次，分数本身的抖动（「题内」噪声），对所有题取平均。资料：「the question-level average variance for each model」
  - **K_A、K_B**：每道题重复采样几次。重复越多，σ²/K 这一项越小。资料：「Resampling can be used to reduce score variance when evaluating any model.」
  - 读公式的三条直觉：① n 和 1/δ² 成正比，资料原话「detecting a gap in performance that is half the size requires 4× the number of samples」；② 噪声（括号里那堆方差）越大，n 越大；③ 多采样（K 变大）只能压低「题内」抖动，压不低 ω² 这一项（题和题之间的差异），后者只能靠加题（我的理解，从公式直接读出）
  - 示意计算（我的理解，**不是讲者或 slide 的数字**）：α = 0.05、power = 0.8 → (1.96 + 0.84)² ≈ 7.84；假设 ω² = 0.1、K 足够大使 σ²/K 可忽略、δ = 0.05 → n ≈ 7.84 × 0.1 / 0.0025 ≈ 314 道题；δ 改成 0.10，n 降到约 78

**3.4 采什么数据**

- slide p.21–23：「Data type / quality: diverse, realistic, difficult, discriminative, accurate」；p.23「Applicable to all forms of evaluation!」。讲者备注：Difficult / discriminative「usually determined by profiling」；Accurate「filtered with humans / models」
- 讲者说 [06:50]–[07:23]：要多样、贴近真实（「accurately reflect the types [of] tasks that the agent has [to] solve in production」）、有难度和区分度，通常靠 profiling（采样 rollouts，看模型哪些能做对、哪些不能）来判断；要准确，靠模型和人工结合过滤。这些原则不只用于人工评测，离线评测也用
- slide p.24：「Human evaluating every new model or experiment is not feasible / realistic」，✅ Reliable Human Eval Process ✅

## 4. Step Two：Offline (Automatic) Evaluation

**4.1 LLM judge 与 rubric**

- slide p.25：「Create an efficient (*but imperfect!*) proxy for human verification」：Humans verify solution quality → Use an automatic proxy；LLM-as-a-Judge + Rubric-based Judges
- 讲者说 [07:39]：每个新模型都人工评就成了瓶颈，「we can't iterate」，所以要做离线/自动评测，用 LLM judge 或 rubric 作为人工的代理
- 讲者说 [08:11]（slide 备注同义）：LLM judge 拿的是通用的高层 quality guidelines，和人工评测一样，对所有任务实例用同一套；rubric 是「a task-specific checklist of individual items that are each separately verified with an LLM judge」
- slide p.26–27：DR Tulu 论文的图。用户问题「How did Netflix manage to successfully adapt One Hundred Years of Solitude, a notoriously difficult book to bring to the screen?」，右边是「Search-Guided Rubrics Co-Evolve with the Policy」的训练示意。p.27 红框：「Use automatic metrics to verify solution quality (similarly to a human)」
- 资料：[DR Tulu: Reinforcement Learning with Evolving Rubrics for Deep Research](https://arxiv.org/abs/2511.19399)（Rulin Shao、Akari Asai 等，2025-11-24）；方法叫 RLER（Reinforcement Learning with Evolving Rubrics），「rubrics are constructed and maintained to co-evolve with the policy model during training」
- 讲者说 [08:41]：喜欢这个例子，因为任务本身非常主观，又和 Netflix 改编有关；rubric 不是高层规范，而是要 judge 逐条独立核对的具体项，「to make the evaluation way more objective」
- slide p.28：「but imperfect!」——自动指标只是代理

**4.2 Meta-evaluation：确认 judge 准不准**

- slide p.29–31：「**Meta-evaluation**: ensure your judge is accurate!」；用人工评测得到 **golden** 数据（图上是金条）；「Measure agreement of judges / rubrics with golden samples」：✅ Correlation metrics、✅ Binary accuracy
- 讲者说 [09:23]：golden data 是人工评测中人们有把握的样本；看 judge/rubric 与它们的一致性，用相关系数、二分类准确率等
- slide p.32「Many possibilities for meta-evaluation!」：Netflix 技术博客截图「Evaluating Netflix Show Synopses with LLM-as-a-Judge」，作者 Gabriela Alessio、Cameron Taylor、Cameron R. Wolfe（截图上的日期像是 Apr 10, 2026，字太小，【待确认】）。讲者备注：「In this blog post, we correlate LLM judge outputs with core streaming metrics at Netflix」
- 讲者说 [09:55]：他在 Netflix 做的项目，「showed that the outputs of this LLM judge were causally correlated with the key streaming metrics like abandonment. So if your LLM judge tells you something is bad, it's likely that somebody will abandon the show」（**出入**见下）
- 资料（二手，[ZenML 的摘要](https://www.zenml.io/llmops-database/scaling-synopsis-quality-evaluation-with-llm-as-a-judge)，Netflix 原文 403 没读到）：四个标准 tone、clarity、precision、factuality；golden set 约 600 条（从约 1,000 条初标中筛出）；judge 二分类准确率 tone 87.85%、clarity 86.99%、precision 90.82%、factuality 85.37%；写作专家之间经 8 轮校准一致性约 80%；与 streaming 指标的关系是**相关**分析（在同一部剧内比较 synopsis 分数变化与 take fraction、abandonment rate 变化），作者说缺少「clean experimental variation in LLM scores」。全部【待确认】，需对照原文

**4.3 一致性要到多少才够**

- slide p.33–34：「What level of agreement is "good"? Depends on the task…」；「**Human agreement is an informative baseline**」
- 讲者说 [10:27]：越主观的任务，能达到的一致性越低；看不同人之间的一致性，以此为目标：judge 与人的一致性做到和人与人之间差不多

**4.4 建 benchmark 和用统计处理噪声**

- slide p.35：「Create benchmark(s) around these automatic verifiers」：Sample data properly (similar to before)；Use LLM judges or rubrics to verify outputs；Continually revise and improve your benchmarks
- slide p.36–37：「How do we handle noise? *Use statistics!*」：
  - Power analysis → benchmark size
  - Report error bars with evaluation results
  - Use re-sampling to reduce variance
- 讲者说 [11:20]：人工评测里非确定的只有被测模型（录音转写为 "In human evaluations, we only have a non-deterministic LLM"）；离线评测里「we have a non-deterministic model and a non-deterministic [judge] model」，所以「apply the kitchen sink of different statistical methods」：用 power analysis 定 benchmark 大小、报 error bars、用 resampling（「taking multiple [samples] from your LLM or from the judge」）降方差
- 白话（资料：Cameron 文章）：error bar / 置信区间的基础是 standard error，「the standard error is just the standard deviation over sample means」；95% 区间用 ±1.96 个 standard error。文章还提醒：两个模型的置信区间重叠就判断「没差别」的做法「overly conservative」，推荐按题配对比较（paired），因为两个模型在同一题上的分数往往相关，配对能降低方差；题数少（文章引用的标准是 n < 100）时 CLT 会低估不确定性
- slide p.38：「**Eval Results + Confidence Signals = Meaningful Capability Improvements**」
- 讲者说 [11:52]：不只报结果，还要报置信信号，并尽量把区间收窄，「We know if one score on any model is better, it's because the model is actually better, not because of noise」
- slide p.39–43：offline 评测可以跑「×1000」次来扩大实验，把最好的结果送去人工评测；p.43「Are we ready to deploy?」
- 讲者说 [12:25]–[12:40]：把效果好的送回人工评测确认；这时可能会问能不能上线了，答案可以是 yes，但会跳过一个很重要的环节

## 5. Step Three：Production Monitoring

- slide p.44–45：「Define and track **production usage metrics** for our agent」：Explicit：thumbs up / down, user preference feedback；Implicit：session length, abandonment rate。「Quality signals as agent is used in production!」
- 讲者说 [12:40]–[13:43]：用 agent 在生产里的使用情况作为另一种评测；显式指标在 chatbot 里随处可见，隐式指标如会话长度、放弃率
- slide p.46：「Measure these metrics in an **A/B test** to validate performance」：Use this as a "soft" model deployment；Vet agent quality in production → full rollout
- 讲者说 [13:43]：在 A/B test 里测这些指标，得到相对于现有版本的表现；「basically treat this as a soft model [deployment] process」，先在生产里验证质量再全量
- slide p.47–48：「Continual **production monitoring** after the agent is deployed」；「Ground your evaluations with data from the real world!」，从 Production Monitoring 画箭头回到 Offline 和 Human Evaluation。备注：find failures, difficult samples, new use cases, OOD tasks
- 讲者说 [14:17]–[14:39]：全量后继续监控，看有没有令人担心的趋势，找 agent 失败的用例，把它们送回其他评测环节

## 6. Step Four：Repeat

- slide p.49–50：「Evaluation is a moving target…」，持续改进：Define new or better criteria；Improve calibration of humans and judges；Find better data to avoid saturation；**Repeat the entire process in a new domain!**
- 讲者说 [14:39]–[15:13]：每个部件都可以更好：新的或更好的质量标准、更好的人工和 judge 校准、从生产里找新的错误样本放进离线评测；或者把整个流程在新领域再做一遍，建一个全新的 eval

## 7. 总结

- slide p.51–54「Takeaways」：
  - 「Evaluation is already hard, *subjective tasks are even noisier*」
  - Our roadmap：Offline Evaluation「Use calibrated LLM judges or rubrics as a proxy to scale experiments」；Human Evaluation「North star for quality, provides a clear quality definition that humans agree on」；Production Monitoring「Monitor and A/B test key production metrics for further evaluation」
  - 「Our roadmap ← *fight noise at every step!*」：Power analysis, confidence intervals, resampling, and more；Reduce variance and provide clear confidence signals
  - 「**Multi-Step Evaluation:** Offline → Human → A/B test → Deploy」
- 讲者说 [15:13]–[16:56]：「we have to fight noise at every step … We need to be very skeptical with evaluation results and try and use statistics」；流程是先跑大量离线评测，把最好的送人工评测（「higher fidelity view」），再 A/B test「test the waters」，最后上线，上线后继续监控、把失败样本送回前面各步
- slide p.55「Further Reading」：Human evaluation（annotation guidelines 3 篇参考；Fleiss' kappa、Krippendorff's alpha）；LLM Judges & Rubrics（setup 3 篇、Netflix 的 LLM-as-a-Judge 工作 2 篇、rubric-based judges 2 篇）；Statistics for LLM Evaluation（概览 2 篇；key concepts：power analysis、confidence intervals、re-sampling、pairwise comparison）；LLM Evaluation Guides（建 benchmark 1 篇、评测 agent 2 篇）。PDF 里能提取到的链接包括 [Anthropic: A statistical approach to model evals](https://www.anthropic.com/research/statistical-approach-to-model-evals)、[Anthropic: Demystifying evals for AI agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)、Hamel Husain 的 [evals FAQ](https://hamel.dev/blog/posts/evals-faq/) 和 [LLM judge](https://hamel.dev/blog/posts/llm-judge/index.html)、Eugene Yan 的 [labeling guidelines](https://eugeneyan.com/writing/labeling-guidelines/) 和 [LLM evaluators](https://eugeneyan.com/writing/llm-evaluators/)、Lilian Weng 的 [human data quality](https://lilianweng.github.io/posts/2024-02-05-human-data-quality/)、[Autorubric](https://arxiv.org/abs/2603.00077)（Delip Rao、Chris Callison-Burch，2026-02）、[TQLite](https://arxiv.org/abs/2608.02975)（Bhavin Jawade、Cameron R. Wolfe，2026-08，Netflix 翻译质量评测），以及 Cameron 自己的 Substack 文章（stats-llm-evals、llm-as-a-judge、rubric-rl、llm-bench、agent-evals）。链接和条目的一一对应没有逐个核对，【待确认】
- 讲者说 [16:56]：「I created this index here, so this links out to further reading on pretty much everything that we talked about」

## Q&A 精确版

没有 Q&A。正文到 [17:26] 结束（「Thank you so much, and I hope it was helpful」），[17:30] 之后是会后闲聊，与本场内容无关，转写里的「观众」标签无意义。

## 勘误表（Whisper 转写 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| go thank you to the slides | go to the slides | 上下文、slide p.2 |
| bench-maxing | benchmaxxing | slide p.3 |
| deterministic depth | deterministic check【待确认】 | slide p.8 |
| string matchings and wallet verification test cases | string matching, symbolic verification, executable test cases | slide p.8 |
| path fail | pass/fail | 上下文 |
| evaluate their livelihood | 【待确认】（大意是「人也很难评」） | 听不清 |
| white color inductor | LLM judge or rubric【待确认】 | slide p.10 备注 |
| this gradient | this grader | 上下文 |
| viewing evaluation, instruction monitoring | human evaluation, production monitoring | slide p.14 |
| re-evaluation | general evaluation【待确认】 | slide 备注「Apply to both subjective evals + general」 |
| binary or larger score | binary or Likert score | slide 备注「binary / likert」 |
| calibrate and exchange the rules | calibrate and update the guidelines【待确认】 | slide p.17 |
| multiple community evaluation samples | multiple evaluations per sample | slide p.17 |
| Power is a really interesting area | Power analysis | slide p.19 |
| not useful | not [due to] noise【待确认】 | 上下文 |
| submit this to be just different case studies | 【待确认】（应是 difficult / discriminative） | slide p.21 |
| pro-finaling | profiling | slide 备注 |
| Dr. Tolu paper | DR Tulu paper | slide p.26、arXiv 2511.19399 |
| cap-specific | task-specific | slide 备注 |
| netflix film | Netflix 改编的《百年孤独》（One Hundred Years of Solitude） | slide p.26 |
| our John Kuehn | our judge【待确认】 | 上下文 |
| between governments and humans | between judges and humans | 上下文、slide p.34 |
| non-deterministic guide model | non-deterministic judge model | 上下文 |
| QM evaluation | human evaluation | 上下文 |
| taking multiple labs … judge's balls | taking multiple samples from your LLM or from the judge【待确认】 | slide p.37「re-sampling」 |
| chat box | chatbots | 上下文 |
| AD test | A/B test | slide p.46 |
| soft model improvement process | "soft" model deployment | slide p.46 |
| repeat it into new buildings | repeat it in a new domain | slide p.50 |
| how I'm judges | LLM judges | 上下文 |
| data testing | A/B testing | 上下文 |
| power analysis confidence in our role for example in the war | power analysis, confidence intervals, resampling【待确认】 | slide p.53 |
| off-line evals | offline evals | — |

## 出入（来源之间不一致的地方）

1. **「causally correlated」vs 相关分析**：讲者说 [09:55] judge 输出与 abandonment 等指标「causally correlated」；slide 备注写的是「we correlate LLM judge outputs with core streaming metrics」；二手摘要称博客是在同一部剧内的相关分析，作者明说缺少「clean experimental variation」。按 slide 和资料，应写「相关」而不是「因果」；需对照 Netflix 原文确认（【待确认】）
2. **人工评测里有几个非确定组件**：转写是「In human evaluations, we only have a non-deterministic LLM」，下一句是「Now we have a non-deterministic model and a non-deterministic [judge] model」。按上下文，前一句指的是只有被测模型非确定（人工评分者不是 LLM）；可能是口误或转写错，写的时候用「人工评测时只有被测模型是非确定的」
3. **$50,000–$100,000 的评测成本**：只有讲者说，slide 上没有，也没给出处
4. **三部分的顺序**：讲者口述「offline evaluation, human evaluation, production monitoring」，但 slide 的讲法是 Step One = Human、Step Two = Offline、Step Three = Production，上线流程是 Offline → Human → A/B test → Deploy。口述顺序只是图上从左到右的读法，不矛盾

## 【待确认】清单

1. Netflix synopsis 博客原文：发布日期（截图像 Apr 10, 2026）、judge 与 streaming 指标是相关还是因果、各项准确率数字（目前只有二手摘要）
2. 「Further Reading」各条目与 PDF 中链接的一一对应
3. 勘误表里听不清的短语：evaluate their livelihood、white color inductor、calibrate and exchange the rules、not useful、submit this to be just different case studies、our John Kuehn、taking multiple labs、confidence in our role for example in the war、deterministic depth、re-evaluation
4. slide p.3 benchmaxxing 访谈截图的出处（PDF 链接里有一条 [Matt Turck 播客 Sebastian Raschka 一期](https://www.mattturck.com/podcast/state-of-llms-2026-rlvr-grpo-inference-scaling-sebastian-raschka)，推测是它，没核对）
