# AI Engineer NYC 2026: Conference Report

My notes from **[AI Engineer NYC 2026](https://ai.engineer/nyc/2026)** (October 12–14, 2026, Sheraton New York Times Square), an event focused on production AI in banking, hedge funds, fintech, insurance and asset management.

This repository lists the 34 sessions I attended: who spoke, what each talk covered, what I took away, and a link to the recording where one exists. It is the companion reference for my conference write-up and talk.

- ⭐ marks a session I starred as a highlight while attending.
- **What it covers** is based on the official abstract and, where a recording exists, its YouTube description. **My takeaways** are from my own notes taken during the talk.
- Recordings are being published gradually. A session without a video link had no recording on the channel when this was last checked (**2026-10-27**).
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
- [Monday, October 12](#monday-october-12) (1 session)
- [Tuesday, October 13](#tuesday-october-13) (19 sessions)
- [Wednesday, October 14](#wednesday-october-14) (14 sessions)

## Sessions at a glance

| Day | Time | Session | Speaker | Video |
| --- | --- | --- | --- | --- |
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

### Unhobbling the Frontier

**Speaker:** Lance Martin (Member of Technical Staff, Anthropic)  
**When / where:** 3:30pm–4:30pm · Shannon Room (NY Ballroom E) · Workshop (1-2 hrs)  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=6a4e7cfd-334a-4d3b-94ed-282acc905448)  
**Recording:** not published yet

**What it covers.** Hands-on workshop on getting more out of frontier models: harness design, prompting, and setting up evals so an agent can be improved by hill-climbing.

**My takeaways**

- Audit your prompts: contradictory instructions make a prompt confusing for the model.
- Use prompt caching on the Claude API. Claude API skills help with cost work (`/claude-api cost-optimize`) and evals (`/claude-api build-eval`, `/claude-api hillclimb`).
- Design evals so they are hill-climbable, and watch for overfitting to the eval set.
- Design the harness for where the model is going, not where it is today.
- Q&A: for open-ended criteria (e.g. UI), use a well-structured rubric humans can reason about, binary scores instead of 0–5, multiple judges or models, and replicates. Pairwise comparison suits ambiguous cases. Let Claude decide when a behavior should become a skill.

## Tuesday, October 13

### Vault: Making Unstructured Data Work for Investing

**Speakers:** Aaron Linsky (Head of Product Engineering, Bridgewater); Sam Green (Team Lead, Applied AI - AIA Labs, Bridgewater)  
**When / where:** 9:00am–9:18am · Simons Room (Metropolitan East) · Featured Keynote  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=a84ca3a8-cc2f-4b50-a635-fc380f6f04f6) · [▶ Video](https://www.youtube.com/watch?v=CLve4Nx_RwI) · [▶ Livestream @ 7:22](https://www.youtube.com/watch?v=akEqiXFo3GM&t=442s)

**What it covers.** Bridgewater built the Vault: 100M+ documents from 135 external sources plus internal memos and meetings, cleaned, permissioned and tagged to the firm’s ontology. Three lessons: humans and agents need different things from search, the biggest wins come from one tool that serves both, and the real prize is agents that proactively surface what deserves an investor’s attention. A hand-built benchmark took precision@10 from about 60% to over 90%.

**My takeaways**

- Directly relevant to agent-driven investing and research workflows.

### Anthropic: Accelerating frontier AI in Financial Services.

**Speakers:** Belinda Neal (Managing Director, Financial Services, Anthropic); Keon Shahab (Applied AI Architect, Anthropic)  
**When / where:** 9:18am–9:36am · Simons Room (Metropolitan East) · Featured Keynote  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=68a1e78b-569e-4314-a56f-f20508123cbe)  
**Recording:** not chaptered in the livestream, and no standalone video found

**What it covers.** Anthropic’s financial-services team frames two frontiers every firm has to reach: the frontier of intelligence (what the best model can do today) and the frontier of adoption (how much of the firm’s real work runs on it). Scaling from a handful of agents to many means building connectors, skills and controls once.

### Balancing Acceleration and Safety for AI Agents

**Speaker:** John Feminella (SVP Architecture, Two Sigma)  
**When / where:** 9:36am–9:54am · Simons Room (Metropolitan East) · Featured Keynote  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=73badd10-6f8c-4c95-b5e7-55fc3d7bafc1) · [▶ Video](https://www.youtube.com/watch?v=iniryju23Sk) · [▶ Livestream @ 23:55](https://www.youtube.com/watch?v=akEqiXFo3GM&t=1435s) · [Speaker write-up (DEV)](https://dev.to/dailycontext/brakes-let-you-go-fast-balancing-acceleration-and-safety-for-ai-agents-49p9)

**What it covers.** Uses the 2020 Citibank/Revlon $900M payment error to show how to give agents real autonomy without locking everything down. Five composable controls: an agent registry, risk-based review, tool access that follows an agent’s declared purpose, shared firm-wide evals, and classifiers that send humans only the approvals that matter. (The video was retitled “What a $900M Bank Error Teaches Us About AI Agents”.)

**My takeaways**

- Agent registry plus scoring, with layered review depending on risk.
- Approval fatigue makes per-action human approval unworkable; a classifier that decides which approvals reach a human is the answer (comparable to Claude’s auto mode).
- Open questions: whether to scope permissions with allow/deny lists or by declared purpose, whether agents declaring their own permissions makes humans the bottleneck, and why evals should be shared firm-wide rather than bespoke.
- Framing: cars have brakes so they can go fast.

### Does Code Quality Even Matter?

**Speaker:** Max Kanat-Alexander (Principal Software Engineer, Capital One)  
**When / where:** 9:54am–10:12am · Simons Room (Metropolitan East) · Featured Keynote  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=5191589c-a70e-4806-99dc-86f7794f46b9) · [▶ Video](https://www.youtube.com/watch?v=bU5gDVyu_28) · [▶ Livestream @ 43:51](https://www.youtube.com/watch?v=akEqiXFo3GM&t=2631s)

**What it covers.** Code quality exists for two things: understanding a system and safely changing it. AI makes a new kind of software common, disposable software with low consequences, few dependents and near-zero replacement cost, where quality does not matter. For durable software it matters more than ever. (The video was retitled “Does Code Quality Still Matter? It Depends on This”.)

**My takeaways**

- Quality risk compounds: poor structure makes the system harder to understand, which makes future problems more likely.
- Durable software lives for years, is depended on by many, has major failure consequences and needs continuous maintenance. Disposable software is the opposite.
- Make software more disposable through isolation, independence, redundancy and limited scope.
- Make rigor cheap for durable software: standardization, tooling inside the workflow, easier testing (test infrastructure such as in-memory fakes), and maximum strictness on lint rules, with false positives mattering less.
- It is not black and white, and durable software engineering is not going away.

### What does an AI engineer do?

**Speaker:** Aparna Dhinakaran (Co-Founder & Chief Product Officer, Arize AI)  
**When / where:** 10:12am–10:30am · Simons Room (Metropolitan East) · Featured Keynote  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=b2ed4456-5671-4e8e-83fd-684c012fef8c) · [▶ Video](https://www.youtube.com/watch?v=ImnZ7s68jw8) · [▶ Livestream @ 1:00:43](https://www.youtube.com/watch?v=akEqiXFo3GM&t=3643s)

**What it covers.** As the field moves from writing features to running factories of agents, the job becomes building the loop: evals that review non-deterministic work, telemetry that agents consume, and agents that act on it. Walks through Arize’s Signal, an always-on agent that reads traces, remembers past issues and opens PRs. (The video was retitled “What Does an AI Engineer Do Now?”.)

### Can Any Model Write Like Me? Building a Benchmark for Creative Writing

**Speaker:** Louis-François Bouchard (CTO & Co-Founder, Towards AI)  
**When / where:** 10:40am–10:58am · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=736edd83-bb08-49f1-90ed-722b7ed07eb2) · [▶ Video](https://www.youtube.com/watch?v=_dTF3NpPzGo) · [Speaker article (DEV)](https://dev.to/dailycontext/what-9050-ai-written-scripts-taught-me-about-slop-1g5n)

**What it covers.** Which model writes a YouTube script that sounds like the author? Breaks down what makes AI writing “slop” and shows how a personal benchmark was built: 10 scripts, 5 drafts per model, 4 LLM judges, nine scoring metrics and 1,000 human A/B checks. (The video was retitled “Why AI Writing Still Reads Like Slop”.)

**My takeaways**

- Slop is mostly overused words and being too general.
- Five drafts per model, because of outliers; judges prefer their own model family.
- The benchmark cost about $3,750, and changing a single rubric or prompt makes experiments expensive.
- Writing with AI: don’t one-shot, don’t use max effort for writing, give very clear directions with long detailed skills and examples, and build your own evaluation.

### Write the Answer First: Generating Eval Datasets for Agents With No Benchmark

**Speaker:** Nathanael Weill (Director, Data Science & AI, Air Canada)  
**When / where:** 11:05am–11:23am · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=c4ba0138-6f6d-4780-93a1-4d8f0c478339) · [▶ Video](https://www.youtube.com/watch?v=c_SPW__jF9w)

**What it covers.** Air Canada has agents with no public benchmark and domain experts with no time. The fix is to write the answer first: start from a known answer, generate the input from it, and check whether the agent finds its way back. A past RAG project went from one expert-graded test a week to dozens of automated tests a day; the same idea is applied to a supplier-contract review agent. (The video was retitled “How to Test AI Agents When No Benchmark Exists”.)

**My takeaways**

- Every added bit of complexity made the earlier RAG system worse, and each change waited a week for expert review. Writing the answer first made the task automatable.
- No public benchmark exists for supplier contract review, so the test set has to be built in-house, paired with a longer outer loop.

### Your Financial Agent Passed Every Eval. Can You Defend Its Decision?

**Speaker:** Anirudh Veeraragavan (Lead Security Engineer, Fieldguide)  
**When / where:** 11:30am–11:48am · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=30afe596-abe2-417c-bd5a-313fa4bc54b3)  
**Recording:** not published yet

**What it covers.** A financial agent can pass every eval and still make a decision nobody can defend. Defensibility needs four controls: authorization, execution control, evidence provenance and evaluation integrity, worked through on a synthetic audit decision trace.

### Lies, Damned Lies, and Evals: How to trust your agents when the stakes are high

**Speaker:** Stephanie Jarmak (AI Engineer, Omni Analytics)  
**When / where:** 11:55am–12:13pm · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=b1fe8396-6fa8-4574-9775-0dc01f56ce57) · [▶ Video](https://www.youtube.com/watch?v=A3FL_zTQKU0)

**What it covers.** Most agent evaluations are easy to misread. Swapping only the harness moved results by 30%; public benchmarks are for shortlisting, not deciding. Design your own evals like an experiment: realistic tasks, repeated runs, full traces and a measurement that matches what you care about. (The video was retitled “Don’t Trust Your AI Agents. Test Them Like a Scientist”.)

**My takeaways**

- Public benchmark vs internal evaluation: a law-firm agent is not served by a terminal-coding benchmark.
- Set up comparison experiments across model, task and harness. In the example, the PI harness did much worse than OpenHands.
- Human review load is a real bottleneck.

### 1,368 AI Failures: What They Teach Us About System Evals

**Speakers:** Vishal Srivastava (Corporate VP, Lead Data Scientist, New York Life Insurance); Tanmay Sah (AI Researcher)  
**When / where:** 12:20pm–12:38pm · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=9d72d7ac-a2a2-42cf-b880-5859316d840c) · [▶ Video](https://www.youtube.com/watch?v=mMw3arwQsL0) · [Speaker article (DEV)](https://dev.to/dailycontext/what-1368-real-world-ai-incidents-teach-us-about-evaluation-71p)

**What it covers.** Analysis of 1,368 documented AI incidents across three areas (technical strength, deployment controls, business impact) and 13 sub-areas covering about 98% of incidents. Weak deployment controls such as human review and monitoring are most closely tied to harm. Includes a build-break-fix loop that tries to prove a system wrong rather than right. (The video was retitled “1,368 AI Failures and What They Teach About Evals”.)

**My takeaways**

- One framework with 13 dimensions, used as a rubric.

### Proving Alpha: Showcasing AI's Value in Fundamental Investing

**Speaker:** Hari Kumar (Head of AI and Data Science, Contour Asset Management)  
**When / where:** 1:40pm–1:58pm · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=ae273f0b-db0f-4638-8ae3-05874bebf9ca) · [▶ Video](https://www.youtube.com/watch?v=ZMZlAn6Pyfg)

**What it covers.** Contour Asset Management, a $6B fundamental long/short fund, tackles what AI spend actually returns. It runs live A/B tests of analysts’ research with and without AI tools, tracks “cost per useful insight”, resurfaces old ideas when the market makes them interesting again, and extends the approach to trading, operations and compliance. (The video was retitled “Proving Alpha: How a Hedge Fund Measures AI’s ROI”.)

**My takeaways**

- The challenge: returns have many factors, so ROI is hard to see. The question to ask is “what did you consider that you couldn’t have before?”: novel ideas, differentiated sources, and ideas acted upon.
- Measure coverage and time per name, before vs after AI; run live A/B comparisons (e.g. a name in three months that is in no AI workflow).
- Agents can track notes; expert calls and credit-card panel data are example inputs.

### Can AI Understand a Distressed Credit?

**Speaker:** Harneet Kaur (Founder, Axim Intelligence)  
**When / where:** 2:05pm–2:23pm · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=adca6e26-1ddc-4f77-8e35-59bf1d1de81d) · [▶ Video](https://www.youtube.com/watch?v=WOszaC1WWbQ)

**What it covers.** Tests whether frontier models can reason about a live distressed situation (iHeartMedia). Two top models disagreed sharply on bankruptcy vs a negotiated restructuring; breaking the analysis into checked layers (extraction, derivation, judgment, prediction) brought them to the same answer, which raises the question of whether agreement is the same as being right.

**My takeaways**

- There is no ground truth for predicting stress or distress, so verification is the open problem.

### Why Frontier Models Fail on Billion Dollar Portfolios

**Speakers:** Chanyeol Choi (Co-founder & CEO, LinqAlpha); Janie Gu (Group Product Manager (Kaggle Benchmarks), Google DeepMind); Jack Haverty (Head of Product, LinqAlpha); Suyeol Yun (Head of AI, LinqAlpha)  
**When / where:** 2:30pm–2:48pm · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=e8e53365-3c1f-4c28-bdd0-44c305a05d57) · [▶ Video](https://www.youtube.com/watch?v=aVbRSIQO2dE)

**What it covers.** Moving one model to the right harness improved its score by more than 60%, as much as switching models. Kaggle (Google DeepMind) found most public benchmarks broken after reproducing 65 of them; LinqAlpha presents MandateBench, which grades whether a model can work like a buy-side analyst, from a fund mandate to a full investment memo. (The video was retitled “Why Frontier Models Fail as Buy-Side Analysts”.)

**My takeaways**

- A good template for how to build and present a domain benchmark.

### AI and the Future of Fundamental Investing

**Speakers:** Jack Woodruff (Senior Advisor, Kepler); Vinoo Ganesh (CEO, Kepler)  
**When / where:** 2:55pm–3:13pm · Simons Room (Metropolitan East) · Stage Talk (18 mins)  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=a244839c-239c-4789-b621-32a3568a218e) · [▶ Livestream @ 5:29:04](https://www.youtube.com/watch?v=akEqiXFo3GM&t=19744s) · [Related talk by Vinoo Ganesh: “Do Not Let Models Invent Numbers”](https://www.youtube.com/watch?v=dZgA6btnb_s) · [Speaker Q&A (DEV)](https://dev.to/dailycontext/keplers-vinoo-ganesh-on-building-ai-you-can-trust-3p0f)

**What it covers.** Fireside chat between Jack Woodruff (Kepler; formerly one of the largest books at Citadel) and Vinoo Ganesh (CEO, Kepler) on what AI is changing in fundamental investing, where it saves real time, and what an investor would never hand over to a model.

### Your agent fix just broke something else

**Speaker:** Sofia Jakovcevic (AI Forward Deployed Engineer, Arize AI)  
**When / where:** 3:20pm–3:38pm · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=2a589e49-4ee4-4c53-bfeb-40bc3db16506) · [▶ Video](https://www.youtube.com/watch?v=v5tm5EAzsAs)

**What it covers.** A one-line prompt change made a support agent 17 points safer and 17 points worse at finishing tasks. Agents fail at runtime, not in code; the session walks through a five-step regression loop built from production traces (datasets, evaluators, baseline experiments, comparisons, CI gates) and closes with agents that read traces and fix themselves.

**My takeaways**

- Aggregate metrics hide regressions; code changes are local, behavioral changes are not.

### Give the Model a Computer: The One Thing Every AI Agent Framework Agrees On

**Speaker:** Mislav Ivanda  
**When / where:** 3:45pm–4:03pm · Sutton Room (NY Ballroom W) · Expo Session (18 mins)  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=8d424512-c7f0-483c-9094-ca899d7bb1fd)  
**Recording:** not published yet

**What it covers.** A year of building Daytona integrations across LangChain, CrewAI, Google ADK, Mastra, Pydantic AI and the OpenAI Agents SDK showed that frameworks that agree on nothing else converge on one primitive: give the model a computer (a sandbox).

**My takeaways**

- A slide comparing frameworks by who controls the flow (AI-decided vs human-orchestrated) was particularly insightful.

### A Hedge Fund as a Function

**Speaker:** Frank Long (Head of AI, Coatue)  
**When / where:** 4:30pm–4:48pm · Simons Room (Metropolitan East) · Featured Keynote  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=9a9dfcd0-459f-43eb-b3f8-004864b4ad2a) · [▶ Livestream @ 6:47:46](https://www.youtube.com/watch?v=akEqiXFo3GM&t=24466s)

**What it covers.** Coatue’s Head of AI on frameworks for thinking about applying AI to investing.

### Frontier Models for Acting in Space and Time

**Speaker:** Pim de Witte (Co-Founder & CEO, General Intuition)  
**When / where:** 4:48pm–5:06pm · Simons Room (Metropolitan East) · Featured Keynote  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=cada670e-c5ea-4977-b3c2-9a68dda26abb) · [▶ Livestream @ 7:02:19](https://www.youtube.com/watch?v=akEqiXFo3GM&t=25339s)

**What it covers.** Pim de Witte introduces General Intuition, a frontier lab building models that act in space and time (raised $650M+ from Khosla and General Catalyst).

### Hermes gives you wings: Build on open foundations.

**Speaker:** Dillon Rolnick (CEO, Nous Research)  
**When / where:** 5:06pm–5:24pm · Simons Room (Metropolitan East) · Featured Keynote  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=871cd5c8-13f4-4fe9-ac49-c8b25d970add) · [▶ Livestream @ 7:21:20](https://www.youtube.com/watch?v=akEqiXFo3GM&t=26480s)

**What it covers.** Nous Research’s CEO makes the case that modern businesses require open agents, built on open foundations (Hermes).

## Wednesday, October 14

### The Hidden Costs of a Faster Model

**Speaker:** Morgan Suo (Head of US Business Development, MiniMax)  
**When / where:** 10:40am–10:58am · McCarthy Room (Riverside Suite) · Breakout Session (18 mins)  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=f3445fe7-308b-4e84-8490-fd78a13db4fb) · [▶ Video](https://www.youtube.com/watch?v=YvQ4BVTWfHI)

**What it covers.** A faster model only helps if the finished job gets faster. Uses one research task to test the changes inside MiniMax’s M3.1 Flash (sparse attention, 4-bit KV cache, low-precision experts, speculative decoding, reasoning budgets), each of which saves compute and can quietly cost accuracy or wait time. (The video was retitled “Is a Faster Model Actually Faster? Test the Whole Job”.)

**My takeaways**

- Verify outputs before trusting a faster model.

### Building Better Agents Through End-to-End Evals

**Speaker:** Ryan Stevens (Director of Applied Science, Ramp)  
**When / where:** 11:05am–11:23am · Shannon Room (NY Ballroom E) · Breakout Session (18 mins)  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=4607dbbe-8f94-4b0e-91ce-257d1cfb5448)  
**Recording:** not published yet

**What it covers.** Ramp built a benchmark of real accounting workflows to test frontier models, tools, skills, memory and the harness end to end. Some intuitive improvements made the system worse; skill ablation identified context that was hurting performance.

**My takeaways**

- Measurement starts from the agent’s goal. The example is closing the books: reconciliation against the ERP. A benchmark needs clean data for simulation.
- Labels → benchmarks → evals → verifiable outputs is essentially backtesting a production agent.
- Pass@k (pass@1, 2, 3) is a useful way to report reliability.
- In accounting, finding the information or files is not hard; reasoning errors are the failure mode.
- GEPA (auto-tuning skills from failures) is worth a deeper look.

### We Let Our Financial Planners Write the Agents

**Speaker:** Matt Ragone (Financial Planner, Range)  
**When / where:** 11:30am–11:48am · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=9b002687-09cf-42e2-9282-532ed9d0c184) · [▶ Video](https://www.youtube.com/watch?v=gpmze-6ueGU)

**What it covers.** Range’s financial planners write and maintain the agent skills they use every day, without writing code: 24 skills company-wide after six months, 17 of them in planning. The planner reviews every recommendation before it reaches a client.

**My takeaways**

- AI adoption does not need an engineering background; examples were cash-flow and insurance analyses.

### The Model Ate the Coding Harness. Now What?

**Speaker:** Jediah Katz (Engineer, Agent Harness, SpaceX Super Intelligence)  
**When / where:** 11:55am–12:13pm · Shannon Room (NY Ballroom E) · Breakout Session (18 mins) · ⭐ starred  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=29083884-87dc-44f8-b412-b494afca8856) · [▶ Video](https://www.youtube.com/watch?v=7Y4sbZgBdnc)

**What it covers.** Jediah Katz (SpaceXAI; previously Cursor’s agent-harness engineer) on what smarter models mean for coding agents, Grok Bot and the future of harnesses. Fancy harnesses no longer add coding intelligence because the data flywheel teaches models the skills they used to patch in; the harness matters more for what models can’t absorb: infrastructure, trust and security, efficiency. (The video was retitled “Is the Agent Harness Dead?”.)

**My takeaways**

- The harness kicks off the data flywheel and unlocks the software factory.
- Grok Bot examples: key website flows, communication, acting vs asking, learning over time.
- Permissions and API fees remain harness concerns.

### A/B Testing AI Products Is Weird: What Changes When the Product Is Probabilistic

**Speaker:** Supriya Subudhi (Staff Product Manager, Bumble)  
**When / where:** 12:20pm–12:38pm · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=11fef189-97e8-4c8e-87e8-dac43d800846) · [Speaker article (DEV)](https://dev.to/dailycontext/ab-testing-ai-products-is-weird-576h)  
**Recording:** not published yet

**What it covers.** Traditional experimentation assumes you can define a feature, measure a response and compare outcomes. AI outputs vary, user responses depend on context, and a metric win does not say why the experience improved. Draws on experimentation in consumer tech and fintech, including an AI-powered experience at Bumble.

**My takeaways**

- A successful A/B test is where the journey begins. Five traps: (1) users asked for it, then ignored it; (2) the average win hides the tail; (3) green dashboard, red bill; (4) swap the model and the result flips; (5) AI regulation keeps changing.
- Token-based pricing is the root of the “green dashboard, red bill” tension; time-based billing with enough throughput might fit better.

### Building a self-improving software factory

**Speaker:** Zach Lloyd (Founder & CEO, Warp)  
**When / where:** 1:40pm–1:58pm · McCarthy Room (Riverside Suite) · Expo Session (18 mins) · ⭐ starred  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=a901d5ea-57c6-433f-9c37-5fe6531adb99)  
**Recording:** not published yet

**What it covers.** Warp argues interactive agents are not scaling (runaway cost, no governance) and that teams need software factories: closed-loop systems for automating development, with shared infrastructure rather than every company rolling its own.

**My takeaways**

- Local agents move to sandboxes so they can be governed; the loop starts from a product idea.
- Factory infrastructure: data, compute and an inference gateway (any model, any harness).
- Measurement over vibes: benchmarks and a score to drive self-improvement. Open questions: data curation, how to define correctness, LLM-as-judge.

### Scaling Agentic RL Across the Globe

**Speaker:** Jason Mancuso (Member of Technical Staff, Research, Modal)  
**When / where:** 2:05pm–2:23pm · Shannon Room (NY Ballroom E) · Expo Session (18 mins) · ⭐ starred  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=e6ccda8d-0613-44f8-b98f-53ba8037b518) · [Stitch (open-source RL framework)](https://github.com/modal-projects/stitch)  
**Recording:** not published yet

**What it covers.** Modal supports customers scaling agent RL across a multi-cloud, multi-region fleet and presents Stitch, an open-source framework for disaggregating RL training.

**My takeaways**

- Why fine-tune: quality of service, proprietary data formats, and competing on task intelligence per dollar.
- RL generalizes better than SFT. With an agent harness, evals and production traces, you can iterate.
- Recipe: GRPO plus masked importance sampling plus score centering to limit drift.

### Your agent needs a skill to improve its skills

**Speaker:** Ben Holmes (DevEx Lead, Warp)  
**When / where:** 2:30pm–2:48pm · Turing Room (Riverside Ballroom) · Breakouts · ⭐ starred  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=0a0cbd67-23fa-4a3d-a8dc-06e3caa3ab2f)  
**Recording:** not published yet

**What it covers.** Warp found its orchestration agent wasting hundreds of tool calls in a month because of one bad instruction in a message-passing skill. Shows how to build a feedback loop that evaluates past agent conversations and finds failures in the skills themselves.

### Build your own team of managed agents

**Speaker:** Jack Wotherspoon (Member of Technical Staff, Google DeepMind)  
**When / where:** 2:55pm–3:13pm · Shannon Room (NY Ballroom E) · Expo Session (18 mins)  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=970f672e-f351-46bb-9790-78c6b1baf81d) · [▶ Video](https://www.youtube.com/watch?v=Y_p3y6zP5QE)

**What it covers.** Gemini Managed Agents run coding agents in cloud sandboxes from a single API call: the Interactions API, the Antigravity harness, environments shared across agents, uploaded skills and AGENTS.md files, scheduled triggers and multi-agent workflows.

**My takeaways**

- Multiple agents can point at the same sandbox.
- An egress proxy keeps secrets out of the sandbox.

### A Roadmap for Evaluating Highly-Subjective Tasks

**Speaker:** Cameron Wolfe (Staff Research Scientist, Netflix)  
**When / where:** 3:20pm–3:38pm · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=f5c310e9-b0bb-4fa6-bc0d-bda758e1c464) · [Slides](https://docs.google.com/presentation/d/146TscsJ21LU4kATVS3ES2oC_oZxoCBsTH3soepY25P4/edit?slide=id.g3ec03477a51_0_0#slide=id.g3ec03477a51_0_0)  
**Recording:** not published yet

**What it covers.** Roadmap for building large-scale automatic evals for subjective tasks: coordinating human annotation, building and evaluating LLM judges, using production monitoring to inform evals, and reducing uncertainty with statistics.

**My takeaways**

- Start with human evaluation: a small set of quality criteria, a clear way to rate them, and specific examples. Offline evaluation, human evaluation and production monitoring work together.

### Code Mode: your agent should write code

**Speaker:** Mikyo King (Head of Open Source, Arize AI)  
**When / where:** 3:45pm–4:03pm · Turing Room (Riverside Ballroom) · Breakouts  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=8413d6ee-ceb1-4095-9f27-49d518f1dd5a) · [▶ Video](https://www.youtube.com/watch?v=dAoOYFx7ZYw)

**What it covers.** Give an agent 40 tools and it gets worse at using any of them; give it a sandbox and it can compose what it needs. Covers how Arize built code mode into Phoenix (Bash, a Python sandbox, SQL, even UI control), its hidden costs (secrets, permissions, durability, double sandboxing), and Harbor evals showing 43% fewer tokens and 31% lower cost. (The video was retitled “Stop Giving Your Agent Tools. Give It a Sandbox”.)

**My takeaways**

- Tool calling vs code mode: (1) tool search, (2) composability (loops, branches, chaining; cf. Claude agent teams), (3) token efficiency, (4) emergent behavior.
- Fewer tool calls makes sense, though in practice it resembles bundling functionality.

### AI in Practice: An Inside Look at Apollo's Approach

**Speakers:** Michael Manapat (Co-Founder, Rowspace); Jack McDonald (Principal at Apollo Global Management, COO of Redding Ridge, Apollo Global Management)  
**When / where:** 4:30pm–4:48pm · Simons Room (Metropolitan East) · Featured Keynote  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=44e9d04a-e8b4-418c-8a85-02fbb9263dd0) · [▶ Video](https://www.youtube.com/watch?v=BOWYujhIQuo) · [▶ Livestream @ 7:50:03](https://www.youtube.com/watch?v=jiUC6IUMdvc&t=28203s)

**What it covers.** Apollo’s CLO business used to spend 2–3 days building one mock portfolio by hand in Excel; with Rowspace it takes minutes. A customer co-presentation on applying AI to fragmented credit data. (The video was retitled “AI Agents Cut 3-Day Portfolio Builds to Minutes”.)

**My takeaways**

- Case 1: portfolio construction for CLOs, tracking where money flows. Days of manual Excel became minutes, in real time.
- Case 2: a warehouse purchase workflow that was manual and ended in a written recap.
- The vendor’s role resembles a forward-deployed engineer: understanding the workflow on top of fragmented data.

### Process Supervision for Agents That Do Long-Horizon Work

**Speaker:** Mitchell Troyanovsky (Co-Founder, Basis)  
**When / where:** 4:48pm–5:06pm · Simons Room (Metropolitan East) · Featured Keynote · ⭐ starred  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=1cbc625f-7f07-49bb-9c5c-57c95f07ba77) · [▶ Livestream @ 8:09:22](https://www.youtube.com/watch?v=jiUC6IUMdvc&t=29362s) · [Behavior specs](https://www.agentbehavior.dev/)

**What it covers.** Basis builds agents for accountants. A long-horizon agent can reach a correct result through unreliable steps, such as a tax conclusion based on pretraining or a blog rather than primary sources. Evaluating long-horizon work needs evidence about both the result and the decisions behind it.

**My takeaways**

- Outcome evals are not enough: two runs can end the same way while the process differs. Developing opinions is not only about accuracy but also things like time.
- Process supervision: judge the steps, not just the answer. The “runtime trading data” analogy fits, since an agent’s context changes in real time.
- Basis’s behavior specs capture an agreed opinion from the builders (agentbehavior.dev).

### Everything will be Simulated

**Speaker:** Cristóbal Valenzuela (Co-Founder and Co-CEO, Runway)  
**When / where:** 5:06pm–5:24pm · Simons Room (Metropolitan East) · Featured Keynote  
**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=af8422e8-d5f8-4dbd-a918-6360f5bfba80) · [▶ Livestream @ 8:29:11](https://www.youtube.com/watch?v=jiUC6IUMdvc&t=30551s)

**What it covers.** Runway’s co-founder on world models mature enough to simulate real environments in real time, and what that opens up for robotics, digital interfaces and science.

**My takeaways**

- The “world interface model” idea is interesting: it can show, for example, the inside of a car directly, with no code required.
