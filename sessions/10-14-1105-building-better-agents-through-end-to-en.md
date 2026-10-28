**English** | [中文](zh/10-14-1105-building-better-agents-through-end-to-en.md)

# Ramp Spent Months on Accounting Evals, and the Payoff Came From Memory and Skill Ablation, Not Auto-Tuning

*At AI Engineer NYC 2026, Ramp's Ryan Stevens walked through how the company built a benchmark of synthetic accounting businesses for its Ramp Stack agent, and which experiments run against it helped and which did not.*

**Breakout Session · Wednesday, October 14, 2026 · 11:05–11:23am · Shannon Room (NY Ballroom E)**
**Speaker:** Ryan Stevens, Director of Applied Science, Ramp

---

Ryan Stevens leads Ramp's applied scientists and analytics engineers. The talk was about Ramp Stack, Ramp's AI system for accounting firms, and about the evaluation work behind it. The session's official description set up the problem: "More context should make an AI agent better… it often did the opposite."

Every slide carried a small header reading "AIRFLOW @ RAMP"; the talk did not explain it. The deck was organized in three parts: "What is your agent's goal?", "Designing your evals" and "Getting value from your evals."

## 1. Understand the job before you build the agent

![Biggest mistake people make is agentifying a process they don't understand](../slides/10-14-1105-building-better-agents-through-end-to-en/01.jpg)
*The opening slide: a meme reading "I have no idea how to do accounting / But I built an agent for it."*

The first slide named what Stevens called the biggest mistake: "agentifying a process they don't understand." (The author's recording begins after this slide, so how Stevens introduced it was not captured.)

![For a job, map out the tasks and what success is](../slides/10-14-1105-building-better-agents-through-end-to-en/02.jpg)
*One job, three tasks, each with its own definition of success.*

"The first task in building an agent for a process is breaking down the job, the tasks, and what success looks like," Stevens said. For a bookkeeper, the job is to close the books. The slide broke it into three tasks:

| Task | Success means |
| --- | --- |
| Reconcile cash | Adjusted bank and ledger balances agree. |
| Record accruals | Unpaid expenses are recorded in the correct period. |
| Explain variances | Material changes are identified and supported by evidence. |

These became categories in Ramp's benchmark, and Stevens said a fourth would appear later.

![We map the exact process a human follows, then translate it to an agent](../slides/10-14-1105-building-better-agents-through-end-to-en/03.jpg)
*A reconciliation: QuickBooks on one side, Ramp on the other, an Excel worksheet in between.*

Once the tasks and success criteria were set, the team "sat down with bookkeepers, we sat down with accounting firms" to learn the full end-to-end process for each task. The slide's example was a reconciliation. On one side is the ERP (here QuickBooks) holding the accounts payable ledger, the bills a company has to pay; on the other is the system that pays them (Ramp). The two must show the same numbers. "If you don't know the term accounts payable, I didn't until a few years ago either," Stevens said. An accountant pulls both sides into Excel row by row, checks whether they tie, works out why they don't, and makes them tie. The slide's worksheet showed three bills:

| Bill | QuickBooks | Ramp | Status |
| --- | --- | --- | --- |
| 101 | $1,000 | $1,000 | Match |
| 102 | $2,000 | $2,000 | Match |
| 103 | Missing | $500 | $500 gap |

Below it, a box labeled "With an agent" automated the same reconciliation. "Pretty easy to agentify this process if you understand exactly what it is," Stevens said.

## 2. Designing the evals

![Agenda: 02 Designing your evals](../slides/10-14-1105-building-better-agents-through-end-to-en/07.jpg)
*The agenda slide marking the second part of the talk.*

Stevens said the talk would use "benchmarks, evals, verifiability" interchangeably, "because frankly I think it's a soup of language."

### Four task categories

![We wanted to build a benchmark, so we designed four task categories](../slides/10-14-1105-building-better-agents-through-end-to-en/04.jpg)
*The benchmark's four categories and task counts (the photo was taken upside down).*

| Category | Definition (from the slide) | Tasks |
| --- | --- | --- |
| Reconciliation | Tie the two sources together, identify differences, and explain or correct the breaks. | 61 |
| Data Entry | Post or update transactions, journal entries, vendor bills, and invoices. | 26 |
| Variance Analysis | Compare actuals against budget, prior periods, or expectations and explain the drivers. | 28 |
| Schedules & Accruals | Build or update supporting schedules, calculate accruals, and carry the right balances into the close. | 45 |
| **Overall** | | **160** |

Variance analysis, Stevens said, is about "why do numbers move." Stevens did not go into schedules and accruals beyond calling it "a complex thing that an accountant is doing."

The 160 total matches Mercor's public APEX-Accounting benchmark, described as "160 tasks across 10 simulated companies" and built with Ramp. Ramp's own "Stack Benchmarking" blog post describes 237 tasks across eight worlds with 3,469 grading criteria. No source explains how the two sets relate.

### Before users, synthetic data

![Building your evals is different depending on the product life cycle](../slides/10-14-1105-building-better-agents-through-end-to-en/06.jpg)
*Before and after users. The red outline marks the data side: "Data is the bottleneck to evals."*

The slide showed two pipelines. Before users: synthetic business data (bills, payments, ledger) feeds a database holding simulated state, which tools read and the agent calls. With users: real customer data feeds a database holding customer state, with the same tools and agent. A second version of the slide circled the data and database half and labeled it "Data is the bottleneck to evals."

"Remember all agents are doing is they're calling tools that hit a database, that's it," Stevens said. Without users, "you need simulation, you need synthetic data. I would highly recommend anytime you're building an agent for an area where you don't have enough users you should build synthetic data." Ramp built its benchmark with Mercor: "They were a great partner for this. You don't have to build it with a company but simulation is pretty important." The with-users phase was out of scope: "I only have 18 minutes." On the bottleneck, Stevens said it was "an obvious statement," but "to evaluate you have to have clean data."

### Synthetic businesses

![We built synthetic businesses with payments, bills, and ERP data](../slides/10-14-1105-building-better-agents-through-end-to-en/08.jpg)
*A company profile for one synthetic business and the documents built for it.*

The example business on the slide was Velo Goods Co., described as a DTC lifestyle and home goods brand selling through Shopify, Amazon FBA and wholesale to specialty retailers, with about $4M in annual revenue and 12 employees, run as a single-entity LLC on QuickBooks Online with Gusto for payroll and a third-party logistics provider. A table beside the profile listed the documents built for it in QuickBooks Online, including a chart of accounts (about 60 accounts), customer and vendor lists (25 to 30 vendors), 12 months of wholesale invoices on Net 30 terms, and bills. "They have a calendar year. They want to do just close at the end of the month. They're multi-channel," Stevens said. The team built "everything that would be needed for a synthetic business in order to do good offline evaluation."

### Evals are backtests

![Lots of words for same concept: we want to backtest agents and measure success](../slides/10-14-1105-building-better-agents-through-end-to-en/09.jpg)
*The Spider-Man pointing meme, with "Evals," "Benchmarks," "Labels" and "Verifiable outputs" pointing at each other.*

Stevens, who said "my training is as an MLE" (machine learning engineer), argued that the vocabulary hides a simple goal. Evals, verifiability and benchmarks "are just trying to find labels… really, you're just looking to run back tests," running agents "against the same set of data day over day over day to find ways to optimize it."

Coding agents, in Stevens's account, have it easy, "because git by nature allows you to replay things": past state is always captured. Production agents raise a harder question: "How do you capture state over time so you can replay?" Before launch, the team builds that state and holds it fixed.

### Task, world, trajectory, score

![For each business, we designed a series of accounting tasks, rubric and LLM as a judge](../slides/10-14-1105-building-better-agents-through-end-to-en/10.jpg)
*One eval case from task to score, graded on the final output only.*

The slide traced one case:

| Stage | Content |
| --- | --- |
| Task | "Reconcile our AP payables and bills on Ramp for Q4 2025." |
| World | Synthetic ERP data (AP ledger, AP aging, credits and adjustments) plus synthetic business data (Ramp bills, Ramp payments, vendor invoices) |
| Trajectory | Read bills and AP data → match bills and payments → investigate differences → final output: an AP reconciliation report |
| Score (final output only) | 67%, 4 of 6 criteria met |

The six criteria were Q4 AP balance correct, bills matched, payments matched and exceptions listed (met), and duplicates flagged and timing gaps explained (not met).

The data environments are called worlds, "a term that Mercor came up with, and I quite like it." A world could also be a snapshot of production data. Each task had "anywhere from 10 to 20 criteria," graded by an LLM judge against a rubric. (Mercor's page gives an average of 13.7 criteria per task.) Ramp deliberately did not grade the trajectory, whether the right tools or skills were used or the path was efficient: "When you're initially building your agent you just want it to be right, to be cheap and to be fast; how it gets there is a little bit less important."

Building all of this took "four to six months before launching this agent," Stevens said, and that investment is typical for areas "slightly orthogonal to what [frontier models are] optimized against." A coding agent, by contrast, will "probably work… out of the box."

### Accuracy and Pass@k

![Two complementary ways to measure performance](../slides/10-14-1105-building-better-agents-through-end-to-en/11.jpg)
*Accuracy gives partial credit; Pass@k asks whether any attempt got everything right.*

The slide set two metrics side by side. Accuracy asks "How many criteria are correct?": four of six is 67%, and "partial credit counts." Pass@k asks "Did any attempt get everything right?" In the slide's example, three attempts scored 4/6, 5/6 and 6/6, so Pass@1 = 0, Pass@2 = 0 and Pass@3 = 1: "One fully correct attempt is enough (k = 3)."

"Pass@k is an all or nothing metric," Stevens said. Where the cost of error is high, as in accounting, "Pass@k might be the thing you're optimizing for… if an accountant can't trust that one of the steps is wrong then they may not use your agent." (Ramp's blog post also defines a stricter metric, Pass^5, which requires all five independent attempts to satisfy every criterion. It appears on the next chart but was not discussed on stage.)

## 3. Getting value from the evals

### Choosing a model

![First obvious use case is to decide which models to use](../slides/10-14-1105-building-better-agents-through-end-to-en/12.jpg)
*Model comparison on Accuracy, Pass@1, Pass@5 and Pass^5, raw models run without Stack's skills or memory.*

The first use is picking a model. The chart compared Ramp Stack with Opus 4.7, Opus 4.6, GPT 5.5, GPT 5.4, Gemini 3.5 Flash and Gemini 3.1 Pro, with 95% confidence intervals from bootstrapping over worlds. Exact bar heights can't be read from the photo; roughly, accuracy sat mostly between 45% and 65% with Ramp Stack highest, Pass@1 between about 5% and 22%, Pass@5 between about 12% and 40%, and Pass^5 below 15% for every model.

"This is from a blog post we wrote when Stack launched," Stevens said. "At the time, the Opus models were the highest performing. That's still what we see when we rerun these benchmarks." Stevens added that "five-five is the best model," without saying on the recording whether that meant GPT 5.5 or Opus 5.5. (Ramp's blog post says Stack runs on GPT 5.5 with skills and memory, that it beat the other agents by 4% on accuracy and 3% on Pass@1, and that GPT 5.5 was nearly 19 points more accurate than GPT 5.4.) The key point was that "accuracy is very high. Pass@1 is very low. These aren't saturated yet," which leaves room for harness work to add Pass@k.

### Why models fail

![We dig deeper into why models are failing to spot optimizations](../slides/10-14-1105-building-better-agents-through-end-to-en/13.jpg)
*Failure counts by type for three frontier models.*

| Failure type | Muse-Spark-1.1 (xHigh) | Claude-Fable-5 (Max) | GPT-5.6-Sol (Max+ Pro) |
| --- | --- | --- | --- |
| Reasoning | 13 | 19 | 21 |
| Information-gathering | 5 | 3 | 2 |
| Instruction-following and prompt understanding | 3 | 1 | 4 |
| Planning and reflection | 1 | 1 | 1 |

For accounting, failures are mostly reasoning failures. "These harnesses are pretty good at actually gathering the right amount of data," Stevens said; accounts payable is "something I could explain to like a five-year-old." The conclusion: "The models, they just ain't that smart, and we've got some room to run here." Stevens said Muse Spark "actually did really well in this." (These three models are a different set from the previous chart, which came from the launch-era blog post. They are the three leaders on Mercor's APEX-Accounting leaderboard, but the slide does not say whether the analysis came from that benchmark.)

### What didn't work

![We ran a lot of experiments, here's what didn't work](../slides/10-14-1105-building-better-agents-through-end-to-en/14.jpg)
*Five experiments that failed to help.*

"You'll see papers of like these awesome things that they did… and they don't show you the 30 things that failed," Stevens said.

| Experiment | Change tested | Result |
| --- | --- | --- |
| Model selection | Gemini, GPT-5.4, GPT-5.5; Claude Opus vs. Sonnet | Benchmark quantified model performance gaps |
| Auto-tuned skills (GEPA) | Automated skill edits with autoresearch | Training gains did not generalize |
| High reasoning effort | High vs. low effort across model combinations | Same accuracy; higher cost |
| Minimal skill bundle | PDF + Excel only vs. SLIM 4-skill baseline | Below the 4-skill baseline |
| Conservative rewrites | Shorter, more cautious finance skills ("xsafe") | No gain over standard skills |

Stevens took a "mini side quest" on the GEPA row, auto-tuning skills. GEPA (Genetic-Pareto) is a prompt optimizer from a 2025 paper that samples a system's trajectories, reflects on them in natural language and proposes prompt edits. For Ramp it was "no good. It might be our agent, but I think the idea of hill climbing with evals on text space is not the first thing I would do here. Intuitively, it's difficult to understand why it would work, i.e. how you can get direction on a gradient. We did not see success here. We've still not seen success."

Stevens wanted to call it out because "there's a lot of noise on auto-research." Andrej Karpathy's autoresearch is about tuning ML models, "which obviously it has a gradient, it has a direction, it's also a finite space it's searching over." The lesson: "Maybe don't think that auto-research is going to solve your bad agent problem."

### What worked

![Here's some experiments that did work](../slides/10-14-1105-building-better-agents-through-end-to-en/15.jpg)
*Three experiments that helped.*

| Experiment | Change tested | Result |
| --- | --- | --- |
| Skill ablation | Trim 8 skills to a 4-skill SLIM bundle | +2.8pp mean score vs. no skills, 6 worlds |
| Trajectory-distilled memory | Distill prior runs into memory; remove period-specific amounts | +5.7pp mean score vs. skills alone, 34 tasks |
| Excel skill compression | Shrink Excel skill by 61%; preserve useful instructions | ≈0pp quality change, $30k/month saved |

Stevens discussed only the first two. The Excel compression row and its $30,000-a-month saving appeared on the slide but not in the talk. (The session description and Ramp's blog put that compression at 14k to 5k characters, about 64%, against the slide's 61%. The blog gives the slim bundle's gain over no skills as +2.3pp, against the slide's +2.8pp.)

### Skill ablation

![Compare skill versus a no-skill baseline](../slides/10-14-1105-building-better-agents-through-end-to-en/16.jpg)
*Average impact of each skill relative to a no-skill baseline.*

Stevens borrowed the idea from feature ablation in machine learning: leave one feature out, or add features back in, to measure "the incrementality of a given feature on model performance." Ramp did it for skills; tools are harder but possible. The goal was to "trim the number of skills, saving context." The chart:

| Skill | Improvement relative to no-skill baseline |
| --- | --- |
| A | +0.55% |
| B | +0.47% |
| C | −0.04% |
| D | −0.72% |
| E | −0.76% |
| F | −1.33% |
| G | −1.67% |

On stage, Stevens said, "I think I've reversed these actually. I think the negatives are like, it's good with the skill and the positives is bad with the skill." Ramp's blog post reads the chart as drawn: only skills A and B were positive on average, C was essentially neutral, and the rest were negative. Either way, Stevens said, "there's definitely differences," and the results became the candidate list for "maybe we should delete these skills, compress these skills." The advice: "Skill ablation, you should do it. Tool ablation, you can also do."

### Testing memory with roll-forward worlds

![People don't test memory systems, we designed a unique way to do that: Roll Forward Worlds](../slides/10-14-1105-building-better-agents-through-end-to-en/17.jpg)
*Two example businesses carry learned rules into the next month; memory on scored 61.5% against 56% off.*

"People don't really test their memory systems," Stevens said; they compress information, store it and read it back. Ramp's approach, roll-forward worlds, runs the same task over two periods. After the first period the agent writes a memory; in the second, the memory is either injected ("if you wanna cheat a little bit") or the agent searches its memories.

The slide gave two examples. A construction company matches records in month one and learns customer rules: how to count income on unfinished jobs and which job each cost belongs to. A restaurant group learns how sales match bank deposits and which store each sale belongs to. In the next month, with the same business and new records, the agent applies those rules to calculate new numbers. The slide's motto: "Remember how the business works. Use fresh numbers. Last month's amounts are not this month's answers." The rules are not written down anywhere, but they can be inferred from the success criteria: "you have to learn these rules in order to be successful at this task."

The bar chart showed memory off at 56% and memory on at 61.5%, a gain of 5.5 points. (The "did work" table gave +5.7pp over 34 tasks; the two may be different slices.) "There was a ton of alpha here," Stevens said. The memories written depended heavily on the memory prompt, and so did success. "This was actually one of the biggest gains we saw."

The talk covered only the gain. Ramp's blog post also reports a counterexample: on one billing task, an agent without memory scored 75% while the memory-enabled agent fell to 25%, because the memory carried over the old task's specific facts. The session description's line that memory should hold "reusable process, not stale facts" refers to this.

### Regression testing in production

![Once it's in production, internal eval platform runs on a schedule to catch regressions](../slides/10-14-1105-building-better-agents-through-end-to-en/18.jpg)
*Ramp's internal eval platform: choose cases, set a schedule, catch regressions.*

"The thing that I think people don't do enough of is what I call regression testing," Stevens said. "I'm not a big believer in CI gating based off of agentic evals. As someone that's seen a lot of non-determinism, a lot of 80%, 70% passage, CI gating is not usually the best idea."

Instead, Ramp hosts its evals on an internal platform others can add to. The screenshot showed three steps. First, choose cases, trials and model: cases filtered by tag (20 tags, with examples such as `excel`, `pdf` and `canonical_benchmark`), three trials per case, Stack's default model and reasoning effort, and a button to "Run 206 matching cases." Second, set a recurring schedule: a job called `stack-scheduled-evals` running at 2:00 AM New York time on Mondays and Thursdays. Third, catch regressions automatically: run the benchmark on the same cases and starting worlds, compare accuracy and Pass@k with a baseline, and review failed tasks and traces. A background agent reviews failures and posts to Slack: "It looks like something's regressed. Go investigate it. Here's what I think broke."

## 4. Takeaways

![What are the takeaways](../slides/10-14-1105-building-better-agents-through-end-to-en/19.jpg)
*The closing slide's four points.*

"I'm at time," Stevens said, and read the four points on the final slide: break a job down into discrete jobs to be done with clear success criteria; build realistic data environments and tasks for offline evaluation; extract as much value as possible from those offline efforts; and make sure offline evaluations are consistently used to judge agent performance.

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=4607dbbe-8f94-4b0e-91ce-257d1cfb5448) · [Stack Benchmarking](https://builders.ramp.com/post/stack-benchmarking) (Ramp Builders) · [Ramp Stack launch](https://ramp.com/blog/ramp-stack-launch) · [Introducing APEX-Accounting, built with Ramp](https://www.mercor.com/apex/newsletter/introducing-apex-accounting-built-with-ramp/) (Mercor) · [GEPA: Reflective Prompt Evolution Can Outperform Reinforcement Learning](https://arxiv.org/abs/2507.19457) · [GEPA code](https://github.com/gepa-ai/gepa) · [karpathy/autoresearch](https://github.com/karpathy/autoresearch)

No video of this session had been published at the time of writing. This write-up is based on the author's room recording, 18 of the author's 19 slide photos (one near-duplicate omitted), and the sources above. The recording ends with Stevens's thanks; there was no Q&A. The [precise notes](precise/10-14-1105-building-better-agents-through-end-to-en.md) (Chinese) keep timestamps and the full record of where the talk, slides and Ramp's blog disagree.
