**English** | [中文](zh/10-12-1105-agent--model--harness-build-your-own-mem.md)

# You Can't Change the Model, So Build the Harness: An Anti-Money-Laundering Agent From Memory Up

*At AI Engineer NYC 2026, Oracle's Nacho Martinez argued that agent quality is won in the harness around a frozen model (its memory, context management, retrieval of tools and skills, and loop), and walked through a workshop that builds one for an autonomous agent flagging suspicious bank transactions.*

**Workshop · Monday, October 12, 2026 · 11:05am–12:05pm · Sutton Room (NY Ballroom W)**
**Speaker:** Nacho Martinez, AI Developer Advocate, Oracle

---

The session's description set out the premise in one line: "Agent = Model + Harness. The model emits tokens; state, memory, retrieval, tool dispatch, identity, retries, and budgets. All these concepts are harness code — and that is where agent quality is won or lost."

Nacho Martinez, by self-description "an AI developer advocate and a data scientist by heart," had an hour and planned to talk for 10 or 15 minutes before attendees worked through the exercise themselves. Attendees who registered a GitHub handle at workshopwaitingroom.com ("I paid too much money for that domain. So, use it") were added to the workshop repository and could run it in GitHub Codespaces, at Martinez's expense. In the end the talk and questions filled almost the whole hour. "I thought like 15 minutes or 20 minutes went by, but one hour went by."

Martinez mentioned a recent free DeepLearning.AI short course, Building Adaptive AI Agents, which covers how a model can "adapt over time, learn over time," one part of what the workshop touched on. (Oracle's announcement lists Martinez and Casius Lee as instructors; Martinez credited the course to Andrew Ng and DeepLearning.AI.)

## 1. The use case: a bank that never sleeps

Told that many attendees worked on Wall Street, Martinez built the workshop around a bank. The motivation came from frontier labs' headlines about launching thousands of agents to attack long-unsolved math problems with no human in the loop. Martinez said agents can't do that alone: "the context window is limited," and memory "is something that a large language model doesn't have inherently." Engineers have to build those parts. "We don't need to focus so much on the frontier models … let's work on the harness, which is the interesting part."

The scenario is a mid-sized bank whose suspicious transactions fall into five types. Martinez's example: a $1 million deposit followed by three days of $50,000 ATM withdrawals. The agent's job is to decide on such transactions, or at least flag them, so the bank's staff spend less time on each. The session description adds the details of the dataset: a fictional Meridian Bank with 25 branches and about 1,200 transactions over 90 days, running on Oracle AI Database.

## 2. The agent stack, and where engineers have control

Martinez described a stack of five layers: the application (a chat interface or mobile app), the model ("the reasoning core," where "we don't get any say"), the data layer, infrastructure and compute. The workshop focused on data, because "everything that we can manipulate and work around is on this part."

The data layer holds the agent loop ("arguably one of the most important parts"), a gateway and MCP for outside data sources, the memory layer, context engineering that summarizes, compacts and shrinks the working context, and tools and skills.

AI applications, Martinez said, take four shapes: LLM chatbots, RAG ("essentially like a glorified vector search"), LLM-driven workflows and AI agents. Tools such as Claude Code, Codex, Pi, OpenClaw and Hermes Agent combine the last two: workflows supply automation, agents supply autonomy, and the combination is an agentic system. Engineers use these systems every day, Martinez said, but rarely design their own applications that way.

## 3. What an agent is, and what the harness is

Martinez compared an agent to a person. The LLM supplies reasoning, like a brain. A database supplies memory. Tools are its limbs for acting on an operating system. Inputs are its senses. Its context is limited the way attention is: "we can't watch seven movies at the same time."

Reasoning comes from the frontier labs. Memory, tool use and perception are what engineers control, "and this is the harness itself": "all the scaffolding that you need to do to build … the tool use, the memory and the perception." Martinez called it "the most interesting topic in AI nowadays," because a different configuration makes the same frontier model better or worse at a task.

The harness has "seven different or eight different components if you consider continual learning," each a decision:

| Component | The decision Martinez described |
| --- | --- |
| Model | Which model to call |
| Storage | Where the data lives |
| Encoding and retrieval | How to measure distance between vectors to retrieve the right data |
| Memory | Short-term plus long-term, or other compartments |
| Semantic layer | What both sides assume without saying it |
| Agent loop | What drives the agent, automatically and repeatedly |
| Context engineering | What enters and leaves the context window |
| Continual learning | How the system improves over time |

For the semantic layer, Martinez used the room: "I'm a speaker and you are listening. You are not singing right now. Why are you not singing? Because it's not appropriate."

## 4. Files versus databases

On the debate between putting everything in files and the "database maximalists," Martinez showed several agents writing to the same file and asked the room for today's workaround. An attendee answered: worktrees. Files, Martinez said, have an inherent problem with concurrent writes: which write came first, and what happens to someone who read the file before it changed. "The work tree … workaround has been already solved by databases 30 years ago": with atomic, consistent, isolated and durable (ACID) writes, the conflict wouldn't arise. Files are good for large amounts of unstructured data, databases for other jobs. "We don't have to go into only one of them."

## 5. Memory: short-term, episodic, procedural, semantic

Agent memory, Martinez said, covers the mechanisms that let an agent use information, refine it and "recall it appropriately when it's time," and engineers choose its categories. The workshop used this split:

| Memory | What it holds |
| --- | --- |
| Short-term | The context window itself; it vanishes when the conversation ends |
| Long-term: episodic | What happened: conversations, tool logs, summaries of earlier conversations |
| Long-term: procedural | How to do things: skills, and workflows as sequences of skills |
| Long-term: semantic | Who you are, your preferences, your favorite framework |

Everything can't go into short-term memory, Martinez argued, because it is limited and because "the bigger the attention matrix is … the worse it performs." A problem is solved "much more accurately" starting from zero tokens than with 256,000 already in context, and beyond a point "the performance goes horribly." (The claim matches widely reported context degradation, but Martinez stated it as holding for every model and every problem without citing a study.)

There are two ways to keep context small. Context summarization asks the model for a short summary and starts the next conversation from it. Compaction stores chunks of the conversation in a database or file under an ID, which the agent can fetch later with an offset, "kind of like reading from a file."

## 6. The agent loop

The agent loop is "the driver." Context assembly pulls in skills, tools, earlier conversations, files and database records. The model reasons through an API call and decides what to do. Then the loop repeats, "automatically and infinitely if you want." Martinez's workshop loop has a "maximum patience" of eight iterations and 60 seconds of reasoning each, partly to protect Martinez's own API credits, and all of it is configurable; the description calls it "a bounded agent_turn loop."

In the workshop notebook, context assembly retrieves the three most relevant tools and three most relevant skills (k = 3 against two vector stores) for each request. Martinez skipped a reranker but suggested adding an embedding model plus reranker to pick the single best of the three, since "the task might be solvable by only one."

## 7. The toolbox and skillbox patterns, and skill distillation

Martinez had about a hundred skills installed, each "like 3,000, 5,000 tokens." Loading them all would exhaust the context before the conversation began. The workshop's answer is to embed tools and skills as vectors, retrieve only the most relevant by similarity search when the loop needs them, then unload them. Martinez estimated that tools and skills can take "more than 50% of the context window" in some cases.

Skill distillation turns past conversations and runs into something reusable. The simplest form writes a SKILL.md file into the same skillbox. Martinez called Hermes Agent "one of the innovators" here. A skill that captured front-end color preferences, for instance, gets picked up automatically next time the user mentions "coloring style," because its embedding matches. Together these give dynamic skill discovery: the loop pulls the right skills and tools as it assembles context.

## 8. How a frozen model gets better

"How does a frozen model get better?" Engineers can't change frontier weights, but they control the data layer. Martinez divided continual learning into three kinds:

| Kind | How it works |
| --- | --- |
| Parametric memory | Changes the model's weights, e.g. low-rank adaptation or fine-tuning |
| Representational memory | Works through embeddings, e.g. a better embedding model for the problem |
| Non-parametric memory | Everything above: memory, retrieval, skills, context |

The workshop covered only the third; the course covers all three.

## 9. Instruction hierarchy

Every chat-completions request, Martinez said, carries "an inherent instruction hierarchy." The system prompt weighs most and the user's request comes third. Tools and skills, attached at the bottom, weigh least. Putting everything in the system prompt would flatten that hierarchy. A slide shown later broke down a typical request:

| Part | Typical size Martinez gave |
| --- | --- |
| System prompt | 5K–10K tokens |
| Developer (app instructions) | — |
| User query | "negligible," 100 to 1,000 tokens |
| Tool outputs and retrieved documents | "80% of the request," 35,000–100,000 tokens |

## 10. The workshop

The workshop interface imagines an anti-money-laundering analyst working with an agent that runs in the background around the clock ("It doesn't sleep"), connected to a database through the harness. It makes decisions autonomously but stops short of acting on them: "It's not ruining people's lives" by freezing accounts. Instead it records its judgments for a human to inspect, saving time in the discovery process.

In the demo, the deployed application generated a stream of fake transactions, some fine and some problematic. A Grok 4.3 model running on Oracle Cloud Infrastructure decided which to add to a suspicious list. The interface showed the agent's context window, the customers and other data in the database, a live analysis in which the agent called tools from its toolbox, searched knowledge stored in vector stores and produced suspicious activity reports, and the 30 most recent events at the bank. The exercise itself is a "student" notebook with to-dos, starting with connecting to the model and the database.

## Q&A

Questions came during the demo; most were hard to hear on the recording and are given as Martinez restated or answered them.

### Memory design

**Does episodic memory get confused with earlier conversations?** It depends on how much weight you give it, which is why the instruction hierarchy matters. Where episodic memory is important, "you might actually want to vectorize that rather than just save it as raw text." In the workshop there is no conflict "because we have a clear separation on the memory … components of the architecture."

**What's the most effective way to work with the memory layer?** "Keep it simple." Start with just short-term and long-term memory. When something in the context window seems worth keeping, "promote it to the long-term memory" as raw text. Later, vectorize it and add a "recall long-term memory" step between observing and reasoning in the loop: memories close enough to the query in vector space enter the context; distant ones are discarded.

### Skills, tools and the context budget

**Can skills go in the system prompt?** They can, but skills "have the least weight out of the four" levels of the hierarchy, and Martinez treats them as "a plug and play place." Martinez and colleague Richmond were experimenting with precomputing embeddings for third-party "skill hubs and tool hubs," such as a collection of MCP servers, so an agent can search them. Tools and skills typically make up about 80% of every request, Martinez said. Requests don't cost "four or five times" more, by Martinez's account, because providers keep "a semantic cache" that skips recomputing content repeated from the previous request. (OpenAI's documentation describes its prompt caching as exact prefix matching, "the entire rendered prefix" must match, rather than semantic similarity.)

**With so much in the toolbox and skillbox, how do you manage the context budget?** A context window never starts at zero; it is "closer to like 10,000." Martinez cited a comparison of how much each harness sends in its first request: Pi, "the most minimalistic and the most efficient," about 6,000 tokens; others "more like 30, 40,000." (Independent measurements vary widely by version and installed tools, and several put Pi's system prompt and tool definitions under 1,000 tokens; the comparison Martinez cited couldn't be found.) The workshop handles it by letting the agent loop control everything: the model first decides what the request needs, then the loop assembles context with the three most relevant tools and skills.

**Why embed skills instead of keeping them as files?** Files can only be searched by regular expressions or keywords, which get slower as files grow. A precomputed embedding of "384 minimum to a 1,000 plus dimensions" captures meaning that keyword search misses. Combining the two, hybrid search with reciprocal rank fusion (RRF), is better still. A plot in the notebook showed the band between keyword and semantic results where hybrid retrieval beats either alone: "a little bit more compute, but then the quality of the retrieval will be better."

### Databases and custom harnesses

**What does MCP solve?** The workshop doesn't use MCP. In a database setting, Martinez said, MCP can stop agents from inventing SQL: with "like 15 engines" and dialect subtleties, a generated query often fails. Pre-written, proven queries exposed through MCP avoid that.

**Should you build your own harness?** Martinez didn't expect anyone to build a new Claude Code; a show of hands for Pi came in "less than I expected." The point is to build small custom harnesses for specific problems. A consultant might build one for a financial client and another for a client with different data or constraints. An agent working an unsolved math problem with no human in the loop needs automatic compaction and "infinite or near infinite patience," unlike the workshop's eight-step loop. "I would start from scratch, because that way you … get to strip 90% of what you don't need." Everything is built around the chat-completions API, so treating the model connection as one plugin of the harness makes it clear "that the smaller … everything is, the better it is at the end."

Martinez then sent attendees to their codespaces and stepped down to help. Martinez offered to keep paying for the codespaces so attendees could finish later, and pointed to the Oracle booth, open through Wednesday evening.

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=898cb76b-41f3-47c0-ba31-6a5f8df40117) · [Video](https://www.youtube.com/watch?v=KtBivPzmOM8) · [Talk page](https://ai.engineer/talks/KtBivPzmOM8) · [Workshop registration](https://workshopwaitingroom.com/) · [Building Adaptive AI Agents (DeepLearning.AI)](https://www.deeplearning.ai/short-courses/building-adaptive-ai-agents/) · [Oracle announcement of the course](https://blogs.oracle.com/developers/we-just-launched-a-new-course-on-how-to-build-adaptive-ai-agents) · [OpenAI prompt caching docs](https://developers.openai.com/api/docs/guides/prompt-caching)

This write-up is based on the official video captions, the session description and the sources above; there are no slide photos for this session, so slides and the demo are described from what Martinez said. The video ends when attendees start the hands-on exercise. The [precise notes](precise/10-12-1105-agent--model--harness-build-your-own-mem.md) (Chinese) keep timestamps and the claims that couldn't be verified.
