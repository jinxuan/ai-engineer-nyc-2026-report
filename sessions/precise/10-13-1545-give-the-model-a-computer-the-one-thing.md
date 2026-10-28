# Give the Model a Computer：精确版

> Precise notes for this session, compiled from the official video captions where available, the author's room recording, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材核对完，slide 17 张逐张读过。录音有两段缺口（开头和中段约 4 分钟），缺口对应的内容只有 slide。没有 Q&A。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| Q&A | 没有录到；Expo Session 只有 18 分钟，录音在 "Thank you." 结束。是否有提问【待确认】 |
| slide | 17 张（IMG_0493–0510，缺 0504）。**排除 IMG_0493**：15:42 拍的，开场前，画面是模糊的会场天花板，不是 slide。IMG_0494（quiz 的糊版）、IMG_0498（不可读）、IMG_0508（slide 14 的糊版）没有新信息。slide 右下角有页码（共 16 页），拍到的是 02、04、07、08、09、10、13、14、15、16；**01、03、05、06、11、12 没拍到** |
| 视频 | 没有 |
| 讲者资料 | [The 7 Best AI Agent Frameworks: Features and Tradeoffs](https://www.daytona.io/dotfiles/best-ai-agent-frameworks)（Mislav Ivanda，Daytona Developer Experience Engineer，2026-09-16）。这场 talk 的「三种 isolation」和「frameworks already voted」那张表都和这篇文章一致，下文引用为「资料（讲者文章）」。注意它是讲者自己写的，不是独立来源 |

## 基本信息

- 时间地点：2026-10-13 3:45–4:03pm，Sutton Room (NY Ballroom W)，Expo Session (18 mins)（session.json）
- 讲者：Mislav Ivanda，Daytona（slide 结尾「@ivanda_mislav」；资料（讲者文章）署名 Developer Experience Engineer, Daytona）。session.json 里 role/company 为空
- 官方简介：讲者做了一年 Daytona 和各 agent 框架的集成（LangChain、CrewAI、Google ADK、Mastra、Pydantic AI、OpenAI Agents SDK 等），发现这些框架「all converge on one identical primitive: give the model a computer」；「Only three answers exist, and two of them stop holding once real users show up」；结论「pick the boundary before the framework. Frameworks are opinions. The boundary is a commitment.」
- slide 是一个本地网页（地址栏 `127.0.0.1:8000/#/N`），每页页脚有 Daytona logo 和「编号 · 小标题」

## 1. Quiz：这是哪个框架？（slide 02，无录音）

- slide「A quiz — Which framework is this?」：

```python
@tool(
    name="run_code",
    description="Execute Python and return its output",
)
async def run_code(code: str) -> str:
    sandbox  = daytona.create()
    response = sandbox.process.code_run(code)
    sandbox.delete()
    return response.result
```

- 我的理解：代码里没有任何框架特有的东西，只有一个 `@tool` 装饰器和 Daytona 的 create / code_run / delete，用来引出「各家框架的这段代码长得一样」。讲者怎么揭晓答案没录到，【待确认】

## 2. 框架什么都不一致，除了这一件（slide 04）

- slide「The question — Frameworks agree on nothing. Except **this**.」，三栏：
  - WHAT AN AGENT IS：graph vs crew vs loop vs product
  - THE LANGUAGE：Python vs TypeScript
  - WHO DRIVES：you write the flow vs the model decides
  - 底部：「They disagree about what an agent even is. And then they all ship the same primitive.」
- 讲者说 [00:00]（录音从这里开始，前半句缺失）：「For LangGraph you explicitly define your agent workflows and trajectories, while for example in CrewAI agents do things on their own and they decide on their own to achieve the end goal.」「companies like this that don't agree on basically anything agree on something, that probably isn't just a fashion, it's a necessity.」
- 讲者说 [00:30]：agent 做有用的事，最终都要执行代码或要 shell access，「and the next question is where will this happen, in which execution environment」

## 3. 三种答案：代码在哪里跑

讲者说 [00:45]：「there are basically three types of possible isolation」。资料（讲者文章）把三种叫做 host execution with permissions / in-process interpreter isolation / external environment isolation。

### 3.1 Answer No1：在你自己的机器上，靠权限（slide 05/06 没拍到）

- 讲者说 [00:53]–[02:12]：最基础的，「probably the one you're using locally when developing with your agent」。定义命令 allow list、approval prompt（「are you sure you want to run this command」，「although probably most of us just click yes」）、interception hooks
- 讲者说 [01:22]：问题很明显：允许 agent 跑 `python -c`，它就能「open any sockets, modify your file system, execute code」；「you essentially defined what your model should use but you didn't define what your model can actually use」。模型被攻破时，等于在本机给它开了后门
- 资料（讲者文章）：这是「a permission model, not containment」

### 3.2 Answer No2：在你进程里的 interpreter（slide 07）

- slide「Answer No2 — In an interpreter, inside **your process**.」：+ QuickJS / V8 isolates；+ no filesystem, no network；+ tools bridged to the host。「→ It isolates the glue, not the tools.」
- 讲者说 [02:12]：把代码执行放进 V8 或 QuickJS 这类 embedded runtime，比第一种好，有网络和文件系统隔离
- 讲者说 [02:28]–[03:19]：问题更隐蔽：agent 有 query database、send email 之类的工具，生成一段调用这些工具的脚本，脚本在 runtime 里跑，「but the problem is that these tools that your agent will call inside that runtime will actually run on your device outside of that runtime」；「you isolate the glue code that your agent generates but you're not actually isolating the actual tool calls」
- 讲者说 [03:19]：举例「if you have the [delete] database [tool]」（转写为 "lead database"，【待确认】）

### 3.3 Answer No3：在另一台计算机上（slide 08）

- slide「Answer No3 — On a **separate computer.**」：+ container or microVM；+ reached over an API；+ can't reach your infra。「→ A compromised step is not a compromised app.」
- 讲者说 [03:23]：推荐的做法，「use a separate computer or VM」，通过 API 交互、控制生命周期；这台 VM「never touches or interferes with your local infrastructure, network, file system」
- 讲者说 [03:54]–[04:40]：最关键的是 slide 最后一句。前两种方式里，你批准的那条 python 命令出问题，就是在破坏你的应用；这里「you can run the worst code on the planet inside the sandbox and it can't interfere with your application logic. You can just throw out that sandbox and spin up another one.」

## 4. Receipts：框架已经投了票（slide 09）

- slide「Receipts — The frameworks already voted.」：

| 时间 | 框架 | slide 原文 |
| --- | --- | --- |
| Apr 2026 | CrewAI | **Removed** CodeInterpreterTool; deprecated **allow_code_execution**. Docs now point to a dedicated sandbox service. |
| Jul 2026 | AG2 1.0 | Retired the **autogen.coding** executors. The new SandboxCodeTool has **no default backend**: the environment is a **required argument**. |
| | Claude Agent SDK | Sandbox is Bash-only, off by default, fails open. Official guidance for untrusted code: **"a dedicated VM."** |
| | OpenAI Agents SDK | Ships **SandboxAgent** with pluggable local, Docker, and **hosted** sandbox clients. |
| | Mastra | **"You must choose its execution boundary deliberately."** Host, isolate, or external, stated plainly in the docs. |

  底部：「They're not adding sandboxes. They're deleting the alternative.」
- 讲者说 [04:31]–[05:56]：「to back up this, that I'm not just talking some nonsense, here's the proof」。CrewAI 2026 年 4 月去掉了在本机跑的 CodeInterpreter tool，现在指向专门的 sandbox 服务；AG2 退役了旧的本地 code executor，新的 SandboxCodeTool 要求你显式指定环境；Claude Agent SDK 默认的 sandbox 是进程级的，例如 Linux 上用 bubblewrap；OpenAI Agents SDK 的 SandboxAgent 原生支持 sandbox provider 和 Docker，也允许本地执行；Mastra 类似。「They already had them, but they are deleting all the alternatives.」
- 资料（讲者文章）：CrewAI 2026 年 4 月从 `crewai-tools` 移除 `CodeInterpreterTool`，并弃用 `Agent` 上的 `allow_code_execution` 和 `code_execution_mode`，计划在 v2.0 删除；AG2 1.0（2026 年 7 月）把 `autogen.coding` 移出主包（旧 executor 留在 AG2 Classic），backend 是必填参数；Mastra 文档原文「because code mode runs model-authored code, you must choose its execution boundary deliberately」
- 资料（[Claude Code sandboxing 文档](https://code.claude.com/docs/en/sandboxing)）：「The sandbox is off by default」；macOS 用 Seatbelt，Linux 用 [bubblewrap](https://github.com/containers/bubblewrap)；「The sandbox covers shell commands only. Claude's file tools, MCP servers, and hooks run outside it」；sandbox 起不来时默认不加 sandbox 继续跑，设 `sandbox.failIfUnavailable: true` 才会退出。即 slide 的「off by default, fails open」属实；「Bash-only」现行文档的说法是覆盖 Bash、PowerShell 和 Monitor 命令（shell commands），见出入
- 资料（[OpenAI Agents SDK Sandbox agents 文档](https://openai.github.io/openai-agents-python/sandbox_agents)，据搜索摘要，未逐页核对）：有 `UnixLocalSandboxClient`、`DockerSandboxClient`（`openai-agents[docker]`）和 E2B、Modal、Daytona 等 hosted client；v0.14.0 引入，beta
- 【待确认】：CrewAI 移除 CodeInterpreterTool 的一手出处（changelog / PR）。我只在 CrewAI 文档 changelog 里看到 v1.15.19（2026-09-04）从文档示例中删掉 CodeInterpreterTool 的条目，没找到 4 月删除类本身的记录；Claude Agent SDK「official guidance: a dedicated VM」的原文出处也没核对到（Claude Code 文档说的是把整个进程放进「container, virtual machine, or the sandbox runtime」）

## 5. 这个 primitive 在长大（slide 10–12，后两页没拍到、录音中断）

- slide「And the primitive keeps growing · 1/3 — First, it was a **tool call.**」：+ run generated code / + analyze a CSV / + return the answer / + throw the computer away
- 讲者说 [05:56]–[06:26]：sandbox 的用法也在变。最早是短命的机器，「your agent would spin it up on demand, run some code or task inside it and shut it down」
- 讲者说 [06:26]：模型更强之后，「people realized they could literally shift all the agents inside the sandbox」，给它文件系统、网络等一切
- 讲者说 [06:41]：最近又一个变化，「if they can run thousands of these execution environments concurrently, they can do really-」（录音在这里断）
- 官方简介对应的三步：「first a tool call, then the place the agent lives, then a thousand machines at once」。2/3、3/3 两页的内容【待确认】（没拍到，也没录到）

## 6. The twist：问错了问题（slide 13，无录音）

- slide「— [So, the question] [you] came in with — ~~Which framework should I pick?~~ Wrong question.」（小标题拍糊了，方括号部分是我按残字补的，【待确认】）

## 7. The asymmetry：什么能换，什么换不了（slide 14，无录音）

- slide「What actually locks you in — Swappable decisions vs. a **commitment.**」

| YOU CAN SWAP LATER | YOU LIVE WITH |
| --- | --- |
| **The framework** — a refactor, teams do it all the time | **The execution posture** — host, in-process, or a separate computer |
| **The model & the prompts** — expect some re-tuning | → shapes every tool you write |
| **Missing features** — evals, tracing, memory: plug in a tool | → becomes your security & tenancy story |
| **The sandbox provider** — they all speak the same five lines | → skipped now, an incident adds it later |

- 我的理解：「the same five lines」对应 slide 02 quiz 里那段 create / code_run / delete 代码

## 8. 结论：先选边界，再选框架（slide 15–16）

- slide「Pick the boundary before the framework.」「Frameworks are opinions. The boundary is a commitment.」
- 讲者说 [11:00]：「when you're starting to work on the product, you define an interface, the execution posture, between an interface you control, and everything except that can be easily swappable in the future」；定好这个接口后，用第三种架构，「giving your agent a proper computer」
- 讲者说 [11:30]：做对了，「the security part regarding the execution is solved」，可以专心做产品功能
- slide「Daytona — $ Thank you.」三个二维码：integration guides / plugins & packages / the full comparison；「Find me after for a chat · @ivanda_mislav」
- 讲者说 [11:50]：集成和 guide 都开源了；第三个二维码是比较各 agent 框架的文章（即上面的资料（讲者文章），【待确认】二维码是否就指向它）

## Q&A 精确版

无。录音在致谢处结束（见素材盘点）。

## 勘误表（Whisper 转写 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| LandGrap | LangGraph | 上下文 |
| Crew AI | CrewAI | slide |
| quick jazz | QuickJS | slide |
| v8 | V8 | slide |
| clue code | glue code | slide（"isolates the glue"） |
| co-interpreter tool | CodeInterpreterTool | slide |
| Cloud Engine SDK | Claude Agent SDK | slide |
| it's special only Sandbox | 疑为 "it's [a Bash-]only sandbox"，【待确认】 | slide（"Bash-only"） |
| bubble wrap | bubblewrap | 资料 |
| some other samples provider | sandbox provider | 上下文 |
| transparent related listed flows | 听不清（疑为 "transparently documented flaws"），【待确认】 | — |
| Monstra | Mastra | slide |
| there are not any sandboxes | they're not adding sandboxes | slide |
| cloud sqp are you sure the ones run this command | 疑为 "Claude: are you sure you want to run this command"，【待确认】 | 上下文 |
| your cloud agent | your Claude agent（或 coding agent），【待确认】 | 上下文 |
| lead database | 疑为 delete database，【待确认】 | 上下文 |
| compromise step is not to compromise that | a compromised step is not a compromised app | slide |
| your call application | 疑为 your whole application，【待确认】 | 上下文 |
| execution posture between an interface you control | the execution posture, behind an interface you control（疑），【待确认】 | slide 14 |

## 出入

1. **Claude Agent SDK「Bash-only」**：slide 写 Bash-only；[Claude Code 现行文档](https://code.claude.com/docs/en/sandboxing)说 sandbox 作用于「Bash, PowerShell, and Monitor commands」，即所有 shell 命令，文件工具、MCP、hooks 在 sandbox 外。实质（只管 shell、不管其他工具）一致，措辞有差。slide 说的是 Agent SDK，我核对的是 Claude Code 文档，两者的 sandbox 配置是同一套，【待确认】Agent SDK 文档是否有单独表述
2. **CrewAI 的时间和动作**：slide 和讲者都说 2026 年 4 月「Removed CodeInterpreterTool」；能找到的 CrewAI 一手记录只有 2026-09-04 v1.15.19 的文档改动（从示例中删掉）。4 月删除的一手出处【待确认】
3. **讲者把三种做法叫「three types of possible isolation」**，但第一种他自己也说不是隔离；讲者文章的措辞是「a permission model, not containment」。这是用语上的不一致，不是事实出入
4. **官方简介 vs 现场**：简介列了 LangChain、Google ADK、Pydantic AI 等，slide 09 的表只有 CrewAI、AG2、Claude Agent SDK、OpenAI Agents SDK、Mastra 五家；讲者文章是七家

## 【待确认】汇总

1. slide 02 quiz 的揭晓（无录音）
3. 「lead database」的原话
4. CrewAI 2026 年 4 月移除 CodeInterpreterTool 的一手出处
5. Claude Agent SDK「official guidance: a dedicated VM」的原文出处
6. OpenAI SandboxAgent 文档细节（只看了搜索摘要）
7. slide 11、12（primitive 2/3、3/3）的内容
8. slide 13 小标题残字
9. 第三个二维码是否指向讲者文章
10. 是否有 Q&A
11. 勘误表里 6 处听不清的转写（"special only Sandbox"、"transparent related listed flows"、"cloud sqp"、"your cloud agent"、"call application"、"posture between an interface"）
