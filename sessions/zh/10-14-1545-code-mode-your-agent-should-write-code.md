[English](../10-14-1545-code-mode-your-agent-should-write-code.md) | **中文**

# 别再给 agent 一份工具菜单，给它一个 sandbox

*在 AI Engineer NYC 2026 上，Arize AI 的 Mikyo King 主张 agent 应该针对工具写代码，而不是一次调一个工具；King 介绍了 Arize 如何把「code mode」做进 Phoenix 的 agent，坦率讲了代价，最后报告了 Arize 自己 eval 里 token 减少 43%、成本降低 31% 的结果。*

**Breakout · 2026 年 10 月 14 日（周三）· 3:45–4:03pm · Turing Room (Riverside Ballroom) · Evaluations Track**
**讲者：** Mikyo King，Arize AI Head of Open Source

---

Mikyo King 自我介绍是「负责 Arize AI 的开源团队」，做 Phoenix（「一个完全开源的 observability 和 eval 框架」）以及 Arize 自己的 agent。这场 talk 要讲清楚 code mode，以及「我们拥抱它的过程」。（公开的视频标题改成了「Stop Giving Your Agent Tools. Give It a Sandbox.」）

King 从历史讲起。30 年来，软件分成两类：给工程师的工具，和给其他所有人的工具。要把收件箱里所有发票都取出来，程序员会写一个脚本，大概「50 行代码」，逐封检查有没有附件，下载并归档。不会编程的人只能手动下载，「受限于软件的界面」，或者买个软件来做。「Claude Code 这类东西真正有意思的地方，是这条边界正在彻底消失。」King 说：coding agent 可以当场造出你需要的任何软件。模型实验室也清楚这一点：「看看 SWE-bench 这些 benchmark，这些模型被训练去做的就是这件事。」所以你自己搭的 agent 也应该能做到。

## 1. 工具太多的问题

![The too many tools problem](../../slides/10-14-1545-code-mode-your-agent-should-write-code/01.jpg)
*六个 MCP server 的工具定义，在开始干活之前就占满了大半个 context window。*

常见的 agent 设计是给模型「一大份工具目录」。用户提一个问题，模型调一个工具，结果塞进 context window，模型推理后再调下一个。幻灯片把这画成一条 context window 进度条，大部分被工具定义占满，后面跟着用户问题、交替出现的 tool call 和结果，模型同时连着六个 MCP server：CRM、Meetings、Email、Calendar、Docs 和 Tasks。

King 说，工具不多的时候，这样「效果还挺好」。但现在团队想要更多 emergent behavior、更多 context、更长的任务周期，context 就膨胀起来：和任务无关的工具定义、返回垃圾内容的工具、需要分页的数据。「你会遇到 context rot。结果还会有 multi-turn，很多很多轮，你还得处理 compaction。」

## 2. code mode 是什么

King 说，code mode 是「另一种架构」。agent 拿到的不是一份包含所有工具的菜单，而是搜索所需工具并执行它的能力，执行发生在「一个编程环境」里，所以工具可以顺序或并行运行，外面还能套条件判断。「一轮里就能调用多个工具，而且只把必要的东西推回模型的 context window。」

![模型给工具编程](../../slides/10-14-1545-code-mode-your-agent-should-write-code/02.jpg)
*一个请求，用一段短程序回答，调用了五个可用工具中的四个。*

幻灯片上的示例请求是「Attach today's Acme call to the Acme deal in our CRM」（把今天和 Acme 的通话附到 CRM 里的 Acme 交易上）。模型写一段在 sandbox 里运行的程序：

```python
call = find_meeting("Acme", "today")
text = get_transcript(call.id)
deal = find_deal("Acme")
attach(deal.id, text)
return "attached"
```

用了四个工具；第五个 `search_email()` 在旁边没被用到，也从没进入 context。

![tool calling 对比 code mode](../../slides/10-14-1545-code-mode-your-agent-should-write-code/03.jpg)
*示意性对比；King 说两边都是「假设的」。*

对比幻灯片把两者并排放在一起：

| | 轮数 | 已用 context | 成本 |
| --- | --- | --- | --- |
| Tool calling（一次调一个） | 8 | 59% | $0.59 |
| Code mode（sandbox 里跑一段程序） | 1 | 2% | $0.02 |

King 介绍这些数字时说它们是「hypotheticals」（假设的），不是实测。用 tool calling，context 随每次调用增长，带来延迟、成本和更多轮次，于是「用户拿到价值所需的时间变长」。code mode「做得好的话」，一轮里需要调多少工具就调多少，只把需要的东西放进 context。Arize 的实测结果在 talk 后面。

## 3. code mode 为什么有效

![Why code mode works](../../slides/10-14-1545-code-mode-your-agent-should-write-code/04.jpg)
*四个理由，各配一张图：progressive disclosure、可组合、token 效率、emergent behavior。*

King 给了四个理由。

**Progressive disclosure（渐进披露）。**agent 不需要一开始就拿到完整目录；「它可以只挑自己需要的工具」。幻灯片上是一个标着「every tool」的网格，只有两块进入了 context。基础模型厂商现在也把 tool search 做成了内置功能。

**可组合。**工具可以顺序或并行调用，可以展开也可以串联。幻灯片上的代码遍历会议，逐个取 transcript，只有在 transcript 提到定价时才给对应的 lead 附上摘要，标注是「loop, call, branch, chain」。King 指出 Claude 的 dynamic workflows 也基于同样的想法，只是用在 sub-agent 上。

**token 效率。**工具输出不必一股脑倒进 context；「模型可以动态地裁剪、搜索特定部分，只把需要的东西放回去」。幻灯片上，transcript 留在 sandbox 里，只有答案进入 context。

**Emergent behavior**，「我们拥抱 code mode 的主要原因之一」：agent 可以自己造工具。比如让它「把这些记录里的个人身份信息都去掉」，tool-calling agent 要么没有对应的工具，要么试图在 context window 里自己处理，「但那样你就在 context window 里泄露信用卡号了，这可不太好」。幻灯片展示了一个「written by the model」（由模型编写）的函数 `redact_pii`，用正则表达式替换卡号，并加入工具栏，和 `get_transcript`、`summarize` 并列。King 关于 code mode 的博客文章用的也是这个信用卡例子。

## 4. Arize 如何把 code mode 做进 Phoenix

![PXI 里的 code mode](../../slides/10-14-1545-code-mode-your-agent-should-write-code/05.jpg)
*PXI 是 Phoenix 里的 agent，它把代码发给 Phoenix server 上的三个执行入口。*

Arize 在 Phoenix 里构建了自己的 agent 和 harness；Arize 的文档叫它 PXI，「仿照 coding agent 设计，但对准的是 Phoenix」。幻灯片显示 PXI 把代码发给 Phoenix server，server 提供三个执行入口：

| 执行入口 | 幻灯片上的例子 | 背后是 |
| --- | --- | --- |
| `bash` | `gql … > spans.json; grep -c ERROR spans.json` | GraphQL API |
| `execute` | `bad = [r for r in runs if r["error"]][:3]` | REST API |
| `executeSql` | `SELECT name, count(*) FROM spans GROUP BY 1` | Database |

「我们从 Bash 开始。」King 说，这是把 sed、grep 这类 Linux 基础命令和 Arize 自己的 CLI 组合起来、把结果存到文件里备用的好办法。接下来是 Python sandbox，Arize「把所有 REST CRUD 接口都提了进去」，用来取 trace 或编辑 dataset，这样 agent 可以组合增删改查操作。最后是 SQL。不是所有事都是 CRUD：像「告诉我所有 P95 延迟是 X、Y、Z 的 LLM span」这样的问题需要聚合查询，「你不会想为了这个把所有 trace 都翻一遍」。

Arize 在 8 月的一篇博客里介绍了 SQL 这部分工作。它给 agent 两个只读工具 `describeSqlSchema` 和 `executeSql`；在 code mode 下，agent 的 Python 在 server 上一个受限的 sandbox 里运行，只有最终返回值发给模型；SQL 会被解析、限定在允许的表和列上、重新生成，再由数据库检查一遍；运行时间、行数和结果大小都有上限。在 8 个遥测问题上各跑 3 次，SQL 加 code mode 平均每个问题约 $0.23，只用检索工具约 $3.97，「成本大约是十七分之一」。

![浏览器里的 code mode](../../slides/10-14-1545-code-mode-your-agent-should-write-code/06.jpg)
*PXI 写一段在 Web Worker 里运行的脚本，来操作 Phoenix 界面。*

Arize 还做了「一套绑定在我们 UI 上的编程范式」。幻灯片展示了 Phoenix 里一个名为 `claude-code` 的项目的 Spans 页面，标着「driven by PXI」，旁边是 PXI 面板。被问到「Show me yesterday's failed tool calls」（给我看昨天失败的 tool call）时，PXI 先搜索了可用的浏览器操作，然后在 Web Worker 里运行了一段脚本：`await ui.timeRange.set(yesterday)`，以及一个 `span_kind == 'TOOL' and status_code == 'ERROR'` 的 span 过滤条件。底部三步：写一段 `ui.*` 脚本，在 Web Worker 里运行，驱动真实的应用。King 说，因为 worker 有「直接连到浏览器里的双向数据绑定」，「你几乎得到了一种类似 computer use 的 emergent behavior……看它自己忙活还挺好玩的」。

## 5. code mode 的代价

「code mode 显然不是免费的。」King 说。「你面对的是 LLM 生成的代码。」

![LLM 生成的代码](../../slides/10-14-1545-code-mode-your-agent-should-write-code/07.jpg)
*运行模型写的代码的四种风险。*

幻灯片列了四种风险：

| 风险 | 幻灯片上的图示 | King 的说法 |
| --- | --- | --- |
| 资源限制 | CPU、内存和时间三条进度条；时间超限，任务被「killed」 | 托管的 sandbox 需要管理 CPU、内存和存活时长 |
| 网络出口 | sandbox 里一个「injected README」把数据发到 evil.com | 「网络是个大问题……你永远不知道网上有什么」 |
| sandbox 里的 secret | `API_KEY=sk-ant…` 加上 `print(env)`，key 就进了日志 | 「每家 sandbox 厂商都在想不同的 vaulting 策略」；最重要的是确保「这些 secret 不会回到 context window 里」 |
| 权限过宽 | 一个 admin token 允许「read logs」，但也允许「delete prod」 | 「权限通常都给得过宽」：前五步可能没问题，第六步就不行；或者每一步都没问题，组合起来就不行；需要的地方加 guardrail |

讲 secret 那段时，King 还提到一个最近发布的、用于在 sandbox 里快速做决策的产品，但视频字幕和现场录音里名字都听不清，这里略去。

![运维复杂度](../../slides/10-14-1545-code-mode-your-agent-should-write-code/08.jpg)
*四类运维开销。*

运维复杂度是讲代价的第二张幻灯片：

| 问题 | 幻灯片上的图示 |
| --- | --- |
| 启动成本 | isolate 启动要几毫秒，microVM 要几百毫秒，完整 VM 要几秒 |
| 更难调试 | tool calling 显示三个已勾选的步骤（`get_spans()`、`filter()`、`summarize()`）；code mode 只显示一个 `run_code()`，返回「summary: empty」 |
| 工具发现 | 先搜索（`search("traces")`），再了解（`get_traces(project, limit)`），然后调用（`get_traces("prod")`） |
| harness 重复建设 | harness 里有 tool search 和代码执行，MCP server 里又有一遍 |

sandbox「不是每次都能完美启动」，所以必须处理失败。调试更难，「因为你现在调试的是代码，而不是单个工具」，而且「工具参数的准确性成了大得多的问题，因为现在工具参数就是代码」。以前模型一开始就知道有哪些工具，现在工具发现需要多一次握手。另外，MCP server 和 harness 都在加 code mode，造成「双重 sandbox 问题」：一个 sandbox 把输出序列化成 JSON，再被解析、喂进另一个 code mode，「这种重复劳动」。

## 6. 尚未解决的问题

![Open problems](../../slides/10-14-1545-code-mode-your-agent-should-write-code/09.jpg)
*持久性、确定性、图片和二进制数据、小模型。*

King 列了四个尚未解决的问题，并补充说「它们不算是 blocker」：

**持久性（durability）。**幻灯片上，现在的运行是一排方块，其中一块是红的；有快照的运行全是绿的。「比如你写了一大段要执行的代码，中间断了。你没法从那一步恢复。」

**确定性。**幻灯片显示 JavaScript 产生了不同的 hash（`a3f ≠ 9c1`），而 Starlark 产生的是同一个。如果确定性和数学计算很重要，语言选择就很重要：「LLM 很擅长写 JavaScript，但它可能不太适合做数学。」

**图片和二进制数据。**返回 JSON 的工具可以干净地传给代码；图片则以 base64 字符串的形式到达；一个 2 GB 的文件「太大，传不了」。code mode 很适合在 sandbox 里处理图片，但把图片取出来很麻烦，因为把整张图片序列化就会让它落进 context；「你可能得做某种签名的图片交接」。

**小模型。**「frontier model 用 code mode 非常厉害」，但为了省钱换成小模型，能力可能就不够了。

## 7. Arize 怎么衡量的

Arize 在自己的平台上评估自己的 agent，并通过 Harbor 框架运行它。Harbor「把 agent 放进 sandbox 环境，在网络关闭的情况下按明确的指令运行」，并重复多次 trial 来「消除大量 flake」。（Harbor 是 Terminal-Bench 团队做的 eval 框架。）eval 写成 Harbor verifier；「Harbor 某种程度上源自 RL 框架。我们给每个任务一个 1 或 0」，Arize 按任务跟踪 pass@k，因为 non-deterministic 的 agent 在不同运行中可能走不同的 trajectory。

![code mode 为 PXI 省下了什么](../../slides/10-14-1545-code-mode-your-agent-should-write-code/10.jpg)
*PXI 在 code mode 前后的 Phoenix 实验对比。*

结果幻灯片展示了一个 Phoenix 实验对比，旁边是四个数字：

| 指标 | 使用 code mode 后的变化 |
| --- | --- |
| Tokens | −43% |
| Cost | −31% |
| Reward | Same |
| Latency | +0.6% |

「code mode 之前和之后，我们把 token 量减少了 43%，成本降了 31%。」King 说；sandbox 带来一些额外开销，「但延迟基本保持不变」。这些收益「在多个入口上都成立，不管是 Claude Code、MCP 还是 CLI」。这个实验的任务集和设置没有给出，这些数字只出现在幻灯片和 talk 里；King 9 月关于 code mode 的博客文章里没有这些数字。

![改进 agent](../../slides/10-14-1545-code-mode-your-agent-should-write-code/12.jpg)
*改进循环：跑 eval，发现回退，分析 trace，调整 agent，重跑。*

最后一张技术幻灯片展示了 Arize 用的循环：跑 eval，发现回退，分析 trace，调整 agent，再重跑同样的任务。「Look at the data」下面的截图是一条 trace（状态 OK，总成本 $0.44，延迟 2m 52s，模型 claude-opus-5，一次调用，26,665 个 prompt token、297 个 completion token，调用的是 `mcp__phoenix__execute`）；「Tune the tools and instructions」下面是一个 coding agent 的终端，显示一张标题为「Code mode, before + after (18 tasks, 2 runs after the change)」的表，比较各实验的 reward、轮数和 prompt token，照片里字太小，看不清。

King 说，同时拥有 trajectory 和 eval「是一个巨大的突破口」。有时加一个新工具，一开始效率反而下降；有了数据，「我们就能告诉 coding agent，在这些具体场景下，指令里哪些部分把它引向了错误的方向」，然后在 eval 上做 hill-climbing。

## 8. 为 code mode 辩护

![The case for code mode](../../slides/10-14-1545-code-mode-your-agent-should-write-code/13.jpg)
*结尾幻灯片：更少的轮次，更便宜，更聪明。「Sandbox it. Trace it. Improve it.」*

结尾幻灯片列了三个好处：更少的轮次、更便宜、更聪明，以及一句口号：「Sandbox it. Trace it. Improve it.」（放进 sandbox，记录 trace，持续改进。）「我们在 Arize 很喜欢 code mode，因为轮次更少。」King 说。「我们希望你能接入任何模型……所以我们希望它更便宜；我们也对 emergent behavior 和 agent 能完成的长周期任务非常兴奋。所以，希望这场 talk 能说服你给你的 agent 一个 sandbox，然后用我们的工具来评估它。」

官方 session 介绍还引用了一项研究：「只用 bash 的 agent 在同一个 benchmark 上比 typed tool 目录高出 21.8 到 24.5 分，同时花的 token 更少」，King 在台上没有提到。这篇论文「Is Bash All You Need?」（arXiv:2609.11999）用 Opus 4.8 和 GPT-5.5，在两个企业 benchmark 上比较了五种工具接口。bash-only 相对 typed tools 高出 21.8 到 24.5 分是在 TheAgentCompany 上；在 APEX-Agents 上是 4.8 到 7.4 分。bash-only 的总 token 少用 19% 到 72%，在 bash 之上再加 typed tools 或保存下来的自制工具，没有带来可检测的提升。这篇论文比较的是 bash 和 typed tools，而不是 code mode 和 tool calling；Arize 自己的结果是 reward 不变、成本更低，而不是分数更高。

---

**链接：** [Schedule](https://ai.engineer/nyc/2026/schedule?session=8413d6ee-ceb1-4095-9f27-49d518f1dd5a) · [Video](https://www.youtube.com/watch?v=dAoOYFx7ZYw) · [Code mode: Why your agent should code](https://arize.com/blog/code-mode/)（Mikyo King）· [Arize Phoenix has a built-in MCP server that lets your agents query traces with SQL](https://arize.com/blog/phoenix-mcp-sql-code-mode/) · [PXI documentation](https://arize.com/docs/phoenix/pxi) · [Harbor](https://github.com/harbor-framework/harbor) · [Is Bash All You Need?](https://arxiv.org/abs/2609.11999)

本文依据官方视频字幕、作者的现场录音、幻灯片照片（标题页没有拍到）以及上面列出的资料。talk 结束时没有 Q&A。[精确笔记](../precise/10-14-1545-code-mode-your-agent-should-write-code.md)（中文）保留了时间戳和完整的转写更正列表。
