**English** | [中文](zh/10-12-1420-langfuse-workshop---evals-and-observabil.md)

# In Regulated Industries, an Agent Needs a Paper Trail

*At AI Engineer NYC 2026, Annabell Schäfer and Doneyli De Jesus of Langfuse showed how a bank can trace, review and gate a customer-facing agent: domain experts write raw comments, an LLM turns them into failure modes, and every prompt change runs through evals in CI before it ships.*

**Workshop · Monday, October 12, 2026 · 2:20–3:20pm · Sutton Room (NY Ballroom W)**
**Speakers:** Doneyli De Jesus, Head of AI Engineering Solutions, Langfuse (by ClickHouse); Annabell Schäfer, AI Engineering Solutions, Langfuse (by ClickHouse)

---

Annabell Schäfer opened with a support conversation that looked perfect. A user asked for a refund; the agent looked up the policy and replied that it had issued one; the user said thanks. Every system metric was green and there was no latency problem. But the refund tool was never called. "The agent lied about doing it to the user," Schäfer said, and the team would only find out weeks later when the customer came back asking where the money was.

"AI agents can fail while the system metrics themselves look green," Schäfer said. That is why teams need a way "to continuously mine for those signals where things are going wrong."

Schäfer joined Langfuse about seven months before the conference. Before that, in what Schäfer called "my previous life," Schäfer built AI agents for medical device compliance, where the team needed ground-truth datasets validated by customers and ran many human-in-the-loop systems. Asked for a show of hands, many in the room had helped bring an agent to production and quite a few had used Langfuse or another observability tool. As for who had solved evals: "For the confident hands, I would love you to come by at our booth and explain us how."

## 1. Why Langfuse exists

Langfuse's founders hit the problem three years earlier, during their Y Combinator batch (W23, according to YC), when they realized "there's nothing like a unit test for AI applications." Langfuse is now "an open source platform to continuously trace, monitor and improve your AI systems." It started as a developer tool, then added domain experts as users, and over the past "8 to 12 months" a third kind of user: coding agents. ClickHouse acquired Langfuse in January 2026.

Schäfer cited adoption figures:

| Metric | As said on stage | Langfuse website |
| --- | --- | --- |
| Developers | more than 150,000 | 100,000+ engineers |
| Fortune 50 companies using it | 21 | 21 |
| Observations ingested per month | more than 90 billion | 90B+ |

Named customers included Ramp, Canva and Khan Academy.

## 2. The AI engineering loop

Working with those companies, Schäfer said, the teams that ship reliable agents and keep improving them follow some form of what Langfuse calls the AI engineering loop: a set of practices repeated continuously rather than once.

| Step | What it means, per Schäfer |
| --- | --- |
| Tracing | The foundation: every step the agent takes, recorded in a log a human can inspect |
| Monitoring | Making sense of traces at scale: aggregate metrics, and signals of what is going wrong |
| Datasets | A representative set of examples the application should handle, and how |
| Experiments | Varying one thing about the system and seeing what happens |
| Evals | Measuring the experiments at scale |

The tracing slide showed a Langfuse trace of an IT support agent, "an agent that is answering questions from your parents so you don't have to do it." (Langfuse's public self-serve workshop is built around the same kind of app, a "Dad IT Support Agent.") The root trace shows total time and cost, and below it each generation (OpenAI chat calls) and each tool call, with inputs, outputs and metadata. That log serves humans digging into a problem and coding agents reading traces at scale.

Monitoring matters because traces have grown too long and numerous for anyone to read by hand. Beyond cost and latency, it covers quality signals: whether compliance guardrails held, and how many users are frustrated. "How many are writing all caps messages," Schäfer said.

Datasets represent what the agent should handle in production, including out-of-scope requests it should decline: an IT support agent should not give investment advice or forecast the weather in New York. Each item has an input and an expected output, which can be a sample reply, an expected behavior or keywords to cover. A dataset serves as a stable benchmark offline or in CI/CD.

Experiments vary one thing, such as the prompt, model, context or a tool description, and Schäfer stressed "isolating concerns": to test whether the same model does better with a new prompt, change only the prompt, "and not five other things." Evals measure the result, because no one wants to look at everything by hand ("which you by the way should probably sometimes do"). The scalable options are LLM-as-a-judge and code-based evals, such as an exact match for classification or a deterministic check for all-caps messages.

Teams enter the loop at different points. For a low-stakes internal chat agent, tracing and a few monitors may be enough to go to production, with test cases added over time. Customer-facing and regulated use cases can't take that risk. In regulated industries, Schäfer said, tracing is always the base: what steps the agent takes, what it sees and which tools it calls. On top of it, teams build ground-truth datasets, which any talk of auto-improvement depends on. Schäfer described three ways to get there:

| Approach | How it works |
| --- | --- |
| Expert curation | Domain experts inside the company build the dataset |
| Shadow runs | The process keeps running manually while the agent runs in parallel; comparing the two measures accuracy and builds ground truth |
| Full human-in-the-loop | Humans stay in every decision, and over time the data is sliced into parts safe to automate and parts that always need a person |

## 3. A banking assistant, traced

Doneyli De Jesus ("you can call me Don") took over with a demo built for financial institutions Langfuse works with. Those teams build a first prototype, De Jesus said, and then leadership asks how to evaluate a customer-facing agent. Asked who worked in financial services, many hands went up: "we're in New York. So, of course, you all are."

A bank's chat assistant looks ordinary, De Jesus said, but behind it sit financial liability, legal liability and risk, and LLMs are "inherently non-deterministic." The goal is to reduce unpredictability, reproduce outcomes and find the failure modes. The demo had three features. Users can impersonate different kinds of customers, from everyday account holders to ultra-high-net-worth clients, to test whether the agent respects guardrails for each persona. Personally identifiable information is masked before anything is stored in Langfuse, because banks don't want card numbers or social security numbers in an observability system; that data stays in their systems of record. And every interaction is scored against the controls and quality gates the bank defined.

Logged in as "Anna," De Jesus asked about international wire fees and got an answer from the knowledge base. The demo interface also displayed the tools and data sources called, which a real product would not show a customer. Then De Jesus typed "I lost my card. This is my number. Is it blocked?" with a card number in plain text. "You cannot control what your users input," De Jesus said, so it has to be caught in the instrumentation. In the Langfuse trace, the number was "completely redacted" by a masking function applied to all defined PII before anything is persisted.

The trace showed the full tree of steps (the Anthropic LLM call, the tools available, a call to list accounts) and, at the top, the scores defined for this use case, calculated automatically on every new trace: whether the answer complied with banking rules (a question about the weather gets no answer), any failure modes, and whether a guardrail blocked the response. Asked "Can I put my savings in bitcoin?", the agent correctly declined and flagged the request as investment advice that needs an advisor. In the trace, the input guardrail named investment advice as the primary risk and a score recorded the same intent, so such cases are easy to filter.

The demo also handled voice. A recorded call to the contact center was transcribed and evaluated for violations; its trace held a link to the audio, the transcript (a customer asking to block a card) and the assistant's reply.

## 4. Datasets as test cases

Customers will always use a system in ways it wasn't designed for, De Jesus said, and with LLMs no one can anticipate every outcome. The anchor for evaluation is the dataset: "your set of questions and expected outputs." For software developers, "these are your test cases." No one ships without QA, De Jesus said, "unless you merge into main or prod, which I'm pretty sure most of you have done."

The expected output doesn't have to be the answer itself. It can be the intent, or what the agent is supposed to do. That is why teams end up with several datasets, each measuring one area of quality. The demo had one for red-teaming (prompt injection and other attempts to get around the guardrails) and one for disputes that had not been resolved.

Domain experts still need to work with developers to define these datasets. De Jesus has seen two ways to start: a team already keeps a spreadsheet of things to test and uploads it as a CSV, or the team adds production traces it judges to be good examples, a way to "dog food" the dataset. Everything shown in the UI can also be done through Langfuse's CLI, MCP server or API. "It's better for me to show it in a UI rather than telling Claude to do it end to end. Make no mistakes."

## 5. Experts comment, the LLM finds the categories

De Jesus called the next step error analysis: looking at traces to see whether the agent took the steps it should, called the tools it needed and gave the right output. Schäfer, in the earlier job, "spent like days, like literally looking at traces." That still matters, but no one can read thousands of production traces. So the team samples a representative slice, say 10% or 20%, or only conversations about disputes or credit cards, and puts it in an annotation queue for a domain expert: "you are the risk expert. I need you to take a look at this output."

The task has to be easy, because experts have day jobs. De Jesus suggested a couple of scores (yes/no, true/false or a 1-to-10 scale) plus a free-text comment. In the demo, De Jesus picked the failure mode "missing a citation" and wrote "Citation is not there. Needs to be included," which took about ten seconds. At 30 seconds each, a hundred traces is under an hour of an expert's time, and dictation tools like Wispr Flow mean they don't even need to type. Each annotation stays attached to its trace as evidence.

Then an LLM reads all the comments and finds the recurring categories. "You're not asking the team to invent categories," De Jesus said. "Give me your unfiltered raw commentary," and let the LLM do the pattern matching it is good at. Those categories become the dimensions of quality to score. The demo's included "cites expected sources," correctness, "formal register" (in some languages, customers should be addressed formally), language match, "must include" and "not unsolicited" upsell.

## 6. A mix of judges and code

For each dimension, the team chooses an evaluator. Langfuse can run a library of evaluators automatically on incoming traces, or the client application can compute scores in real time and send them back. De Jesus's main lesson from customers was to use "a balanced mix" of LLM-as-a-judge and deterministic evaluators, depending on the category. With only LLM judges, "there's a nonzero chance that some of the outputs are … not going to be accurate." Deterministic checks always return the same result for the same input, and blending the two gives a more reliable assessment.

The demo's code evaluator was a regular expression doing an exact match. "This is going to give me the same output 100% of the time," De Jesus said. "This is not going to be, well, 95% or 80%."

An experiment ties it together: a dataset of tasks, run against the agent as it stands today, then rerun after changing the model, the system prompt or the tools the agent can reach. The point, De Jesus said, is to prototype before promoting to production rather than "running on vibes, which is pretty much what we've all been doing."

## 7. Every prompt change runs the evals

In the demo, the system prompt was versioned in Langfuse (some customers keep it in GitHub or GitLab instead), and an automation treated any prompt change as a trigger to launch a CI job in GitHub. The prompt had five versions, each with small changes; version 3 was a development experiment, and version 4 added detail and passed new parameters to the agent's configuration. Having seen regressions, De Jesus promoted version 4 to production by clicking through, "like a caveman," though the API or CLI can do the same with a production label. Seconds later a new CI job started to run the experiment against the dataset.

An earlier run showed the result: the prompt, its version and labels, and a pass. The evaluators in the gate were "must include," source recall, language match, correctness and "not unsolicited upsell," each with a minimum threshold; a score below its threshold fails the gate. The demo showed only a passing run. De Jesus left it there rather than merging automatically: "I'm not crazy enough to just say, okay, cool. So you pass, go merge it." A team could have the pipeline open a pull request for review, or, "like everybody's doing today," push straight to main if the gate clears. Either way there is "the paper trail, so to speak, and the evidence on why you make that decision."

## 8. A dashboard for the business

Organizations now care about "tokenomics," De Jesus said: in 2025 and early 2026 "everybody was token maxing," and now everyone wants value from their tokens. The last part of the demo was a dashboard meant to reassure business stakeholders that the agent is worth it. De Jesus acknowledged "you might argue with me in the logic behind this."

| Panel | What it showed (last seven days) |
| --- | --- |
| Interactions resolved without a human | 1,400, at a total cost of less than $7 |
| Top failure modes, by occurrence | Out of scope, informal register, dispute not resolved, all "creeping up over time" |
| User feedback | Mostly thumbs up; 18% thumbs down |
| Security risk | Most interactions flagged no risk; 89 involved a customer asking for investment advice, where the agent "might have given or might have tried to give" it |

De Jesus worked in a contact center 20 years ago, when such a person was paid $15 to $20 an hour and could not handle 1,400 calls in an hour; the comparison shows "at least some directional business benefit." The failure-mode panel narrows the search: instead of reading thousands of traces, De Jesus filtered for "dispute not resolved" equal to zero, saved it as a view, and had dozens or hundreds of cases to investigate. The same works for the 89 investment-advice cases, filtered by the categorical security-risk score.

The point of the process, De Jesus said, is evidence: a customer-facing agent carries liability, financial and reputational risk, and a systematic loop lets a team show that its outputs follow the guardrails and controls in place.

## 9. Work with your coding agent, but bring taste

Schäfer returned to close. Some teams simply tell a coding agent connected to Langfuse through MCP or the CLI, "Please fix my agent. Make no mistakes." In Langfuse's experience, that produces "rather slop than anything that is actually helping you."

Schäfer's slide plotted time invested against the quality of the system. Full automation ("fix everything, do it for me") lands in the bottom-left corner: little time, low quality, unhappy customers. Going fully manual is no longer right either, since instrumenting the app, finding failure modes and reviewing data can be automated. The target is the balance between the two: humans review data and bring "taste" and domain expertise, especially in building datasets and deciding what the application should solve, while the coding agent instruments the code, finds failure modes, runs evals and runs "20 experiments instead of only one." Schäfer urged attendees to be "the tasteful AI engineer," automating more of the loop over time.

If they were starting tomorrow, Schäfer said, they would do three things. Install the Langfuse skill, a set of markdown files that teaches a coding agent how to work with Langfuse and is connected to the documentation, integration pages and the Langfuse Academy. Collect ground truth to evaluate against, from historical data, production or samples run with domain experts. And in a regulated space, define hard limits: in financial services, the agent "should never give investment advice." Put that in the evaluation set so that, ideally, it never happens, "but at least you will know about it."

Schäfer said the repository De Jesus used is available online, with fictional data, alongside the Langfuse Academy and a self-serve workshop. De Jesus would speak again on Wednesday about deployment gates for AI agents, the Langfuse booth (number 9) offered AI-generated stickers of visitors' faces, and the team invited attendees on a community run with two of the founders.

## Q&A

The questions were not audible on the recording; they are reconstructed from the answers.

### Running Langfuse

**What does Langfuse depend on for storage?** De Jesus said Langfuse has "a hard dependency" on ClickHouse. It started on Postgres, but Postgres "couldn't handle the scale that we achieved," billions of events per day, so it moved to ClickHouse. Teams that need the data elsewhere can export it to S3 or read it through the API, CLI or MCP server.

**Can it be hosted on Amazon?** "You can host it 100% on Amazon," De Jesus said. The managed cloud offering runs on Amazon and needs no setup, and a self-hosted deployment, for example on existing EC2 instances, "comes prepackaged with all the components."

### Trusting the judge

**How do you make sure an LLM judge is consistent and agrees with people (inter-rater reliability)?** "Super important question," Schäfer said, adding that the Langfuse skill helps with this process too (Schäfer couldn't recall the term for it). The goal is a judge that rules the way a human would. The academic approach is to have several human raters score a representative sample, then tune the judge until it agrees with them. The practical approach is to have one person label some data, optimize the judge against that ground truth, and check over time that the sample stays representative. On production data, Schäfer advised starting with "a few too many false positives, so you catch a little more than necessary but don't miss anything," and tuning down from there. The recording ends here.

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=f20b8d60-8a75-4ab3-a2fb-42159024e65f) · [Video](https://www.youtube.com/watch?v=obfPzbapMIM) · [Talk page](https://ai.engineer/talks/obfPzbapMIM) · [Langfuse workshop](https://langfuse.com/workshop) ([repo](https://github.com/langfuse/langfuse-workshop)) · [Langfuse agent skill](https://langfuse.com/docs/api-and-data-platform/features/agent-skill) ([repo](https://github.com/langfuse/skills)) · [Langfuse datasets](https://langfuse.com/docs/evaluation/experiments/datasets) · [Annotation queues](https://langfuse.com/docs/evaluation/evaluation-methods/annotation-queues) · [Langfuse Academy](https://langfuse.com/academy) · [Langfuse joins ClickHouse](https://langfuse.com/blog/joining-clickhouse)

This write-up is based on the official video captions and the sources above; no slide photos were taken at this session, so slides and demos are described from what the speakers said. The schedule's description names the demo bank "Northwind Bank," a name not used on stage, and the banking demo's repository could not be located. The [precise notes](precise/10-12-1420-langfuse-workshop---evals-and-observabil.md) (Chinese) keep timestamps, caption corrections and the list of unverified items.
