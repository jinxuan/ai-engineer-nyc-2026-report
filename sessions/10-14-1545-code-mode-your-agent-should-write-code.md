**English** | [中文](zh/10-14-1545-code-mode-your-agent-should-write-code.md)

# Stop Handing Your Agent a Tool Menu; Give It a Sandbox

*At AI Engineer NYC 2026, Arize AI's Mikyo King argued that agents should write code against their tools instead of calling them one at a time, described how Arize built "code mode" into its Phoenix agent, and was candid about the costs, before reporting 43% fewer tokens and 31% lower cost in Arize's own evals.*

**Breakout · Wednesday, October 14, 2026 · 3:45–4:03pm · Turing Room (Riverside Ballroom) · Evaluations Track**
**Speaker:** Mikyo King, Head of Open Source, Arize AI

---

Mikyo King, who said they "head up the open source group at Arize AI," works on Phoenix, "a fully open source observability and evaluation framework," and on Arize's own agent. The talk set out to explain code mode and "our journey embracing" it. (The published video was retitled "Stop Giving Your Agent Tools. Give It a Sandbox.")

King started with history. For 30 years, software split into two kinds: tools for engineers and tools for everyone else. Asked to pull every invoice out of an inbox, a programmer writes a script, perhaps "50 lines of code," that checks each message for an attachment, downloads it and files it. A non-programmer downloads them by hand, "restricted by the interface of the software," or buys software to do it. "The thing that's really interesting about things like Claude Code is that that boundary is completely dissolving," King said: a coding agent can build whatever software you need on the spot. The model labs know it: "If you look at all the benchmarks like SWE-bench, that is what these models are being trained to do." So the agents you build should be able to do the same.

## 1. The too-many-tools problem

![The too many tools problem](../slides/10-14-1545-code-mode-your-agent-should-write-code/01.jpg)
*Tool definitions from six MCP servers fill most of the context window before any work happens.*

The familiar agent design gives a model "a huge catalog of tools." A user question arrives, the model calls a tool, the result is pushed into the context window, the model reasons and calls another. The slide showed this as a context-window bar mostly filled with tool definitions, followed by a user question and alternating tool calls and results, while the model reached out to six MCP servers: CRM, Meetings, Email, Calendar, Docs and Tasks.

With a modest set of tools, King said, this "can work pretty well." But teams now want more emergent behavior, more context and longer horizons, and the context bloats: tool definitions irrelevant to the task, tools that return junk, data that has to be paginated. "You deal with this kind of context rot. You also deal with multi-turn, a lot of turns, as a result, and you're going to have to deal with compaction."

## 2. What code mode is

Code mode is "an alternative architecture," King said. Instead of a menu of every tool, the agent gets the ability to search for the tool it needs and to execute it, and execution happens in "a programming environment," so tools can run in sequence or in parallel, with conditionals around them. "You can call multiple tools in one turn, and also return only what's necessary to push back into the context window of the model."

![The model programs the tools](../slides/10-14-1545-code-mode-your-agent-should-write-code/02.jpg)
*One request, answered with one short program that calls four of five available tools.*

The slide's example request was "Attach today's Acme call to the Acme deal in our CRM." The model writes a program that runs in a sandbox:

```python
call = find_meeting("Acme", "today")
text = get_transcript(call.id)
deal = find_deal("Acme")
attach(deal.id, text)
return "attached"
```

Four tools are used; a fifth, `search_email()`, sits unused beside them and never enters the context.

![Tool calling vs. code mode](../slides/10-14-1545-code-mode-your-agent-should-write-code/03.jpg)
*An illustrative comparison; King called both sides "hypotheticals."*

The comparison slide put the two side by side:

| | Turns | Context used | Cost |
| --- | --- | --- | --- |
| Tool calling (one call at a time) | 8 | 59% | $0.59 |
| Code mode (one program in a sandbox) | 1 | 2% | $0.02 |

King introduced these as "hypotheticals," not measurements. With tool calling, context grows with every call, adding latency, cost and turns, so "the time your user takes to get to value increases." Code mode, "when done well," can call as many tools as it needs in one turn and put only what is needed into the context. Arize's measured results came later in the talk.

## 3. Why code mode works

![Why code mode works](../slides/10-14-1545-code-mode-your-agent-should-write-code/04.jpg)
*Four reasons, each illustrated: progressive disclosure, composability, token efficiency, emergent behavior.*

King gave four reasons.

**Progressive disclosure.** The agent doesn't need the full catalog up front; "it can just pick and choose the tools that it needs." The slide showed a grid labeled "every tool" with only two tiles reaching the context. Foundation model providers now build tool search in as well.

**Composability.** Tools can be called in sequence or in parallel, fanned out or chained. The slide's code looped over meetings, fetched each transcript, and attached a summary to a lead only if the transcript mentioned pricing, annotated "loop, call, branch, chain." King pointed to Claude's dynamic workflows as built on the same idea, applied to sub-agents.

**Token efficiency.** Tool output need not be dumped into the context; "the model can dynamically kind of trim and search for certain areas and only put what it needs back." On the slide, transcripts stay in the sandbox and only the answer goes to the context.

**Emergent behavior**, "one of the major reasons we've embraced code mode": the agent can build its own tools. Asked to "take all these records and remove personally identifiable information," a tool-calling agent either has no tool for it or tries to do it in its context window, "but then you're leaking credit card numbers in the context window, and that's not great." The slide showed a function "written by the model," `redact_pii`, which replaces card numbers using a regular expression and joins the toolbar next to `get_transcript` and `summarize`. King's blog post on code mode uses the same credit-card example.

## 4. How Arize built code mode into Phoenix

![Code mode in PXI](../slides/10-14-1545-code-mode-your-agent-should-write-code/05.jpg)
*PXI, the agent inside Phoenix, sends code to three execution surfaces on the Phoenix server.*

Arize builds its own agent and harness into Phoenix; Arize's documentation calls it PXI, "modeled after a coding agent, but pointed at Phoenix." The slide showed PXI sending code to the Phoenix server, which offers three execution surfaces:

| Surface | Example on the slide | Backed by |
| --- | --- | --- |
| `bash` | `gql … > spans.json; grep -c ERROR spans.json` | GraphQL API |
| `execute` | `bad = [r for r in runs if r["error"]][:3]` | REST API |
| `executeSql` | `SELECT name, count(*) FROM spans GROUP BY 1` | Database |

"We started with Bash," King said, a good way to combine Linux primitives such as sed and grep with Arize's own CLI and to save results to files for later. Next came a Python sandbox, into which Arize "hoisted all our REST CRUD endpoints," for fetching traces or editing datasets, so the agent could compose create, read, update and delete operations. Last came SQL. Not everything is CRUD: a question like "tell me all the LLM spans that have P95 latency of X, Y, and Z" needs an aggregate query, and "you don't want to have to paginate through all your traces to do that."

Arize described the SQL work in an August blog post. It gives the agent two read-only tools, `describeSqlSchema` and `executeSql`; in code mode the agent's Python runs in a restricted sandbox on the server and only the final return value goes to the model; SQL is parsed, limited to allowed tables and columns, regenerated and checked again by the database; and runtime, row counts and result sizes are capped. On eight telemetry questions run three times each, SQL plus code mode averaged about $0.23 a question against about $3.97 for retrieval-only tools, "about one-seventeenth the cost."

![Code mode in the browser](../slides/10-14-1545-code-mode-your-agent-should-write-code/06.jpg)
*PXI drives the Phoenix UI by writing a script that runs in a Web Worker.*

Arize also built "a programming paradigm in our UI that's bound to our UI." The slide showed Phoenix's Spans page for a project named `claude-code`, marked "driven by PXI," beside a PXI panel. Asked "Show me yesterday's failed tool calls," PXI searched the available browser actions, then ran a script in a Web Worker: `await ui.timeRange.set(yesterday)` and a spans filter for `span_kind == 'TOOL' and status_code == 'ERROR'`. The three steps across the bottom: write one `ui.*` script, run it in a Web Worker, drive the real app. Because the worker has "a two-way data binding directly into our browser," King said, "you kind of almost get this computer use like emergent behavior … it's really pretty fun to watch it cook."

## 5. What code mode costs

"Code mode is not obviously for free," King said. "You're dealing with LLM-generated code."

![LLM-generated code](../slides/10-14-1545-code-mode-your-agent-should-write-code/07.jpg)
*Four risks of running model-written code.*

The slide listed four risks:

| Risk | Slide illustration | What King said |
| --- | --- | --- |
| Resource limits | CPU, memory and time bars; time runs past the limit and the job is "killed" | Hosted sandboxes need managing for CPU, memory and how long they stay alive |
| Network egress | An "injected README" in the sandbox sends data to evil.com | "Networking is a huge one … you never know what's out in the web" |
| Secrets in the sandbox | `API_KEY=sk-ant…` plus `print(env)` puts the key in the logs | "Every sandbox provider is trying to come up with a different vaulting strategy"; above all, make sure "those secrets don't come back into the context window" |
| Over-broad permissions | An admin token allows "read logs" but also "delete prod" | "Typically permissions are overbroad": the first five steps may be fine and the sixth not, or each step fine but the combination not; add guardrails where needed |

In the secrets passage King also mentioned a recently shipped product for making fast decisions inside a sandbox, but the name is unclear in both the video captions and the room recording, so it is left out here.

![Operational complexity](../slides/10-14-1545-code-mode-your-agent-should-write-code/08.jpg)
*Four kinds of operational overhead.*

Operational complexity was the second slide of costs:

| Issue | Slide illustration |
| --- | --- |
| Startup cost | An isolate starts in milliseconds, a microVM in hundreds of milliseconds, a full VM in seconds |
| Harder to debug | Tool calling shows three checked steps (`get_spans()`, `filter()`, `summarize()`); code mode shows one `run_code()` that returns "summary: empty" |
| Tool discovery | Search (`search("traces")`), learn (`get_traces(project, limit)`), then call (`get_traces("prod")`) |
| Duplicating the harness | Tool search and code execution in the harness, and again in the MCP server |

Sandboxes "don't boot all the time perfectly," so failures must be handled. Debugging is harder "because now you're debugging code and you're not debugging individual tools," and "tool parameter accuracy becomes a much bigger issue because the tool parameter now is code." Tool discovery needs an extra handshake where the model once knew its tools up front. And code mode is being added both to MCP servers and to harnesses, creating "a double sandboxing problem": one sandbox serializes its output to JSON, which is parsed and fed into another code mode, "this like double duty."

## 6. Open problems

![Open problems](../slides/10-14-1545-code-mode-your-agent-should-write-code/09.jpg)
*Durability, determinism, images and binary data, and smaller models.*

King listed four open problems, adding, "they're not like blockers":

**Durability.** On the slide, today's run is a row of blocks with one red; a snapshotted run is all green. "Let's say you build a giant piece of code that needs to execute and it breaks in the middle. You can't resume from that step."

**Determinism.** The slide showed JavaScript producing different hashes (`a3f ≠ 9c1`) where Starlark produced the same. If determinism and math matter, the choice of language matters: "JavaScript is something LLMs are really good at doing, but maybe not great for math."

**Images and binary data.** A tool returning JSON passes cleanly to the code; an image arrives as a base64 string; a 2 GB file is "too large to pass." Code mode is good for manipulating images inside the sandbox, but getting them out is tricky, since serializing a whole image would land it in the context; "you might have to do some sort of signed image handoff."

**Smaller models.** "The frontier models are amazing at code mode," but swapping in a smaller model to save money may leave the capability short.

## 7. How Arize measured it

Arize evaluates its agent on its own platform and runs it through the Harbor framework, which "puts the agents into sandbox environments and runs them on clear instructions with network access turned off," repeating trials to "eliminate a lot of the flake." (Harbor is the evaluation framework from the Terminal-Bench team.) Evals are written as Harbor verifiers; "Harbor's sort of rooted in this RL framework. We give it a one or a zero" for each task, and Arize tracks pass@k per task, because a non-deterministic agent can take different trajectories on different runs.

![What code mode saves PXI](../slides/10-14-1545-code-mode-your-agent-should-write-code/10.jpg)
*A Phoenix experiment comparison of PXI before and after code mode.*

The results slide showed a Phoenix experiment comparison with four figures beside it:

| Metric | Change with code mode |
| --- | --- |
| Tokens | −43% |
| Cost | −31% |
| Reward | Same |
| Latency | +0.6% |

"Pre code mode and post code mode, we, you know, decreased the amount of tokens by 43%. The cost went down by 31%," King said; the sandbox added some overhead, "but the latency stayed pretty constant." The gains held "across these multiple surfaces, whether or not it was Claude Code or MCP or the CLI." The task set and settings for this experiment were not given, and these figures appear only on the slide and in the talk; King's September blog post on code mode does not include them.

![Improve the agent](../slides/10-14-1545-code-mode-your-agent-should-write-code/12.jpg)
*The improvement loop: run the evals, spot regressions, analyze the traces, tune the agent, re-run.*

The last technical slide showed the loop Arize uses: run the evals, spot regressions, analyze the traces, tune the agent, and re-run the same tasks. Screenshots under "Look at the data" showed a single trace (status OK, total cost $0.44, latency 2m 52s, model claude-opus-5, one call with 26,665 prompt tokens and 297 completion tokens, calling `mcp__phoenix__execute`); under "Tune the tools and instructions," a coding agent's terminal showed a table titled "Code mode, before + after (18 tasks, 2 runs after the change)" comparing reward, turns and prompt tokens across experiments, too small to read in the photo.

Having both trajectories and evals "is really a huge unlock," King said. Sometimes adding a new tool lowers efficiency at first; with the data, "we can tell a coding agent why in these specific scenarios, what parts of the instruction are guiding it in the wrong direction," and hill-climb the evals.

## 8. The case for code mode

![The case for code mode](../slides/10-14-1545-code-mode-your-agent-should-write-code/13.jpg)
*The closing slide: fewer turns, cheaper, smarter. "Sandbox it. Trace it. Improve it."*

The closing slide named three benefits, fewer turns, cheaper, smarter, and a slogan: "Sandbox it. Trace it. Improve it." "We really like code mode at Arize because of the fewer turns," King said. "We want you to be able to bring any model … so we want it to be cheaper, and we're really excited about the emergent behavior and the long horizon tasks that our agents can do. And so, I hope this talk convinces you to give your agent a sandbox and then use one of our tools to evaluate it."

The official session description also cited research with "bash-only agents beating typed tool catalogs by 21.8 to 24.5 points on the same benchmark while spending fewer tokens," which King did not mention on stage. The paper, "Is Bash All You Need?" (arXiv:2609.11999), compared five tool interfaces on two enterprise benchmarks using Opus 4.8 and GPT-5.5. The 21.8 to 24.5 point gain for bash-only over typed tools was on TheAgentCompany; on APEX-Agents it was 4.8 to 7.4 points. Bash-only used 19% to 72% fewer total tokens, and adding typed tools or saved self-made tools on top of bash brought no detectable gain. The paper compared bash against typed tools rather than code mode against tool calling, and Arize's own result was the same reward at lower cost, not a higher score.

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=8413d6ee-ceb1-4095-9f27-49d518f1dd5a) · [Video](https://www.youtube.com/watch?v=dAoOYFx7ZYw) · [Code mode: Why your agent should code](https://arize.com/blog/code-mode/) (Mikyo King) · [Arize Phoenix has a built-in MCP server that lets your agents query traces with SQL](https://arize.com/blog/phoenix-mcp-sql-code-mode/) · [PXI documentation](https://arize.com/docs/phoenix/pxi) · [Harbor](https://github.com/harbor-framework/harbor) · [Is Bash All You Need?](https://arxiv.org/abs/2609.11999)

This write-up is based on the official video captions, the author's room recording, slide photos (the title slide was not photographed) and the sources above. The talk ended without a Q&A. The [precise notes](precise/10-14-1545-code-mode-your-agent-should-write-code.md) (Chinese) keep timestamps and the full list of transcription corrections.
