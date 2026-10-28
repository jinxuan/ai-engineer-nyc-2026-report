# Balancing Acceleration and Safety for AI Agents：精确版

> Precise notes for this session, compiled from the official video captions, the official livestream, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材整理完，单独视频和直播逐段对照完，Citibank/Revlon 案情按法院判决书核对过。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | 作者拍的 24 张（01–24），全部是这场：直播画面的手机翻拍，下方横幅「Balancing Acceleration and Safety for AI Agents · John Feminella / SVP Architecture · TWO SIGMA」。04 和 03 是同一页（04 上盖着一条手机通知）；14 上盖着 YouTube 播放器控件，顶部能看到直播标题「AI Engineer NYC Livestream \| Bridgewater, Anthropic, Two Sigma, Ari…」。slide 右下角有页码，拍到的最大是 35；开场的免责声明页、合同条款页、「Problem 4」页和结尾页没拍到 |
| 视频 | [iniryju23Sk](https://www.youtube.com/watch?v=iniryju23Sk)，改名「What a $900M Bank Error Teaches Us About AI Agents - John Feminella, Two Sigma」，18:56，上传 2026-10-27。字幕是主要文本 |
| 直播 | 第一天直播 [akEqiXFo3GM](https://www.youtube.com/watch?v=akEqiXFo3GM&t=1437s)，主持人介绍在 23:57，演讲 24:23–43:02。直播时间 ≈ 视频时间 + 24:14（视频 01:48「Revlon」= 直播 26:02）。视频剪掉了开头一句「Hey everyone. All right, I precariously balanced my water over there.」（直播 24:23），其余逐句一致 |
| 讲者文章 | DEV Daily Context 转载的讲者文章 [Brakes Let You Go Fast: Balancing Acceleration and Safety for AI Agents](https://dev.to/dailycontext/brakes-let-you-go-fast-balancing-acceleration-and-safety-for-ai-agents-49p9)（John Feminella，Oct 12），讲五个控制，不含 Citibank 故事 |
| 官方 talk 页 | ai.engineer/talks/iniryju23Sk 返回 404 |
| 案情一手资料 | 一审判决 [In re Citibank August 11, 2020 Wire Transfers, No. 20-cv-6539 (JMF)](https://www.nysd.uscourts.gov/sites/default/files/2021-02/20cv6539%20Citibank%20Opinion.pdf)（S.D.N.Y.，Jesse M. Furman 法官，2021-02-16）；二审推翻的报道 [Torys](https://www.torys.com/our-latest-thinking/publications/2022/09/revlon-erroneous-payment-decision-reversed-on-appeal)、[McGuireWoods](https://www.mcguirewoods.com/client-resources/Alerts/2022/9/i-cant-get-no-satisfaction-2nd-circuit-reverses-remands-revlon-decision)（第二巡回法院判决原文没打开） |

视频章节（资料：YouTube 说明）：0:00 Intro · 0:43 When controls fail · 1:03 The $900M Revlon payment mistake · 4:58 What this has to do with AI agents · 5:22 Humans aren't deterministic either · 6:07 Approaches that don't work · 7:32 Control 1: An agent registry · 9:47 Control 2: Risk-based review · 11:47 Control 3: Tools follow purpose · 14:42 Control 4: Shared evals · 16:17 Control 5: Ending approval fatigue · 18:07 Putting it all together · 18:32 Why cars have brakes

## 基本信息

- 时间地点：2026-10-13（周二）9:36–9:54am，Simons Room (Metropolitan East)，Main Stage，Featured Keynote（资料：[日程](https://ai.engineer/nyc/2026/schedule?session=73badd10-6f8c-4c95-b5e7-55fc3d7bafc1)）
- 原题「Balancing Acceleration and Safety for AI Agents」；视频改名「What a $900M Bank Error Teaches Us About AI Agents」；讲者文章题为「Brakes Let You Go Fast」
- 讲者：John Feminella，SVP Architecture，Two Sigma（横幅、日程）。讲者说（视频 00:12）：「I work on AI architecture and engineering at Two Sigma, which is a hedge fund here in New York」
- 资料：日程 bio：在 Two Sigma 负责大规模 AI 工具和基础设施；之前在 Pivotal、ThoughtWorks、VMware、EY 任技术高管；参与过美国 EPA 的 National Emissions Inventory、联合国追踪难民援助的系统、一个报税平台，以及美联储的即时支付系统 FedNow；住在弗吉尼亚州 Charlottesville
- 主持人介绍（直播 23:57）：「And now to explore how serious organizations can accelerate innovation without compromising trust. Please welcome John Feminella, SVP of architecture at Two Sigma.」
- 官方简介要点：agent 越有用、越强，潜在风险也越大；「too little safety means highly regulated firms are exposed to undue risk; too little innovation means agents cost more to run than they earn」；讲 Two Sigma 用过且有效的技术做法，希望说服听众「there doesn't need to be a tradeoff between these two properties」
- 讲者说（视频 00:20）：先放一页法律免责声明：「One, I promise not to sell or market or offer you any securities at any point in this talk. And two, everything in this is my own opinion and not the opinion of Two Sigma」；「If I don't say those magic words, the 5G microchips implanted in me will explode」

## 1. 控制失效的故事：Citibank 和 Revlon，2020 年 8 月

- 讲者说（视频 00:44）：受监管公司有很多控制，看控制失效时发生了什么，「gives you a good idea about why the controls were there in the first place」
- 讲者说（视频 01:05）：2020 年 8 月之后，一种合同条款突然出现在很多商业贷款协议里，防止这种情况：中间的银行在「a catastrophic mistake」里把借款人欠的全部钱都打给了贷款人，贷款人却说「No, thanks. You paid us back」（用柠檬水摊借钱打比方）
- 资料：这类条款即 LSTA 起草的 erroneous payment provisions（俗称「Revlon blockers」），市场在一审判决后迅速加进信贷协议（[Torys](https://www.torys.com/our-latest-thinking/publications/2022/09/revlon-erroneous-payment-decision-reversed-on-appeal)、[McGuireWoods](https://www.mcguirewoods.com/client-resources/Alerts/2022/9/i-cant-get-no-satisfaction-2nd-circuit-reverses-remands-revlon-decision)）
- slide（照片 01）「Failure of controls:」Revlon（Borrower）→ Citibank（Agent）→ Primary lender group / Remaining lenders
- 讲者说（视频 01:48）：Revlon 在「a big 2016 term loan」下借了钱，贷款拆给「300-plus lenders, hedge funds, credit funds, CLOs, other banks」；Citibank 是 administrative agent，「performing the plumbing that connects the borrowers with the lenders」，「City's not lending its own money」
- 资料（一审判决 p.4）：2016 年 Revlon 借了一笔七年期、18 亿美元的银团贷款（2016 Term Loan），Citibank 任 Administrative Agent；判决引用 Citibank 员工邮件：「Each of the 315 lenders received their pro rata share of 893,944,008.52」
- slide（照片 02）「August 11, 2020: the plan · Pay everyone their interest. Move no principal.」Revlon →「$7.8M interest」→ Citibank（Agent: plumbing）→ interest → Primary lender group（Principal rolls into new facility）、Remaining lenders（Principal stays in 2016 loan）；下方虚线框「Wash account · Goal: send principal here, disburse interest」
- 讲者说（视频 02:22）：8 月 11 日的计划：Revlon 付「about 8 million bucks of accrued interest」给两组贷款人，一组的本金滚入新 facility（「an exchange of paper, not cash」），其余贷款人本金不动，「only interest should be going out, principal should not be moving at all」
- 讲者说（视频 02:44）：银行内部处理软件叫 Flexcube（「Isn't that a fun enterprise software name?」）；在 Flexcube 里最简单的做法是假装 Revlon 还清了整笔贷款，这样利息计算正确；本金部分应该打到银行自己的内部 wash account，「so that it's booked and recorded correctly, but it never leaves the bank's perimeter」
- 资料（一审判决 p.11–12）：在 Flexcube 上最简单的做法是把贷款当作全额偿还记账再重建；本金部分要打进 wash account，即「an internal Citibank account that shows journal entries … used for certain Flexcube transactions」；内部 Fund Sighting Manual 写明，要压住本金付款，「ALL of the below field[s] must be set to the wash account: FRONT[;] FUND[; and] PRINCIPAL」
- slide（照片 03）「The screen」：判决书里的「FIGURE 1」，即 Flexcube 截图：Facility Name「REVLON TERM LOAN 2016」，GL Detail 里 FRONT、FUND、INTEREST、PRINCIPAL 几行被黄框标出，只有 PRINCIPAL 填了 3003000023 并打勾
- slide（照片 05）「What the screen needed · Three fields needed wash account. Only one set.」FRONT ✗、FUND ✗ →「$893.9M out」；PRINCIPAL 3003000023 ✓ → Wash account；脚注「3003000023 = Citi wash account」
- 讲者说（视频 03:19）：front、fund、principal 三个框都应填 wash account 号并打勾，「for various arcane reasons」没打；Flexcube 弹出警告「money's about to leave the bank. Are you sure?」，但钱本来就该出去（付利息），所以「the three approvers who were supposed to approve that looked at it and said, "Yep, that's the right thing"」，操作员 override 了警告
- 资料（一审判决 p.12–14）：Citibank 的「six-eye」审批要三人：maker（Santhosh Kuppusamy Ravi）录入，checker（Arokia Raj）复核（两人都是专为 Citibank 工作的印度 Wipro 公司员工），approver（Vincent Fratta，Delaware 的 senior manager）终审；三人都错误地以为只把 PRINCIPAL 设成 wash account 就够了；Fratta 回邮件「Looks good, please proceed. Principal is going to wash.」；Raj 点确认时弹出「stop sign」：「Account used is Wire Account and Funds will be sent out of the bank. Do you want to continue?」，警告没显示金额，Raj 点了「YES」
- 讲者说（视频 04:02）：本金没进 wash account，Flexcube 把全部未偿本金发给了「all of the 300 lenders, about $900 million instead of $8 million」
- 资料（一审判决 p.2）：「Instead, it made one of the biggest blunders in banking history: It mistakenly wired, in addition to Revlon's $7.8 million, almost $900 million of its own money as well」，付款额「to the penny」等于 Revlon 欠的本息
- slide（照片 06）「"One of the biggest blunders in banking history"」：Citibank →「$893.9M · Citi's own money」→ 两组贷款人；Revlon 灰掉；Wash account「Skipped」
- 讲者说（视频 04:14）：对贷款人是好买卖，因为 Revlon 当时有麻烦，贷款不按面值交易，「getting all your money back on a deal that you thought was only worth 60 cents on the dollar is a great deal for you」
- 资料：第二巡回法院指出这笔贷款当时以「20-30 cents on the dollar」交易（[McGuireWoods](https://www.mcguirewoods.com/client-resources/Alerts/2022/9/i-cant-get-no-satisfaction-2nd-circuit-reverses-remands-revlon-decision) 转述）。和讲者的 60 美分不一致，见「出入」
- slide（照片 07）同标题：Citibank「Please give the money back」→ Lenders，回一张写着「NO」的图
- 讲者说（视频 04:35）：Citi 打电话说是错误，贷款人不还，理由是「you paid us in full, and that's how loans work」，只是还得比约定早；所以商业贷款协议都开始加那条条款
- 资料（一审判决）：部分贷款人退回了，约 5.04 亿美元没退（Citibank 起诉追回 $504,169,401.45）；Furman 法官 2021-02-16 依 discharge-for-value 规则判贷款人可以留下。资料（Torys）：第二巡回法院 2022-09-08 推翻一审、发回重审，贷款人须退还。讲者没讲诉讼结果
- 讲者说（视频 04:58）：「the bank had this procedure, it had these controls, and everybody did what they thought the right thing to do was. That's the problem」；每天都在给真实的 agent 对 Flexcube 这类系统的真实自主权，怎样防止事故，又不至于「locking everything down or just hoping we'll get lucky」

## 2. 安全和创新不是二选一

- 讲者说（视频 05:23）：「You can't really know in advance what an agent will do」，听起来吓人，「until you remember that humans are also non-deterministic systems」；公司不会为了减少人犯错就把人锁在房间里，而是给自主权加控制：「boundaries, visibility」以及让人能高效工作的系统；「safety and innovation are not opposite ends of a spectrum … I think you can have your cake and eat it, too」
- slide（照片 08）「Is safety a [tradeoff]」（标题截断）：天平一端 Safety（锁）、一端 Innovation（闪电），支点下写「Innovate without permission slips」；底部「Too little safety: existential risk. Too little innovation: slow death.」
- slide（照片 09）「What usually doesn't work」：

  | 做法 | slide 说明 |
  | --- | --- |
  | Sandbox everything | Safer, but an agent's value often comes from what it can touch and combine |
  | Lock everything down | Very safe but also not very useful; encourages shadow agents you can't see |
  | Let agents go wild | Gas Town-alikes have poor containerization out of the box, aren't suitable for enterprise use |
  | Policy docs alone | Helpful for staking a claim, but not enforceable at runtime by themselves |

- 讲者说（视频 06:11）：没有真实系统访问的 sandbox「is safe, but it's not very interesting or useful」；放任 agent「the isolation is very weak there」，不适合高度监管的企业；只有 policy 文档「not very enforceable at runtime」
- 资料：Gas Town 是 Steve Yegge 2026 年 1 月发布的开源多 agent 编排器（[heise](https://www.heise.de/en/background/Full-Control-Gas-Town-Orchestrates-Ten-or-More-Coding-Agents-11178824.html?view=print)）；讲者口头没点名，只在 slide 上
- 讲者说（视频 07:02）：讲五个控制，「what I mean are the guardrails and shared capabilities that let teams move in ways that don't hinder what they're trying to do」；「these controls are composable. Each one does one job」
- slide（照片 10）「Controls: Composable, …」（标题被截图提示遮住）：

  | 控制 | slide 说明 |
  | --- | --- |
  | Agent Registry | Catalog every agent: tools, endpoints, owner, product |
  | Risk Factor Scoring | Score each agent's risk at registration and on every update |
  | Activity Controls | Prevent anticipated risky egress or actions |
  | Evaluations | Use shared approaches for common evaluations |
  | Auto Mode Classifiers | Prevent unanticipated risky egress or actions |

## 3. 控制 1：Agent Registry

- slide（照片 11）「Problem 1: agent sprawl」：Team A 一堆 agent、Team B、Team C、一个「no team」的 agent；三个问题「Who owns? What tools? What access?」
- 讲者说（视频 07:34）：agent 到处冒出来：大团队、小团队、个人「running stuff in a uncontrolled way」；形态各异，「random chatbots … MCP servers glued together in this Frankenstein hotchpotch … cool notebook experiments」；回答不了谁拥有、有什么工具，就没法治理，也没法复用，会重复造轮子
- 讲者说（视频 08:23）：常见做法是加闸门，全部注册审批后才能跑；结果连 notebook 实验也排在高风险项目后面，人们要么放弃，要么转向「some shadow IT, less governed path」
- slide（照片 12）「Control: Agent Registry, automatic front door」：Agent（bound endpoints · declared tools · metadata）→ Agent Registry（「registers, retrieves, discovers key information about agents, tools, dependencies」）→ Product Catalog（「tie agents to related products」）；右侧「usable interactively, programmatically, agentically by humans and other agents」
- 讲者说（视频 08:58）：「You can't govern what you can't see」；registry 记录它用哪些工具、和哪些 endpoint 通信、属于哪个产品、谁拥有；人、程序、其他 agent 都能查；「It just happens when you use one of the firm's approved agent building frameworks, it's automatically registered」，也可以手动注册
- 资料（讲者文章）：用批准模板建的 agent 自动注册；定制的 agent 靠启发式检测后自动注册，并通知 owner

## 4. 控制 2：Risk Factor Scoring

- slide（照片 13）「Problem 2: one-size review」：summarizer、doc-drafter、prod-deployer（风险依次升高）都排进同一个「weeks」队列 → Review → Production；虚线「route around」
- slide（照片 14）「Wrong control: committee review for every agent」：trivial agent summarizer → Security → Legal → Risk → Committee → Production，每步「+2 wk」，合计「8+ weeks to ship」
- 讲者说（视频 09:46）：总结 Jira/Linear 工单的 agent 和能碰运行中系统的部署 agent 风险完全不同；同一套审核会让所有 agent 排队几周，人们就绕开流程；「now we've got a trivial summarizer waiting 2 months for the same sign-offs as an agent that is touching production systems」；「the reviewers are drowning in these like low-risk requests」
- 讲者说（视频 10:58）：开放式 agent（「coding agents like Pi or Claude Code」）不知道你要问什么，但企业里部署的大多数 agent「do have very specific purposes」，可以凭它们事先声明的工具、端点、控制区分
- slide（照片 15）「Control: Risk scoring applies differentiated scrutiny」：register or update → Agent Registry（Risk Assessor → Scoring）→ Validate risk score → publish。右侧：「Agents auto-registered on creation · Scored at registration and on each update, by tools requested, purpose, and systems touched · Very high-risk agents may be flagged before publishing」
- 资料（讲者文章）：分数结合观测因素（如工具使用）和定性因素（如用途）；低风险 agent 无需人工即可发布，高风险的标记待审
- 我的理解：讲者口头讲控制 2 时主要讲问题和「声明用途」，没有逐条念照片 15 的流程

## 5. 控制 3：Activity Controls（工具跟着用途走）

- slide（照片 16）「Problem 3: differentiated access」：Agent → Role account（svc-reports, svc-trading, svc-etl, …）· API endpoint（GET /orders, POST /trades, …）· Agent tool（web_search, run_sql, …）· Other agent（summarizer, planner, …）· Data store（orders_db, research_lake, …）· External web（*.example.com, api.vendor.io, …）
- 讲者说（视频 11:43）：越开放的 agent 越像 Claude Code 或 Pi，能做的事越多，每一种都是「a different door to a different kind of risk」；给 agent 一个 role account 的权限比只读公开网页危险得多；问题不是能不能做 X，而是「what's the granularity」
- slide（照片 17）「Wrong control: block all outbound access」：Agent 被框住「Block all outbound」，所有连线打叉；「Safe, but useless」
- 讲者说（视频 12:29）：给所有 agent 同一份短 allow list、其余全封，「that's safe, but it's also useless」；团队要么放弃要么绕墙；「as author John Shedd wrote, "A ship in harbor is safe, but that is not what ships are for."」
- 资料：Quote Investigator 考证这句出自 John A. Shedd 1928 年的格言集《Salt from My Attic》，原文作「…not what ships are built for」（[Quote Investigator](https://quoteinvestigator.com/?p=7781)）；讲者文章里也引了这句
- slide（照片 18）「Fix: access depends on resource and agent purpose」：

  | agent（purpose） | Agent tool | External web | Data store | API endpoint | Other agent | Role account |
  | --- | --- | --- | --- | --- | --- | --- |
  | summarizer（research） | Allow | Allow | Allow | Deny | Allow | Deny |
  | deployer（release ops） | Gated | Deny | Gated | Allow | Gated | Gated |

  脚注「Same tool, different answer」
- 讲者说（视频 13:11）：让 agent 事先声明用途和需要的工具；research summarizer 可以用 web 工具查资料，release deployer 不行，「they have no reason to use that tool」；「as the agent author, you're in the best position to say, "I need these four things out of the 500 that are available"」
- slide（照片 19）「Control: Activity controls」：Agent Registry ↔ Compute Platform；container environment 里 Pod 中的 Agent Instance 经 Egress Proxy 出去，到 Approved Destination 放行，到 Blocked Destination「Blocked!」
- 讲者说（视频 13:55）：agent 在受管环境里运行时，初始化时查 registry 拿到适用的 policy，翻译成可执行的规则（能访问哪些 endpoint、哪些 host）；「nobody's writing a ticket every time they need to change the access」；只要符合风险分数、在 registry 里，「you're governed, you're approved」
- 资料（讲者文章）：agent 跑在只放行批准目的地的 proxy 后面，proxy 不信任 agent 代码、fail closed，policy 直接取自 registry

## 6. 控制 4：Shared Evaluations

- 讲者说（视频 14:46）：各团队造了很多套评估方式，有的什么都没有；「we'd be upset if teams were not writing unit tests, right? And we consider evals to be kind of the same thing」；模型变化「is like a runtime dependency」
- slide（照片 20）「Wrong control: central eval gate for every change」：Queue（typo · prompt · tool · config · +40）→ Central eval gate（full suite, every change）→ weeks → Release；「Innovation stalls」
- 讲者说（视频 15:23）：中央评估要等全套 eval 和委员会签字，「every like prompt tweak, every one-line change is going to sit in that same queue」
- slide（照片 21）「Control: Shared eval framework」：左（哭脸）「Good evals need many parts, mostly same for every agent」，每个 agent 自己做 eval processing、eval integration、trace ingestion、agent logic；右（笑脸）「Eval framework does common work; agent authors do less」，agent 只写 agent logic，其余由 Eval Framework 承担
- 讲者说（视频 15:57）：「we want to have one shared approach to how we do evals … run by the firm and available to everybody」，「all you've got to do is just write what's true about your agent」
- 资料（讲者文章）：结果跨团队汇总，显示模型在本公司语境下的表现（公开 benchmark 和具体公司对应很差）；框架负责 trace 收集和 pipeline，每次改动都跑 eval，像 CI/CD
- 「Problem 4」那页没拍到【待确认】标题

## 7. 控制 5：Auto Mode Classifiers

- slide（照片 22）「Problem 5: approval fatigue」：一摞「Allow?」提示 → Reviewer → 一串对勾，其中一个红色「Risky action approved」；「Yes, yes, yes …」
- 讲者说（视频 16:17）：人在环中逐个审批，小规模可以，并行或生产中「review requests come in faster than anybody can be expected to approve them」；「you succumb to review fatigue … eventually you click yes on something that's, you know, activating a new nuclear bomb」
- 讲者说（视频 16:52）：「earlier this year a director at an AI research lab told her open claw agent to suggest emails to delete from her inbox and then wait for her approval」，邮箱太大，agent 压缩上下文时丢了「Wait for my approval」，删了几百封邮件，从手机叫停也停不下
- 资料：2026 年 2 月，Meta Superintelligence Labs 的 alignment director Summer Yue 公开讲了 OpenClaw 删除她 200 多封邮件的事：compaction 丢了「不要在我批准前行动」的指令，她无法从手机叫停，只好跑去 Mac mini 上停掉（[TechCrunch, 2026-02-23](https://techcrunch.com/2026/02/23/a-meta-ai-security-researcher-said-an-openclaw-agent-ran-amok-on-her-inbox)；[Simon Willison](https://simonwillison.net/2026/feb/23/summer-yue)）。讲者没点名
- slide（照片 23）「Fix: let classifier decide, escalate only what matters」：一列 tool call → Classifier → Allow（many: run now）、Ask human（few: human decides）、Block（rare: dangerous）
- 讲者说（视频 17:17）：让「independent, conservative classifiers」处理例行调用，只把最重要的请求交给人；「If you're saying yes 99.99999% of the time, then that's probably just a rule that the agent should follow」；不明确或可能危险的交给人，明确危险的直接拒；有显式规则时走确定性的 fast path，没有时由 classifier 判断
- slide（照片 24）「Control: Auto-mode classifier — allow, soft or hard deny」：

  | 环节 | slide 内容 |
  | --- | --- |
  | INPUTS / TRIGGER | Tool call proposed（Main model emits tool_use; permission mode = auto） |
  | Fast path (deterministic) | explicit rules exist；Settings `~/.claude/settings.json` → Allow（Action runs）/ Deny（Action blocked） |
  | Slow path (nondeterministic) | no explicit rules；Classifier：Stage 1 · fast triage（errs toward blocking, quick allow / block）→ Stage 2 · deep review（escalates uncertain cases, "thinking" adjudication）；Classifier rules `claude auto-mode config` → Allow / Soft deny（Blocked; agent may clarify）/ Hard deny（Action blocked） |
  | EXECUTE & CHECK | Tool runs if allowed → Prompt injection probe（checks result for injection）→ Result added to transcript |

- 资料：Anthropic 工程博客 [How we built Claude Code auto mode](https://www.anthropic.com/engineering/claude-code-auto-mode)（John Hughes，2026-03-25）：transcript classifier 两阶段，第一阶段「a fast single-token (yes/no) filter instructed to err toward blocking」，被标记的才进第二阶段 chain-of-thought 复核；服务端 probe 在工具输出到达 agent 前检查 prompt injection；被拒后 Claude 要找更安全的路，连续 3 次或累计 20 次拒绝就停下交给人；`claude auto-mode defaults` 显示默认规则
- 我的理解：照片 24 画的就是 Claude Code 的 auto mode（文件路径和命令都是 Claude Code 的）；讲者口头没提 Claude Code auto mode 的名字，讲者文章称为「auto mode classifiers」，两阶段描述一致
- 出入：slide 上的命令写作 `claude auto-mode config`，Anthropic 博客提到的是 `claude auto-mode defaults`【待确认】是否两个子命令都存在

## 8. 收尾：为什么车有刹车

- 讲者说（视频 18:06）：五件事合起来：registry 让每个 agent 从创建起就可见；给每个 agent 打分，让人工审核用在对的地方；agent 跑在 guardrails 上，只碰需要的东西；共享 evals 和指标，大家达到同一标准不必重造轮子；在 runtime 治理，确保分类在那里发生。「each of these controls works together」
- 讲者说（视频 18:33）：「Why do cars have brakes? It's not just so that you can stop, it's so that you can go fast and stop safely. So controls for our agents works the same way.」

## Q&A 精确版

没有 Q&A。视频在 18:47 掌声处结束；直播里讲者 43:00 说「Thank you very much」，掌声后直接进入下一场（Capital One 的 Max Kanat-Alexander）的介绍。

## 勘误表（YouTube 字幕 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| City / City Bank | Citi / Citibank | slide |
| Flex Cube（直播） | Flexcube | 判决书 |
| CLLOs's（直播） | CLOs | 上下文 |
| acred interest（直播） | accrued interest | 视频字幕 |
| MPC servers | MCP servers | 上下文 |
| Jira linear tickets | Jira / Linear tickets | 上下文 |
| open claw | OpenClaw | 产品名 |
| John Finnell（直播） | John Feminella | slide、日程 |
| we want to take the same approach（直播）/ we don't want to take the same approach（视频） | we don't want to take the same approach | 上下文（后文说两种情况分别处理） |

## 出入

1. **贷款交易价**：讲者说贷款人以为只值「60 cents on the dollar」；第二巡回法院说当时以 20–30 美分交易（二手转述）
2. **金额**：讲者口头「about 8 million」「about $900 million」「300-plus lenders」；slide 和判决书是 $7.8M、$893.9M（$893,944,008.52）、315 家贷款人
3. **谁 override 了警告**：讲者说三个审批人都认为没问题、操作员 override 了警告；判决书细节是 maker 录入、checker 复核、approver 邮件批准，最后 checker（Raj）在没显示金额的警告上点了 YES。两者不矛盾，讲者是简化
4. **诉讼结果**：讲者只讲到贷款人拒还；一审（2021-02）判贷款人可以留下约 5 亿，二审（2022-09-08）推翻
5. **Shedd 原话**：讲者说「not what ships are for」，Quote Investigator 考证的原文是「not what ships are built for」
6. **审批疲劳例子**：讲者说「hundreds of emails」，报道是 200 多封
7. **auto mode 命令**：slide `claude auto-mode config`，Anthropic 博客 `claude auto-mode defaults`
8. **讲者文章 vs 演讲**：文章不含 Citibank 故事；文章明说 registry 会启发式发现定制 agent，演讲只讲了自动注册和手动注册

## 【待确认】清单

1. 第二巡回法院判决原文（20–30 美分的原话、案号 No. 21-487）
2. 「Problem 4」页的标题和内容（没拍到）
3. 照片 08 标题被截断的后半句
4. `claude auto-mode config` 是否是真实子命令
5. Two Sigma 内部这些控制的具体实现（讲者说是个人观点，没有给内部数字）
