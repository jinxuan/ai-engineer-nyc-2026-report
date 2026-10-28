# Your agent fix just broke something else：精确版

> Precise notes for this session, compiled from the official video captions where available, the author's room recording, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材整理完，正文逐节和 slide、视频字幕、录音对过。这场没有 Q&A（讲者结尾说「If you have any questions, I'll be in the back」）。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| 官方视频 | [v5tm5EAzsAs](https://www.youtube.com/watch?v=v5tm5EAzsAs)，2026-10-24 上传，标题「Your Agent Fix Just Broke Something Else - Sofia Jakovcevic, Arize AI」。正文 [00:12]–[19:15]，**没有 Q&A**。自动字幕是正文的主要依据，时间标「视频 mm:ss」。视频描述带章节（见下） |
| slide | 作者拍的 15 张（IMG_0478–0492），全部属于本场，没有别场的照片。其中 0478–0485、0490–0492 是 slide，0486–0489 是 Arize 平台 demo 的屏幕。0479–0481 是同一页逐步展开。**Step 01、Step 04、Step 05 和 Part 02 的 slide 没拍到** |
| 一手资料 | Arize 博客 [AI agent regression testing with Arize AX](https://arize.com/blog/ai-agent-regression-testing-with-arize-ax/)（Nancy Chauhan、Fuad Ali，2026-09），同一个 0.89 → 0.72 的例子，但场景不同（见第 1 节）；Arize 文档 [CI/CD for automated experiments](https://arize.com/docs/ax/improve/ci-cd-for-automated-experiments)；[Arize-ai/arize-skills](https://github.com/Arize-ai/arize-skills) |

视频章节（视频描述原文）：00:00 Intro: observability for agents · 01:08 The real problem: fixes that break other things · 02:26 A one-line prompt change: +17 and -17 · 04:48 Agents fail at runtime, not in code · 06:25 The five-step regression loop · 06:48 Step 1: Build datasets from production traces · 07:50 Step 2: Score with evaluators · 09:48 Steps 3-4: Baseline experiments and comparisons · 11:14 Step 5: Gate merges in CI · 11:48 Demo: experiments in Arise · 15:38 Using coding agents and skills · 16:12 Closing the loop at agent speed

## 基本信息

- 时间地点：2026-10-13 3:20–3:38pm，Turing Room (Riverside Ballroom)，Breakouts，Evaluations Track
- 讲者：Sofia Jakovcevic，Arize AI。slide 署名「Sofia Jakovcevic · Forward Deployed Engineer」（IMG_0478）；视频 00:14 自我介绍「I'm a forward deployed engineer at Arize」。日程写 AI Forward Deployed Engineer；prep 时查到 Arize 团队页写 AI Solutions Engineer（见 `prep.md`，没有再核对）
- slide 标题页（IMG_0478）：标签「LIVE DEMO · AGENT OBSERVABILITY & EXPERIMENTATION」；副标题「Turning agent observability into a regression loop that tests every agent change before it ships.」
- 讲者说（视频 00:16）：「Arize is an observability backend that is designed to make AI work」；observability 是「shining a light in the black box」
- 结构（讲者说，视频 01:47）：三部分。一、为什么 agent regression 难发现（agents fail at runtime）；二、五步 regression loop，先概览再在 Arize 平台上 demo；三、「closing that regression loop at agentic speed」，以及 agent observability 的下一步（她预告的「paradigm shift」）

## 1. 例子：一行策略修改，安全 +17，完成率 −17

- slide（IMG_0479–0481）「**The fix that broke the agent**」：「A one-line policy edit made a support agent safer. It also stopped finishing tasks, and the average didn't move.」右栏 BASELINE → CANDIDATE：

| 指标 | Baseline | Candidate | 变化 |
| --- | --- | --- | --- |
| Action safety | 0.81 | 0.98 | +0.17 |
| Task completion | 0.89 | **0.72** | −0.17 |
| Average of both | 0.85 | 0.85 | 0.00 |

- 讲者说（视频 02:29–02:48）：security engineer 看了 support agent 的 policy，要加一行，「ask your agent to confirm before it takes any action」。「Sounds simple. Sounds harmless.」
- 讲者说（视频 03:11）：「We're really coding on vibes, and we don't want to be」，要用「numeric evaluation suite backed data」
- 讲者说（视频 03:28–04:15）：改动上线一周后在 production 数据上跑 eval，action safety「17-point improvement」，task completion「17-point degradation」。原因：每个动作前都要确认，「If a user leaves the session, that session is never completed to the end」
- 讲者说（视频 04:23）：「ironically enough, the overall average of both is net zero … just because you improve the dimension of one agent does not mean that you're improving the agent as a whole」；「would you have shipped this change?」
- 资料：同样的 0.89 → 0.72 出现在 Arize 博客（Nancy Chauhan、Fuad Ali，2026-09）。但博客的场景是 Wonder Toys 电商 agent，原问题是不问订单号和确认就取消订单，新策略要求购买和取消前确认；博客**没有给 action safety 的具体数字**。台上 0.81 → 0.98 和平均 0.85 → 0.85 只见于本场 slide（我的理解：同一组 demo 数据换了个包装，【待确认】是否同一套实验）

## 2. Agents fail at runtime：diff 说改了什么，trace 说发生了什么

- slide（IMG_0482，Part 01）「**Agents fail at runtime**」：「The code you changed is rarely the place the failure shows up」
- slide（IMG_0483）「**Why agent regressions are different**」：「Code changes are local, but behavioral effects aren't. Small changes can reshape the entire execution path. The diff tells you what changed → The trace tells you what it did」
  - 左「WHAT CODE REVIEW SEES」：`system_prompt.md`，`- 1 line` / `+ 1 line`，「Small change, easy to approve.」
  - 右「WHAT RUNS IN PRODUCTION」：`agent.run` → `llm.plan` → `tool.lookup_account` → `llm.decide_action` → **`policy.check → ask user`**（红色高亮）→ `llm.respond`，「Plans, tool calls, and branches you only see in a trace.」
- 讲者说（视频 05:03–05:40）：code reviewer 看到的是「a one-line change in the system prompt markdown file」；但这一行改变了 agent 的运行：「adding additional steps … changing how the agent works with other sub-agents … changing how the agent calls tools」，这些可能有「thousands of possible trajectories」，「are not in your static code. They're only in your observability back end」
- 讲者说（视频 05:43）：「**the diff tells you what changed, but the trace tells you what actually happened**」；「traces are becoming the new source of truth」
- 讲者说（视频 06:01）：传统软件「logic was written down」，现在「the logic only exists at runtime」；写下来的和实际发生的之间「is filled with silent errors」，所以 regression testing 要从 trace 开始

## 3. 五步 regression loop

讲者说（视频 06:28）：「We're going to start with our production traces. We're going to build evals, run experiments, and then close it all up to be an automatic gate at CI.」五步按视频章节是：dataset → evaluators → baseline experiment → 改一处再比较 → CI gate。

**Step 1：从 production traces 选数据集**（slide 没拍到）

- 讲者说（视频 06:48–07:45）：用真实 production traces，「We don't need to synthesize data」。数据集要包含四类：
  1. 代表平均工作流的 representative 样本
  2. known failures（「often fails at eval metrics」的）
  3. edge cases，「Maybe not as common, but just as important」
  4. **successful workflows that we want to protect**：「one thing that we often see users forget」，因为不只要确认 fix 有效，「we also want to test that we're not going to break something that was already working」

**Step 2：加 evaluators**

- slide（IMG_0484，Step 02）「**Add evaluators**」：「An LLM judge for semantic questions, code for deterministic ones. Score each dimension on its own; a single quality score averages tradeoffs away.」
  - `task_completion` — LLM judge；`action_safety` — LLM judge；`valid_tool_args` — Code
  - LLM-as-a-judge 示例（task_completion）：「You are grading a support agent's trace. Did the agent fully complete the user's request, with no step left unfinished? Answer with one label: `complete` | `incomplete`」
  - Code evaluator 示例（valid_tool_args）：`def valid_tool_args(output) -> bool: return all(TOOLS[c["name"]].accepts(c["args"]) for c in output["tool_calls"])`
- 讲者说（视频 08:06–09:49）：evaluator 把 hallucination、accuracy、tool trajectory、step efficiency 这些「subjective in nature」的指标量化。LLM-as-a-judge 判一个 span、一条 trace 或一段日志，得到 binary classification（0/1），也可以 0–5 或多标签；还可以用 code-based evals 或 ML-based evals。「A code evaluator is great for your deterministic checks, but an LLM as a judge is good for your non-deterministic checks」
- 我的理解：slide 上「Score each dimension on its own; a single quality score averages tradeoffs away」就是第 1 节平均分 0.85 → 0.85 的教训

**Step 3：跑 baseline experiment**

- slide（IMG_0485，Step 03）「**Run the baseline experiment**」：「Run the current agent against the dataset with nothing changed. This is the control every candidate gets compared against.」「Same dataset · same evaluators · no changes」
  - 代码：`experiment = client.run_experiment(dataset_id="support-agent-regressions", task=run_agent, evaluators=[task_completion, action_safety, valid_tool_args], experiment_name="baseline",)`
  - 结果：task_completion **0.89** · action_safety **0.81** · valid_tool_args **1.00**
- 讲者说（视频 10:11）：「you can do this in the SDK, it's just run experiment. You'll pass in that data set, pass in that task, pass in your evaluation suite」
- 资料：Arize AX 文档里 CI 用的 Python SDK 调用是 `client.experiments.run()`（参数 name、dataset、task、evaluators）（[文档](https://arize.com/docs/ax/improve/ci-cd-for-automated-experiments)）。slide 上的 `client.run_experiment(...)` 写法和文档不同，可能是简化示意，【待确认】

**Step 4：改一处，重跑，比较**（slide 没拍到）

- 讲者说（视频 10:24–11:13）：加上那一处改动，「rerun the evaluation」；可以同时试多个 prompt、多个模型。例子：团队说要换「the new Fable model, it's five times more expensive」，「You're not going to switch without proving that you're going to get a good ROI」。「changing the prompt, changing handoffs, changing tools, changing models … All without having to deploy to production」（「Fable」和「five times」是她举的假设例子，不是测出来的数字，我的理解）

**Step 5：放进 CI**（slide 没拍到）

- 讲者说（视频 11:14–11:47）：每个 PR 在 CI（例：GitHub Actions）里跑一次 experiment，对照团队认定重要的 evals，「you don't merge until it meets all of the regression criteria that you have listed」，「no longer pushing on vibes」
- 资料（[Arize CI/CD 文档](https://arize.com/docs/ax/improve/ci-cd-for-automated-experiments)）：GitHub Actions 是支持的 CI 之一（另有 Azure DevOps、GitLab CI/CD、Jenkins、Harness）；文档示例里的门禁是脚本取某个 eval 分数的均值和阈值 0.7 比较，低于阈值就以 exit code 1 退出，让 CI 失败

## 4. Demo：Arize 平台

**实验对比页**（IMG_0486，数据集 `refund-policy-regression`，Examples 100，Experiment Analysis 6）

- 顶部卡片：action_safety **+18 pts**，Best #5 · 99%；task_completion **+6 pts**，Best #4 · 95%
- 实验列表（照片可见 5 行，#1 被遮住）：

| # | 实验名 | Example count | action_safety (Avg) | task_completion (Avg) |
| --- | --- | --- | --- | --- |
| 6 | refund-follow-up | 100 | 0.95 | 0.86 |
| 5 | refund-strict | 100 | 0.99 | 0.58 |
| 4 | refund-generous | 100 | 0.70 | 0.95 |
| 3 | refund-v1-unguided | （被挡） | 0.64 | 0.78 |
| 2 | refund-candidate | （被挡） | 0.98 | 0.72 |

  #2 的 0.98 / 0.72 就是第 1 节的 candidate。#6、#5 的创建时间是 10/12/2026 12:06 AM / 12:05 AM
- 讲者说（视频 11:54–12:28）：「I'm showcasing this exact refund agent」；baseline 之后是「plus 17 points on action safety and minus 17 points on task completion」；每一行是「a different test that I ran with different prompt changes, potentially different model changes」，看哪一个「has the best trade-off」；「overall I'm going to have a plus 18 … points on action safety and plus six on task completion」
- **注意**（我的理解，按照片读数）：+18 和 +6 分别是**两个不同实验**的最好成绩（#5 refund-strict 的 0.99，#4 refund-generous 的 0.95），各自和 baseline 0.81 / 0.89 相比。照片里没有哪一个实验同时做到两项提升：refund-strict 的完成率是 0.58，refund-generous 的安全分是 0.70；最接近的是 #6 refund-follow-up（0.95 / 0.86，安全 +14、完成率 −3）。见「出入」

**Prompt Playground：测单个 agent**（IMG_0487–0489）

- 讲者说（视频 12:36–13:20）：新建 playground（命名「AIE NYC」），「a single agent … is essentially just a single prompt with a set of tools attached to it, a model [and] LLM configurations」。从 prompt hub 拉 prompt 的 version 1，选一个已经挑好 edge cases、known failures、successful workflows 的「100-row data set」，复制一份改一行，两个 prompt 并排跑
- 屏幕（IMG_0488–0489）：prompt `refund-prompt v1` 和 `refund-prompt v2` 并排，模型选的是 `gpt-4o`；system prompt 开头「You are the order-support agent for Northwind Goods. You handle refund requests that arrive from email and chat. You are the last step before the ticket is closed, so every reply must leave the customer with a decision they can act on. Do not ask a teammate to take over, and do not end the turn by saying you will look into it later.」；user 模板字段 Ticket id / Customer message / Order age in days / Reason / Order value；数据集 `refund-policy-playgr…`，100 rows；第一行 ticket `T-032`：「The order arrived but the charger was missing. Order placed 10 days ago.」
- 讲者说（视频 13:41–14:35）：加上 task completion 和 action safety 两个 evaluator；还有很多预置的（hallucination、user frustration、code templates、security checks），也可以自定义。跑完后在 dataset 的 experiment 页里能看 latency、evals、total cost、error rates，跟踪 PR 改动和 playground 跑的结果

**Agent Playground：测多 agent 系统**

- 讲者说（视频 14:40–15:31）：agent playground 通过 HTTP 等方式连接「remote agents」，数据集每一行都调用你的 agent、取输出、跑 evals、打分；可以测「a very complicated multi-agentic system」，包括改 sub-agents 之间怎么通信
- 资料：Arize 博客里的流程同样是建 remote agent 配置（endpoint、headers、input schema）、存 baseline 和 candidate 两个 preset、在 Agent Playground 用同一数据集和同一组 evaluators 跑，再在 Compare 视图里对照

**用 coding agent 和 skills**

- 讲者说（视频 15:32–16:12）：也能用 SDK、REST、CI、CLI，「but my favorite method to do this is actually with coding agents」；「Arize has a public repository of skills that allows you to connect to every feature available in the Arize UI」，这样把 IDE 里 coding agent 写的代码和 observability backend 里实际发生的事连起来
- 资料：[Arize-ai/arize-skills](https://github.com/Arize-ai/arize-skills)，覆盖 tracing、datasets、experiments、evaluators、annotations、prompt optimization，需先装 AX CLI（[Arize 文档](https://arize.com/docs/ax/agents/arize-skills)）。「every feature」是讲者的说法，我没逐项核对

## 5. 闭环：从工程师闭环到 agent 闭环

- slide（IMG_0490，Part 03）「**Closing the loop at agent speed**」：「The same loop, run by agents instead of people.」
- 讲者说（视频 16:22）：「We only have 2 minutes left here」，讲两个 takeaway

**Takeaway 1：every failure should become a test**

- slide（IMG_0491）「**Today, engineers close the loop**」：「The workflow is repeatable. The manual investigation is the bottleneck.」
  - Observe（Trace every agent run in production.）→ Evaluate（Score live traces with the same evaluators.）→ Change（Edit a prompt, model, tool, skill, or policy.）→ Test（Run the regression experiment in CI.）→ Improve（Ship what passes. Fee[d] new failures back in.，照片右侧被裁）
  - 回路箭头：「New production traces become regression cases」
- 讲者说（视频 17:03–17:22）：失败的 case 回到 observability 层，「These new production traces that failed then become new cases in our regression testing suite … **every failure should become a test**. Every failure should make your agents stronger and harder to break.」

**Takeaway 2：agent 自己调查、自己修**

- slide（IMG_0492）「**From Observability to Self-Improvement**」：「Helping humans debug agents → Giving agents the feedback loop to continuously improve themselves」
  - HUMAN MANUAL TRIAGE「The Manual Bottleneck」：Human reads the dashboard, triages, and ships the fix. The ultimate bottleneck
  - AUTOMATED AGENT TRACING「Closing the Loop at Agent Speed」：Agents read traces directly to self-correct and turn failures into improvement evidence
- 讲者说（视频 17:35–18:26）：「what if an agent could do the investigation and propose a fix? … Humans are the bottleneck here」；「observability is no longer about humans understanding agents, it's about agents understanding themselves」
- 讲者说（视频 18:34）：「If your coding agents are pushing PRs and I know they are, they're pushing PRs without actually knowing what's happening in production. That means they're pushing PRs blind.」
- 讲者说（视频 19:02）：「**the best agents don't just learn from failures. The best agents fix themselves.**」

## Q&A 精确版

无。视频在 [19:15]「Thank you」后结束；录音结尾讲者说「If you have any questions, I'll be in the back」，台上没有问答。

## 勘误表（Whisper / 自动字幕 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| Sophia Yakoubovitch | Sofia Jakovcevic | slide、日程 |
| Arise | Arize | slide |
| eight-action safety / a action safety | action safety | slide |
| defamation（录音） | degradation | 视频字幕 |
| improving the vision as of both（录音） | improving the agent as a whole | 视频字幕 |
| has the information（录音） | has the intuition | 视频字幕 |
| system comp marked on file（录音） | system prompt markdown file | 视频字幕、slide |
| this coaching was local（录音） | this code change was local | 视频字幕 |
| true calls（录音） | tool calls | 视频字幕 |
| lucrative or factual（录音） | hallucinated or factual | 视频字幕 |
| L as a judge / LM as a judge | LLM-as-a-judge | slide |
| fable model | Fable（Claude 模型名） | 上下文 |
| 100 node data set（录音） | 100-row data set | 视频字幕、屏幕 |
| test completion / past completion（录音） | task completion | slide |
| pre-built hallucinators（录音） | pre-built evaluators | 视频字幕 |
| bank experiment（录音） | baseline experiment | 视频字幕 |
| 80 speed / a gentle speed / at agent speed | agentic speed | 视频字幕、slide |
| instead of running it by people, we run it by people（录音） | … we want to run it by agents | 视频字幕、slide |
| model tool scale policy change | model, tool, skill, policy change | slide（IMG_0491） |
| shooting those fixes（录音） | shipping those fixes | 视频字幕 |
| agendic engineering | agentic engineering | 上下文 |
| "turned a quantitative measurement into a qualitative score" | 讲者口误，意思应是把 qualitative（主观）指标变成 quantitative 分数 | 上下文（我的理解） |

## 出入

1. **demo 里的「+18 / +6」不是同一个实验**：讲者说「overall I'm going to have a plus 18 … on action safety and plus six on task completion」（视频 12:22），听起来像一个配置同时做到。按照片，+18 来自 #5 refund-strict（安全 0.99，但完成率 0.58），+6 来自 #4 refund-generous（完成率 0.95，但安全 0.70）。这两个数字是平台卡片上各指标的「Best」，不是一个实验的结果（我的理解，根据 IMG_0486 读数；#1 那一行被遮住，没法确认 baseline 在表里的值）
2. **0.89 → 0.72 的例子出处**：简介和 slide 的 0.89 → 0.72 与 Arize 博客一致，但博客是 Wonder Toys 购物 agent 的取消订单场景，台上是 support / refund agent（屏幕上是 Northwind Goods 的 order-support agent）；action safety 0.81 → 0.98 只在台上出现
3. **SDK 写法**：slide 写 `client.run_experiment(...)`，Arize AX 文档是 `client.experiments.run(...)`
4. **讲者头衔**：slide 和视频是 Forward Deployed Engineer；日程是 AI Forward Deployed Engineer；prep 时看到的 Arize 团队页是 AI Solutions Engineer

## 【待确认】汇总

1. 台上的 refund agent 例子和 Arize 博客的 Wonder Toys 例子是否同一套实验数据
2. slide `client.run_experiment(...)` 是否对应某个 SDK 版本，还是示意写法
3. demo 实验表 #1 行（baseline）的数值（被观众挡住）
4. Step 01、Step 04、Step 05 的 slide 原文（没拍到，只有口述）
5. IMG_0491 最后一格「Improve」的完整小字（照片右侧被裁，只见「Ship what passes. Fee[d] new failures back in.」）
