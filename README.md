# AI Engineer NYC 2026: Conference Report

My notes from **[AI Engineer NYC 2026](https://ai.engineer/nyc/2026)** (October 12–14, 2026, Sheraton New York Times Square), an event focused on production AI in banking, hedge funds, fintech, insurance and asset management.

This repository lists the 38 sessions I attended: who spoke, what each talk covered, what I took away, and a link to the recording where one exists. It is the companion reference for my conference write-up and talk.

- ⭐ marks a session I starred as a highlight while attending.
- **📰 Full write-up** (in `sessions/`) is a complete English article for each session: every section and slide of the talk, data from the slides as tables, and the Q&A where there was one. **Precise notes** (in `sessions/precise/`, Chinese) are the underlying verified record with timestamps, speech-to-text corrections and source-by-source discrepancies.
- Some talk titles were changed when the video was published; the schedule title is used here and the video title is noted when different.

## Where to watch

| Source | Link |
| --- | --- |
| AI Engineer YouTube channel (all videos) | <https://www.youtube.com/@aiDotEngineer/videos> |
| AIE NYC 2026 Complete Playlist | <https://www.youtube.com/playlist?list=PLLKkCXQeuCis> |
| AI Engineer NYC 2026: Online Talks | <https://www.youtube.com/playlist?list=PLVanfCmVOgIg> |
| Day 1 livestream (Oct 13, Main Stage) | <https://www.youtube.com/watch?v=akEqiXFo3GM> |
| Day 2 livestream (Oct 14, Main Stage) | <https://www.youtube.com/watch?v=jiUC6IUMdvc> |
| Official schedule | <https://ai.engineer/nyc/2026/schedule> |

For Main Stage talks, the livestream links jump to the chapter marker the organizers published for that talk.

## Contents

- [Sessions at a glance](#sessions-at-a-glance)
- [Monday, October 12](#monday-october-12) (5 sessions)
- [Tuesday, October 13](#tuesday-october-13) (19 sessions)
- [Wednesday, October 14](#wednesday-october-14) (14 sessions)

## Sessions at a glance

| Day | Time | Session | Speaker | Video |
| --- | --- | --- | --- | --- |
| October 12 | 9:00am–11:00am | [Orchestrating Claude Managed Agents](#orchestrating-claude-managed-agents) | CJ Avilla, Harrison Stall | [▶](https://www.youtube.com/watch?v=kiClTcJUnhc) |
| October 12 | 11:05am–12:05pm | [Agent = Model + Harness: Build your own Memory-Aware AML AI Agent Harness for Financial Crime Detection](#agent--model--harness-build-your-own-memory-aware-aml-ai-agent-harness-for-financial-crime-detection) | Nacho Martinez | [▶](https://www.youtube.com/watch?v=KtBivPzmOM8) |
| October 12 | 1:15pm–2:15pm | [Beyond “Done”: Building Verification Loops for AI Agents](#beyond-done-building-verification-loops-for-ai-agents) | Nick Nisi, Zack Proser | [▶](https://www.youtube.com/watch?v=q4WC4TowkBo) |
| October 12 | 2:20pm–3:20pm | [Langfuse Workshop - Evals and Observability in regulated Industries](#langfuse-workshop---evals-and-observability-in-regulated-industries) | Doneyli De Jesus, Annabell Schäfer | [▶](https://www.youtube.com/watch?v=obfPzbapMIM) |
| October 12 | 3:30pm–4:30pm | [Unhobbling the Frontier](#unhobbling-the-frontier) | Lance Martin | – |
| October 13 | 9:00am–9:18am | [Vault: Making Unstructured Data Work for Investing](#vault-making-unstructured-data-work-for-investing) | Aaron Linsky, Sam Green | [▶](https://www.youtube.com/watch?v=CLve4Nx_RwI) |
| October 13 | 9:18am–9:36am | [Anthropic: Accelerating frontier AI in Financial Services.](#anthropic-accelerating-frontier-ai-in-financial-services) | Belinda Neal, Keon Shahab | – |
| October 13 | 9:36am–9:54am | [Balancing Acceleration and Safety for AI Agents](#balancing-acceleration-and-safety-for-ai-agents) | John Feminella | [▶](https://www.youtube.com/watch?v=iniryju23Sk) |
| October 13 | 9:54am–10:12am | [Does Code Quality Even Matter?](#does-code-quality-even-matter) | Max Kanat-Alexander | [▶](https://www.youtube.com/watch?v=bU5gDVyu_28) |
| October 13 | 10:12am–10:30am | [What does an AI engineer do?](#what-does-an-ai-engineer-do) | Aparna Dhinakaran | [▶](https://www.youtube.com/watch?v=ImnZ7s68jw8) |
| October 13 | 10:40am–10:58am | [Can Any Model Write Like Me? Building a Benchmark for Creative Writing](#can-any-model-write-like-me-building-a-benchmark-for-creative-writing) | Louis-François Bouchard | [▶](https://www.youtube.com/watch?v=_dTF3NpPzGo) |
| October 13 | 11:05am–11:23am | [Write the Answer First: Generating Eval Datasets for Agents With No Benchmark](#write-the-answer-first-generating-eval-datasets-for-agents-with-no-benchmark) | Nathanael Weill | [▶](https://www.youtube.com/watch?v=c_SPW__jF9w) |
| October 13 | 11:30am–11:48am | [Your Financial Agent Passed Every Eval. Can You Defend Its Decision?](#your-financial-agent-passed-every-eval-can-you-defend-its-decision) | Anirudh Veeraragavan | – |
| October 13 | 11:55am–12:13pm | [Lies, Damned Lies, and Evals: How to trust your agents when the stakes are high](#lies-damned-lies-and-evals-how-to-trust-your-agents-when-the-stakes-are-high) | Stephanie Jarmak | [▶](https://www.youtube.com/watch?v=A3FL_zTQKU0) |
| October 13 | 12:20pm–12:38pm | [1,368 AI Failures: What They Teach Us About System Evals](#1368-ai-failures-what-they-teach-us-about-system-evals) | Vishal Srivastava, Tanmay Sah | [▶](https://www.youtube.com/watch?v=mMw3arwQsL0) |
| October 13 | 1:40pm–1:58pm | [Proving Alpha: Showcasing AI's Value in Fundamental Investing](#proving-alpha-showcasing-ais-value-in-fundamental-investing) | Hari Kumar | [▶](https://www.youtube.com/watch?v=ZMZlAn6Pyfg) |
| October 13 | 2:05pm–2:23pm | [Can AI Understand a Distressed Credit?](#can-ai-understand-a-distressed-credit) | Harneet Kaur | [▶](https://www.youtube.com/watch?v=WOszaC1WWbQ) |
| October 13 | 2:30pm–2:48pm | [Why Frontier Models Fail on Billion Dollar Portfolios](#why-frontier-models-fail-on-billion-dollar-portfolios) | Chanyeol Choi, Janie Gu, Jack Haverty, Suyeol Yun | [▶](https://www.youtube.com/watch?v=aVbRSIQO2dE) |
| October 13 | 2:55pm–3:13pm | [AI and the Future of Fundamental Investing](#ai-and-the-future-of-fundamental-investing) | Jack Woodruff, Vinoo Ganesh | [▶](https://www.youtube.com/watch?v=akEqiXFo3GM&t=19744s) |
| October 13 | 3:20pm–3:38pm | [Your agent fix just broke something else](#your-agent-fix-just-broke-something-else) | Sofia Jakovcevic | [▶](https://www.youtube.com/watch?v=v5tm5EAzsAs) |
| October 13 | 3:45pm–4:03pm | [Give the Model a Computer: The One Thing Every AI Agent Framework Agrees On](#give-the-model-a-computer-the-one-thing-every-ai-agent-framework-agrees-on) | Mislav Ivanda | – |
| October 13 | 4:30pm–4:48pm | [A Hedge Fund as a Function](#a-hedge-fund-as-a-function) | Frank Long | [▶](https://www.youtube.com/watch?v=akEqiXFo3GM&t=24466s) |
| October 13 | 4:48pm–5:06pm | [Frontier Models for Acting in Space and Time](#frontier-models-for-acting-in-space-and-time) | Pim de Witte | [▶](https://www.youtube.com/watch?v=akEqiXFo3GM&t=25339s) |
| October 13 | 5:06pm–5:24pm | [Hermes gives you wings: Build on open foundations.](#hermes-gives-you-wings-build-on-open-foundations) | Dillon Rolnick | [▶](https://www.youtube.com/watch?v=akEqiXFo3GM&t=26480s) |
| October 14 | 10:40am–10:58am | [The Hidden Costs of a Faster Model](#the-hidden-costs-of-a-faster-model) | Morgan Suo | [▶](https://www.youtube.com/watch?v=YvQ4BVTWfHI) |
| October 14 | 11:05am–11:23am | [Building Better Agents Through End-to-End Evals](#building-better-agents-through-end-to-end-evals) | Ryan Stevens | – |
| October 14 | 11:30am–11:48am | [We Let Our Financial Planners Write the Agents](#we-let-our-financial-planners-write-the-agents) | Matt Ragone | [▶](https://www.youtube.com/watch?v=gpmze-6ueGU) |
| October 14 | 11:55am–12:13pm | [The Model Ate the Coding Harness. Now What?](#the-model-ate-the-coding-harness-now-what) ⭐ | Jediah Katz | [▶](https://www.youtube.com/watch?v=7Y4sbZgBdnc) |
| October 14 | 12:20pm–12:38pm | [A/B Testing AI Products Is Weird: What Changes When the Product Is Probabilistic](#ab-testing-ai-products-is-weird-what-changes-when-the-product-is-probabilistic) | Supriya Subudhi | – |
| October 14 | 1:40pm–1:58pm | [Building a self-improving software factory](#building-a-self-improving-software-factory) ⭐ | Zach Lloyd | – |
| October 14 | 2:05pm–2:23pm | [Scaling Agentic RL Across the Globe](#scaling-agentic-rl-across-the-globe) ⭐ | Jason Mancuso | – |
| October 14 | 2:30pm–2:48pm | [Your agent needs a skill to improve its skills](#your-agent-needs-a-skill-to-improve-its-skills) ⭐ | Ben Holmes | – |
| October 14 | 2:55pm–3:13pm | [Build your own team of managed agents](#build-your-own-team-of-managed-agents) | Jack Wotherspoon | [▶](https://www.youtube.com/watch?v=Y_p3y6zP5QE) |
| October 14 | 3:20pm–3:38pm | [A Roadmap for Evaluating Highly-Subjective Tasks](#a-roadmap-for-evaluating-highly-subjective-tasks) | Cameron Wolfe | – |
| October 14 | 3:45pm–4:03pm | [Code Mode: your agent should write code](#code-mode-your-agent-should-write-code) | Mikyo King | [▶](https://www.youtube.com/watch?v=dAoOYFx7ZYw) |
| October 14 | 4:30pm–4:48pm | [AI in Practice: An Inside Look at Apollo's Approach](#ai-in-practice-an-inside-look-at-apollos-approach) | Michael Manapat, Jack McDonald | [▶](https://www.youtube.com/watch?v=BOWYujhIQuo) |
| October 14 | 4:48pm–5:06pm | [Process Supervision for Agents That Do Long-Horizon Work](#process-supervision-for-agents-that-do-long-horizon-work) ⭐ | Mitchell Troyanovsky | [▶](https://www.youtube.com/watch?v=jiUC6IUMdvc&t=29362s) |
| October 14 | 5:06pm–5:24pm | [Everything will be Simulated](#everything-will-be-simulated) | Cristóbal Valenzuela | [▶](https://www.youtube.com/watch?v=jiUC6IUMdvc&t=30551s) |

## Monday, October 12

### Orchestrating Claude Managed Agents

**Speakers:** CJ Avilla (Member of Technical Staff, Anthropic); Harrison Stall (Member of Technical Staff, Anthropic)  
**When / where:** 9:00am–11:00am · Sutton Room (NY Ballroom W) · Workshop (1-2 hrs)  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=c415d432-5b6f-4b4a-9d2c-2e02cfde99db) · [▶ Video](https://www.youtube.com/watch?v=kiClTcJUnhc)

**What it covers.** Two-hour workshop on dynamic workflows in Claude Managed Agents and the controls that keep them in bounds. The recording frames the problem as the same coordination wall Unix, OS/360 and Linux hit, now applied to running many agents at once. (The video was retitled “Why One AI Agent Isn’t Enough (And How to Run 1,000)”.)

<details><summary>Official talk description</summary>

> Dynamic workflows in Claude Managed Agents, and the controls that keep them in bounds.

</details>

### Agent = Model + Harness: Build your own Memory-Aware AML AI Agent Harness for Financial Crime Detection

**Speaker:** Nacho Martinez (AI Developer Advocate, Oracle)  
**When / where:** 11:05am–12:05pm · Sutton Room (NY Ballroom W) · Workshop (1-2 hrs)  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=898cb76b-41f3-47c0-ba31-6a5f8df40117) · [▶ Video](https://www.youtube.com/watch?v=KtBivPzmOM8) · [Workshop site (exercises)](https://workshopwaitingroom.com/) · [Speaker GitHub](https://github.com/jasperan)

**What it covers.** The model is the part you can’t change; the harness is where agent quality is won or lost. The workshop builds that harness from primitives on Oracle AI Database with an AML dataset: long-term memory and schema scanning, semantic and hybrid retrieval, a vector-indexed toolbox and skillboxes, and a bounded agent-turn loop. (The video was retitled “Memory-Aware Agent Harnesses for Anti-Money Laundering”.)

<details><summary>Official talk description</summary>

> Agent = Model + Harness. The model emits tokens; state, memory, retrieval, tool dispatch, identity, retries, and budgets. All these concepts are harness code — and that is where agent quality is won or lost.
>
> In this hands-on workshop, you build that harness from primitives against Oracle AI Database and a realistic AML dataset: Meridian Bank, 25 branches, ~1,200 transactions over 90 days, and transactions flagged by Anti Money Laundering (AML) rules.
>
> We will go through long-term memory and schema scanning with OAMP, semantic and hybrid RRF retrieval with ONNX in-database embeddings, vector-indexed toolbox and skillboxes, and a bounded agent_turn loop.
>
> After this workshop, you will know everything required to build your own agent harness from scratch, staying faithful to best agent harness and AI practices.
>
> You leave with a readable ~100-line loop you can operate, and a mental model that you can improve for your financial AI agents.

</details>

### Beyond “Done”: Building Verification Loops for AI Agents

**Speakers:** Nick Nisi (Engineering Manager, WorkOS); Zack Proser (AI Engineer, WorkOS)  
**When / where:** 1:15pm–2:15pm · Sutton Room (NY Ballroom W) · Workshop (1-2 hrs)  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=edb4f982-0cb8-48ee-8bf7-410255a40cb3) · [▶ Video](https://www.youtube.com/watch?v=q4WC4TowkBo) · [Slides & exercises (workshop workbench; returned 401, may need a login)](https://talks.workos.cloud/beyonddone)

**What it covers.** Build a verification and repair loop around an AI agent: Playwright checks browser behavior, an evaluator judges whether the collected evidence supports each acceptance criterion, and the loop accepts the work, requests a bounded repair, or escalates to a human. Then try to fool it with deliberately broken outputs to test the verifier itself. (The video was retitled “Teach Your Agent to Prove It’s Done”.)

<details><summary>Official talk description</summary>

> Delegating work takes more than a capable model and a good prompt. It takes a system that can check results, catch unsupported claims, and decide when to retry or ask for help.
>
> In this hands-on, 60-minute workshop, you’ll build a verification and repair loop around an AI agent. We’ll use Playwright to check browser behavior and Jev to evaluate whether collected evidence supports specific acceptance criteria. Your code will combine those signals to accept work, request a bounded repair, or escalate to a human.
>
> Then we’ll try to fool it. Using deliberately broken outputs and misleading evidence, you’ll test not just the agent, but the verifier itself, and explore the trade-off between catching failures and interrupting people unnecessarily.
>
> Drawing on real projects involving delegated work and agent supervision, this workshop focuses on patterns you can apply with your own stack. You’ll leave with a working verification loop and a practical method for deciding what to automate, what to check, and where human judgment still belongs.

</details>

### Langfuse Workshop - Evals and Observability in regulated Industries

**Speakers:** Doneyli De Jesus (Head of AI Engineering Solutions, Langfuse (by ClickHouse)); Annabell Schäfer (AI Engineering Solutions, Langfuse by ClickHouse)  
**When / where:** 2:20pm–3:20pm · Sutton Room (NY Ballroom W) · Workshop (1-2 hrs)  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=f20b8d60-8a75-4ab3-a2fb-42159024e65f) · [▶ Video](https://www.youtube.com/watch?v=obfPzbapMIM) · [Workshop materials](https://langfuse.com/workshop)

**What it covers.** Closing the loop from demo to production in a regulated industry, using a banking support agent (Northwind Bank): end-to-end tracing, guardrails that block and record risky requests, quality and compliance evals on datasets in CI/CD, and a dashboard of top failure modes. (The video was retitled “Evals for High-Stakes AI Agents”.)

<details><summary>Official talk description</summary>

> Building an AI agent demo is easy. Getting it into production is hard: you need to know what it actually does, catch it when it goes wrong, and keep improving it. In regulated industries you also have to prove all of that. In this hands-on workshop, Annabell Schäfer and Doneyli De Jesus from Langfuse show how to close that loop, using a banking support agent, Northwind Bank, as the example.
>
> You'll see how to:
>
> • Trace every agent interaction end to end
>
> • Use guardrails to catch and block risky requests, and record each block
>
> • Evaluate answers for quality and compliance using datasets as part of your CI/CD
>
> • Spot your agent's top failure modes on a dashboard and estimate the business impact
>
> The session suits both AI leaders and engineers. Bring a laptop if you want to follow along: a seed script loads the demo into your own Langfuse project.

</details>

### Unhobbling the Frontier

**Speaker:** Lance Martin (Member of Technical Staff, Anthropic)  
**When / where:** 3:30pm–4:30pm · Shannon Room (NY Ballroom E) · Workshop (1-2 hrs)  
**Links:** **[📰 Full write-up](sessions/10-12-1530-unhobbling-the-frontier.md)** · [Schedule](https://ai.engineer/nyc/2026/schedule?session=6a4e7cfd-334a-4d3b-94ed-282acc905448) · [Speaker’s blog: Automating eval design and hillclimbing with Claude](https://claude.dev/blog/automating-eval-design-and-hillclimbing/) · [Precise notes (中文)](sessions/precise/10-12-1530-unhobbling-the-frontier.md)  
**Recording:** not published yet

**What it covers.** Lance Martin’s thesis: many applications sit inside the cost–performance frontier (not on it) because of small misconfigurations. Five themes, each with a command in the `claude-api` skill for Claude Code: audit your prompts, use the prompt cache, design a good eval, hillclimb against it, and design the harness for tasks that run for hours to days. The slides took about 23 minutes and the rest of the hour was Q&A.

<details><summary>Official talk description</summary>

> In this workshop, we'll share several simple ways to get the most out of fronter models, including harness design, prompting, and settin g up evaluations for successful hillclimbing.

</details>

**My takeaways**

- **Prompt audit.** Tricks written for older models (verification rituals, thoroughness boosters) only cost tokens on frontier models, and contradictory rules now hurt: a frontier model takes both seriously and gets confused. Upgrading a model is a good moment to audit the prompt, not just change the model ID.
- **Prompt cache.** The cache is a strict prefix match, so even a one-byte change invalidates it. Order the prompt from least to most frequently changing. `/claude-api cost-optimize` found cost reductions of up to about 72% on internal benchmarks with no meaningful change in performance.
- **Good eval design** (matches the blog): tasks mirror production; a stronger model or more effort scores higher; there is headroom below 100% that is not explained by ambiguous or impossible tasks; run-to-run variance is low. `/claude-api build-eval` walks through the data, the eval and the grader with you, and is not opinionated about where your data lives. Read a sample of scored transcripts before trusting a grader.
- **Hillclimbing.** The main risk is overfitting: benchmark quirks leak into the harness (e.g. an OCR tool added because the eval needs it). `/claude-api hillclimb` splits the eval into train and test, shows the analyzer only train failures, makes one change per round, and reverts if train improves but test stays flat. It is not guaranteed to converge; when it stalls, the cause is often ambiguous cases or a bad grader.
- Start hillclimbing on text surfaces (prompts, skills): cheap to change and tightly coupled to the output. Harness code changes are large and harder to attribute. Prompts are also the easiest thing to overfit.
- **Cost as the objective.** You can hillclimb for lower cost at parity performance even on a saturated eval. A stronger model at low effort is often cheaper and better than a weaker model at high effort; without an eval, try Opus 5.5 at low effort first (effort is not a hard token cap). Blog example (44 support tickets, 14 held out): Opus 4.8 at high effort scored 74.4% at 4.6¢ per ticket; Sonnet 5 at low effort with an improved prompt scored 98.9% at 1¢; on the held-out tickets 90.5% vs 78.6% at about one fifth of the cost.
- **Harness for long-running work.** Tasks now run for hours to days, so Claude Managed Agents separate the brain (harness) from the hands (sandbox): the session log is its own append-only object and can be recovered, and secrets live in a vault Claude never touches directly.
- **Q&A.** For eval criteria without a clear answer (e.g. UI), write a rubric a human could score with, use binary checks instead of 1–10 scales, have several models grade (disagreement is a bad sign), and run replicates. Pairwise comparison works well for ambiguous cases. Let Claude decide when a repeated workflow should become a skill. Avoid adversarial sampling (only cases one model gets wrong). Human taste and direction stay with people; Claude can scan traces for suspicious cases like an offline online-evaluator. Overfitting, not eval awareness, is the bigger concern when hillclimbing.
- Where the sources differ: the talk gave the `claude-api` skill’s starting score as 68% and the cost saving as about 90%; the blog and slides say 66.1% and about one fifth of the cost, which are the figures used above.

<details><summary>Slide photos (15)</summary>

<a href="slides/10-12-1530-unhobbling-the-frontier/01.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/02.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/03.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/04.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/05.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/06.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/07.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/08.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/09.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/10.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/11.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/12.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/13.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/14.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-12-1530-unhobbling-the-frontier/15.jpg"><img src="slides/10-12-1530-unhobbling-the-frontier/15.jpg" width="260" alt="slide"></a>

</details>

## Tuesday, October 13

### Vault: Making Unstructured Data Work for Investing

**Speakers:** Aaron Linsky (Head of Product Engineering, Bridgewater); Sam Green (Team Lead, Applied AI - AIA Labs, Bridgewater)  
**When / where:** 9:00am–9:18am · Simons Room (Metropolitan East) · Featured Keynote  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=a84ca3a8-cc2f-4b50-a635-fc380f6f04f6) · [▶ Video](https://www.youtube.com/watch?v=CLve4Nx_RwI) · [▶ Livestream @ 7:22](https://www.youtube.com/watch?v=akEqiXFo3GM&t=442s)

**What it covers.** Bridgewater built the Vault: 100M+ documents from 135 external sources plus internal memos and meetings, cleaned, permissioned and tagged to the firm’s ontology. Three lessons: humans and agents need different things from search, the biggest wins come from one tool that serves both, and the real prize is agents that proactively surface what deserves an investor’s attention. A hand-built benchmark took precision@10 from about 60% to over 90%.

<details><summary>Official talk description</summary>

> Model intelligence and persistence have reached a level where we believe applied AI in the enterprise is now about context and connectors. Out of the box frontier intelligence in the investment domain is fundamentally limited by access to information. Bridgewater's investment philosophy is fundamental and systematic. We work to uncover truths about how economies operate, building up that understanding on one of the world's most comprehensive bitemporal structured data stores. But our investors don't work from structured data alone. They consume research, news, transcripts, and internal writing every day. An agent tasked with investment problems needs access to the same information our best human investors have, so we built the Vault: a centralized, high-quality repository of our best external and internal unstructured data, serving humans and agents alike. We'll walk through why we chose to solve this problem ourselves, tracing the Vault from our first instinct, which was to buy it, through expanding it across all of our third-party research and into our internal knowledge, building it up from a point solution into a platform. The Vault already powers PAT, our first generation of analyst agents, used daily by hundreds of Bridgewater investors, and is now enabling our second-generation Researcher.

</details>

**My takeaways**

- Directly relevant to agent-driven investing and research workflows.

<details><summary>Slide photos (26)</summary>

<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/01.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/02.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/03.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/04.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/05.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/06.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/07.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/08.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/09.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/10.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/11.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/12.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/13.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/14.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/15.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/16.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/17.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/18.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/19.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/19.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/20.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/20.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/21.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/21.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/22.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/22.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/23.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/23.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/24.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/24.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/25.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/25.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0900-vault-making-unstructured-data-work-for/26.jpg"><img src="slides/10-13-0900-vault-making-unstructured-data-work-for/26.jpg" width="260" alt="slide"></a>

</details>

### Anthropic: Accelerating frontier AI in Financial Services.

**Speakers:** Belinda Neal (Managing Director, Financial Services, Anthropic); Keon Shahab (Applied AI Architect, Anthropic)  
**When / where:** 9:18am–9:36am · Simons Room (Metropolitan East) · Featured Keynote  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=68a1e78b-569e-4314-a56f-f20508123cbe)  
**Recording:** not chaptered in the livestream, and no standalone video found

**What it covers.** Anthropic’s financial-services team frames two frontiers every firm has to reach: the frontier of intelligence (what the best model can do today) and the frontier of adoption (how much of the firm’s real work runs on it). Scaling from a handful of agents to many means building connectors, skills and controls once.

<details><summary>Official talk description</summary>

> Frontier AI is moving faster than financial services is adopting it, and that gap is the opportunity. In this keynote, Belinda Neal and Keon Shahab of Anthropic set out the two frontiers every firm now has to reach: the frontier of intelligence, what the best model can do today, and the frontier of adoption, how much of the firm's real work runs on it. They show how to go from a handful of agents to many by building connectors, skills and controls once, and what it looks like when a whole industry does the same, with Claude for Financial Advisors as a worked example. You will leave with three questions to take back to your own firm.

</details>

<details><summary>Slide photos (17)</summary>

<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/01.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/02.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/03.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/04.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/05.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/06.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/07.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/08.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/09.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/10.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/11.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/12.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/13.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/14.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/15.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/16.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/17.jpg"><img src="slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/17.jpg" width="260" alt="slide"></a>

</details>

### Balancing Acceleration and Safety for AI Agents

**Speaker:** John Feminella (SVP Architecture, Two Sigma)  
**When / where:** 9:36am–9:54am · Simons Room (Metropolitan East) · Featured Keynote  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=73badd10-6f8c-4c95-b5e7-55fc3d7bafc1) · [▶ Video](https://www.youtube.com/watch?v=iniryju23Sk) · [▶ Livestream @ 23:55](https://www.youtube.com/watch?v=akEqiXFo3GM&t=1435s) · [Speaker write-up (DEV)](https://dev.to/dailycontext/brakes-let-you-go-fast-balancing-acceleration-and-safety-for-ai-agents-49p9)

**What it covers.** Uses the 2020 Citibank/Revlon $900M payment error to show how to give agents real autonomy without locking everything down. Five composable controls: an agent registry, risk-based review, tool access that follows an agent’s declared purpose, shared firm-wide evals, and classifiers that send humans only the approvals that matter. (The video was retitled “What a $900M Bank Error Teaches Us About AI Agents”.)

<details><summary>Official talk description</summary>

> As AI agents grow in utility and power, they also grow in potential risk. For some, this creates a balancing act: too little safety means highly regulated firms are exposed to undue risk; too little innovation means agents cost more to run than they earn.  How do we balance the desire for safe agentic AI usage with the goal of using agents to carry out innovative research and engineering? In this talk, I’ll cover some of the key technical approaches we use and have found highly effective at Two Sigma, and along the way hopefully convince you that there doesn’t need to be a tradeoff between these two properties.

</details>

**My takeaways**

- Agent registry plus scoring, with layered review depending on risk.
- Approval fatigue makes per-action human approval unworkable; a classifier that decides which approvals reach a human is the answer (comparable to Claude’s auto mode).
- Open questions: whether to scope permissions with allow/deny lists or by declared purpose, whether agents declaring their own permissions makes humans the bottleneck, and why evals should be shared firm-wide rather than bespoke.
- Framing: cars have brakes so they can go fast.

<details><summary>Slide photos (24)</summary>

<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/01.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/02.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/03.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/04.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/05.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/06.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/07.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/08.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/09.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/10.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/11.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/12.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/13.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/14.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/15.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/16.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/17.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/18.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/19.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/19.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/20.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/20.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/21.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/21.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/22.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/22.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/23.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/23.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/24.jpg"><img src="slides/10-13-0936-balancing-acceleration-and-safety-for-ai/24.jpg" width="260" alt="slide"></a>

</details>

### Does Code Quality Even Matter?

**Speaker:** Max Kanat-Alexander (Principal Software Engineer, Capital One)  
**When / where:** 9:54am–10:12am · Simons Room (Metropolitan East) · Featured Keynote  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=5191589c-a70e-4806-99dc-86f7794f46b9) · [▶ Video](https://www.youtube.com/watch?v=bU5gDVyu_28) · [▶ Livestream @ 43:51](https://www.youtube.com/watch?v=akEqiXFo3GM&t=2631s)

**What it covers.** Code quality exists for two things: understanding a system and safely changing it. AI makes a new kind of software common, disposable software with low consequences, few dependents and near-zero replacement cost, where quality does not matter. For durable software it matters more than ever. (The video was retitled “Does Code Quality Still Matter? It Depends on This”.)

<details><summary>Official talk description</summary>

> There's so much debate online about "should we read the code?" Some people are yelling, "Everybody should build everything with a dark factory!" Others cry out, "The slop will destroy us all!" What this whole conversation is missing is principles for understanding how we would even make these decisions. Come hear the author of Code Simplicity say what he thinks is true now with AI coding, and see if there's some way to reliably find a path through all these opposing viewpoints.

</details>

**My takeaways**

- Quality risk compounds: poor structure makes the system harder to understand, which makes future problems more likely.
- Durable software lives for years, is depended on by many, has major failure consequences and needs continuous maintenance. Disposable software is the opposite.
- Make software more disposable through isolation, independence, redundancy and limited scope.
- Make rigor cheap for durable software: standardization, tooling inside the workflow, easier testing (test infrastructure such as in-memory fakes), and maximum strictness on lint rules, with false positives mattering less.
- It is not black and white, and durable software engineering is not going away.

<details><summary>Slide photos (16)</summary>

<a href="slides/10-13-0954-does-code-quality-even-matter/01.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/02.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/03.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/04.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/05.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/06.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/07.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/08.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/09.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/10.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/11.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/12.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/13.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/14.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/15.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-0954-does-code-quality-even-matter/16.jpg"><img src="slides/10-13-0954-does-code-quality-even-matter/16.jpg" width="260" alt="slide"></a>

</details>

### What does an AI engineer do?

**Speaker:** Aparna Dhinakaran (Co-Founder & Chief Product Officer, Arize AI)  
**When / where:** 10:12am–10:30am · Simons Room (Metropolitan East) · Featured Keynote  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=b2ed4456-5671-4e8e-83fd-684c012fef8c) · [▶ Video](https://www.youtube.com/watch?v=ImnZ7s68jw8) · [▶ Livestream @ 1:00:43](https://www.youtube.com/watch?v=akEqiXFo3GM&t=3643s)

**What it covers.** As the field moves from writing features to running factories of agents, the job becomes building the loop: evals that review non-deterministic work, telemetry that agents consume, and agents that act on it. Walks through Arize’s Signal, an always-on agent that reads traces, remembers past issues and opens PRs. (The video was retitled “What Does an AI Engineer Do Now?”.)

<details><summary>Official talk description</summary>

> Coding agents have effectively eliminated the need for manually writing code, and the current frontier is how to eliminate code review. But where are we going if we look past that? What happens when agents master devops, architecture, scaling, and security? What is an engineer if agents can do all the building? In this look into the next few years, Aparna Dhinakaran argues that the core skill that remains is the definition of “good”: knowing what your users want, and what it looks like when it’s “done”, are the two things agents don’t know and aren’t going to get to. At the same time, more and more AI is going to be involved in software, so software is going to get steadily less deterministic. That presents a dual challenge: you have to get good at defining success, while simultaneously success gets harder to measure. How to tackle those challenges is the subject of this talk.

</details>

<details><summary>Slide photos (12)</summary>

<a href="slides/10-13-1012-what-does-an-ai-engineer-do/01.jpg"><img src="slides/10-13-1012-what-does-an-ai-engineer-do/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1012-what-does-an-ai-engineer-do/02.jpg"><img src="slides/10-13-1012-what-does-an-ai-engineer-do/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1012-what-does-an-ai-engineer-do/03.jpg"><img src="slides/10-13-1012-what-does-an-ai-engineer-do/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1012-what-does-an-ai-engineer-do/04.jpg"><img src="slides/10-13-1012-what-does-an-ai-engineer-do/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1012-what-does-an-ai-engineer-do/05.jpg"><img src="slides/10-13-1012-what-does-an-ai-engineer-do/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1012-what-does-an-ai-engineer-do/06.jpg"><img src="slides/10-13-1012-what-does-an-ai-engineer-do/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1012-what-does-an-ai-engineer-do/07.jpg"><img src="slides/10-13-1012-what-does-an-ai-engineer-do/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1012-what-does-an-ai-engineer-do/08.jpg"><img src="slides/10-13-1012-what-does-an-ai-engineer-do/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1012-what-does-an-ai-engineer-do/09.jpg"><img src="slides/10-13-1012-what-does-an-ai-engineer-do/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1012-what-does-an-ai-engineer-do/10.jpg"><img src="slides/10-13-1012-what-does-an-ai-engineer-do/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1012-what-does-an-ai-engineer-do/11.jpg"><img src="slides/10-13-1012-what-does-an-ai-engineer-do/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1012-what-does-an-ai-engineer-do/12.jpg"><img src="slides/10-13-1012-what-does-an-ai-engineer-do/12.jpg" width="260" alt="slide"></a>

</details>

### Can Any Model Write Like Me? Building a Benchmark for Creative Writing

**Speaker:** Louis-François Bouchard (CTO & Co-Founder, Towards AI)  
**When / where:** 10:40am–10:58am · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** **[📰 Full write-up](sessions/10-13-1040-can-any-model-write-like-me-building-a-b.md)** · [Schedule](https://ai.engineer/nyc/2026/schedule?session=736edd83-bb08-49f1-90ed-722b7ed07eb2) · [▶ Video](https://www.youtube.com/watch?v=_dTF3NpPzGo) · [Speaker article (DEV)](https://dev.to/dailycontext/what-9050-ai-written-scripts-taught-me-about-slop-1g5n) · [Precise notes (中文)](sessions/precise/10-13-1040-can-any-model-write-like-me-building-a-b.md)

**What it covers.** Which model writes a YouTube script that sounds like the author? Breaks down what makes AI writing “slop” and shows how a personal benchmark was built: 10 scripts, 5 drafts per model, 4 LLM judges, nine scoring metrics and 1,000 human A/B checks. (The video was retitled “Why AI Writing Still Reads Like Slop”.)

<details><summary>Official talk description</summary>

> Four months ago, out of frustration with slop, I set out to answer one question: which model writes a YouTube script that sounds like me. Not best in general, not best at writing. Which one passes as my voice, on my channel, by my standards, and isn't cringe to read out loud.
>
> So I vibe coded my own benchmark to answer it.
>
> Benchmarking 148 models took about two weeks, two 20x subscriptions on Claude and Codex, and $80 in API credits. That was never the bottleneck.
>
> The four months went into learning to build a proper benchmark. Deciding what a score means when the thing you are measuring is taste. Writing rubrics that quantify my subjectivity. Working out whether Elo was even the right tool. Finding out the same drafts average 74 out of 100 with one judge and 88 with another. Writing 10 scripts with two human editors and withholding half, so I could tell whether a model is contaminated.
>
> Most of the benchmark uses automated judging, but human judging is crucial for analyzing taste.
>
> So I added human ratings, sat down, and rated the first few hundred pairwise comparisons myself. My evaluation was broken, and only doing the work by hand revealed it.
>
> I fixed it, rated again, and I am releasing the benchmark publicly to get help rating these models.
>
> I will show the architecture, the numbers, what it cost, and the failure modes I would look for first in yours.

</details>

**My takeaways**

- Slop is mostly overused words and being too general.
- Five drafts per model, because of outliers; judges prefer their own model family.
- Iterating is expensive: per the slide, re-judging the whole board after one rubric change cost $2,350, and regenerating and re-judging it cost $3,750.
- Writing with AI: don’t one-shot, don’t use max effort for writing, give very clear directions with long detailed skills and examples, and build your own evaluation.

<details><summary>Slide photos (31)</summary>

<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/01.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/02.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/03.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/04.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/05.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/06.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/07.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/08.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/09.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/10.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/11.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/12.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/13.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/14.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/15.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/16.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/17.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/18.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/19.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/19.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/20.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/20.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/21.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/21.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/22.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/22.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/23.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/23.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/24.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/24.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/25.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/25.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/26.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/26.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/27.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/27.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/28.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/28.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/29.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/29.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/30.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/30.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1040-can-any-model-write-like-me-building-a-b/31.jpg"><img src="slides/10-13-1040-can-any-model-write-like-me-building-a-b/31.jpg" width="260" alt="slide"></a>

</details>

### Write the Answer First: Generating Eval Datasets for Agents With No Benchmark

**Speaker:** Nathanael Weill (Director, Data Science & AI, Air Canada)  
**When / where:** 11:05am–11:23am · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** **[📰 Full write-up](sessions/10-13-1105-write-the-answer-first-generating-eval-d.md)** · [Schedule](https://ai.engineer/nyc/2026/schedule?session=c4ba0138-6f6d-4780-93a1-4d8f0c478339) · [▶ Video](https://www.youtube.com/watch?v=c_SPW__jF9w) · [Precise notes (中文)](sessions/precise/10-13-1105-write-the-answer-first-generating-eval-d.md)

**What it covers.** Air Canada has agents with no public benchmark and domain experts with no time. The fix is to write the answer first: start from a known answer, generate the input from it, and check whether the agent finds its way back. A past RAG project went from one expert-graded test a week to dozens of automated tests a day; the same idea is applied to a supplier-contract review agent. (The video was retitled “How to Test AI Agents When No Benchmark Exists”.)

<details><summary>Official talk description</summary>

> Not long ago, nobody knew what a good evaluation strategy for a RAG system looked like. Each team invented its own, and it took months. Then RAGAS arrived. Faithfulness, context relevance, and answer relevance became a playbook you could look up, and the same evaluation now takes a week.
>
> What's the equivalent for a custom agent, like one that reviews contracts, triages invoices, or runs your internal workflows? Nothing. There's no benchmark, no labels, and no playbook, so every team is back to inventing its own and losing the same months.
>
> This talk shares the techniques I used to generate eval datasets for an agent no benchmark covers, along with how to tell when a generated dataset will help you and when it will quietly mislead you. It won't be RAGAS for agents, but it will be enough to get started on Monday.

</details>

**My takeaways**

- On the 2024 RAG project, each added component (better parsing, chunker, re-ranker, …) left quality flat, and every change waited a week for expert review. Writing the answer first made testing automatable; on the 2026 contract project the feedback loop went from 4 weeks to 5 minutes.
- No public benchmark exists for supplier contract review, so the test set has to be built in-house, paired with a longer outer loop.

<details><summary>Slide photos (26)</summary>

<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/01.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/02.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/03.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/04.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/05.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/06.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/07.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/08.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/09.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/10.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/11.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/12.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/13.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/14.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/15.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/16.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/17.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/18.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/19.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/19.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/20.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/20.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/21.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/21.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/22.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/22.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/23.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/23.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/24.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/24.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/25.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/25.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1105-write-the-answer-first-generating-eval-d/26.jpg"><img src="slides/10-13-1105-write-the-answer-first-generating-eval-d/26.jpg" width="260" alt="slide"></a>

</details>

### Your Financial Agent Passed Every Eval. Can You Defend Its Decision?

**Speaker:** Anirudh Veeraragavan (Lead Security Engineer, Fieldguide)  
**When / where:** 11:30am–11:48am · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** **[📰 Full write-up](sessions/10-13-1130-your-financial-agent-passed-every-eval-c.md)** · [Schedule](https://ai.engineer/nyc/2026/schedule?session=30afe596-abe2-417c-bd5a-313fa4bc54b3) · [Precise notes (中文)](sessions/precise/10-13-1130-your-financial-agent-passed-every-eval-c.md)  
**Recording:** not published yet

**What it covers.** A financial agent can pass every eval and still make a decision nobody can defend. Defensibility needs four controls: authorization, execution control, evidence provenance and evaluation integrity, worked through on a synthetic audit decision trace.

<details><summary>Official talk description</summary>

> A financial agent can pass every eval and still produce a decision no one can defend. The citation may point to the wrong document. The reasoning may omit the key business rule. The benchmark may treat last year's policy as today's ground truth. A high aggregate score can hide the failure that matters most.
>
> Defensibility requires four distinct controls: authorization, execution control, evidence provenance, and evaluation integrity. Using a synthetic audit decision trace, I'll work through each control: limiting what an agent can do, preserving what it did so contested steps can be reproduced, binding its material claims to the correct sources, and verifying whether its benchmark can be trusted. I'll also show where the agent should stop and a human should decide.
>
> You'll leave with a practical assurance pattern for financial agents whose decisions may be challenged months or years later. When that challenge comes, "the model said so" is not a defensible answer.

</details>

<details><summary>Slide photos (19)</summary>

<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/01.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/02.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/03.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/04.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/05.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/06.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/07.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/08.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/09.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/10.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/11.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/12.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/13.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/14.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/15.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/16.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/17.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/18.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1130-your-financial-agent-passed-every-eval-c/19.jpg"><img src="slides/10-13-1130-your-financial-agent-passed-every-eval-c/19.jpg" width="260" alt="slide"></a>

</details>

### Lies, Damned Lies, and Evals: How to trust your agents when the stakes are high

**Speaker:** Stephanie Jarmak (AI Engineer, Omni Analytics)  
**When / where:** 11:55am–12:13pm · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** **[📰 Full write-up](sessions/10-13-1155-lies-damned-lies-and-evals-how-to-trust.md)** · [Schedule](https://ai.engineer/nyc/2026/schedule?session=b1fe8396-6fa8-4574-9775-0dc01f56ce57) · [▶ Video](https://www.youtube.com/watch?v=A3FL_zTQKU0) · [Precise notes (中文)](sessions/precise/10-13-1155-lies-damned-lies-and-evals-how-to-trust.md)

**What it covers.** Most agent evaluations are easy to misread. Swapping only the harness moved results by 30%; public benchmarks are for shortlisting, not deciding. Design your own evals like an experiment: realistic tasks, repeated runs, full traces and a measurement that matches what you care about. (The video was retitled “Don’t Trust Your AI Agents. Test Them Like a Scientist”.)

<details><summary>Official talk description</summary>

> Your coding agent scores 74%. The release decision is due today. Nobody can say what that number has earned the right to decide.
>
> That ambiguity matters everywhere, but especially in regulated industries. Agents can modify transaction systems, access controls, reporting pipelines, and compliance-sensitive infrastructure. When software can affect money, customers, or regulatory obligations, “the benchmark looked good” is not a defensible release argument. Trust has to be supported by evidence that can survive challenge.
>
> An eval score compresses a model, task set, execution environment, evaluator, grader, and one sample from a variable process into a single number. Financial institutions have spent decades developing disciplines for decisions built on imperfect models: define the intended use, validate performance, expose limitations, independently challenge assumptions, and monitor what happens in production. In AI, we often just print the result to one decimal place and put it on a leaderboard.
>
> In this talk, I’ll put one convincing score on trial. The audience will choose between two release candidates, then reconsider that decision as I reveal what the aggregate concealed: a +5.4-point win in my own repository-level eval that shrank to +0.35 after repeated runs; a widely cited coding benchmark whose sponsor stopped reporting it after uncovering flawed tasks and contamination; and model graders that agreed with one another while missing the defects the release gate was supposed to catch.
>
> We’ll rebuild the decision around five checks:
>
> - What decision is this score allowed to change?
>
> - How large is the run-to-run variance?
>
> - Does the task distribution match the real workload?
>
> - Which plausible failures can the evaluator actually detect?
>
> - What are the grader’s true-positive and false-positive rates at the chosen threshold?
>
> You’ll leave with a practical evidence standard for deciding when an agent has earned greater autonomy, and when a precise score is disguising uncertainty that still belongs with a human decision-maker.

</details>

**My takeaways**

- Public benchmark vs internal evaluation: a law-firm agent is not served by a terminal-coding benchmark.
- Set up comparison experiments across model, task and harness. In the cited Terminal-Bench subset, Claude Opus 5 solved 36/63 tasks under OpenHands but 19/63 under the minimal Pi harness, while GPT-6 Astra went the other way (31 vs 38): the harness alone can flip a model ranking.
- Human review load is a real bottleneck.

<details><summary>Slide photos (12)</summary>

<a href="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/01.jpg"><img src="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/02.jpg"><img src="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/03.jpg"><img src="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/04.jpg"><img src="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/05.jpg"><img src="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/06.jpg"><img src="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/07.jpg"><img src="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/08.jpg"><img src="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/09.jpg"><img src="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/10.jpg"><img src="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/11.jpg"><img src="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/12.jpg"><img src="slides/10-13-1155-lies-damned-lies-and-evals-how-to-trust/12.jpg" width="260" alt="slide"></a>

</details>

### 1,368 AI Failures: What They Teach Us About System Evals

**Speakers:** Vishal Srivastava (Corporate VP, Lead Data Scientist, New York Life Insurance); Tanmay Sah (AI Researcher)  
**When / where:** 12:20pm–12:38pm · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** **[📰 Full write-up](sessions/10-13-1220-1368-ai-failures-what-they-teach-us-abou.md)** · [Schedule](https://ai.engineer/nyc/2026/schedule?session=9d72d7ac-a2a2-42cf-b880-5859316d840c) · [▶ Video](https://www.youtube.com/watch?v=mMw3arwQsL0) · [Speaker article (DEV)](https://dev.to/dailycontext/what-1368-real-world-ai-incidents-teach-us-about-evaluation-71p) · [Precise notes (中文)](sessions/precise/10-13-1220-1368-ai-failures-what-they-teach-us-abou.md)

**What it covers.** Analysis of 1,368 documented AI incidents across three areas (technical strength, deployment controls, business impact) and 13 sub-areas covering about 98% of incidents. Weak deployment controls such as human review and monitoring are most closely tied to harm. Includes a build-break-fix loop that tries to prove a system wrong rather than right. (The video was retitled “1,368 AI Failures and What They Teach About Evals”.)

<details><summary>Official talk description</summary>

> We analyzed 1,368 documented AI failures across healthcare, financial services, law enforcement, professional services, and consumer technology.
>
> The clearest lesson was not simply that AI models fail.
>
> AI risk lives in the system, not just the model.
>
> In our analysis, 92% of incidents implicated multiple weaknesses simultaneously. The typical documented failure crossed several parts of the deployed system: technical behavior, data, workflows, human oversight, controls, exposure, and downstream consequence.
>
> That creates a fundamental problem for how we evaluate increasingly agentic AI systems.
>
> Most evals still ask some version of one question:
>
> Did the model or agent complete the task correctly?
>
> But task success is not system success.
>
> An agent can reach the expected answer after retrieving unsupported evidence, selecting the wrong tool, passing invalid arguments, crossing an authority boundary, or taking an action that should never have been available to it.
>
> The output can say “pass” while the system tells a different story.
>
> Drawing on the research behind our forthcoming book, Evaluable AI: From Model Evals to System Evals, this talk introduces a different unit of evaluation: the deployed AI system.
>
> We use Risk-Based Evaluability Design (R-BED) to organize system evaluation around three connected questions:
>
> Technical Failure: Where can the system fail?
>
> Deployment Harm: Under what conditions can that failure reach people, processes, or consequential decisions?
>
> Business Impact: If it does, how broadly and severely can the consequence propagate, and what can be recovered?
>
> These questions cannot be collapsed into a single score or task-success metric. A technically weak system may remain contained. A seemingly small technical error may become material because authority, exposure, or controls allow it to propagate. And a system that appears to perform well may still be difficult to trust if its behavior cannot be reconstructed and challenged.
>
> This leads to the concept at the center of the talk: evaluability.
>
> Evaluability is the ability to determine, with evidence, whether a deployed AI system remains within acceptable bounds and continues to serve its intended purpose.
>
> For agentic systems, that means making the trajectory observable: the evidence retrieved, sources used, tool selected, arguments passed, state transitions made, authority exercised, human interventions triggered, and actions taken.
>
> Tanmay Sah and I will demonstrate this through a reproducible evaluation pipeline applied to benchmark agent trajectories. We will show where task-level success can conceal failures elsewhere in the trajectory, what can be evaluated directly from system evidence, where an evaluator or judge is required, and when the correct evaluation result is not pass or fail but insufficient evidence.
>
> We then turn those findings into an engineering discipline:
>
> BUILD: Define the system claim, operating boundary, evidence requirements, and permitted authority.
>
> BREAK: Stress the system, discover failure regions, and trace failures across evidence, execution, tools, workflows, and controls.
>
> FIX: Constrain, redesign, route, monitor, or intervene, then verify that the change actually worked.
>
> The key insight is that meaningful system evals cannot simply be bolted onto an AI application after it has been built.
>
> If the system does not expose the evidence needed to reconstruct what happened, important questions become impossible to answer.
>
> Evaluability has to be designed into the system.
>
> As AI systems become more autonomous and gain access to tools, workflows, data, and consequential actions, the distance between a technical error and a real-world consequence is shrinking.
>
> The next generation of evals therefore cannot stop at asking:
>
> “Did the model get the answer right?”
>
> They must also ask:
>
> Where can the system fail? Can that failure reach the real world? What happens if it does, and did we build enough evidence into the system to know?

</details>

**My takeaways**

- One framework with 13 dimensions, used as a rubric.

<details><summary>Slide photos (13)</summary>

<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/01.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/02.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/03.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/04.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/05.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/06.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/07.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/08.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/09.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/10.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/11.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/12.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/13.jpg"><img src="slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/13.jpg" width="260" alt="slide"></a>

</details>

### Proving Alpha: Showcasing AI's Value in Fundamental Investing

**Speaker:** Hari Kumar (Head of AI and Data Science, Contour Asset Management)  
**When / where:** 1:40pm–1:58pm · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** **[📰 Full write-up](sessions/10-13-1340-proving-alpha-showcasing-ais-value-in-fu.md)** · [Schedule](https://ai.engineer/nyc/2026/schedule?session=ae273f0b-db0f-4638-8ae3-05874bebf9ca) · [▶ Video](https://www.youtube.com/watch?v=ZMZlAn6Pyfg) · [Precise notes (中文)](sessions/precise/10-13-1340-proving-alpha-showcasing-ais-value-in-fu.md)

**What it covers.** Contour Asset Management, a $6B fundamental long/short fund, tackles what AI spend actually returns. It runs live A/B tests of analysts’ research with and without AI tools, tracks “cost per useful insight”, resurfaces old ideas when the market makes them interesting again, and extends the approach to trading, operations and compliance. (The video was retitled “Proving Alpha: How a Hedge Fund Measures AI’s ROI”.)

<details><summary>Official talk description</summary>

> Every investment firm should have AI in their budget. However, far fewer can say what it returned, especially when it comes to a traditional, fundamental equity investing strategy. We are going to showcase the methods we've built to prove how agent workflows measurably contribute to performance when returns are made up of so many different factors.
>
> In a long/short hedge fund, the AI workflows we've developed sit directly in the investment process, rather than alongside it. Collaborating with our investors & researchers to develop tooling together has led to high-value products that get used daily, including those for new idea generation, overnight news synthesis, and earnings preview. These workflows greatly improve the research process, and we are now able to trace their impact back to the book.
>
> We'll discuss how every idea that a workflow produces is logged at the moment of generation, along with its source and reasoning, so it can be scored for utility. From there, we have built systems to track how many ideas reach our PM to become positions, and how those positions perform against given benchmarks. For softer AI-native signals, we grade against the traditional way that same metric would have been produced and measure both time & signal improvements. This has made great strides to solve one of the biggest challenges in AI implementation: understanding the ROI and alpha generated by every tool and decision made via these systems.
>
> We'll walk through this measurement and attribution process, both from a technical lens as well as a product framework. Ranging from the successes to pitfalls, this talk will have you energized and prepared to start measuring the impact of AI in your investment decisions.

</details>

**My takeaways**

- The challenge: returns have many factors, so ROI is hard to see. The question to ask is “what did you consider that you couldn’t have before?”: novel ideas, differentiated sources, and ideas acted upon.
- Measure coverage and time per name, before vs after AI; run live A/B comparisons (e.g. a name in three months that is in no AI workflow).
- Agents can track notes; expert calls and credit-card panel data are example inputs.

<details><summary>Slide photos (8)</summary>

<a href="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/01.jpg"><img src="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/02.jpg"><img src="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/03.jpg"><img src="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/04.jpg"><img src="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/05.jpg"><img src="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/06.jpg"><img src="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/07.jpg"><img src="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/08.jpg"><img src="slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/08.jpg" width="260" alt="slide"></a>

</details>

### Can AI Understand a Distressed Credit?

**Speaker:** Harneet Kaur (Founder, Axim Intelligence)  
**When / where:** 2:05pm–2:23pm · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** **[📰 Full write-up](sessions/10-13-1405-can-ai-understand-a-distressed-credit.md)** · [Schedule](https://ai.engineer/nyc/2026/schedule?session=adca6e26-1ddc-4f77-8e35-59bf1d1de81d) · [▶ Video](https://www.youtube.com/watch?v=WOszaC1WWbQ) · [Precise notes (中文)](sessions/precise/10-13-1405-can-ai-understand-a-distressed-credit.md)

**What it covers.** Tests whether frontier models can reason about a live distressed situation (iHeartMedia). Two top models disagreed sharply on bankruptcy vs a negotiated restructuring; breaking the analysis into checked layers (extraction, derivation, judgment, prediction) brought them to the same answer, which raises the question of whether agreement is the same as being right.

<details><summary>Official talk description</summary>

> Distressed credit and restructuring is one of the most specialized areas of finance. The data is messy, definitions are unclear, and even extracting a single data point can require judgment. In bankruptcy, simple facts (i.e. cash on balance sheet, debt outstanding) can be disputed and depend on context or legal interpretation.
>
> This talk explores what it takes to build AI for a complex, regulated, and judgment-heavy domain. I’ll share my start up (Axim's) approach to "analytical provenance" and "DistressedBench," a new eval framework designed specifically around the reasoning tasks required in distressed investing.

</details>

**My takeaways**

- There is no ground truth for predicting stress or distress, so verification is the open problem.

<details><summary>Slide photos (8)</summary>

<a href="slides/10-13-1405-can-ai-understand-a-distressed-credit/01.jpg"><img src="slides/10-13-1405-can-ai-understand-a-distressed-credit/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1405-can-ai-understand-a-distressed-credit/02.jpg"><img src="slides/10-13-1405-can-ai-understand-a-distressed-credit/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1405-can-ai-understand-a-distressed-credit/03.jpg"><img src="slides/10-13-1405-can-ai-understand-a-distressed-credit/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1405-can-ai-understand-a-distressed-credit/04.jpg"><img src="slides/10-13-1405-can-ai-understand-a-distressed-credit/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1405-can-ai-understand-a-distressed-credit/05.jpg"><img src="slides/10-13-1405-can-ai-understand-a-distressed-credit/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1405-can-ai-understand-a-distressed-credit/06.jpg"><img src="slides/10-13-1405-can-ai-understand-a-distressed-credit/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1405-can-ai-understand-a-distressed-credit/07.jpg"><img src="slides/10-13-1405-can-ai-understand-a-distressed-credit/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1405-can-ai-understand-a-distressed-credit/08.jpg"><img src="slides/10-13-1405-can-ai-understand-a-distressed-credit/08.jpg" width="260" alt="slide"></a>

</details>

### Why Frontier Models Fail on Billion Dollar Portfolios

**Speakers:** Chanyeol Choi (Co-founder & CEO, LinqAlpha); Janie Gu (Group Product Manager (Kaggle Benchmarks), Google DeepMind); Jack Haverty (Head of Product, LinqAlpha); Suyeol Yun (Head of AI, LinqAlpha)  
**When / where:** 2:30pm–2:48pm · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** **[📰 Full write-up](sessions/10-13-1430-why-frontier-models-fail-on-billion-doll.md)** · [Schedule](https://ai.engineer/nyc/2026/schedule?session=e8e53365-3c1f-4c28-bdd0-44c305a05d57) · [▶ Video](https://www.youtube.com/watch?v=aVbRSIQO2dE) · [Precise notes (中文)](sessions/precise/10-13-1430-why-frontier-models-fail-on-billion-doll.md)

**What it covers.** Janie Gu (Google DeepMind, Kaggle Benchmarks) explains why most AI benchmarks are broken: they don’t reflect real work, are expensive to keep current, and their scores can’t be verified; Kaggle reproduced 65 industry benchmarks and found five recurring failure modes. Suyeol Yun (LinqAlpha) presents Mandate-Bench, co-developed with Google DeepMind and Kaggle, which grades whether a model can work like a buy-side analyst from a fund mandate to an investment memo: 50 tasks across 13 sectors, with preliminary results on 10 of them where the best average was 58/100 (Claude Opus 5.5 in Claude Code). The full set is due on Kaggle in November 2026. (The video was retitled “Why Frontier Models Fail as Buy-Side Analysts”; the abstract’s “>60% from the right harness” figure does not appear in the talk.)

<details><summary>Official talk description</summary>

> Your harness matters as much as your model when it comes to institutional buy-side research. In our benchmarks, moving one model to the right harness improved its score by more than 60%, as much as switching models entirely. Off-the-shelf agents fail because they miss hidden accounting traps, trust stale summary tables, and burn their token budget in degenerate search loops.
>
> We’re launching Mandate-Bench, a benchmark built by LinqAlpha with Google DeepMind’s Kaggle Benchmarks team to test real investment origination: the work an analyst does before a portfolio manager ever sizes a position. We evaluate agents on licensed point-in-time market data across 1,300 US stocks and nearly 5,000 SEC filings, where only 15.8% of frontier model runs meet the bar of an institutional portfolio manager.
>
> In this talk, we will walk through our benchmark findings and break down lessons learned from shipping agents in production to real hedge fund clients with billions in AUM.

</details>

**My takeaways**

- A good template for how to build and present a domain benchmark.

<details><summary>Slide photos (28)</summary>

<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/01.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/02.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/03.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/04.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/05.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/06.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/07.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/08.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/09.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/10.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/11.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/12.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/13.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/14.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/15.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/16.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/17.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/18.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/19.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/19.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/20.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/20.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/21.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/21.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/22.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/22.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/23.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/23.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/24.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/24.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/25.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/25.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/26.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/26.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/27.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/27.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/28.jpg"><img src="slides/10-13-1430-why-frontier-models-fail-on-billion-doll/28.jpg" width="260" alt="slide"></a>

</details>

### AI and the Future of Fundamental Investing

**Speakers:** Jack Woodruff (Senior Advisor, Kepler); Vinoo Ganesh (CEO, Kepler)  
**When / where:** 2:55pm–3:13pm · Simons Room (Metropolitan East) · Stage Talk (18 mins)  
**Links:** **[📰 Full write-up](sessions/10-13-1455-ai-and-the-future-of-fundamental-investi.md)** · [Schedule](https://ai.engineer/nyc/2026/schedule?session=a244839c-239c-4789-b621-32a3568a218e) · [▶ Livestream @ 5:29:04](https://www.youtube.com/watch?v=akEqiXFo3GM&t=19744s) · [Related talk by Vinoo Ganesh: “Do Not Let Models Invent Numbers”](https://www.youtube.com/watch?v=dZgA6btnb_s) · [Speaker Q&A (DEV)](https://dev.to/dailycontext/keplers-vinoo-ganesh-on-building-ai-you-can-trust-3p0f) · [Precise notes (中文)](sessions/precise/10-13-1455-ai-and-the-future-of-fundamental-investi.md)

**What it covers.** Fireside chat between Jack Woodruff (Kepler; formerly one of the largest books at Citadel) and Vinoo Ganesh (CEO, Kepler) on what AI is changing in fundamental investing, where it saves real time, and what an investor would never hand over to a model.

<details><summary>Official talk description</summary>

> Jack Woodruff ran one of the largest books at Citadel and went on to found Candlestick Capital. Today he's the buyer every finance AI company wants to win, and in this fireside chat with Vinoo Ganesh, CEO of Kepler and former Head of Business Engineering at Citadel, he'll speak candidly about what AI is changing in fundamental investing and what it isn't. Jack will share where AI is saving real time in his process, what he'd never hand over to a model, and why the judgment call still belongs to the investor. They'll also take on the questions most people only ask in private. Will the biggest funds win everything? If everyone has the same models, where does alpha come from? And when does a number have to be right, not just plausible?

</details>

### Your agent fix just broke something else

**Speaker:** Sofia Jakovcevic (AI Forward Deployed Engineer, Arize AI)  
**When / where:** 3:20pm–3:38pm · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** **[📰 Full write-up](sessions/10-13-1520-your-agent-fix-just-broke-something-else.md)** · [Schedule](https://ai.engineer/nyc/2026/schedule?session=2a589e49-4ee4-4c53-bfeb-40bc3db16506) · [▶ Video](https://www.youtube.com/watch?v=v5tm5EAzsAs) · [Precise notes (中文)](sessions/precise/10-13-1520-your-agent-fix-just-broke-something-else.md)

**What it covers.** A one-line prompt change made a support agent 17 points safer and 17 points worse at finishing tasks. Agents fail at runtime, not in code; the session walks through a five-step regression loop built from production traces (datasets, evaluators, baseline experiments, comparisons, CI gates) and closes with agents that read traces and fix themselves.

<details><summary>Official talk description</summary>

> Agent behavior emerges at runtime. So why are we still debugging only at the code level? A prompt edit, model swap, new skill, or tool change can fix exactly what you were looking at while quietly breaking something you weren't. In one example, a one-line policy fix raised an agent's action-safety score while dropping task completion from 0.89 to 0.72. The aggregate outcome looked fine. The regression was hiding underneath it. This session is a hands-on walkthrough of how to turn your agent observability into a continuous regression testing loop. We'll build a regression dataset from real production traces, run a candidate change against it as a full-agent experiment, diagnose competing metrics, and put the workflow into CI so every change is evaluated before it ships. We'll automate the full feedback loop from observe → evaluate → change → test → improve. Observability is becoming more than humans understanding agents. It's becoming the infrastructure agents need to understand and improve themselves.

</details>

**My takeaways**

- Aggregate metrics hide regressions; code changes are local, behavioral changes are not.

<details><summary>Slide photos (15)</summary>

<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/01.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/02.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/03.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/04.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/05.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/06.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/07.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/08.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/09.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/10.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/11.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/12.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/13.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/14.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1520-your-agent-fix-just-broke-something-else/15.jpg"><img src="slides/10-13-1520-your-agent-fix-just-broke-something-else/15.jpg" width="260" alt="slide"></a>

</details>

### Give the Model a Computer: The One Thing Every AI Agent Framework Agrees On

**Speaker:** Mislav Ivanda  
**When / where:** 3:45pm–4:03pm · Sutton Room (NY Ballroom W) · Expo Session (18 mins)  
**Links:** **[📰 Full write-up](sessions/10-13-1545-give-the-model-a-computer-the-one-thing.md)** · [Schedule](https://ai.engineer/nyc/2026/schedule?session=8d424512-c7f0-483c-9094-ca899d7bb1fd) · [Precise notes (中文)](sessions/precise/10-13-1545-give-the-model-a-computer-the-one-thing.md)  
**Recording:** not published yet

**What it covers.** A year of building Daytona integrations across LangChain, CrewAI, Google ADK, Mastra, Pydantic AI and the OpenAI Agents SDK showed that frameworks that agree on nothing else converge on one primitive: give the model a computer (a sandbox).

<details><summary>Official talk description</summary>

> Building an agent is easy. Deciding where its code runs is the part that follows you to production.
>
> This talk comes from a year of building Daytona integrations across the agent ecosystem: LangChain, CrewAI, Google ADK, Mastra, Pydantic AI, the OpenAI Agents SDK, and a long tail behind them. Integration after integration, the same pattern kept surfacing. Frameworks that agree on nothing, not even the language, not the mental model, not who controls the flow, all converge on one identical primitive: give the model a computer. We'll trace why.
>
> Every framework ends up facing the same question: where does the model's code actually run? Only three answers exist, and two of them stop holding once real users show up. That's why the past year went the way it did, with frameworks deleting their own code interpreters rather than defending defaults they couldn't. And that's why the computer keeps growing: first a tool call, then the place the agent lives, then a thousand machines at once.
>
> You'll leave with one rule: pick the boundary before the framework. Frameworks are opinions. The boundary is a commitment.

</details>

**My takeaways**

- A slide comparing frameworks by who controls the flow (AI-decided vs human-orchestrated) was particularly insightful.

<details><summary>Slide photos (17)</summary>

<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/01.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/02.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/03.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/04.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/05.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/06.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/07.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/08.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/09.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/10.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/11.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/12.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/13.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/14.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/15.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/16.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1545-give-the-model-a-computer-the-one-thing/17.jpg"><img src="slides/10-13-1545-give-the-model-a-computer-the-one-thing/17.jpg" width="260" alt="slide"></a>

</details>

### A Hedge Fund as a Function

**Speaker:** Frank Long (Head of AI, Coatue)  
**When / where:** 4:30pm–4:48pm · Simons Room (Metropolitan East) · Featured Keynote  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=9a9dfcd0-459f-43eb-b3f8-004864b4ad2a) · [▶ Livestream @ 6:47:46](https://www.youtube.com/watch?v=akEqiXFo3GM&t=24466s)

**What it covers.** Coatue’s Head of AI on frameworks for thinking about applying AI to investing.

<details><summary>Official talk description</summary>

> Frameworks for Thinking about Applying AI to Investing

</details>

<details><summary>Slide photos (21)</summary>

<a href="slides/10-13-1630-a-hedge-fund-as-a-function/01.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/02.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/03.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/04.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/05.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/06.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/07.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/08.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/09.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/10.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/11.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/12.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/13.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/14.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/15.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/16.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/17.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/18.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/19.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/19.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/20.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/20.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1630-a-hedge-fund-as-a-function/21.jpg"><img src="slides/10-13-1630-a-hedge-fund-as-a-function/21.jpg" width="260" alt="slide"></a>

</details>

### Frontier Models for Acting in Space and Time

**Speaker:** Pim de Witte (Co-Founder & CEO, General Intuition)  
**When / where:** 4:48pm–5:06pm · Simons Room (Metropolitan East) · Featured Keynote  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=cada670e-c5ea-4977-b3c2-9a68dda26abb) · [▶ Livestream @ 7:02:19](https://www.youtube.com/watch?v=akEqiXFo3GM&t=25339s)

**What it covers.** Pim de Witte introduces General Intuition, a frontier lab building models that act in space and time (raised $650M+ from Khosla and General Catalyst).

<details><summary>Official talk description</summary>

> Pim de Witte is the co-founder and CEO of General Intuition, the frontier lab for acting in space and time. He previously founded Medal, the world’s largest and fastest-growing platform for gaming clips, where millions of gamers share their best moments. Before gaming and AI, he worked with Médecins Sans Frontières and ran the largest Runescape private server at 14. General Intuition recently raised $650M+ from Khosla and General Catalyst.

</details>

<details><summary>Slide photos (2)</summary>

<a href="slides/10-13-1648-frontier-models-for-acting-in-space-and/01.jpg"><img src="slides/10-13-1648-frontier-models-for-acting-in-space-and/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1648-frontier-models-for-acting-in-space-and/02.jpg"><img src="slides/10-13-1648-frontier-models-for-acting-in-space-and/02.jpg" width="260" alt="slide"></a>

</details>

### Hermes gives you wings: Build on open foundations.

**Speaker:** Dillon Rolnick (CEO, Nous Research)  
**When / where:** 5:06pm–5:24pm · Simons Room (Metropolitan East) · Featured Keynote  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=871cd5c8-13f4-4fe9-ac49-c8b25d970add) · [▶ Livestream @ 7:21:20](https://www.youtube.com/watch?v=akEqiXFo3GM&t=26480s)

**What it covers.** Nous Research’s CEO makes the case that modern businesses require open agents, built on open foundations (Hermes).

<details><summary>Official talk description</summary>

> Modern businesses require open agents.

</details>

<details><summary>Slide photos (4)</summary>

<a href="slides/10-13-1706-hermes-gives-you-wings-build-on-open-fou/01.jpg"><img src="slides/10-13-1706-hermes-gives-you-wings-build-on-open-fou/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1706-hermes-gives-you-wings-build-on-open-fou/02.jpg"><img src="slides/10-13-1706-hermes-gives-you-wings-build-on-open-fou/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1706-hermes-gives-you-wings-build-on-open-fou/03.jpg"><img src="slides/10-13-1706-hermes-gives-you-wings-build-on-open-fou/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-13-1706-hermes-gives-you-wings-build-on-open-fou/04.jpg"><img src="slides/10-13-1706-hermes-gives-you-wings-build-on-open-fou/04.jpg" width="260" alt="slide"></a>

</details>

## Wednesday, October 14

### The Hidden Costs of a Faster Model

**Speaker:** Morgan Suo (Head of US Business Development, MiniMax)  
**When / where:** 10:40am–10:58am · McCarthy Room (Riverside Suite) · Breakout Session (18 mins)  
**Links:** **[📰 Full write-up](sessions/10-14-1040-the-hidden-costs-of-a-faster-model.md)** · [Schedule](https://ai.engineer/nyc/2026/schedule?session=f3445fe7-308b-4e84-8490-fd78a13db4fb) · [▶ Video](https://www.youtube.com/watch?v=YvQ4BVTWfHI) · [Precise notes (中文)](sessions/precise/10-14-1040-the-hidden-costs-of-a-faster-model.md)

**What it covers.** A faster model only helps if the finished job gets faster. Uses one research task to test the changes inside MiniMax’s M3.1 Flash (sparse attention, 4-bit KV cache, low-precision experts, speculative decoding, reasoning budgets), each of which saves compute and can quietly cost accuracy or wait time. (The video was retitled “Is a Faster Model Actually Faster? Test the Whole Job”.)

<details><summary>Official talk description</summary>

> Model upgrades are often sold as more capability per dollar. In production, the harder question is what quietly changed underneath: attention sparsity, 4-bit quantization, speculative decoding, or how we prompt and template the model itself. This talk is a practical tour of those changes and the tests they require. Using a recent model release as a concrete case study, we’ll examine how blockwise 4-bit attention and MoE quantization change scale selection and rounding; how speculative decoding changes serving assumptions; and how reasoning controls can alter the path from API request to prompt construction. Attendees will leave with a good understanding of the hidden costs of change that affect latency, output quality, reproducibility, observability, and rollback for teams shipping AI in latency-sensitive and high-stakes environments.

</details>

**My takeaways**

- Verify outputs before trusting a faster model.

<details><summary>Slide photos (13)</summary>

<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/01.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/02.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/03.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/04.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/05.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/06.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/07.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/08.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/09.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/10.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/11.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/12.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1040-the-hidden-costs-of-a-faster-model/13.jpg"><img src="slides/10-14-1040-the-hidden-costs-of-a-faster-model/13.jpg" width="260" alt="slide"></a>

</details>

### Building Better Agents Through End-to-End Evals

**Speaker:** Ryan Stevens (Director of Applied Science, Ramp)  
**When / where:** 11:05am–11:23am · Shannon Room (NY Ballroom E) · Breakout Session (18 mins)  
**Links:** **[📰 Full write-up](sessions/10-14-1105-building-better-agents-through-end-to-en.md)** · [Schedule](https://ai.engineer/nyc/2026/schedule?session=4607dbbe-8f94-4b0e-91ce-257d1cfb5448) · [Precise notes (中文)](sessions/precise/10-14-1105-building-better-agents-through-end-to-en.md)  
**Recording:** not published yet

**What it covers.** Ramp built a benchmark of real accounting workflows to test frontier models, tools, skills, memory and the harness end to end. Some intuitive improvements made the system worse; skill ablation identified context that was hurting performance.

<details><summary>Official talk description</summary>

> More context should make an AI agent better. In our accounting agent, it often did the opposite. At Ramp, we built a benchmark of real-world accounting workflows to test frontier models, tools, skills, memory, and the agent harness end to end. What we found was that some of the most intuitive improvements actually made the system worse. In this talk, I’ll walk through what we learned building and benchmarking a production finance agent: how we used skill ablation to identify context that was hurting performance, reduced one frequently loaded skill from 14k to 5k characters without sacrificing quality, and learned the hard way that agent memory needs to preserve reusable process, not stale facts from the last task. The broader lesson is that production agents are systems problems, not prompt problems. Better models help, but tool design, context management, telemetry, evals, latency, and cost all interact. I’ll share the metrics and development loop we use to decide whether an agent change actually makes the product better. Attendees will leave with a practical framework for evaluating agent-harness changes against quality, reliability, latency, and cost rather than optimizing any one of them in isolation.

</details>

**My takeaways**

- Measurement starts from the agent’s goal. The example is closing the books: reconciliation against the ERP. A benchmark needs clean data for simulation.
- Labels → benchmarks → evals → verifiable outputs is essentially backtesting a production agent.
- Pass@k (pass@1, 2, 3) is a useful way to report reliability.
- In accounting, finding the information or files is not hard; reasoning errors are the failure mode.
- GEPA (auto-tuning skills from failures) is worth a deeper look.

<details><summary>Slide photos (19)</summary>

<a href="slides/10-14-1105-building-better-agents-through-end-to-en/01.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/02.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/03.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/04.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/05.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/06.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/07.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/08.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/09.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/10.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/11.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/12.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/13.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/14.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/15.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/16.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/17.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/18.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1105-building-better-agents-through-end-to-en/19.jpg"><img src="slides/10-14-1105-building-better-agents-through-end-to-en/19.jpg" width="260" alt="slide"></a>

</details>

### We Let Our Financial Planners Write the Agents

**Speaker:** Matt Ragone (Financial Planner, Range)  
**When / where:** 11:30am–11:48am · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** **[📰 Full write-up](sessions/10-14-1130-we-let-our-financial-planners-write-the.md)** · [Schedule](https://ai.engineer/nyc/2026/schedule?session=9b002687-09cf-42e2-9282-532ed9d0c184) · [▶ Video](https://www.youtube.com/watch?v=gpmze-6ueGU) · [Precise notes (中文)](sessions/precise/10-14-1130-we-let-our-financial-planners-write-the.md)

**What it covers.** Range’s financial planners write and maintain the agent skills they use every day, without writing code: 24 skills company-wide after six months, 17 of them in planning. The planner reviews every recommendation before it reaches a client.

<details><summary>Official talk description</summary>

> We show how our CFPs built organizational skills to make planning more efficient and effective. Our planners write and maintain the agent skills they use daily, and none of them write code.
>
> We started by using AI as a chat box to polish client messages. The real shift came when work began happening in the background: I can be on a call with one client while a full cash flow or insurance analysis is drafted for another, then review, adjust, and send recommendations afterward. A planner’s bottleneck was never typing speed, but being in only one place at a time.
>
> That requires an agent that works where the work lives and knows how we do planning, not how planning works in general. That’s a skill. To build one: pick a repeatable workflow, walk the agent through one real case while narrating your judgment, package it with the questions it must ask and the limits it must respect, test it on five to ten real cases, then ship it with an owner and a reviewer.
>
> Six months in, we have 24 skills (17 in planning), and adoption spread to sales, product, and marketing without a mandate. I’ll cover what changed when subject matter experts built for their own niches, how we peer-review skills like we review plans, and our open question: how to check quality at scale when the output is judgment.

</details>

**My takeaways**

- AI adoption does not need an engineering background; examples were cash-flow and insurance analyses.

<details><summary>Slide photos (13)</summary>

<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/01.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/02.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/03.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/04.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/05.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/06.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/07.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/08.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/09.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/10.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/11.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/12.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1130-we-let-our-financial-planners-write-the/13.jpg"><img src="slides/10-14-1130-we-let-our-financial-planners-write-the/13.jpg" width="260" alt="slide"></a>

</details>

### The Model Ate the Coding Harness. Now What?

**Speaker:** Jediah Katz (Engineer, Agent Harness, SpaceX Super Intelligence)  
**When / where:** 11:55am–12:13pm · Shannon Room (NY Ballroom E) · Breakout Session (18 mins) · ⭐ starred  
**Links:** **[📰 Full write-up](sessions/10-14-1155-the-model-ate-the-coding-harness-now-wha.md)** · [Schedule](https://ai.engineer/nyc/2026/schedule?session=29083884-87dc-44f8-b412-b494afca8856) · [▶ Video](https://www.youtube.com/watch?v=7Y4sbZgBdnc) · [Precise notes (中文)](sessions/precise/10-14-1155-the-model-ate-the-coding-harness-now-wha.md)

**What it covers.** Jediah Katz (SpaceXAI; previously Cursor’s agent-harness engineer) on what smarter models mean for coding agents, Grok Bot and the future of harnesses. Fancy harnesses no longer add coding intelligence because the data flywheel teaches models the skills they used to patch in; the harness matters more for what models can’t absorb: infrastructure, trust and security, efficiency. (The video was retitled “Is the Agent Harness Dead?”.)

<details><summary>Official talk description</summary>

> Jediah Katz discusses how we built Cursor and Grok Bot, and where we think the agent harness is going.

</details>

**My takeaways**

- The harness kicks off the data flywheel and unlocks the software factory.
- Grok Bot examples: key website flows, communication, acting vs asking, learning over time.
- Permissions and API fees remain harness concerns.

<details><summary>Slide photos (23)</summary>

<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/01.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/02.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/03.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/04.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/05.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/06.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/07.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/08.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/09.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/10.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/11.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/12.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/13.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/14.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/15.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/16.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/17.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/18.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/19.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/19.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/20.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/20.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/21.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/21.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/22.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/22.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/23.jpg"><img src="slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/23.jpg" width="260" alt="slide"></a>

</details>

### A/B Testing AI Products Is Weird: What Changes When the Product Is Probabilistic

**Speaker:** Supriya Subudhi (Staff Product Manager, Bumble)  
**When / where:** 12:20pm–12:38pm · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** **[📰 Full write-up](sessions/10-14-1220-ab-testing-ai-products-is-weird-what-cha.md)** · [Schedule](https://ai.engineer/nyc/2026/schedule?session=11fef189-97e8-4c8e-87e8-dac43d800846) · [Speaker article (DEV)](https://dev.to/dailycontext/ab-testing-ai-products-is-weird-576h) · [Precise notes (中文)](sessions/precise/10-14-1220-ab-testing-ai-products-is-weird-what-cha.md)  
**Recording:** not published yet

**What it covers.** Traditional experimentation assumes you can define a feature, measure a response and compare outcomes. AI outputs vary, user responses depend on context, and a metric win does not say why the experience improved. Draws on experimentation in consumer tech and fintech, including an AI-powered experience at Bumble.

<details><summary>Official talk description</summary>

> Traditional product experimentation assumes that we can define a feature, measure a response, and compare outcomes. AI products make that much harder. Outputs can vary, user responses can depend on context, and a metric improvement does not necessarily tell you why the experience improved.
>
> Drawing on experimentation across consumer technology and fintech, as well as the development of an AI-powered product experience at Bumble, I will examine what product teams need to consider when evaluating AI-powered experiences.
>
> We will look at common experimentation failure modes including metric selection, novelty effects, premature conclusions, and misleading behavioural signals, then connect them to the additional complexity introduced when the product experience itself is generated or personalised. I will share a practical approach for defining the product question first, selecting meaningful outcomes, interpreting experiment results, and deciding when an AI feature is ready to scale.

</details>

**My takeaways**

- A successful A/B test is where the journey begins. Five traps: (1) users asked for it, then ignored it; (2) the average win hides the tail; (3) green dashboard, red bill; (4) swap the model and the result flips; (5) AI regulation keeps changing.
- Token-based pricing is the root of the “green dashboard, red bill” tension; time-based billing with enough throughput might fit better.

<details><summary>Slide photos (8)</summary>

<a href="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/01.jpg"><img src="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/02.jpg"><img src="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/03.jpg"><img src="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/04.jpg"><img src="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/05.jpg"><img src="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/06.jpg"><img src="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/07.jpg"><img src="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/08.jpg"><img src="slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/08.jpg" width="260" alt="slide"></a>

</details>

### Building a self-improving software factory

**Speaker:** Zach Lloyd (Founder & CEO, Warp)  
**When / where:** 1:40pm–1:58pm · McCarthy Room (Riverside Suite) · Expo Session (18 mins) · ⭐ starred  
**Links:** **[📰 Full write-up](sessions/10-14-1340-building-a-self-improving-software-facto.md)** · [Schedule](https://ai.engineer/nyc/2026/schedule?session=a901d5ea-57c6-433f-9c37-5fe6531adb99) · [Precise notes (中文)](sessions/precise/10-14-1340-building-a-self-improving-software-facto.md)  
**Recording:** not published yet

**What it covers.** Warp argues interactive agents are not scaling (runaway cost, no governance) and that teams need software factories: closed-loop systems for automating development, with shared infrastructure rather than every company rolling its own.

<details><summary>Official talk description</summary>

> Interactive agents aren’t scaling. Every company is coping with runaway costs, lack of governance, and difficulty automating.
>
> This has led to the need for software factories: a closed-loop system for automating development, with primitives that provably improve throughput and efficiency over time.
>
> Right now, folks are trying to roll their own internal software factories from bare metal, solving the same problems 100 different ways. We need shared, flexible infrastructure that everyone can build upon.
>
> In this talk, we’ll share the core building blocks every company needs to build their own self-improving software factories based on our own experience building Warp Factories:
>
> 1. Cloud infrastructure to centralize agent conversations, skills, and security permissions
>
> 2. Configuration as code for humans and agents to manage that cloud infrastructure
>
> 3. Metrics for humans and agents to track efficiency and code quality, beyond “number of PRs shipped”
>
> 4. Self-improvement loops for agents to review past conversations, score the quality, and propose improvements to the skills driving the factory
>
> You’ll walk away with a clear understanding of how to improve coding agent ROI, and how to deploy a system of your own using Warp Factories.
>
> This session will include both slides and a live demonstration of:
>
> - How to run tasks through a software factory
>
> - How "self-improvement" works, watching agents review past conversations to suggest improvements to skills in a code repository
>
> This will include info about Warp Factories, but is meant to be useful to any team learning about software factories or building their own.

</details>

**My takeaways**

- Local agents move to sandboxes so they can be governed; the loop starts from a product idea.
- Factory infrastructure: data, compute and an inference gateway (any model, any harness).
- Measurement over vibes: benchmarks and a score to drive self-improvement. Open questions: data curation, how to define correctness, LLM-as-judge.

<details><summary>Slide photos (19)</summary>

<a href="slides/10-14-1340-building-a-self-improving-software-facto/01.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/02.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/03.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/04.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/05.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/06.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/07.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/08.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/09.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/10.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/11.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/12.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/13.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/14.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/15.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/16.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/17.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/18.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1340-building-a-self-improving-software-facto/19.jpg"><img src="slides/10-14-1340-building-a-self-improving-software-facto/19.jpg" width="260" alt="slide"></a>

</details>

### Scaling Agentic RL Across the Globe

**Speaker:** Jason Mancuso (Member of Technical Staff, Research, Modal)  
**When / where:** 2:05pm–2:23pm · Shannon Room (NY Ballroom E) · Expo Session (18 mins) · ⭐ starred  
**Links:** **[📰 Full write-up](sessions/10-14-1405-scaling-agentic-rl-across-the-globe.md)** · [Schedule](https://ai.engineer/nyc/2026/schedule?session=e6ccda8d-0613-44f8-b98f-53ba8037b518) · [Stitch (open-source RL framework)](https://github.com/modal-projects/stitch) · [Precise notes (中文)](sessions/precise/10-14-1405-scaling-agentic-rl-across-the-globe.md)  
**Recording:** not published yet

**What it covers.** Modal supports customers scaling agent RL across a multi-cloud, multi-region fleet and presents Stitch, an open-source framework for disaggregating RL training.

<details><summary>Official talk description</summary>

> The industry is rushing to integrate agents into every surface of every product and service. To do so sustainably, keeping cost, quality, performance, and reliability in check, most organizations have turned to posttraining and deploying open-source models. Reinforcement learning is the most critical component of the posttraining stack. At Modal, we've needed to support our customers in scaling up agent RL across our multi-cloud, multi-region fleet. We'll present Stitch, our open-source framework for disaggregating RL rollouts across clouds, regions, and heterogenous GPU types, as well as a stable recipe for posttraining agentic models on off-policy rollout data.

</details>

**My takeaways**

- Why fine-tune: quality of service, proprietary data formats, and competing on task intelligence per dollar.
- RL generalizes better than SFT. With an agent harness, evals and production traces, you can iterate.
- Recipe: GRPO plus masked importance sampling plus score centering to limit drift.

<details><summary>Slide photos (24)</summary>

<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/01.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/02.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/03.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/04.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/05.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/06.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/07.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/08.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/09.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/10.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/11.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/12.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/13.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/14.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/15.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/16.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/17.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/18.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/19.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/19.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/20.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/20.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/21.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/21.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/22.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/22.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/23.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/23.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1405-scaling-agentic-rl-across-the-globe/24.jpg"><img src="slides/10-14-1405-scaling-agentic-rl-across-the-globe/24.jpg" width="260" alt="slide"></a>

</details>

### Your agent needs a skill to improve its skills

**Speaker:** Ben Holmes (DevEx Lead, Warp)  
**When / where:** 2:30pm–2:48pm · Turing Room (Riverside Ballroom) · Breakouts · ⭐ starred  
**Links:** **[📰 Full write-up](sessions/10-14-1430-your-agent-needs-a-skill-to-improve-its.md)** · [Schedule](https://ai.engineer/nyc/2026/schedule?session=0a0cbd67-23fa-4a3d-a8dc-06e3caa3ab2f) · [Precise notes (中文)](sessions/precise/10-14-1430-your-agent-needs-a-skill-to-improve-its.md)  
**Recording:** not published yet

**What it covers.** Warp found its orchestration agent wasting hundreds of tool calls in a month because of one bad instruction in a message-passing skill. Shows how to build a feedback loop that evaluates past agent conversations and finds failures in the skills themselves.

<details><summary>Official talk description</summary>

> We just found out our orchestration agent was wasting hundreds of tool calls this past month. The reason? One bad instruction in our message passing skill.
>
> Peeling back the curtain, there are dozens more examples you probably have in your own agent skills too. As you scale these agents to longer-running workloads, those gaps add up to real token cost.
>
> To solve it, you'll need a feedback loop for agentic self-improvement. I'll show you how to build one: a system that evaluates past agent conversations, finds failure points, and turns those findings into skill improvements you'd want to merge. We'll set up the evaluation loop from scratch, and show how to automate it as part of a software factory.
>
> You'll walk away with the skills you need (pun intended) to close the loop and let your agentic system improve itself.

</details>

<details><summary>Slide photos (20)</summary>

<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/01.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/02.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/03.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/04.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/05.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/06.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/07.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/08.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/09.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/10.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/11.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/12.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/13.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/14.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/15.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/16.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/17.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/18.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/19.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/19.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/20.jpg"><img src="slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/20.jpg" width="260" alt="slide"></a>

</details>

### Build your own team of managed agents

**Speaker:** Jack Wotherspoon (Member of Technical Staff, Google DeepMind)  
**When / where:** 2:55pm–3:13pm · Shannon Room (NY Ballroom E) · Expo Session (18 mins)  
**Links:** **[📰 Full write-up](sessions/10-14-1455-build-your-own-team-of-managed-agents.md)** · [Schedule](https://ai.engineer/nyc/2026/schedule?session=970f672e-f351-46bb-9790-78c6b1baf81d) · [▶ Video](https://www.youtube.com/watch?v=Y_p3y6zP5QE) · [Precise notes (中文)](sessions/precise/10-14-1455-build-your-own-team-of-managed-agents.md)

**What it covers.** Gemini Managed Agents run coding agents in cloud sandboxes from a single API call: the Interactions API, the Antigravity harness, environments shared across agents, uploaded skills and AGENTS.md files, scheduled triggers and multi-agent workflows.

<details><summary>Official talk description</summary>

> To do real work, an agent needs its own computer where it can research, write files, and execute code. Giving agents their own sandbox means you can stop babysitting your agents and let them run and verify their own work in a secure environment.
>
> This used to be difficult to set up or do on your own but now it is a single API call away. Managed agents give you a cloud sandbox, a file system, and a full agent loop in a single API call. Build an agent for your daily tasks or build one into your product that you can scale to thousands of customers.
>
> Let’s put it to the test by building a full team of financial agents in just a few minutes!

</details>

**My takeaways**

- Multiple agents can point at the same sandbox.
- An egress proxy keeps secrets out of the sandbox.

<details><summary>Slide photos (19)</summary>

<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/01.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/02.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/03.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/04.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/05.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/06.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/07.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/08.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/09.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/10.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/11.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/12.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/13.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/14.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/15.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/16.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/16.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/17.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/17.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/18.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/18.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1455-build-your-own-team-of-managed-agents/19.jpg"><img src="slides/10-14-1455-build-your-own-team-of-managed-agents/19.jpg" width="260" alt="slide"></a>

</details>

### A Roadmap for Evaluating Highly-Subjective Tasks

**Speaker:** Cameron Wolfe (Staff Research Scientist, Netflix)  
**When / where:** 3:20pm–3:38pm · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** **[📰 Full write-up](sessions/10-14-1520-a-roadmap-for-evaluating-highly-subjecti.md)** · [Schedule](https://ai.engineer/nyc/2026/schedule?session=f5c310e9-b0bb-4fa6-bc0d-bda758e1c464) · [Slides (public Google Slides)](https://docs.google.com/presentation/d/146TscsJ21LU4kATVS3ES2oC_oZxoCBsTH3soepY25P4/edit?slide=id.g3ec03477a51_0_0#slide=id.g3ec03477a51_0_0) · [Precise notes (中文)](sessions/precise/10-14-1520-a-roadmap-for-evaluating-highly-subjecti.md)  
**Recording:** not published yet

**What it covers.** Roadmap for building large-scale automatic evals for subjective tasks: coordinating human annotation, building and evaluating LLM judges, using production monitoring to inform evals, and reducing uncertainty with statistics.

<details><summary>Official talk description</summary>

> This presentation provides a technical roadmap for building large-scale automatic evaluations for subjective tasks. Specific concepts that will be covered include setting up and coordinating human annotation efforts, building and evaluation LLM judges, using production monitoring to inform model evaluations, and reducing evaluation uncertainty via statistics. By combining these components and monitoring them properly, we can create a scalable evaluation framework that evolves and improves over time as our understanding of a domain deepens.

</details>

**My takeaways**

- Start with human evaluation: a small set of quality criteria, a clear way to rate them, and specific examples. Offline evaluation, human evaluation and production monitoring work together.

<details><summary>Slide photos (1)</summary>

<a href="slides/10-14-1520-a-roadmap-for-evaluating-highly-subjecti/01.jpg"><img src="slides/10-14-1520-a-roadmap-for-evaluating-highly-subjecti/01.jpg" width="260" alt="slide"></a>

</details>

### Code Mode: your agent should write code

**Speaker:** Mikyo King (Head of Open Source, Arize AI)  
**When / where:** 3:45pm–4:03pm · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** **[📰 Full write-up](sessions/10-14-1545-code-mode-your-agent-should-write-code.md)** · [Schedule](https://ai.engineer/nyc/2026/schedule?session=8413d6ee-ceb1-4095-9f27-49d518f1dd5a) · [▶ Video](https://www.youtube.com/watch?v=dAoOYFx7ZYw) · [Precise notes (中文)](sessions/precise/10-14-1545-code-mode-your-agent-should-write-code.md)

**What it covers.** Give an agent 40 tools and it gets worse at using any of them; give it a sandbox and it can compose what it needs. Covers how Arize built code mode into Phoenix (Bash, a Python sandbox, SQL, even UI control), its hidden costs (secrets, permissions, durability, double sandboxing), and Harbor evals showing 43% fewer tokens and 31% lower cost. (The video was retitled “Stop Giving Your Agent Tools. Give It a Sandbox”.)

<details><summary>Official talk description</summary>

> Give an agent 40 tools and it gets worse at using any of them. Give it a sandbox and a way to write code and it can compose whatever it needs. Recent research has bash-only agents beating typed tool catalogs by 21.8 to 24.5 points on the same benchmark while spending fewer tokens, and the industry is quietly moving in that direction. The shift is really one of abstraction. We used to obsess over prompts; now we optimize skills. We used to design individual tools; now we ship MCP servers and plugins. We've stopped handing agents primitive verbs and started handing them programming. This talk makes the case for code mode while staying realistic about what it costs, and closes with how our open-source observability stack exposes agent traces to coding agents through read-only SQL and code mode, and how we use our own evals and tooling to improve agent outcomes.

</details>

**My takeaways**

- Tool calling vs code mode: (1) tool search, (2) composability (loops, branches, chaining; cf. Claude agent teams), (3) token efficiency, (4) emergent behavior.
- Fewer tool calls makes sense, though in practice it resembles bundling functionality.

<details><summary>Slide photos (13)</summary>

<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/01.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/02.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/03.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/04.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/05.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/06.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/07.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/08.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/09.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/10.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/11.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/12.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1545-code-mode-your-agent-should-write-code/13.jpg"><img src="slides/10-14-1545-code-mode-your-agent-should-write-code/13.jpg" width="260" alt="slide"></a>

</details>

### AI in Practice: An Inside Look at Apollo's Approach

**Speakers:** Michael Manapat (Co-Founder, Rowspace); Jack McDonald (Principal at Apollo Global Management, COO of Redding Ridge, Apollo Global Management)  
**When / where:** 4:30pm–4:48pm · Simons Room (Metropolitan East) · Featured Keynote  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=44e9d04a-e8b4-418c-8a85-02fbb9263dd0) · [▶ Video](https://www.youtube.com/watch?v=BOWYujhIQuo) · [▶ Livestream @ 7:50:03](https://www.youtube.com/watch?v=jiUC6IUMdvc&t=28203s)

**What it covers.** Apollo’s CLO business used to spend 2–3 days building one mock portfolio by hand in Excel; with Rowspace it takes minutes. A customer co-presentation on applying AI to fragmented credit data. (The video was retitled “AI Agents Cut 3-Day Portfolio Builds to Minutes”.)

<details><summary>Official talk description</summary>

> Rowspace customer co-presenting keynote

</details>

**My takeaways**

- Case 1: portfolio construction for CLOs, tracking where money flows. Days of manual Excel became minutes, in real time.
- Case 2: a warehouse purchase workflow that was manual and ended in a written recap.
- The vendor’s role resembles a forward-deployed engineer: understanding the workflow on top of fragmented data.

### Process Supervision for Agents That Do Long-Horizon Work

**Speaker:** Mitchell Troyanovsky (Co-Founder, Basis)  
**When / where:** 4:48pm–5:06pm · Simons Room (Metropolitan East) · Featured Keynote · ⭐ starred  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=1cbc625f-7f07-49bb-9c5c-57c95f07ba77) · [▶ Livestream @ 8:09:22](https://www.youtube.com/watch?v=jiUC6IUMdvc&t=29362s) · [Basis behavior specs](https://www.agentbehavior.dev/)

**What it covers.** Basis builds agents for accountants. A long-horizon agent can reach a correct result through unreliable steps, such as a tax conclusion based on pretraining or a blog rather than primary sources. Evaluating long-horizon work needs evidence about both the result and the decisions behind it.

<details><summary>Official talk description</summary>

> When an agent takes on hours of research, data extraction, tool use, and judgment, it may make hundreds of decisions before producing a final result. That result can be correct even when the steps behind it are unreliable. Evaluating long-horizon work requires evidence about both the result and the decisions that produced it.
>
> Basis builds AI agents for accountants and encounters this problem across complex accounting work. An agent may reach the correct tax conclusion while relying on pretraining or a blog instead of checking the tax code and regulations. An outcome eval marks the answer correct, even though the research would not meet the standard an accountant expects.
>
> Basis developed behavior specs to evaluate those decisions across the full trajectory. Each spec defines how an agent should behave when a specific situation occurs. A judge reviews the trajectory and determines whether that situation arose and whether the agent followed the expected behavior. Basis partnered with Braintrust to publish the approach as an open standard for teams building long-horizon agents.
>
> In this session, Mitch Troyanovsky will explain why outcome evals become expensive and slow to iterate on for long-horizon work, how Basis chooses the few behaviors worth measuring, and how behavior failures guide changes to prompts, tools, or runtime context. He will also discuss how teams can evaluate behavior across production traffic without turning the spec into an unmanageable list of every rule an agent might follow.

</details>

**My takeaways**

- Outcome evals are not enough: two runs can end the same way while the process differs. Developing opinions is not only about accuracy but also things like time.
- Process supervision: judge the steps, not just the answer. The “runtime trading data” analogy fits, since an agent’s context changes in real time.
- Basis’s behavior specs capture an agreed opinion from the builders (agentbehavior.dev).

<details><summary>Slide photos (16)</summary>

<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/01.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/01.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/02.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/02.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/03.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/03.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/04.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/04.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/05.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/05.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/06.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/06.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/07.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/07.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/08.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/08.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/09.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/09.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/10.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/10.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/11.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/11.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/12.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/12.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/13.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/13.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/14.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/14.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/15.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/15.jpg" width="260" alt="slide"></a>
<a href="slides/10-14-1648-process-supervision-for-agents-that-do-l/16.jpg"><img src="slides/10-14-1648-process-supervision-for-agents-that-do-l/16.jpg" width="260" alt="slide"></a>

</details>

### Everything will be Simulated

**Speaker:** Cristóbal Valenzuela (Co-Founder and Co-CEO, Runway)  
**When / where:** 5:06pm–5:24pm · Simons Room (Metropolitan East) · Featured Keynote  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=af8422e8-d5f8-4dbd-a918-6360f5bfba80) · [▶ Livestream @ 8:29:11](https://www.youtube.com/watch?v=jiUC6IUMdvc&t=30551s)

**What it covers.** Runway’s co-founder on world models mature enough to simulate real environments in real time, and what that opens up for robotics, digital interfaces and science.

<details><summary>Official talk description</summary>

> AI models have spent years learning to generate text, images, and video. The next frontier of AI development focuses on models that understand space, motion, and the physical dynamics of the real world. Cristóbal Valenzuela, Co-CEO and Co-Founder of Runway, will discuss world models mature enough to simulate real environments in real time, and what this progress opens up for robotics, digital interfaces, and broader scientific discovery.

</details>

**My takeaways**

- The “world interface model” idea is interesting: it can show, for example, the inside of a car directly, with no code required.
