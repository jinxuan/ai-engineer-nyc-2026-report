**English** | [中文](zh/10-13-1545-give-the-model-a-computer-the-one-thing.md)

# Pick Where Agent Code Runs Before You Pick a Framework

*At AI Engineer NYC 2026, Daytona's Mislav Ivanda argued that agent frameworks which agree on almost nothing have converged on one primitive, giving the model a computer, and that the execution boundary is the one decision teams can't easily undo.*

**Expo Session · Tuesday, October 13, 2026 · 3:45–4:03pm · Sutton Room (NY Ballroom W)**
**Speaker:** Mislav Ivanda, Developer Experience Engineer, Daytona

---

Mislav Ivanda spent a year building integrations between Daytona, which runs sandboxes for AI agents, and agent frameworks such as LangChain, CrewAI, Google ADK, Mastra, Pydantic AI and the OpenAI Agents SDK. According to the session description, the lesson was that the frameworks "all converge on one identical primitive: give the model a computer," that there are only three ways to do it, and that "two of them stop holding once real users show up."

The talk was a 16-page slide deck served from a local web page. The author's room recording misses the opening minutes and about four minutes in the middle; those parts are described from the slides alone.

## 1. A quiz: which framework is this?

![A quiz: Which framework is this?](../slides/10-13-1545-give-the-model-a-computer-the-one-thing/03.jpg)
*The opening quiz: a code-execution tool with nothing framework-specific in it.*

The second slide asked the audience to identify a framework from this code:

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

Apart from a generic `@tool` decorator, the function only creates a Daytona sandbox, runs the code in it, deletes the sandbox and returns the result. Nothing in it identifies a framework. How Ivanda revealed the answer was not recorded.

## 2. Frameworks agree on nothing, except this

![Frameworks agree on nothing. Except this.](../slides/10-13-1545-give-the-model-a-computer-the-one-thing/04.jpg)
*Three ways the frameworks differ, and one thing they share.*

The slide listed three ways the frameworks disagree:

| What an agent is | The language | Who drives |
| --- | --- | --- |
| graph vs crew vs loop vs product | Python vs TypeScript | you write the flow vs the model decides |

Beneath it: "They disagree about what an agent even is. And then they all ship the same primitive."

The recording picks up mid-sentence here. "For LangGraph you explicitly define your agent workflows and trajectories, while for example in CrewAI agents do things on their own and they decide on their own to achieve the end goal," Ivanda said. When companies that "don't agree on basically anything agree on something, that probably isn't just a fashion, it's a necessity."

Ivanda's explanation: any agent doing something useful eventually needs to execute code or get shell access, "and the next question is where will this happen, in which execution environment." There are, Ivanda said, "basically three types of possible isolation."

## 3. Answer No. 1: on your own machine, guarded by permissions

The slides for the first answer were not photographed, but the recording covers it. This is the most basic approach, "probably the one you're using locally when developing with your agent": an allow list of commands, approval prompts asking whether you're sure you want to run a command ("although probably most of us just click yes"), and interception hooks.

The problem, Ivanda said, is obvious. Allow an agent to run `python -c` and it can "open any sockets, modify your file system, execute code." "You essentially defined what your model should use, but you didn't define what your model can actually use." If the model is compromised, the approach effectively opens a back door on the developer's machine. (Ivanda's own comparison article calls this "a permission model, not containment," which is why the talk's label of "isolation" fits it loosely.)

## 4. Answer No. 2: in an interpreter inside your process

![Answer No2: In an interpreter, inside your process](../slides/10-13-1545-give-the-model-a-computer-the-one-thing/07.jpg)
*The second answer: "It isolates the glue, not the tools."*

The second slide listed the approach's features: QuickJS or V8 isolates, no filesystem and no network, and tools bridged to the host. Its verdict: "It isolates the glue, not the tools."

Running agent-generated code inside an embedded runtime such as V8 or QuickJS is better than the first answer, Ivanda said, because it gives network and filesystem isolation. But the flaw is subtler. Suppose the agent has tools like "query database" or "send email." It writes a script that calls them, and the script runs inside the runtime, "but the problem is that these tools that your agent will call inside that runtime will actually run on your device outside of that runtime." A malicious agent can act through the tools rather than directly: "you isolate the glue code that your agent generates, but you're not actually isolating the actual tool calls." The agent can use the email or database tools themselves to carry out malicious commands.

## 5. Answer No. 3: on a separate computer

![Answer No3: On a separate computer](../slides/10-13-1545-give-the-model-a-computer-the-one-thing/08.jpg)
*The third answer: "A compromised step is not a compromised app."*

The recommended approach, Ivanda said, is "a separate computer or VM." The slide listed: a container or microVM, reached over an API, that can't reach your infrastructure. The application talks to it through an API and controls its lifecycle, and it "never touches or interferes with your local infrastructure, network, file system."

The key is the slide's last line: "A compromised step is not a compromised app." In the first two approaches, a bad step, such as a Python command a developer approved, damages the application itself. Here, "you can run the worst code on the planet inside the sandbox and it can't interfere with your application logic. You can just throw out that sandbox and spin up another one."

## 6. Receipts: the frameworks already voted

![The frameworks already voted](../slides/10-13-1545-give-the-model-a-computer-the-one-thing/10.jpg)
*Five frameworks moving away from local code execution.*

"To back up this, that I'm not just talking some nonsense, here's the proof," Ivanda said, and showed the direction frameworks are moving:

| Date | Framework | What the slide says |
| --- | --- | --- |
| Apr 2026 | CrewAI | Removed CodeInterpreterTool; deprecated `allow_code_execution`. Docs now point to a dedicated sandbox service. |
| Jul 2026 | AG2 1.0 | Retired the `autogen.coding` executors. The new SandboxCodeTool has no default backend: the environment is a required argument. |
| | Claude Agent SDK | Sandbox is Bash-only, off by default, fails open. Official guidance for untrusted code: "a dedicated VM." |
| | OpenAI Agents SDK | Ships SandboxAgent with pluggable local, Docker, and hosted sandbox clients. |
| | Mastra | "You must choose its execution boundary deliberately." Host, isolate, or external, stated plainly in the docs. |

The slide's conclusion: "They're not adding sandboxes. They're deleting the alternative."

Ivanda walked through the rows. CrewAI removed the tool that ran a code interpreter locally and now points to dedicated sandbox services; AG2 retired its old local executors, and its new tool requires the developer to name the environment explicitly. The Claude Agent SDK's default sandbox works at the process level, using a tool such as bubblewrap on Linux. The OpenAI Agents SDK supports sandbox providers and Docker natively but still allows local execution, and Mastra is similar. "They already had them," Ivanda said, "but they are deleting all the alternatives."

Some of these claims can be checked against documentation and some cannot. Ivanda's comparison article gives the same account of CrewAI (the tool removed from `crewai-tools`, and `allow_code_execution` and `code_execution_mode` deprecated ahead of removal in v2.0), of AG2 1.0 (`autogen.coding` moved out of the main package), and quotes Mastra's documentation directly. Claude Code's sandboxing documentation confirms that the sandbox is off by default, uses Seatbelt on macOS and bubblewrap on Linux, and by default continues without sandboxing if it can't start; it describes the sandbox as covering shell commands (Bash, PowerShell and Monitor), not only Bash, while file tools, MCP servers and hooks run outside it. A first-hand CrewAI record of an April removal, and the source of the "dedicated VM" quotation, could not be found; the only CrewAI changelog entry located is a September 2026 documentation change removing the tool from examples.

## 7. The primitive keeps growing

![First, it was a tool call](../slides/10-13-1545-give-the-model-a-computer-the-one-thing/11.jpg)
*Stage one of three: the sandbox as a disposable tool.*

The way sandboxes are used has also changed, Ivanda said. The slide, marked 1 of 3, read "First, it was a tool call": run generated code, analyze a CSV, return the answer, throw the computer away. In early agents, the sandbox was a short-lived machine: "your agent would spin it up on demand, run some code or task inside it and shut it down."

As models became more capable, "people realized they could literally shift all the agents inside the sandbox," giving the agent files, network access and everything else. And recently, Ivanda said, people have understood that "if they can run thousands of these execution environments concurrently, they can do really—" and the recording breaks off there. The session description summarizes the three stages as "first a tool call, then the place the agent lives, then a thousand machines at once." The slides for stages two and three were not photographed.

## 8. The twist: the wrong question

![Which framework should I pick? Wrong question.](../slides/10-13-1545-give-the-model-a-computer-the-one-thing/12.jpg)
*The question the audience arrived with, struck through.*

The next captured slide addressed the question the audience came in with. "Which framework should I pick?" was struck through, with the answer beneath it: "Wrong question." This section of the talk was not recorded.

## 9. What actually locks you in

![Swappable decisions vs. a commitment](../slides/10-13-1545-give-the-model-a-computer-the-one-thing/14.jpg)
*What can be changed later, and the one decision that can't.*

The slide titled "What actually locks you in" divided decisions in two:

| You can swap later | You live with |
| --- | --- |
| **The framework**: a refactor, teams do it all the time | **The execution posture**: host, in-process, or a separate computer |
| **The model & the prompts**: expect some re-tuning | → shapes every tool you write |
| **Missing features**: evals, tracing, memory: plug in a tool | → becomes your security & tenancy story |
| **The sandbox provider**: they all speak the same five lines | → skipped now, an incident adds it later |

The "same five lines" echo the quiz at the start: creating a sandbox, running code and deleting it look the same whichever framework or provider is used.

## 10. Pick the boundary before the framework

![Pick the boundary before the framework](../slides/10-13-1545-give-the-model-a-computer-the-one-thing/16.jpg)
*"Frameworks are opinions. The boundary is a commitment."*

The closing message: "Pick the boundary before the framework. Frameworks are opinions. The boundary is a commitment."

When the recording resumes, Ivanda was giving the advice. When starting a product, define the execution posture as an interface you control, and "everything except that can be easily swappable in the future." Once that interface exists, use the third architecture, "giving your agent a proper computer." Done right, "the security part regarding the execution is solved," and the team can get on with building product features.

![Daytona: Thank you](../slides/10-13-1545-give-the-model-a-computer-the-one-thing/17.jpg)
*Three QR codes: integration guides, plugins and packages, and the full comparison.*

The final slide carried three QR codes, labeled "integration guides," "plugins & packages" and "the full comparison," and the handle @ivanda_mislav. Ivanda said the integrations and guides were open source, and that the third code led to an article comparing agent frameworks in more depth.

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=8d424512-c7f0-483c-9094-ca899d7bb1fd) · [The 7 Best AI Agent Frameworks: Features and Tradeoffs](https://www.daytona.io/dotfiles/best-ai-agent-frameworks) (Mislav Ivanda, Daytona) · [Claude Code sandboxing documentation](https://code.claude.com/docs/en/sandboxing) · [bubblewrap](https://github.com/containers/bubblewrap) · [OpenAI Agents SDK: Sandbox agents](https://openai.github.io/openai-agents-python/sandbox_agents)

No video of this session has been published. This write-up is based on the author's room recording, which has gaps at the start and in the middle (those sections rely on slide photos only), slide photos, and the sources above; Ivanda's comparison article is the speaker's own writing, not an independent source. The recording ends at "Thank you" with no Q&A. The [precise notes](precise/10-13-1545-give-the-model-a-computer-the-one-thing.md) (Chinese) keep timestamps and the full record of which claims could and could not be verified.
