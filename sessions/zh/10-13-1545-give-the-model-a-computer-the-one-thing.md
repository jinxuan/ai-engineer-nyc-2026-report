[English](../10-13-1545-give-the-model-a-computer-the-one-thing.md) | **中文**

# 先选 agent 代码在哪跑，再选框架

*在 AI Engineer NYC 2026 上，Daytona 的 Mislav Ivanda 指出：几乎在任何事上都意见不合的 agent 框架，在一个 primitive 上走到了一起——给模型一台电脑；而执行边界是团队很难回头改的那个决定。*

**Expo Session · 2026 年 10 月 13 日（周二）· 3:45–4:03pm · Sutton Room (NY Ballroom W)**
**讲者：** Mislav Ivanda，Developer Experience Engineer，Daytona

---

Mislav Ivanda 花了一年时间，在 Daytona（给 AI agent 跑 sandbox 的公司）和 LangChain、CrewAI、Google ADK、Mastra、Pydantic AI、OpenAI Agents SDK 这些 agent 框架之间做集成。按 session 介绍的说法，得到的教训是：这些框架「都收敛到同一个 primitive：给模型一台电脑」，做法只有三种，而且「其中两种一旦有真实用户进来就撑不住了」。

这场 talk 用的是一份 16 页的幻灯片，从本地网页放出来。作者的现场录音缺了开头几分钟和中间大约四分钟；这些部分只能靠幻灯片来描述。

## 1. 小测验：这是哪个框架？

![A quiz: Which framework is this?](../../slides/10-13-1545-give-the-model-a-computer-the-one-thing/03.jpg)
*开场小测验：一个执行代码的 tool，里面没有任何框架特有的东西。*

第二页幻灯片让观众从这段代码认出是哪个框架：

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

除了一个通用的 `@tool` decorator，这个函数只是创建一个 Daytona sandbox、在里面跑代码、删掉 sandbox、返回结果。里面没有任何东西能看出是哪个框架。Ivanda 是怎么揭晓答案的，录音里没有。

## 2. 框架们什么都不同意，除了这一点

![Frameworks agree on nothing. Except this.](../../slides/10-13-1545-give-the-model-a-computer-the-one-thing/04.jpg)
*框架之间的三处分歧，和一个共同点。*

幻灯片列出了框架之间的三处分歧：

| agent 是什么 | 语言 | 谁来驱动 |
| --- | --- | --- |
| graph vs crew vs loop vs product | Python vs TypeScript | 你写流程 vs 模型来决定 |

下面写着："They disagree about what an agent even is. And then they all ship the same primitive."（它们连 agent 是什么都意见不一。然后全都发布了同一个 primitive。）

录音从这里半句话开始接上。「LangGraph 里你要显式定义 agent 的工作流和 trajectory，而比如在 CrewAI 里，agent 自己做事，自己决定怎么达成最终目标，」Ivanda 说。当那些「基本上什么都不同意的」公司在某件事上达成一致，「那大概就不只是潮流，而是必需」。

Ivanda 的解释是：任何做点有用事情的 agent，最终都需要执行代码或拿到 shell 权限，「接下来的问题就是这件事在哪里发生，在哪个执行环境里」。Ivanda 说，「基本上有三种可能的隔离方式」。

## 3. 第一种答案：在你自己的机器上，靠权限把关

第一种答案的幻灯片没拍到，但录音里有。这是最基础的做法，「大概就是你本地开发 agent 时在用的那种」：一个命令 allow list，弹出确认问你是否真要运行某条命令（「虽然我们大多数人大概都直接点 yes」），再加上拦截用的 hook。

Ivanda 说，问题很明显。允许 agent 运行 `python -c`，它就能「打开任意 socket、修改你的文件系统、执行代码」。「你定义的是模型应该用什么，但没定义模型实际上能用什么。」一旦模型被攻破，这种做法等于在开发者的机器上开了一扇后门。（Ivanda 自己的对比文章把这称为「a permission model, not containment」（权限模型，而不是隔离），所以 talk 里叫它「isolation」有点勉强。）

## 4. 第二种答案：在你进程里的解释器中

![Answer No2: In an interpreter, inside your process](../../slides/10-13-1545-give-the-model-a-computer-the-one-thing/07.jpg)
*第二种答案："It isolates the glue, not the tools."*

第二种答案的幻灯片列出了这种做法的特点：QuickJS 或 V8 isolate，没有文件系统、没有网络，tool 桥接到宿主机。它的结论："It isolates the glue, not the tools."（隔离的是胶水代码，不是 tool。）

Ivanda 说，把 agent 生成的代码放进 V8 或 QuickJS 这样的嵌入式 runtime 里跑，比第一种好，因为有了网络和文件系统隔离。但它的缺陷更隐蔽。假设 agent 有「查数据库」或「发邮件」这样的 tool，它写一段脚本去调用这些 tool，脚本在 runtime 里跑，「但问题是，agent 在 runtime 里调用的这些 tool，实际上是在 runtime 外面、在你的设备上执行的」。恶意的 agent 可以不直接动手，而是借 tool 动手：「你隔离了 agent 生成的胶水代码，但并没有隔离真正的 tool call。」agent 可以直接用邮件或数据库 tool 去执行恶意命令。

## 5. 第三种答案：在另一台电脑上

![Answer No3: On a separate computer](../../slides/10-13-1545-give-the-model-a-computer-the-one-thing/08.jpg)
*第三种答案："A compromised step is not a compromised app."*

Ivanda 推荐的做法是「另一台电脑或 VM」。幻灯片列出：一个 container 或 microVM，通过 API 访问，碰不到你的基础设施。应用通过 API 和它对话、控制它的生命周期，它「从不触碰、也不干扰你本地的基础设施、网络、文件系统」。

关键在幻灯片最后一行："A compromised step is not a compromised app."（一个步骤被攻破，不等于整个应用被攻破。）前两种做法里，一个坏步骤，比如开发者批准了的一条 Python 命令，会伤到应用本身。这里，「你可以在 sandbox 里跑这个星球上最烂的代码，它也干扰不了你的应用逻辑。把那个 sandbox 扔掉，再起一个就行」。

## 6. 证据：框架们已经投了票

![The frameworks already voted](../../slides/10-13-1545-give-the-model-a-computer-the-one-thing/10.jpg)
*五个框架正在离开本地代码执行。*

「为了证明我不是在胡说，这里是证据，」Ivanda 说，然后展示了各框架的走向：

| 日期 | 框架 | 幻灯片上的说法 |
| --- | --- | --- |
| Apr 2026 | CrewAI | 移除了 CodeInterpreterTool；弃用 `allow_code_execution`。文档现在指向专门的 sandbox 服务。 |
| Jul 2026 | AG2 1.0 | 下线了 `autogen.coding` executor。新的 SandboxCodeTool 没有默认后端：环境是必填参数。 |
| | Claude Agent SDK | Sandbox 只管 Bash，默认关闭，fails open。对不可信代码的官方建议："a dedicated VM." |
| | OpenAI Agents SDK | 发布 SandboxAgent，可插拔的本地、Docker 和托管 sandbox client。 |
| | Mastra | "You must choose its execution boundary deliberately." 文档里直接写明 host、isolate 或 external。 |

幻灯片的结论："They're not adding sandboxes. They're deleting the alternative."（它们不是在加 sandbox，而是在删掉其他选项。）

Ivanda 逐行讲了一遍。CrewAI 移除了在本地跑代码解释器的 tool，现在指向专门的 sandbox 服务；AG2 下线了旧的本地 executor，新 tool 要求开发者显式指定环境。Claude Agent SDK 的默认 sandbox 在进程层面工作，在 Linux 上用 bubblewrap 这类工具。OpenAI Agents SDK 原生支持 sandbox provider 和 Docker，但仍然允许本地执行，Mastra 也类似。「它们本来就有这些，」Ivanda 说，「但它们正在删掉所有其他选项。」

这些说法有的能和文档对上，有的对不上。Ivanda 的对比文章对 CrewAI 的说法一致（该 tool 从 `crewai-tools` 中移除，`allow_code_execution` 和 `code_execution_mode` 已弃用、将在 v2.0 删除），对 AG2 1.0 也一致（`autogen.coding` 移出主包），还直接引用了 Mastra 的文档。Claude Code 的 sandboxing 文档证实 sandbox 默认关闭，在 macOS 上用 Seatbelt、在 Linux 上用 bubblewrap，默认情况下如果启动不了就不带 sandbox 继续运行；文档描述的 sandbox 覆盖 shell 命令（Bash、PowerShell 和 Monitor），不只是 Bash，而文件 tool、MCP server 和 hook 都在它之外运行。CrewAI 在 4 月移除该 tool 的一手记录，以及「dedicated VM」这句引文的出处，都没找到；能找到的唯一一条 CrewAI changelog 是 2026 年 9 月的一次文档改动，把该 tool 从示例中删掉。

## 7. 这个 primitive 还在长大

![First, it was a tool call](../../slides/10-13-1545-give-the-model-a-computer-the-one-thing/11.jpg)
*三个阶段中的第一个：sandbox 作为用完即弃的 tool。*

Ivanda 说，sandbox 的用法也变了。标着 1 of 3 的幻灯片写着 "First, it was a tool call"（一开始，它是一次 tool call）：跑生成的代码、分析一个 CSV、返回答案、把电脑扔掉。在早期的 agent 里，sandbox 是一台短命的机器：「agent 按需把它拉起来，在里面跑点代码或任务，然后关掉。」

随着模型能力变强，「大家意识到可以把整个 agent 都搬进 sandbox」，给 agent 文件、网络访问和其他一切。Ivanda 说，最近大家又明白了，「如果能同时跑几千个这样的执行环境，就能做到真正——」录音在这里断了。session 介绍把三个阶段概括为「先是一次 tool call，然后是 agent 住的地方，再然后是一次一千台机器」。第二、三阶段的幻灯片没拍到。

## 8. 反转：问错了问题

![Which framework should I pick? Wrong question.](../../slides/10-13-1545-give-the-model-a-computer-the-one-thing/12.jpg)
*观众带着进场的问题，被划掉了。*

下一张拍到的幻灯片回应了观众进场时带着的问题。"Which framework should I pick?"（我该选哪个框架？）被划掉，下面是回答："Wrong question."（问错了。）talk 的这一段没有录到。

## 9. 真正把你锁住的是什么

![Swappable decisions vs. a commitment](../../slides/10-13-1545-give-the-model-a-computer-the-one-thing/14.jpg)
*哪些以后能换，以及那个换不了的决定。*

标题为 "What actually locks you in" 的幻灯片把决定分成两类：

| 以后可以换 | 你得一直带着 |
| --- | --- |
| **框架**：一次重构，团队经常这么干 | **执行姿态（execution posture）**：host、in-process，或另一台电脑 |
| **模型和 prompt**：预计要重新调一些 | → 决定你写的每一个 tool |
| **缺的功能**：eval、tracing、memory：接一个工具就行 | → 成为你的安全和多租户方案 |
| **Sandbox provider**：它们说的都是同样的五行 | → 现在跳过，以后一次事故会把它补上 |

「同样的五行」呼应了开头的小测验：创建 sandbox、跑代码、删掉它，不管用哪个框架或 provider 看起来都一样。

## 10. 先选边界，再选框架

![Pick the boundary before the framework](../../slides/10-13-1545-give-the-model-a-computer-the-one-thing/16.jpg)
*"Frameworks are opinions. The boundary is a commitment."*

收尾的信息是："Pick the boundary before the framework. Frameworks are opinions. The boundary is a commitment."（先选边界，再选框架。框架是观点，边界是承诺。）

录音恢复时，Ivanda 正在给建议。做一个产品时，先把执行姿态定义成一个你自己掌控的接口，「除此之外的一切将来都能轻松替换」。有了这个接口，就用第三种架构，「给你的 agent 一台真正的电脑」。做对了，「执行相关的安全问题就解决了」，团队可以专心做产品功能。

![Daytona: Thank you](../../slides/10-13-1545-give-the-model-a-computer-the-one-thing/17.jpg)
*三个二维码：集成指南、插件和包、完整对比。*

最后一页有三个二维码，分别标着 "integration guides"、"plugins & packages" 和 "the full comparison"，还有账号 @ivanda_mislav。Ivanda 说这些集成和指南都是开源的，第三个二维码通向一篇更深入对比 agent 框架的文章。

---

**链接：** [日程](https://ai.engineer/nyc/2026/schedule?session=8d424512-c7f0-483c-9094-ca899d7bb1fd) · [The 7 Best AI Agent Frameworks: Features and Tradeoffs](https://www.daytona.io/dotfiles/best-ai-agent-frameworks)（Mislav Ivanda，Daytona）· [Claude Code sandboxing 文档](https://code.claude.com/docs/en/sandboxing) · [bubblewrap](https://github.com/containers/bubblewrap) · [OpenAI Agents SDK: Sandbox agents](https://openai.github.io/openai-agents-python/sandbox_agents)

这场 session 没有公开视频。本文依据作者的现场录音（开头和中间有缺口，这些部分只依据幻灯片照片）、幻灯片照片和上面列出的来源写成；Ivanda 的对比文章是讲者本人写的，不是独立来源。录音在「Thank you」处结束，没有 Q&A。[精确笔记](../precise/10-13-1545-give-the-model-a-computer-the-one-thing.md)（中文）保留了时间戳，以及哪些说法能核实、哪些不能的完整记录。
