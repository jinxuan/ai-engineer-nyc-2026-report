[English](../10-13-1012-what-does-an-ai-engineer-do.md) | **中文**

# AI engineer 现在的活是搭 loop，不是写 feature

*在 AI Engineer NYC 2026 上，Arize AI 的 Aparna Dhinakaran 认为，AI engineer 现在要搭的是一整套 agent 系统，由它们来写、审、监控软件；Dhinakaran 还讲了 Arize 怎样做出 Signal：一个读生产 trace、提 issue、开 pull request 的 always-on agent。*

**Featured Keynote · 2026 年 10 月 13 日（周二）· 10:12–10:30am · Simons Room (Metropolitan East)，Main Stage**
**讲者：** Aparna Dhinakaran，Co-Founder & Chief Product Officer，Arize AI

---

「每年我们对 AI engineer 是做什么的都有个说法，而这个定义每年都在变，」Aparna Dhinakaran 一开口就把大会名字里的这个问题重新拿出来问。主持人的引子是：「AI 承担了越来越多的构建工作」之后，工程师还剩下什么；日程简介则说，Dhinakaran 会论证剩下的核心技能是定义「good」。台上，Dhinakaran 用 15 分钟里的大部分时间讲了一个具体案例：Arize 怎样做了一个替用户跑「构建—改进」循环的 agent。

## 1. Karpathy 的漏斗

![1000x Agent Engineer](../../slides/10-13-1012-what-does-an-ai-engineer-do/01.jpg)
*Andrej Karpathy 帖子里的四层漏斗，旁边是 Dhinakaran 自己的回复。*

Dhinakaran 说时机正好，因为 Andrej Karpathy「这个周末刚发了一条」，讲大多数人和站在 frontier、「跑着成千上万 agent、成群 always-on agent」的人之间越来越大的差距。这页标题是「1000x Agent Engineer」，复述了那个漏斗：

| 人数 | slide 原文 |
| --- | --- |
| ~6B | (~75%) have barely touched an LLM（几乎没碰过 LLM） |
| ~1–2B | (~20%) casual, free-tier users（偶尔用、用免费版） |
| ~20M | (~0.2%) professionals in math and code, where an agent finishes weeks of work from a prompt（数学和代码领域的专业人士，一个 prompt 就能让 agent 干完几周的活） |
| ~5K | (~0.00006%) with internal frontier access, running swarms of thousands of agents（能用内部 frontier 系统，跑着成千上万个 agent） |

旁边是原帖截图：Karpathy 把四月的一条帖子翻出来，因为人们对 LLM 能力的理解差距在「widening」，现在已经不太像两拨人各说各话，「more a sharp funnel」（更像一个陡峭的漏斗）。报道显示这条帖子发于 10 月 11 日，Karpathy 自己称这些数字是「napkin math」（餐巾纸上的粗算）。下面是 Dhinakaran 的回复：实验室在解决的只是一部分问题类型（「有数据可解、可测试的那些」），并因此「感受到了惊人的威力」。「实验室的速度会体现在企业里吗？」

「任何人看到这张图，第一个问题都是：我在哪一层？」Dhinakaran 说。这条帖子等于画了一张路线图：接下来几年，每个人怎样成为那 5,000 人之一。

## 2. 抽象层越来越高

![How we got here](../../slides/10-13-1012-what-does-an-ai-engineer-do/02.jpg)
*AI 能力上的五级台阶，从高级语言到 agent 工厂。*

| 时期 | 阶段 | 说明 |
| --- | --- | --- |
| 1970s | High-level languages | Frameworks and tests |
| 2022–24 | Chat assistants | Copy code in and out by hand（手动把代码复制进复制出） |
| 2025 | Local agents | You approve each step（每一步由你批准） |
| 2025–26 | Cloud agents | Work in the background（在后台干活） |
| Now | Factory of agents | Thousands of agents running for hours（成千上万个 agent 连跑几个小时） |

Dhinakaran 说，抽象层「已经升了几十年」（slide 从 1970s 开始画），最近几年更快：从高级语言，到复制粘贴代码，到 local agents，再到 cloud agents。「现在的预期是，我们每个人怎样开始运营一支 agent 团队，或者一座 agent 工厂。」

## 3. 搭 loop，不是写 feature

接着 Dhinakaran 对比了这份工作的过去和现在（那页没有拍到）。「你的工作不再只是写一个 feature、写一个 AI feature，而是怎样搭出那个去写 feature 的 loop。」不再是自己读 diff，而是搭系统和 agent 去做 code review，写 eval 去审那些 non-deterministic 的产出。不再只是配好 metrics 和 telemetry，而是让 agent 去读 observability 数据、找出模式。

这个 loop 大家都熟悉，「build、eval、ship、monitor、improve」，后面几页 slide 角上都挂着这五个阶段的标签。AI engineer 现在负责的，「不只是做每一步，而是怎样搭出那个……真正去做每一步的 loop」。

## 4. Signal 读 trace

![We built Signal to run this loop](../../slides/10-13-1012-what-does-an-ai-engineer-do/03.jpg)
*Signal 在 trace 里找 issue、给出证据、附上修复。*

为了替客户跑这个 loop，Arize 做了 Signal。slide 列了它的三件事：「Finds issues — in your traces」（在 trace 里找 issue）、「Shows the evidence — the runs behind each issue」（给出每个 issue 背后的 run）、「Links a fix — a pull request」（附上一个 pull request 作为修复），旁边是 Signal 的 issue 界面截图，带随时间变化的计数。Signal 读 trace，识别 telemetry 里的模式，「如果有 repo 权限，它还能直接提一个修复或 PR，」Dhinakaran 说。「Signal 是一个 always-on agent。」

![Signal starts with reading traces](../../slides/10-13-1012-what-does-an-ai-engineer-do/04.jpg)
*看起来像是一个银行助手的 trace，以及 Signal 从中找出的三个高严重度 issue。*

slide 上的例子是一张 trace 表，看起来来自一个银行助手（「Show me all transactions over $500 this month」「Apply for a personal loan of $10,000」），其中三组被 Signal 标了出来，对应三个 issue，都标为 High：「Says an action is done without calling a tool」（没调用工具就说操作已完成）、「States balances it never looked up」（报出从没查过的余额）、「Pre-approves without checking the profile」（没查用户资料就给预批）。Signal 按设定的节奏读一定量的 trace，并且要记得以前见过什么：某个当时像 issue 却没记下来的东西，随着数据增多，可能会变成 critical。

Arize 的文档对 Signal 的描述一样：一个按计划扫描项目 trace 的 worker，把反复出现的失败归成 issue，并为每个 issue 写一份带证据和建议修复的调查。按文档的说法，从接入的 GitHub repo 开 pull request 只在 Enterprise 方案里有；Free 和 Pro 只有 issue detection。

## 5. telemetry 的读者变成了 agent

![The future of telemetry](../../slides/10-13-1012-what-does-an-ai-engineer-do/05.jpg)
*三个阶段：人来读；人读了再 prompt agent；agent 直接读。*

| 阶段 | 谁读 observability 数据 | slide 说明 |
| --- | --- | --- |
| 1 | Human dev | The human is the only reader.（人是唯一的读者） |
| 2 | Human dev + agent | Human reads, prompts an agent to write.（人读，再 prompt agent 去写） |
| 3 | Agents | The agent reads it directly.（agent 直接读） |

「到底是谁在消费 telemetry 数据，这件事已经变了，」Dhinakaran 说。现在的问题是「怎样设置你的 telemetry 数据，让 agent 成为它的主要消费者」。这些数据同时也成了唤醒 always-on agent 的 trigger。

## 6. 一次 run 的内部

搭一座 always-on agent 工厂，第一个设计问题是什么让它们醒来。Signal 可以按固定节奏跑（「每 3 小时、6 小时，去拉最近……若干条 trace」），也可以由事件触发，比如 Slack 消息、用户投诉或某个 metric。（Arize 文档给出的默认是每六小时一次，可以在每三小时到每月之间调整。）

![Anatomy of an agent run](../../slides/10-13-1012-what-does-an-ai-engineer-do/06.jpg)
*control plane、sandbox 的各层，以及产出去了哪里。部分 slide 被视频播放器的控件挡住。*

这页把一次 run 分成 control plane 和 sandbox 两部分。左边，一个 trigger（「cron, event, click」）先经过准入步骤（「quotas, caps, flags」），然后创建 job（照片里这一格被挡住一部分）。sandbox 是「isolated, ephemeral」（隔离、用完即弃）的，分层搭建，最稳定的在最下面：base image（runtime、CLI、skills）、一层 scoped keys（被挡住一部分）、instructions（system prompt、permissions）、workspace（repo clone、scratch 目录）、run context（memory 文件、tool server）和 task prompt。右边，结果通过 tool call 出去，每一轮都被 trace，teardown 时 pod 被回收。

Signal 每次醒来，Arize 都新建一个 sandbox，装进 trace 的上下文、APM logs 和 metrics 之类的第三方数据、repo 权限和所需的 key。「有一个 proxy，信息进出 sandbox 的唯一通道就是这一个 proxy，」Dhinakaran 说。sandbox 里的一切都被 trace，产出的 issue 存进 memory。

## 7. 留下什么，扔掉什么

![What happens after each run](../../slides/10-13-1012-what-does-an-ai-engineer-do/07.jpg)
*一次 run 结束时，有意保留的和直接扔掉的。*

| 有意保留 | 扔掉 |
| --- | --- |
| Output：给出的答案或做出的改动 | Sandbox：run 结束后拆掉 |
| Traces：每一轮 | Credentials：短期有效，用完吊销 |
| Memory：文件，不是聊天记录 | Workspace：repo clone、scratch 文件 |
| New skills：按需保存 | Leftovers：由空闲清扫回收 |

agent 找到的 issue 会被保存，「这样下次再起一个 agent，就不用从零开始」。agent 自己的动作也被 trace，包括它有没有开 PR；run 中学到的 skill「也可以存进 repo」。sandbox、credentials 和 repo 权限都会被移除。

## 8. Durable agent

![How to keep the agent durable](../../slides/10-13-1012-what-does-an-ai-engineer-do/08.jpg)
*某一步失败时，新的 sandbox 和 harness 从保存的状态接着跑。*

如果 sandbox 或 harness 在 run 中途挂了，Signal「不应该一切从头来」，Dhinakaran 说。「它应该能从失败的那一步接着做。」slide 上，第 1、2 步完成，第 3 步失败，一个新的 sandbox 和 harness 从「saved state, outside the sandbox」（保存在 sandbox 外面的状态）接着跑第 3 步；保存的内容包括每个完成的步骤、对话、memory，以及下一步是哪一步。

## 9. 不绑定模型和 harness

![Build model and harness agnostic](../../slides/10-13-1012-what-does-an-ai-engineer-do/09.jpg)
*knowledge、tools 和 controls 保持不动，用一个 eval set 给每种组合打分。*

Signal「不只支持我们一开始用的 Claude Code，也支持 Cursor 和 Codex」，目标是支持主要的 harness，包括开源的，也支持同一家族里的任意模型，比如在 Claude Code 下用任意一个 Anthropic 模型。（Arize 文档说 Signal 默认跑在 Claude Code 上，三选一是 Enterprise 的选项。）slide 画出了保持不动的部分：「knowledge in files」（skills、memory、instructions）、「tools over MCP」（任何 harness 都用同一套工具）和「controls」（sandbox、权限、审核）；可替换的是 harness A 或 B、model A 或 B，每种组合都由一个 eval set 按质量和成本打分。

Dhinakaran 给了两个理由。这是每次运行时评估 agent 质量的办法；它也能控制成本：「能不能把成本降下来，同时保持 rigor……保持 Signal 实际检测出的 issue 的质量。」

## 10. 写 eval，再评估 evaluator

「AI engineer 真的需要学会写 eval，」Dhinakaran 说。Signal 每开一个 PR，都有 eval 在 agent 上面跑来测它。一个 eval 可以很简单：一个 dataset、一个 prompt，检查输出有没有回归。

![From assertions to evals](../../slides/10-13-1012-what-does-an-ai-engineer-do/10.jpg)
*一个客服 agent 的 dataset、一个打分 prompt，以及 judge 给的标签。*

slide 上的打分 prompt 是：「You are grading a support agent. Given the question and the answer, reply with a label and a reason. correct: matches our policy / incorrect: it does not.」四个例子：

| 问题 | agent 的回答 | judge 的标签和理由 |
| --- | --- | --- |
| Cancel my subscription | Done. It ends on Nov 1. | correct：符合取消政策 |
| Refund for order 4412? | Refunds take 90 days. | incorrect：政策是 5 到 7 天，不是 90 天 |
| What is your return policy? | 30 days, free returns. | correct：符合退货政策 |
| Change my address | Updated to 12 Main St. | correct：按要求更新了地址 |

eval 还必须「和人的判断对齐」，Dhinakaran 认为这「大概正是 AI engineer 应该花大量时间的地方」：和自己的 agent 一起看 trace，找出常见错误。「我昨天听 Lance 讲了用来做 eval 的 Claude skill，」Dhinakaran 说，指的是前一天 Anthropic 的 Lance Martin 的 workshop。Arize 也有自己的 skill，帮人搭 eval、把 eval 和人的判断对齐。（Arize 文档里有一个 `arize-align-evaluator` skill，会报告 accuracy、confusion matrix 和各标签的 precision、recall；Dhinakaran 没有点名。）「转向定义什么是 good，对 AI engineer 来说大概就是在这里变得非常非常重要。」

![You evaluate the evaluator](../../slides/10-13-1012-what-does-an-ai-engineer-do/11.jpg)
*五个 case 上人和 judge 的标签：80% 一致。*

| Case | Human | Judge | 一致 |
| --- | --- | --- | --- |
| Cancel my plan | correct | correct | ✓ |
| Refund for #4412? | incorrect | incorrect | ✓ |
| Return policy? | correct | correct | ✓ |
| Change my address | correct | incorrect | ✗ |
| Close my account | incorrect | incorrect | ✓ |

Dhinakaran 说，团队常问的是「我怎么信任这个 evaluator？」，或者「我怎么相信它和我的判断一样好？」Arize 对自己产品的做法是：「我们会评估 evaluator。」团队建 golden-truth 数据集，跑 eval，拿结果和人的标签比对、对齐，「确保 judge 真的和人意见一致……然后才发布新的 evaluator」。slide 上的例子在五个 case 上 judge 和人的一致率是 80%。这些 eval 随后可以 offline 用，也可以 online 用。

## 11. Trace 一切，包括 coding agent

agent 搭好、always-on、上线前也跑过 eval 之后，「目标是 trace 一切」，而现在这意味着「不只 trace 这些 agent，也 trace 搭出它们的那些 coding agent」：Claude Code、Cursor、Codex，好让团队看清「agent 工厂」里发生了什么。

![Online evaluation](../../slides/10-13-1012-what-does-an-ai-engineer-do/12.jpg)
*从实时 trace 到作为修复回到工厂的五步。*

这页把 online evaluation 列成五步：每次 agent run 的实时 trace；eval 给每个 span 打分；失败聚成模式；模式变成 eval case；作为修复回到工厂。offline eval 用来测新改动；上线后它们变成跑在 telemetry 上的 online eval，结果再被拉回来给 agent 读。整个系统就成了：「收集数据，eval 这些数据，在上面跑 agent，好找出问题、找到差距在哪。」

## 12. 弥合差距

回到开头的问题，Dhinakaran 说「今天实验室里发生的事和很多企业里发生的事之间，差距相当大」，部分原因是「企业内部的数据比实验室里那种高度受限、可测试的数据乱得多」。AI engineer 就是弥合这个差距的角色：trace 一切，跑 eval，搭一个让 agent 能从收集到的 telemetry 中改进的系统，「这样你就不再只是在做一个个 feature，而是在搭那个让你的产品不断改进的系统」。Dhinakaran 请大家去 Arize 的展台看 Signal，然后结束了演讲。

---

**链接：** [Schedule](https://ai.engineer/nyc/2026/schedule?session=b2ed4456-5671-4e8e-83fd-684c012fef8c) · [视频：「What Does an AI Engineer Do Now?」](https://www.youtube.com/watch?v=ImnZ7s68jw8) · [Day 1 直播 1:00:16 处](https://www.youtube.com/watch?v=akEqiXFo3GM&t=3616s) · [Arize Signal 文档](https://arize.com/docs/ax/observe/signal) · [From Signal to PR](https://arize.com/blog/from-signal-to-pr/)（Arize 博客）· [Align evals to human feedback](https://arize.com/docs/ax/evaluate/align-evals-to-human-feedback) · [Lance Martin workshop 的文章](10-12-1530-unhobbling-the-frontier.md)

本文依据官方视频字幕（和 Day 1 直播对照过）、作者翻拍的直播 slide 照片和 Arize 的文档。这场演讲没有 Q&A。[精确笔记](../precise/10-13-1012-what-does-an-ai-engineer-do.md)（中文）保留了时间戳、字幕勘误和待确认的问题。
