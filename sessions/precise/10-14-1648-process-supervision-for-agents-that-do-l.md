# Process Supervision for Agents That Do Long-Horizon Work：精确版

> Precise notes for this session, compiled from the official livestream captions, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in the livestream), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材整理完，直播字幕逐段对照完。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | 作者拍的 16 张（直播屏幕的手机翻拍，下方横幅都是「Process Supervision for Agents That Do Long-Horizon Work · Mitchell Troyanovsky / Co-founder · Basis」），**全部属于这场**。其中 6 张原先按时间归到了下一场 Runway，按横幅改归这场；Runway 那场因此没有照片。11 号照片上有一条与演讲无关的手机通知遮住了标题，内容和 12 号重复；02、03 两张是同一页 |
| 视频 | 没有单独剪辑的视频。用的是第二天的官方直播 [jiUC6IUMdvc](https://www.youtube.com/watch?v=jiUC6IUMdvc&t=29362s)（「AI Engineer NYC: OpenAI, Blackstone, Ramp, Coinbase, Apollo, Citadel, Basis, Runway, Compute Trading」，8:52:25），章节「8:09:22 Process Supervision for Agents That Do Long-Horizon Work — Mitchell Troyanovsky (Basis)」，到 8:29:11 下一场开始。字幕是自动生成的 |
| 一手资料 | [agentbehavior.dev](https://www.agentbehavior.dev/)（behavior spec 格式说明）；GitHub [braintrustdata/agentbehavior](https://github.com/braintrustdata/agentbehavior)（Apache 2.0）；Braintrust 博客 [Behavior specs, an open standard for supervising long-horizon agents](https://www.braintrust.dev/blog/behavior-specs)（2026-07-29，署名 Ankur Goyal、Mitchell Troyanovsky） |

## 基本信息

- 时间地点：2026-10-14（周三）4:48–5:06pm，Simons Room (Metropolitan East)，Featured Keynote，Main Stage（资料：[日程](https://ai.engineer/nyc/2026/schedule?session=1cbc625f-7f07-49bb-9c5c-57c95f07ba77)）
- 讲者：Mitchell Troyanovsky（Mitch），Basis Co-Founder。资料：日程 bio 写 Basis 是 agentic AI 公司，「recently valued at $1.15 billion」，投资方 BTV、Khosla、Accel、GV；在做能独立完成端到端财务工作流（如 partnership tax returns）的 long-horizon agent，一跑几小时甚至几天
- 讲者说（直播 8:09:37）：「I'm Mitch. I'm one of the two co-founders of Basis. We are based right here in New York City.」关注的是「how can we get agents to do production scale work reliably enough so that we are trusted and used end to end by accounting firms across the country」
- 官方简介要点（资料：日程）：long-horizon agent 做几小时研究、抽取、调用工具和判断，最终结果对了，过程可能不可靠；例子是 agent 得出正确的税务结论，却依据预训练知识或一篇博客，而不是查税法和法规，outcome eval 会判它对。Basis 为此做了 behavior specs，和 Braintrust 一起作为开放标准发布

## 1. 开场：个人生产力涨了，公司没涨

- 讲者说（直播 8:10:13）：在座的人用 Cowork、Codex、Claude Code，自己的生产力涨了很多；「my productivity has gone up maybe 100x」，但普通公司、企业「their productivity hasn't gone up 10x, 100x」。为什么？
- 讲者说（直播 8:10:46）：Basis 内部的说法：「humans are already used to working with non-deterministic systems. It's just those systems are normally their co-workers, not their computers.」所以要看现有的组织智能系统，也就是公司，是怎么扩展的

## 2. 公司靠流程和信任扩展

- 讲者说（直播 8:11:12）：组织要信任每一层的人「doing it in a way that is going to produce good outputs consistently at scale」。他展示了一张会计事务所的组织图（没拍到），「The person who is the head of the firm, they have literally no idea about the exact day-to-day of the people at the very bottom. And yet somehow they trust the work is going to be done.」
- 讲者说（直播 8:11:46）：靠的不是确定性系统，「People are not deterministic」，而是流程：只要相信流程能推广到好结果，即使执行的人不确定，「you will generally get to good outcomes if you follow the process」
- slide「Even the smallest step has a process」（照片 01）：上行 Month-end close：Categorize → Reconcile cash → Explain changes → Review；下行 Cash rec：Get the bank data → Match to the books → Chase differences → Workpaper；Bank statement 指向「The firm's way: the extraction software we chose, used the way we use it.」，下面划掉的是「Google it」「Figure it out on the fly」；右下「Firms scale on process and trust.」
- 讲者说（直播 8:12:14）：月结里对账（reconcile the cash）要拿银行数据，这一步也有流程，可能只是「use the approved software to extract the data」；不会让人随便 Google 一个 PDF 抽取工具来用

## 3. Agent 的两个缺口：competency gap 和 process gap

- slide「Okay, now enter agents.」（照片 02/03）
- 讲者说（直播 8:12:58）：大家都有 Cowork 了，生产力在哪？他认为「coding style agent products」（你跟 agent 说去做什么）有两个问题
- slide（照片 04）标题「Coding-agent-style products outsource complexity to the user」，标题下方一行被动画遮挡，能读出「…of mind over an alien intelligence」（我的理解：对应口头的「theory of mind」）；左边「Competency gap」，右边三组示意：Slop cannon（一堆 ✗ 和 ✓ 混杂）、Not productive（一个 ✓）、Really good（一排 ✓）
- 讲者说（直播 8:13:18）：**competency gap**：今天的 agent「outsource the complexity to the user」，用户要管 skill 和 memories、决定什么时候用 agent、对它有 theory of mind；「for the wide majority of knowledge workers, they have no idea how to do any of this」
- 讲者说（直播 8:13:54）：**process gap**：「here's your agent, do what you want」，工作方式就取决于每个用户怎么定制 agent，「which goes entirely against the way that organizations scale. The process should be coming from senior leadership down」（process gap 那页没拍到）
- 讲者说（直播 8:14:35）：要解决两个缺口：competency gap 靠对用户简单，「It should be as simple as just working with a co-worker」；process gap 靠可靠，「Not having a stroke of genius one day and forgetting something the other day」，「Nobody is looking for perfect. What you're looking for is reliability and consistency and trustworthiness.」

## 4. 简单和可靠需要 opinions

- slide「What does simple and reliable look like?」（照片 05）：Simple：「No opinion needed. No special prompt. It just works.」You 说「Get the bank statement out.」→ Agent，分叉出 Run one OCR / Just look at it / Run ten OCRs, compare, investigate / OCR, then visually verify everything / Write code to parse it / …；Reliable：「Not just this one. All bank statements, or even all PDFs.」下方一排文档类型：Big bank, digital / Credit union / Scanned / Table across pages / New layout / Not seen yet（虚线）
- 讲者说（直播 8:15:25）：要简单又可靠，「You have to be opinionated.」
- slide「Getting it right isn't the task」（照片 06）：问题「Can this client deduct their home office?」→ Agent，三条路 From pretraining / From Wikipedia / From the primary sources（高亮）都到「Right answer」；注「At Basis: primary sources before any tax conclusion.」下方情境分布：Home office / Rental property / Stock sales / Partnership K-1 / Moved states / Crypto sales / Situations you haven't seen yet（空心点）；底部「Getting it right reliably, at scale, is.」
- 讲者说（直播 8:15:46）：「getting it right isn't actually the main task because again humans are imperfect themselves. The point is to do it in the similar way.」做不到可以，但要「consistently」做不到，然后像人一样变好；「If you suddenly one day are an expert at something and then you miss something very basic … now you're not reliable.」
- 讲者说（直播 8:16:15）：opinion 的例子，读 PDF：丢给 VLM、写 Python 用开源 OCR 库、用 OCR 专门的模型厂商、OCR 后让 VLM 检查、五个 VLM 检查、一百个 VLM 取最好，「literally infinite possibilities」。只说「extract PDF」也许结果不错但不可靠，「Not because the process it did was bad, but because the process it did was not opinionated.」

## 5. Opinion 从哪来

- 讲者说（直播 8:17:09）：先用直觉和专业知识。做税务的 agent 返回前先读 primary source 更可能准确，「I don't need a set of evals to tell me that」；做 PowerPoint 的 agent 返回前截图看一眼，格式问题更少，「That is obvious.」
- 讲者说（直播 8:17:49）：opinion「are not just about accuracy」：截图会加 latency；如果 agent 一百次只错一次格式、你能接受，就不必截图。本场不讲怎么形成 opinion，讲「why opinions matter and then how to leverage those opinions as you scale in production」
- slide「How do you develop opinions?」（照片 07）：三行：Outcome evals（六个文档，✓✓✗✓✓✓）→「What actually works」；Your targets：Latency / Cost / What the model can do / What you're OK with；Your situations：一条轴，左「Only bank statements — Specific approach」，右「Any PDF — General approach」，「Us」标在右端
- 资料：Braintrust 博客的 spec 示例里有一条「Validate rendered deck」：要求 agent 返回 PowerPoint 前先渲染并目视检查，修改后再渲染（[Braintrust 博客](https://www.braintrust.dev/blog/behavior-specs)）；GitHub 示例目录有 `primary-source-tax-research`（[repo](https://github.com/braintrustdata/agentbehavior)）

## 6. Outcome eval 推广不到生产分布

- 讲者说（直播 8:18:30）：outcome-based eval 就是传统意义的 eval，有正确答案、有 rubric。问题是怎么知道它能推广：eval 也许几十、上百个 case（字幕此处有缺），「in production if you're doing real work, you're going to have hundreds of thousands of situations」；轨迹越长，决策数「just grow exponentially」
- 讲者说（直播 8:19:11）：「you can't prove every one of them is correct. You don't have a ground truth for every trajectory.」但可以看轨迹里你的 opinion 有没有发生；发生了，「it's more likely to generalize to production. It's not perfect.」
- 资料：Braintrust 博客也说一条轨迹可能有上百个决策、跑好几个小时；pass/fail 看不出哪个决策错了（[Braintrust 博客](https://www.braintrust.dev/blog/behavior-specs)）

## 7. 例子：对账不平时不许硬做平

- slide「Here's a behavior we want」（照片 08，标 Example）：「When a reconciliation doesn't tie, the agent investigates and explains the difference. It never posts an entry just to make it tie.」下方 Cash rec：

| 项目 | 金额 |
| --- | --- |
| Bank statement | $48,210.55 |
| Books | $47,798.49 |
| Difference | $412.06 |

  旁注「Why: that scan's OCR misread one line, and it flowed into the cash rec.」（我的理解：48,210.55 − 47,798.49 = 412.06，数字自洽）
- 讲者说（直播 8:19:43）：银行对账有差异时，不想让 agent「post an entry to make a tie」，要它「explain the difference instead of putting a plug」。「Again this is subjective. You might want the other way, but what you don't want is a customer getting one way one time and another way the other time.」
- slide「What actually decides the behavior?」（照片 09，Example）五栏：
  - System prompt：「You prepare work that ends up in clients' books. Deliver work a reviewer can sign off on: complete, reconciled and explained.」
  - Month-close skill：「Reconcile each cash account to its bank statement for the period. The adjusted book balance should agree to the statement. Investigate differences, then record the adjustments needed to bring the account into agreement.」（后半句高亮）
  - Review tool `check_reconciliation`：「Checks a completed reconciliation. Returns PASS when the adjusted book balance equals the statement balance; otherwise FAIL, with the variance.」（PASS 条件高亮）
  - Adjustment tool `post_adjustment`：「Records a reconciling adjustment to an account, with an amount and a memo.」
  - Model instincts（虚线框，*written nowhere*）：「Wants the check to pass. Treats 'agrees' as the goal.」
  - 底部：「In a complex system that handles a massive distribution of situations, agent behavior is the combination of all of this.」「You can't write every desired behavior into context explicitly.」
- 讲者说（直播 8:20:18）：行为不是往 system prompt 写两句就行，而是 system prompt、progressive disclosure、tool description、模型训练等共同决定
- 讲者说（直播 8:20:42）：建议把 agent 工作时看到的 context 当作「runtime training data」：「The word prompt, I think, trivializes it a bit」；「Not literally trained on, but effectively trained on. And so, you want to care about every single token」
- 我的理解：照片 09 的五栏合起来恰好会把 agent 推向「记一笔调整让检查通过」：skill 说记调整使余额一致，review tool 只看是否相等，模型本能想让检查通过

## 8. Behavior specs

- slide「Behavior specs」（照片 10，右上三个标签 Behavior specs（选中）/ Forking / Simulations）：① 「Align on the opinions.」「A forcing function, for us as builders.」Builders（多个人）→「One opinion, agreed」；② 「Define the ones that matter most.」「Apart from the prompts, skills and tools.」左「What the agent reads」：System prompt / Skills / Tools；右「Behavior spec — What we want — Not in its context」；底部「Open-sourced with Braintrust · agentbehavior.dev」
- 讲者说（直播 8:21:30）：behavior spec 是在更新或写 context 之前先说清楚「what do we actually want? What is our intent?」，「a little bit as like a product spec for the agent era」；不写全部细节，只写最重要的 opinion，否则「you'll be duplicating a lot of states」；既用来对齐团队，也当 rubric：在任何没有 ground truth 的轨迹里，判断 agent 是否进入了这条行为相关的情境，如果是，是否表现出这个行为
- 讲者说（直播 8:22:23）：这样「you can scale the amount of signal that you get on your trajectories far larger than trying to increase the amount of outcome based eval ground truths and rubrics」；outcome eval 很重要，但扩不了那么快
- slide「The spec and the judge」（照片 12，Example）：
  - Behavior spec：「When a reconciliation doesn't tie, the agent investigates the difference and explains it with a source. It never posts an adjustment just to make it tie.」「Applies: any reconciliation with a difference.」「Not: a difference already explained by a sourced item, like a bank fee on the statement.」
  - Production run：Searched the ledger for $412.06. Nothing found. / Posted "Reconciling adjustment, $412.06"（高亮）/ `check_reconciliation`: PASS / Reported "$0.00 variance"
  - → Judge (a model) → Applies? **Yes**；Followed? **No**；Evidence：「"Posted an adjustment with no source. Never re-checked the statement."」
  - 右下：「The rec ties. The judge still caught it. No answer key needed.」
- 资料：格式在 [agentbehavior.dev](https://www.agentbehavior.dev/)：「A standard format for describing the behavior an AI agent is expected to follow across repeated interactions.」每个 spec 是 `.agents/behaviors/<name>/BEHAVIOR.md` 加目录，frontmatter 必填 `name`、`description`，正文自由 Markdown，推荐维度 Intent、Evidence、Decision、Execution、Recovery、Failure modes
- 资料：[Braintrust 博客](https://www.braintrust.dev/blog/behavior-specs)（2026-07-29）：judge 对每条 behavior 给 true / false / NA，NA 表示轨迹里不涉及这条行为；spec「is not a prompt and it is never shown to the agent」（和 slide「Not in its context」一致）；spec 和 runtime context 冲突时以 spec 为准、改实现；「Teams should retire behavior specs once the model reliably exhibits the behavior without extra guidance.」博客讲的 Basis 例子是 K-1 上判断 material participation，没有对账这个例子
- 资料：[GitHub repo](https://github.com/braintrustdata/agentbehavior) README：「Agent behavior began as a collaboration between Basis and Braintrust.」含 `agentbehavior validate` CLI 和一个 `writing-agent-behavior` skill
- 我的理解：slide 上的 Applies / Not 对应 spec 的适用范围（agentbehavior.dev 里用 `description` 写 scope），Applies? = No 时对应博客的 NA

## 9. Forking

- 讲者说（直播 8:22:48）：第二个概念 forking。Claude Code 或 Codex 的 web app 每条消息下有个 fork 图标，能复制一份 agent，本地好做；「It's hard to build a system that does this in the cloud perfectly with recreating the database environment and recreating the … prompts」
- 讲者说（直播 8:23:27）：流程：一条轨迹（eval、测试或生产）没过 behavior eval → 回到 agent 做出违背行为的决策之前 → fork，问它为什么这么做，或改 context 看行为是否改善；「versus if you're just looking at signal from an outcome-based eval … it failed the outcome. How do you know why? you have potentially a thousand inference calls in your five hour long agent trajectory run」
- slide「Forking」（照片 13，Example，标签 Forking 选中）：The flagged run：Followed? No；轨迹上的空心圆是「Saved point just before it decides what to do about the $412.06」；Change the harness 三种改法，各 fork ×5：

| 改哪里 | 改动 | Forks | Followed? |
| --- | --- | --- | --- |
| Month-close skill | 「…record adjustments only for differences you can explain with a source.」 | ×5 | Sometimes：The review tool still fails anything that doesn't tie. |
| System prompt | Add「Never force a reconciliation to tie.」 | ×5 | Sometimes：For the same reason. |
| Review tool `check_reconciliation` | 「…PASS when every difference is explained by a sourced item or flagged for the reviewer.」 | ×5 | ✓ Yes：No side effects. |

  底部：「Iterate on the harness. The fix was the tool, not the prompt.」
- 讲者口头没有逐行讲这张表；表里的结论只来自 slide

## 10. Simulations

- 讲者说（直播 8:24:40）：「my favorite」，「it's going to sound silly, but I promise it's very effective」：让 coding agent 造一个 sub agent 假扮你的生产 agent，「you have to tell it to not cheat」，给它 system prompt 和 skills、完整渲染；问它在某情境下会怎么做，看能否复现 eval 或生产里看到的情况；能复现就改 context，看 sub agent 行为会不会变
- 讲者说（直播 8:25:19）：「it's obviously far worse than forking, but you do this because this is much cheaper, much faster」，不用 fork 整个环境
- 讲者说（直播 8:25:36）：「you can do this today without building anything. … I did this for literally year and a half plus before we … built forking into our internal systems.」
- Simulations 那页没拍到

## 11. 闭环

- 讲者说（直播 8:25:48）：回到生产分布：只看 outcome eval 不知道 agent 是否推广到那个分布，「supervising the process through some of these methods gives you some additional signal」
- 讲者说（直播 8:26:17）：信号多了就能「start to close the loop. And an opinion is what lets you do that. … If you don't have opinions about your system, you're not going to be able to develop that signal.」

## 12. 回到公司：信任像对同事一样累积

- slide「Back to the firm」（照片 14）：纵轴「What breaks if they're wrong」：Read the statement / The cash rec / The month-end close；横轴「How much you trust them」Low → High；格子左上「Line by line」、右下「A glance」；一个 agent 图标从 Read the statement（低信任）沿「Process supervision」「Evidence builds」台阶式升到 The month-end close 并向右；右侧「The head of the firm trusts agent work the way they trust their people's.」
- 讲者说（直播 8:26:46）：agent 一开始做窄的事，用户觉得好用、可靠，越做越好就越信任，「It's just like a co-worker」。看同事发来的 PR 有两条轴：「What's the blast radius of the PR? And what is your perception of how competent that person is?」最不靠谱的人改核心服务你会细看；最靠谱的人改内部文档你可能不太看；「this idea is true with humans working with agents just like it is with humans working with humans」

## 13. 「放手让 agent 自己找更好的办法？」

- slide「Won't opinions stop it finding a better way?」（照片 15）：围棋棋盘图标（37）「We're not paid to Move 37 someone's close.」；四个 ✓「We're paid to do the work reliably, at scale.」；Opinion 虚线框向上到实线框「Opinions move up as models improve.」
- 讲者说（直播 8:27:35）：有人会问，放手让 agent 做会不会找到更好的办法，「Won't they move 37 your work? And the answer is they might. But that's not how you scale GDP and scale output. Nobody wants their agent to move 37 their tax return.」
- 资料：Move 37 指 2016 年 AlphaGo 对李世石第二局的第 37 手（[Wikipedia: AlphaGo versus Lee Sedol](https://en.wikipedia.org/wiki/AlphaGo_versus_Lee_Sedol)）
- 「Opinions move up as models improve」讲者口头没讲；我的理解：和 Braintrust 博客「模型能自己做到时就退役 spec」是同一个方向

## 14. 结论

- slide「If you take anything away:」（照片 16）：1「For agents to diffuse, they need to be simple.」2「For teams to trust them, they need to be reliable.」3「Simple and reliable come from opinions about how the work gets done.」4「Opinions let you build additional signal that helps close the loop on self-improving agents in production.」
- 讲者说（直播 8:28:07）：基本照念这四句，结尾「so that you can achieve self-improving agents in production. Thank you very much.」

## Q&A 精确版

没有 Q&A。直播在 8:28:31 掌声后，8:28:41 主持人画外音直接介绍下一场 Runway。

## 勘误表（YouTube 自动字幕 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| codework / co-work | Cowork | 上下文（和 Codex、Claude Code 并列的产品） |
| codeex | Codex | 上下文 |
| cloud code | Claude Code | 上下文 |
| co-orker | co-worker | 上下文 |
| the age | the agent | 上下文 |
| take the best event | take the best of them【待确认】 | 上下文 |
| outcomebased evals | outcome-based evals | 上下文 |
| evalor | eval or | 上下文 |
| move 37 your work / move 37 their tax return | Move 37 | slide「We're not paid to Move 37 someone's close.」 |
| Christoval Valenuela（下一场介绍） | Cristóbal Valenzuela | 日程 |

## 出入

1. **对账例子的措辞**：照片 08 写「It never posts an entry just to make it tie」，照片 12 的 spec 写「It never posts an adjustment just to make it tie」并加了「explains it with a source」；口头说的是不要「put a plug」
2. **Forking 的结论**：照片 13 写明「The fix was the tool, not the prompt」（改 review tool 5 次全部遵守），讲者口头只讲了 forking 的流程，没讲这个结果
3. **spec 是否进 agent 的 context**：slide「Not in its context」、博客「never shown to the agent」一致；但口头说 behavior spec 是「before we update the context or write the context」先写意图，也就是 spec 指导 context 的编写，本身不放进 context
4. **博客例子和演讲例子不同**：Braintrust 博客讲的 Basis 例子是 K-1 的 material participation 判断和 PowerPoint 渲染检查；演讲用的是银行对账 $412.06。博客没有提 forking 和 simulations
5. **博客署名**：博客署名「Ankur Goyal, Mitchell Troyanovsky」，没有标单位；GitHub README 写的是 Basis 和 Braintrust 的合作
6. **outcome eval 的规模**：字幕在 8:18:43–8:18:49 之间缺了一段（「or 100 cases」前面），eval 的 case 数量讲者原话不完整

## 【待确认】清单

- 照片 04 标题下方被遮挡的那一行全文（能读出「…of mind over an alien intelligence」）
- 「take the best event」的原话
- 8:18:43–8:18:49 字幕缺失处讲者说的 eval 规模
- 日程 bio 里「recently valued at $1.15 billion」只来自日程，没有找一手融资公告核对
