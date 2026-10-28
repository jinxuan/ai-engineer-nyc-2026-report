# How Not to Trust Your Agents: Evaluate the Configuration, the Trace and the Scorer

*At AI Engineer NYC 2026, Omni's Stephanie Jarmak, a former astronomer, argued that agent evaluations should be run like scientific experiments built around a decision, and showed how cost savings, benchmark rankings, scoring rules, quality gates and even traces can mislead.*

**Breakout (Evaluations track) · Tuesday, October 13, 2026 · 11:55am–12:13pm · Turing Room (Riverside Ballroom)**
**Speaker:** Stephanie Jarmak, AI Engineer, Omni Analytics (previously Sourcegraph; physics PhD; research affiliate, Center for Astrophysics | Harvard & Smithsonian; maintainer of the open-source agent orchestration system Gas City)

---

Stephanie Jarmak introduced herself as "an AI engineer as of a couple weeks ago at Omni Analytics," whose platform offers "a governance semantic layer to have a shared understanding of business data" through its agent, Blobby. Before that Jarmak spent a year at Sourcegraph as a solutions engineer for Amp, a coding agent, then moved into a role running Sourcegraph's evaluation platform, where Jarmak "built a very large public benchmark called CodeScaleBench to understand how agents are working across very complex code bases." Before that, Jarmak was an astronomer.

"I come from this from a research scientist point of view, where I relentlessly question everything, hence the title of Lies, Damned Lies, and Evals," Jarmak said. "It says how to trust your agents, but I think largely this talk is mainly about how to not trust your agents."

Every slide carried a citation in its footer, most often to chapters of Jarmak's book *Engineering Reliable Coding Agents: Evaluating and Operating the System Around the Model*.

## 1. Cheaper, but the failure remained

![Lower cost. The retrieval failure remained.](../slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/01.jpg)
*A 28% cost reduction with no meaningful score change, while the retrieval failure persisted.*

The first example came from Sourcegraph, where Jarmak was testing the company's code search MCP tool and trying to work out "why isn't this coding agent actually using the tool that I'm trying to get it to use?" Jarmak changed the agent's initial prompt and reran the evaluation. The slide showed the results:

| Measure | Result |
| --- | --- |
| Observed cost (on the affected task family) | −28% |
| Primary evaluation score | Δ +0.0048 (t = 0.27) |
| Elapsed time | +3.9% |

"It was 28% cheaper. That's interesting," Jarmak said. "But that shouldn't really change any of my decisions around what I'm actually interested in measuring, … because it still didn't use the tool." The cost drop might be worth exploring as its own question about variance, but the underlying problem was untouched: the search missed relevant evidence, and the agent trusted the empty result, "just trusting that that information wasn't there. And that was purely because it didn't use that tool." The slide's footnote: "Little evidence of score improvement does not establish equivalent quality."

## 2. The ranking changes with the harness

![The ranking changes with the harness](../slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/02.jpg)
*The same 63 tasks, two models, two harnesses: the winner flips.*

The second example came from a third-party study of the effect of the harness: 63 tasks requiring no GPU from Terminal-Bench 4, "a pretty popular, widely used benchmark for coding agents." Suppose you are choosing which model to put into your harness. The study compared Claude Opus 5 and GPT-6 Astra:

| Harness | Claude Opus 5 | GPT-6 Astra |
| --- | --- | --- |
| OpenHands | 36/63 (57.1%) | 31/63 (49.2%) |
| PI | 19/63 (30.2%) | 38/63 (60.3%) |

With OpenHands, one of the open harnesses, Opus 5 won. "When they swapped just the harness itself, then they saw that Astra did a much better job," Jarmak said, "like 30-plus percent sort of difference." (Under PI, Astra led by about 30 points; under OpenHands, Opus 5 led by about 8.) The source paper, Li et al.'s "Finding the Right Fit," reports the same figures and summarizes that model rankings reverse across harnesses. The slide also cited Hardy et al.'s "Agent Evaluation Reliability," which finds that rankings of fixed model-and-scaffold systems are far more reliable than rankings of the underlying models alone.

The slide's lesson: "Compare the configuration you will actually run." "You might see all these benchmarks and see the fancy results, but it might not do what you expect it to do within your actual environment," Jarmak said.

## 3. Public benchmarks versus internal evaluation

![Public benchmarks and internal evaluation](../slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/03.jpg)
*Public benchmarks for the landscape and a shortlist; internal comparison on target work for product decisions.*

Jarmak drew a distinction. "An evaluation is you're making measurements to make a decision about your product or process." A benchmark is "a standardized reference for comparison." The slide showed two uses. For the landscape and a shortlist, a reference benchmark with its reported protocol produces a candidate shortlist, with the caveat that "results depend on the task mix and evaluation protocol." For product and process improvement, you compare candidates on your target work, make a change, and loop: "Diagnose, change and retest. Communicate value against a baseline."

Public benchmarks help "have a broader understanding of the larger landscape and get a short list," Jarmak said, but you shouldn't over-index on them "because of the spiky nature of all of the training of these different models." The example: "If you are a law firm, you don't want to necessarily be looking at the Terminal-Bench results, unless of course you are looking at incorporating coding agents with your devs."

The distinction is about visibility, not purpose: "Internal benchmarks exist. Public benchmarks can be used internally." Organizations can build their own benchmarks that reflect real workloads and use them in "this sort of iterative flywheel process where you are increasingly hill climbing" on configurations. The footnote: "The decision determines what evidence we need."

## 4. Design the experiment around the decision

![Design the experiment around the decision](../slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/04.jpg)
*Tasks, repeats and configurations, with cost measured per independently accepted outcome.*

Once the configurations are shortlisted, Jarmak said, approach the evaluation "as a scientist" and decide which variables to change. The slide named three, and asked for quality, reliability, people's time and total cost to be reported together:

| Dimension | Question | What the slide showed |
| --- | --- | --- |
| Tasks | Workload coverage | task families A, B, C, each with tasks |
| Repeats | Variation between runs | the same task run three times |
| Configurations | How results depend on the pairing | a 2×2 grid of models A/B and harnesses A/B |

Below them sat a single metric: total operating cost divided by independently accepted outcomes.

On tasks, pick ones that reflect real work, possibly clustered into families. Jarmak built CodeScaleBench because "at the time it was largely like SWE-bench was just bug fixing, but there's a whole other different swath of tasks that are relevant to developers across the software development life cycle." A model "might be more effective at code understanding than it is at actually fixing a bug." On repeats: "these systems are non-deterministic," so decide how many runs you need. On configurations: model and harness have "interaction type effects." The footnote warned: "Repeated runs are not independent tasks. A better relative score does not establish that either system is reliable enough."

## 5. Trace where evidence becomes a conclusion

![Trace where evidence becomes a conclusion](../slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/05.jpg)
*Available, called, returned, interpreted: and two proposed tests.*

"Gather as much observability data as possible. … Retain all of your traces," Jarmak said. Returning to the unused search tool, the slide broke the path into four stages: available, called, returned and interpreted. First ask whether the tool was even available: "if it wasn't, then you stop there" and fix that. Then whether it was called, and what it returned, and so on "along the pipeline of available information."

The slide suggested recording the search (where and how you searched, gaps or incomplete results, what you couldn't check) and two tests: a search-limits test (the same empty result, with the search limits made explicit) and an evidence test (the same agent and settings, supplied with known relevant evidence). The footnote was careful: "A trace tells us where to investigate. These proposed tests have not yet demonstrated a repair."

![Better evidence can still go unused](../slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/06.jpg)
*Test the evidence source and the agent's use of it separately.*

Even when a better tool is in place, the result can stay the same. Suppose "it finally uses the much better tool. But then the actual outcome that you care about at the end of the task, that still doesn't change," Jarmak said. The slide proposed testing the two halves separately: control A substitutes trusted information at the evidence source; control B bypasses the component that uses the evidence; then look at the outcome. In any deployment comparison, "keep non-use and failed attempts in the result." Jarmak's advice: keep all of that in mind "when you're trying to basically be a detective in understanding what is actually going on in your system." The slide's footnote said this was a simulation result with production effects untested, and that "tool use alone does not identify the tool's causal effect." (It cited a source listed as "Alzahrani · Verify Claims, Not Scores," which the author could not locate.)

## 6. Audit the scoring rule and the answer key

![Audit the scoring rule and the answer key](../slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/07.jpg)
*295 files returned, 6 relevant: perfect recall, 2% precision.*

"You also want to really control what the measurement is that you're making," Jarmak said. In comparing search tools, Jarmak wanted a score that balanced precision with recall. With grep, the agent was "pulling in the entire kitchen sink": in this example, "Claude went out and got like 295 files returned when it only really needed six." The slide's numbers, from one trial of Jarmak's CodeProbe tool:

| Recorded output | Scoring rule |
| --- | --- |
| 295 files returned | Recall 1.0 |
| 6 reference files found | Precision 0.020339 (about 2.0%) |
| 289 outside the reference set | Actual F-beta score 0.025295 (beta = 0.5) |

(Jarmak described it on stage as "like an F1 score"; the slide specifies F-beta with beta = 0.5, which weights precision more heavily. The figures check out: 6/295 = 0.020339.) If all you care about is not missing anything relevant, "then maybe a recall of one is sufficient," but Jarmak was optimizing for something else.

The slide proposed challenges for any scorer: a known-good answer, an empty answer, a return-everything answer, and a valid alternative absent from the key. And it asked for an auditable result record: the scoring rule, the reference version and the run identity. The lesson for public benchmarks' single-number scores: ask "what was the actual validator measuring, and why did they consider that to be important? Because what you might want is going to be dependent entirely on your business needs."

## 7. Test whether the gate changes execution

![Test whether the gate changes execution](../slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/08.jpg)
*An approval-queue audit found paths that allowed execution when required components were missing.*

Evals overlap with quality gates and reviews, Jarmak said. Jarmak has been experimenting with a "software factory" with approval queues: "I want to automate as much as possible for the work to get done, but I also do not trust these systems like at all." Regular audits found a problem. A request went through approval and required checks, and "the agent noticed things were going wrong, but then there was no mechanism in place for it to actually enforce a stopping situation." The slide: "My approval-queue audit found paths that allowed execution when required components were missing."

The fix is to "decouple what's happening with an agent or some other system flagging something versus actually enforcing that gate within the code itself," and then decide how it escalates to a human. The proposed enforcement test runs request, approval plus required checks, execution and observed effect, while deliberately breaking a control: reject or cancel, remove a required component, or change the proposal after approval. Outcomes should be measured separately: authorized completion, safe stop, and people's repair work. The footnote added: "Also measure false blocks on legitimate work; this audit did not establish unauthorized production writes."

## 8. The human review load

Escalation has a cost, Jarmak said, on a slide the author did not photograph. The example came from "auditing and repairing LLM-as-a-judge failures that were happening in a separate system," which had "a 1% false alarm rate. And if all of those alarms that are raised go to a human, that would result in 10,000 reviews. And does your organization actually have the capacity for domain experts to be going and doing that?" Think about where the bottleneck will be, how to change alarm rates, and how consequential each alarm is downstream.

## 9. Some evidence needs another observer

![Some evidence needs another observer](../slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/09.jpg)
*The harness's own record passed a hash check while missing every injected change; an independent record caught all of them.*

"The actual agent trace itself can sometimes not be reliable or trustworthy," Jarmak said. Researchers seeded security issues into an agent's setup. "They had a hash check in place to check for any sort of tampering with that system, and … everything passed. It didn't notice any of those injected security issues." When they compared against an independent record of all the model traffic, "they were able to detect everything."

The slide traced the path from model to harness to tool to destination, with three possible records: an independent record of model traffic, the harness's self-written record, and a record of the resulting state. The harness's own record passed the hash check 28/28; the independent record comparison detected 28/28 of the 28 deliberately introduced changes. The footnote limited the claim: "Detection beyond this scope is untested; model traffic does not prove execution or destination state." For security claims, the slide said, test missed detections, false alarms and blind spots. (The slide cites "Dai et al. · Hearsay," a source the author could not locate.)

## 10. A retrieval upgrade, run properly

![A retrieval upgrade: the experiment I would run](../slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/10.jpg)
*A proposed workflow before deployment, and what to watch after.*

Returning to the original search example, the slide laid out the experiment Jarmak would run. Before deployment: a matched comparison on target work, then an independent challenge ("Can an independent challenger find the decisive evidence?"), then running alongside current work, testing state changes in isolation. After deployment: track completion, repair, latency and cost; audit accepted work and failures; check traces against real outcomes; watch for workload and version changes; and assign an owner, interventions and regression tests ("Do scores predict real outcomes?"). The footnote: "Set restriction or rollback triggers before deployment."

At Omni, Jarmak is building the evaluation harness this way, classifying how users interact with the agent from internal production data: "Q&A, which may be easy questions to harder questions. Or it could be dashboard building and app building." Representative proxies help, "but ultimately, production is truth." Measurement has to continue after deployment, and changes with "a higher blast radius" can be run in shadow mode first.

## 11. Make the next autonomy decision explicit

![Make the next autonomy decision explicit](../slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/11.jpg)
*A proposed autonomy decision, the evidence it needs, and three ways out.*

The last step is deciding "what level of risk you are going to be willing to accept," Jarmak said. In the software factory, when the agent seeks autonomy for a lower-risk decision, it states the next action, why a human needs to weigh in, the operating scope, and which human check it proposes to remove. The slide framed a proposed autonomy decision as "additional action + operating scope + human check removed," tested against an evidence bar (representative repeated outcomes; tolerable failures and total burden), a validated evaluator with execution controls ("accept good work, detect defects and enforce the operating boundary"), and a human owner who decides acceptable risk and conflicting business values. There are three outcomes: expand within the tested boundary, retain review, or stop and escalate "for missing evidence, a failed gate or changed scope." The goal is to define "when is it safe to autonomously deploy versus stop and escalate to the human."

## 12. Resources

![Evaluation procedures and references](../slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/12.jpg)
*Jarmak's skills repository and book.*

Jarmak closed with two resources. The GitHub repository `sjarmak/coding-agent-workflows` contains a set of agent skills, including three procedures directly relevant to the talk: evaluation design, harness traceability and reliability procedures. And the book, which Jarmak called "this monstrous 300-page arXiv tome that you can point your agents at," *Engineering Reliable Coding Agents*, published on arXiv in August 2026, opens: "AI coding agents are commonly evaluated as models but deployed as systems." "You can point your agents at all of this to pull in some of those best practices into your own systems," Jarmak said.

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=b1fe8396-6fa8-4574-9775-0dc01f56ce57) · [Video: Don't Trust Your AI Agents. Test Them Like a Scientist](https://www.youtube.com/watch?v=A3FL_zTQKU0) · [Engineering Reliable Coding Agents (arXiv:2608.13867)](https://arxiv.org/abs/2608.13867) · [Online edition](https://www.sjarmak.ai/books/engineering-reliable-coding-agents) · [coding-agent-workflows](https://github.com/sjarmak/coding-agent-workflows) · [CodeProbe](https://github.com/sjarmak/codeprobe) · [Li et al., Finding the Right Fit: Model-Harness Interactions across Agent Tasks](https://arxiv.org/abs/2610.00917) · [Omni's Blobby](https://docs.omni.co/docs/ai/queries)

The schedule abstract promised material that does not appear in the video or the slides: an audience vote between two release candidates, a +5.4-point win in a repository-level eval that shrank to +0.35 after repeated runs, a coding benchmark whose sponsor stopped reporting it, and model graders that agreed with each other while missing defects. The video ends with applause and no Q&A. This write-up is based on the official video captions, the author's room recording and slide photos (the title slide and slide 10 were not photographed), and the sources above. The [precise notes](precise/10-13-1155-lies-damned-lies-and-evals-how-to-trust.md) (Chinese) keep timestamps and the full record of where the sources disagree.
