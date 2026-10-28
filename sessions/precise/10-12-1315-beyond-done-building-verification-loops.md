# Beyond "Done": Building Verification Loops for AI Agents：精确版

> Precise notes for this session, compiled from the official video captions and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材整理完，视频字幕逐段过完。没有 slide 照片，slide 内容只能从讲者的口述推断。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | 没有照片。讲者说所有 slide 都放在 workbench 里（见下），workbench 需要登录，打不开 |
| 视频 | [q4WC4TowkBo](https://www.youtube.com/watch?v=q4WC4TowkBo)，改名「Teach Your Agent to Prove It's Done — Nick Nisi & Zack Proser, WorkOS」，1:01:04，上传 2026-10-18。YouTube 自动字幕，只用「>>」标换人，下文的说话人是按上下文（自称、互相称呼、口头禅）判断的。章节：00:00 Why "done" isn't done / 03:17 What it takes to trust agent output / 09:34 Give agents goals they can check / 17:31 Closing the feedback loop / 28:41 Making everything feedback-loopable / 36:29 The moment the agent knows it's not done / 39:33 LLM judges and eval baselines / 53:32 Q&A |
| 官方 talk 页 | [ai.engineer/talks/q4WC4TowkBo](https://ai.engineer/talks/q4WC4TowkBo)：链接「workshop workbench」[talks.workos.cloud/beyonddone](https://talks.workos.cloud/beyonddone)（跳转到 WorkOS 登录页，没看到内容）；Further reading：Nick Nisi [Case Statement: Building a Harness](https://nicknisi.com/posts/case-statement/)、WorkOS [Agent Experience: Build without leaving your terminal](https://workos.com/blog/agent-experience) |
| 其他一手资料 | TypeSafe AI 的 Jev 发布文 [Introducing System One models and Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev)（地址取自二手报道，正文没打开）；PocketOS 删库事件的报道（[Decrypt](https://decrypt.co/365897/ai-agent-deletes-startup-database-9-seconds-founder-says)、[OECD AI incident](https://oecd.ai/en/incidents/2026-04-27-6153)）；Bun 的 Rust 重写（[Simon Willison, 2026-07-08](https://simonwillison.net/2026/Jul/8/rewriting-bun-in-rust/)） |

## 基本信息

- 时间地点：2026-10-12（周一）1:15–2:15pm，Sutton Room (NY Ballroom W)，Workshop (1-2 hrs)（资料：日程，[session 链接](https://ai.engineer/nyc/2026/schedule?session=edb4f982-0cb8-48ee-8bf7-410255a40cb3)）
- 原题「Beyond "Done": Building Verification Loops for AI Agents」，视频改名「Teach Your Agent to Prove It's Done」
- 讲者：Nick Nisi（日程：Engineering Manager, WorkOS）；Zack Proser（日程：AI Engineer, WorkOS）
- 讲者说（视频 01:21）：Nisi「I'm an engineering manager at WorkOS」，住在 Nebraska 的 Omaha；「I haven't written code since July 30th, 2025. I know because that's the day I got back from Laracon and I saw PHP and I was like, "Never again."」
- 讲者说（视频 01:45）：Proser「I'm an engineer on the Applied AI team at WorkOS」
- 官方简介（日程）：60 分钟动手 workshop，给 agent 搭 verification and repair loop：用 Playwright 检查浏览器行为，用 Jev 判断收集到的证据是否支持某条 acceptance criteria；代码把两种信号合起来决定接受、有限次修复或升级给人；然后用故意弄坏的输出和误导性证据去「骗」verifier，讨论「catching failures and interrupting people unnecessarily」之间的取舍
- 我的理解：实际 workshop 讲了 Playwright 式的 proof、Jev 做 judge、saboteur/detective 练习，但「accept / bounded repair / escalate」三分支和「打扰人」的取舍在视频里没有专门展开

## 1. 为什么「done」不是 done

- 讲者说（视频 00:30，Nisi）：开场提到一条推文里的事故，「an agent just started deleting production databases. And in 9 seconds, everything was gone」。目标是在不逐行看代码、甚至不看每个 PR 的前提下，能逐步信任 agent，「build that trust over time」
- 资料：这件事是 2026 年 4 月底 PocketOS 创始人 Jer Crane 公开的：Cursor 里跑 Claude Opus 4.6 的 coding agent 用一个权限过宽的 Railway token，一次调用删掉了生产数据库和同卷备份，「It took 9 seconds」（[Decrypt](https://decrypt.co/365897/ai-agent-deletes-startup-database-9-seconds-founder-says)、[OECD AI incident](https://oecd.ai/en/incidents/2026-04-27-6153)）。讲者没有点名，【待确认】讲者指的就是这件事（我的理解：「9 seconds」和「production database」都对得上）
- 讲者说（视频 01:12，Proser）：不只是防错，「foundational to achieving true scale with agents」
- 讲者说（视频 01:51，Nisi）：仓库就是 workbench，所有 slide 都在里面；网址先念成 workos.cloud/…，更正为「talks.workos.cloud/beyond-done」（talk 页上的链接是 `/beyonddone`）。workbench 里有每个练习的分步指南、术语 glossary、「ask anything」按钮（一个基于这套 slide 的 RAG，字幕作 reg），也可以直接问讲者
- 讲者说（视频 03:17，Nisi）：信任 agent 输出的第一层是看代码、像普通 reviewer 一样审 PR，但 agent 写得比人快，「a signal-to-noise ratio that starts coming into play and we just have too much code to review」；人不再写代码，对代码也越来越陌生。可以用 Playwright 这类浏览器工具验证（提交表单、看到成功提示），agent 可以自报结果，「you can trust that often times, but not all the time」

## 2. Demo：Midnight Snack Club 的 waitlist

- 讲者说（视频 04:56，Nisi）：在仓库里跑 `pnpm snack up`（Nisi 把 pnpm 简写成 `p`），打开浏览器里的「Midnight Snack Club」app，一个 waitlist。输入邮箱（字幕「zach@spamhim.com」，玩笑），页面显示 success，邮箱出现在页面上；只检查页面的测试会通过
- 讲者说（视频 05:49）：重新加载页面，名单是空的：UI 更新了，但没有持久化；「we didn't check the other half」
- 讲者说（视频 06:30，Proser）：现场举手：大半人在大量用 agent，这些人也都开着多个终端/会话/agent。「If this is the 18th, you know, lane of work that you're driving today」，注意力分散时，agent 会说「the code looks good, it builds, it runs, the test pass, and it's still wrong」。把 agent 比作「herds of insane token machines」
- 讲者说（视频 07:31，Nisi）：引用 X 上「a famous potato」Lauren Tan（字幕作 Lauren 10）的话，改口后是「if an agent can't verify its own work, nothing else matters」
- 资料：Lauren Tan 在 X 和 GitHub 上的 handle 是 poteto（[x.com/poteto](https://x.com/poteto)），「potato」是这个梗。二手报道说她 2026 年 4 月加入 Cursor、现在做 Grok Bot，讲过用一套可复现的验证 CLI 让 agent 证明自己修好了 bug（[redreamality 的转述](https://redreamality.com/blog/lauren-tan-poteto-trust-before-parallel/)）。这句话的原文出处没找到，【待确认】
- 讲者说（视频 07:47，Proser）：随着模型能力快速提升，verification、reliability、evals 是最基础的事；越来越多 harness 内置 RSI（recursive self-improvement），「we may all be maintaining multiple self-evolving code bases eventually」。问题是怎么打好 eval 基础，让 agent 知道「what good looks like」并自己 hill climb，以及怎么给 agent 失败的反馈
- 讲者说（视频 08:30，Nisi）：信任不是 0 到 1，「it's a gradient」
- 讲者说（视频 08:49，Proser）：WorkOS 有一个 applied AI 团队，给其他团队和自己做很多东西；在有 foundational eval、「a baseline that can be run and trusted」的代码库上迭代，体验「night and day」

## 3. 给 agent 可检查的目标

- 讲者说（视频 09:30，Nisi）：只说「make no mistakes」没用，「You need to give it clear, defined goals to get to and give it a way to observe and check those goals」。每个没写清的结果细节，agent 都会推断或编造，它的假设未必是你的假设；要有一套系统让它「observe, understand what you want, observe that, repair that and then recheck that」
- 讲者说（视频 10:26，Proser）：过时的做法是一个巨大 PR、一个跑三四天的 agent 会话、反复说「fix this, make it better」；有效的做法是把任务拆干净，每个任务的 prompt 里写清楚它怎么知道自己做完了、怎么验证 done。「we have people on our team that are 900 PR's a month and and no incidents and like improving KPIs for the software」。「I'm almost gardening multiple code bases and letting them self evolve」
- 讲者说（视频 11:16，Nisi）：两人各自做了工具，「It's too early to really classify on anything, so experimentation is key」。Nisi 自己工具里的一句话：「status comes from records, not model prose」（字幕作 model pros）；要截图、视频、测试结果、smoke test，「I don't care about the code as much. I care about the result」；代码烂也可能交付结果，代码好也可能交付不了
- 资料：Nisi 的博客 [Case Statement: Building a Harness](https://nicknisi.com/posts/case-statement/)（2026-03-19）介绍他在 WorkOS 做的开源 harness「case」：派发六个 agent 走一条程序化流水线修 bug、发 feature；verifier agent 与 implementer 分开，implementer 不能开 PR；evidence marker 由脚本生成，必须有真实测试输出或 Playwright 截图，「creates tamper-proof evidence that work was actually done」。博客里没有「status comes from records」这句，【待确认】这句出自哪个工具

### Proser 的 CTX

- 讲者说（视频 12:34，Proser）：自己做的工具叫 context 或 CTX。起初是跨 harness 的 memory 层（同时在试 ChatGPT、Claude Code、Pi 等），「average day is like 18 or 30 terminal tabs」。它是一个跑在 Cloudflare 上的简单 web app，最初 vibe coded；能把 secret 交给 agent 而 agent 看不到凭据本身；现在在试 verifier：agent 做完工作后通过 MCP 连接回 CTX 说「am I done? Please verify it」
- 讲者说（视频 13:29）：例子：一个工作流要验证 PR 创建正确，CTX 在一个已知 verifier 上跑 evaluator（起步可以只是一个 CLI，之后变成可复用的固定组件），去 GitHub API 确认 PR 存在、健康、merge checks 全过，才把工作流里这个节点标成 satisfied，否则告诉 agent 还有哪些没过。演示画面里的一个 verification gate 要求「all pull requests were merged」，右侧栏给出状态并引用被检查的仓库和 PR；Proser 说自己的 app 都要配「weird pixel art」
- 讲者说（视频 14:37）：可以把标准化的 verifier 放进库、skill 或工具里在组织内共享，逐步减少人工监督
- 资料：没有找到 CTX 的公开资料，【待确认】

### Nisi 的 Riker

- 讲者说（视频 14:53，Nisi）：另一个工具叫 Riker（字幕 Ryker / Riker，拼法【待确认】），「a engineering-focused Grokbot or Muse」，交付方式灵活，接手后的工作是「babysit」PR：修测试、处理 review comment，同时确认修改仍符合「working software」的总体目标，需要人时再叫他。流程：建一个 check，判断要跑哪些工具，跑完确认通过，保留证据并以不同方式展示，再决定何时停。让人不必陷在「shipping the final 10%」的细节里
- 【待确认】「Muse」指什么产品（字幕原样）

## 4. 闭环：从人手检查到确定性的 CLI

- 讲者说（视频 16:36）：现场让听众说出 LLM 之前人怎么验证一个 web app：刷新页面看应用是否还在、开新 tab、看 API 返回码是否符合预期、local storage 或别的持久化、console 日志、network、读页面内容、和页面交互
- 讲者说（视频 17:26，Proser）：人工手凿代码的年代这是开发者的核心职责；早期 LLM 时代是「Here's three paragraphs of text. Go execute these instructions.」两者的差距就是 agent 能否验证自己在真实应用里做到了、且没弄坏别的。否则得到的就是「the confabulations that LLMs are designed to give you」
- 讲者说（视频 18:08，Nisi）：agent 在纯终端里做不到这些，需要工具。写成一段话的指令（「always verify the change in a browser」「Remember to reload and check」「Check that the data persists」「Look at the console for errors」）越写越具体，很多 skill 和保存的 prompt 就是这样，但漏一条或情况变了就留下解读空间。更好的是一个脚本，「doing more of a deterministic check across a number of things」，把输出喂回 agent，循环跑到没有错误
- 讲者说（视频 19:21，Proser）：这还是「uniform」：同一个 prompt 给 30 个 agent 结果会有差异，固定成一个背后是 bash 的命令就确定得多。「the folks uh like Lauren Tan and others that are achieving thousands of PRs ostensibly a month」说这是他们最先下功夫、真正解锁速度的地方。如果你的 web app 还没有一个「bespoke CLI that allows any agent to fully flex the functionality that you care about verifying」，只靠 prompt 会难走得多
- 资料（二手）：redreamality 转述 Lauren Tan 的演讲说她上个月合并约 2,000 个 PR、X 帖子里写 2,500，并提到她内部的验证 skill「Control Glass」带一个可复现的 CLI（[链接](https://redreamality.com/blog/lauren-tan-poteto-trust-before-parallel/)）。一手出处没核对，【待确认】

### snack：workshop 的验证 CLI

- 讲者说（视频 20:21，Nisi）：workshop 做了一个小工具 `snack`（`pnpm snack`），「not meant to be like a reusable tool」，是给自己应用搭类似工具的模板。功能：
  - 健康：`up`（起应用）、down、`doctor`（数据库在不在、网络通不通等）
  - drive：`join`（加入 waitlist）、`reload`、`reset`（清空数据）
  - 观察：`snapshot`、network、`storage`（local storage）、console 日志、`screenshot`、跑 eval
  - 证明：`prove`（确定性检查）和 `judge`（用一个「system one model」判断方向是否正确，不要求逐字匹配）
- 讲者说（视频 21:39，Proser）：在一个原本没有这类工具的仓库里加上它，速度收益属于整个团队，大家都能「significantly more PRs significantly faster and significantly safer」
- 讲者说（视频 22:07，Nisi）：验证和测试的方式可能比应用本身更重要：有了它就能把应用转写成别的语言再证明它能用。「like the Bun team did, convert it to Rust」；TypeScript「convert it to Go」；「I think Theo just last week converted it to Rust again」。「if you have all of those verifiers in place, then your source code doesn't matter as much」
- 资料：Bun 从 Zig 重写为 Rust，2026-05-14 合并，Bun 团队文章「Rewriting Bun in Rust」（[Simon Willison, 2026-07-08](https://simonwillison.net/2026/Jul/8/rewriting-bun-in-rust/)）。TypeScript 编译器移植到 Go 是微软 2025 年宣布的原生移植（我的理解：讲者说的「Go」指这个）。「Theo … converted it to Rust again」指什么没查到，【待确认】
- 讲者说（视频 22:54，Nisi）：工具给 agent「eyes」，「The LLM is the brain」，工具是它在电脑上伸手交互的身体
- 讲者说（视频 23:29）：演示 `p snack up`、`p snack snapshot`（页面的 JSON 快照，加入一个邮箱后能在快照里看到）、`p snack screenshot`。目标是「proving back to the human」：截图、录视频、贴到 PR 上（「You can do that with the the GH CLI now」）
- 讲者说（视频 24:35，Proser）：也给 agent Chrome debugging tools：「Don't guess and, you know, spin your wheels for 3 hours」，找到是哪一部分让渲染变慢
- 讲者说（视频 24:46，Nisi）：关心的是用户看到什么，「Is it correctly rendering in this browser at this width with dark mode, with light mode」，可以无限次循环跑
- 讲者说（视频 25:27）：流程示例：doctor → join `ada@example.test` → storage 有 → reload → storage 空 → snapshot 确认为空。agent 在回到人面前之前就能发现「I'm not actually persisting this」

（26:01 起第一轮提问，见 Q&A）

## 5. 让一切都 feedback-loopable

- 讲者说（视频 28:04，Nisi）：AMP 团队有一篇好文章（名字记不清，「loop-driven develop…」），例子是 canvas 里一个圆、一颗弹球，圆上有缝，球打准了会掉出去、游戏结束。核心是「making things feedback loopable」：模型看不到运行中的 canvas，但如果每一帧打印球的坐标，发现某处跳到屏幕另一侧（每帧移动超过一像素），模型就能帮你 debug
- 资料：没找到这篇 Amp 文章，标题【待确认】
- 讲者说（视频 29:17，Proser）：做到这点后，可以同时生成 30 个版本，「speaking to that eval」，挑一个；如果 token 继续变便宜，「coming in on Monday and saying like I would like to see 35 versions of this new feature」
- 讲者说（视频 29:47，Nisi）：proof 没法通用，必须针对你的应用。Midnight Snack 的 `prove` 用 Playwright：先确认 waitlist 为空，填入邮箱，点「join waitlist」按钮，期望 status 元素出现「waitlisted」，再看 inner text 里有没有这个邮箱。写过 Playwright、React Testing Library 端到端测试的人都熟。交给 agent 当工具，它能自己证明，并且成为回归测试
- 讲者说（视频 31:16，Nisi）：「a successful screenshot isn't proof」：要确认截图是真的跑完命令得到的状态，否则「it might just be like another lie」，可能有副作用，或走了 UI 里不可能走的路径。要从头跑到尾
- 讲者说（视频 31:52，Proser）：循环可靠可复现之后，「a failure starts to drive future work」：agent 失败了自己再试、再跑 verify，你不用在一天结束时回来推这个 PR
- 讲者说（视频 32:41，Nisi）：问谁写好的错误信息，几乎没人举手。错误信息比以往更重要，因为最能读懂错误的是 agent；失败要明确说为什么、怎么进入这个状态，agent 就能 self-heal
- 讲者说（视频 33:25，Proser）：这听起来像在 LLM 外面包确定性，但其实不想那样，这也合 AMP 团队的思路：agent 聪明，要让它有创造力，「constraints can be can equal freedom」。给结果而不是步骤；Nisi 在实验「outcome-driven development」。过度规定步骤「can actually lead to lower performance in some cases」；「You'll know you're here when this command returns this.」
- 讲者说（视频 34:17，Nisi）：最新一批 frontier 模型（字幕「Fable, Astra, Saul 6.1, uh Opus 5.5」）都擅长有创造力；只给终点，它们可能多绕一点、抓到更多 edge case、或问出关键问题；规定死路线就不会偏离。「something that we we've only come around to really appreciating in the latest batch of Frontier models」
- 【待确认】「Astra」「Saul 6.1」是哪两个模型（字幕原样；Saul 可能是 Sol）
- 讲者说（视频 35:09，Nisi）：做 TDD 的 red-green-refactor：「Verify this, reproduce it, show me … fix it, and then show me the proof of that」，证明怎么给和修复本身一样重要

## 6. agent 知道自己没做完的那一刻

- 讲者说（视频 35:50）：跑 `p snack doctor`：检查 Node、Chromium 已装、session 存在、应用在跑、构建成功
- 讲者说（视频 36:22）：`snack join ada@example.test`（「Sorry, we don't type anymore」），`snack storage` 显示为空
- 讲者说（视频 36:48，Proser）：「at this moment your agent realizes I am not done and I'm not going to drop this work on the floor」
- 讲者说（视频 37:25）：应用持久化修复和「honest copy」后跑 `prove`，开了浏览器但失败：Nisi 说自己还没真正应用修复
- 讲者说（视频 37:53，Proser）：receipts 的概念：比如客服工单系统里人类怎么算把一张工单答好；把 agent 每一次运行保存下来，现在可以喂给 LLM 找修复，将来 RSI 工具成熟后是现成的数据集，可以做「very comprehensive evals」让 agent hill climb

## 7. LLM judge、system one 模型和 eval baseline

- 讲者说（视频 39:12，Nisi）：修好持久化后，应用还可能「lie」：reload 后状态是 waitlisted，截图上的文案却是「you're in, your seat is confirmed」，两者不一致。跑 `p snack judge` 失败：「the confirmation text breaks the contract」。怎么在不把测试和页面文字绑死的情况下证明它工作？答案是 system one 模型
- 讲者说（视频 40:24，Proser）：System 1 / System 2 来自心理学：System 1 是快速、自动的反应（开车时有人突然变道你本能修正），System 2 是「I need to figure out what I'm going to do with my life. So, I'm going to go book an Airbnb and like think about it for 3 days」。LLM 这边「now we've got Jev and then immediately after we've got Cleft from Cloudflare, you know, 2 days later. That's even faster.」
- 资料：Jev 是 TypeSafe AI 2026-09-15 发布的「System One model」，输入非结构化状态，输出带置信度的类型化决策（是/否、选项、分数），不生成文本（[TypeSafe 发布文](https://typesafe.ai/blog/introducing-system-one-models-and-jev)，内容据 [agentpedia 的概述](https://agentpedia.codes/blog/jev-system-one-models)，一手原文没打开）。Cloudflare 的是 Clef / Clef-flash（字幕作 Cleft），Workers AI 团队训练，二手报道给的发布日期是 2026-10-01（[llmreference](https://www.llmreference.com/model-family/clef)），和「2 days later」对不上，【待确认】
- 讲者说（视频 41:14，Nisi）：另一个比喻：那盏灯是什么颜色，你脱口说黄色是 System 1；问它为什么是黄色就要想物理

### saboteur 与 detective 练习

- 讲者说（视频 41:40，Nisi）：游戏：saboteur 随机挑一个 snack 埋一个秘密 bug；你作为 detective，只用 snack 找到它，「Name the bug and give give proof that you found it」。命令 `pnpm snack up --bug random`
- 讲者说（视频 43:00）：现场演示：应用非 headless 跑，bug「hidden until you stop up」；跑 doctor，最后一条 receipt 是 fail，但那份 proof 对应的代码已经不存在了（是上一次运行的）。问听众 bug 是什么，有人答「Wrong email?」；Nisi 一度以为是端口错了（62174），又说端口没错；演示没有得出结论
- 讲者说（视频 45:11，Nisi）：「I need some verification tests around my verification my contrived verification examples」。可能的结局：named the bug with output；said it works but it didn't；said it works and it did；blamed a bug that wasn't there；right bug but no evidence
- 讲者说（视频 45:44，Proser）：评分标准各团队可以不同，「The most important thing to do is to start and to create one, create an eval baseline and to use it consistently and to evolve it together as a team」，让每个 PR 自动跑这个 eval，知道改动是更安全、更好、更差还是没变化
- 讲者说（视频 46:13，Nisi）：每次漏掉一个就在「map」上加一行。bug 种类：只存在内存里（memory only）、写进错误的邮箱、丢了确认、over-promised（截图或 judge 能发现它说过了其实没过）、console error（console 里可能没有，但 receipt 里有，或在到达 console 之前就被捕获）、或者 none（什么 bug 都没有，「that's a real answer as well」）
- 讲者说（视频 47:18）：doctor 显示「proof that no longer exists」，因为代码可能从昨天到今天变了很多，也可能是「a famous date bug」之类的情况；工具要做得足够灵活

### 回到人做的检查

- 讲者说（视频 48:08，Proser）：把人以前在 Chrome 里深夜 debug 时做的检查搬进一个确定性 CLI。建议：如果你有 web app 还没有「custom verifier CLI」，这是最高杠杆的起点；再加一个展示「what good looks like」的 foundational eval；给 agent 看到性能、网络、流量、渲染的能力，web 用 Chrome DevTools，iOS / macOS 原生应用也有越来越多的同类项目。「you need to give your agent the ability to see absolutely everything that you would normally see as a developer」
- 讲者说（视频 49:30，Nisi）：给它越多「arms or eyes」越能接手；还要给它何时用、怎么用的直觉：你自己收到 bug 时怎么复现。WorkOS 的场景很多是 sign-on：「is it properly setting a cookie? Is it setting an expiration」
- 讲者说（视频 50:53，Proser）：保存 receipts，比如电商应用的完整用户旅程：登录、看到目录花了 7 秒、加购、弃购。可以有一个整夜运行的系统汇总最新 receipts，「today we saw eight failures in the caching module」，晚上就发 PR 修并在 PR 里证明修好了。高产出团队还给人看的 PR 附上走查录屏，早上要过 15 个重要 PR 时先看录屏再决定看不看代码
- 讲者说（视频 52:29，Proser）：自我演化工具成为默认后，「there will be a specific advantage for folks that have preserved uh clean data sets」；竞争对手有而你没有，他们的开发周期会快得多
- 讲者说（视频 52:56，Nisi）：unit tests、evals、smoke tests 加上让模型会跑会改的方式，新功能加进来时把旧的都跑一遍，「climb that evidence ladder」，「that really becomes the spec that shows what our app does」

## Q&A 精确版

视频里的提问多数听不清，下面的问题是根据讲者的复述或回答还原的。

**中途提问（视频 26:01 起）**

1. **（视频 26:08）是否让模型写 JSON 来把代码变成确定性的？** Nisi 复述问题后答「yes」：用 unit test 包住，覆盖 edge case，确认代码真的代表了从 system one 模型期望拿回的东西；并保留这些测试，防止随时间漂移
2. **（视频 26:54）这套流程是否适用于全栈 / 后端？**（问题听不清，从回答推断）Proser：团队做很多内部工具和一些面向客户的应用，多是 TypeScript、部署在 Cloudflare 的全栈应用，「It works perfectly」，也可以有专门针对后端或数据库操作的 eval。Nisi：所以不发一个通用工具，「You kind of have to build up the tools that fit the app」。Proser：不同团队的 eval 差别会很大，比如室内设计领域和只看速度的领域
3. **（视频 44:39）练习是否需要 API key？**（问题听不清，从回答推断，【待确认】）Nisi：「for that one, yeah, you do」。Proser：工具把它设成可选，没有也能跑完所有练习；调用很便宜。Nisi：注册送 $5，「I think that'll last you like 3 years」（我的理解：指 judge 用的 Jev）

**结尾 Q&A（视频 53:38 起）**

4. **（视频 53:43）这些检查怎么保证被执行：skill 还是 CI？** 提问者说自己倾向于不用 skill，因为跑不跑由模型决定，宁愿让 CI 强制执行。Nisi：可以放进 CI；可以做成 hook，git hook「probably the last one I would reach for」，或 Claude Code hook（字幕 cloud code）；他自己可能写一个完整的 Pi extension 在后台跑；最简单让全队都用上的办法是放一个「agents.md not a cloud.md」，写上「on change verify things by running this」或「before you open a PR do this」，大家要刻意才能绕过。Proser：本地开发时 hook 也很合适，可以全队共享
5. **（视频 55:47 / 56:38）CI 和本地跑相比好处是什么？** Nisi：CI 是一部分，但真正的价值是本地的快速开发：给 agent 一个终点（「I want to be able to add an email to the waitlist and then it gets persisted」）和验证方法。好处是反馈更快、能把自己并行化：「right now I have three agents running. Uh that's a very low day for me, but I'm at a conference」，平时 10 到 15 个；不想回来时发现「No, that's not actually correct」再重来。Proser：理想是一天开的每条 lane 晚上回来都已正确完成，不用逐个推。Nisi：他依赖 CI 和自动 code review，现场问有多少人用 Greptile（字幕 Reptile）、CodeRabbit、Qodo（字幕 Codo）；团队用 Greptile，「if it gives me a five out of five and all of my CI is passing I feel extremely confident」。这里的重点是更快、更少轮次到达那个状态；而且可能加了「10,000 lines of absolutely nothing」也拿到 5/5 并通过 CI，验证目标能让 agent 不跑偏
6. **（视频 59:41）怎么放心让 agent 的改动上生产？**（问题听不清，从回答推断，【待确认】；字幕没有标说话人，从语气看是 Proser，【待确认】）可以先部署 preview 验证，再上 prod 并立即监控。新出现的模式：大部分由 agent 实现的部署，至少有一个 agent 专门负责生产 rollout，盯指标、Datadog、feature flag，能判断「this is actually bad and we're reverting it」。「I truly understand and get the like queasiness around it」，但「it seems inevitable」

结束（视频 1:00:39）：WorkOS 在 expo hall booth 6，进门右手边。

## 勘误表（YouTube 自动字幕 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| Zach Prosser / Zach | Zack Proser | 日程 |
| Lauren 10 | Lauren Tan | 上下文（「famous potato」= poteto）、讲者 19:39 处的「Lauren Tan」 |
| workos.cloud/umbeyond-done / talks.workos.cloud/beyond-done | talks.workos.cloud/beyonddone | 官方 talk 页链接 |
| a reg | a RAG | 上下文 |
| PNPM snack / P snack | `pnpm snack`（`p` 是 Nisi 的 alias） | 讲者说 |
| model pros | model prose | 上下文 |
| Cloud Code / cloud code hook | Claude Code | 上下文 |
| five coding | vibe coding | 上下文 |
| Ryker / Riker | Riker（拼法【待确认】） | 字幕两种写法都有 |
| Cleft | Clef | 二手报道 |
| Reptile | Greptile | 官方 talk 页 |
| Codo | Qodo | 我的理解 |
| DataDog | Datadog | 通用写法 |
| get hook | git hook | 上下文 |
| cloud.md | CLAUDE.md | 上下文 |

## 出入

1. **Nisi 的职位**：日程和他在台上说的是 Engineering Manager；官方 talk 页写 Developer Experience Engineer，他的博客自称 DX engineer
2. **workbench 地址**：台上念的是 talks.workos.cloud/beyond-done（带连字符），talk 页链接是 /beyonddone
3. **官方简介和实际内容**：简介写了「accept / bounded repair / escalate to a human」三分支和「catching failures vs interrupting people」的取舍；视频里没有专门讲这部分，重点在 snack CLI、Playwright proof、judge 和 saboteur/detective 练习
4. **Clef 的时间**：讲者说 Jev 之后「2 days later」就有 Cloudflare 的同类模型；二手报道里 Jev 是 9 月 15 日、Clef 是 10 月 1 日
5. **detective 练习的演示**：现场没有成功找出 bug，Nisi 说自己的输出有问题要回去 debug
6. **视频章节和实际 Q&A**：章节把 Q&A 放在 53:32，实际 26:01 就有一轮中途提问，44:39 还有一个关于练习的提问

## 【待确认】清单

1. 开场「9 seconds」删库事件是否就是 PocketOS（讲者没点名）
2. 「if an agent can't verify its own work, nothing else matters」在 Lauren Tan 原帖里的确切措辞和出处；她「thousands of PRs a month」的一手出处
3. 「status comes from records, not model prose」出自 Nisi 哪个工具
4. CTX、Riker 的公开资料和 Riker 的拼法；「Muse」指什么
5. AMP 团队「feedback loopable」那篇文章的标题和链接
6. 「Astra」「Saul 6.1」是哪两个模型
7. 「Theo just last week converted it to Rust again」指什么
8. Clef 的发布日期（二手 10-01）以及 Jev 发布文的一手内容
9. Q&A 第 2、3、6 题的原问题（音频听不清）
10. workbench（talks.workos.cloud/beyonddone）的内容和配套仓库地址（需要登录）
