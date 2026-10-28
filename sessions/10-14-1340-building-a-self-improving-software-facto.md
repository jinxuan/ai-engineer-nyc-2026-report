# Coding Agents Belong in a Cloud Factory That Scores and Improves Itself

*At AI Engineer NYC 2026, Warp's Zach Lloyd argued that companies should move coding agents off engineers' laptops into a "cloud software factory" defined in code, measured on their own data and improved by agents, then showed Warp's version of one.*

**Expo Session · Wednesday, October 14, 2026 · 1:40–1:58pm · McCarthy Room (Riverside Suite)**
**Speaker:** Zach Lloyd, Founder & CEO, Warp

---

Most engineers today work with coding agents interactively, Zach Lloyd said: on their laptops, perhaps with several running at once, "interactively prompting agents to build features and fix bugs." Where most companies Lloyd speaks with want to get to "is something that is more like automated development": deploying agents "to get the most possible business value at the lowest possible cost in a way that is governed and secure."

The session's abstract set out four building blocks for getting there: cloud infrastructure that centralizes agent conversations, skills and security permissions; configuration as code; metrics "beyond 'number of PRs shipped'"; and self-improvement loops "for agents to review past conversations, score the quality, and propose improvements to the skills driving the factory." Lloyd spent about six and a half minutes on slides making that case, then switched to a live demo of Warp Factories, Warp's product for it.

## 1. From interactive to automated development

![The evolution of agentic development](../slides/10-14-1340-building-a-self-improving-software-facto/01.jpg)
*"The evolution of agentic development": chat, autocomplete, interactive prompting (today), automated development (next).*

The first photographed slide drew a rising curve through four stages: chat, autocomplete, interactive prompting, marked "today," and automated development, marked "next." Interactive agents, Lloyd said, run into limits: "you're bottlenecked by a person running" them, and "it's hard to control costs if everyone has their own setup," and hard to govern.

![More AI usage != more business value](../slides/10-14-1340-building-a-self-improving-software-facto/02.jpg)
*Three problems with today's agent usage, beside three news headlines.*

The next slide, "More AI usage != more business value," listed three problems: ad hoc interactive usage with no automation; no systematic way to optimize cost and quality; and no central layer for governance and control. Beside them were three headlines:

| Outlet (as shown) | Headline |
| --- | --- |
| Forbes | "Is The Cult Of 'Tokenmaxxing' Just Another Fad Or The New Normal?" |
| VB | "The AI governance mirage: Why 72% of enterprises don't have the control and security they think they do" |
| Forbes | "Uber Burns Its 2026 AI Budget In Four Months On Claude Code" |

The Uber story was widely reported: Uber's CTO, Praveen Neppalli Naga, told The Information that the company's 2026 AI coding budget had been used up in four months, and Fortune later reported the COO questioning the return on that spending. The Forbes article itself and the VB article's 72% figure were not checked for this write-up.

## 2. Local agents are ungovernable

![Local agents are ungovernable agents](../slides/10-14-1340-building-a-self-improving-software-facto/03.jpg)
*"Local agents are ungovernable agents."*

Lloyd named two problems with agents that run on a laptop. The first is security: if a local agent is compromised, "it basically has the ability to take over whatever the user who signed in on their laptop is, so that's not a good thing." The second is governance: "everyone is configuring their own MCPs, their own skills, their own rules, and if you can't see what's going on, you can't monitor it."

"The solution to both these problems is to sandbox your agents and move them to the cloud," Lloyd said. The product category emerging to support that is what Warp calls a cloud software factory, a term Lloyd acknowledged "is kind of overloaded."

## 3. The factory loop

![A cloud software factory](../slides/10-14-1340-building-a-self-improving-software-facto/04.jpg)
*The factory as a loop, with three points where humans review.*

The slide drew the factory as a cycle: a new product idea or bug report, then triage, spec, implement, verify, ship and monitor, and back to the start. Three human checkpoints sat on the loop: reviewing the spec, reviewing the code and reviewing the product.

In Lloyd's description, "every company is going to set up a loop in the cloud where a combination of humans and agents do work." Humans put work in; agents handle parts of the software life cycle. A triage agent works with a human on a spec, agents do the implementation, and they do "computer use verification." A business will want to "define all the rules for how humans and agents interact, and then get to a spot where you can really, really measure and improve over time," Lloyd said, because "getting this concept right is going to be the key driver to how your organization ships software."

## 4. Infrastructure that is open, adaptable and defined in code

![Open, adaptable, and defined in code](../slides/10-14-1340-building-a-self-improving-software-facto/05.jpg)
*"Software factories should be built on an infrastructure stack that is open, adaptable, and defined in code."*

"At Warp, our belief is that you should think of this as infrastructure," Lloyd said: not a single product, but "an infrastructure stack that you can build on top of and scale." The slide named three properties, and Lloyd explained each.

Open means the factory supports different models, "whether it's open-weight or frontier," and different harnesses, because "the best models and harnesses are constantly changing." Adaptable means "the factory should work with your existing infrastructure." Defined in code means the factory has a strict definition covering "what are the workflows, what are the skills, what are the MCPs, where do these run." Once that definition is in version control, "you get the ability to test different versions of your factory. You get the ability for coding agents to update your factory."

Lloyd's September blog post "The Factory Stack" uses a slightly different second word: "open, composable and defined in code," where composable means a company can adopt part or all of the stack. The post calls the code-defined part "Terraform for factories." The slide and Lloyd on stage both said adaptable.

## 5. AI sovereignty

![Your data, your compute, your inference gateway](../slides/10-14-1340-building-a-self-improving-software-facto/07.jpg)
*Four kinds of ownership, revealed one line at a time (the first build showed only "Your data").*

"Another thing that I'm hearing from businesses is AI sovereignty matters," Lloyd said. The slide built up four lines: your data, your compute, your inference gateway, and any model, any harness. A company should own "all the data that goes in and all the data that comes out," run agents on its own infrastructure and compute ("This is what most larger enterprises that we work with want"), provide its own inference, and support any model and harness to "really preserve optionality." Software factories, Lloyd said, are "very key to your business," and companies should take ownership of their core ingredients. (The phrase "AI sovereignty" does not appear in "The Factory Stack," which makes similar points under its data and inference layers, including that no external provider should train on factory data.)

## 6. Measurement over vibes

![Measurement > Vibes](../slides/10-14-1340-building-a-self-improving-software-facto/08.jpg)
*"Measurement > Vibes": scoring, self-improvement and benchmarking.*

On any given day on Twitter, Lloyd said, someone claims one model or another is best, and there are public benchmarks such as SWE-bench and Terminal-Bench. But the goal as development becomes more automated is "being measurement first on your own data." The slide listed three capabilities a factory should have. Scoring means being able to "eval how your coding agents are doing." Self-improvement means feeding those scores to agents that can improve the factory. Benchmarking means building your own benchmarks: "take all of your real coding agent tasks and try them against different models and measure how it goes."

## 7. Two kinds of engineers

![Two types of engineers will use your factory](../slides/10-14-1340-building-a-self-improving-software-facto/09.jpg)
*Product engineers and factory engineers.*

The last idea slide split engineers into two roles. Product engineers still build features and fix bugs for customers, but they do it "through the factory," putting work into it. The second kind Lloyd called factory engineering: "either your platform team or just your general engineers will have to get good at" making the factory better, "improving the skills, setting the context, changing the model mix." (Lloyd's September talk at another AI Engineer event was titled "Software Engineering Is Becoming Factory Engineering.")

## 8. The demo: Warp Factories

![Let's look at a demo](../slides/10-14-1340-building-a-self-improving-software-facto/10.jpg)
*The switch to the demo.*

"I'm going to show what some of this looks like in practice," Lloyd said, "in the context of our own product, Warp Factories." The author's room recording ends at that point, so Lloyd's narration of the demo was not recorded. What follows is reconstructed from photos of the screen, with Warp's own blog posts used to explain what the features are. How Lloyd interpreted the numbers on screen is not known.

### The agents

![The Agents page of the wilson factory](../slides/10-14-1340-building-a-self-improving-software-facto/11.jpg)
*The Agents page for a factory named wilson, on Warp's staging environment.*

The demo ran on Warp's staging site in a factory named `wilson`. A banner on the Agents page read: "Configuration is read-only here. Make changes in `warpdotdev/factory-dev` at `main:v1`," meaning the factory's definition lives in a GitHub repository. Five agents were listed, matching the stages of the loop from section 3:

| Agent | Description on screen |
| --- | --- |
| wilson Foreman | "Orchestrates the factory workflow and dispatches each gated step." |
| code-review | "Reviews factory pull requests and routes findings to rework or human resolution." |
| implementation | "Implements, validates, and updates code changes." |
| spec | partly hidden ("Writes and …") |
| triage | "Triages requests and establishes task state." |

The sidebar showed the product's structure: New Chat, Inbox, Runs, MCPs and apps, Secrets, Integrations and Users at the top level, and under the factory a Dashboard, Agents, Automations, Scorers, Self-improvement (with a count of 31), Benchmarks, Factory definition and Settings.

![The foreman agent's detail page](../slides/10-14-1340-building-a-self-improving-software-facto/12.jpg)
*The foreman agent, "Managed in GitHub."*

The foreman's detail page was marked "Managed in GitHub," pointing at the same repository and branch, with tabs for Prompt, Automations and Settings. Its agent resources included Slack (Warp managed), Figma, Sentry, a staging debugger, and a secret named `WILSON_SENTRY_AUTH_TOKEN`.

### A scorer

![The Redundant Tests scorer](../slides/10-14-1340-building-a-self-improving-software-facto/13.jpg)
*A scorer that judges whether the implementation agent wrote low-value tests.*

The next screen showed a scorer called "Redundant Tests," described as judging "whether the implementation agent's final delivered test diff includes redundant, low-signal, or trivial tests — judging the test artifact, not whether a human later asked to delete it." The page showed an all-time scorer cost of $526.24 and a 30-day pass-rate trend. One scored run was classified "No surplus tests," with a run time of 3 minutes 22 seconds and a cost of $0.26.

Warp's post "The missing feedback loop for software factories" (Varoon Kodithala, August 2026) explains how scorers work. Like the rest of the factory, each scorer is defined in a file that names the agent it evaluates, labels scored from 0 to 1, a pass threshold (for example 0.8) and a sample rate, 25% by default. After a run ends, a cloud agent grades the conversation against the rubric and returns pass or fail with a reason. Built-in scorers cover code quality, efficiency, task and procedure compliance, and verbosity. The post's example of an efficiency failure was an agent that used 275 computer-use actions to change one line of CSS.

### Self-improvement

![The Self-improvement page](../slides/10-14-1340-building-a-self-improving-software-facto/14.jpg)
*Proposed changes to the factory, with an approval rate of 13 out of 51.*

The Self-improvement page showed an approval rate of 13 of 51, or 25%, with changes "grouped by PR or review opened time," and an inbox of proposals awaiting review. Their titles were changes to the agents' skills and instructions:

| Proposal (as shown) |
| --- |
| Keep Go module files tidy after dependency changes ("A Go dependency update left obsolete go.sum entries and required a review-driven cleanup after CI failed…") |
| Report only findings and unexamined dimensions in reviews |
| Handle CI failure follow-ups without a plan-only skill |
| Make design snapshot lookup and pre-write locate step satisfiable |
| Two more, partly hidden ("Trust the du…ility judgment", "Read mat…menting") |

According to the same Warp post, an automation runs "every few hours," collects new scorer failures and hands them in batches to a self-improvement agent. That agent looks for root causes in the transcripts and proposes diffs to skills and configuration, as a review branch for Warp-managed factories or a pull request for GitHub-managed ones, with an instruction to "make redrafts, not additions." The post said Warp's internal factory shipped "over 300 PRs" in a week and merged more than a dozen self-improvement PRs within a few days. Its summary: "Every nudge you give your agents, every review you leave, should flow into concrete improvements to your software factory."

### Benchmarks

![The Benchmarks list](../slides/10-14-1340-building-a-self-improving-software-facto/15.jpg)
*Benchmarks defined for the factory.*

![The Benchmarks list, photographed at an angle](../slides/10-14-1340-building-a-self-improving-software-facto/16.jpg)
*The same list, showing more entries.*

The Benchmarks page listed task sets built from the factory's own work: default-model-selection benchmarks for the review, spec, triage and foreman agents; warp-bench-smoke-test, warp-bench-1.0 and warp-bench-1.0-mini; WarpBench 0.1 and 0.2; a benchmark named after a ticket (REV-2396, about team settings); a UI computer-use implementation set; and a "Mini WarpBench."

![WarpBench 0.1 results](../slides/10-14-1340-building-a-self-improving-software-facto/17.jpg)
*Correctness against average cost per trial for several models on WarpBench 0.1.*

One run of WarpBench 0.1, dated September 2 at 10:45 PM, plotted correctness against average cost per trial. Two values were labeled on screen; the others below are read off the chart and are approximate:

| Model | Correctness | Average cost per trial |
| --- | --- | --- |
| gpt-5.6-sol (high) | 94.8% (labeled) | $6.23 (labeled) |
| glm 5.3 flash | about 93.5% (read from chart) | $1.99, labeled "lowest cost option" |
| claude opus 5 (high) | about 92.5% (read from chart) | above $10 (read from chart) |
| gemini 3.7 flash | about 90.5% (read from chart) | not legible |

A warning at the bottom said some results were unscored and suggested using Rescore to retry them; it showed 28 of 29 tasks scored. The page is an instance of the "create your own benchmarks" idea from section 6: Warp's own tasks, run against several models, with cost and correctness side by side.

### The stack

![The Factories Stack](../slides/10-14-1340-building-a-self-improving-software-facto/18.jpg)
*Seven layers of the software factory stack, from factories-as-code up to access.*

Lloyd then showed a diagram of the full stack (a Figma page titled "The Software Factory Stack"), the same seven layers described in "The Factory Stack" post:

| Layer | Components on the slide |
| --- | --- |
| 06 Access | Factory MCP (any coding agent) · comms platform (Slack, Teams) · task tracker (Jira, Linear) · code forge (GitHub, GitLab) · any device (web, mobile) · programmatic (API, SDK) |
| 05 Orchestration | Native orchestration (triage, spec, implement, review, verify) · multi-agent (Warp, Claude Code, Codex) · human in the loop (live steering and handoff) · automations (trigger, cron) |
| 04 Improvement | Metrics (autonomy, cost, cycle time) · scorers (evaluate completed runs) · benchmarks (compare models or skills per task set) · self-improvement (automatic observer loop) |
| 03 Inference | Warp-hosted inference (multi-model, frontier and open-weight) · bring your own inference (Bedrock, Vertex) · custom model routing (LLM routing rules) |
| 02 Compute | Sandbox (Docker image, Kubernetes, any remote environment) · agent runner (Mac, Linux, Windows; configurable disk, RAM, CPU) · a third item cut off |
| 01 Data & context | Runs and transcripts (agent traces) · agent memory (vector store) · agent artifacts (PRs, reports, attachments) · MCPs and internal knowledge · access/RBAC (cut off) |
| 00 Factories-as-code | factory.yaml · agent definitions · automation definitions · launch config |

In the blog's version, the bottom layer defines agent configuration, runners, repositories, automations, integrations and scorers "all defined in a factory.yaml file," and the improvement layer adds cost per PR, replay and self-improvement agents.

![Try Warp Factories today](../slides/10-14-1340-building-a-self-improving-software-facto/19.jpg)
*The closing slide.*

The talk ended on a slide reading "Try Warp Factories today" with a QR code.

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=a901d5ea-57c6-433f-9c37-5fe6531adb99) · [The Factory Stack](https://www.warp.dev/blog/the-factory-stack) (Zach Lloyd) · [The missing feedback loop for software factories](https://www.warp.dev/blog/engineering-self-improving-software-factories) (Varoon Kodithala) · [Software Engineering Is Becoming Factory Engineering](https://www.youtube.com/watch?v=tUPPVhBBcoM) (Lloyd's September talk, background only) · [Fortune on Uber's AI spending](https://www.fortune.com/2026/05/26/uber-coo-ai-spending-tokens-claude-code/)

No video of this session had been published at the time of writing. This write-up is based on the author's room recording, which covers only the first six and a half minutes (the slides, up to the start of the demo), slide and screen photos, and the Warp blog posts above. The demo and anything after it, including whether there was a Q&A, were not recorded. The [precise notes](precise/10-14-1340-building-a-self-improving-software-facto.md) (Chinese) keep timestamps and the full list of unverified details.
