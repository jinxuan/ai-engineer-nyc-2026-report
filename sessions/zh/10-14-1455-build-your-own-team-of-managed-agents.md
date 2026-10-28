[English](../10-14-1455-build-your-own-team-of-managed-agents.md) | **中文**

# 一次 API 调用，给 agent 一台自己的电脑

*在 AI Engineer NYC 2026 上，Google DeepMind 的 Jack Wotherspoon 展示了 Gemini Managed Agents 如何用一次 API 调用在云端 sandbox 里跑完整的 agent loop，然后把三个金融 agent 串在同一个共享 sandbox 上，对一只股票做调研、估值并写成报告。*

**Expo Session · 2026 年 10 月 14 日（周三）· 2:55–3:13pm · Shannon Room (NY Ballroom E)**
**讲者：** Jack Wotherspoon，Google DeepMind Member of Technical Staff（DevEx engineer）

![talk 开始时的 Shannon Room](../../slides/10-14-1455-build-your-own-team-of-managed-agents/01.jpg)

---

「我是 Google DeepMind 的 DevEx engineer。」Jack Wotherspoon 开场先问台下，谁听说过或用过 cloud agent 或 managed agent。Wotherspoon 说，这个想法很简单：「不用在自己机器上跑 harness、跑 coding agent，直接在云端的 sandbox 里跑就行。」

这场 18 分钟的 expo session 先讲为什么要这么做，再过了一遍 Google 版本（Gemini API 里的 Managed Agents）的组成部分，最后用三个围绕金融分析的现场演示收尾。

## 1. 为什么 agent 需要自己的电脑

![有了 sandbox，agent 能自己验证工作](../../slides/10-14-1455-build-your-own-team-of-managed-agents/02.jpg)
*第 2 张（共 19 张）：agent 有了自己的电脑之后跑的四步循环。*

第一个理由是验证。这张幻灯片标题是「With a sandbox, agents verify their own work」，写道：「有自己电脑的 agent 不会把没测过的代码丢给你，它会运行脚本、检查 traceback、修 bug，然后交回验证过的结果。」它列了四步：

| 步骤 | 发生了什么 | 幻灯片上的例子 |
| --- | --- | --- |
| 01 Write code | 写 Python 代码解析财报、计算现金流 | `write dcf.py` |
| 02 Execute code | 在 Linux 容器里运行脚本，捕获 stderr | `python3 dcf.py` |
| 03 Test and validate | 发现缺列或 NaN 行，修补代码后重跑 | `fix KeyError → retry` |
| 04 Deliver artifact | 结束这一轮之前确认图表渲染正常 | `chart.png ✓` |

页脚写着「Autonomous verification loop runs inside the cloud sandbox · Zero human prompts required」。Wotherspoon 说，在笔记本上跑这个循环会有副作用：「有时它会抢走浏览器，开始替你运行东西，你开始收到各种弹窗。」

![笔记本扩展不了](../../slides/10-14-1455-build-your-own-team-of-managed-agents/03.jpg)
*第 3 张：本地运行意味着要批准每条命令，或者把机器暴露出去。*

第二个理由是「你的笔记本扩展不了」。这张幻灯片对比了两种环境。在笔记本上，agent「卡在权限提示上」（一个 `Allow bash execution & network access? [y/N]` 提示），和你共用个人文件、SSH key 和登录中的会话，合上盖子就停，也没法扩展到成千上万个客户 session。在云端 sandbox 里，它「自主运行并验证」：一个隔离的 Linux 容器，「本地 blast radius 为零」，在后台异步运行，规模可以「从你的日常工作流扩展到成千上万的用户」。

Wotherspoon 从分享的角度讲了这件事：一个调好了 skill 和个人 coding agent 的开发者，没有简单的办法把这套配置交给同事，或者做进一个应用里，而且「你不会想让部署给客户的应用跑在你自己的订阅上」。手动搭替代方案很繁琐：起一个容器或 VM，装好东西，管理它，自己做 harness engineering，「更别提网络、防火墙规则和那一堆安全相关的事了」。

## 2. 一次调用 Interactions API

![现在只要一次 API 调用](../../slides/10-14-1455-build-your-own-team-of-managed-agents/04.jpg)
*第 5 张：整套配置就是一次 `interactions.create` 调用。*

「不管是 Gemini managed agents 还是 Claude managed agents，」Wotherspoon 说，这一类产品「归结起来就是一次 API 调用」，这场 talk 讲的是 Google 在 Gemini API 里的版本。幻灯片说，Interactions API「开箱即用地给你一个 Google 托管的 Linux sandbox、一个持久化文件系统和 Antigravity agent loop」，并展示了这次调用：

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

Wotherspoon 把 Interactions API 描述为「我们 SDK 和客户端库新的、以 agent 为先的版本」。旧的模型接口是一个输入、一个输出，「对 agent 来说扩展不起来」；一次 interaction 可以跨很多步、写文件、调 MCP 工具。这个 API「今年早些时候 GA 了」，「现在算是 managed agent 的大本营」。和普通模型调用相比有两处不同：你指定的是一个 agent（这里是基础的 Antigravity agent）而不是模型，并且设置 `environment="remote"`，告诉它「我们要一个在云端跑的 managed agent」。Google 的文档补充说，`environment` 也可以传一个已有 environment 的 ID，以复用它的文件和状态，或者传一个完整的配置对象，带自定义 source 和网络规则。

## 3. 架构和组成部分

![Gemini Managed Agents 架构](../../slides/10-14-1455-build-your-own-team-of-managed-agents/05.jpg)
*第 6 张：用户请求发给 Gemini API，由它驱动一个 sandbox。*

架构图追踪了一个请求（「Analyze Q3 filings and create a valuation report…」）：从用户到 Gemini API，API 里有工具（`code_execution`、文件系统、skills、MCP 工具）和模型，标为 Gemini 4 Argon。API 向 sandbox 发 execute 调用并取回结果。sandbox 里，根文件系统包含 `AGENTS.md`、`skills/` 和 `mcp_tools`，一个终端面板写着「Isolated Linux VM · 4 vCPU · 16 GB」和「Egress proxy injects credentials」。上方有四个输入流向 sandbox：GCS、一个仓库、inline 内容和凭证。（幻灯片页脚给出的机器是 Debian、4 vCPU、16 GB VM；Google 的 Antigravity agent 文档没有列出这些规格，所以这些数字只有幻灯片一个来源。）

Wotherspoon 说，API 有内置的服务端工具，比如 Google Search 和读写文件的文件系统工具，也接受自定义函数和 MCP 工具。sandbox 跑「在 Google Cloud 基础设施上」，启动时可以加载 agent skill、一个 `agents.md` 文件或一个 GitHub 仓库。

![managed agent 的组成部分](../../slides/10-14-1455-build-your-own-team-of-managed-agents/06.jpg)
*第 7 张：六个组成部分，每个都对应控制它的 API 参数。*

组成部分这张幻灯片列了六块：

| 组成部分 | 参数 | 作用（幻灯片原话） |
| --- | --- | --- |
| Harness | `agent_config` | 「规划工作、调用工具、检查自己的结果。」 |
| Environment | `environment` | 「它自己的 Linux 电脑，文件和包会持久保存。」 |
| Tools | `tools + mcp` | 「内置 bash、文件、web search 和远程 MCP server。」 |
| Skills | `.agents/` | 「AGENTS.md 里的指令和 SKILL.md 里可重复的工作流。」 |
| Credentials | `credentials` | 「API key 在 sandbox 外注入，secret 保持安全。」 |
| Schedule | `triggers` | 「cron 定时和后台运行，你不在时也在工作。」 |

Wotherspoon 在台上讲的是五大支柱（harness、environment、skills、credentials、triggers），视频的章节标题也是这五个；幻灯片多了 Tools 作为第六个。harness 是 Antigravity harness，「和 Google 那边 Antigravity 系列产品里跑的是完全同一个 harness」。environment「算是一个独立的实体」，所以多个 agent 可以指向同一个 sandbox，在彼此的文件上继续工作。skill 就像公司的「playbook」。credentials 是「managed agent 新加的一块」，把 token 或 MCP OAuth 注入 sandbox。trigger 用来定时运行；Wotherspoon 举的例子是「每天早上 8:30、开盘前」做金融分析，结果通过 webhook 发到 Slack，或者一个「每天早上查你们团队 Jira 看板」的 agent。

## 4. harness 和背后的模型

![Antigravity harness 跑这个循环](../../slides/10-14-1455-build-your-own-team-of-managed-agents/07.jpg)
*第 8 张：模型只是 `agent_config` 里的一个参数。*

harness 这张幻灯片说，它「和 Antigravity 产品基于同一个 harness。它规划多步工作、执行工具、验证自己的输出，并允许你配置底层模型」。模型用一行设置：`agent_config={"type": "antigravity", "model": "gemini-4-argon"}`，示例输入是「Audit all 10-K footnotes and reconcile segment revenue across 5 years」。提供两个模型：`gemini-4-argon`，用于「复杂编码和金融建模的深度推理」；`gemini-3.8-flash`，「快速、低延迟的默认选项，用于日常 agent 工作流」。Wotherspoon 的说法是：要速度和效率用 Flash，要深度推理或重分析用 Argon。Google 的文档确认 Flash 是默认值，所以 Argon 必须显式选择；文档还说 managed agent 的模型在创建 agent 时就固定了。

「我们昨天刚发布了 Gemini 4 Argon，我们新的 frontier model。」Wotherspoon 说。「它在金融任务上特别强」，在跨大量文档的数据分析和多模态工作上也是。下一张幻灯片（第 9 张；作者没拍到）为 Argon 列了三条宣传点：

| 宣传点 | 幻灯片原文 |
| --- | --- |
| #1 on the Vals Index | 「在金融、编码、法律和税务工作上领先 frontier model，把 Google 内部 agentic 代码提交量提升了 35%。」 |
| Long decode up to 1M tokens | 「共享 1M context window（输入 + thinking + 输出），thinking_level 可调（low、medium、high）。」 |
| Pair with 3.8 Flash for cost and speed | 「把最难的推理交给 Argon，快速的日常轮次交给 Gemini 3.8 Flash。」 |

媒体报道称 Argon 位居 Vals Index 第一；35% 这个数字没找到出处，只出现在幻灯片上。Wotherspoon 解释了 long decode：「通常模型的输出 token 上限在 64K 左右」，而在 Gemini 4 上，「100 万的 context window 或 token 上限现在由输入、thinking 和输出共享」，所以对于数学推理或金融建模这种「输出可能有好多好多页」的任务，「模型真的能做下来」。

## 5. 对话和 sandbox 是分开的状态

![Interactions 和 Environments 都是有状态的](../../slides/10-14-1455-build-your-own-team-of-managed-agents/08.jpg)
*第 10 张：一个 agent 写入 `/workspace`；第二个 agent 用全新的对话接手同一台机器。*

Wotherspoon 把 interaction 和 environment 称为「我们后端两个不同的有状态的东西」。interaction 保存对话（消息和推理轮次）；把一个 interaction 的 ID 作为 `previous_interaction_id` 传给下一次调用，就构成第二轮。environment 是 sandbox 的文件系统，即 `/workspace` 里的文件、数据和已安装的包，文件可以通过 API 上传或下载。

把两者分开，就能做 multi-agent。在幻灯片的代码里，一个 `market-researcher` agent 在一个新的远程 environment 里把 Q3 财报存到 `/workspace/q3.csv`；然后启动一个 `quant-analyst` agent，传入 `environment=step1.environment_id`，让它用同一个文件画估值图。第二个 agent「有一个全新的 context window 和对话，但复用了 environment」，Wotherspoon 说。「算是一种 multi-agent 工作流。」

## 6. 工具、playbook 和保存的 agent

![内置 sandbox 工具加远程 MCP](../../slides/10-14-1455-build-your-own-team-of-managed-agents/10.jpg)
*第 11 张：四个内置工具，几行代码挂上一个 GitHub MCP server。*

工具这张幻灯片说，每个 managed agent「启动时就开启了 shell、文件系统和 web grounding」：

| 工具 | 作用 |
| --- | --- |
| `code_execution` | 运行 Bash、Python 和 Node，捕获 stdout/stderr |
| `filesystem` | 在 `/workspace` 范围内读、写、编辑和搜索文件 |
| `google_search` | 实时 web search 和 `url_context` 网页抓取 |
| `mcp_server` | 通过 HTTP 挂接外部 Model Context Protocol server |

代码示例用一个名为 `github-prod` 的已存凭证挂上了 GitHub 的 MCP server（`https://api.githubcopilot.com/mcp`），让 agent 根据 `/workspace` 里的 DCF 结论开一个跟踪 issue。Wotherspoon 说，远程 MCP 让用户「接上你喜欢的服务」。

![用你的 playbook 配置 agent](../../slides/10-14-1455-build-your-own-team-of-managed-agents/11.jpg)
*第 12 张：把指令、skill 和数据挂载进 sandbox，再把这套配置保存为一个 agent。*

skill 这张幻灯片说，你可以「把 AGENTS.md、SKILL.md、Git 仓库或 Cloud Storage bucket 直接挂载进 sandbox，然后把这套配置升级为一个保存的 agent」。文件树里有 `.agents/AGENTS.md`（人设和规则）、`.agents/hooks.json`（guardrail）、`.agents/skills/dcf-model/SKILL.md`，以及从 Git 或 GCS 挂载的 `/workspace/filings/`。代码往 environment 里放了两个 source：一个 inline 的 `AGENTS.md`，内容是「You are a rigorous equity analyst」，和一个挂载到 `.agents/skills` 的仓库（`https://github.com/org/fin-skills`）。代码上方三个标签页说明了工作流：inline 定制、保存 agent、按 ID 调用。

Wotherspoon 说，你的 agent 在个人机器上遵循的 playbook，在云端也能照样跑，而且没有「覆盖文件或把你机器搞乱」的风险。一个工作流跑通之后，用 `agents.create` 保存，「之后用 agent ID 就能很方便地调用你的个人 agent」。

## 7. secret 永远不进 sandbox

![secret 永远不进 sandbox](../../slides/10-14-1455-build-your-own-team-of-managed-agents/12.jpg)
*第 13 张：egress proxy 在请求出去的路上附上 token。*

凭证这张幻灯片写道：「通过 client.credentials.create() 一次性存好 token 和凭证。agent 调用 allowlist 里的域名时，egress proxy 在线路上注入 secret。」图里有三个框。agent sandbox 调用 `api.polygon.io` 或 `github.com`，「内存和环境变量里都没有 token」；egress proxy「校验 network.allowlist，附上 bearer_token 或刷新 oauth2」；外部 API「收到经过认证的请求；sandbox 永远无法窃取 key」。代码把一个 bearer token 存为 `polygon-prod`，并用这个凭证把 `api.polygon.io` 加入 allowlist。

Wotherspoon 把 proxy 描述为 sandbox 和 API 之间的一层，它「在 sandbox 里放一个占位用的假 token，这样 agent 就永远不会失控，说『哦，我看到这儿有个 GitHub token，让我看看上面还有什么』」。agent 调用 GitHub API 时，proxy 匹配域名并注入真正的 token。（口头说法提到 sandbox 里有一个占位 token；幻灯片说的是「zero tokens」。两者都认同真正的 secret 留在外面。）

## 8. 定时、后台运行和一支 agent 团队

![定时运行，或在后台并行展开](../../slides/10-14-1455-build-your-own-team-of-managed-agents/13.jpg)
*第 14 张：一个工作日 cron trigger，和一个非阻塞的后台任务。*

定时这张幻灯片展示了两种模式。用 `client.triggers.create()` 创建一个名为 `pre-market-briefing` 的 trigger，按 cron `0 7 * * 1-5`、时区 `America/New_York`（工作日早上 7:00；口头举例是 8:30）运行 `quant-analyst` agent，输入是「Refresh morning risk report」，复用一个共享 environment。一个后台任务 `background=True` 让 agent「Run 10,000 Monte Carlo paths on NVDA」，用 `client.interactions.get(id=job.id)` 轮询。Wotherspoon 建议的用法有：每日简报、扫描邮件并建议回复、开盘前看一下市场。

![一个 prompt，三个专门的金融 sandbox](../../slides/10-14-1455-build-your-own-team-of-managed-agents/14.jpg)
*第 15 张：一个协调者把工作分给三个专家 agent。*

因为 managed agent 就是一次 API 调用，一个 agent 可以调用其他 agent，把工作分发出去。团队架构这张幻灯片里有一个协调者「Portfolio Lead」，它「把股票分析并行拆给三个专门的 managed agent」，每个都保存了自己的 `AGENTS.md` 人设、`SKILL.md` 工作流和 Python 包：

| Agent | 任务 | 产出 |
| --- | --- | --- |
| `sec-filing-researcher` | 提取 10-K 和 10-Q 的分部数据表 | `segments.json` |
| `quant-dcf-modeler` | 用 Python 跑 Monte Carlo 和敏感性分析 | `valuation.png` |
| `exec-report-writer` | 综合出投资论点和风险备忘录 | `memo.pdf` |

产出经 Files API 汇总成一份「Verified Briefing」，包含「经过审核的数字、图表和可供决策者使用的 PDF」。Wotherspoon 主张用「细分、目标明确、只有一个核心用途的 agent」，而不是把「一个非常非常宽泛的任务」交给一个 agent，那样「没办法衡量它实际在做什么」。

## 9. 三个演示

![来实际测试一下](../../slides/10-14-1455-build-your-own-team-of-managed-agents/15.jpg)
*第 16 张：现场构建，researcher 对 NVIDIA 的输出正在流式显示。*

**调研、建模、报告。**现场构建在同一个共享 sandbox 上按顺序跑了三个 agent，标的是 NVDA，标语是「Re-use environments to allow agents to pick up where others left off」。第 1 步，`sec-filing-researcher` 拉取季度营收和分部数据，写入 `/workspace/metrics.json`。第 2 步，`quant-dcf-modeler` 复用 environment ID，基于这个文件跑 Python DCF，存为 `valuation.md`。第 3 步，`exec-report-writer` 读取这两个文件，存为 `investment_memo.md`。

屏幕上 researcher 的输出给出的财季是 Q2 FY2027（截至 2026-07-26，2026-08-26 提交，来自 Form 10-Q / Form 8-K），营收 $96,221 million（$96.22B），去年同期 $46,743 million，同比增长 105.85%。这些是 agent 的演示输出，没有和 NVIDIA 的财报核对过。

![估值敏感性表](../../slides/10-14-1455-build-your-own-team-of-managed-agents/16.jpg)
*quant agent 的 `valuation.md`：不同折现率和增长率下的隐含股价和上涨空间。*

quant agent 的 `valuation.md`（4,826 字节）里有一份「Valuation Sensitivity Analysis」。隐含股价矩阵把折现率（WACC）和永续终值增长率交叉排列：

| WACC \ 增长率 | 2.5% | 3.0% | 3.5% | 4.0% | 4.5% |
| --- | --- | --- | --- | --- | --- |
| 8.0% | $287.48 | $311.49 | $340.84 | $377.52 | $424.69 |
| 8.5% | $262.89 | $282.53 | $306.11 | $334.92 | $370.93 |
| 9.0% | $242.10 | $258.42 | $277.70 | $300.85 | $329.13 |
| 9.5% | $224.29 | $238.02 | $254.04 | $272.98 | $295.70 |
| 10.0% | $208.86 | $220.55 | $234.03 | $249.77 | $268.36 |
| 10.5% | $195.37 | $205.41 | $216.89 | $230.13 | $245.58 |

第二个矩阵给出隐含上涨空间，可见行里从 −0.90%（WACC 9.5%、增长率 2.5%）到 +87.65%（WACC 8.0%、增长率 4.5%）。Wotherspoon 指出，每一步都保存为 step trace（函数结果、文件写入、代码执行），可以追踪和优化；quant agent 因为共享 sandbox，「可以从那个 metrics.json 接着做它的任务」，「对股价做预测」。

![report writer 在工作](../../slides/10-14-1455-build-your-own-team-of-managed-agents/17.jpg)
*第 3 步运行中：report writer 从共享 workspace 读取 `valuation.md` 和 `dcf_model.py`。*

第三个 agent 把「调研和知识堆」变成一份报告；它的 trace 显示它查看了 `/workspace/valuation.md`、列出了 `/workspace`、读取了 `dcf_model.py`。Wotherspoon 说，挂上 MCP server 的话，它可以写进 Google Doc 或发到 Slack。

![用 Google Drive 数据做的财务故事](../../slides/10-14-1455-build-your-own-team-of-managed-agents/18.jpg)
*第二个演示：用 Google Drive 里一家公司的收据生成的交互式报告。*

**来自 Google Drive 的报告。**第二个演示展示了一个嵌在应用里的 managed agent，连接到一个 Google Drive，「我们往里面倒了一堆公司年度数据的收据」。一个 skill 告诉它怎么生成交互式报告。屏幕上的结果是「Nimbus Cloud SaaS · FY 2025 · Finance Story」，停在 12 个面板中的第 8 个「Treasury Pulse」：「24 settlements. $148,068 peak surge」，配有经营结算和现金流图表，结算后节奏 1.5，十月经营高峰 24。Wotherspoon 说，凡是个人 coding agent 能做的事，都可以变成一个每天为公司跑、或者跑在你自己应用里的 managed agent。

![AI Studio 里的 Tickr](../../slides/10-14-1455-build-your-own-team-of-managed-agents/19.jpg)
*第三个演示：Tickr，AI Studio gallery 里的一个财务文件分析器。*

**AI Studio 里的 Tickr。**「今天上手最简单的办法，是去 AI Studio 免费开始。」Wotherspoon 说，然后演示了 AI Studio gallery 里的一个应用「Financial Research Agent [AIE]」。页面写着「Tickr, your intelligent financial document analyzer」，它「自动查找并综合最近的 SEC 文件和公开披露」，覆盖 Form 10-K 和 10-Q、8-K，以及 3、4、5 和 13F，页脚写着「Gemini can make mistakes, don't rely on it for financial advice」。Wotherspoon 输入了 VOO；「这个 ticker agent 由我们好几个不同的 managed agent 组成」，每个负责一种不同的分析。最后一个 agent 生成了一份 PDF 报告，包含分析、要点、图表和来源链接：「它真的去了 Vanguard 网站，找到不同的文件，读进来，然后做了分析。」

「managed agents 就讲到这里。」Wotherspoon 说。「非常感谢。」

---

**链接：** [Schedule](https://ai.engineer/nyc/2026/schedule?session=970f672e-f351-46bb-9790-78c6b1baf81d) · [Video](https://www.youtube.com/watch?v=Y_p3y6zP5QE) · [Introducing Managed Agents in the Gemini API](https://blog.google/innovation-and-ai/technology/developers-tools/managed-agents-gemini-api) · [Interactions API: our primary interface for Gemini models and agents](https://blog.google/innovation-and-ai/technology/developers-tools/interactions-api-general-availability) · [Expanding Managed Agents in Gemini API](https://blog.google/innovation-and-ai/technology/developers-tools/expanding-managed-agents-gemini-api/) · [Antigravity agent documentation](https://ai.google.dev/gemini-api/docs/antigravity-agent)

本文依据官方视频字幕、作者的现场录音、幻灯片照片（第 1、4 和 17–19 张没有拍到）以及 Google 的文档。talk 结束时没有 Q&A。[精确笔记](../precise/10-14-1455-build-your-own-team-of-managed-agents.md)（中文）保留了时间戳，以及完整的更正和待解决问题列表。
