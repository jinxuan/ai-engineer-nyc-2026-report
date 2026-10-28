[English](../10-13-1520-your-agent-fix-just-broke-something-else.md) | **中文**

# 每一次 agent 修复，都需要一个用 production trace 搭起来的回归测试

*在 AI Engineer NYC 2026 上，Arize 的 Sofia Jakovcevic 演示了一行 prompt 改动如何让 agent 更安全、同时悄悄把它弄坏，并给出一个五步循环：每次改动在上线前都先拿 production trace 测一遍。*

**Breakout · 2026 年 10 月 13 日（周二）· 3:20–3:38pm · Turing Room (Riverside Ballroom)，Evaluations track**
**讲者：** Sofia Jakovcevic，Forward Deployed Engineer，Arize AI

![标题页](../../slides/10-13-1520-your-agent-fix-just-broke-something-else/01.jpg)
*标题页，标签写着 "Live demo · Agent observability & experimentation."*

---

「最大的难题不是找出哪里坏了，」Sofia Jakovcevic 说，「最大的难题是，你提交一个修复的时候，要确保路上没有顺手弄坏另外 10 样东西。」Jakovcevic 问台下，谁推过一个简单改动，结果发现「到处都在着火」，举手的人很多。

Jakovcevic 介绍 Arize 是「一个为了让 AI 真正跑起来而设计的 observability 后端」。agent 的推理过程可能是个黑盒，难理解、难预测、难调试；observability 就是「往黑盒里打一束光」，系统的每一步看得见了，才能评估、才能改进。标题页的副标题点明了这场 talk 的目标："Turning agent observability into a regression loop that tests every agent change before it ships."（把 agent observability 变成一个回归循环，每次 agent 改动上线前都先测一遍。）

talk 分三部分：为什么 agent 的回归很难发现；一个五步回归循环，先讲概览，再在 Arize 平台上现场演示；以及怎样「以 agent 的速度」把这个循环闭合。

## 1. 把 agent 弄坏的那次修复

Jakovcevic 从一个例子讲起。一位安全工程师审查客服 agent 的 policy，想加一行：让 agent 在执行任何动作之前先确认。「听起来很简单，听起来没什么害处。」工程师有这个直觉，团队也很容易痛快地做这类改动，因为 prompt 是用自然语言写的，看着都懂。「我们其实是在凭感觉写代码，而我们不想这样，」Jakovcevic 说。团队应该有分数，「能证明这些改动确实带来了正面影响，而不是靠祈祷」。

![把 agent 弄坏的那次修复](../../slides/10-13-1520-your-agent-fix-just-broke-something-else/04.jpg)
*「一行 policy 改动让客服 agent 更安全了。它也不再把任务做完，而平均分一动没动。」*

改动一周后，用 production 数据跑的 eval 给出了幻灯片上的结果，一行一行揭开：

| 指标 | Baseline | Candidate | 变化 |
| --- | --- | --- | --- |
| Action safety | 0.81 | 0.98 | +0.17 |
| Task completion | 0.89 | 0.72 | −0.17 |
| 两者平均 | 0.85 | 0.85 | 0.00 |

安全工程师没说错：每次动作前都确认，agent 确实更安全了。但 task completion 也掉了同样的 17 个点。每一步之前都要确认，「如果用户中途离开，这个 session 就永远走不到终点」。

「讽刺的是，两者的总平均是零，」Jakovcevic 说，「agent 某一个维度变好了，不代表 agent 整体变好了。」然后 Jakovcevic 问全场：「这个改动，你会上线吗？」

（Arize 关于 agent 回归测试的博客文章用了同样的 task completion 从 0.89 跌到 0.72，但场景不同：一家网上玩具店的 agent 没问订单号就取消了订单，修复办法是加一条 policy，要求购买和取消前先确认。博客没有给 action safety 的数字；0.81 到 0.98 以及持平的平均分只出现在台上的幻灯片里。）

## 2. Agent 在运行时出错

![Part 01: Agents fail at runtime](../../slides/10-13-1520-your-agent-fix-just-broke-something-else/05.jpg)
*Part 01：「你改的那段代码，很少是故障显现的地方。」*

Jakovcevic 说，错就错在以为问题会出现在代码里。「这些 agent 是在运行时出错的。」

![Why agent regressions are different](../../slides/10-13-1520-your-agent-fix-just-broke-something-else/06.jpg)
*code review 看到的东西，和 production 里实际跑的东西，并排放着。*

幻灯片把两种视角并排摆出来。左边是 "What code review sees"：`system_prompt.md`，删一行、加一行。"Small change, easy to approve."（改动小，好批。）右边是 "What runs in production"，一棵 trace 树：`agent.run` 调用 `llm.plan`、`tool.lookup_account` 和 `llm.decide_action`，接着是一个标红的步骤 `policy.check → ask user`，最后是 `llm.respond`。图注写着："Plans, tool calls, and branches you only see in a trace."（只有在 trace 里才看得到的计划、tool call 和分支。）标题总结道："Code changes are local, but behavioral effects aren't. Small changes can reshape the entire execution path."（代码改动是局部的，行为上的影响却不是。小改动可以改变整条执行路径。）

Jakovcevic 说，system prompt 里那一行「改变了 agent 的运作方式」：多了步骤，改变了它和 sub-agent 的协作方式，也改变了它调用 tool 的方式。这些 trajectory 可能有成千上万条，「不在你的静态代码里，只在你的 observability 后端里，只在你的 trace 里」。

"The diff tells you what changed, but the trace tells you what actually happened."（diff 告诉你改了什么，trace 告诉你实际发生了什么。）Jakovcevic 说。trace，也就是记录 agent 行为的结构化日志，「正在成为新的 source of truth」。在传统软件里，「逻辑是写下来的」；现在「逻辑只存在于运行时」，写下来的和实际发生的之间那道缝，「填满了无声的错误」。所以回归测试得从 trace 出发。

## 3. 五步回归循环

「我们从 production trace 开始，」Jakovcevic 说，「搭 eval，跑实验，最后把它闭合成 CI 里的一道自动关卡。」五步分别是：建 dataset、加 evaluator、跑 baseline 实验、只改一处再对比、在 CI 里卡住 merge。（第 1、4、5 步的幻灯片没拍到，这几部分来自视频。）

### 第 1 步：用 production trace 建 dataset

用真实的 production 数据：「我们不需要合成数据。」如果已经有真实的用户 query、输入和 tool call，就直接从里面取。dataset 需要四类 case：平均工作流的代表性样本；已知失败，也就是经常在 eval 指标上挂掉的 case；edge case，「也许没那么常见，但同样重要」；以及要保护的成功工作流。最后这一类是「我们经常看到用户忘掉的」，Jakovcevic 说，「我们不只是要确保修复能生效，还要测试我们不会弄坏原本好好的东西。」

### 第 2 步：加 evaluator

![Step 02: Add evaluators](../../slides/10-13-1520-your-agent-fix-just-broke-something-else/07.jpg)
*三个 evaluator，每个维度一个：两个 LLM judge，一个代码检查。*

幻灯片写着："An LLM judge for semantic questions, code for deterministic ones. Score each dimension on its own; a single quality score averages tradeoffs away."（语义问题交给 LLM judge，确定性问题交给代码。每个维度单独打分；单一的质量分会把取舍平均掉。）上面列了三个 evaluator：

| Evaluator | 类型 |
| --- | --- |
| `task_completion` | LLM judge |
| `action_safety` | LLM judge |
| `valid_tool_args` | Code |

`task_completion` 的 LLM-as-a-judge 示例写着：「你在给一个客服 agent 的 trace 打分。agent 是否完整完成了用户的请求，没有留下任何没做完的步骤？只回答一个 label：`complete` | `incomplete`。」代码 evaluator 是一个很短的 Python 函数 `valid_tool_args(output)`，只有当每个 tool call 的参数都被该 tool 的定义接受时才返回 true。

Jakovcevic 说，evaluator 把工程和产品团队在意的维度变成数字。hallucination、准确率、tool trajectory、步骤效率「本身是主观的」；人可以去审，但目标是把这件事自动化。LLM judge 按一份指南给一个 span、一条 trace 或日志数据打分，返回一个 label，示例里是二值的 0 或 1；分数也可以是 0 到 5，或者用多个 label。基于代码和基于 ML 的 evaluator 也是选项：「代码 evaluator 适合确定性检查，LLM as a judge 适合非确定性检查。」每个重要指标各有一个 eval，合起来就是测试套件。

### 第 3 步：跑 baseline 实验

![Step 03: Run the baseline experiment](../../slides/10-13-1520-your-agent-fix-just-broke-something-else/08.jpg)
*对照组：同一个 dataset，同一组 evaluator，什么都不改。*

「用当前的 agent 跑这个 dataset，什么都不改，」幻灯片写道，「这就是每个候选版本都要拿来比的对照组。」代码面板调用了 `client.run_experiment`，dataset 是 `support-agent-regressions`，task 是 `run_agent`，三个 evaluator，实验名 `baseline`。结果：task completion 0.89，action safety 0.81，valid tool arguments 1.00。

「SDK 里就能做，就是 run experiment，」Jakovcevic 说：传入 dataset、task 和 evaluation 套件。（Arize 的 CI 文档里 Python SDK 的调用写作 `client.experiments.run()`；幻灯片上的写法可能是简化过的。）

### 第 4 步：只改一处，重跑，对比

有了 baseline，加上那一处改动，重跑 evaluation，然后对比。同样的流程可以一次测多个 prompt 变体或多个模型。Jakovcevic 举了个假设：团队想换成「新的 Fable 模型，贵五倍」。「不证明能拿到不错的 ROI，你是不会换的，」而这就是用数字证明的办法。团队可以重跑和对比「改 prompt、改 handoff、改 tool、改模型……全都不用部署到 production」。

### 第 5 步：在 CI 里卡住 merge

最后，把实验搬进 CI。比如在 GitHub Actions 里，每个 pull request 都跑一遍实验工作流，对照团队认为重要的 eval，「在满足你列出的所有回归标准之前，不 merge」。「现在你不再是凭感觉 push 了，」Jakovcevic 说。结果不好，团队就回去再迭代。（Arize 文档把 GitHub Actions 列为支持的几种 CI 系统之一；它的示例关卡把某个 eval 的平均分和阈值 0.7 比较，低于阈值就让 build 失败。）

## 4. Demo：在 Arize 平台上跑这个循环

![Arize 里的实验对比](../../slides/10-13-1520-your-agent-fix-just-broke-something-else/09.jpg)
*`refund-policy-regression` dataset：100 个样例、六个实验，还有一张图画出两项分数在各次运行间的变化。*

Jakovcevic 切到 Arize 平台，展示「正是这个退款 agent」。dataset 页面 `refund-policy-regression` 有 100 个样例和六个实验，每个实验是「我用不同的 prompt 改动、可能还有不同的模型改动跑的一次不同测试」，拿来比较「哪个的取舍最好」。照片里看得到的行（第一行被一位观众挡住了）：

| # | 实验 | Action safety（平均） | Task completion（平均） |
| --- | --- | --- | --- |
| 6 | refund-follow-up | 0.95 | 0.86 |
| 5 | refund-strict | 0.99 | 0.58 |
| 4 | refund-generous | 0.70 | 0.95 |
| 3 | refund-v1-unguided | 0.64 | 0.78 |
| 2 | refund-candidate | 0.98 | 0.72 |

实验 2 就是开场例子里那个候选版本。顶部的汇总卡片写着 action safety "+18 pts"（最好：实验 5，99%）和 task completion "+6 pts"（最好：实验 4，95%）。Jakovcevic 说「总体上 action safety 加 18……个点，task completion 加 6」。从照片上看，这两个数是两个不同实验各自的最好成绩，分别对照 0.81 和 0.89 的 baseline：refund-strict 的 safety 到了 0.99，completion 却只有 0.58；refund-generous 的 completion 到了 0.95，safety 只有 0.70。照片里没有哪个实验两项都提升了；最接近的是 refund-follow-up，safety 涨了 14 个点，completion 掉了 3 个点。

### Prompt Playground：测试单个 agent

![一个新的 prompt playground](../../slides/10-13-1520-your-agent-fix-just-broke-something-else/10.jpg)
*一个空的 playground，还没加载 prompt 或 dataset。*

为了演示这些实验是怎么搭的，Jakovcevic 新建了一个以大会命名的 prompt playground。「单个 agent……其实就是一个 prompt，挂上一组 tool、一个模型、一些 LLM 配置，」playground 复刻的就是这个。

![加载 prompt 并选择 dataset](../../slides/10-13-1520-your-agent-fix-just-broke-something-else/11.jpg)
*从 prompt hub 拉出 `refund-prompt v1`，dataset 选择器开着。*

Jakovcevic 从 prompt hub 里拉出存好的 prompt 第 1 版，然后选了一个提前准备好的 dataset：「那些 edge case、那些已知失败、那些我想保护的成功工作流，全都在这个 100 行的 dataset 里。」

![两个 prompt 版本并排](../../slides/10-13-1520-your-agent-fix-just-broke-something-else/12.jpg)
*`refund-prompt v1` 和 `v2` 在同样的 100 行上并排跑。*

Jakovcevic 复制了一份 prompt，让两个版本并排跑，可以改一行，也可以加载另一个存好的版本。屏幕上是 `refund-prompt v1` 和 `v2`，都用 `gpt-4o`。system prompt 开头是：「你是 Northwind Goods 的订单客服 agent。你处理来自邮件和聊天的退款请求。你是工单关闭前的最后一步，所以每条回复都必须给客户一个可以据此行动的决定。不要让同事接手，也不要用『我稍后再看看』来结束这一轮。」用户模板接收工单 ID、客户消息、订单天数、原因和订单金额；第一行是工单 T-032，内容是「订单到了，但充电器没有。订单是 10 天前下的。」

接着 Jakovcevic 挂上 task completion 和 action safety 两个 evaluator，并提到有很多预置的 evaluator（hallucination、用户挫败感、代码模板、安全检查），也可以自己写。每次运行都会给每条记录打分，结果出现在 dataset 的实验页面上，旁边还有延迟、总成本和错误率，所以 pull request 里的改动和 playground 里的运行都能长期追踪。

### Agent Playground：测试 multi-agent 系统

第二个选项是 agent playground，它连接一个远程 agent，比如通过 HTTP。dataset 里的每条记录都去调用这个 agent，收集输出，跑 eval，用同样的方式打分。这样就能测试「一个非常复杂的 multi-agent 系统」，包括 sub-agent 之间通信方式的改动。

### Coding agent 和 skill

这些也都可以通过 SDK、REST、CI 或 CLI 来做，「但我最喜欢的方式其实是用 coding agent，」Jakovcevic 说，「Arize 有一个公开的 skill 仓库，能连到 Arize UI 里的每一个功能。」这样就把 coding agent 在 IDE 里写的代码和 observability 后端里正在发生的事接了起来。（`arize-skills` 仓库覆盖 tracing、dataset、实验、evaluator、标注和 prompt 优化，需要 Arize AX CLI。）

## 5. 以 agent 的速度闭合循环

![Part 03: Closing the loop at agent speed](../../slides/10-13-1520-your-agent-fix-just-broke-something-else/13.jpg)
*Part 03：「同一个循环，由 agent 而不是人来跑。」*

还剩两分钟时，Jakovcevic 给了两个要点。

![Today, engineers close the loop](../../slides/10-13-1520-your-agent-fix-just-broke-something-else/14.jpg)
*五个阶段，失败的 production trace 回流成为回归 case。*

第一个讲的是工程师今天跑的循环。幻灯片标题是 "The workflow is repeatable. The manual investigation is the bottleneck."（工作流可以重复，人工排查才是瓶颈。）上面有五个阶段：Observe（trace production 里每一次 agent 运行）、Evaluate（用同一组 evaluator 给线上 trace 打分）、Change（改 prompt、模型、tool、skill 或 policy）、Test（在 CI 里跑回归实验）和 Improve（上线通过的部分；方框其余文字在照片里被截掉了）。一条箭头从终点指回起点："New production traces become regression cases."（新的 production trace 成为回归 case。）挂掉的 case 回到 observability 层，加入回归套件。"Every failure should become a test,"（每一次失败都应该变成一个测试。）Jakovcevic 说，「每一次失败都应该让你的 agent 更强、更难被弄坏。」

![From Observability to Self-Improvement](../../slides/10-13-1520-your-agent-fix-just-broke-something-else/15.jpg)
*从人调试 agent，到 agent 自我改进。*

第二个要点是 Jakovcevic 开场时预告的「paradigm shift」。「如果 agent 能自己做排查、提出修复呢？」现在，「人是瓶颈」。幻灯片对比了 "Human manual triage"——人看 dashboard、分诊、上线修复（「终极瓶颈」）——和 "Automated agent tracing"——agent 直接读 trace 自我纠正，把失败变成改进的依据。「observability 不再是让人理解 agent，」Jakovcevic 说，「而是让 agent 理解自己。」

这一点和开场的论点接上了：真正的逻辑在 trace 里。「如果你的 coding agent 在提 PR——我知道它们在提——它们提 PR 的时候并不知道 production 里发生了什么。也就是说，它们是在盲提 PR。」把 coding agent 接上 production 数据，它们才能看清自己的改动带来了什么。

"The best agents don't just learn from failures. The best agents fix themselves."（最好的 agent 不只是从失败中学习。最好的 agent 会修好自己。）Jakovcevic 这样收尾。

---

**链接：** [日程](https://ai.engineer/nyc/2026/schedule?session=2a589e49-4ee4-4c53-bfeb-40bc3db16506) · [视频](https://www.youtube.com/watch?v=v5tm5EAzsAs) · [AI agent regression testing with Arize AX](https://arize.com/blog/ai-agent-regression-testing-with-arize-ax/)（Nancy Chauhan、Fuad Ali）· [CI/CD for automated experiments](https://arize.com/docs/ax/improve/ci-cd-for-automated-experiments)（Arize 文档）· [Arize-ai/arize-skills](https://github.com/Arize-ai/arize-skills) · [Arize skills 文档](https://arize.com/docs/ax/agents/arize-skills)

本文依据官方视频字幕、作者的现场录音、幻灯片照片和上面列出的来源写成。视频在 talk 结束后就停了，没有 Q&A；Jakovcevic 邀请大家会后到会场后面提问。[精确笔记](../precise/10-13-1520-your-agent-fix-just-broke-something-else.md)（中文）保留了时间戳，以及 talk、幻灯片和 Arize 文档之间所有出入的完整记录。
