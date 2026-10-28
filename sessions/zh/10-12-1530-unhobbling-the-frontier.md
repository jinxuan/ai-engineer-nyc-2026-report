[English](../10-12-1530-unhobbling-the-frontier.md) | **中文**

# 你的应用多半停在 frontier 里面

*在 AI Engineer NYC 2026 上，Anthropic 的 Lance Martin 认为，大多数 Claude 应用都因为一些小的配置错误，白白多花了钱、少拿了质量。Martin 用 23 分钟讲了五个修法，又用 48 分钟回答了关于 eval、hill-climbing 和 effort 的提问。*

**Workshop · 2026 年 10 月 12 日（周一）· 3:30–4:30pm · Shannon Room (NY Ballroom E)**
**讲者：** Lance Martin，Member of Technical Staff，Anthropic（之前在 LangChain）

![Lance Martin 在台上](../../slides/10-12-1530-unhobbling-the-frontier/15.jpg)

---

Lance Martin 一开场只放了一张图。横轴是成本，纵轴是性能，一条曲线标出所有高效的配置，也就是 Pareto frontier。一个标着「your application」的红点落在曲线里面很远的地方，一条虚线箭头把它向左推到曲线上，旁边写着「a 'free' win (lower cost, no performance drop)」（一次「白捡」的收益：成本更低，性能不降）。

「很多时候，你其实并不在这条 frontier 上，」Martin 说，「你的应用停在 frontier 里面，本来可以站到线上去，差的只是一些小的配置错误。今天我就讲怎么修。」

![大多数应用停在 frontier 里面](../../slides/10-12-1530-unhobbling-the-frontier/01.jpg)
*「Most applications sit inside the frontier」：挪到曲线上，成本降了，性能没丢。*

这就是标题里「unhobbling」（解开束缚）的意思：模型本身有能力，是它周围的配置拖了后腿。Martin 讲了五个主题，每个都对应 Claude Code 里 `claude-api` skill 的一条命令：审查 prompt、用好 prompt cache、设计一个好的 eval、在 eval 上 hill-climb、为长时间运行的任务搭 harness。

## 1. Prompt 会积累债务

「Prompt 跟代码一样会积累债务，」Martin 说。几代模型之前，开发者往 prompt 里加了「think step by step」「double-check your work」「be maximally thorough」这类指令，或者给模型塞 scratchpad 和思考模板。Frontier 模型已经用不着这些了：「它们自己会验证结果，用不着［你叫它］认真。」留着这些指令会抬高成本，还可能拉低性能。

幻灯片把这类债务分成三种：

![Prompt 指令会作为债务累积](../../slides/10-12-1530-unhobbling-the-frontier/02.jpg)
*七种 prompt 债务，按它们对应用造成的影响分组。*

| 影响 | 模式 | 幻灯片上怎么说 |
| --- | --- | --- |
| **Taxes cost** | Verification rituals | 「verify twice before answering」：实测 **每张工单成本 +48%，准确率零变化** |
| | Thoroughness boosters | 「be maximally thorough」：每张工单不必要的 tool call 从 **0.07 涨到 0.95**（旧模型对新模型） |
| | Procedure and scratchpad scaffolds | 在模型自带的 thinking 之上再叠一层分步模板 |
| | Worked-example stuffing | few-shot 示例教模型走一长串它根本不需要的推理链 |
| | Anti-laziness tool boosters | 「always check the docs first」：没东西可查也被逼着去查 |
| | Dead weight | 早已下线功能的 runbook，每次请求都要读一遍、付一遍钱 |
| **Breaks accuracy** | Contradictory rules | 两条隔了几个月加进去的规则；模型两条都引用，然后卡住：**埋进一对矛盾规则，准确率 −9 个点** |
| **Breaks outright** | Dated configuration | 针对旧一代 API 的设置，要么被拒，要么被悄悄换了含义 |

矛盾规则讲得最多。旧模型听指令的能力弱，碰到两条冲突的规则会无视其中一条，「好歹还能用」。Frontier 模型两条都当真：「矛盾的规则经常把它们搞糊涂。」模型越强，矛盾的伤害越大。

![两条审查命令](../../slides/10-12-1530-unhobbling-the-frontier/03.jpg)
*同一套审查，从两个 skill 都能调到。*

修起来只要一条命令。`/claude-api prompt-audit` 检查调用 API 的代码；`/checkup prompt-audit` 检查 Claude Code 自己的配置。两者底下调的是同一个 skill，它扫描 prompt、skill 和 tool 描述，找出这些模式并删掉。Martin 说 Anthropic 内部用得很多。

![Prompt audit 提升性能、降低成本](../../slides/10-12-1530-unhobbling-the-frontier/04.jpg)
*在一个 50 张工单的客服 eval 上模拟一次迁移。*

为了展示效果，Martin 做了一个 50 张工单的客服 eval，模拟从 Opus 4.8 迁移到 Opus 5.5，把每种反模式逐个注入，再对结果取平均：

| 配置 | 每张工单成本 | 准确率 |
| --- | --- | --- |
| Opus 4.8，旧 prompt | 2.45¢ | 88.1% |
| Opus 5.5，同一个 prompt（只换模型） | 2.02¢ | 90.0% |
| Opus 5.5，跑完 prompt-audit | 1.83¢ | 92.0% |

光换模型就有提升；审查完 prompt，点又往左上挪了一截。实际的结论是：每次迁移模型都是审查 prompt 的好时机，别只改个 model ID。

## 2. 用 prompt cache：质量上没有任何代价

Agent 要跑很多轮，每轮只新增一点内容，所以 context 大部分是重复的。「你不想为同样的东西付两次钱。」命中 cache 的 token 大约按基础价的十分之一计费（Martin 建议去 pricing 页面看准确数字），速度也更快。

难点在于 cache 是严格的前缀匹配：「前缀里哪怕改一个字节，cache 就失效了。」Martin 建议按变化频率从低到高排 prompt：system 指令和 tools 放最前，跨 session 全局缓存；然后是 `CLAUDE.md` 这类项目级 memory；然后是 session 状态；最后是每轮都在增长的消息。Claude Console 能看到一个应用的 cache 命中率，Claude Code 团队对自己的命中率盯得「非常非常虔诚」。

![cost-optimize 通过修好 prompt caching 降低成本](../../slides/10-12-1530-unhobbling-the-frontier/05.jpg)
*在使用默认 harness 的内部 benchmark 上跑 cost-optimize。*

`/claude-api cost-optimize` 检查 cache 的使用情况（顺带也查 effort 级别和 prompt）。Martin 和同事在几个 harness 没有正确缓存的内部 benchmark 上跑了它：

| Benchmark | 成本变化 | 成本 | 准确率变化（95% CI） |
| --- | --- | --- | --- |
| tau2-bench (retail)，客服 agent | −72.9% | $48.68 → $13.20 | +2.6pp (−2.1 to +7.3) |
| LegalBench，法律分类 | −66.9% | $38.46 → $12.74 | −0.8pp (−1.3 to −0.3) |
| OfficeQA Pro，文档问答 | −71.7% | $252.14 → $71.26 | +1.5pp (−4.0 to +7.0) |
| SWE-bench Verified，agentic coding | −24.3% | $14.42 → $10.92 | +1.1pp (−2.6 to +4.8) |

Martin 的总结是「成本最多降 72%，性能没有实质变化」，并称 caching「纯粹是省钱」。表格大体支持这个说法，只有一处细节：LegalBench 的准确率变化虽小，整个置信区间却都在零以下。

## 3. 给 Claude 一个能爬的 eval

「Frontier Claude 尤其擅长自我纠错，」Martin 说，「只要给 Claude 一个结构良好的 evaluation，它基本上就能自己在这个 eval 上 hill climb。」前提是这个 eval 爬得动。

![好 eval 的四个要素](../../slides/10-12-1530-unhobbling-the-frontier/06.jpg)
*分数对每次尝试消耗的 token，三种尺寸的模型，各自 low、medium、high 三档 effort。*

图上画的是一个 frontier 模型、一个中等模型和一个更小的模型，各在 low、medium、high effort 下的分数与 token 消耗。好的 eval 能看出四件事：(1) 模型越强，分数越高；(2) 思考越多，分数越高；(3) 离满分还有余量（headroom）；(4) 多次运行之间方差小。Martin 配套的博客文章还加了排在最前面的第五条：任务要贴近生产环境，而不是只图好生成、好打分。

Martin 最常见到的问题是任务有歧义。一个 eval 看上去还有 20% 的余量，「但没有任何模型真能拿到」，于是团队烧 token 去爬一个并不存在的天花板。博客给了一个判断方法：不管重复多少次、每次都失败的任务，多半有歧义；好的任务是两个领域专家会给出同一个结论的任务。方差也可能藏在配置里（effort 没有一致地应用）或环境里（上一轮试验留下的文件或 git 历史直接把答案递给了 agent）。

命令是 `/claude-api build-eval`。它「对你的数据放在哪儿完全没有预设」，任何 tracing 方案都能配合：描述问题，把它指向原始数据（比如 trace），它会「给你看数据、给你看 evaluation，和你一起核对 grader［和］评判标准」。据博客，它按以下优先顺序取样例输入：生产环境的 transcript（先问清保留期限和敏感数据）、bug 报告和客服工单、五到十个手写案例、从代码库合成的案例。然后它提出最便宜的合适 grader：输出受约束时用代码检查（精确匹配、固定标签集、JSON schema、跑通测试）；输出开放时用带 rubric 的 LLM judge，rubric「写成可以核对的论断（不是 1 到 5 分的量表）」。Judge 不能是被测的那个模型。在信任 grader 之前，它会和你一起给几个案例打分，对同一个输出跑两次看结论是否一致，检查超时和被截断的回答；如果 baseline 已经在 95% 上下或更高，它会发出警告。

## 4. Hill-climb，但别骗自己

有了好的 eval，「frontier Claude 模型 hill climb 起来非常非常有效……不需要你在 loop 里。」危险在于 overfitting：把系统调成适应 eval 的怪癖，而不是适应生产环境。「一上生产，性能根本没变好。这很让人沮丧，因为你花了很多钱。」

![小心 overfitting](../../slides/10-12-1530-unhobbling-the-frontier/07.jpg)
*Benchmark 的特征如何塑造 harness。虚线箭头是一次彻底的泄漏。*

幻灯片把 benchmark 的特征和它们对 harness 的影响配成对：

| Benchmark 有…… | ……于是 harness 多了 |
| --- | --- |
| 需要 OCR 的任务组合 | 一个 OCR 工具 |
| 任务都在 `/app` 下 | 「always cd /app, run pytest」 |
| 有特色的措辞 | 针对这些措辞调过的 prompt |
| 你读过的失败案例 | 每个失败一个补丁 |
| 带答案的公开 repo | 一个去 curl 参考答案的 agent（彻底的泄漏） |

`/claude-api hillclimb` 从经典统计里借了一个办法：train/test split。Martin 特意说明它「当然不完美。你得仔细审查。」

![Hill-climbing 循环](../../slides/10-12-1530-unhobbling-the-frontier/08.jpg)
*每轮只改一处，留出 test split，回滚规则写死。*

幻灯片上的循环只改一个面（比如 prompt），模型和 harness 保持不变。开始前先做 pre-flight：把 baseline 跑几遍，定出噪声底线。每一轮，analyzer 只读 train split 里的失败案例，形成一个假设，提出一处修改。两个 split 每轮都打分，按三条规则决定去留：train 和 test 都变好，保留；train 变好但 test 不动，视为 overfitting，回滚；任一变差，视为退化，回滚。「如果看到 train 在涨、test 不动，就说明你可能在 overfitting。」

博客在 split 之外还加了两条习惯：永远不要把失败案例的内容贴进 prompt；从结构上让模型够不着答案，因为模型会通过找到答案来「reward hack」。博客也建议了往哪里使劲：改起来便宜的面，改动对分数的影响能归因得清楚的地方（skill 描述和 skill 触发率是很好的一对），以及确实还有余量的目标。分数连续两三轮不动时，climber 停止修改，把剩下的每个失败按原因分类，常常会发现有歧义的案例或坏掉的 grader。最后它报告 test 结果和置信区间；如果提升落在噪声范围内，它会建议不要合并。

### 案例一：对 skill 本身做 hill-climbing

![claude-api skill 的 hill-climbing](../../slides/10-12-1530-unhobbling-the-frontier/09.jpg)
*claude-api skill 在大约二十几轮里的通过率，分三个阶段。*

Martin 用一个基于 Anthropic 文档构建的 eval 对 `claude-api` skill 做 hill-climbing，让 Claude Code 能访问 skill 原文和文档。通过率从 66.1% 升到 87.9%（台上说的是 68%；幻灯片和博客写的是 66.1%）：

| 阶段 | 改了什么 | 通过率 |
| --- | --- | --- |
| 1 | 补上缺失的章节和类型表（八个功能完全没覆盖；C# 和 Java 的类型表有错） | 66.1% → 约 77% |
| 2 | 修正 skill 指导 Claude 写代码的方式：停滞之后，climber 发现 Claude 会写训练数据里的旧版 API 写法，于是在 skill 开头加了一张旧写法到新写法的对照表 | → 80.1% |
| 3 | 修 grader，外加更多 skill 修改（始终没有改善的任务指向有问题的示例或 grader） | → 87.9% |

「简单的文本类的面，非常非常适合这种流程，」Martin 说。

### 案例二：为成本做 hill-climbing

![以成本为目标的 hill-climbing](../../slides/10-12-1530-unhobbling-the-frontier/10.jpg)
*以成本为目标的 climber 在客服 benchmark 上走过的路径。*

告诉它「reduce cost」之后，climber 在模型、effort 和 prompt 的组合里搜索，找能降成本又不跌破 baseline 的配置。博客给出了客服 benchmark 上的数字（44 张工单：30 张用于搜索，14 张留出）：

| 步骤 | 配置 | 准确率（搜索用工单） | 每张工单成本 |
| --- | --- | --- | --- |
| 1 | Opus 4.8，high effort（baseline） | 74.4% | 4.6¢ |
| 2 | Opus 5.5，low effort，审查过的 prompt | 87.8% | 1.9¢ |
| 3 | Sonnet 5，low effort | 88.9% | 1¢ |
| 4 | Sonnet 5，low effort，改进的 prompt（路由规则、退款上限交叉引用） | 98.9% | 1¢ |

在 14 张留出的工单上，最终配置得分 90.5%，原配置是 78.6%，成本大约是原来的五分之一。Martin 在台上说的是「成本降了 90%」；按博客的数字算，更接近 78%。（幻灯片把第 2 步标成「Opus 5」；博客写的是 Opus 5.5。）

有两点比较突出。即使 eval 已经饱和，成本照样可以当目标：「我不在乎性能，我只想降成本。」另外，更强的模型用更低的 effort，往往胜过更弱的模型更卖力地干，「就像让一个更聪明的人［可以］少花点力气」，因为它花的 token 更少。这条经验现在已经写进了 skill。

## 5. 为连跑几天的 agent 搭 harness

最后一个主题往前看。

![任务时长随时间的变化](../../slides/10-12-1530-unhobbling-the-frontier/11.jpg)
*Claude 模型能自主完成的任务长度，以人类完成所需时间来衡量。*

这张图仿照 METR 的 time-horizon 图，分成三个时代：autocomplete 和 chat、同步的本地 agent、异步的远程 agent。数据点从 Claude Sonnet 3.5（21 分钟）一路升到 Sonnet 3.7（1 小时）、Opus 4（1.7 小时）、Opus 4.5（4.9 小时）和 Opus 4.6（12 小时）。Martin 说 Claude 3 Opus「只能自主干大概四分钟的活」；现在任务时长「超过一天到两天」，已经超出 METR 的方法能轻松测量的范围。到了这个长度，可靠性和安全性的要求都不一样了。

![Managed Agents 架构](../../slides/10-12-1530-unhobbling-the-frontier/12.jpg)
*Harness 在中间，session、sandbox、tools 和 orchestration 是各自独立的组件。*

Claude Managed Agents 建立在 Martin 所说的「decoupling the brain from the hands」（把大脑和手分开）之上，这也是 Martin 四月份参与撰写的一篇 engineering 文章的标题。Harness 是大脑；sandbox 是手，代码在里面跑。「想象你有一个 agent 挂一整夜……半夜 sandbox 挂了。状态没了，session log 没了，什么都没了。」所以 session log 是一个独立的 append-only 对象，和两者都分开。Sandbox 挂了，harness 收到一个 tool call 错误，再起一个新 sandbox；harness 挂了，新的 harness 从 log 接着跑。Secret 存在 sandbox 外面的 vault 里：Claude 能用，但「从来没有直接访问权」，这对一个「比如连续干好几天」的 agent 很要紧。

![基于 Managed Agents 的 agent 自动化](../../slides/10-12-1530-unhobbling-the-frontier/13.jpg)
*Martin 和 CJ Avilla 博客文章里的参考自动化。*

Martin 最后用一篇和 CJ Avilla 合写的文章里的例子收尾。一个定时任务唤醒 agent。它通过 MCP 或 HTTPS 只读地读取信息源，凭证从 vault 取。Guardrail 限制轮数、时间和花费，heartbeat 让人能发现它是否停了。Memory 分成两份：agent 自己写的状态（每个信息源一个书签、已经说过什么、每次运行的日志），和读者写的偏好（范围、长度、通知谁、哪些天别打扰、什么时候停）。发帖之前它先检查是否已经发过，然后把简报发到一个事先批准的目的地。公开版本在工作日早上读 Slack 频道和 GitHub pull request，发到一个 Slack 频道。

![Onboarding 命令](../../slides/10-12-1530-unhobbling-the-frontier/14.jpg)
*在 Claude Code 里运行它就能把这个例子搭起来。*

带着文章的 URL 运行 `/claude-api managed-agents-onboard`，就能把这个例子建出来。Martin 用 Anthropic 同事现在的工作方式结束了演讲：给 agent 一个 eval，让它爬一整夜，「比如早上回来，它已经好了一大截。」

## Q&A

提问持续了 48 分钟，下面按主题归类。

### 谁来建 eval：人还是 Claude？

**Eval 设计能自动化到什么程度？** Martin 引用了 Hamel Husain 的反馈。Hamel Husain 公开在一个客服应用上试了 `build-eval`，认为这个 skill 应该先给用户看原始数据。Martin 同意，并且已经开了一个 pull request 加数据查看器。Claude 很擅长扫描离线日志、标出可疑案例，「相当于一个 online evaluator，只不过看的是你的离线日志」，但「人在 loop 里仍然有很大的作用」。

**人和 Claude 的分工会怎么变？** 人负责定方向：「你到底想评估什么？为什么重要？这有点像品味。」这一点清楚之后，Claude 读原始 trace，标出应用不达标的地方，人审这些案例，把它们变成 evaluator。「这通常是 Claude 和人很好的搭配。」

**Eval 归谁管，产品还是工程？** 「在 Anthropic，每个人都可以建 eval」，通常按产品归属来分。这类 skill 让更多角色能建 eval，因为 eval runner 这类管道活 Claude 都能干。

**该用哪个 eval 或 tracing 工具？** Martin 来 Anthropic 之前在 LangChain，这次没有选边。规模小的时候「显然可以自己搭」，Claude 也能低成本地为某个具体问题做一个定制的数据查看器。

**Eval 集要多大？** Martin 没有仔细研究过：「说实话，直接问 Claude」，Claude 有一套估算规模的经验法则（skill 会预先估算 案例数 × 重复次数 × 模型数）。小的集合更容易 overfit、噪声更大，也可能代表不了生产环境。

**Agent 的 eval 用单轮还是完整轨迹？** Agent 通常需要多轮 eval。`build-eval` 可以把 trace 提升进 eval 集；给整条轨迹打分还是只看最终结果，是一个选择，命令里附了指导。

### 给模糊的输出打分

**没有明确成功指标的产品（比如 UX）怎么评估？** 能用确定性检查就用。否则，对 LLM judge 来说：写一份人也能照着打分的 rubric；「别用随意的量表……从零到五打分，非常模糊」，优先用二元判断，把多个标准拆成几个二元分类器（「从一到十给这个打分，噪声极大」）；跨模型测试（「让 Sonnet、Opus 和 Fable 都用你的 rubric 打分。如果它们意见都不一致，那是个坏信号」）；跑重复实验，确认方差低。

**Pairwise 比较和 Elo 呢？** 「Pairwise 比较其实可以相当好。」说「这个网页比那个好」比给一个网页打五分制分数容易得多。

**模型既然聪明到能用分级量表，为什么坚持二元？** 「我说得有点太绝对了。当然可以用。」只要方差低、跨模型结果能复现、人能分辨相邻两档的区别，量表就没问题。二元是一条捷径，而且 grader 往往是小而便宜的模型。

**有没有可验证性更弱的领域的例子？** 生成式 UI 的 eval，用 pairwise judge 在多次试验里比较配置 A 和 B。LLM judge 在检索、写作和给 trace 打标签上也很常见。

### Hill-climbing 实战

**能对人类反馈做 hill-climb 吗？** 能：「Hill climbing 只需要你有一个 eval，有某种 Claude 能用来判断好坏的信号。」把反馈变成这个信号就行。

**应该从哪个面开始？** 文本：prompt 和 skill 受约束，而且「和输出高度耦合」。Harness 代码改出来的 diff 很大，可能和要测的东西对不上。问题是 prompt 很容易 overfit，即使有 train/test split 也一样：「Claude 很擅长把 prompt 扭成一个在你的 eval 上表现很好、但并不代表生产情况的样子。」

**Prompt 或 SOP 什么时候该变成 skill？** 「我其实觉得直接让 Claude 来做就好」：把工作流走一遍，然后让 Claude 把它变成 skill。做 hill-climbing 时，Martin 从一个粗糙的 skill 开始，让 Claude 从那里往上爬；`claude-api` skill 起初差不多只是一张文档地图。

**怎么看 eval 的分布？** 看数据，确保案例代表生产环境。避免「adversarial sampling」（对抗式采样），也就是只拿某个模型做错的案例来建 eval：「模型是参差不齐的（jagged）」，下一个模型可能全都能做对，eval 最后变成围着某一个模型的弱点长出来的样子。挑「从第一性原理看就难或有意思」的案例。

**Hill-climbing 保证收敛吗？** 「完全不保证收敛。」卡住时，climber 会把剩下的失败聚类；最后那 20% 常常是有问题的 grader 示例或有歧义的指令，「不是 Claude 做不到更好，而是 eval 结构不好。」skill 例子里的第 2 阶段就是这么回事。

**DSPy 有用武之地吗？** Martin 很尊重 DSPy 团队和 Omar Khattab。这个 skill 是开源的，「只是一些非常简单、通用的指令，用来搭 hill climbing 循环」，所以大概可以和 DSPy 一起用，不过 Martin 自己没用过。

**预算失控怎么办？** Skill 会问预算并跟踪花费。失控的花费也是要把 eval 设计好的另一个理由：如果最后 20% 根本做不到，「你会在上面烧掉海量 token」。

**能同时优化几个指标吗？** 能，成本那个例子就同时优化了成本和性能。Martin 通常考虑成本、性能和速度，「挺合理的三件套」；目标越多问题越难，所以可以考虑「一次只改一样，串行地来」。

**跑一次要 14 个小时的任务呢？** 这正是 Martin 偏爱 prompt 这类快的面的原因。长任务也能爬，但需要一个能连续可靠运行很多小时的 harness（内部用的是 Managed Agents）：「只是基础设施的工作量大了很多。」

**这像不像在 loss function 上做 backprop？** 和训练很像，连 train/test split 都一样，但「这些算是运行时的修改」，是在模型部署之后对 prompt 和 skill 做的。

**Eval awareness 有多大影响？** Anthropic 发表过相关研究，把它当作训练层面的问题；但在这里，「更大的问题通常是 overfitting」：Claude 找到一些让数字上涨、却对生产没帮助的 prompt 技巧，并不是因为它知道自己在被测试。

**这和 Karpathy 的 autoresearch 有什么关系？** 很多动机都来自它：面对清晰、可验证的目标，模型自我纠错的能力很强。Autoresearch 针对的是 training loss 这类东西，而这条命令是为带 split 的 eval 设计的，但循环是一样的。「如果你有一个 nanoGPT repo，我肯定你能拿这个 skill 去用。」

### Effort 和 prompt 审查

**五档 effort 怎么选？** 有 eval 的话，让 climber 把模型和 effort 一起搜。原则是：「更强的模型用更低的 effort……更省 token，所以比更弱的模型用 high effort 更便宜。」Climber 先用 low effort 试各个模型，直到性能跌破 baseline；在 Martin 的测试里，Haiku 4.5 用 low effort 退化得很厉害。没有 eval 的话：「Opus 5.5，low effort。试试看。」有人问「是 low，不是 medium？」，Martin 说从 low 开始再往上调；很多人一上来就用 max，那「极其吃 token」。（Anthropic 文档写明 Opus 5.5 默认是 medium，所以 low 比默认低一档。）

**Effort 到底是什么意思？** 大致是「想多努力」。更聪明的模型用 low effort，是从更高的起点出发、花更少的 thinking token；便宜模型用 high effort 则相反。

**Effort 是硬上限吗？** 「不是硬上限。」即使是 high effort，简单问题也不会消耗大量 token。差别体现在 frontier 级别的问题上，xhigh 或 max 可能比 low 或 medium 多用很多 token，结果却差不多，「因为 Claude 会做更多工作来验证结果」。Anthropic 的文档也这么说：effort 是「a behavioral signal, not a strict token budget」（一个行为信号，而不是严格的 token 预算）。

**Prompt 审查是针对特定模型的吗？** 这些反模式对 5.5 这一代肯定是多余的；分界线比较模糊，大概在 Opus 4.5 前后。迁移到 Opus 5.5 时就该跑一遍。Skill 里有一些针对特定模型的指导；Haiku 也许还能从少量 booster 里受益，「我不确定是不是这样」。

### 可靠性与跟上节奏

**Context engineering 里，什么真正能提高可靠性？** 来自 Recursive Language Models 那项工作（Zhang、Kraska 和 Khattab）的思路：把 context 作为一个对象，和 harness 分开，harness 通过 tool 去查询它。如果 context 只是一串消息，「你做一次 compaction，被压缩掉的东西就全没了。」Managed Agents 的 session log 就是这么做的。

**我们是在围着模型的优化方式做设计吗？** 一位听众引用了一个 Opus 4.8 agent 的话：「I'm optimized to produce fluid, forward-moving, plausible output, and plausible is not verified.」（我被优化成输出流畅、不断往前推进、看似合理的内容，而看似合理不等于经过验证。）Martin 的回答把整场串了起来。很多任务不容易验证，所以得靠 eval 扛住：好的目标、清晰的打分、跨模型的一致、低方差。给模型一个结构糟糕的 eval，「它哪儿也去不了，而且这不是模型的错。」

**模型变得这么快，团队怎么跟上？** 模型变强，eval 分数应该上涨；「如果没看到上涨，我会回头质疑 eval 的结构。」写出能难住 Fable 或 Opus 5.5 的 eval 本身就很难，是「一个反复出现的挑战」。

**值得在外层循环里优化 eval 本身吗？** Martin 讲了 tracing 公司那边的一个模式：部署出去的 agent 产生 trace，online evaluator 给它们打分，低分的进入离线 eval 集，系统在这个集合上 hill-climb，通过的案例变成回归测试。「这是一个非常好的模式。」录音到这里结束。

---

**链接：** [Schedule](https://ai.engineer/nyc/2026/schedule?session=6a4e7cfd-334a-4d3b-94ed-282acc905448) · [Automating eval design and hillclimbing with Claude](https://claude.dev/blog/automating-eval-design-and-hillclimbing/)（Lance Martin）· [Scaling Managed Agents: Decoupling the brain from the hands](https://www.anthropic.com/engineering/managed-agents) · [Building effective agent automations](https://claude.dev/blog/building-effective-agent-automations/) · [Effort documentation](https://platform.claude.com/docs/en/build-with-claude/effort) · [Recursive Language Models](https://arxiv.org/abs/2512.24601)

截至撰写时，这场 workshop 还没有公开的录像。本文依据作者在现场的录音、幻灯片照片和上面列出的资料。[精确笔记](../precise/10-12-1530-unhobbling-the-frontier.md)（中文）保留了时间戳，并完整记录了演讲、幻灯片和博客之间不一致的地方。
