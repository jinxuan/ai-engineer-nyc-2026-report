[English](../10-13-1155-lies-damned-lies-and-evals-how-to-trust.md) | **中文**

# 怎样「不信任」你的 agent：评估配置、trace 和打分器

*在 AI Engineer NYC 2026 上，Omni 的 Stephanie Jarmak（曾是天文学家）提出，agent 评估应该像科学实验一样围绕一个决策来设计，并展示了成本节省、benchmark 排名、打分规则、质量关卡，甚至 trace 本身，都可能误导人。*

**Breakout（Evaluations track）· 2026 年 10 月 13 日（周二）· 11:55am–12:13pm · Turing Room (Riverside Ballroom)**
**讲者：** Stephanie Jarmak，AI Engineer，Omni Analytics（之前在 Sourcegraph；物理学博士；Center for Astrophysics | Harvard & Smithsonian 研究员；开源 agent 编排系统 Gas City 的维护者）

---

Stephanie Jarmak 自我介绍说是「几周前刚到 Omni Analytics 的 AI 工程师」，Omni 的平台通过它的 agent Blobby 提供「一个治理用的 semantic layer，让大家对业务数据有共同的理解」。在那之前，Jarmak 在 Sourcegraph 待了一年，先是做 coding agent Amp 的 solutions engineer，后来转去负责 Sourcegraph 的评估平台，在那里「做了一个很大的公开 benchmark，叫 CodeScaleBench，用来了解 agent 在非常复杂的代码库里表现如何」。再往前，Jarmak 是一名天文学家。

「我是从研究科学家的角度来看这件事的，我会不停地质疑一切，所以标题叫 Lies, Damned Lies, and Evals，」Jarmak 说，「标题说的是怎么信任你的 agent，但我觉得这场演讲主要讲的是怎么不信任你的 agent。」

每张幻灯片的页脚都有一条引用，最常引的是 Jarmak 的书 *Engineering Reliable Coding Agents: Evaluating and Operating the System Around the Model* 的各个章节。

## 1. 更便宜了，但失败还在

![Lower cost. The retrieval failure remained.](../../slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/01.jpg)
*成本降了 28%，分数没有实质变化，检索失败依旧存在。*

第一个例子来自 Sourcegraph。Jarmak 当时在测试公司的代码搜索 MCP 工具，想搞清楚「为什么这个 coding agent 就是不用我想让它用的那个工具？」Jarmak 改了 agent 的初始 prompt，重新跑了评估。幻灯片上的结果：

| 指标 | 结果 |
| --- | --- |
| 观测到的成本（在受影响的任务族上） | −28% |
| 主要评估分数 | Δ +0.0048 (t = 0.27) |
| 耗时 | +3.9% |

「便宜了 28%。挺有意思，」Jarmak 说，「但这不应该改变我在真正想衡量的东西上的任何决定……因为它还是没用那个工具。」成本下降也许值得作为一个关于方差的问题单独研究，但底下的问题一点没动：搜索漏掉了相关证据，而 agent 相信了这个空结果，「就这么相信了那些信息不存在。而这完全是因为它没用那个工具。」幻灯片的脚注：「Little evidence of score improvement does not establish equivalent quality.」（看不出分数提升，并不能说明质量相当。）

## 2. 换个 harness，排名就变了

![The ranking changes with the harness](../../slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/02.jpg)
*同样 63 个任务，两个模型，两个 harness：赢家反转了。*

第二个例子来自一项关于 harness 影响的第三方研究：从 Terminal-Bench 4 里选了 63 个不需要 GPU 的任务，Terminal-Bench 是「一个相当流行、广泛使用的 coding agent benchmark」。假设你在选给自己的 harness 配哪个模型。这项研究比较了 Claude Opus 5 和 GPT-6 Astra：

| Harness | Claude Opus 5 | GPT-6 Astra |
| --- | --- | --- |
| OpenHands | 36/63 (57.1%) | 31/63 (49.2%) |
| PI | 19/63 (30.2%) | 38/63 (60.3%) |

用开源 harness 之一 OpenHands 时，Opus 5 赢了。「他们只换了 harness 本身，就发现 Astra 做得好得多，」Jarmak 说，「差了 30 多个百分点那种。」（在 PI 下，Astra 领先约 30 个点；在 OpenHands 下，Opus 5 领先约 8 个点。）原始论文，Li 等人的「Finding the Right Fit」，报告了同样的数字，并总结说模型排名在不同 harness 之间会反转。幻灯片还引用了 Hardy 等人的「Agent Evaluation Reliability」，该研究发现，对固定的「模型 + scaffold」系统做排名，比只对底层模型排名可靠得多。

幻灯片的教训：「Compare the configuration you will actually run.」（比较你真正会跑的那个配置。）「你可能看到各种 benchmark 和漂亮的结果，但在你实际的环境里，它未必会按你预期的那样表现，」Jarmak 说。

## 3. 公开 benchmark 与内部评估

![Public benchmarks and internal evaluation](../../slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/03.jpg)
*公开 benchmark 用来看全局、出候选名单；产品决策要靠在目标工作上的内部对比。*

Jarmak 做了一个区分。「Evaluation 是你为了对自己的产品或流程做决定而进行的测量。」Benchmark 是「一个用于比较的标准化参照」。幻灯片展示了两种用途。用来看全局、出候选名单时，一个参照 benchmark 加上它公布的测试规程，产出一份候选名单，前提是「结果取决于任务组合和评估规程」。用来改进产品和流程时，你在目标工作上比较候选，做一个改动，然后循环：「Diagnose, change and retest. Communicate value against a baseline.」（诊断、修改、重测。相对 baseline 说明价值。）

公开 benchmark 有助于「对更大的全局有更宽的了解，拿到一份候选名单」，Jarmak 说，但不要过度依赖它们，「因为这些不同模型的训练都有尖峰性（spiky）」。例子是：「如果你是一家律所，你不一定要看 Terminal-Bench 的结果，除非你当然是想给自己的开发人员配 coding agent。」

这个区分关乎可见性，而不是用途：「Internal benchmarks exist. Public benchmarks can be used internally.」（内部 benchmark 是存在的。公开 benchmark 也可以在内部用。）组织可以建自己的 benchmark 来反映真实工作负载，用在「这种迭代式的飞轮流程里，在各种配置上不断 hill climbing」。脚注：「The decision determines what evidence we need.」（决策决定了我们需要什么证据。）

## 4. 围绕决策设计实验

![Design the experiment around the decision](../../slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/04.jpg)
*任务、重复次数和配置，成本按每个被独立接受的结果来计算。*

Jarmak 说，配置的候选名单定下来之后，要「像科学家一样」对待评估，决定改哪些变量。幻灯片列出了三个，并要求把质量、可靠性、人的时间和总成本放在一起报告：

| 维度 | 问题 | 幻灯片上的内容 |
| --- | --- | --- |
| Tasks | 工作负载覆盖 | 任务族 A、B、C，每个族里有若干任务 |
| Repeats | 多次运行之间的差异 | 同一个任务跑三次 |
| Configurations | 结果如何依赖于搭配 | 模型 A/B 与 harness A/B 的 2×2 网格 |

下面是一个单一指标：总运营成本除以被独立接受的结果数。

任务方面，挑能反映真实工作的任务，可以按族归类。Jarmak 做 CodeScaleBench 是因为「当时基本上就是 SWE-bench，只有修 bug，但软件开发生命周期里还有一大片其他对开发者有意义的任务」。一个模型「可能在理解代码上比在真正修 bug 上更有效」。重复方面：「这些系统是非确定性的」，所以要决定需要跑多少次。配置方面：模型和 harness 之间有「交互效应」。脚注提醒：「Repeated runs are not independent tasks. A better relative score does not establish that either system is reliable enough.」（重复运行不等于独立的任务。相对分数更好，并不能说明哪个系统足够可靠。）

## 5. 追踪证据在哪里变成了结论

![Trace where evidence becomes a conclusion](../../slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/05.jpg)
*可用、被调用、返回、被解读：以及两个建议的测试。*

「尽可能多地收集可观测性数据。……保留你所有的 trace，」Jarmak 说。回到那个没被用上的搜索工具，幻灯片把路径拆成四个阶段：可用、被调用、返回、被解读。先问这个工具到底可不可用：「如果不可用，就到此为止」，先把这个修好。然后问它有没有被调用、返回了什么，就这样「沿着可用信息的管道」一路往下查。

幻灯片建议记录搜索过程（在哪里、怎么搜的，有哪些缺口或不完整的结果，哪些没法检查），并做两个测试：一个搜索限制测试（同样的空结果，但把搜索限制明确写出来），一个证据测试（同样的 agent 和设置，提供已知相关的证据）。脚注措辞很谨慎：「A trace tells us where to investigate. These proposed tests have not yet demonstrated a repair.」（trace 告诉我们该去哪里查。这些建议的测试还没有证明能修好问题。）

![Better evidence can still go unused](../../slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/06.jpg)
*把证据来源和 agent 对证据的使用分开测试。*

即使换上了更好的工具，结果也可能不变。假设「它终于用上了那个好得多的工具。但你在任务结束时真正关心的结果，还是没变，」Jarmak 说。幻灯片建议把两半分开测：对照 A 在证据来源处换上可信的信息；对照 B 绕过使用证据的那个组件；然后看结果。在任何部署对比中，「keep non-use and failed attempts in the result」（把没用工具和尝试失败的情况都留在结果里）。Jarmak 的建议是，「当你基本上是在当侦探、想搞清楚系统里到底发生了什么的时候」，要把这些都记在心里。幻灯片的脚注说，这是一个模拟结果，生产环境中的效果还没测试，并且「tool use alone does not identify the tool's causal effect」（光看用没用工具，并不能确定工具的因果效应）。（它引用的来源标为「Alzahrani · Verify Claims, Not Scores」，作者没能找到。）

## 6. 审计打分规则和答案

![Audit the scoring rule and the answer key](../../slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/07.jpg)
*返回 295 个文件，其中 6 个相关：recall 满分，precision 2%。*

「你还要真正把控你在做的是什么测量，」Jarmak 说。在比较搜索工具时，Jarmak 想要一个在 precision 和 recall 之间取得平衡的分数。用 grep 时，agent「把整个厨房水槽都搬来了」：在这个例子里，「Claude 出去拿回来大概 295 个文件，其实只需要六个」。幻灯片上的数字来自 Jarmak 的 CodeProbe 工具的一次试验：

| 记录的输出 | 打分规则 |
| --- | --- |
| 返回 295 个文件 | Recall 1.0 |
| 找到 6 个参考文件 | Precision 0.020339 (about 2.0%) |
| 289 个在参考集之外 | Actual F-beta score 0.025295 (beta = 0.5) |

（Jarmak 在台上说的是「类似 F1 score」；幻灯片写明是 beta = 0.5 的 F-beta，更看重 precision。数字核对无误：6/295 = 0.020339。）如果你只在乎不漏掉任何相关的东西，「那 recall 为 1 也许就够了」，但 Jarmak 要优化的是别的东西。

幻灯片为任何打分器提出了几项挑战：一个已知正确的答案、一个空答案、一个把所有东西都返回的答案，以及一个答案里没有、但同样成立的替代答案。它还要求一份可审计的结果记录：打分规则、参考答案的版本和运行的身份标识。对公开 benchmark 那种单一数字的分数，教训是要问：「实际的 validator 在衡量什么？他们为什么认为这很重要？因为你想要的东西完全取决于你的业务需求。」

## 7. 测试关卡是否真的改变了执行

![Test whether the gate changes execution](../../slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/08.jpg)
*一次审批队列审计发现，有些路径在缺少必需组件时仍然允许执行。*

Jarmak 说，eval 和质量关卡、评审是有重叠的。Jarmak 一直在试验一个带审批队列的「software factory」：「我想尽可能自动化，把活干完，但我同时对这些系统完全不信任。」定期审计发现了一个问题。一个请求经过了审批和必需的检查，「agent 注意到事情不对了，但没有任何机制让它真正强制停下来」。幻灯片上写着：「My approval-queue audit found paths that allowed execution when required components were missing.」（我的审批队列审计发现，有些路径在缺少必需组件时仍然允许执行。）

修法是「把 agent 或其他系统标出问题，和在代码里真正强制执行关卡这两件事解耦」，然后决定怎么升级给人。建议的强制执行测试依次跑请求、审批加必需检查、执行和观察到的效果，同时故意破坏一道控制：拒绝或取消，去掉一个必需组件，或者在审批之后修改提案。结果要分开衡量：经授权的完成、安全停止，以及人的修复工作。脚注补充：「Also measure false blocks on legitimate work; this audit did not establish unauthorized production writes.」（也要衡量对正当工作的误拦；这次审计并没有证实存在未经授权的生产写入。）

## 8. 人工审核的负担

Jarmak 说，升级给人是有成本的，这张幻灯片作者没有拍到。例子来自「审计和修复另一个系统里出现的 LLM-as-a-judge 失败」，那个系统「误报率是 1%。如果所有报出来的警报都交给人，那就是 10,000 次审核。你的组织真的有那么多领域专家去做这件事吗？」要想清楚瓶颈会出现在哪里，怎么调整警报率，以及每个警报在下游的后果有多严重。

## 9. 有些证据需要另一个观察者

![Some evidence needs another observer](../../slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/09.jpg)
*Harness 自己的记录通过了 hash 检查，却漏掉了所有被注入的改动；一份独立的记录把它们全都抓到了。*

「Agent trace 本身有时候也不可靠、不可信，」Jarmak 说。研究人员在一个 agent 的配置里埋了一些安全问题。「他们有一个 hash 检查，用来检查对系统的任何篡改，然后……全都通过了。它一个被注入的安全问题都没发现。」当他们拿一份所有模型流量的独立记录来对比时，「就全都能检测出来」。

幻灯片画出了从模型到 harness 到工具到目的地的路径，以及三种可能的记录：模型流量的独立记录、harness 自己写的记录，以及最终状态的记录。Harness 自己的记录通过了 hash 检查 28/28；独立记录的对比检测出了 28 个故意引入的改动中的 28/28。脚注限定了这个结论的范围：「Detection beyond this scope is untested; model traffic does not prove execution or destination state.」（超出这个范围的检测未经测试；模型流量不能证明执行情况或目的地的状态。）幻灯片说，对于安全方面的论断，要测漏检、误报和盲区。（幻灯片引用的是「Dai et al. · Hearsay」，作者没能找到这个来源。）

## 10. 一次检索升级，按规矩来做

![A retrieval upgrade: the experiment I would run](../../slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/10.jpg)
*建议的部署前流程，以及部署后要盯的东西。*

回到最初的搜索例子，幻灯片列出了 Jarmak 会做的实验。部署前：在目标工作上做配对比较，然后做独立挑战（「Can an independent challenger find the decisive evidence?」，一个独立的挑战者能否找到决定性的证据？），然后和现有工作并行运行，在隔离环境里测试状态变更。部署后：跟踪完成率、修复、延迟和成本；审计被接受的工作和失败；拿 trace 和真实结果对照；留意工作负载和版本的变化；指定负责人、干预措施和回归测试（「Do scores predict real outcomes?」，分数能预测真实结果吗？）。脚注：「Set restriction or rollback triggers before deployment.」（部署前就设好限制或回滚的触发条件。）

在 Omni，Jarmak 正按这个思路搭评估 harness，用内部的生产数据给用户和 agent 的交互方式分类：「Q&A，可能从简单问题到更难的问题。也可能是搭 dashboard 和搭应用。」有代表性的代理数据有帮助，「但归根到底，生产环境才是真相」。部署之后测量必须继续，而「影响范围（blast radius）更大」的改动可以先在 shadow mode 下跑。

## 11. 把下一个自主权决策说清楚

![Make the next autonomy decision explicit](../../slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/11.jpg)
*一个提议的自主权决策、它需要的证据，以及三种出路。*

最后一步是决定「你愿意接受什么程度的风险」，Jarmak 说。在 software factory 里，当 agent 为一个风险较低的决策申请自主权时，它要说明下一步动作、为什么需要人来拍板、运行范围，以及它提议去掉哪一道人工检查。幻灯片把一个提议的自主权决策写成「additional action + operating scope + human check removed」（新增的动作 + 运行范围 + 去掉的人工检查），要对照三样东西来检验：一个证据门槛（有代表性的多次重复结果；可容忍的失败和总负担）、一个经过验证、带执行控制的 evaluator（「accept good work, detect defects and enforce the operating boundary」，接受好的工作、发现缺陷、守住运行边界），以及一个决定可接受风险、裁决相互冲突的业务价值的人类负责人。结果有三种：在测试过的边界内扩展，保留审核，或者「因为缺少证据、关卡未通过或范围变化」而停下并升级。目标是界定「什么时候可以安全地自主部署，什么时候要停下来升级给人」。

## 12. 资源

![Evaluation procedures and references](../../slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/12.jpg)
*Jarmak 的 skill 仓库和书。*

Jarmak 最后给了两份资源。GitHub 仓库 `sjarmak/coding-agent-workflows` 收了一组 agent skill，其中有三个流程和这场演讲直接相关：evaluation design、harness traceability 和 reliability procedures。还有那本书，Jarmak 称之为「这本 300 页的 arXiv 巨著，你可以直接丢给你的 agent」。*Engineering Reliable Coding Agents* 于 2026 年 8 月发布在 arXiv 上，开篇第一句是：「AI coding agents are commonly evaluated as models but deployed as systems.」（AI coding agent 通常被当作模型来评估，却被当作系统来部署。）「你可以让你的 agent 读这些，把其中一些最佳实践引入你自己的系统，」Jarmak 说。

---

**链接：** [Schedule](https://ai.engineer/nyc/2026/schedule?session=b1fe8396-6fa8-4574-9775-0dc01f56ce57) · [Video: Don't Trust Your AI Agents. Test Them Like a Scientist](https://www.youtube.com/watch?v=A3FL_zTQKU0) · [Engineering Reliable Coding Agents (arXiv:2608.13867)](https://arxiv.org/abs/2608.13867) · [Online edition](https://www.sjarmak.ai/books/engineering-reliable-coding-agents) · [coding-agent-workflows](https://github.com/sjarmak/coding-agent-workflows) · [CodeProbe](https://github.com/sjarmak/codeprobe) · [Li et al., Finding the Right Fit: Model-Harness Interactions across Agent Tasks](https://arxiv.org/abs/2610.00917) · [Omni's Blobby](https://docs.omni.co/docs/ai/queries)

Schedule 上的摘要承诺了一些在视频和幻灯片里都没有出现的内容：现场观众在两个候选发布版本之间投票，一次在仓库级 eval 里 +5.4 个点的胜利、多次重复运行后缩水到 +0.35，一个赞助方不再公布结果的 coding benchmark，以及一些彼此意见一致、却都漏掉了缺陷的模型 grader。视频以掌声结束，没有 Q&A。本文依据官方视频字幕、作者现场的录音和幻灯片照片（标题页和第 10 张幻灯片没有拍到），以及上面列出的资料。[精确笔记](../precise/10-13-1155-lies-damned-lies-and-evals-how-to-trust.md)（中文）保留了时间戳，并完整记录了各来源之间不一致的地方。
