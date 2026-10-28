**English** | [中文](zh/10-13-1340-proving-alpha-showcasing-ais-value-in-fu.md)

# A Hedge Fund Tries to Measure What AI Changes Before the P&L Shows It

*At AI Engineer NYC 2026, Hari Kumar of Contour Asset Management described how a small fundamental fund is trying to put a number on AI's value: live A/B tests of analysts' research, a "cost per useful insight" metric, and an honest count of the time spent fixing tools. Kumar deliberately gave no results.*

**Breakout · Tuesday, October 13, 2026 · 1:40–1:58pm · Turing Room (Riverside Ballroom)**
**Speaker:** Hari Kumar, Head of AI and Data Science, Contour Asset Management (previously data science at Meta and BCG)

---

![Measuring the impact of AI in fundamental investing](../slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/01.jpg)
*The title slide: "Measuring the impact of AI in fundamental investing."*

Hari Kumar's slides carried a different title from the schedule: "Measuring the impact of AI in fundamental investing," with the subtitle "What we are trying to capture, and where are are heading" (sic). Kumar introduced Contour Asset Management as "a hedge fund based in the city" that has "been around for a little over a decade," and said Kumar had joined in "the past year-ish." In Kumar's description, "we're a fundamental long short shop, around like 15 20 people, 6 billion AUM. And mostly invest in the TMT space," meaning technology, media and telecom. (The headcount and assets figure come from the talk and the video description; no independent source for them was found.)

Kumar's mandate was to show "impact across all areas of the firm, largely starting with this research process." The hard part is explaining that "we're spending this amount of money and time on AI spend … What are we actually gaining from it? Both from a P&L standpoint, which is to come, but also just from a more operational and are we getting good ideas perspective."

Kumar set expectations early: "intentionally trying to keep this a little bit vague and more in terms of the operational and process parts of it," leaving technical details for conversations afterwards. The talk contained no quantitative results.

## 1. What would have happened without AI?

![What would have happened without AI?](../slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/02.jpg)
*Three questions behind the fund's A/B tests, with the footnote "No clean control group. No easy counterfactual."*

The "fundamental question" slide framed the counterfactual in three parts:

| | Area | Question |
| --- | --- | --- |
| 01 | Investment decisions | Would we have found the same idea or reached the same conclusion? |
| 02 | Research access | How long would sourcing the information have taken manually? |
| 03 | Outcomes | Is the benefit better judgment, faster execution, or both? |

Its footnote admitted the difficulty: "No clean control group. No easy counterfactual."

The fund has about ten analysts, each with their own coverage; at a small fund, Kumar said, there is "one person that covers 30 to 40 names." The question is whether, "in a AI and more agentic world," an analyst "would have come to the same type of conclusion with the AI tools or without the AI tools." For the past few months the fund has been "creating live AB tests by recording and documenting their entire process that they would have done in a pre-AI world and how they're doing it as we roll out more workflows and tools."

Kumar described one form of the test. For a single name, over a three-month cycle "from one earnings report to the following one," the analyst first records the process, data sources and conclusions without any LLMs. Then, as tools arrive, the same process is run again with the portfolio manager. The tools include one that helps with expert-call interviews and an agent that can "field a consumer survey that's done by an agent instead of a human." For expert calls, the comparison is between an agent running the call while the analyst synthesizes the notes, and an agent synthesizing the notes while the analyst only draws the final conclusion. In the end it asks "how long would it have taken to source some of this information manually versus … in a more agentic manner." Kumar gave no figures from these tests.

## 2. Measuring the process, not only the P&L

Much of the work is idea generation, and theses mature at different speeds: some "can be proven out in a short-term scale … at least in like the 3-month window," while others "take 6 months or a year to manifest." That is why, Kumar said, "the challenge with something like this is you can't show ROI immediately. It's more so from a process-based manner": whether the tools improved the consumer-survey process or the process of updating models, and "are we actually changing the way someone's working or we just adding more thrust to their process." (The captions read "thrust"; the room recording heard "thrash.")

## 3. Better thinking, not just faster work

![Better thinking, not just faster work](../slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/03.jpg)
*The research-workflows slide and the question analysts are asked.*

The first numbered section of the deck, research workflows, was titled "Measure the incremental insight: better thinking, not just faster work." Under "How to Better Challenge a Thesis," the goal was "to surface sources an analyst would not otherwise use," and the use cases were expert-call and consumer-survey notes, idea generation, and alternative data feeding "second/third-order connections." A side panel gave the question to ask analysts: "What did you consider that you wouldn't have before?" and the instruction to "track novel ideas, differentiated sources, and ideas acted upon." The footer, partly hidden behind an audience member's head, began "The value is additional signal: not simply…"

Fundamental investing, Kumar said, means coming up with a thesis but not being "so anchored to a thesis that you come up with initially"; data, experts and other insight should remove those anchors, and "this is where I think that there's a lot of impact measurement when it comes to AI tooling." The fund's idea-generation agents add layers to existing ideas, by name and by analyst: given a few data points from alternative data or an expert, the agent adds conclusions and an estimate of whether the idea will pay off within months or take longer.

Kumar described a specific problem these agents address. An analyst works on an idea for a few days, the PM isn't interested or other work crowds it out, and it is dropped until months later, when "maybe you miss like a key price window." So an agent is "tracking through all of the notes that you've been taking, even if you only worked on an idea for a day, tracking it against the market," and decides whether it is worth flagging again "to not just you, but the PM and the trader." "The timing is actually the most important thing," Kumar said, so the agent also considers "when you should enter exit a position."

## 4. Bringing alternative data together

Funds spend heavily on new datasets, Kumar said, such as "satellite mapping or really granular credit card data," but "they're only helpful when they're all brought together." The example was Yipit (YipitData), whose credit card panel data "lives in isolation, but now with MCP and API access for things like Yipit, you can feed it into our agents, which is kind of what we've done." The agent reads transcripts of expert calls and analysts' notes, overlays credit card panel or spend data, and checks how the two corroborate each other.

## 5. Time spent, and "cost per useful insight"

Time is the other hard part of ROI. Money goes into tools, model updates and AI, but more important is "what is an analyst spending their day on." The fund built metrics for it: "these are the insights that have been generated by an analyst per day. What was the time spent on that? So, tracking time spent on each name. And what were the eventual P&L outcomes."

Investing has "a very very obvious success metric when it comes to P&L," Kumar said, but it is slow, "especially from a more fundamental shop, where you're not trading by the day," with returns "expected to materialize over months-long horizons." So the fund tracks "things like a cost per useful insight, which is the metric that we've come up with." Every idea generated in a day is logged, and the fund follows "Where do those ideas eventually land? What were the interesting ones? And what were the tools that were taken to come up with an idea like that?" Kumar noted that this measures "not just AI spend, but general tech investment." Kumar did not define the metric's numerator and denominator, say what counts as "useful," or give any values.

## 6. Beyond the research desk

![Bring the same discipline to the rest of the firm](../slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/06.jpg)
*The firmwide section: trading, operations, and finance and analytics.*

The third section of the deck, "Beyond the research desk," applied the same discipline to the rest of the firm:

| | Function | What the slide lists |
| --- | --- | --- |
| 01 | Trading | Order history, execution timing, and historical lookbacks |
| 02 | Operations | Repetitive work, exceptions, and process reliability |
| 03 | Finance / Analytics | Query, reconcile, and understand internal history |

The footer was partly hidden, beginning "As workflows become repeatable, more ou…"

In trading, the fund measures entry and exit timing and how positions are spread across sectors and across several names in one sector, with AI agents tracking those names. A trader's time "should not just be spent on executing orders. It should also be spent on bringing idea generation, feeding ideas to our analysts and associates." In operations, many workflows sit inside Bloomberg or an order-keeping system, each vendor has its own AI tools, and they are hard to connect. The fund is consolidating operations and compliance processes, such as order reconciliation and tracking vendor information, into "one specific tool" instead of external vendors' "piecemeal solutions," which Kumar called "a pretty big priority for us." Kumar acknowledged this part was "not necessarily to the point of this talk, a way to really capture ROI"; the aim is to free trading and operations staff for the fund's main goals: "create new ideas, understand the market, and focus on timing."

## 7. Same data spend, different utility

![Same data spend. Different utility.](../slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/05.jpg)
*The data-access slide: subscriptions used manually versus through agents, and the "new vendor math."*

The data-access slide (section 02 in the deck, though Kumar discussed it near the end) contrasted two ways of using a subscription:

| | Flow |
| --- | --- |
| Before | Subscription → manual search → extraction → research |
| With MCP / APIs | Subscription → agent calls → reusable research context |

Under "The new vendor math" it listed what to measure instead: utilization, calls, citations, adoption and cost per useful insight. Its footer: "Programmable access becomes a competitive requirement."

A fund's data spending, Kumar said, is either a set of separate tools ("an AlphaSense for … data, a Yipit for alt data") or a single platform ("buying a Rogo membership, or investing in like Kensho"). Contour is building internally: combining the analysts' tools into one platform and trying to "up level the spend that we have right now." Before, an analyst searched AlphaSense in its web interface and moved the results into a model by hand. "Things like Rogo will kind of take that one step further by allowing you to have all that data within your model itself, and you can have a platform that you chat with Rogo agents for." Contour keeps AlphaSense and Yipit but connects them to its own platform, which "learns from our analysts habits and is programmed specifically for each individual analyst and their model pattern." The question is "how can you increase the value of your subscription?" With vendors shipping MCP servers and APIs, "the goal should be to learn continuously."

## 8. From chat tool to operating layer

![From chat tool to operating layer](../slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/07.jpg)
*Where the fund sees AI heading.*

The "direction of travel" slide listed three shifts:

| From | To |
| --- | --- |
| Standalone chat | Agents embedded in workflows |
| Manual retrieval | Programmable data access |
| One-off answers | Repeatable, auditable processes |

Its footer named the differentiator as "data access, workflow design, and human judgment." Kumar did not go through it line by line; the spoken version came in the closing: "as AI adoption increases, especially at a lot of these smaller fundamental shops, having one unified layer that's doing a lot of different tasks across analysts is going to be really, really valuable."

## 9. An honest accounting

![Measuring ROI based on a variety of factors](../slides/10-13-1340-proving-alpha-showcasing-ais-value-in-fu/08.jpg)
*Upside and real costs, marked "a work in progress."*

The last content slide, "An honest accounting," marked as a work in progress, set the upside ("New signal. Better decisions. Expanded research capacity.") against the real costs ("Model errors. Broken connectors. Verification. Rework."). It ended: "Measure usage alongside quality, outcomes, and failure rates. Additionally explore longer-term tracking of investment ideas."

Bringing new tools to very tech-forward investors means something is always breaking, Kumar said, cases "where model calls break or the connector breaks down every couple days, and then it's your job to fix it." ROI has to count "the time that [is] being taken away trying to fix bugs, that someone from the tech side cannot fix itself," and, "when it comes to time spent for, let's say, valuable insight, what is the time detracted to understand how these tools work as well." That time is falling, Kumar said, "but it's a metric that we're still considering."

Kumar closed by inviting anyone with questions to connect after the talk; there was no Q&A in the session.

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=ae273f0b-db0f-4638-8ae3-05874bebf9ca) · [Video](https://www.youtube.com/watch?v=ZMZlAn6Pyfg) · [Rogo (Kleiner Perkins)](https://kleinerperkins.com/perspectives/rogo-the-ai-platform-for-global-finance)

This write-up is based on the official video captions, the author's room recording and slide photos. The session description promised more than the talk delivered: it mentioned tracing logged ideas through to positions and their performance against benchmarks, and tools for overnight news synthesis and earnings previews, none of which came up on stage. The recording ends without a Q&A. The [precise notes](precise/10-13-1340-proving-alpha-showcasing-ais-value-in-fu.md) (Chinese) keep timestamps and the full record of where the talk and slides differ.
