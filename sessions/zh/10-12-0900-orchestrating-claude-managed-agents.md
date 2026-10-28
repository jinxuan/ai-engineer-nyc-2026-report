[English](../10-12-0900-orchestrating-claude-managed-agents.md) | **中文**

# 让 Claude 自己画组织架构图：flows 把最多 1,000 个 agent 带进 Claude Managed Agents

*在 AI Engineer NYC 2026 上，Anthropic 的 CJ Avilla 和 Harrison Stall 介绍了 flows，也就是 Claude Managed Agents 的 dynamic workflows 功能：Claude 通过 API 自己规划、运行多 agent 拓扑。两人认为，安全地跑几百个 agent，关键在 budget、隔离，以及让凭据永远进不了 sandbox。*

**Workshop · 2026 年 10 月 12 日（周一）· 9:00–11:00am · Sutton Room (NY Ballroom W)**
**讲者：** CJ Avilla，Member of Technical Staff，Anthropic · Harrison Stall，Member of Technical Staff，Anthropic

---

CJ Avilla 从操作系统的历史讲起。1969 年，在「离这里大约 30 英里」的 Bell Labs，Ken Thompson 的妻子带一岁的儿子去加州探亲，他独自有了一个月。Avilla 说，Thompson 把这个月按周切开，kernel、editor、assembler 和「one other thing」各一周，月底做出了 Unix。Avilla 说，能做到靠的是「conceptual integrity」：「一个人能把整个项目装在脑子里，从头到尾做完」。

反例是同一时期 IBM 的 OS/360：一个押上整个公司的项目，按 Avilla 的说法，IBM 估了大约 5,000 man-years，投了多达 1,000 名工程师。Fred Brooks 把教训写进《The Mythical Man-Month》，其中包括「surgical team」：一位掌握全局设计的 chief，身边围着大约十个专家。「我们也想拿同样的结构，把它放大得多得多。」Avilla 说。

Linux 给出了做法。Avilla 举了最近发布的 Linux 7.2：「来自 300 家公司的 2,600 多位开发者」（LWN 统计为 2,652 位开发者；公司数没有核对）。一开始并不是这样。1998 年，Linus Torvalds 被邮件发来的 patch 淹没，在 kernel 邮件列表上说要去度假，Larry McVoy 回了 Avilla 引用的那句话：「The problem is that Linus doesn't scale.」（问题在于 Linus 不 scale。）到 2002 年前后，Torvalds 从星形结构（所有人都把 patch 发给他）转向「layers of trust」：由受信任的 maintainer 审代码，只有例外情况才到他那里。slide 把两者并排：大约 36 个人都指向同一个人，对比一棵每层最多四个人的树。

Avilla 把这和今天对照：管理很多 agent 的工程师「可能开始感觉自己成了瓶颈」，坐在 GitHub 里批一大堆 agent 写的 pull request。这个 workshop 讲的就是把这种结构交给 Claude。

## 1. Flows：Claude 编排自己的拓扑

Harrison Stall 宣布了这个功能。「上周五」（据 Anthropic release notes 是 10 月 9 日），Anthropic 四月推出的托管 agent API Claude Managed Agents 加入了讲者口中的 flows：「下一代 multi-agent orchestration，你可以让 Claude 完整编排它自己的整个 multi-agent 拓扑」。Anthropic 文档把这个功能叫 dynamic workflows，每次执行叫一个 workflow run。

这个想法来自 Claude Code：在那里输入「ultracode」（或者直接要求用 workflow），Claude 就会写一个 JavaScript 程序，把工作分发给很多 subagent。不用手工画「classify and act，或者 fan out and synthesize adversarial verification 那样的」拓扑，「你直接告诉 Claude……Claude 靠模型训练就知道……怎么有效地编排自己」。

Stall 用自己所说的「hand waved numbers」（拍脑袋的数字）解释为什么要这样：单个 agent 大约一小时就「撑不住了」；coordinator 带一组 subagent（Managed Agents 的第一版多 agent 设计）能撑久一点；dynamic workflows 则允许「任意复杂、嵌套的一组 agent 全部并行运行、相互配合」。

Stall 说，自己搭多 agent harness 的团队一再碰到同样的问题：重启 container（「一百个 agent 里有一个……在任务跑到两小时时挂了怎么办」）；一个 coordinator 要盯 60 个 agent（「Linus 那样是 scale 不了的」）；还看不清整张图的状态。Stall 还提起 Opus 4.5 之后的那个 meme：人们把笔记本半开着带在身上，免得 Claude Code 断掉，坐在公园里一直按「approve, approve, approve」。「对我们的野心来说，这根本 scale 不了。」

现场大约一半人听说过 Claude Managed Agents。Stall 介绍说，它是一个让 Anthropic 管理 agent 生命周期的 API：定义 agent（skills、tools、MCP servers），在 session 里运行，通过 durable event log 观察。五月发布的第一版多 agent 要求事先定义每个 subagent，还要监听每个 agent 的 thread，就像操作系统里靠互发消息共享状态的线程。「搭起来不算最简单。」

有了 flows，lead agent 先写计划（Stall 的例子是「把我一百万行的 mono repo 从 Python 迁到 Rust」），然后分发给它自己启动、停止的 agent，分 phase 完成「离散的、可验证的工作单元」。一条 event stream 能看到所有 agent 在做什么；budget 给花费封顶；方向不对时可以通过 API interrupt。和 Claude Code 不同，这一切在 API 后面运行，所以可以按 schedule、由 webhook 或 CI 触发。Stall 举的例子是 GitHub 上的 bug-hunter workflow。

上限由 Anthropic 文档规定：一个 workflow run 在整个生命周期内最多启动 1,000 个 agent，同时最多 64 个在工作（不保证），默认运行 24 小时。只有 session 的 primary agent 能启动 run，所以 run 不嵌套。开启方式是 `multiagent_20261001` 类型。

## 2. Demo：股票 watchlist scanner

「因为我知道在座有……不少做金融的朋友」，Stall 在 Claude Console 里跑了 stock watchlist scanner 模板。「Managed agents」下的 Quick start 页面列着一键模板，最底下有一个 dynamic workflow 标签页。模板会创建一个跑在 Claude Opus 5.5 上的 agent，带 multiagent 类型和一段设定 persona 的 system prompt。

agent 还需要一个 environment，「基本上就是给 Claude 用的一台电脑」。Stall 说，过去 10 到 12 个月里，给 Claude 一台电脑让它「全面强了非常非常多」。Stall 用早上准备好的 environment 开了一个 session，输入 Apple、Nvidia、Amazon、JPM、XOM 和 COST。Sessions 面板里能看到 lead agent（「金字塔顶上的那个人，CEO」）和它的 session thread，每个 thread 是「单个 context window、agent 的一次实例化」，并行启动。早先跑完的一次「每一步大概只有六个并发 agent」；Claude 会按任务决定数量。

## 3. 从 console 到生产

Avilla 接着讲原型之后 flows 往哪里去。通常下一步是把 agent 接进应用：每个工作日开盘前五分钟跑一次 scanner，或者从 Slack 触发。代码上，开启 flows 就是创建 agent 时传 multiagent 配置，并在第一个 event 里告诉 lead agent「run a workflow」。「你要做的就这些。」

默认情况下 Claude 会自己临时造出需要的 subagent。团队也可以传入预先配置好的 subagent，比如专门看 Yahoo Finance、新闻源或 SEC EDGAR 的。agent 可以用自定义 skills、MCP servers 和 custom tools，environment 也可以是放在你自己基础设施里的 self-hosted sandbox。

在 scanner 里，flow 把每个 ticker 交给一个单独的 scan agent，「一个隔离的 context，它只关心自己那一个 ticker」，其他 ticker 的搜索结果影响不到它。之后是同样隔离的 review 阶段。讲者把 scanner 两种方式各跑了一次。对比 slide（具体数字没有念出来）显示 flow 的成本「高得多」，但「找全细节的效果好得多」。flows 适合「我想挖得很深，确保不放过任何角落，也愿意为此花钱」的时候。

prompt 还能决定钱花在哪。Avilla 的例子：scan 用 Haiku（「一个很快、很高效的模型」），每个 ticker 两个 agent；review 用 Fable，「每个 ticker 两三个 agent」，因为细节往往是在 review 里漏掉的。每个 flow 都可以「按你想在这个环节投入多少，伸缩」。对重要任务，Avilla 建议配一套完整的 eval。

这些模板也打包进了 Claude Code 的 Claude API skill：`/claude-api managed-agents-onboard` 加模板名就能搭起来，底下用 Anthropic 的 `ant` CLI 调 API。Avilla 说自己经常从 Claude Code 而不是 console 建 managed agent。团队发现开头的 prompt 越开放越好（「用一个 workflow，每个 ticker 一个 agent……我大概想要的是这些」），先让 Claude 设计 flow，再去微调。

现场扫 QR code 可以领到大约 50 美元的 API credits。Avilla 还提到当周的消息：Max 和 Team 订阅现在附带 API credits，Max 5x 每月 100 美元，Team 最多 500 美元。（Anthropic 文档确认了这些数字，并写明 credits 覆盖 API 和 Managed Agents，不覆盖 Claude Code。）

## 4. Bug Hunter：把 review 的环闭上

第一轮提问之后，Avilla 回到上一届 AI Engineer 的一个主题：「怎么把这些环闭上」，让系统更多地自己跑。Bug Hunter 是 Anthropic 在自己的 pull request 上用的一个 flow，有模板，也在 Claude Code 里。

单个 agent review 一个 PR，效果不如这条流水线。代码被切成 slice，每个 slice 交给一个有独立 context 的 agent，记下「这里可能有问题」。第二阶段再看同一个 slice，对每个候选表态同意或不同意。第三阶段挑出可能是真问题的候选。然后是 prover 阶段：下载 repo，尝试复现每个 bug。实际使用时，结果以 inline PR comment 的形式出现，带严重程度。Avilla 说，它的价值就是好的人类 reviewer 带来的东西：对 PR 根本没提到的「代码库某个遥远角落」的了解。用 `/claude-api managed-agents-onboard bug-hunter` 搭建模板。

## 5. Explainer Studio：做视频的 flow

关注 X 上 @ClaudeDevs 的人不多（「你们关注得不够」）。这个账号发布新功能时常配动画讲解视频，其中很多是 flow 做的。它先调研细节，然后规划、侦察、事实核查，写一份 design brief，再交给逐场景制作和做动画的 production 阶段；实际还有音效、配音、脚本等阶段。Avilla 说，一条视频四年前可能要「20 个小时左右」，现在是「一个很短的 prompt」。

示例视频讲的是 dynamic workflows：任务是在 300 份合同里找 change-of-control 条款，接着讲 session threads、multiagent 设置，以及为什么要设 budget。旁白是 Avilla 自己的声音。有人问能不能直接发到 YouTube：「不能发 YouTube……不过我们应该做。」现场投票，「马桶是怎么工作的」以微弱优势胜过自行车。视频带 Claude 品牌和 Anthropic 的字体；Avilla 建议先在 Claude Design 里建一套品牌设计系统，再交给 flow。

## 6. 单个 agent 为什么会失败：偷懒、偏爱自己、目标漂移

Avilla 说出了团队做 dynamic workflows 时看到的三种失败模式。

**Agentic laziness（偷懒）。** 让一个 agent 检查 50 个服务、找故障的 root cause（「别碰 production」），看到 35 个健康之后，它开始告诉自己「看起来都没问题」，就不再往下看。coordinator 也有同样的问题：它要记数，它的 subagent 也会偷懒。flow 给每个服务一个专门的 agent，所以「到最后，我们确定每一个服务都查过了」：fan out 加 loop until done。

**Self-preferential bias（偏爱自己）。** 让 agent 写一首诗，再问它好不好：「它会说，『好』。」代码也一样。advisor pattern 把 generator 和 judge 分开，利用「generator discriminator gap」，但规模一大，一组固定的 agent 就不够用。flow 可以把「有自己隔离 context 的 adversarial review」铺到很多 agent 上。

**Goal drift（目标漂移）。** 「别碰 prod」是 prompt 的最后一句。context 一次次填满、compact，这条指令可能就丢了。flow 在开头定一次目标，然后「以非常一致的方式，复制进每个阶段的每个 sub agent」。

## 7. 规模化的安全：declare、contain、detect

Stall 说，安全是「搭 agent 时要做对的那一件事」，因为 flows 会把一个 agent 变成可能几百个。slide 上列了「六个非常具体的」风险，Stall 讲到的是这些：

| 风险 | Stall 的说法 |
| --- | --- |
| Prompt injection | 读开放网页的 agent 可能捡到「ignore previous instructions … exfiltrate」之类的文字。Anthropic 在模型层面处理，也用「做 tool call 检查的分类器，类似 Claude Code 里的 auto mode」。 |
| 凭据泄露 | 「你给 Claude 什么凭据，它就扮演那个身份。」VM 磁盘上的 API key 文件，就算藏起来也可能泄露，所以「凭据永远不必碰到 sandbox」。 |
| 数据外泄 | 拿到敏感数据的 agent 不能把它分享给第三方。 |
| 越权 | budget 和其他限制，加上 permission policy，只有你授权的工具才能运行。 |
| 可见性 | durable event stream 记录每个 agent 的完整轨迹，「基本门槛」。 |

Stall 说，开发者的工作归结为「declare, contain, detect」。declare：说清 agent 是什么、有哪些权限、skills 和 tools。contain：从物理上限制它，通过 environment 策略只允许访问某些域名，或者只开 web search 不开 web fetch，这样「即使在最坏的情况下，它能造成的伤害……也很小」。detect：通过 event stream 发现问题，把跑偏的 session interrupt 掉。

## 8. 把 brain 和 hands 分开

Stall 说，Managed Agents 四月发布时有个 meme，说 Anthropic「把这场争论了结了」：「brain 和 hands 应该分开」（这也是 Anthropic 一篇工程博客的主题，「Scaling Managed Agents: Decoupling the brain from the hands」）。brain 是跑 inference 的 loop 加上 session state；hands 是 agent 行动的地方，比如在 sandbox 上跑 grep，或在你自己的 Mac 上跑一个 custom tool。「拿到 hand 的访问权，应该……完全没有办法去操纵 brain。」

凭据也按同样的方式分开。每个需要 secret 的 container 里放的都是假值，直接拿去调用不会成功。只有在出口的 egress proxy，它才被换成真正的凭据。「从物理上就没有办法 prompt Claude……把凭据从 sandbox 里弄出来。」目标是「你不应该能一枪打到自己的脚」，把 key 泄露出去。

## 9. API 里的控制手段

Avilla 逐一讲了这些控制。任何 session 都能设 budget：「跑这个 flow，你有 1.25 美元。」flow 没跑完就到了上限，可以追加，它会接着跑，不用重做已完成的工作。budget 和 Managed Agents 的另一个功能 Outcomes 搭配：「这是我想要的 outcome、我期望的 rubric，这是 budget。」第二段示例代码发送 `user.interrupt` 事件，Avilla 说它「会停下 flow」，然后你可以重新给 prompt。（Anthropic 文档的说法更窄：interrupt 只停 agent 当前的 turn，「不结束任何 run」；要停 run，得发消息让 agent 去停。）Avilla 建议做个练习：给 flow 设一个故意偏低的 budget，然后追加，看它恢复运行。

因为每个事件都有记录，可以让第二个 managed agent 审计第一个：通过 API 拉出 transcript，问它「告诉我我的 agent 有没有做什么意外的事，或者访问了什么……不该访问的东西」。

Avilla 说，「最容易摘的果子」是把 permission policy 从每次都问改成 auto mode。人在公园里一路点「approve, approve, approve」，「大概 97% 的时候都会点同意」；Claude 用 auto mode 判断每个 tool call 是安全还是有破坏性，「耐心得多」。（Anthropic 八月关于把 auto mode 设为 Claude Code 默认模式的文章给出了同样的 97%，并报告一项受控研究里人工审查拦下 13.6% 的危险命令，auto mode 拦下 89%。Managed Agents 在九月加入了 `auto` permission policy。）

## 10. Vault、最小工具集和网络白名单

Stall 最后一个例子用的是 Twilio API。天真的做法是上传一个带 key 的 `.env` 文件，让 container 里的 Python 脚本调 Twilio，key 就以明文留在那里。正确做法是把 key 存进 vault，作为 environment variable 类型的凭据。injection location 设置说明 secret 在出站流量里出现在哪里（bearer auth header 还是请求体），allowed hosts 列表说明它只能被替换给哪个域名，因为「我们不希望 Twilio API key 被发到……mydomain.com」。创建 session 时带上 vault，平台在 egress 层把占位符换成真值。

工具集要尽可能小。web fetch 和 web search 可以关掉。MCP 工具要格外小心，因为事先不知道会返回哪些工具：「万一 [GitHub] 被攻破了，攻击者加了个……我的数据外泄工具呢？」所以 MCP 工具可以逐个开启。最后，environment 的网络可以收紧，比如只允许 `api.twilio.com`；调用其他任何地方，「会在……防火墙这一层物理上失败」。

Avilla 回到开场收尾：「我们……很多时候是在重新发明」1960、70 年代那些组织经验的「轮子」。「从模板开始，把它改成……你自己的。告诉我们你发现了什么。」

## Q&A

提问分两轮，一轮在 demo 之后（大约半小时），一轮在结尾。录音里大多数问题听不清，下面按讲者的复述整理。录音分辨不出是谁回答的，就记在两人名下。

### 规模和野心

**最近 Navier-Stokes 之类的科学成果集中出现，是用这种方式做的吗？** 「也许我们的……demo 里，股票 ticker 不够有野心。」Stall 说。flows 是「一种……把 test time compute 砸到难题上的方式」；Stall 不能介绍 Anthropic 内部的研究 harness，但把它做成 API，是让这类工作「普通公众也能做」的一步。Avilla 补充说，旧的 coordinator 加 roster 设计限制 subagent 数量，flows 允许「我记得是 1,000 个 agent」，lead agent 还可以同时或接连跑多个 flow。周五，Avilla 用这个 prompt 起了一个 flow：「Ken Thompson made Unix in a week. Can you try and make an operating system that's Unix compatible with a Python interpreter? Use flows … feel free to be brave and use up to 1,000 agents」（Ken Thompson 一周做出了 Unix。你能不能试着做一个兼容 Unix、带 Python 解释器的操作系统？用 flows……大胆点，最多用 1,000 个 agent），外加一个 GitHub repo。Avilla 原本希望周一早上 9 点能拿出成品。它跑了「大概 85 个小时」还没结束，进行到第三、四个 flow，repo 里有大约 1,000 个 commit。

**agent 加多了会不会边际递减，像公司变大后的协调成本？** Stall 没有数字：这个领域很新，「每个 subagent 的能力曲线」以后也许能算出来。Avilla 补充，同样 100 个 agent，放在 flow 里效果更好，「因为各个 context 之间有组织和隔离，有 gating，有不同的阶段」。

**Claude 怎么知道该用多大规模？** 「evals and lower defaults」（靠 eval 和更低的默认值），Stall 说。扫一遍股票不该起 1,000 个 agent；Navier-Stokes 那样的问题，可能需要你明确推着 flow 加大并行度。

**拓扑能嵌套几层？** 目前「只有一层嵌套」，不支持任意复杂的 fan-out；等「安全这些东西真正锁定」之后，可能会支持更深的嵌套。（Anthropic 文档的说法一致：run 不嵌套。）

**大项目应该预先建好依赖图吗？** 「当然。」作为文件上传进 session，告诉 Claude 它在哪。

**一个两天的 run 跑到一半有了新信息怎么办？** interrupt session，补上信息（「lemma X 可能对你有用」）。primary agent 了解 flow 的状态，可以启动一个新 flow，同时用上新信息和历史。不支持的是在原计划中间插一步、再重跑后面的步骤。

### 运行、观察和引导 flows

**Claude Code 里的 Claude API skill 是在我机器上跑，还是在 Managed Agents 里跑？** Avilla：Claude Code 按你的设置在本地或云端跑，它是另一套系统；它发 API 调用去创建 agent、session 等资源。

**运行时能看到拓扑吗？** Stall 说可以，在 console 里看；每个 subagent 的 session thread 也都能通过 API 拿到，可以自己做 UI。Avilla 演示了在 console 里钻进单个 subagent 和它的事件，包括 lead agent 传给每个 subagent 的 payload。Stall 指出 dashboard 本身就建在 public API 上：「你在这里看到的任何东西，都完全可以自己手搓。」

**模型知道自己的 budget 吗？** 目前不知道。budget 是防止开销失控的 guardrail，而且「如果你把 budget 告诉它，Claude 会有点焦虑」，Stall 说。Avilla 讲了机制：session 到了比如 5 美元，你会收到 budget reached 事件，可以在 session 运行中追加 budget，flow 从停下的地方继续，已完成的工作不会丢。

**subagent 之间能互发消息吗？** Stall 说，API 里不能直接发，但它们共享一个文件系统，可以通过它传话；API 能看到 agent message sent 和 received 事件。

**context 怎么隔开？** 每个 agent 跑在自己的 session thread 里，「在数据库里物理上就是分开的 context window」，有自己的 system prompt；agent 之间的消息都看得到，「如果你想确认……某个中层管理 agent 没有对它的下属做你不想要的事」。

**共享文件系统能分区吗？** 目前只能靠 prompt；没有目录级的权限层，Stall 觉得这「挺有用」，答应带回去。有人问 hooks，Stall 指向 tool permissions：读文件请求可以转到你的系统，由它检查是哪个 agent 在读，比如拒绝访问 `/mount/foobar`。

**像造操作系统这样的长任务，怎么查看中间状态？** Avilla 的 prompt 里要了一个文档写手团队（「用 Sonnet 5.5……我很喜欢它写出来的文档」），在 GitHub repo 里给每个阶段写文档。也可以 interrupt，或者让 Claude 持续更新一个报告文件。Stall 补充，写进 outputs 目录的东西会持久保存，可以通过 Files API 下载。

**flow 中途能加入人的反馈吗？** 给 Claude 一个 custom tool，比如「request human feedback」，它调用时你来回答。

**怎么防止无限循环或级联错误？** 「算是能吧」，Stall 说。设 budget；除此之外，Claude「很擅长发现行为异常的 subagent」并把它们剔除，从过去的 run 里学习也有帮助。

**没有变化的时候，flow 能只跑一部分吗？** 平台上目前不行，只能告诉 Claude 按条件行事。Stall 会把这种判断放在应用层。单个 agent 30 秒能做完的任务不适合 flows，flows「是给……有野心的复杂任务用的」。

**能不能按工作类型而不是 system prompt 来调整行为？** Stall 提到按 session 设置的 effort，以及按 session 覆盖 agent 字段。

### 让 flows 随时间变好

**怎么随时间降低 token 开销？** Stall 说这是 hill-climbing 问题：在 Claude Code 里用 hill-climb skill 优化 prompt，或者把运行的 transcript 交给 Claude，问 flow 哪里做多了、哪里做少了。Avilla 补充 Dreams：Managed Agents 的一个功能，拿一组 session 去改进挂在上面的 memory store。跑了比如十次之后，dream 可以「把记忆凝练、改进，从一个 session 带到下一个」。Stall：「我刚才基本上是在描述手搓你自己的 dreaming。」

**flows 能改进自己的 prompt 吗？** 不能自动做。deployments 可以按 schedule 跑 session，但没有按 schedule 自我改进的机制，Stall 觉得可以用这些组件搭出来。Avilla 介绍 `/claude-api hillclimb` 命令是「把 Andrej Karpathy 的 auto research……做成一个 skill」。新的 Haiku 模型出来时，Avilla 给家里做了个猜字谜游戏（一个摄像头、一张卡片，Haiku 来猜比划的动作），对它的 prompt 跑了 hill-climb。Claude 生成了一个 artifact，让 Avilla 录下自己划船、打网球的片段，再用这些片段调 frames per second 之类的细节。

**只能靠品味判断的输出，比如视频，怎么迭代？** Avilla 建议从 Claude Code 开始：手工或用 dynamic workflows 把流程摸清楚，让 Claude 把它写成 skill，在 Managed Agents 的 flow 里用这个 skill，再由人在两两对比的输出里挑一个，用 hill-climb 改 prompt。

**用 markdown 定义和观察 agent，是对的做法吗？** 「看情况」，Stall 说，但「如果你把 API key 放进 markdown 文件……那肯定不行。别这么做。」Avilla 补充一个技巧：一份主文档做索引，指向其他文档（kernel 在这、assembler 在那、editor 在那），每个 subagent 只读自己需要的。

### 集成和运行环境

**workflow 能打包成 plugin 吗？** 目前不能。skills 之类的单个 plugin 组件已经能直接放进 API。workflow 生成的 prompt 是个文本文件，可以提交进 Claude Code，「大概……八九不离十」。

**能让一部分 agent 跑在 Mac 或别的机器上吗？** 用 custom tools：模型请你的代码去执行动作，你在那台机器上执行，再把结果返回。

**数据在 VPN 后面怎么办？** MCP tunnels 让 agent 私密地访问 MCP，按 Stall 的说法目前基于 Cloudflare tunnels，不会把任何东西暴露到公网。

**self-hosted 会有什么不同吗？** 有些功能在 self-hosted sandbox 上还不完全支持，但 flows 支持。

**Bug Hunter 怎么处理跨 repo 的依赖？** 「基本由 Claude 决定」：你授权的每个 repo 都会挂进 container，有干净的 Bazel build 就用它，没有就搜符号。每次 PR review 都重新挂载 repo，就像 CI 的 checkout。

**游戏内 telemetry 呢？** event stream 可以实时流式读取，也可以列出来。Managed Agents 接受图片输入，但把游戏截图发过去做实时动作「用户体验会很差」，因为延迟；拿截图调试一次崩溃则「完全可行」。

**event stream 有什么好用法？** Stall 提到内部有人做了一个竞技 Catan 模拟，用事件解释每个玩家为什么这么走。Avilla 建议做聊天界面，tool call 可以展开成 generative UI，比如 watchlist scanner 发现有意思的东西时画一张带技术分析的图。

**怎么在组织里共享 agent？** Stall 提到一个 Terraform provider 和 console 的导出功能，但没有一键分享。Avilla 说大多数客户会在上面做一层产品：比如在「一个非常流行的项目管理工具」的 Kanban 看板上拖一张卡片，就在后台启动一个 managed agent session 或 flow。

**Claude Tag 和 flows 是什么关系？** Stall 说，Claude Tag 是把团队信息连起来的 Slack bot，「是一个有点正交的产品面」；你可以用 flows 做类似的东西。Avilla 说 Anthropic 各产品之间的缝会越来越少，Claude Tag 在内部是「每天都在用的主力」。最后有人问 Anthropic 内部怎么工作，讲者说他们「非常 Claude Tag pilled」：它会看过去一天的 Jira 动态并建议改 ticket，「你只要说 @Claude 每天……早上 6 点做这个……开箱就能用」。

**什么时候用 ultracode，什么时候用 flows？** ultracode 是坐在笔记本前、在 Claude Code 里交互着用；flows 用于可重复的事，从任何产品通过 API 触发。「我们其实是把 Ultra Code 的……抽象或方法，推广成了 Claude Managed Agents API 上的 flows。」

**平台上发生恶意行为，谁负责？** 「很遗憾，我不是律师。」和任何平台型业务一样，要「defense-in-depth」（纵深防御）：技术栈的每一层都尽力而为。session 到此结束。

---

**链接：** [日程](https://ai.engineer/nyc/2026/schedule?session=c415d432-5b6f-4b4a-9d2c-2e02cfde99db) · [视频](https://www.youtube.com/watch?v=kiClTcJUnhc) · [Talk 页面](https://ai.engineer/talks/kiClTcJUnhc) · [Multiagent orchestration 文档](https://platform.claude.com/docs/en/managed-agents/multiagent-orchestration) · [Workflow runs 文档](https://platform.claude.com/docs/en/managed-agents/workflow-runs) · [Claude Platform release notes](https://platform.claude.com/docs/en/release-notes/overview) · [Claude Code dynamic workflows](https://code.claude.com/docs/en/workflows) · [Scaling Managed Agents: Decoupling the brain from the hands](https://www.anthropic.com/engineering/managed-agents) · [Auto mode is now the default in Claude Code](https://claude.com/blog/auto-mode-default-in-claude-code) · [API credits for Max and Team plans](https://platform.claude.com/docs/en/about-claude/api-credits-for-subscribers) · [Latent.Space 对这场 workshop 的报道](https://latent.space/p/claude-dynamic-workflows) · [Larry McVoy, "A solution for growing pains" (1998)](https://lkml.rescloud.iu.edu/9809.3/0957.html)

本文依据官方视频字幕和上面列出的资料写成；这场没有 slide 照片，slide 内容按讲者口述描述。[精确版笔记](../precise/10-12-0900-orchestrating-claude-managed-agents.md)保留了时间戳、完整的提问列表，以及演讲和文档说法不一致的地方。
