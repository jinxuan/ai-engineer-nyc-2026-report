# Unhobbling the Frontier：outline

> Precise notes for this session, compiled from the official video captions where available, the author's room recording, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-28）：素材整理完、Q&A 逐条校对完。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | 作者拍的 14 张（IMG_0214–0227）+ 1 张讲者照片（IMG_0228）。数字都从照片上读过 |
| 视频 | 没有。AI Engineer 频道截至 2026-10-28 没上传这场 workshop |
| 讲者博客 | [Automating eval design and hillclimbing with Claude](https://claude.dev/blog/automating-eval-design-and-hillclimbing/)（Lance Martin，2026-09-28）。覆盖第 3、4 节，数字比现场口述准 |
| 其他一手资料 | [Scaling Managed Agents](https://www.anthropic.com/engineering/managed-agents)（Lance Martin、Gabe Cemaj、Michael Cohen，2026-04-08）；[Building effective agent automations](https://claude.dev/blog/building-effective-agent-automations/)（Lance Martin、CJ Avilla，2026-10-08）；[Effort 文档](https://platform.claude.com/docs/en/build-with-claude/effort) |

来源标记：**slide**、**讲者说**（录音里有）、**资料**（上表的一手资料，附链接）、**我的理解**、**【待确认】**。「资料」是这篇新加的标记，因为讲者的博客和文档比口述更准。

## 基本信息

- 时间地点：2026-10-12 3:30–4:30pm，Shannon Room (NY Ballroom E)，Workshop。**slide 只讲了约 23 分钟，剩下约 48 分钟全是问答**（讲者说：「the slides won't take an hour … question and answer is actually much more fun」）
- 讲者：Lance Martin，Anthropic Member of Technical Staff。之前在 LangChain（讲者说，[38:05]）
- 官方简介：「several simple ways to get the most out of frontier models, including harness design, prompting, and setting up evaluations for successful hillclimbing」
- 题目里的 unhobbling 指的是：模型本身没问题，是你的配置把它「绊住」了（我的理解）

## 开场：大多数应用在 frontier 曲线「里面」

- slide「Most applications sit inside the frontier」：cost–performance 图上有一条 Pareto frontier（efficient configurations），「your application」是曲线内侧的一个红点，向左平移到曲线上叫「a "free" win (lower cost, no performance drop)」
- 讲者说 [00:00]：「many times, you're not actually on this Frontier, your application is actually sitting inside this Frontier that you could be on, and it's because of minor [misconfigurations]」（转写为 "disfigurations"）
- 五个主题：审计 prompt、用好 prompt cache、eval 设计、hillclimb、为长时任务设计 harness。每个主题对应 Claude Code 里 `claude-api` skill 的一条命令

## 1. Prompt 审计：prompt 会像代码一样欠债

**slide「Prompt instructions accumulate as "debt"」，三类七种：**

| 类别 | 反模式 | slide 上的说明和数字 |
| --- | --- | --- |
| They tax cost | Verification rituals | 「verify twice before answering」— measured **+48% cost per ticket, zero accuracy change** |
| | Thoroughness boosters | 「be maximally thorough」— unneeded tool calls per ticket go **0.07 → 0.95**, old model vs new |
| | Procedure & scratchpad scaffolds | step-by-step templates that stack on top of the model's native thinking |
| | Worked-example stuffing | few-shot examples that teach long reasoning chains the model doesn't need |
| | Anti-laziness tool boosters | 「always check the docs first」— forced lookups with nothing to look up |
| | Dead weight | runbooks for features you no longer have — read and paid for every request |
| They break accuracy | Contradictory rules | two rules added months apart; the model quotes both and freezes — measured **−9 accuracy points from one planted pair** |
| They break outright | Dated configuration | settings for an older API generation — rejected or silently reinterpreted |

- 讲者说 [01:15]–[01:48]：旧模型 instruction following 不够强，遇到两条矛盾的规则会忽略其中一条，「so it kind of still worked」；frontier model「will often get confused by contradicted rules」
- 命令（slide）：`/checkup prompt-audit` 和 `/claude-api prompt-audit`。讲者说 [01:48]–[02:18]：两个是不同的 skill 入口，底层是同一份 skill。`/checkup` 用来检查 Claude Code 自身的配置（skills、prompts），`/claude-api` 用来检查调用 API 的代码。会检查 skills、tools、prompts
- slide「often can improve performance and reduce cost」：50 张客服 ticket 的 eval，模拟从 Opus 4.8 迁到 Opus 5.5，逐个注入反模式后取平均：
  - Opus 4.8 legacy：2.45¢ · 88.1%
  - → model upgrade → Opus 5.5 unaudited：2.02¢ · 90.0%
  - → prompt-audit → Opus 5.5 audited：1.83¢ · 92.0%
- 讲者说 [04:29]：「Run [it on] any code using the API」；在 Anthropic 内部用得很多

## 2. Prompt cache：不动性能的省钱

- 讲者说 [04:42]：agent 跑很多轮，每轮只加一点，大部分 context 重复，「You don't want to pay for it twice」。cache「billed at like a tenth the rate of base tokens … you can check the pricing page」，也更快
- 讲者说 [05:26]：**strict prefix match**，「even a byte change in your prefix will invalidate the cache」。Claude Console 能看 cache hit rate；Claude Code 团队「track the prompt cache hit rate very, very religiously」
- 讲者说 [06:11]：prompt 的组织顺序：system instructions 和 tools 全局缓存 → `CLAUDE.md`/memory 按项目 → session state 按 session → messages 每轮递增（转写为 "Cloud and VM memory"，【待确认】是否就是 CLAUDE.md memory）
- 命令：`/claude-api cost-optimize`（slide 标题）。讲者说 [07:11]：除了 cache，还会查 effort 和 prompt
- slide「cost-optimize can reduce cost by fixing prompt caching」，用默认 harness 跑内部 benchmark：

| Benchmark | 成本变化 | 金额 | 准确率变化（95% CI） |
| --- | --- | --- | --- |
| tau2-bench (retail)，customer-service agents | −72.9% | $48.68 → $13.20 | +2.6pp（−2.1 到 +7.3） |
| LegalBench，legal classification | −66.9% | $38.46 → $12.74 | **−0.8pp（−1.3 到 −0.3）** |
| OfficeQA Pro，document Q&A | −71.7% | $252.14 → $71.26 | +1.5pp（−4.0 到 +7.0） |
| SWE-bench Verified，agentic coding | −24.3% | $14.42 → $10.92 | +1.1pp（−2.6 到 +4.8） |

- 讲者说 [06:41]：「reduce costs up to 72%, but no meaningful change in performance」；[07:43]「It's like [essentially] just pure cost saving」
- **出入（我的理解）**：LegalBench 的 CI 整段在 0 以下，是一个小但统计上显著的下降。「no meaningful change」对三个 benchmark 成立，对 LegalBench 说法偏宽。−0.8pp 很小，但「纯省钱」不是在每个 benchmark 上都绝对成立

## 3. Eval 设计：给 Claude 一个爬得动的 eval

- 讲者说 [08:00]：「Frontier Claude in particular is very good at self-correction. So if you give Claude a well-structured evaluation, it can basically just hill climb in that eval」；前提是 eval「climbable」
- slide（无标题的四要素图）：x 轴 tokens per attempt，y 轴 score，三条曲线 frontier / mid-size / smaller model，三档 low / medium / high effort：
  1. performance improves with a more capable model
  2. performance improves with more thinking (higher effort)
  3. headroom left to improve
  4. low run-to-run variance
- 资料（博客）在四点之前还有一条：**eval tasks mirror production**；并补充：headroom 不能来自 impossible 或 ambiguous 的题，「a common tell is that a task fails every evaluation run, regardless of the number of replicates」；好题的标准是「two domain experts would reach the same verdict」；variance 也可能藏在配置和环境里（effort 没统一，上一次 trial 留下的文件或 git history 把答案递给 agent）
- 讲者说 [09:36]：很多流行的 eval 里都有 ambiguous 的题，「it looks like … I have 20% more than I can get on this, but no model can actually achieve it」
- 命令：`/claude-api build-eval`。讲者说 [10:06]：「not opinionated at all about where your data lives」，可以接任何 tracing 方案；给它问题、关注点和原始数据（traces），它会「show you the data, show you the evaluation, sanity check the grader, the criteria with you」
- 资料（博客）build-eval 的流程：
  - 样本来源的优先级：production transcripts（先问数据保留和敏感数据）→ bug reports 和 support tickets → 5–10 个手写 case → 从代码库合成
  - grader 选最便宜的那种：输出空间受限就用程序检查（exact match、固定标签集、JSON schema、测试通过）；开放式就用 LLM-as-judge，rubric 写成「checkable claims (not a 1-to-5 scale)」；有 baseline 时让 judge 盲比、随机顺序；judge 不能是被测的模型
  - 先给几个 case 打分让你确认；「read a sample of scored transcripts before believing your evaluator」
  - 跑 baseline 时的诊断：grader 对同一输出跑两次看 verdict 是否变；查 timeout、API error、截断；baseline ≥ 约 95% 就提示改去优化成本或延迟

## 4. Hillclimb：最大的风险是 overfitting

- 讲者说 [10:57]：「Frontier Claude models are very, very effective at hill climb … without you in the loop」
- slide「Beware of overfitting」，benchmark 的特征如何塑造 harness：

| The benchmark | → The harness |
| --- | --- |
| task mix needs OCR | + OCR tool |
| tasks live in /app | + always cd /app, run pytest |
| distinctive phrasings | + prompt tuned to phrasings |
| failures you've read | + one patch per failure |
| public repo with answers | + curl the reference solution（虚线：the outright leak） |

- 讲者说 [12:13]：「You run into production and performance is not better at all. That's frustrating because you spend a lot of money」
- 命令：`/claude-api hillclimb`。讲者说 [12:43]：「of course, not perfect. You have to audit this carefully」
- slide（hillclimb 流程图）：Surface（the thing we edit, e.g. a prompt）→ Application（model + harness, held fixed）+ Eval（**Test split: held out, never shown to the analyzer, scored every round**；Train split）→ Decision：train↑ test↑ keep the diff；train↑ test flat = overfit, revert；either↓ = regression, revert。Analyzer「reads train failures, forms a hypothesis」，proposed diff「one change per round」。Pre-flight：「baseline × N reps sets the noise floor」
- 讲者说 [13:13]：分析器「will only ever look at the train split」；「If you see train improving but test flat, it's a signal that you might be overfitting」
- 资料（博客）防 overfitting 的三条：split the cases；「Never paste failures into the prompt」；「Keep the answers structurally out of the model's reach」（reward hacking）
- 资料（博客）选 hillclimb surface 的三条：cheap iteration、attributable（例：skill triggering，指标是 trigger rate，直接耦合 skill description）、well-scoped objective
- 资料（博客）卡住时：「When the score stalls for two or three rounds, Claude reads each remaining train failure and sorts it by cause」；结束时报告 test 结果和置信区间，「If the gain is within noise, it says so and recommends against merging」

**案例一：hillclimb `claude-api` skill 本身**（slide + 资料）

- 基于官方文档的 eval；给 hillclimber 文档和 SDK 的访问权限
- 66.1% → 74.3% → 74.0% → 77.5% → 77.2% → 80.1% → 84.0% → **87.9%**（约 24 轮），三个阶段：
  1. adding missing sections and type tables（博客：缺 8 个功能的覆盖，补上到 74%；C# 和 Java 类型表错误，修完到 77%）
  2. fixing how the skill tells Claude to write code（博客：卡了两轮后按根因分桶，发现内容都在，但 Claude 按训练先验写旧版 API，比如固定 budget 的 extended thinking；在 skill 顶部加了一张「旧写法 → 新写法」对照表，到 80%）
  3. fixing graders, plus more skill edits（博客：始终不涨的题说明题或 grader 有问题）
- 讲者说 [14:39] 起点是「68%」，slide 和博客是 66.1%，**以 66.1% 为准**
- 讲者说 [15:11]：「simple text-based [surfaces] are very, very good for this kind of [Claude hill climbing] process」

**案例二：用 hillclimb 降成本**（slide + 资料）

- slide 路径（adopted path · train-split read）：
  1. Opus 4.8 | high effort，baseline，starting accuracy 74.4%
  2. Opus 5 | low effort，move to higher tier & audit prompt
  3. Sonnet 5 | low effort，drop to Sonnet 5 at low effort
  4. Sonnet 5 | low effort，improve prompt
- 资料（博客）的数字：44 张 ticket，30 张用于搜索、14 张留出。Opus 4.8 high 74.4% / 4.6¢ → **Opus 5.5** low（审计 prompt 后）87.8% / 1.9¢ → Sonnet 5 low 88.9% / 1¢ → Sonnet 5 low + 改进 prompt（routing rules 和 refund-cap cross-reference）98.9% / 1¢。**14 张留出 ticket 上 90.5% vs 原配置 78.6%，成本约为原来的五分之一**。Opus 5.5 的 input/output 比 Opus 4.8 便宜 20%，cache read 便宜 60%
- 讲者说 [16:17]：「we basically got a 90% reduction in cost」。**出入**：按博客 4.6¢ → 1¢ 约 −78%，留出集约五分之一。以博客为准
- **【待确认】** 第 2 步的模型：slide 写「Opus 5」，博客写「Opus 5.5」，录音转写是「Opus 5.0.hi」听不清
- 讲者说 [17:17]：eval 已经饱和也能用，「I don't care about performance, I just want to reduce cost」；经验：**更强的模型配低 effort，往往比弱模型配高 effort 更便宜也更好**，「It's like giving someone who's smarter, [they] can try less to actually get a better [result]」，这条经验写进了 hillclimb skill

## 5. Harness：为跑几天的 agent 设计

- slide「Task duration (time for a human)」：三个阶段 Autocomplete / Chat → Synchronous local agents → Asynchronous remote agents。数据点：Claude Sonnet 3.5 21 min、Sonnet 3.7 1 hr、Opus 4 1.7 hr、Opus 4.5 4.9 hr、Opus 4.6 12 hr；起点是 Claude 3 Opus
- 讲者说 [18:11]–[18:44]：METR 风格的图（转写为 "meter charts"）；Claude 3 Opus「capable of only, like, four minutes of autonomous work」；2025 年到了小时级；现在「exceeding, like, a day to two days … outside the scope of what [METR] can even measure now」
- 讲者说 [19:15]：Claude Managed Agents 的原则是「decoupling the brain from the hands」，他四月写了那篇 engineering blog（资料：[Scaling Managed Agents](https://www.anthropic.com/engineering/managed-agents)，2026-04-08）
- slide（架构图）：Harness 居中，周围 Session、Sandbox、Tools（+Resources/MCP）、Orchestration
- 讲者说 [19:47]–[20:53]：agent 跑一整夜，sandbox 半夜挂了，「You lose state, you lose your session log, everything」。所以 session log 是独立对象，append-only、可恢复；harness 或 sandbox 挂了都能通过 orchestrator 重启。安全：「never gives Claude direct access to secrets」，secrets 在 vault 里
- 资料（engineering blog）原文：session 是「the append-only log of everything that happened」；container 挂掉时「the harness caught the failure as a tool-call error and passed it back to Claude」；harness 崩了可以从 session log 恢复，「nothing in the harness needs to survive a crash」；凭据「held in a vault outside the sandbox」，「The harness is never made aware of any credentials」
- slide（agent automation 架构）：Schedule（scheduled deployment）唤醒 Agent（model + prompt, the run loop and the judgement rules）；Sources read-only，via MCP or HTTPS，credentials from a vault；Guardrails：caps on turns, time and spend + heartbeat that someone watches；Already posted? → Destination（one tool, one place, approved in advance）→ Reader；Memory 分两块：State（the agent writes it：a bookmark per source, what it has said, a log of every run）、Preferences（the reader writes, the agent reads：scope, length, who gets notified, quiet days, when to stop）
- 资料（[Building effective agent automations](https://claude.dev/blog/building-effective-agent-automations/)，Lance Martin、CJ Avilla，2026-10-08）：参考实现在工作日 7:32 AM（纽约时间）跑，读 Slack 频道和 GitHub PR，发一份 brief 到一个 Slack 频道；Slack 确认发出后才更新记录；每次运行有花费上限
- slide 上的命令：`/claude-api managed-agents-onboard https://claude.dev/blog/building-effective-agent-automations/`
- 讲者说 [22:48]：同事现在的工作方式是给 agent 一个 eval，让它跑一整夜 hillclimb，「come back, for example, the morning, and it's significantly improved」

## Q&A 精确版

**Q1 [23:23] eval 设计能自动化到什么程度？多久需要人「下到抽象阶梯底层」去核对和纠正？**
Lance 先讲了他和 Hamel（Hamel Husain，eval 领域的知名顾问）在 Twitter 上的交流：Hamel 公开试用了 build-eval，反馈是这个 skill 应该先带用户看原始数据，Lance 同意，正在改。Hamel 的例子是一个客服应用：面对一堆 traces，第一个问题是「这里有哪些失败？哪些该升级进 eval 集？」。Claude 很擅长扫离线日志、标出可疑样本，「effectively like acting as like an online evaluator but looking at your offline logs」。他已经提了 PR，要加一个中间步骤：先把原始数据放进一个 viewer 给用户看，而不是现在这样直接分析数据、建 eval。结论：「there is still quite a role for the human in the loop there. But Claude [can] assist quite a bit.」
（Hamel 的那条推文没找到原文，【待确认】要不要附链接）

**Q2 ★ [25:39] 能用 human feedback 做 hillclimb 吗？（提问者举的例子是做一个 oracle 式的 agent）怎么做最好？**
「Hill climbing only requires that you have an eval and you have some signal that Claude can use to judge good versus bad.」只要把 human feedback 变成 eval 的信号，就完全可以在上面 hillclimb。

**Q3 ★ [26:19] hillclimb 最适合从哪个 surface 开始？**
从文本开始，特别是 prompt：它是「a very constrained surface to modify」，而且「highly coupled to the output」。他做 skill hillclimb 时，被测的东西直接读 skill，所以耦合很清楚。对比 agent harness：每轮改 harness 代码，diff 大得多，「more surface area for confusion」，改动可能和要测的输出脱钩，更难解读。也有人在 harness 上做了很多 hillclimb，可行但更难。代价是 prompt 最容易 overfit，即使有 train/test split：「prompts are very easy to wiggle. Claude's very good at wiggling the prompt into a shape that does well in your eval that isn't really representative of what's in production.」

**Q4 ★ [28:27] 什么时候该把 prompt 或 SOP 升级成 skill？**
「Claude is quite good at judging that」：先走一遍工作流，再让 Claude 把它变成 skill，「I do that all the time」，「I actually think you should just have Claude do that」。追问「做 eval 时怎么判断」：他的做法是先有一个 skill 的 boilerplate 当起点，然后让 Claude 改 skill 文本、在 eval 上提高分数。例子就是 `claude-api` skill：它本质上是文档的提炼（「go to this page if you want to learn about this」），有很多小坑，所以起始分数很低。「usually with skills, you're kind of starting with some baseline. And it could be like a very crude default … give Claude something to like start to climb from.」

**Q5 ★ [30:33] 产品没有明确的成败指标（UX、用户感受）时，eval 怎么做？**
「That's a classic.」能找到确定性检查就优先用。否则用 LLM-as-judge，四条：
1. 先写一份你作为人自己能拿来打分的 rubric
2. 「don't use arbitrary scales. I see this all the time. Like grade it from zero to five. It's very ambiguous.」尽量用 binary（good vs bad）；要评多个维度，就拆成多个 binary classifier。「the worst thing is [telling] Claude, like from one to ten, rate this. That's extremely noisy.」
3. 跨模型检验：「Have Sonnet, Opus, and Fable all grade using your rubric. If they all disagree, that's like a bad sign.」
4. 跑 replicates，确认 variance 很小
做到这些，「LLM-as-judge with a good, well-structured rubric can be quite effective」。

**Q6 ★ [32:41] 模糊的 eval 用 pairwise comparison、算 ELO 怎么样？**
「Actually, I should have mentioned that. Pairwise comparisons can actually be quite good.」比较两个产物比给一个产物打分容易得多，对人和模型都是：「you can typically answer, oh, I like this webpage better than this one, versus [grading] this webpage between zero and five. It's much harder.」问题允许的话，这是处理模糊打分的有效办法。

**Q7 [33:02] hillclimb 内部做 train/test split，那实践中怎么理解 eval 的分布？**
回到仔细看数据。他说的「分布」是：你抽出来的 eval case 要代表 production 里你在意的情况。要警惕 **adversarial sampling**：只挑某个模型做不对的题，比如看到 Sonnet 5 做不对这些题就全升级进 eval，可是「models are jagged」，Opus 5 可能全都做得对，「a particular pocket of one model might just get [solved] by another」。建议：「Promote cases of evals that are hard or interesting from first principles, from your own intuition, from looking at production data, not necessarily completely sampled from the failures of any given model」，否则 eval 的分布会被某个模型的 jagged 能力塑形，换模型就失效。

**Q8 [35:23] 既然 frontier model 够聪明，为什么不能用分档（bucket）或打分式的 eval，非要 binary？**
「I'm being a little bit overly prescriptive. You obviously can.」只是他看到很多人走得太远：随意的 0–10 分、打分规则不清楚。只要满足前面几条就行：variance 低、跨模型结果一致、人能区分相邻两档。「binary is like a shortcut to simplify the problem usually.」另外打分常常要用更小更便宜的模型，「you really don't want [the most expensive model] doing your grading, depending on your cost sensitivity」（模型名听不清，【待确认】）。想清楚了也可以用 multi-category classification。

**Q9 ★ [36:28] 你和同事会给个人日常用的 skill 建 eval 吗，还是只给公开给很多人用的 skill 建？**
看任务的重要性，因人而异。他自己没怎么给个人自动化建 eval。但他举了个例子：「if you say you were in finance and like you've automated parts of your personal workflow, but quality is extremely important, like this is a work output that's going to [be shared] externally or something」，那即使是自己做的工具，也绝对应该建 eval。管日历的个人助手就不一定。

**Q10 [38:05] （提问没录清，从回答推断是「该用哪家 eval / tracing 工具」）**
「I actually work[ed] at LangChain [before] Anthropic」，所以不替任何一家站台（转写为 "I actually work at Langchain for Anthropic"）。他用过很多家。小规模的话「you obviously can roll your own」，不需要复杂的系统。他们正往 skill 里加一个简单的 data viewer；Claude 很会搭 UI 和可视化，针对你的问题做一个专用的看数据的 app 很容易，也不贵。

**Q11 [38:59] hillclimb 保证收敛吗？会不会卡在局部最优？**
「It's not guaranteed to converge at all.」他见过很多次卡住。所以 hillclimber 里有一个 fallback：连续几轮卡住，就把剩下的失败 case 聚类，找原因。常见情况是没先跑 build-eval、eval 集没仔细建，「maybe that last 20% are all faulty grader examples, ambiguous instructions. This happens all the time.」卡住「not because Claude can't do better, it's because the eval is poorly structured」。他那个 skill 的案例里，第二阶段就卡住过，聚类后找到一批模糊的题并改写了。
资料（博客）：「When the score stalls for two or three rounds, Claude reads each remaining train failure and sorts it by cause」，这一步不改代码，只分桶。

**Q12 [40:29] DSPy 这类优化框架在这里有用武之地吗？**（转写为 "GS5" / "DSPi"）
他很尊重 DSPy 团队和 Omar（Omar Khattab）。这只是 Claude Code 里的一个 skill，而且开源，可以把它 vendor 到别的地方。「just some very simple, general instructions for setting up the hill climbing loop」，所以可能和 DSPy 兼容。他自己没用过 DSPy，不清楚细节。

**Q13 [41:28] 同一个模型有 5 档 effort，怎么选？**
「Effort's confusing, isn't it? … I'm also confused by effort.」有 eval 的话，hillclimber 可以替你搜 model × effort。原则：「typically, the stronger model at lower efforts is … more token efficient and therefore cheaper than a weaker model at high effort」。做法是先在 low effort 下依次试不同模型，直到性能掉到 baseline 以下。例子：他试过 Haiku 4.5 low effort，明显退化，就没采用（他补了一句现在有 Haiku 5.5，好很多）。
没有 eval 时的通用建议：「try strong model low effort … Opus 5.5, low effort. Try that.」
追问「Low, not medium?」：「try low to start. You can always dial back up … I like very smart model not running too hard」，这常常是成本和性能的甜点。很多人一上来就开 max effort，「It's extremely token hungry」。
资料（Effort 文档）：五档是 low、medium、high、xhigh、max；**Opus 5.5 的默认是 medium**，所以「Opus 5.5 + low」比默认还低一档；文档建议「Run an effort sweep on your own evals rather than carrying settings over from an earlier model」

**Q14 [43:59] effort 到底是什么意思？**
「basically kind of call it how hard to think」。low 是「别想那么多」，往上调是「想得更多」。所以更聪明的模型配 low effort，在更高的智能基线上花更少的 thinking token；便宜的模型（比如 Sonnet 5）配 high effort，会花很多很多 token 在思考上。（他本想打开博客演示，现场没网。）

**Q15 [44:55] 有没有遇到 eval 预算失控？（提问者说他们定时跑测试，DBU 预算失控，只好降低测试频率）**
skill 会先问你预算，然后帮你跟踪 hillclimb 的花费。失控确实会发生，这正说明 eval 设计重要：如果最后 20% 根本不可能做对，「You burn a huge number of tokens trying to solve those final, you know, 20% of problems that are effectively not solvable.」

**Q16 [45:50] 人在 loop 里定义成功标准、写 eval，Claude 也能帮写 eval，而且会越来越强。人和 Claude 的分工以后会怎么变？**
人负责 steer：「What are you actually trying to evaluate? And why is it important? So it's kind of taste」。又拿 Hamel 公开试用 build-eval 的客服应用举例：什么重要、你在意什么，由人来告诉 Claude；说清楚之后，Claude 看原始 traces，找出应用在哪里没达到目标。这和 tracing 系统里的 online evaluator 一样：你告诉它在意什么，它标出可疑的 trace。人 review 这些 case，再基于它们建 evaluator，「That's typically a very nice marriage between Claude and human when it comes to building evals.」

**Q17 [48:00] eval 通常由谁负责，产品团队还是工程团队？**
看问题。「At Anthropic, everyone is empowered to build evals」，不按团队划分，通常按产品 ownership。这类 skill 降低了门槛，能建 eval 的人多了很多，Anthropic 里很多不同角色都在建。eval runner 之类的工作 Claude 都能做，人负责的是 taste 和方向。

**Q18 [49:05] 能举一个可验证性较弱的领域里的 eval 例子吗？**
generative UI 的 eval：用 pairwise judge 比较配置 A 和 B 哪个生成的 UI 更好，跑很多次 trial 汇总得分。另外 retrieval、写作、给 trace 打标签（这个好还是不好）这类任务，LLM-as-judge 都极其常见。

**Q19 [50:10] 这和 Karpathy 的 autoresearch 是什么关系？**（转写为 "auto research"）
hillclimb 的很多动机就来自 autoresearch：模型很会自我修正，给它一个结构清楚的问题就能爬。Karpathy 的例子里目标是可验证的，比如把 loss 降下来（他说记不清具体问题）。这条命令是为「eval + split」的 hillclimb 设计的，autoresearch 是建一个产物然后看 loss，但改一下应该也能用。
追问「能从 hillclimb 扩展到 autoresearch 吗？」：只是问题的形式不同，循环是一样的：跑实验、看结果、调整。「if you had, like, a nanoGPT repo, you could use this skill with it, I'm sure.」核心直觉相同：模型很会自我修正。
（我的补充，【待确认】：据二手资料，autoresearch 的指标是 val_bpb，repo 在 github.com/karpathy/autoresearch，没核对 repo 本身）

**Q20 [53:04] 一个指标的提升以牺牲另一个为代价怎么办？能找到两全的点吗？**
降成本那个案例就是同时看 cost 和 performance。可以让 Claude 做多目标优化。追问最多能串几个目标：他没推太远，通常看 cost、performance、speed，「That feels like a pretty reasonable trifecta」。目标越多问题越难，「Maybe even think about kind of changing one thing at once, doing it serially.」

**Q21 [54:21] prompt audit 针对特定模型吗？Sonnet、Opus、Haiku 都适用吗？**
prompt audit 是今天讲的东西里最简单、最该先试的。这些反模式对 5.5 这一代 frontier model「definitively」不需要了；往前推到哪一代还适用，界线不精确，「maybe like Opus 4.5 is the point at which these things start to become less relevant」。他的建议：迁到 Opus 5.5 时就做一次。
追问 Opus 5.5 和 Sonnet 5.5 的区别：skill 里有一些针对模型的说明。比如 Haiku 可能没那么自主，也许需要多一点 booster，「I don't know if that's true. I'm just kind of suggesting something.」

**Q22 [55:57] （回到 effort，他主动补充澄清）effort 是 token 的硬上限吗？**
「It is not a hard cap.」它告诉 Claude 该想多少；即使开很高的 effort，简单问题也不会乱花 token。差别出现在难题上，比如 frontier coding 问题：xhigh 或 max 和 medium/low 相比，token 用量可能差很多，结果却差不多，「because Claude will do more work to verify the result」。所以在 frontier 难度的问题上要更小心选 effort。
资料（Effort 文档）原文：「Effort is a behavioral signal, not a strict token budget.」

**Q23 [57:32] 用 eval 让模型自我修正，是不是和神经网络里 backprop 加 loss function 的 hillclimbing 很像？**
很像训练：给模型一个 objective，它去改系统来提升。连 train/test split 这种训练时的原则都用上了。区别是「these are kind of run-time modifications you can make」：模型部署之后，在你自己的领域里改 prompt、skill 这类 surface。

**Q24 [58:42] （提问前半段缺失）Claude 意识到自己在被 eval（eval awareness）对这里影响多大？**
Anthropic 在这方面发表过不少东西，在训练的不同阶段确实是一个问题。但在这个场景里「the bigger concern … is typically more overfitting」：Claude 会在你给的 eval 分布上找到让数字上涨、但对 production 没用的 prompt 技巧，「that happens all the time」，这不是因为它意识到被评估，而是「you've given it a prompt and Claude can find ways to shape this prompt to make the number go up and it'll just do that」。所以用 train/test split 来做正则化。
（Anthropic 的相关研究：二手报道提到 Sonnet 4.5 system card 和 NLA 研究，没找到一手原文，【待确认】是否引用）

**Q25 [1:00:11] eval 数据集最少需要多大？**
没专门研究过，「frankly, just ask Claude」：跑这条命令时，Claude 有估算 eval 规模的启发式，skill 里也有指导。样本太少的主要问题：容易 overfit、variance 大、可能不代表 production。
资料（博客）：build-eval 会告诉你 eval 的规模（cases × repeats × model，以及大约要跑多久）；hillclimb 开始前先检查噪声是否小于你会采取行动的最小改进，不够就建议加重复或加 case。

**Q26 [1:01:40] 单个任务要跑 14 小时这种 eval 怎么 hillclimb？（提问者的比喻：不能每改一次就在回测上把模型完整重训一遍）**
这正是他偏好 prompt 这类改得快的 surface 的原因。超长任务也能做，但 hillclimb 会难很多，等信号的时间长得多。这就回到第五节的 harness：需要能连续可靠运行很多小时的 harness，内部用的是 Managed Agents，「it just becomes a bigger infrastructure lift」，要处理基础设施故障。

**Q27 [1:02:58] agent 的 eval 该看单轮，还是看整条 trajectory？**
做 agent 通常需要 multi-turn eval。build-eval 可以看 traces、把某些 trace 升级进 eval 集。打分有很多流派：看整条 trajectory，或者只看最终结果，命令里有相应的指导。

**Q28 [1:03:55] context engineering 里，什么真正提升可靠性，什么被高估了？**
他推荐 RLM（Recursive Language Models）的思路，Managed Agents 也用了这个思路：「the context object is independent from the harness」，harness 通过工具去查询这个独立存放的 context 对象。好处是可恢复：如果只是一串 messages，「you do a compaction event, and everything you compacted is gone」。做长时 agent 时你不知道将来什么会重要，有了独立的 session log 就随时能回查。（他只回答了「什么有用」，没回答「什么被高估」。）
资料：RLM 论文是 Alex L. Zhang、Tim Kraska、Omar Khattab（MIT CSAIL），arXiv:2512.24601，2025-12；做法是把长 prompt 放进 REPL 环境里的一个变量，让模型写代码去查、递归调用子模型

**Q29 [1:06:13] 提问者念了一段他 6 月和一个 Opus 4.8 agent 的对话：「I'm optimized to produce fluid, forward-moving, plausible output, and plausible is not verified.」他说 Codex 也说过类似的话。问：我们现在的设计是不是在迁就模型被优化出来的方式？这会变吗？**
这就是要在 eval design 上花大力气的原因。模型是非确定性的，很多场景的结果不容易验证，「not everything is a [verifiable] problem」。在这些场景，好的目标、清晰的打分标准、多模型交叉验证、低 variance 都非常重要。「if you … tell the model, okay, optimize performance on this very poorly structured evaluation, it won't go anywhere, and it's not the model's fault」。这也是题目 unhobbling 的含义：前期把 eval 设计好，才能真正用出 frontier model 的能力。

**Q30 [1:09:14] 模型变得太快，今天做的东西和一年前完全不同，团队总在用昨天的模型的思路做事。怎么跟上？**
模型变强时，你的 eval 分数应该跟着涨（就像四要素图那样）；如果没有，「I would go back and question the eval structure」。另一面是模型很快把 eval 做饱和，需要不断出新的、更难的 eval，而现在要设计出能难住 Fable、Opus 5.5 的 eval 本身就很难，「a recurring challenge」。

**Q31 [1:11:17] 值得在外面再套一层，持续优化 eval 本身吗？**（问题转写不清，【待确认】）
他在各家 tracing 公司那里看到这样的流程：线上 agent 产生 traces → online evaluator 打分 → 低分的 trace 被拉进离线 eval 集 → 在上面 hillclimb → 全部通过后放进 regression tests → 继续循环。「That's like a very good pattern, I think.」
（录音到 [1:11:57] 截止，后面可能还有问题没录到。）

## 勘误表（Whisper 转写 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| Claud / Cloud / clock / claw / quad | Claude | 上下文 |
| plot code / cloud code | Claude Code | slide |
| checkup prompt / cloud API prompt | `/checkup prompt-audit` / `/claude-api prompt-audit` | slide |
| email / email set / emails | eval / eval set / evals | 上下文 |
| disfigurations | misconfigurations | 上下文 |
| OpenSky5 / Opus 5.0.hi | Opus 5.5（第二处见案例二的【待确认】） | slide、博客 |
| Sonic 5 | Sonnet 5 | 上下文 |
| icon / climb（模型名） | Haiku | 上下文 |
| meter charts | METR | 上下文 |
| Asian artist | agent harness | 上下文 |
| edge block | engineering blog | 资料 |
| Paralyze comparisons | pairwise comparisons | 上下文 |
| GS5 / DSPi | DSPy | 上下文（Omar = Omar Khattab） |
| Hamel / Hamill | Hamel Husain | 上下文 |
| auto research / nanoGDP | autoresearch / nanoGPT | 上下文 |
| online deviler | online evaluator | 上下文 |
| Fables | Fable（Claude 模型） | 上下文 |
| 68%（skill 起点） | 66.1% | slide、博客 |
| 90% reduction in cost | 约 −78%（4.6¢ → 1¢），留出集约五分之一 | 博客 |

