# Every Agent Fix Needs a Regression Test Built From Production Traces

*At AI Engineer NYC 2026, Arize's Sofia Jakovcevic showed how a one-line prompt change can make an agent safer while quietly breaking it, and laid out a five-step loop that tests every change against production traces before it ships.*

**Breakout · Tuesday, October 13, 2026 · 3:20–3:38pm · Turing Room (Riverside Ballroom), Evaluations track**
**Speaker:** Sofia Jakovcevic, Forward Deployed Engineer, Arize AI

![Title slide](../slides/10-13-1520-your-agent-fix-just-broke-something-else/01.jpg)
*The title slide, tagged "Live demo · Agent observability & experimentation."*

---

"The biggest challenge isn't determining what broke," Sofia Jakovcevic said. "The biggest challenge is that when you do submit a fix, you want to make sure that you don't break 10 other things along the way." Jakovcevic asked who had pushed a simple change only to find "everything's on fire," and saw a lot of hands.

Jakovcevic introduced Arize as "an observability backend that is designed to make AI work." An agent's reasoning can be a black box, hard to understand, predict and debug; observability is "essentially shining a light in the black box," and once a system's steps are visible they can be evaluated and improved. The title slide's subtitle stated the aim of the talk: "Turning agent observability into a regression loop that tests every agent change before it ships."

The talk had three parts: why agent regressions are hard to detect, a five-step regression loop shown first as an overview and then live in the Arize platform, and how that loop can be closed "at agentic speed."

## 1. The fix that broke the agent

Jakovcevic started with an example. A security engineer reviews a support agent's policy and wants to add one line: ask the agent to confirm before it takes any action. "Sounds simple. Sounds harmless." The engineer has the intuition, and teams are quick to make changes like this because prompts are written in natural language and feel understandable. "We're really coding on vibes, and we don't want to be," Jakovcevic said. Teams should instead have scores "that prove that these changes are actually making a positive impact and not just crossing our fingers."

![The fix that broke the agent](../slides/10-13-1520-your-agent-fix-just-broke-something-else/04.jpg)
*"A one-line policy edit made a support agent safer. It also stopped finishing tasks, and the average didn't move."*

A week after the change, evaluations on production data told the story the slide shows, built up one row at a time:

| Metric | Baseline | Candidate | Change |
| --- | --- | --- | --- |
| Action safety | 0.81 | 0.98 | +0.17 |
| Task completion | 0.89 | 0.72 | −0.17 |
| Average of both | 0.85 | 0.85 | 0.00 |

The security engineer was right: confirming before every action made the agent safer. But task completion fell by the same 17 points. With a confirmation before every step, "if a user leaves the session, that session is never completed to the end."

"Ironically enough, the overall average of both is net zero," Jakovcevic said. "Just because you improve the dimension of one agent does not mean that you're improving the agent as a whole." Then Jakovcevic asked the room: "Would you have shipped this change?"

(Arize's blog post on agent regression testing uses the same 0.89 to 0.72 drop in task completion, but in a different setting: an online toy store's agent that canceled orders without asking for an order number, fixed with a policy requiring confirmation before purchases and cancellations. The blog gives no action-safety figures; the 0.81 to 0.98 and the flat average appear only on the stage slide.)

## 2. Agents fail at runtime

![Part 01: Agents fail at runtime](../slides/10-13-1520-your-agent-fix-just-broke-something-else/05.jpg)
*Part 01: "The code you changed is rarely the place the failure shows up."*

The mistake, Jakovcevic said, was assuming the error would show up in the code. "These agents are failing at runtime."

![Why agent regressions are different](../slides/10-13-1520-your-agent-fix-just-broke-something-else/06.jpg)
*What code review sees, next to what runs in production.*

The slide put the two views side by side. On the left, under "What code review sees": `system_prompt.md`, one line removed and one line added. "Small change, easy to approve." On the right, under "What runs in production," a trace tree: `agent.run` calls `llm.plan`, `tool.lookup_account` and `llm.decide_action`, then a step highlighted in red, `policy.check → ask user`, and finally `llm.respond`. The caption read: "Plans, tool calls, and branches you only see in a trace." The headline summarized it: "Code changes are local, but behavioral effects aren't. Small changes can reshape the entire execution path."

The one line in the system prompt "shifted how the agent operates," Jakovcevic said: it added steps, changed how the agent worked with sub-agents and how it called tools. Those trajectories, possibly thousands of them, "are not in your static code. They're only in your observability back end. They're only in your traces."

"The diff tells you what changed, but the trace tells you what actually happened," Jakovcevic said. Traces, the structured logs of what agents do, "are becoming the new source of truth." In traditional software, "logic was written down"; now "the logic only exists at runtime," and the gap between what was written and what happens "is filled with silent errors." Regression testing therefore has to start from traces.

## 3. The five-step regression loop

"We're going to start with our production traces," Jakovcevic said. "We're going to build evals, run experiments, and then close it all up to be an automatic gate at CI." The five steps were: build a dataset, add evaluators, run a baseline experiment, make one change and compare, and gate merges in CI. (The slides for steps 1, 4 and 5 were not photographed; those parts come from the video.)

### Step 1: Build the dataset from production traces

Use real production data: "We don't need to synthesize data." If real user queries, inputs and tool calls exist, pull from them directly. The dataset needs four kinds of cases: a representative sample of the average workflow; known failures, the cases that often fail on eval metrics; edge cases, "maybe not as common, but just as important"; and successful workflows to protect. That last group is "one thing that we often see users forget," Jakovcevic said. "We're not just trying to make sure that the fix is going to work, but we also want to test that we're not going to break something that was already working."

### Step 2: Add evaluators

![Step 02: Add evaluators](../slides/10-13-1520-your-agent-fix-just-broke-something-else/07.jpg)
*Three evaluators, one per dimension: two LLM judges and one code check.*

The slide read: "An LLM judge for semantic questions, code for deterministic ones. Score each dimension on its own; a single quality score averages tradeoffs away." It listed three evaluators:

| Evaluator | Type |
| --- | --- |
| `task_completion` | LLM judge |
| `action_safety` | LLM judge |
| `valid_tool_args` | Code |

The LLM-as-a-judge example for `task_completion` read: "You are grading a support agent's trace. Did the agent fully complete the user's request, with no step left unfinished? Answer with one label: `complete` | `incomplete`." The code evaluator was a short Python function, `valid_tool_args(output)`, returning true only if every tool call's arguments are accepted by that tool's definition.

Evaluators, Jakovcevic said, turn dimensions that matter to engineers and product teams into numbers. Hallucination, accuracy, tool trajectory and step efficiency are "subjective in nature"; a human could review them, but the goal is to automate that. An LLM judge grades a span, a trace or log data against a guide and returns a label, in the example a binary 0 or 1; scores can also run from 0 to 5 or use multiple labels. Code-based and ML-based evaluators are options too: "A code evaluator is great for your deterministic checks, but an LLM as a judge is good for your non-deterministic checks." Each important metric gets its own eval, which together form the testing suite.

### Step 3: Run the baseline experiment

![Step 03: Run the baseline experiment](../slides/10-13-1520-your-agent-fix-just-broke-something-else/08.jpg)
*The control run: same dataset, same evaluators, no changes.*

"Run the current agent against the dataset with nothing changed," the slide said. "This is the control every candidate gets compared against." Its code panel called `client.run_experiment` with the dataset `support-agent-regressions`, the task `run_agent`, the three evaluators and the experiment name `baseline`. The results: task completion 0.89, action safety 0.81, valid tool arguments 1.00.

"You can do this in the SDK, it's just run experiment," Jakovcevic said: pass in the dataset, the task and the evaluation suite. (Arize's CI documentation shows the Python SDK call as `client.experiments.run()`; the slide's form may be simplified.)

### Step 4: Make one change, rerun, compare

With a baseline in place, add the one change, rerun the evaluation and compare. The same process tests several prompt variants or several models at once. Jakovcevic gave a hypothetical: a team wants to switch to "the new Fable model, it's five times more expensive." "You're not going to switch without proving that you're going to get a good ROI," and this is how to prove it with numbers. Teams can rerun and compare "changing the prompt, changing handoffs, changing tools, changing models … all without having to deploy to production."

### Step 5: Gate merges in CI

Finally, the experiment moves into CI. In GitHub Actions, for example, each pull request runs through the experiment workflow against the evaluations the team considers important, "and you don't merge until it meets all of the regression criteria that you have listed." "Now, you're no longer pushing on vibes," Jakovcevic said. If the result is negative, the team goes back and iterates. (Arize's documentation lists GitHub Actions among several supported CI systems; its example gate compares an eval's mean score against a threshold of 0.7 and fails the build below it.)

## 4. Demo: the loop in the Arize platform

![Experiment comparison in Arize](../slides/10-13-1520-your-agent-fix-just-broke-something-else/09.jpg)
*The `refund-policy-regression` dataset: 100 examples and six experiments, with a chart of both scores across runs.*

Jakovcevic switched to the Arize platform, showing "this exact refund agent." The dataset page, `refund-policy-regression`, had 100 examples and six experiments, each "a different test that I ran with different prompt changes, potentially different model changes," compared to see "which one has the best trade-off." The rows visible in the photo (the first row was hidden behind an audience member):

| # | Experiment | Action safety (avg) | Task completion (avg) |
| --- | --- | --- | --- |
| 6 | refund-follow-up | 0.95 | 0.86 |
| 5 | refund-strict | 0.99 | 0.58 |
| 4 | refund-generous | 0.70 | 0.95 |
| 3 | refund-v1-unguided | 0.64 | 0.78 |
| 2 | refund-candidate | 0.98 | 0.72 |

Experiment 2 is the candidate from the opening example. Summary cards at the top read "+18 pts" for action safety (best: experiment 5, 99%) and "+6 pts" for task completion (best: experiment 4, 95%). Jakovcevic said "overall I'm going to have a plus 18 … points on action safety and plus six on task completion." Read from the photo, those are the best scores of two different experiments, each against the baseline of 0.81 and 0.89: refund-strict reached 0.99 safety but only 0.58 completion, and refund-generous reached 0.95 completion with 0.70 safety. No single visible experiment improved both; the closest, refund-follow-up, gained 14 points of safety and lost 3 of completion.

### Prompt Playground: testing a single agent

![A new prompt playground](../slides/10-13-1520-your-agent-fix-just-broke-something-else/10.jpg)
*A fresh playground, before a prompt or dataset is loaded.*

To show how such experiments are built, Jakovcevic created a new prompt playground named for the conference. "A single agent … is essentially just a single prompt with a set of tools attached to it, a model, LLM configurations," and the playground replicates that.

![Loading the prompt and choosing a dataset](../slides/10-13-1520-your-agent-fix-just-broke-something-else/11.jpg)
*`refund-prompt v1` pulled from the prompt hub, with the dataset picker open.*

Jakovcevic pulled version 1 of the stored prompt from the prompt hub, then chose a dataset prepared in advance: "those edge cases, those known failures, and those successful workflows that I want to protect, all listed in this 100-row data set."

![Two prompt versions side by side](../slides/10-13-1520-your-agent-fix-just-broke-something-else/12.jpg)
*`refund-prompt v1` and `v2` run side by side on the same 100 rows.*

Jakovcevic duplicated the prompt so two versions could run side by side, either by editing a line or by loading a separate saved version. The screen showed `refund-prompt v1` and `v2`, both on `gpt-4o`. The system prompt began: "You are the order-support agent for Northwind Goods. You handle refund requests that arrive from email and chat. You are the last step before the ticket is closed, so every reply must leave the customer with a decision they can act on. Do not ask a teammate to take over, and do not end the turn by saying you will look into it later." The user template took a ticket ID, customer message, order age in days, reason and order value; the first row, ticket T-032, read "The order arrived but the charger was missing. Order placed 10 days ago."

Jakovcevic then attached the task completion and action safety evaluators, noting many pre-built ones (hallucination, user frustration, code templates, security checks) and the option to write custom ones. Each run scores every record, and results appear on the dataset's experiment page alongside latency, total cost and error rates, so pull-request changes and playground runs can be tracked over time.

### Agent Playground: testing a multi-agent system

The second option was the agent playground, which connects to a remote agent, for example over HTTP. Each record in the dataset calls the agent, collects its output, runs the evals and scores it the same way. That makes it possible to test "a very complicated multi-agentic system," including changes to how sub-agents communicate.

### Coding agents and skills

All of this can also be done through the SDK, REST, CI or a CLI, "but my favorite method to do this is actually with coding agents," Jakovcevic said. "Arize has a public repository of skills that allows you to connect to every feature available in the Arize UI." That connects the code a coding agent writes in the IDE with what is happening in the observability backend. (The `arize-skills` repository covers tracing, datasets, experiments, evaluators, annotations and prompt optimization, and requires the Arize AX CLI.)

## 5. Closing the loop at agent speed

![Part 03: Closing the loop at agent speed](../slides/10-13-1520-your-agent-fix-just-broke-something-else/13.jpg)
*Part 03: "The same loop, run by agents instead of people."*

With two minutes left, Jakovcevic gave two takeaways.

![Today, engineers close the loop](../slides/10-13-1520-your-agent-fix-just-broke-something-else/14.jpg)
*Five stages, with failed production traces feeding back in as regression cases.*

The first described the loop engineers run today. The slide, "The workflow is repeatable. The manual investigation is the bottleneck," showed five stages: Observe (trace every agent run in production), Evaluate (score live traces with the same evaluators), Change (edit a prompt, model, tool, skill or policy), Test (run the regression experiment in CI) and Improve (ship what passes; the rest of the box was cut off in the photo). An arrow ran back from the end to the start: "New production traces become regression cases." Cases that fail go back to the observability layer and join the regression suite. "Every failure should become a test," Jakovcevic said. "Every failure should make your agents stronger and harder to break."

![From Observability to Self-Improvement](../slides/10-13-1520-your-agent-fix-just-broke-something-else/15.jpg)
*From humans debugging agents to agents improving themselves.*

The second takeaway was the "paradigm shift" Jakovcevic had promised at the start. "What if an agent could do the investigation and propose a fix?" Today, "humans are the bottleneck." The slide contrasted "Human manual triage," where a human reads the dashboard, triages and ships the fix ("the ultimate bottleneck"), with "Automated agent tracing," where agents read traces directly to self-correct and turn failures into evidence for improvement. "Observability is no longer about humans understanding agents," Jakovcevic said. "It's about agents understanding themselves."

The link back to the opening argument was that the real logic lives in traces. "If your coding agents are pushing PRs, and I know they are, they're pushing PRs without actually knowing what's happening in production. That means they're pushing PRs blind." Connecting coding agents to production data gives them a full picture of what their changes do.

"The best agents don't just learn from failures," Jakovcevic concluded. "The best agents fix themselves."

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=2a589e49-4ee4-4c53-bfeb-40bc3db16506) · [Video](https://www.youtube.com/watch?v=v5tm5EAzsAs) · [AI agent regression testing with Arize AX](https://arize.com/blog/ai-agent-regression-testing-with-arize-ax/) (Nancy Chauhan, Fuad Ali) · [CI/CD for automated experiments](https://arize.com/docs/ax/improve/ci-cd-for-automated-experiments) (Arize docs) · [Arize-ai/arize-skills](https://github.com/Arize-ai/arize-skills) · [Arize skills documentation](https://arize.com/docs/ax/agents/arize-skills)

This write-up is based on the official video's captions, the author's room recording, slide photos and the sources above. The video ends after the talk without a Q&A; Jakovcevic invited questions afterward at the back of the room. The [precise notes](precise/10-13-1520-your-agent-fix-just-broke-something-else.md) (Chinese) keep timestamps and the full record of where the talk, slides and Arize's documentation differ.
