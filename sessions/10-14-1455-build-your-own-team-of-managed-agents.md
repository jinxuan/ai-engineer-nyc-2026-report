# Build your own team of managed agents：精确版

> Precise notes for this session, compiled from the official video captions where available, the author's room recording, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材整理完，视频和现场录音逐段对照完，产品名对过 Google 文档和博客。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | 作者拍的 20 张（IMG_0722–0741），全部属于这场。slide 是网页式 deck，右下角有页码，拍到 2、3、5、6、7、8、9、10（两张）、11、12、13、14、15、16 /19 页，以及 demo 画面；**第 1、4、17–19 页没拍到**。数字从照片上读 |
| 视频 | [Y_p3y6zP5QE](https://www.youtube.com/watch?v=Y_p3y6zP5QE)「Build Your Own Team of Managed Agents - Jack Wotherspoon, Google DeepMind」，19:43，上传 2026-10-26。字幕 `subs/Y_p3y6zP5QE.txt`。**和现场是同一场**：措辞逐句一致，视频时间 ≈ 录音时间 + 62 秒 |
| 一手资料 | Google 官方博客：[Introducing Managed Agents in the Gemini API](https://blog.google/innovation-and-ai/technology/developers-tools/managed-agents-gemini-api)、[Interactions API: our primary interface for Gemini models and agents](https://blog.google/innovation-and-ai/technology/developers-tools/interactions-api-general-availability)、[Expanding Managed Agents in Gemini API](https://blog.google/innovation-and-ai/technology/developers-tools/expanding-managed-agents-gemini-api/)；文档 [Antigravity agent](https://ai.google.dev/gemini-api/docs/antigravity-agent)（已读）。博客正文没打开，日期来自搜索结果摘要 |

## 基本信息

- 时间地点：2026-10-14 2:55–3:13pm，Shannon Room (NY Ballroom E)，Expo Session (18 mins)（资料：日程）。Expo session 一般是赞助商场次（我的理解）
- 讲者：Jack Wotherspoon，日程写「Member of Technical Staff, Google DeepMind」；讲者说（视频 00:12）：「I am a DevEx engineer at Google DeepMind」
- 官方简介：「Managed agents give you a cloud sandbox, a file system, and a full agent loop in a single API call」；「Let's put it to the test by building a full team of financial agents in just a few minutes!」
- 产品官方名称：Google 博客写「**Managed Agents in the Gemini API**」；讲者和 slide 用「Gemini Managed Agents」。文档里的预置 agent 叫 **Antigravity agent**，「a general-purpose managed agent on the Gemini API」，agent ID `antigravity-preview-09-2026`（资料：[Antigravity agent 文档](https://ai.google.dev/gemini-api/docs/antigravity-agent)）
- 资料（时间线，来自 Google 博客的搜索摘要）：Managed Agents 2026-05-19 预览发布；Interactions API 2026-06-23 GA；2026-07-07 加入 background execution、remote MCP、custom function calling、credential refresh；之后还有一篇加 3.6 Flash 和 hooks 的更新。日期【待确认】（没打开博客正文）

## 1. 为什么要 cloud / managed agent

- 讲者说（视频 00:34）：cloud 或 managed agent「instead of running a harness and running coding agent on your own machine, you can actually just run them in the cloud on a sandbox」
- slide（2/19，「01 — Why a Computer Matters」）「With a sandbox, agents verify their own work.」：「Instead of handing you untested code, an agent with its own computer runs the script, inspects the traceback, fixes the bug, and returns the verified output.」四步：Step 01 Write code（`write dcf.py`）→ Step 02 Execute code（`python3 dcf.py`）→ Step 03 Test and validate（「Spots a missing column or NaN row, patches the code, and re-runs」，`fix KeyError → retry`）→ Step 04 Deliver artifact（`chart.png ✓`）。底栏「Autonomous verification loop runs inside the cloud sandbox · Zero human prompts required」
- 讲者说（视频 01:16）：在自己笔记本上跑验证，「sometimes it steals the browser … You start getting pop-ups」
- slide（3/19，「02 — Local vs. Cloud」）「Your laptop doesn't scale」：「Running agents locally forces you to choose between approving every shell command by hand or exposing your personal machine.」On your laptop「Blocked on permission prompts」（`Allow bash execution & network access? [y/N]`）：Shares your personal files, SSH keys, and active sessions / Stops the moment you close your laptop lid / Can't scale to thousands of customer sessions。In a cloud sandbox「Runs & verifies autonomously」：Isolated Linux container with zero local blast radius / Runs asynchronously in the background / Scales from your daily workflow to thousands of users
- 讲者说（视频 01:32）：「your laptop doesn't scale」：自己调好的 skills 和 coding agent 怎么分享给同事、怎么集成进 app；「You don't want your app that you're deploying to customers to be running with your own kind of subscription」
- 讲者说（视频 02:12）：自己搭要起容器或 VM、自己做 harness engineering，「let's not even start to talk about networking or setting up firewall rules」

## 2. 一次 API 调用：Interactions API

- slide（5/19，「04 — Gemini Managed Agents」）「Now it's a single API call.」：「The Interactions API gives you a Google-hosted Linux sandbox, a persistent filesystem, and the Antigravity agent loop out of the box.」代码：

```python
from google import genai
client = genai.Client()
interaction = client.interactions.create(
    agent="antigravity-preview-09-2026",
    environment="remote",
    input="Pull Q3 earnings, model cash flow in Python, "
          "and export a chart to valuation.png.",
)
print(interaction.output_text)
```

- 讲者说（视频 02:39）：「whether it's Gemini managed agents, Claude managed agents, we'll be talking about Gemini managed agents today, which is our offering in the Gemini API at DeepMind」（字幕作 cloud managed agents，按上下文应为 Claude）；「it boils down to just being a single API call」
- 讲者说（视频 03:28）：Interactions API 是「our kind of new agent first version of our SDKs and client libraries」；以前 model 是一进一出，不适合 agent；interaction 可以多步、写文件、调 MCP tools；「this new interactions API that we launched and went GA earlier this year, is now our kind of home for managed agents」；把 model 换成 agent，设 `environment="remote"`
- 资料：文档中 `environment` 有三种写法：`"remote"`（新建默认 sandbox）、已有的 environment ID（复用文件和状态）、完整 `EnvironmentConfig` 对象（自定义 sources 和网络规则）

## 3. 架构和五个 building blocks

- slide（6/19，「05 — Architecture Overview」）「Gemini Managed Agents」：USER「Analyze Q3 filings and create a valuation report…」→ GEMINI API（code_execution、filesystem、Skills、MCP tools；模型 GEMINI 4 ARGON）→ EXECUTE / RESULTS ↔ SANDBOX（ONLINE）：ROOTFS 里有 `AGENTS.md`、`skills/`、`mcp_tools`；TERMINAL「Isolated Linux VM · 4 vCPU · 16 GB」「Egress proxy injects credentials」；底栏「Debian · 4 vCPU · 16 GB VM · Zero-Trust Egress」。上方四个输入源：GCS、REPOSITORY、INLINE、CREDENTIALS
- 讲者说（视频 04:32）：Gemini API 有内置的 server-side tools（Google Search、读写文件的 filesystem 工具），也可以加 custom functions 或 MCP tools；sandbox「running on kind of Google Cloud infrastructure」；可以上传 agent skills、agents.md，启动时加载 GitHub repo
- slide（7/19，「06 — Core Primitives」）「Building blocks of managed agents」：Harness（`agent_config`）「Plans the work, calls tools, and checks its own results.」；Environment（`environment`）「Its own Linux computer with files and packages that persist.」；Tools（`tools + mcp`）「Built-in bash, files, web search, and remote MCP servers.」；Skills（`.agents/`）「Instructions in AGENTS.md and repeatable workflows in SKILL.md.」；Credentials（`credentials`）「API keys injected outside the sandbox so secrets stay safe.」；Schedule（`triggers`）「Cron schedules and background runs that work while you're away.」
- 讲者说（视频 05:29）：harness 是 Antigravity harness，「the exact same harness that's running in our anti-gravity suite of products over at Google」；environment「is kind of its own entity」，多个 agent 可以指向同一个 sandbox；skills 就像公司里的 playbooks；credentials「is a new piece of managed agent which essentially lets us inject either kind of tokens or your MCP OAuth into the managed agent sandbox」；triggers：比如每天早上 8:30 开盘前做财务分析，结果可以通过 webhook 发 Slack，或查团队的 Jira board
- 资料：文档说 Antigravity agent「uses the same harness as the Antigravity IDE」；内置工具 `code_execution`、`google_search`、`url_context` 默认开启，设了 environment 后自动加 filesystem 工具；remote MCP 走 streamable HTTP；可以把 `AGENTS.md` 和 `.agents/skills/` 挂进 sandbox。文档没有给出 vCPU、内存、OS（指向另一个 Environments 页面，那个 URL 返回 404），所以 slide 上的「Debian · 4 vCPU · 16 GB」**未核对**，【待确认】
- 讲者口头说「five」个 pillar（视频 05:22 章节名「harness, environment, skills, credentials, triggers」），slide 上是六块（多了 Tools）

## 4. 模型：Gemini 4 Argon 和 Flash

- slide（8/19，「Harness」）「The Antigravity harness runs the loop.」：「Built on the same harness as Antigravity products. It plans multi-step work, executes tools, verifies its own output, and lets you configure the underlying model.」两个模型：`gemini-4-argon`「Deep reasoning for complex coding and financial modeling.」；`gemini-3.8-flash`「Fast, low-latency default for everyday agent workflows.」代码里 `agent_config={"type": "antigravity", "model": "gemini-4-argon"}`，input「Audit all 10-K footnotes and reconcile segment revenue across 5 years.」
- slide（9/19）「Gemini 4 Argon: built for your hardest tasks」：「Now available in the Gemini API and Google AI Studio — purpose-built for deep coding, financial analysis, and multi-step agentic work.」三点：1「#1 on the Vals Index — Leads frontier models across finance, coding, legal, and tax work, lifting agentic code submissions at Google by 35%.」2「Long decode up to 1M tokens — Shared 1M context window (input + thinking + output) with tunable thinking_level (low, medium, high).」3「Pair with 3.8 Flash for cost and speed — Route your hardest reasoning to Argon and fast everyday turns to Gemini 3.8 Flash.」
- 讲者说（视频 07:10）：「Yesterday we just launched Gemini 4 Argon which is our new frontier model」；快和省用 Gemini Flash，深度推理用 Argon；「It's especially good at financial tasks」，也擅长跨大量文档做数据分析，以及 multimodal
- 讲者说（视频 08:10）：long decode：「normally models have an output token max of around 64K tokens」，Gemini 4 让 1M 的窗口「shared between input, thinking, and output」，适合极长的数学推理或财务建模输出
- 资料：文档里 `agent_config.model` 可选 `gemini-3.8-flash`（**默认**）、`gemini-4-argon`（complex software engineering and long-horizon tasks）、`gemini-3.6-flash`、`gemini-3.5-flash-lite`；managed agent 的模型在创建时固定，「cannot override the model at interaction time」
- 资料（二手）：Gemini 4 Argon 2026-09-30 公布（先给安全研究者）、**2026-10-13 广泛开放**，和讲者「yesterday」（10/14 讲）一致；Vals Index 第一，68.90%（[techjacksolutions](https://techjacksolutions.com/?p=96279)、[DeepLearning.AI The Batch](https://www.deeplearning.ai/the-batch/argon-a-low-hallucination-model-build-for-knowledge-work)）。Google 原文（blog.google「introducing-gemini-4-argon」）没打开，【待确认】。slide 上「lifting agentic code submissions at Google by 35%」没找到来源，【待确认】

## 5. Interactions 和 Environments 分开，多个 agent 共用一个 sandbox

- slide（10/19，「Environment」）「Interactions and Environments are stateful」：「Conversation history and filesystem are both kept in state server-side separately. One agent can prepare files in the environment and a second agent can step right into that same computer with a fresh conversation.」Interactions（`previous_interaction_id`）「Keeps the conversation messages and reasoning turns.」；Environments（`environment`）「Keeps /workspace files, data, and installed packages.」代码：

```python
# 1. Researcher saves data to /workspace
step1 = client.interactions.create(
    agent="market-researcher", environment="remote",
    input="Save Q3 filings to /workspace/q3.csv",
)
# 2. Quant reuses the same computer with a fresh chat
step2 = client.interactions.create(
    agent="quant-analyst", environment=step1.environment_id,
    input="Plot a valuation chart from /workspace/q3.csv",
)
```

- 讲者说（视频 09:05）：「these are kind of two different stateful things on our back end」；interaction 管对话，把一个 interaction 的 ID 传给下一个就是第二轮；environment 是 sandbox 上的文件系统，可以用 API 上传下载文件。分开后，第二个 agent「has a very fresh kind of context window and conversation, but it's reusing the environment」，「Kind of a multi-agent workflow」

## 6. 工具、playbook 和保存 agent

- slide（11/19，「Tools」）「Built-in sandbox tools plus remote MCP.」：「Every managed agent starts with shell, filesystem, and web grounding enabled—and connects to remote HTTP MCP servers in a few lines.」code_execution「Run Bash, Python, and Node with stdout/stderr capture.」；filesystem「Read, write, edit, and search files across /workspace.」；google_search「Live web search and url_context page fetching.」；mcp_server「Attach external Model Context Protocol servers over HTTP.」代码示例接 GitHub MCP（`"url": "https://api.githubcopilot.com/mcp"`，`"credential": "github-prod"`）
- slide（12/19，「Skills」）「Configure agents with your playbooks」：「Mount AGENTS.md, SKILL.md, Git repos, or Cloud Storage buckets directly into the sandbox — then promote your setup to a saved agent.」目录：`.agents/AGENTS.md`（Persona & rules）、`.agents/hooks.json`（Guardrails）、`.agents/skills/dcf-model/SKILL.md`、`/workspace/filings/`（Mounted from Git or GCS）。三步：1 Customize inline → 2 Save agent → 3 Invoke by ID；代码里 sources 有 `"type": "inline"`（target `.agents/AGENTS.md`，content「You are a rigorous equity analyst.」）和 `"type": "repository"`（`https://github.com/org/fin-skills` → `.agents/skills`）
- 讲者说（视频 10:46）：内置 code execution、读写文件、Google Search；也支持 remote MCP servers（字幕作「SMTP servers」）
- 讲者说（视频 11:40）：示例上传 agents.md、连一个 GitHub repo 让 agent 查 bug；agents API 可以 `agents.create`，调好的 agent 存下来，之后「invoke your personal agent by calling it with its agent ID」
- 资料：文档：managed agent 由 `agents.create` 创建，参数 `base_agent` 和 `agent_config`

## 7. 安全：secrets 不进 sandbox

- slide（13/19，「Credentials」）「Secrets never enter the sandbox.」：「Store tokens and credentials once via client.credentials.create(). When the agent calls an allowlisted domain, the egress proxy injects the secret on the wire.」图：Container「Agent Sandbox — Calls api.polygon.io or github.com with zero tokens in memory or env.」→（No secret）→ Wire Injection「Egress Proxy — Verifies network.allowlist, attaches bearer_token or refreshes oauth2.」→（+ Bearer ····）→ Upstream「External API — Receives authenticated request; sandbox can never exfiltrate the key.」代码 `client.credentials.create(id="polygon-prod", type="bearer_token", token="pk_live_...")`，environment 里 `"network": {"allowlist": [{"domain": "api.polygon.io", "credential": "polygon-prod"}]}`
- 讲者说（视频 12:21）：「secrets never actually enter the sandbox」；proxy 在 sandbox 和 API 之间，往 sandbox 里放一个 placeholder「dummy token」，agent 不会「go rogue and say, oh, I see that I have this GitHub token laying around」；调 GitHub API 时 proxy 匹配域名再注入真 token
- 资料：文档：trigger 触发的运行应引用已存的 credential，「the egress proxy resolves on each run」；也支持 inline `transform` 规则
- 出入：讲者说 sandbox 里放的是 placeholder / dummy token；slide 写「zero tokens in memory or env」。两种说法不完全一样（我的理解：可能是 sandbox 里有占位符但没有真值），【待确认】

## 8. 定时触发和后台扇出

- slide（14/19，「Schedule」）「Run on a schedule or fan out in the background.」：「Schedule recurring jobs with client.triggers.create() or launch non-blocking parallel runs with background=True.」代码一：`client.triggers.create(display_name="pre-market-briefing", schedule="0 7 * * 1-5", time_zone="America/New_York", interaction={"agent": "quant-analyst", "input": "Refresh morning risk report.", "environment": shared_env_id})`；代码二：`client.interactions.create(agent="quant-analyst", environment="remote", input="Run 10,000 Monte Carlo paths on NVDA.", background=True)`，然后 `client.interactions.get(id=job.id)` 轮询
- 讲者说（视频 13:12）：每天早上的 briefing、扫邮件建议回哪些、开盘前看股市，「a way to run one of your agents on a set schedule」
- 资料：文档：trigger 用 cron 和 IANA 时区，每次运行复用同一个 environment；状态 active / paused / disabled；默认 `max_consecutive_failures` 5、`execution_timeout_seconds` 600
- 出入：讲者口头举例「8:30 a.m. every morning before the market opens」；slide 代码的 cron 是 `0 7 * * 1-5`（工作日 7:00）
- slide（15/19，「07 — Team Architecture」）「One prompt, three specialized financial sandboxes.」：「Each custom agent is saved with its own AGENTS.md persona, SKILL.md workflows, and pre-installed Python packages.」Coordinator「Portfolio Lead — Splits ticker analysis across three specialized managed agents in parallel.」→ `sec-filing-researcher`「Extracts 10-K & 10-Q segment tables」→ `segments.json`；`quant-dcf-modeler`「Runs Monte Carlo & sensitivity in Python」→ `valuation.png`；`exec-report-writer`「Synthesizes thesis & risk memo」→ `memo.pdf` → Files API「Verified Briefing — Audited numbers, charts, and PDF ready for decision-makers.」
- 讲者说（视频 13:37）：managed agent 本身就是 API 调用，所以一个 agent 可以调用别的 managed agent、fan out；拆成「niche and specific targeted agents that have one core purpose」，比给一个 agent 一个很宽的任务更可控

## 9. Demo

**Demo 1：SEC 研究 → quant → 报告**（slide 16/19「08 — Live Build / Let's put it to the test.」，「Re-use environments to allow agents to pick up where others left off.」，Shared Sandbox `ec57bb717224412b2157ce4cd675352b`，Ticker NVDA）

- 三步：Step 01 Research `sec-filing-researcher`「Pulls quarterly revenue & segments and writes /workspace/metrics.json」；Step 02 Quant Model `quant-dcf-modeler`「Reuses environment_id, runs Python DCF on metrics.json, saves valuation.md」；Step 03 Briefing `exec-report-writer`「Reuses environment_id, reads both files, and saves investment_memo.md」
- 屏幕上的 metrics 输出（slide 照片）：`"fiscal_quarter": "Q2 FY2027"`、`"period_ended": "2026-07-26"`、`"filing_date": "2026-08-26"`、`"sec_filing": "Form 10-Q / Form 8-K"`、revenue `"amount_millions": 96221`（`"$96.22B"`）、`"prior_year_amount_millions": 46743`、`"yoy_growth_pct": 105.85`。这些是 demo 里 agent 的输出，**没有核对 NVIDIA 实际财报**
- valuation.md（4826 bytes）「Valuation Sensitivity Analysis」：Implied Share Price 矩阵（WACC 8.0%–10.5% × 永续增长 2.5%–4.5%），例如 WACC 8.0% / g 2.5% 为 $287.48，WACC 8.0% / g 4.5% 为 $424.69，WACC 10.5% / g 2.5% 为 $195.37；Implied Upside 矩阵从 −0.90% 到 +87.65%
- 讲者说（视频 15:03）：每一步都存成 step trace（function result、write to file、code execution），可以追踪、优化；quant agent 从同一个 environment 里的 `metrics.json` 接手，跑 Python 做 valuation，「doing a forecast for the share price」；第三个 agent 写报告；如果接了 MCP，可以写进 Google Doc 或发 Slack

**Demo 2：接 Google Drive 做数据可视化**

- slide（照片）：一段视频「NIMBUS CLOUD SAAS · FY 2025 · FINANCE STORY」「08 / 12 · TREASURY PULSE」「24 settlements. $148,068 peak surge.」，三格：OPERATING SETTLEMENT & CASHFLOW PULSE（Receipts）、POST-SETTLEMENT PACE 1.5、OCTOBER OPERATING SURGE 24
- 讲者说（视频 16:34）：managed agent 接了 Google Drive，里面是「a bunch of receipts of our annual kind of company data」，有一个 skill 告诉它怎么做 interactive report

**Demo 3：AI Studio gallery 里的 Tickr**

- slide（照片）：AI Studio 里的应用「Financial Research Agent [AIE]」，页面标题「Tickr, your intelligent financial document analyzer」，「Automatically finds and synthesizes recent SEC filings and public disclosures.」覆盖 Form 10-K & 10-Q、Form 8-K、Forms 3, 4, 5 & 13F；底部「Gemini can make mistakes, don't rely on it for financial advice.」
- 讲者说（视频 17:44）：最简单的开始方式是去 AI Studio 免费用；这个 demo 输入 VOO，「this ticker agent is a bunch of our different managed agents」；最后生成 PDF 报告，带图表和引用的资源，「it actually went to the Vanguard site, found the different documents」
- 讲者说（视频 19:33）：「And that is it on managed agents. Thank you very much.」

## Q&A 精确版

没有 Q&A。现场录音和视频都以「Thank you very much」结束（录音 18:20，视频 19:33），讲者在 demo 时说「since we have a few minutes」，没有留提问时间。

## 勘误表（Whisper / YouTube 字幕 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| Jack Witherspoon（视频字幕） | Jack Wotherspoon | 日程 |
| Cloud Managed Agents / cloud managed agents | Claude Managed Agents（和自家产品并列提到） | 上下文（我的理解） |
| The Gemini API, the deep one（录音） | the Gemini API at DeepMind | 视频字幕 |
| anti-gravity harness / anti-gravity studio products | Antigravity harness / Antigravity suite of products | slide、文档 |
| MTP tools / SMTP servers | MCP tools / MCP servers | slide |
| in my GA earlier this year（录音） | went GA earlier this year | 视频字幕 |
| plate books | playbooks | 视频字幕 |
| a query for teams to do your board（录音） | queries your team's Jira board | 视频字幕 |
| Jeremiah 4 / Gemini 4 part one（录音） | Gemini 4 / Gemini 4 Argon | 视频字幕、slide |
| spit out a container | spin up a container | 视频字幕 |
| SSE filing researcher | SEC filing researcher（`sec-filing-researcher`） | slide |
| main page is the one（录音） | managed agent is the one | 视频字幕 |
| has kind of a scale | has a skill | 视频字幕 |
| external votes | external folks | 视频字幕 |
| do the OO / VOO | VOO（Vanguard S&P 500 ETF 代码） | 视频字幕、上下文 |
| Banger site | Vanguard site | 视频字幕 |
| ticker agent is a bunch of art for managed agents | a bunch of our different managed agents | 视频字幕 |

## 出入

1. **building blocks 数量**：口头和视频章节是五个（harness、environment、skills、credentials、triggers），slide 是六块（加 Tools）
2. **默认模型**：讲者说快的用「Gemini Flash」；slide 和文档写明是 `gemini-3.8-flash`，而且文档说它是**默认模型**，Argon 要显式指定；文档还说 managed agent 的模型创建后不能在单次 interaction 里改
3. **sandbox 里有没有 token**：口头说放一个 placeholder / dummy token；slide 说「zero tokens in memory or env」
4. **触发时间例子**：口头 8:30am，slide 代码 cron 是工作日 7:00 America/New_York
5. **Argon 的数字**：「#1 on the Vals Index」二手报道支持（68.90%）；「lifting agentic code submissions at Google by 35%」没找到来源
6. **sandbox 规格**：slide 写 Debian · 4 vCPU · 16 GB，文档 Antigravity agent 页没有写规格

## 【待确认】清单

1. sandbox 规格「Debian · 4 vCPU · 16 GB」（Environments 文档页 404）
2. 「lifting agentic code submissions at Google by 35%」的出处
3. Gemini 4 Argon 发布日期和 Vals Index 名次（只有二手报道，Google 原文没打开）
4. Managed Agents / Interactions API GA 等日期（只看到 Google 博客的搜索摘要）
5. sandbox 里是 placeholder token 还是没有 token
6. slide 第 1、4、17–19 页内容（没拍到）
