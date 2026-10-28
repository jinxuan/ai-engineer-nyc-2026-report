# Lies, Damned Lies, and Evals：精确版

> Precise notes for this session, compiled from the official video captions where available, the author's room recording, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材整理完、slide 数字逐一读过并验算。**没有 Q&A**。日程简介承诺的几个核心案例（+5.4 → +0.35 等）**演讲里没有出现**，见「出入」第 1 条。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| 视频 | 有：[Don't Trust Your AI Agents. Test Them Like a Scientist](https://www.youtube.com/watch?v=A3FL_zTQKU0)（YouTube ID A3FL_zTQKU0）。自动字幕是正文主要来源，标「视频 mm:ss」。视频在 [17:04] 掌声处结束，无 Q&A。视频时间比录音时间晚约 15–45 秒 |
| slide | 作者拍的 12 张（IMG_0407–0418），全部属于本场。slide 右下角有页码：拍到 02–09、11、12（无页码）、13、14；**缺 01（标题页）和 10**（讲「1% false alarm → 10,000 reviews」那页） |
| 讲者资料 | 专著 *Engineering Reliable Coding Agents: Evaluating and Operating the System Around the Model*（[arXiv:2608.13867](https://arxiv.org/abs/2608.13867)，2026-08-14；[在线版](https://www.sjarmak.ai/books/engineering-reliable-coding-agents)）；技能仓库 [github.com/sjarmak/coding-agent-workflows](https://github.com/sjarmak/coding-agent-workflows)。slide 每页底注都引用书的章节 |
| 其他一手资料 | Li et al. [Finding the Right Fit](https://arxiv.org/abs/2610.00917)（harness 对比的出处）；Hardy et al. Agent Evaluation Reliability；Omni 文档 |

## 基本信息

- 时间地点：2026-10-13 11:55am–12:13pm，Turing Room (Riverside Ballroom)，Breakouts，Evaluations Track
- 讲者：Stephanie Jarmak。讲者说（视频 [00:12]）：「an AI engineer as of a couple weeks ago at Omni Analytics」，Omni 的平台有「a governance semantic layer to have a shared understanding of business data using our beloved agent Blobby」；之前在 Sourcegraph 一年，「a solutions engineer for Amp, a coding agent」，后来主要做评测平台，「I built a very large public benchmark called CodeScaleBench」；再之前是天文学家
  - 资料：Blobby 是 Omni 的 AI 查询助手（[Omni 文档](https://docs.omni.co/docs/ai/queries)）
  - 资料（日程 bio）：物理学 PhD；开源 agent 编排系统 Gas City 的维护者；Center for Astrophysics | Harvard & Smithsonian 的 research affiliate
- 讲者说（视频 [01:04]）：「It says how to trust your agents, but I think largely this talk is mainly about how to not trust your agents」
- 每页 slide 的底注引用了书的章节号，下面各节一并记录

## 1. 便宜了 28%，但检索失败还在（slide 02）

- slide 02「Lower cost. The retrieval failure remained.」：OBSERVED COST **−28%** on the affected task family；PRIMARY EVALUATION SCORE **Δ +0.0048**（t = 0.27）；ELAPSED TIME **+3.9%**。流程：Search missed relevant evidence → Agent trusted the empty result。底注：「The retrieval failure remained. Little evidence of score improvement does not establish equivalent quality.」引 Jarmak · Engineering Reliable Coding Agents, ch. 18
- 讲者说（视频 [01:11]，录音 [00:55]）：在 Sourcegraph 测「their code search MCP tool」，想搞清楚「why isn't this coding agent actually using the tool」；她改了 agent 的初始 prompt 再跑，「it was 28% cheaper. That's interesting. But … that shouldn't really change any of my decisions … because it still didn't use the tool」；agent 收到空结果后「just trusting that that information wasn't there … purely because it didn't use that tool」

## 2. 换 harness，排名就反过来（slide 03）

- slide 03「The ranking changes with the harness」：Same 63 tasks requiring no GPU. Different harness.

| Harness | Claude Opus 5 | GPT-6 Astra |
| --- | --- | --- |
| OpenHands | 36/63（57.1%） | 31/63（49.2%） |
| PI | 19/63（30.2%） | 38/63（60.3%） |

  「Compare the configuration you will actually run.」底注：One scored run per task / configuration, after infrastructure retries. 引 Li et al. · Finding the Right Fit；Hardy et al. · Agent Evaluation Reliability
- 讲者说（视频 [02:57]）：「from a third-party source where they had 63 or so tasks on Terminal-Bench 4」；OpenHands 下 Opus 5 赢，「when they swapped just the harness itself, then they saw that Astra did a much better job … like 30-plus percent sort of difference」；结论「evaluating the closest configuration that you can」
- 资料（出处论文）：Yixuan Li 等，[Finding the Right Fit: Model-Harness Interactions across Agent Tasks](https://arxiv.org/abs/2610.00917)（arXiv:2610.00917，2026-10-01）。四个可配置 harness（OpenHands、DeepSeek Harness、PI、openJiuwen）× 五个模型（Claude Opus 5、GPT-6 Astra、GLM-5.3、Kimi K3、DeepSeek V4 Pro）× 三个 benchmark（TUA-Bench、ALE-CLI、Terminal-Bench 4），加上 Codex-GPT、Claude Code-Claude 两个原生组合，共 66 个配置；「63-task non-H100 (CPU-only) subset」of Terminal-Bench 4 v4.0.0。论文原文「Claude records 36/63 in OpenHands and GPT records 31/63」，PI「records 19/63 and 38/63」，**与 slide 完全一致**。摘要结论：「Model rankings reverse across harnesses」；vendor 自家 harness 不一定最好；花费更高不一定分更高
- 资料（Hardy et al.）：Michael Hardy、Ruhana Azam、Anka Reuel、Mykel Kochenderfer、Sanmi Koyejo，*Agent Evaluation Reliability: More Tasks Won't (Always) Fix An Agent Leaderboard*（2026-09/10）。用 Bayesian 方差分解看 22 个 benchmark：固定 model-scaffold 系统的排名可靠性 0.935–0.994，「底层模型」排名可靠性只有 0.148–0.841；scaffold 覆盖不足时，再加同类任务也最多提高 0.097。arXiv 编号（二手资料给的是 2610.00651）没在 arXiv 上核对，【待确认】

## 3. public benchmark vs 内部 evaluation（slide 04）

- slide 04「Public benchmarks and internal evaluation」：Benchmark: a standardized reference for comparison。LANDSCAPE AND SHORTLIST：Reference + reported protocol → Candidate shortlist（Results depend on the task mix and evaluation protocol.）；PRODUCT AND PROCESS IMPROVEMENT：Compare on target work → Product/process change，回环（Diagnose, change and retest. Communicate value against a baseline.）。「Internal benchmarks exist. Public benchmarks can be used internally.」底注：「Public and internal describe visibility, not exclusive purposes. The decision determines what evidence we need.」引书 chs. 2–3
- 讲者说（视频 [04:12]）：「an evaluation is you're making measurements to make a decision about your product or process」；public benchmark 用来「have a broader understanding of … the larger landscape and get a short list」，不要 over-index，「because of the spiky nature of all of the training of these different models」
- 讲者说（视频 [04:47]）：「if you are a law firm, you don't want to necessarily be looking at like the Terminal-Bench results unless of course you are looking at incorporating coding agents with your devs」
- 讲者说（视频 [05:16]）：组织内部也可以建 benchmark，「this sort of iterative flywheel process where you are … increasingly sort of hill climbing」

## 4. 围绕决策设计实验（slide 05）

- slide 05「Design the experiment around the decision」：Report quality, reliability, people's time and total cost together。
  - TASKS · Workload coverage：Family A / B / C → task i / j / k
  - REPEATS · Variation between runs：Same task run 1 / run 2 / run 3
  - CONFIGURATIONS · How results depend on the pairing：Model A/B × Harness A/B 四格
  - 底部公式：**Total operating cost ÷ Independently accepted outcomes**
  - 底注：「Repeated runs are not independent tasks. A better relative score does not establish that either system is reliable enough.」引书 chs. 1–2；Hardy et al.
- 讲者说（视频 [05:54]）：任务要能代表真实负载，可以按 family 聚类；她做 CodeScaleBench 的原因是「at the time it was largely like SWE-bench was just bug fixing, but there's a whole other different swath of tasks … across the software development life cycle」；「a model might be more effective at code understanding than it is at actually fixing a bug」
- 讲者说（视频 [06:30]）：要决定跑多少次，「these systems are non-deterministic」；model 和 harness 的组合有「interaction type effects」

## 5. 追踪证据如何变成结论（slides 06–07）

- slide 06「Trace where evidence becomes a conclusion」：Available → Called → Returned → **Interpreted**。RECORD THE SEARCH：Where and how you searched / Gaps or incomplete results / What you couldn't check。SEARCH-LIMITS TEST：Same empty result, make the search limits explicit。EVIDENCE TEST：Same agent and settings, supply known relevant evidence。底注：「A trace tells us where to investigate. These proposed tests have not yet demonstrated a repair.」引书 chs. 2 & 18
- 讲者说（视频 [07:05]）：「retain all of your traces」；先看「was that tool even available? … if it wasn't, then you stop there」，再看有没有被调用、返回了什么，「keep going along the pipeline of available information」
- slide 07「Better evidence can still go unused」：Test the evidence and the agent's use of it separately。Evidence source（CONTROL A：Substitute trusted information）→ Component using the evidence（CONTROL B：Bypass the component using the evidence）→ Outcome。Deployment comparison：Keep non-use and failed attempts in the result。底注：「Simulation result; production effects remain untested. Tool use alone does not identify the tool's causal effect.」引 Alzahrani · Verify Claims, Not Scores（【待确认】，没找到这篇）；书 ch. 2
- 讲者说（视频 [07:57]）：工具终于被用上了，「but then the actual outcome that you care about at the end of the task, like that still doesn't change」；「keep all of these different things in mind like the non-use and the failed attempts … be a detective」

## 6. 审计打分规则和答案 key（slide 08）

- slide 08「Audit the scoring rule and the answer key」：
  - RECORDED OUTPUT：**295** files returned；**6** reference files found；**289** outside the reference set
  - SCORING RULE：RECALL **1.0**；PRECISION **0.020339**（about 2.0%）；ACTUAL F-BETA SCORE **0.025295**（beta = 0.5）
  - PROPOSED CHALLENGES：Known-good answer / Empty answer / Return-everything answer / Valid alternative absent from the key
  - AUDITABLE RESULT RECORD：Scoring rule / Reference version / Run identity
  - 底注：「One output, measured against one reference set. The score was precision-sensitive; intent and prevalence are not established.」引 Jarmak · CodeProbe trial e5d7a4e7 / commit 2495927
  - 我的理解（验算）：precision = 6/295 = 0.020339；F0.5 = 1.25·P·R / (0.25·P + R) = 0.025295，与 slide 一致
- 讲者说（视频 [08:58]）：「I was interested in optimizing for like a better basically like an F1 score balancing for precision in addition to recall」；「in this example was with like grep … Claude went out and got like 295 files returned when it only really needed six」；如果只在乎不漏，「then maybe a recall of one is sufficient」
- 讲者说（视频 [09:37]）：看 public benchmark 的单一分数时要问「what was the actual validator measuring and why did they consider that to be important?」
- 资料：CodeProbe 是讲者的开源工具，用自己仓库已合并的 PR 做评测（[github.com/sjarmak/codeprobe](https://github.com/sjarmak/codeprobe)，来自 prep 记录，未再次打开核对）

## 7. 质量门：flag 和执行要分开（slide 09）

- slide 09「Test whether the gate changes execution」：My approval-queue audit found paths that allowed execution when required components were missing。PROPOSED ENFORCEMENT TEST：Request → **Approval + required checks** → Execution → Observed effect。DELIBERATELY BREAK A CONTROL：Reject or cancel / Remove a required component / Change the proposal after approval。MEASURE OUTCOMES SEPARATELY：Authorized completion / Safe stop / People's repair work。底注：「Deliberately break a control next. Also measure false blocks on legitimate work; this audit did not establish unauthorized production writes.」引书 chs. 4 & 17
- 讲者说（视频 [10:11]）：她在实验一个「software factory」，有 approval queues，「I want to automate as much as possible … but I also do not trust these systems like at all」；审计发现「the agent noticed things were going wrong, but then there was no mechanism in place for it to actually like enforce a stopping situation」；所以要把 agent「flagging something」和「actually enforcing that gate within the code itself」分开

## 8. 人工审核负担（slide 10，没拍到）

- 讲者说（视频 [11:14]，录音 [10:42]）：例子来自「auditing and repairing LLM as a judge failures that were happening in a separate system」，「a 1% false alarm rate. And if all of those … go to a human, that would result in 10,000 reviews」；「does your organization actually have the capacity for domain experts」
- 我的理解：1% 误报对应 10,000 次复核，意味着总量约 100 万条；讲者没说总量，【待确认】

## 9. trace 本身也可能不可信（slide 11）

- slide 11「Some evidence needs another observer」：For security claims, test missed detections, false alarms and blind spots。Model → Harness → Tool → Destination；对应三种记录：**Independent record of model traffic** / Self-written harness record / Record of the resulting state。Own record passed the hash check **28/28**；Independent record comparison detected **28/28**。底注：「28 deliberately introduced changes. Detection beyond this scope is untested; model traffic does not prove execution or destination state.」引 Dai et al. · Hearsay
- 讲者说（视频 [12:21]）：研究者往 agent 里注入安全问题，「they had like a hash check in place to check for like any sort of tampering … everything passed. It didn't notice any of those injected security issues」；用独立记录比对「all of the traffic history」，「then they were able to detect everything」
- 资料：「Dai et al. · Hearsay」没找到。主题相近的一篇是 arXiv:2609.30266《LLM Agents Can Easily Tamper With Their Own Traces》（Qin、Schmotz 等，2026-09-24），建议用 agent 控制不到的独立拦截机制记录 trace。**不确定是否就是 slide 引用的那篇**，【待确认】

## 10. 一次检索升级，她会怎么做实验（slide 12）

- slide 12「A retrieval upgrade: the experiment I would run」：PROPOSED EVALUATION WORKFLOW：Matched target-work comparison → Independent challenge → Run alongside current work. Test state changes in isolation.（Can an independent challenger find the decisive evidence?）AFTER DEPLOYMENT：Completion, repair, latency, cost / Audit accepted work and failures / Check traces against real outcomes / Watch workload and version changes / Owner, intervention, regression tests（Do scores predict real outcomes?）。底注：「Set restriction or rollback triggers before deployment. The monitoring loop and independent-review tests are proposed.」引书 chs. 16–17；Google SRE: monitoring and alerting
- 讲者说（视频 [13:42]）：在 Omni 做评测 harness，看内部生产数据，把用户和 agent 的交互分类：「Q&A, which may be easy questions to harder questions. Or it could be dashboard building and app building」；[14:36]「ultimately like production is truth」；可以在 shadow 模式下收集，「if … you have a higher blast radius」

## 11. 把下一步自主权的决定说清楚（slide 13）

- slide 13「Make the next autonomy decision explicit」（左侧被裁）：PROPOSED AUTONOMY DECISION：Additional action + operating scope + human check removed。[EV]IDENCE BAR：[R]epresentative repeated outcomes；[To]lerable failures and total burden。VALIDATED EVALUATOR + EXECUTION CONTROLS：Accept good work, detect defects and enforce the operating boundary。[H]uman owner: acceptable risk and conflicting business values。三个出口：Expand within the tested boundary / Retain review / **Stop and escalate**（…for missing evidence, a failed gate or changed scope）。底注：「[Domai]n-specific standard to test. Procedures support the work; they do not establish reliability or supply missing evidence.」引书 closing
- 讲者说（视频 [15:22]）：「what level of risk you are going to be willing to accept」；在她的 software factory 里，agent 要写明下一步动作、为什么需要人来判断、操作范围、建议去掉哪项人工检查；要定出「when is it safe to autonomously deploy versus stop and escalate to the human」

## 12. 资源（slide 14）

- slide 14「Evaluation procedures and references」：Coding Agent Workflows（二维码，github.com/sjarmak/coding-agent-workflows）；THREE PROCEDURES：Evaluation design / Harness traceability / Reliability procedures；BOOK AND PAPER：*Engineering Reliable Coding Agents: Evaluating and Operating the System Around the Model*，sjarmak.ai/books/engineering-reliable-coding-agents，arxiv.org/abs/2608.13867
- 讲者说（视频 [16:31]）：「a GitHub repo with a bunch of the different skills … direct relevance to evaluation design and harness traceability and reliability procedures」；「this like monstrous 300-page archive tome that you can point your agents at」
- 资料：arXiv:2608.13867，作者 Stephanie Jarmak，2026-08-14 提交，cs.SE。摘要首句「AI coding agents are commonly evaluated as models but deployed as systems.」；包含 206 条 reliability records 的目录、evidence ledger、五个可复用 agent skills。在线版 19 章，分六部分（评测测量与实验设计 / 评测与打分系统 / 隔离与恢复 / context engineering / 人工审核与问责 / 研究议程）
- 资料：coding-agent-workflows 仓库，MIT，派生自 Everything Claude Code；把 skills、角色、workflow 渲染成 Claude Code、Codex、Amp 的原生配置；「Agent systems」一类是「eval design, traceability, and reliability protocols」

## Q&A 精确版

无。视频在 [17:04] 掌声处结束；作者录音转写到 [16:18] 只有一句听不清的观众插话（"I'll be sure to get one. Yeah, yeah, yeah."，【待确认】是否有人追问资源链接），之后录音结束。

## 勘误表（字幕 / 录音转写 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| Balabi（录音）/ Bloopy（视频） | Blobby（Omni 的 agent） | Omni 文档 |
| Sourcecraft（录音） | Sourcegraph | 视频 |
| collusion engineer for AM（录音） | solutions engineer for Amp | 视频 |
| code scale bench | CodeScaleBench | 日程 bio、上下文 |
| lies and lies and emails（录音） | Lies, Damned Lies, and Evals | 题目 |
| turtle benchmarks（录音）/ Turbo Bench 4（视频） | Terminal-Bench 4 | 论文 |
| Opus 5 then and Astra | Claude Opus 5 and GPT-6 Astra | slide |
| third free source（录音） | third-party source | 视频 |
| SDV bench / STB bench | SWE-bench | 上下文 |
| EBRX（录音）/ grep（视频） | grep | 视频 |
| 28 cheaper（录音） | 28% cheaper | slide |
| elements judge（录音） | LLM-as-a-judge | 视频 |
| student number scores（录音） | single number scores | 视频 |
| harness disability（录音） | harness traceability | slide |
| 300 page archive tome | 300-page arXiv tome（arXiv:2608.13867） | slide |

## 出入（来源之间不一致的地方）

1. **日程简介承诺的内容没讲**：简介说要「put one convincing score on trial」，让观众在两个 release candidate 间投票，并揭示三件事：「a +5.4-point win in my own repository-level eval that shrank to +0.35 after repeated runs」、一个被 sponsor 停止报告的流行 coding benchmark（「flawed tasks and contamination」）、「model graders that agreed with one another while missing the defects」；最后围绕五个检查重建决策（这个分数能改变什么决定 / run-to-run variance 多大 / 任务分布是否匹配真实负载 / evaluator 能检测哪些失败 / grader 在选定阈值下的 TPR 和 FPR）。**视频字幕和 12 张 slide 里都没有这些内容**（没有投票、没有 +5.4/+0.35、没有 grader 的 TPR/FPR）。实际演讲是 slide 02–14 的结构。【待确认】现场是否有没拍到的页面讲过（slide 01 和 10 没拍到，但视频字幕里也没有）
2. **F1 还是 F-beta**：讲者说「like an F1 score」；slide 08 写的是 F-beta，beta = 0.5（偏向 precision），数值 0.025295 是 F0.5。以 slide 为准
3. **harness 差距的说法**：讲者说「30-plus percent sort of difference」。slide 上同一 harness 内的差距：PI 下 Astra 比 Opus 5 高 30.1 个百分点；OpenHands 下 Opus 5 比 Astra 高 7.9 个点。Opus 5 从 OpenHands 换到 PI 掉了 26.9 个点。「30-plus」只对 PI 内的差距大致成立
5. **295 个文件是谁返回的**：讲者说「Claude went out and got like 295 files」；slide 08 只写 recorded output，引的是 CodeProbe trial，没写是哪个 agent
6. **「28% cheaper」的对象**：slide 写「on the affected task family」，讲者口头没限定范围

## 【待确认】汇总

1. 日程简介里的 +5.4 → +0.35、benchmark 被 sponsor 停报、grader 一致却漏检，现场是否讲过（视频里没有）
2. slide 01、10 的内容
3. Pi 被 Earendil 收购（只见二手资料）
4. Hardy et al. 的 arXiv 编号
5. 「Alzahrani · Verify Claims, Not Scores」的出处
6. 「Dai et al. · Hearsay」的出处，及其与 arXiv:2609.30266 的关系
7. 1% 误报 → 10,000 次复核对应的总量
8. 结尾观众插话的内容

