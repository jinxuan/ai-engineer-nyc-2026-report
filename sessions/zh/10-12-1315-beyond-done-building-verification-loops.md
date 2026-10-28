[English](../10-12-1315-beyond-done-building-verification-loops.md) | **中文**

# agent 能证明自己做完了，才算做完

*在 AI Engineer NYC 2026 上，WorkOS 的 Nick Nisi 和 Zack Proser 认为，给 coding agent 最值得做的东西，是一个针对自家应用、能让 agent 检查自己工作的工具：失败会驱动下一次修复，而不是落到人的桌上。*

**Workshop · 2026 年 10 月 12 日（周一）· 1:15–2:15pm · Sutton Room (NY Ballroom W)**
**讲者：** Nick Nisi，Engineering Manager，WorkOS；Zack Proser，AI Engineer（Applied AI 团队），WorkOS

---

Nick Nisi 开场讲了一件很多人在社交媒体上见过的事故：一个 agent「just started deleting production databases. And in 9 seconds, everything was gone」。Nisi 没有点名，但细节和 2026 年 4 月的 PocketOS 事件对得上：Cursor 里一个跑 Claude Opus 4.6 的 agent，一次调用删掉了生产数据库和备份。Nisi 说，这个 workshop 要做的是防住这种失败，同时「still trusting your agents and not having to look at every line, even look at every PR」，并且「build that trust over time」。

Proser 补充说，verification 不只是防错：「this is also … foundational to achieving true scale with agents」。

Nisi 自我介绍是 engineering manager，从 2025 年 7 月 30 日起就没写过代码：「that's the day I got back from Laracon and I saw PHP and I was like, "Never again."」Proser 在 WorkOS 的 Applied AI 团队。workshop 围绕一个两人称为「workbench」的仓库展开，发布在 talks.workos.cloud/beyonddone（现在要先登录 WorkOS）。据 Nisi 介绍，里面有 slide、每个练习的分步页面、术语表，还有一个「ask anything」按钮，背后是一个基于这套 slide 的检索系统。

## 1. 为什么看代码已经不够

信任 agent 工作的第一种方式，是像审任何 pull request 一样审它的代码。但 Nisi 说，agent 写代码比人读代码快，「a signal-to-noise ratio … starts coming into play and we just have too much code to review」。不再写代码的开发者，对代码也越来越陌生。Playwright 这类浏览器工具可以确认表单提交了、成功提示出现了，agent 也可以自己报告这些结果。「You can trust that often times, but not all the time.」

demo 应用把这个缺口演示了出来。运行 `pnpm snack up`（Nisi 把 `pnpm` 设了别名 `p`）会打开「Midnight Snack Club」，一个 waitlist 页面。Nisi 输入一个邮箱，页面显示成功并列出地址，只检查页面的测试会通过。然后 Nisi 刷新页面，名单是空的。UI 更新了，但什么都没存下来。「We didn't check the other half.」

Proser 让大家举手。超过一半的人在大量用 agent 开发，这些人也都同时开着多个终端、聊天会话或 agent。Proser 说，「If this is the 18th … lane of work that you're driving today」，注意力很分散，细小的 bug 就会混进来：agent 说「the code looks good, it builds, it runs, the test pass, and it's still wrong」。

Nisi 接着引用了 X 上一位「famous potato」Lauren Tan 的话（handle 是 poteto）：「If an agent can't verify its own work, nothing else matters」（如果 agent 无法验证自己的工作，别的都无从谈起）。（原帖没找到，措辞按 Nisi 念的。）Proser 说，业界正在形成共识：verification、reliability 和 evals 是基础，尤其是越来越多 harness 内置 recursive self-improvement 之后，「we may all be maintaining multiple self-evolving code bases eventually」。Nisi 说信任是「a gradient」，不是从 0 到 1 的开关。Proser 说，在 WorkOS 的 Applied AI 团队，在一个有「a baseline that can be run and trusted」的代码库里工作，体验是「night and day」。

## 2. 给 agent 能检查的目标

Nisi 说，告诉 agent「make no mistakes」没用。「You need to give it clear, defined goals to get to and give it a way to observe and check those goals.」每一个没写清楚的结果，agent 都会去推断或编造，而它的假设未必和你一致。系统要让 agent 能「observe, understand what you want, observe that, repair that and then recheck that」。

Proser 把老办法称为「tired」：一个巨大的 PR，一个跑三四天的 agent 会话，反复说「fix this, make it better」。有效的是把任务拆干净，并明确告诉每个 agent 它怎么知道自己做完了。Proser 说：「We have people on our team that are 900 PRs a month and … no incidents and … improving KPIs for the software.」「I'm almost gardening multiple code bases and letting them self evolve.」

两位讲者都做了自己的工具，Nisi 说现在正该这样：「It's too early to really classify on anything, so experimentation is key.」Nisi 自己工具里的一句话概括了思路：「status comes from records, not model prose」（状态来自记录，而不是模型写的文字）。Nisi 要的是截图、视频、测试结果和 smoke test，而不是 agent 说一句看起来没问题。「At the end of the day, I don't care about the code as much. I care about the result.」talk 页链接的 Nisi 2026 年 3 月的文章「Case Statement: Building a Harness」，介绍了 WorkOS 开源 harness「case」里类似的设计：独立的 verifier agent，以及只能由脚本根据真实测试输出或 Playwright 截图生成的 evidence marker。

**CTX。** Proser 的工具 CTX（「context」）起初是一个跨 harness 共享的 memory 层（Proser 同时在试 ChatGPT、Claude Code、Pi 等，平常一天开着「18 or 30 terminal tabs」）。它是一个跑在 Cloudflare 上的简单 web app，最初是 vibe coding 写出来的，还能把 secret 交给 agent 而不让 agent 看到凭据。最新的实验是集中式 verification。agent 做完工作后通过 MCP 问 CTX：「Am I done? Please verify it.」对于一个要求开 pull request 的工作流步骤，CTX 会跑一个已知的 verifier（起步可以只是一个简单的 CLI，之后打磨成可复用的组件），通过 GitHub API 确认 PR 存在、状态健康、所有 merge check 都通过。只有这样才把这一步标为 satisfied，否则告诉 agent 还有什么没过。demo 里有一个 gate 要求「all pull requests were merged」，侧边栏显示状态，以及它检查过的仓库和 PR。Proser 设想把标准 verifier 放进库、skill 或工具里，在整个组织共享。

**Riker。** Nisi 的工具 Riker 是「an engineering-focused Grok Bot」，接下一项工作后就负责盯着它：修测试、回应 review comment，并确认修改仍然服务于「能工作的软件」这个目标。它会建一个 check，决定跑哪些工具，跑完后保留证据、用不同方式展示出来，再决定何时停。Nisi 说，目的是不用陷在「the minutia of shipping the final 10%」里。

## 3. 从人工检查到确定性的 CLI

Proser 问：LLM 出现之前，开发者怎么验证一个 web app？现场列出了：刷新页面、开新 tab、看 API 返回码、看 local storage 或其他持久化、读 console 日志和网络请求、读页面内容并和它交互。这些检查是「hand chiseling code」年代的本职工作。早期用 LLM 时，换成了「three paragraphs of text. Go execute these instructions.」Proser 说，两者的差距就在于 agent 能否在真实应用里验证自己做到了要求、且没有弄坏别的。做不到的话，「you're just getting the confabulations that LLMs are designed to give you」。

agent 光靠终端做不了这些，它需要工具，也需要知道要证明什么。Nisi 把 prompt 指令一级级往上写：从「always verify the change in a browser」（「whatever that means」）到「remember to reload and check」「check that the data persists」「look at the console for errors」。很多 skill 和保存的 prompt 就是这样写的，但漏掉一条或情况一变，就全凭解读。更好的办法是一个脚本，跑「a deterministic check across a number of things」并把输出喂回去，agent 就能一直循环到没有错误。

Proser 补充说，脚本还有一致性。同一个 prompt 给 30 个 agent，结果各不相同；一个背后是 bash 调用的命令就不会。像 Lauren Tan 这样「achieving thousands of PRs ostensibly a month」的开发者说过，这是这些人最先下功夫的地方，也是「actually unlocked the velocity」的东西。Proser 的建议：如果你的 web app 还没有「a bespoke CLI that allows any agent to fully flex the functionality that you care about verifying」，只靠 prompt 会「a much rougher time」。

workshop 里的这个 CLI 叫 `snack`。Nisi 说它不是拿来复用的，只是给你为自己的应用做一个类似工具的模板：

| 类别 | 讲者描述的命令 |
| --- | --- |
| 健康 | `up` 和 down；`doctor`（数据库在不在跑、网络通不通） |
| 驱动 | `join`（加入 waitlist）、`reload`、`reset`（清空所有数据） |
| 观察 | `snapshot`、network、`storage`（local storage）、console 日志、`screenshot`、跑 eval |
| 证明 | `prove`（确定性检查）和 `judge`（一个「system one」模型，判断结果对不对，不要求逐字匹配） |

Proser 说，加上这样一个工具，收益归整个团队，大家都能「significantly more PRs significantly faster and significantly safer」。Nisi 说得更远：验证一个应用的方式「might even be more important than … the actual application」。有了 verifier，应用可以用另一种语言重写并证明它能用，就像 Bun 团队迁到 Rust（Bun 借助 LLM 从 Zig 重写的版本在 2026 年 5 月合并），以及 TypeScript 迁到 Go。「If you have all of those verifiers in place, then your source code doesn't matter as much.」

Nisi 说，工具给了 agent 身体：「The LLM is the brain」，工具是它在电脑上伸手做事的方式。demo 里，`p snack snapshot` 把页面以 JSON 返回，Nisi 加了一个地址后它出现在快照里；`p snack screenshot` 截下页面。最终目标是「proving back to the human」：把截图和视频附到 pull request 上，GitHub CLI 现在就能做。Proser 加上了 Chrome 的调试工具：「Don't guess and … spin your wheels for 3 hours. Like actually figure out which part was causing the render to be slow.」Nisi 说，重要的是「what a user sees」：页面是否「in this browser at this width with dark mode, with light mode」正确渲染，需要多少次就在循环里检查多少次。

接着 Nisi 演示了 agent 可以自己跑的一串操作：`doctor`，用 `ada@example.test` 加入，查 storage（地址在），刷新，再查 storage（空了），用 snapshot 确认。agent 在汇报之前就会发现「I'm not actually persisting this」。

## 4. 让一切都能进入反馈循环

Nisi 提到 Amp 团队的一篇文章（Nisi 想不起标题），例子是 canvas 上一个圆里弹跳的球，圆上有一道缝，球从缝里掉出去游戏就结束。模型看不到 canvas。但如果程序每一帧都打印球的坐标，而球从屏幕一侧跳到另一侧，每帧移动超过一个像素就说明有 bug，模型就能找到它。核心是「making things feedback loopable」。Proser 说，有了这个，就能针对同一个 eval 同时生成 30 个版本挑一个；如果 token 继续变便宜，团队可能每周一上班就要「35 versions of this new feature」。

Nisi 说，proof 没法通用，必须针对你的应用。snack 页面的 `prove` 用 Playwright：确认 waitlist 为空，填入邮箱，点「join waitlist」按钮，期望 status 元素显示「waitlisted」，并检查它的文字里有这个地址。写过 Playwright 或 React Testing Library 端到端测试的人都做过这些。把它作为工具交给 agent，它就成了可复现的证据，也是每次页面改动都会跑的回归检查。

Nisi 还提醒：「A successful screenshot isn't proof.」截图必须显示真正一步步跑出来的预期状态；否则「it might just be like another lie」，可能是 UI 里根本走不到的路径，或者带着副作用。

Proser 说，一旦这些循环稳定、可重复，「a failure starts to drive future work」。agent 失败了会自己重试、再验证，你在忙别的，晚上也不用回来把一个 PR 推完最后一程。

Nisi 问谁写好的错误信息，几乎没人举手。Nisi 说，错误信息比以往更重要，因为现在最好的读者是 agent：让失败明确说出为什么发生、应用是怎么进入这个状态的，agent 就能自己修好。

Proser 强调，目的不是在模型外面包一层确定性。agent 有能力，也应该有创造力，「constraints can … equal freedom」。所以要给它结果，Nisi 把这叫作「outcome-driven development」并在实验。规定得太细「can actually lead to lower performance in some cases」；要说的是「you must get here. You'll know you're here when this command returns this.」Nisi 说，最新的 frontier 模型（包括 Opus 5.5 和 Fable）都擅长自己找路：给它终点，它可能绕一点、抓到更多 edge case，或者问出一个关键问题；规定死路线，它就不会偏离。Nisi 说这是到「the latest batch of frontier models」才真正可行的。Nisi 把这个循环比作 red-green-refactor：「Verify this, reproduce it, show me … fix it, and then show me the proof of that.」agent 怎么给出证明，和修复本身一样重要。

## 5. agent 知道自己还没做完的那一刻

Nisi 跑了 `p snack doctor`，它检查 Node 和 Chromium 已安装，session 存在、在运行、已构建。然后是 `snack join ada@example.test`（「Sorry, we don't type anymore」）和 `snack storage`，结果为空。Proser 说：「This is the point at which in the real world … you've just gotten the value out of this entire system, cause at this moment your agent realizes I am not done and I'm not going to drop this work on the floor.」接着跑 `prove`，打开了浏览器，失败了；Nisi 说持久化的修复其实还没应用。

Proser 介绍了「receipts」：每次运行的记录，比如人类怎样算把一张客服工单处理好。agent 的每一次运行现在就是有价值的数据，可以喂给 LLM 找修复；以后自我改进的工具成熟了，价值更大，因为它会成为全面 eval 的数据集，让 agent 在上面 hill-climb。

## 6. LLM judge 和 saboteur 游戏

Nisi 说，修好持久化之后，应用仍可能说谎。刷新后状态是「waitlisted」，截图上的文案却是「you're in, your seat is confirmed」。跑 `p snack judge` 失败了，因为「the confirmation text breaks the contract」。问题是怎么证明页面正常，又不把测试和页面上的具体文字绑死。Nisi 说，答案是「system one」模型。

Proser 解释了这个来自心理学的说法。System 1 是开车时有人突然变道、你本能修正方向；System 2 是「I need to figure out what I'm going to do with my life. So, I'm going to go book an Airbnb and like think about it for 3 days.」到了 LLM 这边，Proser 说，「now we've got Jev and then immediately after we've got Clef from Cloudflare」，「even faster」。Jev 是 TypeSafe AI 2026 年 9 月 15 日发布的「System One model」，返回带置信度的类型化决策（是/否、一个选项、一个分数），不生成文本。Cloudflare 的 Clef 模型发布得更晚（据二手报道是 10 月 1 日，不是 Jev 之后两天）。Nisi 给了一个更简单的版本：问你灯是什么颜色，你马上说「黄色」；问为什么是黄色，你就开始思考了。

然后是游戏。saboteur 在 snack 应用里随机埋一个 bug（`pnpm snack up --bug random`），你作为 detective 只能用 `snack` 找到它、说出它、证明它。现场演示没按计划进行。`doctor` 报告上一条 receipt 失败了，但指向的是「proof … for code that no longer exists」，是之前一次运行留下的。台下有人猜是邮箱错了；Nisi 怀疑端口，后来又觉得端口没问题。Nisi 说：「I need some verification tests around my … contrived verification examples.」

Nisi 列出一轮游戏可能的结局：detective 说出了 bug 并给出输出；说没问题但其实有；说没问题而且确实没有；指认了一个不存在的 bug；找对了 bug 但没有证据。埋的 bug 包括：数据只存在内存里、存了错误的邮箱、丢了确认信息、过度承诺（截图或 judge 能发现）、console error（出现在 receipt 里但不在 console 里），以及根本没有 bug，「and that's a real answer as well」。Nisi 说，每漏掉一次就是「a new line that can go in the map」。

Proser 说，各团队的评分方式会不同：「The most important thing to do is to start and to create one, create an eval baseline and to use it consistently and to evolve it together as a team」，直到每个 PR 都会跑它，告诉你改动能不能安全合入。

## 7. 让 agent 看到开发者能看到的一切

Proser 把这个 workshop 概括为：把开发者以前深夜在 Chrome 里做的事，搬进一个确定性的 CLI。如果你的 web app 还没有「a custom verifier CLI」，那是「one of the highest leverage things you could start with」；再加一个展示「what good looks like」的基础 eval，以及通过 Chrome DevTools 看到性能、网络流量和渲染的能力，iOS 和 macOS 应用也有类似的项目。「You need to give your agent the ability to see absolutely everything that you would normally see as a developer.」

Nisi 补充说，agent 还需要知道什么时候用哪个工具，这来自你自己怎么复现 bug。在 WorkOS，很多是 sign-on 相关：「Is it properly setting a cookie? Is it setting an expiration?」

Proser 又回到 receipts。对电商应用来说，就是完整的用户旅程：用户有没有看到错误，商品目录是不是 7 秒才出来，购物车有没有被放弃。一个整夜运行的系统可以汇总这些记录，发现「eight failures in the caching module」，开一个 PR 并证明修好了。高产出的团队还会附上 agent 走一遍应用的录屏，早上面对「15 important PRs」的 reviewer 可以先看录屏。Proser 说，等自我演化的工具成为默认，手里有干净数据集的团队会比没有的竞争对手「significantly faster」。Nisi 为这一节收尾：模型能跑的 unit test、eval 和 smoke test，让团队能「climb that evidence ladder」，这些证据「really becomes the spec that shows what our app does」。

## Q&A

workshop 中途有两次提问，结尾一次。录音里大多数问题听不清，下面根据讲者的回答还原。

### 确定性和适用范围

**你们是否让模型写 JSON，把代码变成确定性的？** Nisi 说是的，并用 unit test 包住它，捕捉 edge case，确认输出真的代表了 system one 模型应该返回的东西。Nisi 会保留这些测试，防止检查随时间漂移。

**这套方法能用在前端以外吗？** Proser 说团队做的大多是部署在 Cloudflare 上的 TypeScript 全栈应用，「it works perfectly」，也有专门针对后端或数据库操作的 eval。Nisi 说这就是两人不发通用工具的原因：「You kind of have to build up the tools that fit the app.」Proser 补充说，不同业务的 eval 会「wildly differ」，从室内设计到只看速度的领域都不一样。

**练习需要 API key 吗？**（根据回答推断。）Nisi 说：「For that one, yeah, you do.」Proser 说工具把它设成可选，没有 key 每个练习也能跑完，调用也很便宜。Nisi 说新账号送 $5，「and I think that'll last you like 3 years」。

### 怎么保证检查被执行

**skill 要靠模型自己选择去用，是不是该让 CI 强制执行检查？** Nisi 列了几种做法：CI；git hook，「probably the last one I would reach for」；Claude Code hook；或者在后台跑的 Pi extension。Nisi 说，让所有人都用上的最简单办法是「just put an AGENTS.md, not a CLAUDE.md」在仓库里，写上比如每次重大改动或开 PR 之前要跑这个验证；大家「have to go out of their way to ignore that」。Proser 补充说，本地开发时 hook 可以在团队里共享。

**在本地跑检查，相比 CI 有什么好处？** Nisi 说 CI 是一部分，但真正的收益是本地的快速循环：给 agent 一个终点和验证它的办法。这样 Nisi 能并行跑多个 agent，当天是三个（「a very low day for me, but I'm at a conference」），平时 10 到 15 个，而不会回来时发现 agent 说做完了其实没做完。Proser 把目标描述为：晚上回来，早上开的每一条 lane 都已正确完成，不用「turn by turn nudge」任何一个。Nisi 依赖 CI 和自动 code review：团队用 Greptile，「if it gives me a five out of five and all of my CI is passing I feel extremely confident」。验证的意义是更快、更少轮次地到达这个状态，并让 agent 不跑偏，因为 agent 可能加上「10,000 lines of absolutely nothing」，通过 review 却什么也没改变。

### 生产环境

**怎么放心让 agent 的改动上生产？**（根据回答推断；字幕没有清楚标出回答者，看起来是 Proser。）回答建议先部署到 preview 环境验证，再上生产并立即监控。对大部分由 agent 实现的部署，一个正在出现的模式是至少有一个 agent 专门负责 rollout，盯着指标、Datadog 和 feature flag，能判断「this is actually bad and we're reverting it」。「I truly understand and get the … queasiness around it, … but … it seems inevitable.」最后讲者请大家去 expo hall 的 WorkOS 6 号展位。

---

**链接：** [日程](https://ai.engineer/nyc/2026/schedule?session=edb4f982-0cb8-48ee-8bf7-410255a40cb3) · [视频](https://www.youtube.com/watch?v=q4WC4TowkBo) · [Talk 页](https://ai.engineer/talks/q4WC4TowkBo) · [Workshop workbench](https://talks.workos.cloud/beyonddone)（需要登录） · [Case Statement: Building a Harness](https://nicknisi.com/posts/case-statement/)（Nick Nisi） · [Agent Experience: Build without leaving your terminal](https://workos.com/blog/agent-experience)（WorkOS） · [Introducing System One models and Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev)（TypeSafe AI） · [Rewriting Bun in Rust](https://simonwillison.net/2026/Jul/8/rewriting-bun-in-rust/)（Simon Willison）

本文根据官方视频字幕和以上资料整理；这场没有拍 slide 照片，slide 和 demo 按讲者的口述描述。[精确版笔记](../precise/10-12-1315-beyond-done-building-verification-loops.md)保留了时间戳、字幕勘误和待确认事项。
