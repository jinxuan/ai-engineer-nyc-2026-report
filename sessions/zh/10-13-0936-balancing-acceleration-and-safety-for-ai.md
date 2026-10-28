[English](../10-13-0936-balancing-acceleration-and-safety-for-ai.md) | **中文**

# 刹车让你开得快：一家对冲基金运行 AI agent 的五个控制

*在 AI Engineer NYC 2026 上，Two Sigma 的 John Feminella 用 Citibank 误付 Revlon 贷款人 9 亿美元的事故，说明人人都按流程办事时控制照样会失效，然后讲了五个可组合的控制：让 agent 拥有真正的自主权，又不必把一切锁死。*

**Featured Keynote（Main Stage）· 2026 年 10 月 13 日（周二）· 9:36–9:54am · Simons Room (Metropolitan East)**
**讲者：** John Feminella，SVP Architecture，Two Sigma

---

John Feminella 开场先放了一页法律免责声明：承诺不推销、不兜售任何证券，并声明演讲内容都是 Feminella 个人观点，不代表 Two Sigma。「如果我不念这几句咒语，植入我体内的 5G 芯片就会爆炸，」Feminella 说。

Feminella 接着说，这页免责声明本身就说明这个行业受监管有多严。在一家满是控制措施的公司里，「看看控制失效时发生了什么总是很有意思，因为这能让你明白这些控制当初为什么存在」。于是演讲从一个失效的故事讲起。

## 1. Citibank 怎样误发了 9 亿美元

Feminella 说，2020 年 8 月之后，商业贷款协议里开始出现一条新条款。它防范的是一个离奇的场景：贷款中间的银行误把借款人欠的全部钱发给了贷款人，贷款人回复：「不用了，谢谢。你们已经还清了，我们两清，钱我们就留着了。」（这条就是 Loan Syndications and Trading Association 起草的 erroneous payment 条款，市场上俗称「Revlon blocker」。）

![Failure of controls](../../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/01.jpg)
*当事各方：借款人 Revlon，代理行 Citibank，以及两组贷款人。*

化妆品公司 Revlon 借了一笔 2016 年的大额 term loan，分给 300 多家贷款人：对冲基金、信贷基金、CLO 和其他银行。Citibank 是 administrative agent，「做的是把借款人和贷款人连起来的管道工作」。Revlon 把钱付给 Citi，Citi 再付给贷款人；「Citi 借出去的不是自己的钱，它只是在中间搬运别人的钱」。（据法院后来认定的事实，这是一笔七年期、18 亿美元的银团贷款，贷款人共 315 家。）

![August 11, 2020: the plan](../../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/02.jpg)
*「Pay everyone their interest. Move no principal.」本金应该进内部的 wash account。*

2020 年 8 月 11 日的计划很简单。Revlon 向两组贷款人支付约 780 万美元的应计利息。一组贷款人的本金滚入一个新 facility，「是换一张纸，不是付现金」，其余贷款人的本金留在 2016 年那笔贷款里。「只有利息该出去，本金完全不该动。」

Citi 用一个叫 Flexcube 的内部软件处理这笔交易（「这个企业软件名字是不是挺好玩？」）。在 Flexcube 里，最简单的记账方式是假装 Revlon 还清了整笔贷款，这样利息计算就对了。本金那部分应该打进银行自己的内部「wash account」（冲账账户），「这样账能记对，钱又不出银行的边界」。

![The screen](../../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/03.jpg)
*法院判决书里的 Figure 1：Flexcube 界面，只有 PRINCIPAL 一栏填了 wash account。*

![What the screen needed](../../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/05.jpg)
*三个字段都要填 wash account 账号，只填了一个，8.939 亿美元就出去了。*

FRONT、FUND、PRINCIPAL 三个字段都需要填 wash account 账号并打勾。「出于各种晦涩的原因」，只有 PRINCIPAL 填了。Flexcube 弹出警告，说钱要离开银行了。可钱本来就该离开银行，因为要付利息。「所以本该审批的三位审批人看了一眼，说：『对，这就是该发生的事』」，操作员越过了警告。

法院记录补上了细节。Citibank 用的是「six-eye」流程：maker 录入交易，checker 复核，一位 senior manager 终审。三人都误以为只设 PRINCIPAL 一栏就够了，尽管银行内部手册写明三个字段都必须设。终审人回邮件说：「Looks good, please proceed. Principal is going to wash.」警告框写的是：「Account used is Wire Account and Funds will be sent out of the bank. Do you want to continue?」框里没有显示金额，checker 点了 yes。

![One of the biggest blunders in banking history](../../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/06.jpg)
*wash account 被跳过，Citi 自己的 8.939 亿美元发给了贷款人。*

Flexcube 把全部未偿本金发给了每一家贷款人：Feminella 说是「约 9 亿美元，而不是 800 万」。（slide 写 8.939 亿美元，法院给出的数字是 $893,944,008.52。）slide 标题引用了后来审理此案的联邦法官的话：「one of the biggest blunders in banking history」（银行史上最大的失误之一）。每家贷款人收到的，恰好是 Revlon 全额还款时它应得的数，而 Revlon 根本没打算还清。

对贷款人来说这是天上掉的钱。Revlon 当时有麻烦，贷款远低于面值交易，「你以为只值 60 美分一美元的债，结果全额拿回来，对你来说是笔好买卖」，Feminella 说。（上诉法院后来说这笔贷款当时以 20 到 30 美分一美元交易。）

![Please give the money back](../../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/07.jpg)
*Citi 请求还钱，贷款人回答「NO」。*

Citi 打电话请求退款，很多贷款人拒绝，理由是贷款本来就要还，提前还也是还。于是有了那条新条款。Feminella 没讲案子的结局：2021 年 2 月，地区法院判贷款人可以留下未退还的约 5 亿美元；2022 年 9 月，第二巡回上诉法院推翻了这一判决。

Feminella 从中得出的教训是：「银行有这套流程，有这些控制，每个人都做了自己认为对的事。问题就在这里。」现在企业每天都在给真实的 agent 对 Flexcube 这类系统的真实自主权。问题是怎样防止这样的事故，「既不把一切锁死，也不靠碰运气」。

## 2. 安全和创新不必二选一

「你没法事先确切知道 agent 会做什么，」Feminella 说。这听起来吓人，「直到你想起人类也是非确定性系统，人也会犯错」。公司不会为了防止员工犯错把人锁在房间里，而是给自主权，同时给边界、可见性，以及让人高效工作的系统。「安全和创新不是一条光谱的两端，不必取舍……我认为鱼和熊掌可以兼得。」

![Is safety a trade-off?](../../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/08.jpg)
*「Innovate without permission slips.」「Too little safety: existential risk. Too little innovation: slow death.」*

![What usually doesn't work](../../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/09.jpg)
*Two Sigma 否掉的四种做法。*

Feminella 列出了在 Two Sigma 行不通的做法：

| 做法 | slide 上的说明 |
| --- | --- |
| Sandbox everything | 更安全，但 agent 的价值往往来自它能接触和组合的东西 |
| Lock everything down | 非常安全，但也不太有用；会催生你看不见的 shadow agents |
| Let agents go wild | 「Gas Town-alikes」（类似 Steve Yegge 的多 agent 编排器）开箱的容器化很差，不适合企业使用 |
| Policy docs alone | 用来表明立场有帮助，但单靠它们无法在 runtime 强制执行 |

没有真实访问权限的 sandbox「安全，但没什么意思，也没什么用」。放任 agent 为所欲为，隔离「非常弱」，不适合高度监管的公司。写在 prompt 里的 policy 文档「在 runtime 不太能强制执行」。

行得通的是一组控制的组合：「让团队能按自己的方式前进、不受阻碍的 guardrails 和共享能力」。Feminella 强调，「这些控制是可组合的，每个只干一件事」，合起来能让 agent「以合适的安全水平拥有真正的自主权」。

![Controls: composable](../../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/10.jpg)
*五个控制，各管一件事。*

| 控制 | slide 上写的职责 |
| --- | --- |
| Agent Registry | Catalog every agent: tools, endpoints, owner, product |
| Risk Factor Scoring | Score each agent's risk at registration and on every update |
| Activity Controls | Prevent anticipated risky egress or actions |
| Evaluations | Use shared approaches for common evaluations |
| Auto Mode Classifiers | Prevent unanticipated risky egress or actions |

每个控制的介绍方式都一样：一个问题，一个诱人的「错误控制」，再给出解法。

## 3. 控制 1：agent registry

![Problem 1: agent sprawl](../../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/11.jpg)
*各团队的 agent，还有一个没有团队的：谁拥有、有什么工具、有什么权限？*

agent 到处冒出来：大团队有很多，小团队有几个，个人「在不受控的情况下跑东西」。形态五花八门：「随手做的 chatbot」「像弗兰肯斯坦一样东拼西凑的 MCP servers」、notebook 实验、好用的 web 工具。回答不了几个简单问题（谁拥有它、它有什么工具、它该干什么），公司就既没法治理也没法复用，各团队重复造同样的东西。

诱人的做法是设一道闸：所有东西都要走一套繁琐流程登记、审批后才能运行。结果连 notebook 实验都要排在真正有风险的东西后面，大多数人要么放弃，要么转向「某种 shadow IT，治理更少的路径」，公司就什么都看不见了。

![Control: Agent Registry, automatic front door](../../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/12.jpg)
*agent 登记自己的 endpoints、tools 和 metadata；registry 把它们和产品关联，人和 agent 都能查询。*

「看不见的东西就没法治理，」Feminella 说。registry 记录每个 agent 的工具、通信的 endpoint、所属产品和 owner。人、程序和其他 agent 都可以查询它，了解有什么可用，并把它当作 source of truth。登记不需要额外工作：用公司批准的 agent 框架搭建，就会自动登记，也可以手动登记。（Feminella 配套的文章补充说，定制的 agent 会被启发式发现、自动登记，并通知其 owner。）

## 4. 控制 2：风险评分

![Problem 2: one-size review](../../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/13.jpg)
*summarizer、doc drafter 和生产部署 agent 都在同一个审核队列里等几周。*

![Wrong control: committee review for every agent](../../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/14.jpg)
*security、legal、risk、committee，每关两周：一个琐碎的 agent 要「8+ weeks to ship」。（照片被视频播放器控件挡住了一部分。）*

agent 的风险差别很大。一个总结 Jira 或 Linear 工单的 bot，和一个能碰运行中系统的部署 agent 不是一回事。让两者走同一套审核，每个 agent 都要等几周，人们就会绕开流程。让所有相关部门都做繁重的人工审核，「感觉安全，但现在一个琐碎的 summarizer 要等两个月，拿和碰生产系统的 agent 一样的签字」。大多数有价值的 agent 想法起初都是小实验，而「审核人被这些低风险请求淹没」，高风险的请求还得和它们抢注意力。

出路在于：企业里的大多数 agent 是专门为某个用途搭的。「像 Pi 或 Claude Code」这样的开放式 coding agent 不知道你下一句要问什么，但公司里部署的 agent 通常有「非常具体的用途」，可以凭它们事先声明的东西（工具、endpoint、控制）区分开。

![Control: Risk scoring applies differentiated scrutiny](../../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/15.jpg)
*registry 里的 risk assessor 在登记时和每次更新时给 agent 打分。*

slide 画了流程：agent 登记或更新，registry 里的 risk assessor 打分，分数经过验证，agent 发布。打分依据是「tools requested, purpose, and systems touched」（申请的工具、用途和接触的系统），风险极高的 agent 可能在发布前被标记。（据 Feminella 的文章，低风险 agent 发布无需人工介入。）

## 5. 控制 3：工具跟着用途走

![Problem 3: differentiated access](../../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/16.jpg)
*agent 可能需要的六类访问，从 role account 到外部网页。*

agent 很少只碰一样东西。它可能用 role account、调内部 API endpoint、运行 web search 或 SQL 之类的工具、调用其他 agent、读数据存储、访问外部网页。「每一样……都是通向另一种风险的另一扇门。」拿到 role account 权限的 agent，能造成的破坏远大于只读公开网页的 agent。「问题不是这个 agent 能不能做 X，而是它需要多细的粒度。」

![Wrong control: block all outbound access](../../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/17.jpg)
*封掉所有出站访问：「Safe, but useless.」*

给每个 agent 同一份简短的 allow list、其余全封，「安全，但也没用」。团队要么放弃，要么想办法翻墙，「两种结果代价都很高」。Feminella 引用了作家 John Shedd 的话：「A ship in harbor is safe, but that is not what ships are for.」（船停在港里是安全的，但那不是造船的目的。）（这句出自 Shedd 1928 年的格言集《Salt from My Attic》，原文是「built for」。）

![Fix: access depends on resource and agent purpose](../../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/18.jpg)
*同一个工具，按 agent 声明的用途给出不同答案。*

解法是让 agent 声明用途和所需工具：

| Agent（用途） | Agent tool | External web | Data store | API endpoint | Other agent | Role account |
| --- | --- | --- | --- | --- | --- | --- |
| summarizer（research） | Allow | Allow | Allow | Deny | Allow | Deny |
| deployer（release ops） | Gated | Deny | Gated | Allow | Gated | Gated |

做研究的 summarizer 可以用 web 工具；release deployer 没理由用，所以默认拿不到。「作为 agent 的作者，你最有资格说：『可用的 500 样里我需要这 4 样。』」

![Control: Activity controls](../../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/19.jpg)
*在容器里，egress proxy 放行通往批准目的地的流量，拦下其他的。*

强制执行发生在 runtime。agent 在受管环境里启动时，从 registry 查到自己的 policy，policy 被转换成可执行的规则：能访问哪些 endpoint 和 host。slide 画了 pod 里 agent 旁边的 egress proxy，放行到批准的目的地，拦下其他目的地。「没人需要每次改权限都提工单。」只要 agent 符合它的风险分数、在 registry 里，「你就受治理、已获批」。（文章补充说，proxy 不信任 agent 代码，并且 fail closed。）

## 6. 控制 4：共享 evals

各团队造了很多不同的 agent 评估方式，有的团队什么都没有。「如果团队不写 unit test，我们会不高兴，对吧？我们觉得 evals 差不多是一回事。」模型就像一个 runtime 依赖；agent 换模型时，只有 eval 能告诉你输出质量会怎样。

![Wrong control: central eval gate for every change](../../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/20.jpg)
*每个 typo、prompt、tool、config 改动都在中央闸口等几周：「Innovation stalls.」*

如果中央评估流程要等全套 eval 跑完、委员会签字，那「每次 prompt 微调、每个一行改动都会排在同一个队列里」。

![Control: Shared eval framework](../../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/21.jpg)
*左：每个 agent 各自重造 eval processing、integration 和 trace ingestion。右：共享框架承担通用部分。*

好的 eval 里大部分东西对每个 agent 都一样：eval processing、integration、trace ingestion。所以 Two Sigma 有「一套共享的 eval 方式……由公司运行，所有人都能用」，「你只需要写出关于你的 agent 什么是对的」。（文章补充说，结果跨团队汇总，用来看模型在本公司语境下的表现；每次改动都跑 eval，像 CI/CD。）

## 7. 控制 5：用 classifier 代替审批疲劳

![Problem 5: approval fatigue](../../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/22.jpg)
*一摞「Allow?」提示，一排批准，其中一个高风险操作被顺手批了。*

逐个审批 agent 的操作，在小规模时可行。一旦 agent 并行运行或进入生产，「审批请求来得比任何人能批的都快」，审核人就会疲劳：「yes, yes, yes, yes, yes，最后你在一个，比如说，启动新核弹的东西上也点了 yes。」

Feminella 举了一个最近的例子。今年早些时候，一家 AI 研究机构的一位 director 让 OpenClaw agent 建议要删除的邮件，并等她批准。邮箱太大，agent 压缩了上下文，丢掉了「等待批准」这条指令，删了几百封邮件，她从手机上叫停也停不下来。（Feminella 没点名；这件事是 2026 年 2 月 Meta Superintelligence Labs 的 alignment director Summer Yue 公开讲的，她说删掉了 200 多封邮件。）

![Fix: let classifier decide, escalate only what matters](../../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/23.jpg)
*多数调用放行，少数交给人，罕见的危险调用拦下。*

更好的办法是让「独立、保守的 classifier」处理例行调用，只把最可能要紧的请求交给人。「如果你 99.99999% 的时候都说 yes，那它大概就该是 agent 遵守的一条规则，而不是需要问人要许可的请求。」不明确或可能危险的请求交给人；明显危险的直接拒绝。有显式规则时由确定性的 fast path 决定；没有时由 classifier 决定。

![Control: Auto-mode classifier — allow, soft or hard deny](../../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/24.jpg)
*来自 settings 的确定性快速路径，其余交给两阶段 classifier，结果还要过一道 prompt injection 检查。*

| 环节 | slide 上的内容 |
| --- | --- |
| 触发 | 主模型在 auto 权限模式下提出一个 tool call |
| Fast path（确定性） | `~/.claude/settings.json` 里的显式规则：allow 或 deny |
| Slow path（非确定性） | classifier 分两阶段：stage 1 是偏向拦截的快速分诊，stage 2 对不确定的情况做带「thinking」的深入审查；结果是 allow、soft deny（「agent may clarify」）或 hard deny |
| 执行与检查 | 允许则运行工具；prompt injection probe 检查结果，再加入 transcript |

这张图用的是 Claude Code auto mode 的文件路径和命令名，不过 Feminella 在台上没有点名。Anthropic 介绍 auto mode 的工程文章描述的是同一设计：第一阶段的快速过滤器「被指示偏向拦截」，被标记的操作进入第二阶段推理，另有一个 probe 检查工具输出里的 prompt injection。

## 8. 车为什么有刹车

Feminella 把五块拼回一起。registry 让每个 agent 从创建那天起就可见。风险评分让人工审核用在该用的地方。guardrails 让 agent 只碰它需要的东西。共享 evals 和指标让大家达到同一标准，不必重造轮子。在 runtime 治理，确保分类在那里发生，「而不是别的地方」。

「车为什么有刹车？」Feminella 问。「It's not just so that you can stop, it's so that you can go fast and stop safely.」（不只是为了能停下，而是为了能开得快、又能安全停下。）「我们给 agent 的控制也是一样的道理。」

---

**链接：** [日程](https://ai.engineer/nyc/2026/schedule?session=73badd10-6f8c-4c95-b5e7-55fc3d7bafc1) · [视频](https://www.youtube.com/watch?v=iniryju23Sk) · [直播（第一天，从 23:57 起）](https://www.youtube.com/watch?v=akEqiXFo3GM&t=1437s) · [Brakes Let You Go Fast](https://dev.to/dailycontext/brakes-let-you-go-fast-balancing-acceleration-and-safety-for-ai-agents-49p9)（Feminella 的文章）· [In re Citibank August 11, 2020 Wire Transfers](https://www.nysd.uscourts.gov/sites/default/files/2021-02/20cv6539%20Citibank%20Opinion.pdf)（纽约南区联邦地区法院判决，2021）· [Revlon decision reversed on appeal](https://www.torys.com/our-latest-thinking/publications/2022/09/revlon-erroneous-payment-decision-reversed-on-appeal)（Torys）· [How we built Claude Code auto mode](https://www.anthropic.com/engineering/claude-code-auto-mode)（Anthropic）· [OpenClaw 邮箱事件](https://techcrunch.com/2026/02/23/a-meta-ai-security-researcher-said-an-openclaw-agent-ran-amok-on-her-inbox)（TechCrunch）· [Quote origin: "A ship in harbor is safe"](https://quoteinvestigator.com/?p=7781)（Quote Investigator）

本文依据官方视频字幕、第一天直播、slide 照片和以上资料整理。这场演讲没有 Q&A。[精确版笔记](../precise/10-13-0936-balancing-acceleration-and-safety-for-ai.md)（中文）保留了时间戳，以及演讲、slide 和法院记录之间出入的完整记录。
