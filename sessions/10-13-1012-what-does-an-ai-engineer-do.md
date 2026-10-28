**English** | [中文](zh/10-13-1012-what-does-an-ai-engineer-do.md)

# The AI Engineer's Job Is Now Building the Loop, Not the Feature

*At AI Engineer NYC 2026, Arize AI's Aparna Dhinakaran argued that AI engineers now build the systems of agents that write, review and monitor software, and walked through how Arize built Signal, an always-on agent that reads production traces, files issues and opens pull requests.*

**Featured Keynote · Tuesday, October 13, 2026 · 10:12–10:30am · Simons Room (Metropolitan East), Main Stage**
**Speaker:** Aparna Dhinakaran, Co-Founder & Chief Product Officer, Arize AI

---

"Every year we have an idea of what an AI engineer does and that definition kind of changes," Aparna Dhinakaran said, opening a talk that revisited the question in the conference's name. The host framed it as a question about what is left for engineers "as AI takes on more of the building," and the session description promised an argument that defining "good" is the core skill that remains. On stage, Dhinakaran spent most of the 15 minutes on a concrete case: how Arize built an agent that runs the build-and-improve loop for its users.

## 1. Karpathy's funnel

![1000x Agent Engineer](../slides/10-13-1012-what-does-an-ai-engineer-do/01.jpg)
*A four-tier funnel from Andrej Karpathy's post, with Dhinakaran's own reply.*

The timing was good, Dhinakaran said, because Andrej Karpathy had "just tweeted something this weekend" about a growing gap between most people and those at the frontier "running thousands of agents and swarms of always-on agents." The slide, titled "1000x Agent Engineer," reproduced the funnel:

| People | What the slide says |
| --- | --- |
| ~6B | (~75%) have barely touched an LLM |
| ~1–2B | (~20%) casual, free-tier users |
| ~20M | (~0.2%) professionals in math and code, where an agent finishes weeks of work from a prompt |
| ~5K | (~0.00006%) with internal frontier access, running swarms of thousands of agents |

Beside it was a screenshot of the post, in which Karpathy revived an April message because the gap in understanding of LLM capability was "widening" and now looked "more a sharp funnel" than two groups talking past each other. Coverage of the post dates it to October 11 and notes that Karpathy called the numbers "napkin math." Under it was Dhinakaran's reply: labs are "tackling a subset of problem types (those with data to solve and testable) and feeling the awesome power. Will the pace of labs show up in business?"

"The first question anyone looks at this is, where am I on this?" Dhinakaran said. The post amounted to a road map for how everyone might, in the next few years, become one of the 5,000.

## 2. Rising abstraction

![How we got here](../slides/10-13-1012-what-does-an-ai-engineer-do/02.jpg)
*Five steps up in AI capability, from high-level languages to a factory of agents.*

| Period | Stage | Description |
| --- | --- | --- |
| 1970s | High-level languages | Frameworks and tests |
| 2022–24 | Chat assistants | Copy code in and out by hand |
| 2025 | Local agents | You approve each step |
| 2025–26 | Cloud agents | Work in the background |
| Now | Factory of agents | Thousands of agents running for hours |

The abstraction layer has been rising "for a couple of decades now," Dhinakaran said (the slide starts in the 1970s), and faster in the last few years: from high-level languages, to copy-pasting code, to local agents, to cloud agents. "The expectation now is how do all of us start running a team of agents or factories of agents."

## 3. Build the loop, not the feature

Dhinakaran then contrasted the job then and now (no photo was taken of that slide). "Your job's no longer to just write the feature, write an AI feature, it's about how do you build the loop to write that feature." Instead of reading the diff, the engineer sets up systems and agents to do code review and writes evals to review non-deterministic work. Instead of only setting up metrics and telemetry, the engineer has agents read the observability data and find patterns.

The loop is the familiar one, "the build, the eval, ship, you monitor, you improve it," and the later slides carried those five stages as tabs in the corner. The AI engineer is now responsible "not just for doing each of those steps, but how do you build the loop that … actually does each of those steps."

## 4. Signal reads the traces

![We built Signal to run this loop](../slides/10-13-1012-what-does-an-ai-engineer-do/03.jpg)
*Signal finds issues in traces, shows the evidence and links a fix.*

To run that loop for its customers, Arize built Signal. The slide listed three jobs, "Finds issues — in your traces," "Shows the evidence — the runs behind each issue," and "Links a fix — a pull request," next to a screenshot of Signal's issue view with counts over time. Signal reads the traces, identifies patterns in the telemetry, and "can actually put up a fix or a PR if it has access to the repo," Dhinakaran said. "Signal's an always-on agent."

![Signal starts with reading traces](../slides/10-13-1012-what-does-an-ai-engineer-do/04.jpg)
*Traces from what looks like a banking assistant, and three high-severity issues Signal found in them.*

The example on the slide was a table of traces from what appears to be a banking assistant ("Show me all transactions over $500 this month," "Apply for a personal loan of $10,000"), with three groups flagged by Signal and three issues, each rated High: "Says an action is done without calling a tool," "States balances it never looked up," and "Pre-approves without checking the profile." Signal reads a configurable volume of traces on a cadence and must remember what it has seen before: something that looked like an issue but wasn't recorded may turn out to be critical as more data arrives.

Arize's documentation describes Signal the same way: a worker that scans a project's traces on a schedule, groups recurring failures into issues and writes an investigation for each, with evidence and a suggested fix. According to the docs, opening pull requests from an attached GitHub repository is limited to the Enterprise plan; Free and Pro get issue detection only.

## 5. Agents become the readers of telemetry

![The future of telemetry](../slides/10-13-1012-what-does-an-ai-engineer-do/05.jpg)
*Three phases: the human reads, the human reads and prompts an agent, the agent reads directly.*

| Phase | Who reads observability data | Slide caption |
| --- | --- | --- |
| 1 | Human dev | The human is the only reader. |
| 2 | Human dev + agent | Human reads, prompts an agent to write. |
| 3 | Agents | The agent reads it directly. |

"The future of who's actually consuming telemetry data has really changed," Dhinakaran said. The question is now "how do you set up your telemetry data so that agents are the primary consumers of that data." That data also becomes the trigger that wakes always-on agents.

## 6. Inside a run

The first design question for a factory of always-on agents is what wakes them. Signal can run on a cadence ("every 3 hours, 6 hours, go pull the last … X amount of traces") or be triggered by events such as Slack messages, user complaints or metrics. (Arize's docs give a default of every six hours, adjustable from every three hours to monthly.)

![Anatomy of an agent run](../slides/10-13-1012-what-does-an-ai-engineer-do/06.jpg)
*The control plane, the sandbox's layers and what comes out. Part of the slide is hidden behind the video player's controls.*

The slide split a run into a control plane and a sandbox. On the left, a trigger ("cron, event, click") passes through an admission step ("quotas, caps, flags") before a job is created (that box is partly hidden in the photo). The sandbox, "isolated, ephemeral," is built in layers, with the most stable at the bottom: a base image (runtime, CLIs, skills), a layer of scoped keys (partly hidden), instructions (system prompt, permissions), a workspace (repo clone, scratch directory), run context (memory files, tool servers) and the task prompt. On the right, results leave through a tool call, every turn is traced, and the pod is reclaimed at teardown.

Every time Signal wakes, Arize builds a fresh sandbox holding the trace context, third-party data such as APM logs and metrics, repository access and the keys it needs. "There's a proxy, the only way to get information in and out of the sandbox is by this single proxy," Dhinakaran said. Everything inside is traced, and the issues it produces are saved to memory.

## 7. What stays and what goes

![What happens after each run](../slides/10-13-1012-what-does-an-ai-engineer-do/07.jpg)
*Kept on purpose versus thrown away when a run ends.*

| Kept, on purpose | Thrown away |
| --- | --- |
| Output: the answer or change it made | Sandbox: torn down after the run |
| Traces: every turn | Credentials: short-lived, revoked |
| Memory: files, not chat history | Workspace: repo clone, scratch files |
| New skills: saved as needed | Leftovers: caught by an idle sweep |

Issues the agent found are saved "so that the next time an agent gets spun up, it doesn't have to start from ground zero." The agent's own actions are traced, including whether it opened a PR, and skills it learned during a run "can also get saved to the actual repo." The sandbox, credentials and repository access are removed.

## 8. Durable agents

![How to keep the agent durable](../slides/10-13-1012-what-does-an-ai-engineer-do/08.jpg)
*When a step fails, a new sandbox and harness resume from saved state.*

If the sandbox or the harness fails mid-run, Signal "shouldn't have to start everything from scratch," Dhinakaran said. "It should actually just be able to pick up from the step that it failed." The slide showed steps 1 and 2 completing, step 3 failing, and a new sandbox and harness resuming at step 3 from "saved state, outside the sandbox": each finished step, the conversation, memory, and which step is next.

## 9. Model- and harness-agnostic

![Build model and harness agnostic](../slides/10-13-1012-what-does-an-ai-engineer-do/09.jpg)
*An eval set scores every pairing while knowledge, tools and controls stay put.*

Signal supports "not just Claude Code, which is what we started with, but it also supports Cursor and Codex," with the goal of supporting the major harnesses, including open-source ones, and any model in a family, for example any Anthropic model under Claude Code. (Arize's docs say Signal runs on Claude Code by default and that the choice among the three is an Enterprise option.) The slide showed what stays put, "knowledge in files" (skills, memory, instructions), "tools over MCP" (the same tools for any harness) and "controls" (sandboxes, permissions, review), while harness A or B and model A or B are swapped, each pairing scored by an eval set for quality and cost.

Dhinakaran gave two reasons. It is a way to evaluate the agent's quality each time it runs, and it controls cost: "can we reduce the cost and actually still maintain the rigor … and maintain the quality of issues that are actually detected by Signal."

## 10. Write evals, then evaluate the evaluator

"An AI engineer really needs to kind of learn how to write evals," Dhinakaran said. Each time Signal opens a PR, evals run on top of the agent to test it. An eval can be as simple as a dataset and a prompt with outputs checked for regressions.

![From assertions to evals](../slides/10-13-1012-what-does-an-ai-engineer-do/10.jpg)
*A support-agent dataset, a grading prompt and the judge's labels.*

The slide's grading prompt read: "You are grading a support agent. Given the question and the answer, reply with a label and a reason. correct: matches our policy / incorrect: it does not." The four examples:

| Question | Agent's answer | Judge's label and reason |
| --- | --- | --- |
| Cancel my subscription | Done. It ends on Nov 1. | correct: matches the cancellation policy |
| Refund for order 4412? | Refunds take 90 days. | incorrect: policy says 5 to 7 days, not 90 |
| What is your return policy? | 30 days, free returns. | correct: matches the return policy |
| Change my address | Updated to 12 Main St. | correct: address updated as asked |

Evals also need to be "aligned with human judgment," and this, Dhinakaran argued, is "probably where an AI engineer should be spending a lot of their time": looking at traces alongside their agents and identifying the common errors. "I was hearing Lance yesterday talk about the Claude skill to build evals," Dhinakaran said, referring to Anthropic's Lance Martin's workshop the day before. Arize has a skill of its own to help build evals and align them with human judgment. (Arize's documentation describes an `arize-align-evaluator` skill that reports accuracy, a confusion matrix and per-label precision and recall; Dhinakaran did not name it.) "This is probably where the shift to defining what is good becomes really, really important for an AI engineer."

![You evaluate the evaluator](../slides/10-13-1012-what-does-an-ai-engineer-do/11.jpg)
*Human and judge labels on five cases: 80% agreement.*

| Case | Human | Judge | Agree |
| --- | --- | --- | --- |
| Cancel my plan | correct | correct | ✓ |
| Refund for #4412? | incorrect | incorrect | ✓ |
| Return policy? | correct | correct | ✓ |
| Change my address | correct | incorrect | ✗ |
| Close my account | incorrect | incorrect | ✓ |

The question teams ask, Dhinakaran said, is "how do I trust the evaluator?", or "how do I trust that it's as good as my judgment?" Arize's answer for its own products: "We actually evaluate the evaluators." The team builds golden-truth datasets, runs the evals, compares them with human labels and aligns them "to make sure that the judge actually agrees with the human … before we actually ship out a new evaluator." The slide's example scored 80% judge-human agreement on five cases. Those evals can then be used both offline and online.

## 11. Trace everything, including the coding agents

Once an agent is built, always on and evaluated before shipping, "the goal is to actually trace everything," and that now means "not just tracing the agents, but also tracing the coding agents behind the agents that built this": Claude Code, Cursor and Codex, so a team can understand what happened "in the factory of agents."

![Online evaluation](../slides/10-13-1012-what-does-an-ai-engineer-do/12.jpg)
*Five steps from live traces back into the factory as a fix.*

The slide laid out online evaluation in five steps: live traces from every agent run; evals score every span; failures cluster into patterns; patterns become eval cases; and they go back into the factory as a fix. Offline evals test a new change; in production they become online evals on the telemetry, and their results are pulled back for agents to read. The system becomes: "collect data, eval that data, run agents on top of it so that you can find problems and figure out where the gaps are."

## 12. Closing the gap

Returning to the opening question, Dhinakaran said "there's a pretty big gap between what's happening in the labs today and what's happening with a lot of businesses," partly because "the data inside of businesses are a lot more messy than what is very constrained and testable inside of a lab." AI engineers are the role to close that gap: trace everything, run evals, and build a system in which agents improve from the telemetry being collected, "so that you're no longer just building individual features, but you're actually building the system to improve your products." Dhinakaran pointed listeners to Arize's booth to see Signal and closed the talk.

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=b2ed4456-5671-4e8e-83fd-684c012fef8c) · [Video: "What Does an AI Engineer Do Now?"](https://www.youtube.com/watch?v=ImnZ7s68jw8) · [Day 1 livestream at 1:00:16](https://www.youtube.com/watch?v=akEqiXFo3GM&t=3616s) · [Arize Signal documentation](https://arize.com/docs/ax/observe/signal) · [From Signal to PR](https://arize.com/blog/from-signal-to-pr/) (Arize blog) · [Align evals to human feedback](https://arize.com/docs/ax/evaluate/align-evals-to-human-feedback) · [Lance Martin's workshop write-up](10-12-1530-unhobbling-the-frontier.md)

This write-up is based on the official video captions (checked against the Day 1 livestream), the author's slide photos of the livestream and Arize's documentation. The talk ended without Q&A. The [precise notes](precise/10-13-1012-what-does-an-ai-engineer-do.md) (Chinese) keep timestamps, caption corrections and the open questions.
