**English** | [中文](zh/10-13-0918-anthropic-accelerating-frontier-ai-in-fi.md)

# Anthropic's Pitch to Finance: Build the Agent Foundation Once

*At AI Engineer NYC 2026, Anthropic's Belinda Neal and Keon Shahab told financial firms that the model is no longer the constraint on agents; building every agent by hand is. Their slides laid out a five-layer "agent foundation" and used Claude for Financial Advisors as the industry-wide example.*

**Featured Keynote (Main Stage) · Tuesday, October 13, 2026 · 9:18–9:36am · Simons Room (Metropolitan East)**
**Speakers:** Belinda Neal, Managing Director, Financial Services, Anthropic; Keon Shahab, Applied AI Architect, Anthropic

---

**A note on sources.** No recording of this talk is available. It was shown on the Day 1 livestream (the author's photos are of the stream, with its viewer counter), but the published livestream recording skips it: the host introduces the two Anthropic speakers at 6:53, and the next talk shown is Bridgewater's. No standalone video had been published at the time of writing. This write-up is therefore built only from the slide photos and the official session description. It reports what the slides say, not what the speakers said, and it cannot say whether there was Q&A.

![Who you are hearing from](../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/01.jpg)
*The speakers' slide: Neal spent 19-plus years at Goldman Sachs; Shahab spent five years at Databricks.*

The opening slide introduced the speakers. Belinda Neal, Anthropic's Managing Director for Financial Services, previously spent more than 19 years at Goldman Sachs across engineering, global markets and equity research. Keon Shahab, an Applied AI Architect, was previously a lead solutions architect for financial services at Databricks, for five years. Judging by the speaker inset in the photos, Neal presented the opening and closing sections and Shahab presented the five layers.

The session description framed the argument: frontier AI "is moving faster than financial services is adopting it, and that gap is the opportunity." Every firm, it said, has two frontiers to reach: "the frontier of intelligence, what the best model can do today, and the frontier of adoption, how much of the firm's real work runs on it."

## 1. Three frontiers

![Taking the frontier to the industry](../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/02.jpg)
*The talk's structure: intelligence, adoption, and the same foundation applied across an industry.*

| # | Frontier | Heading | What the slide says |
| --- | --- | --- | --- |
| 1 | The frontier of intelligence | Applying intelligence | What the best model can do today. An alpha opportunity. |
| 2 | The frontier of adoption | Scaling agents | Building the agent foundation and the operating model. A scale opportunity. |
| 3 | Applied to financial services | Compounding impact | The same foundation, built once for all of wealth. An industry opportunity. |

The slide described the talk as "how to take on the frontier opportunity across these three."

## 2. The frontier of intelligence

![Frontier AI changes what's possible](../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/03.jpg)
*Illustrative curves for coding, cyber and finance, each crossing a capability threshold at a different time.*

A chart of three illustrative, time-shifted curves showed coding, cyber and finance each rising from early use toward "autonomous fleets of agents." Per the footnote, coding crossed the threshold with Claude Opus 4.5 (marked Nov '25) and cyber with Claude Mythos Preview (marked Apr '26; Anthropic announced that model with Project Glasswing in April 2026 and has not released it generally). Finance was marked "early agents" today. A quote attributed to Atte Lahtiranta of Citadel read: "Our investment professionals live in data and analytical models. Claude for Excel meets them there and gives them a step-change in how fast they can build, test, and trust their work." The footnote sourced it to Anthropic's financial services keynote, "The Briefing." (Press coverage of Anthropic's May 2026 financial-services launch quotes Lahtiranta with different wording; the slide's version could not be traced to its source.)

![The Claude model family](../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/04.jpg)
*Four models from "faster and lower cost" to "more capable," each with a customer quote.*

The model-family slide matched each current Claude model to finance work and a customer quote:

| Model | Positioning | Use cases on the slide | Customer quote on the slide |
| --- | --- | --- | --- |
| Haiku 5.5 | Fastest and lowest cost | Classification and routing; field extraction, chat | AlphaSense: "Ask in Document is one of our big sources of spend, doing about 8M calls a week in production." |
| Sonnet 5.5 | For everyday work at volume | Analysis and reporting; filings at volume | Balyasny: "Claude Sonnet 5.5 scored ahead of Sonnet 5 and used about 121k tokens per answer, where Sonnet 5 used 497k." |
| Opus 5.5 | The daily driver for complex finance work | Decks and merger models; credit agreement redlines; day-to-day agentic coding | Rogo: "At its lowest effort setting, Claude Opus 5.5 beat Opus 5 at high effort on our BigFinance Bench with about 60% fewer output tokens." |
| Fable 5.1 | For the hardest work | Quant and trading research; the hardest coding problems; long unattended runs | Millennium: "A particular piece of code had an extremely rare crash, about one in a million runs, that nobody on our team had explained in four to five years. … Claude Fable 5.1 was the first to find it." |

![Smarter, cheaper and safer with every release](../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/05.jpg)
*Capability, cost and prompt-injection figures, with a footnote saying they were still to be confirmed.*

A third slide made the case that models are getting "smarter, cheaper and safer with every release":

| Claim | Headline figure | Detail on the slide |
| --- | --- | --- |
| Intelligence is going up | 16+ hrs of work a model can finish on its own | In 2023: minutes |
| Cost per task is going down | 40% less to run: Opus 5.5 against Opus 5 | Haiku 5.5: 75% less; Sonnet 5.5: up to 30% less; Fable 5.1: 25% less |
| And it is getting safer | 1.0% of prompt-injection attacks succeed on Fable 5.1 | Fable 5, the release before: 6.5%; other frontier models: 9% to 53% |

The footnote cited METR task-length measurements for capability, Anthropic's launch pages for cost (each model against the one it replaced), and the Gray Swan indirect prompt injection benchmark (attack success within 15 attempts) for safety. It ended: "Figures to confirm before the talk." These figures could not all be checked. A public benchmark aggregator, compiling vendor-reported results, lists Fable 5.1 at 1.0% on the Gray Swan test. One report put Opus 5.5's token price 20% below Opus 5's; the slide's 40% is cost per task, a different measure.

## 3. The frontier of adoption: from a few agents to many

![Agents work. Now scale them.](../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/06.jpg)
*Four stages, from chatbots in 2023 to "many agents, one foundation."*

The adoption slide traced four stages:

| When | Stage | What the slide says | Example on the slide |
| --- | --- | --- | --- |
| 2023: firms built chatbots | Claude assists | "Ask it things. Marvel a bit. Move on." | A generic answer about travel policies |
| 2024: firms ran pilots | Claude collaborates | "RAG, a knowledge base, a pilot or three." | "Found it in the 2024 travel policy (p. 4): international trips need director approval." |
| 2025 to now: a few agents, built by hand | Claude works | "Agents doing real work." ("We are here") | "Done. I fixed the billing bug across 12 files, added tests, and opened a PR for your review." |
| Next: many agents, one foundation | Claude operates | "Fleets of agents run whole processes." | |

Its conclusion: "The model is no longer the constraint. Building every agent by hand is. Next: how to build the agent foundation."

![The agent foundation: five layers, built once](../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/07.jpg)
*Two layers for what an agent can do, three for what lets you trust it, on top of the model.*

The foundation has five layers on top of the model, each answering one question:

| Group | Layer | Question |
| --- | --- | --- |
| What lets you trust it | 5. Traces and evals | How do you know it still works? |
| | 4. Sandbox and approval | Who signs off before it acts? |
| | 3. Identity and permissions | Who is it acting as? |
| What it can do | 2. Skills | Does it know your business and your processes? |
| | 1. Connectors | Can it reach your systems and data? |
| Base | The model | Can it do the work? "Most teams start here." |

The layer slides that followed were each marked "illustrative example."

### Layer 1: connectors

![Layer 1: Connectors](../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/08.jpg)
*Five agents and five systems joined through one MCP layer.*

The connectors slide showed five agents (trade breaks, KYC review, credit memo, onboarding, surveillance) reaching five systems (orders, market data, payments, documents, CRM) through a single MCP layer. The arithmetic was the argument: wiring 100 agents to 50 systems directly means 5,000 integrations, and "one system changes. Every agent breaks." Through connectors it is 150 (100 agents plus 50 systems), and "one system changes. One connector to fix." The slide's summary: "Wire each system once. Control it in one place. Any agent can use it."

### Layer 2: skills

![Layer 2: Skills](../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/09.jpg)
*A skill file for investigating trade breaks, owned by operations and versioned like code.*

The skills example was a file, `break-investigation/SKILL.md`, describing "how this firm investigates a trade break" in three steps: match both sides of the trade; find the field that disagrees; "propose the fix. Never apply it." It was tagged as owned by operations, reviewed like code, and at version 14. "Any agent that needs the method picks it up," the slide said. "Write the method down once. The model becomes a specialist."

### Layer 3: identity and permissions

![Layer 3: Identity and permissions](../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/10.jpg)
*A trade-break investigator with the ops-analyst role: reads allowed, CRM denied, writes proposal-only.*

The same agent, with identity `trade-break-investigator` and role `ops-analyst`, had scoped permissions:

| Action | Resource | Result |
| --- | --- | --- |
| read | Orders | Allowed |
| read | Market data | Allowed |
| read | CRM | Denied |
| write | Orders | Propose only |

"Every call is checked against the role, and logged." The principle: "No agent is a super-user. Each one is scoped to its job."

### Layer 4: sandbox and approval

![Layer 4: Sandbox and approval](../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/12.jpg)
*Propose, dry run, human approval, apply, enforced by a hook rather than the model.*

The approval flow had four steps: the agent proposes (a booking amendment); it is staged as a dry run, with nothing real moving; a person approves (the operations lead); and the change is applied exactly as approved. Enforcement does not rely on the model: "The skill asks. A hook enforces. Code that runs before every action. The model cannot skip it." The slide's summary: "Nothing real moves until a named person approves."

Layer 5, traces and evals, appeared on the overview and summary slides; no separate slide for it was photographed.

![A strong foundation lets you move fast](../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/13.jpg)
*Layers 3 to 5 answer the first three questions a risk committee asks.*

The section closed on the regulatory angle. "The controls a risk committee asks for are the same ones that let a team ship the next agent without starting over." The first three questions such a committee asks map to layers 3, 4 and 5: who is it acting as, who signs off before it acts, and how do you know it still works. "Regulated does not mean slow. Every new agent inherits all three."

## 4. Applied to an industry: Claude for Financial Advisors

![Claude for Financial Advisors: the orchestrator across the stack](../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/14.jpg)
*Claude at the center of an advisor's tools, with connectors, skills and approval as layers 1, 2 and 4.*

The third frontier applied the same foundation to a whole industry. The slide put Claude at the center of an advisor's systems: CRM, custodian, portfolio and reporting, planning, alternatives, meeting capture, estate and documents, and market data. It said that less than 20% of an advisor's time is spent with clients, "the rest goes to 10+ disconnected tools," citing Kitces Research. (Anthropic's product article cites the same research as about a sixth of a practice's time.) Three of the five layers mapped onto the product: connectors ("partner tools calculate: deterministic numbers from the systems you trust"), skills ("Claude orchestrates: runs each workflow end to end across the tools"), and approval ("the advisor approves: nothing is saved or sent without sign-off"). The product launched on September 14, 2026, at the Future Proof conference. Anthropic's own description matches the approval layer: Claude stages actions such as CRM updates and draft client messages for the advisor to approve, and regulated activities "remain subject to human review and approval."

![Layer 1: the industry connects once](../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/15.jpg)
*Connector partners grouped by category.*

| Category | Logos on the slide |
| --- | --- |
| Custody & investments | BlackRock, Charles Schwab, iCapital, Vanguard |
| Portfolio, planning & reporting | Addepar, Envestnet, Orion, SS&C Black Diamond, wealth.com |
| Data & research | FactSet, Morningstar, S&P Global, Snowflake |
| CRM & client meetings | HubSpot, Redtail, Salesforce, Wealthbox, Zocks |
| Core software | Box, Docusign, Dropbox, Google Workspace, Microsoft, Slack, Zoom |

The photo is blurry, and a few logos were identified by shape. Anthropic's current partner list overlaps with the slide but is not identical; it has since added names such as CAIS, Franklin Templeton, Invesco and MSCI.

![Layer 2: eight workflows give time back](../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/16.jpg)
*The eight skills that ship with the product.*

The eight skills that ship with Claude for Financial Advisors were pre-meeting prep, meeting follow-up, rebalance review, alternatives brief, estate and tax brief, prospect intake, advisor onboarding, and compliance review. "Use them as they come, or adapt them to your firm." Anthropic's product page lists the same eight, under slightly longer names.

## 5. Three questions to take back

![Take the frontier to your firm](../slides/10-13-0918-anthropic-accelerating-frontier-ai-in-fi/17.jpg)
*One workflow, one foundation, one connector.*

The closing slide turned the three frontiers into the "three questions to take back to your own firm" promised in the session description:

| Frontier | Ask | Question |
| --- | --- | --- |
| Intelligence | One workflow | Which use case are you applying the frontier models to? |
| Adoption | One foundation | How are you building the agent foundation to scale from a few to many? |
| Applied to financial services | One connector | How are you publishing connectors and skills for your users to build on? |

It ended: "How will you take on the frontier opportunity?"

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=68a1e78b-569e-4314-a56f-f20508123cbe) · [Livestream (Day 1; the host's introduction at 6:53, the talk itself is not in the recording)](https://www.youtube.com/watch?v=akEqiXFo3GM&t=413s) · [Claude for Financial Advisors](https://claude.com/resources/articles/claude-for-financial-advisors) (Anthropic) · [Future Proof coverage](https://investmentnews.com/practice-management/anthropics-big-deal-claude-for-financial-advisors-feels-the-love-future-proof/268189) (InvestmentNews) · [Gray Swan IPI results](https://benchlm.ai/benchmarks/gray-swan-ipi) (BenchLM)

There is no recording of this talk, so this write-up is based on slide photos only, plus the official session description and the sources above; it reflects what the slides showed, not what was said, and whether there was Q&A is unknown. The [precise notes](precise/10-13-0918-anthropic-accelerating-frontier-ai-in-fi.md) (Chinese) list every figure that could not be verified.
