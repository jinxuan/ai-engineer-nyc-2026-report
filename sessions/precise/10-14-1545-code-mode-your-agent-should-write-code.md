# Code Mode: your agent should write code：精确版

> Precise notes for this session, compiled from the official video captions where available, the author's room recording, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材整理完，视频和现场录音逐段对照完，摘要里的论文和 Harbor 核对过。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | 作者拍的 13 张（IMG_0743–0755），全部属于这场。标题页没拍到。数字从照片上读 |
| 视频 | [dAoOYFx7ZYw](https://www.youtube.com/watch?v=dAoOYFx7ZYw)，改名「Stop Giving Your Agent Tools. Give It a Sandbox - Mikyo King, Arize AI」，14:38，上传 2026-10-27。字幕 `subs/dAoOYFx7ZYw.txt`。**和现场是同一场**：措辞逐句一致，视频时间 ≈ 录音时间 + 53～59 秒 |
| 一手资料 | 讲者博客 [Code mode: Why your agent should code](https://arize.com/blog/code-mode/)（Mikyo King，2026-09）；[Arize Phoenix has a built-in MCP server that lets your agents query traces with SQL](https://arize.com/blog/phoenix-mcp-sql-code-mode/)（Nancy Chauhan、Roger Yang、Mora Vigo Malusardi，2026-08）；论文 [Is Bash All You Need?](https://arxiv.org/abs/2609.11999)（arXiv:2609.11999）；PXI 文档 [arize.com/docs/phoenix/pxi](https://arize.com/docs/phoenix/pxi)；Harbor（[GitHub harbor-framework/harbor](https://github.com/harbor-framework/harbor)，经搜索摘要） |

## 基本信息

- 时间地点：2026-10-14 3:45–4:03pm，Turing Room (Riverside Ballroom)，Breakouts，Evaluations Track（资料：日程）
- 原题「Code Mode: your agent should write code」，视频改名「Stop Giving Your Agent Tools. Give It a Sandbox」
- 讲者：Mikyo King，Head of Open Source，Arize AI。讲者说（视频 00:12）：「I head up the open source group at Arize AI」，做「a fully open source observability and evaluation framework」（Phoenix），也做自己的 agent（字幕把名字写成 Mikail、公司写成 Arise）
- 官方简介要点：「Give an agent 40 tools and it gets worse at using any of them」；「Recent research has bash-only agents beating typed tool catalogs by 21.8 to 24.5 points on the same benchmark while spending fewer tokens」；结尾讲「how our open-source observability stack exposes agent traces to coding agents through read-only SQL and code mode」
- **简介里的 21.8–24.5 那句研究，两版讲稿里都没有出现**，拍到的 slide 里也没有（见出入）

## 1. 开场：程序员和非程序员之间的边界在消失

- 讲者说（视频 00:36）：过去 30 年软件分两类，给工程师的和给非工程师的。例子：从邮箱里拿出所有发票，程序员写个约 50 行的脚本；非技术人员只能手工下载，「restricted by the interface of the software」，还得买软件
- 讲者说（视频 01:18）：「the thing that's really interesting about things like Claude Code is that that boundary is completely dissolving」；labs 知道这一点，「If you look at all the benchmarks like SWE-bench, that is what these models are being trained to do」；所以你做的 agent 也应该能动态地造出所需的软件
- 视频章节名：「The line between engineers and everyone else is dissolving」

## 2. 传统 tool calling 的问题

- slide「The too many tools problem」：左边 CONTEXT WINDOW 堆叠条，图例 Tool definitions（占大部分）/ User question / Tool call / Tool result；中间 Model（「tool call 3」）；右边 MCP SERVERS：CRM、Meetings、Email、Calendar、Docs、Tasks
- 讲者说（视频 01:59）：给 agent 一大份 tool catalog，每次调用的结果推进 context window，再推理、再调用；工具不多时这样「can work pretty well」。但现在要更多 emergent behavior、更多 context、更长 horizon，于是 context bloat：无关的工具定义、返回垃圾的工具、需要翻页的数据，「you deal with this kind of context rot … multi-turn, a lot of turns … compaction」

## 3. Code mode 是什么

- 讲者说（视频 03:08）：另一种架构：不给一整份工具菜单，而是给「the ability to search for the specific tool」和执行能力，而且执行是在「a programming environment」里，可以顺序、并行、加条件；「you can call multiple tools in one turn, and also return only what's necessary to push back into the context window」
- slide「The model programs the tools」：USER「Attach today's Acme call to the Acme deal in our CRM.」→ Model 写 program → SANDBOX：

```python
call = find_meeting("Acme", "today")
text = get_transcript(call.id)
deal = find_deal("Acme")
attach(deal.id, text)
return "attached"
```

  右边 TOOLS：`find_meeting()`、`get_transcript()`、`find_deal()`、`attach()`，以及没用到的 `search_email()`
- slide「Tool calling vs. code mode」（示意对比）：Tool calling「one call at a time」：**8 turns · 59% context used · $0.59 cost**；Code mode「Sandbox · one program」：**1 turn · 2% context used · $0.02 cost**
- 讲者说（视频 03:49）：把两者「hypotheticals」并排：tool calling 每次调用都在积累 context，增加 latency、cost 和轮数，「the time your user takes to get to value increases」；code mode「when done well」一轮里调多少工具都行，只把需要的放回 context
- **注意**：讲者自己说这是「hypotheticals」，slide 上 8 turns / 59% / $0.59 vs 1 / 2% / $0.02 是示意数字，不是测量结果（讲者说，视频 03:50）

## 4. Code mode 为什么有效：四个理由

- slide「Why code mode works」四格：
  1. **Progressive disclosure**：「every tool」一大格 → context 里只有两个
  2. **Composability**：`for m in meetings: t = get_transcript(m); if "pricing" in t: attach_to_lead(m, summarize(t))`，旁注 loop / call / branch / chain
  3. **Token efficiency**：「sandbox · transcripts」→ context 里只有「answer」
  4. **Emergent behavior**：`# written by the model` `def redact_pii(t): return CARD.sub("-", t)`，工具栏 `get_transcript`、`summarize` 后多了 `+ redact_pii`
- 讲者说（视频 04:41）：
  1. 不用一开始就给整份 catalog，「there's a progressive disclosure of tools」；foundation model 厂商现在也内置了 tool search
  2. composability：顺序、并行、fan out、chaining；「If you've heard of obviously Claude's dynamic workflows is built on this composability but composability of sub agents」
  3. token efficiency：不用把工具输出全塞进 context，模型可以「trim and search」只放回需要的
  4. emergent behavior，「one of the major reasons we've embraced code mode」：比如「take all these records and remove personally identifiable information」，tool calling 没有这个工具，或者在 context 里做，「you're leaking credit card numbers in the context window」
- 资料：讲者博客也用了脱敏信用卡号的例子（正则匹配 13–16 位数字），并链接了 Claude Code 的 dynamic workflows 文档 code.claude.com/docs/en/workflows（[Arize 博客](https://arize.com/blog/code-mode/)）

## 5. Arize 的实践：Phoenix 里的 PXI

- 资料：PXI（Phoenix Intelligence，读作 pixie）是 Arize 开源 Phoenix 里内置的 AI engineering agent，「modeled after a coding agent, but pointed at Phoenix」，arize-phoenix 17.0.0+ beta，带一个 sandboxed bash 工具（[PXI 文档](https://arize.com/docs/phoenix/pxi)，经搜索摘要；[Meet PXI](https://arize.com/blog/meet-pxi/) 没打开）
- slide「Code mode in PXI」：PXI →（code）→ PHOENIX SERVER，三种执行面：
  - **bash**：`gql … > spans.json; grep -c ERROR spans.json` ← GraphQL API
  - **execute**：`bad = [r for r in runs if r["error"]][:3]` ← REST API
  - **executeSql**：`SELECT name, count(*) FROM spans GROUP BY 1` ← Database
- 讲者说（视频 06:14）：「we build our own agent and harness inside of Phoenix … and we started with Bash」，用 sed、grep 这些 Linux primitive 加自家 CLI，结果落到文件里；然后加了 Python sandbox，「hoisted all our REST CRUD endpoints」（拿 traces、改 datasets）进编程环境；再加了写 SQL 的能力：不是所有事都是增删改查，比如「tell me all the LLM spans that have P95 latency of X, Y, and Z」，不想翻页翻完所有 traces
- 资料（Phoenix SQL 博客）：两个只读工具 `describeSqlSchema`、`executeSql`；code mode 下 agent 写的 Python 在服务端受限的 Monty sandbox 里跑，只把最终返回值给模型；SQL 会被解析、限定在允许的表和列、重新生成，再由数据库校验（SQLite authorizer 或 Postgres query plan 检查）；有运行时长、行数、结果大小上限。基准：8 个 telemetry 问题 × 3 次，SQL + code mode 平均约 $0.23 / 题，retrieval-only 工具约 $3.97，「about one-seventeenth the cost」（[Arize 博客](https://arize.com/blog/phoenix-mcp-sql-code-mode/)）
- slide「Code mode in the browser」：标签「driven by PXI」，Phoenix 里 Projects › claude-code 的 Spans 页（Sep 22，82 spans，过滤 `parent_id is None`）；右边「PXI in Phoenix」：用户问「Show me yesterday's failed tool calls」→ `search_browser_actions ✓` → `execute_browser_action`（WEB WORKER）：`await ui.timeRange.set(yesterday)`、`await ui.spansFilter.set("span_kind == 'TOOL' and status_code == 'ERROR'")`。底部三步：1 Writes one `ui.*` script → 2 Runs in a Web Worker → 3 Drives the real app
- 讲者说（视频 07:39）：在 UI 上也做了一套「programming paradigm … bound to our UI」，能改时间范围、读前端数据；放在「a web worker that has a two-way data binding directly into our browser」，得到类似 computer use 的 emergent behavior

## 6. Code mode 的代价

- slide「LLM-generated code」四格：**Resource limits**（cpu、memory、time 条，time 超线「killed」）；**Network egress**（sandbox 里「injected README」把 data 发到 evil.com）；**Secrets in the sandbox**（`API_KEY=sk-ant…` + `print(env)` → logs 里出现 `sk-ant…`）；**Over-broad permissions**（sandbox 里一个 admin token，`read logs ✓` 但也 `delete prod ✓`）
- 讲者说（视频 08:19）：「code mode is not obviously for free」：
  - 资源：用托管 sandbox 要管 CPU、内存、sandbox 存活多久
  - 网络：「Networking is a huge one … you never know what's out in the web」，要想清楚 network egress
  - secrets：「every sandbox provider is trying to come up with a different vaulting strategy」；举了一个在 sandbox 里用某种东西做快速决策的新例子（字幕「use Jev inside of your sandbox like Pi uh durable just shipped」，听不清，【待确认】），关键是「make sure that those secrets don't come back into the context window」
  - 权限：「typically permissions are overbroad」，前五步对，第六步不对；或单步对、组合起来不对，必要时加 guardrail
- slide「Operational complexity」四格：**Startup cost**（isolate：ms；microVM：100s of ms；full VM：seconds）；**Harder to debug**（tool calling 三步 `get_spans() ✓ filter() ✓ summarize() ✓` 对比 code mode 一个 `run_code()` 得到「summary: empty」）；**Tool discovery**（search `search("traces")` → learn `get_traces(project, limit)` → call `get_traces("prod")`）；**Duplicating the harness**（harness 里有 tool search + code exec，MCP server 里又有一套 tool search + code exec）
- 讲者说（视频 09:35）：sandbox「don't boot all the time perfectly」；更难 debug，「now you're debugging code and you're not debugging individual tools」；「tool parameter accuracy becomes a much bigger issue because the tool parameter now is code」；tool discovery 要多一次握手；**double sandboxing**：code mode 被塞进 MCP，也被塞进 harness，结果被序列化成 JSON、解析后又进另一个 code mode，「double duty」

## 7. 开放问题

- slide「Open problems」四格：**Durability**（today 一排块里有一块红；snapshot 一排全绿）；**Determinism**（JavaScript `a3f ≠ 9c1`，Starlark `a3f = a3f`）；**Images and binary data**（tool returns `{json}` → `r.traces ✓`；image → `"iVBORw0KGgoAAAA…"`；2 GB file → `× too large to pass`）；**Smaller models**（large model ✓，small model ✗）
- 讲者说（视频 10:42）：「They're not like blockers」：
  - durability：一大段代码中途挂了，「You can't resume from that step」
  - determinism 和数学：要考虑用哪种语言，「JavaScript is something LLMs are really good at doing, but maybe not great for math」
  - 多模态 / 二进制：在 sandbox 里处理图片很好，但拿出来难，不想把整张图序列化进 context，「you might have to do some sort of signed image handoff」
  - 小模型：「the Frontier models are amazing at code mode」，换小模型省钱时能力可能跟不上

## 8. 怎么评估：Harbor 和结果

- 讲者说（视频 11:44）：dogfood 自家的 Arize Phoenix；用 **Harbor framework** 把 agent 放进 sandbox 环境、「clear instructions with network access turned off」，多次 trial 去掉 flake；eval 写成「Harbor verifiers」，「Harbor's sort of rooted in this RL framework. We give it a one or a zero」；跟踪每个任务的 **pass@k**
- 资料：Harbor 是 Terminal-Bench 团队发布的 agent eval 框架，Terminal-Bench 2.0 的官方 harness；每个 trial 起一个 sandbox，跑 agent loop，再由 verifier 产出 reward 和日志；可接 Daytona、Modal 等 sandbox；也用于生成 RL rollouts（经搜索摘要，[GitHub harbor-framework/harbor](https://github.com/harbor-framework/harbor)、[Terminal-Bench 论文](https://arxiv.org/pdf/2601.11868)，原文未逐字核对）
- slide「What code mode saves PXI」（Phoenix 实验对比截图，右栏数字）：**Tokens −43% · Cost −31% · Reward Same · Latency +0.6%**
- 讲者说（视频 12:53）：「pre code mode and post code mode, we, you know, decreased the amount of tokens by 43%. The cost went down by 31%」，sandbox 有一些开销，「but the latency stayed pretty constant」；覆盖「whether or not it was Claude Code or MCP or the CLI」
- slide「Improve the agent」：Run the evals → Spot regressions → Analyze the traces → Tune the agent，回环「re-run the same tasks」；截图：一次 trace Status OK、Total Cost $0.44、Latency 2m 52s，模型 claude-opus-5，单次调用 26,665 prompt → 297 completion tokens，工具 `mcp__phoenix__execute`；右边一张「Code mode, before + after (18 tasks, 2 runs after the change)」的表，比较几组实验的 Reward / Turns / Prompt tokens（数字在照片里看不清，【待确认】）。底注「Look at the data」「Tune the tools and instructions」
- 讲者说（视频 13:23）：同时有 trajectory 和 eval 是「a huge unlock」，可以告诉 coding agent 怎么 hill climb：比如第一次加一个新工具时效率反而下降，就能告诉 coding agent 是指令里哪部分把它带偏了

## 9. 结论

- slide「The case for code mode」：Fewer turns · Cheaper · Smarter；「Sandbox it. Trace it. Improve it.」
- 讲者说（视频 13:58）：「We really like code mode at Arize because of the fewer turns」；「We want you to be able to bring any model … we want it to be cheaper」；对 emergent behavior 和 long-horizon task 很兴奋；「I hope this talk convinces you to give your agent a sandbox and then use one of our tools to evaluate it.」

## 简介里的研究：Is Bash All You Need?

讲者在台上没有提，但官方简介引用了，prep 也列了，这里核对原文：

- 资料：「Is Bash All You Need? An Empirical Study of Tool Interfaces for Enterprise Digital Worker Agents」，Hazel Mak、Susheel Suresh、Sahil Bhatnagar、Barry Wang、Chhaya Methani、Alejandro Gutierrez Munoz，arXiv:2609.11999（cs.SE），2026-09-10 提交（[arXiv](https://arxiv.org/abs/2609.11999)；arXiv 页面没列机构，【待确认】）
- 比较五种工具接口：typed tools、typed tools + bash、bash only、bash + agent 自己合成并保留的工具、programmatic tool calling (PTC)；模型 Opus-4.8 和 GPT-5.5；两个企业基准 **TheAgentCompany** 和 **APEX-Agents**
- **21.8–24.5 个百分点是 TheAgentCompany 上 bash only 相对 typed tools 的提升**；在 APEX-Agents 上只有 4.8–7.4 个百分点
- bash only 总 token 少 19–72%；在 bash 上再加 typed tools 或持久化合成工具，没有可检测的合并得分提升；PTC 比直接 typed 调用省 token、表现相近，但质量和成本都不如 bash only
- 论文的建议：能隔离任意执行时用 bash only；安全或合规要求固定 tool catalog 时用 PTC

## Q&A 精确版

没有 Q&A。录音在「that was it, thank you」后约 20 秒结束（录音 13:13 起），视频在 14:29「Thank you」处结束。

## 勘误表（Whisper / YouTube 字幕 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| Mikail（视频字幕） | Mikyo | 日程 |
| Arise / RI | Arize | 日程、slide |
| Codemob / Codemone / Code mod / CodeMon / Codewelt | code mode | slide |
| cloud code / Cloud Code | Claude Code | 上下文 |
| glow inside of the context / context window blow | bloat | 视频字幕 |
| conduct window / content | context window | 视频字幕 |
| interaction（录音 01:40） | compaction | 视频字幕 |
| tools are fun | tools up front | 视频字幕 |
| concordability | composability | 视频字幕 |
| coaching efficiency | token efficiency | 视频字幕 |
| refs, HUD | REST CRUD endpoints | 视频字幕 |
| alarm scans / LM spans | LLM spans | 上下文 |
| patinate | paginate | 视频字幕 |
| LMS code | LLM-generated code | slide「LLM-generated code」 |
| bolting strategy | vaulting strategy | 视频字幕 |
| JEB / Jev，Pi on durable does shit / Pi uh durable just shipped | 【待确认】 | 两版转写都听不清 |
| drive railing | guard railing | 视频字幕 |
| don't move all the time | don't boot all the time | 视频字幕 |
| injected into RCP | injected into MCP | 视频字幕 |
| Your ability is a problem | durability is a problem | 视频字幕、slide |
| giant piece of power that you can break from the field | giant piece of code that … breaks in the middle | 视频字幕 |
| sign and make a handoff | signed image handoff | 视频字幕 |
| front-chair models | frontier models | 视频字幕 |
| cost setting … swallow more files | cost saving … swap out a smaller model | 视频字幕 |
| hardware framework / hardware verifiers | Harbor framework / Harbor verifiers | 视频字幕、YouTube 描述 |
| pass a k | pass@k | 上下文 |
| cost went down by 39%（录音） | 31% | slide、视频字幕、YouTube 描述 |
| emails（录音 12:33） | evals | 视频字幕 |
| give your education a sandbox | give your agent a sandbox | 视频字幕 |

## 出入

1. **成本降幅**：录音转写是「39%」，视频字幕、slide（Cost −31%）和 YouTube 描述都是 31%。以 31% 为准，录音是 ASR 错误
2. **简介里的 21.8–24.5 研究没进讲稿**：简介说「bash-only agents beating typed tool catalogs by 21.8 to 24.5 points on the same benchmark」，台上两个版本都没讲。论文原文里这个区间只在 TheAgentCompany 上成立，另一个基准 APEX-Agents 是 4.8–7.4 点；论文比较的是 bash-only vs typed tools，**不是**「code mode（search + execute 工具的编程环境）vs tool calling」。讲者的 Phoenix 实验报告的是 token 和成本，reward「Same」，没有分数提升
3. **8 turns / $0.59 vs 1 turn / $0.02**：slide 上很醒目，但讲者口头说是「hypotheticals」；实测是 tokens −43%、cost −31%
4. **latency**：slide 是 +0.6%，口头是「stayed pretty constant」
5. **两篇 Arize 博客的数字不同**：讲者 9 月的 code mode 博客没有 43% / 31% 的数字；8 月的 Phoenix SQL 博客是另一组实验（8 个问题，$0.23 vs $3.97，约 1/17 成本）。台上的 43% / 31% 目前只有 slide 和视频为来源

## 【待确认】清单

1. secrets 那段「use Jev inside of your sandbox like Pi … durable just shipped」原话和所指产品
2. 「Improve the agent」截图里 18 个任务的前后对比表数字（照片看不清）
3. 43% / 31% 实验的任务数和设置（只有 slide 和口头，没找到书面来源）
4. 「Is Bash All You Need?」作者机构
5. Harbor 的细节（来自搜索摘要和镜像文档，没打开官方 repo）
6. 「Meet PXI」博客原文（只看到搜索摘要）
