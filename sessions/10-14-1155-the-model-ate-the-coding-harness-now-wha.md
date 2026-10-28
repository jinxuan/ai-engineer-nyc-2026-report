**English** | [中文](zh/10-14-1155-the-model-ate-the-coding-harness-now-wha.md)

# Models Absorbed the Coding Harness, but the Harness Still Opens the Next Frontier

*At AI Engineer NYC 2026, Jediah Katz, who worked on Cursor's agent harness and now works on Grok Bot, argued that smarter models have made a fancy coding harness unnecessary, and that the harness still matters for new kinds of work, infrastructure, trust and cost.*

**Breakout Session · Wednesday, October 14, 2026 · 11:55am–12:13pm · Shannon Room (NY Ballroom E)**
**Speaker:** Jediah Katz, Engineer, Agent Harness, SpaceX Super Intelligence (previously at Cursor)

![Title slide: The Model Ate the Coding Harness. Now What?](../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/01.jpg)
*The title slide.*

---

"I joined Cursor early in 2025 as the longest tenured engineer working on our agent harness and recently I came along to SpaceX as part of the acquisition of Cursor," Jediah Katz said. The question for the talk: "Is the agent harness dead?" (The published video carries that question as its title.)

Katz then read out a line that sounded like a chatbot's preamble: "Here's a more natural and flowing version of that script which reads like a cohesive conference talk rather than a series of short points." After a beat, Katz said it was deliberate: "Just making sure everyone's still got their slop detectors on before lunch."

## 1. From prompt engineer to harness engineer

![Prompt Engineer, Context Engineer, Harness Engineer](../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/02.jpg)
*Three job titles rising across the slide.*

The story, Katz said, starts in early 2023, "when prompt engineer was the hottest title out there." By mid-2025 it had become context engineer, "someone who programmatically puts the best context for the model together on the fly." And "only at the beginning of this year did the industry land on the term agent harness and harness engineering to describe work that my team and I have been doing for a while."

## 2. What's in a harness

![Agent harness architecture](../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/04.jpg)
*The harness wraps the model in two layers and connects it to environments, connectors, inference and apps.*

"The harness is like the eyes and the hands for the model. It wraps around the model, steers it, and connects it to the world," Katz said. Users reach the harness through apps, and the harness connects the model to computers and to third-party services such as MCP servers.

The diagram showed the models at the center, wrapped by two layers:

| Layer | Components (from the slide) | Katz's description |
| --- | --- | --- |
| Context & Orchestration | Prompts, Tools, Memory, Compaction, Model Routing, Guardrails | "Kind of like the wild west." It controls "which models are called when and what context they see." |
| Runtime | Loop, State, Transport, Auth, Triggers, Observability | The foundation: "where the core agent loop runs and how state is stored so that work can resume." Building it "is a lot like traditional software engineering." |

Around the harness, the slide drew four connections: Environments, Connectors, Inference and Apps. The context and orchestration layer is what most people mean by "the harness," and it is where Katz primarily works, including "a bunch of tricks you might not know that we do to make the agent behave in the way that we want."

## 3. Peak harness, then "the harness is dead"

![Michael Truell's tweet about the browser built in Cursor](../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/05.jpg)
*Cursor CEO Michael Truell, January 14, 2026: a browser built with GPT-5.2 that ran for a week.*

"In the new year, we kind of hit peak harness," Katz said. The slide showed a tweet from Cursor's Michael Truell: "We built a browser with GPT-5.2 in Cursor. It ran uninterrupted for one week." It described "3M+ lines of code across thousands of files," with a from-scratch rendering engine in Rust including HTML parsing, CSS cascade, layout, text shaping, paint and a custom JS VM, and said it "kind of" works. (Simon Willison's coverage of Cursor's write-up cited over a million lines across about a thousand files.) It used a new long-running harness called Grind Swarm, "a multi-model harness that orchestrated a lot of agents and a lot of machines," Katz said. "It was very expensive to run. It actually took down Cursor maybe once or twice to get this demo."

![Is the harness already dead?](../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/06.jpg)
*Four posts arguing the harness is shrinking or gone.*

"Like anything in the world of agents, it only took a few months of peak harness for people online to start saying that the harness was dead." The slide collected four examples:

| Source | Title |
| --- | --- |
| Thariq (@trq212, Anthropic), July 24 | "The new rules of context engineering for Claude 5 models" |
| Garry Tan (@garrytan, YC) | "Thin Harness, Fat Skills" |
| Cursor blog, April 30, 2026 (Stefan Heule and Jediah Katz) | "Continually improving our agent harness" |
| Xuan Huang (@Huxpro), September 29, 2026 | "Harness is dead" |

In Katz's summary, Thariq described killing much of Claude Code's system prompt, and Tan argued the harness would fade and only skills would matter. Katz also counted the post Katz co-wrote with colleague Stefan Heule about removing guardrails from Cursor that had patched over gaps in the model's intelligence: a small model that existed only to apply code edits, because frontier models weren't good at it, and coding-specific instructions on "how to explore the codebase and how to check your work." The common thread: "They were saying that smarter models are what killed it."

## 4. The evidence: simple harnesses keep up on coding evals

![BASH plus a secrets.env file](../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/07.jpg)
*The simplest possible harness: a shell and a file of (fake) API keys.*

Katz asked the audience to imagine "a very simple harness where the model only has access to the shell and to all of my secrets. Please don't screenshot this slide by the way. This one's off limits." The slide showed "BASH +" a `secrets.env` file, "everything the agent needs (please don't commit this)," filled with obviously fake keys for OpenAI, Anthropic, GitHub, Slack, Stripe, AWS and others. Asked who thought this harness would do as well on coding evals as a sophisticated one, "a few hands" went up, "but most of you know [it wouldn't]."

![Two studies on harness choice](../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/08.jpg)
*Left: UC Berkeley on SWE-bench Lite. Right: Apple and EPFL on MLE-bench, shown through Pedro Domingos's tweet.*

Two recent studies tested exactly that. The first, from UC Berkeley, "tried seven different frontier models on a series of harnesses including Pi, Codex, and Claude Code," and found, "particularly on the newer smarter models," that "the choice of harness did not actually make a difference significantly in the score on SWE-bench Lite." The slide's caption read "Harness choice barely moved SWE-Bench-Lite success rate." The bar chart, read from the photo:

| Model | Pi | Codex | Claude Code |
| --- | --- | --- | --- |
| Claude Fable 5 | 96.7% | 96.7% | 97.8% |
| Claude Opus 4.8 | 82.2% | 88.9% | 86.7% |
| Claude Sonnet 4.6 | 64.4% | 68.9% | 66.7% |
| Claude Haiku 4.5 | 60.0% | 57.8% | 52.2% |
| GPT-5.6 Sol | 74.4% | 73.3% | 77.8% |
| GPT-5.6 Luna | 53.3% | 55.6% | 55.6% |
| Kimi K3 | 72.2% | 74.4% | (cut off in photo) |

The gaps are smallest at the top: about one point for Claude Fable 5. They reach 6.7 points for Opus 4.8 and 7.8 points for Haiku 4.5, which fits Katz's qualifier about newer, smarter models.

The second study, from Apple and EPFL, "found similarly that a dedicated MLE harness did not make a big difference on MLE-bench." The slide showed Pedro Domingos's tweet summarizing it as "Harnesses are useless," quoting the paper "How Much of a Harness Does a Strong Agent Need for Autonomous ML Engineering?" (Kirill Brilliantov, Alejandro Hernández-Cano and Emmanuel Abbé). Its highlighted sentence: under an equal time budget and the same frontier LLM, open-source state-of-the-art harnesses "provide no advantages over a single session of a minimal-harness baseline, pointing to the backbone as the primary driver for performance."

Katz accepted the finding: "A fancy harness in this day and age no longer adds coding intelligence. I think that is true. I know that's surprising." The things harnesses used to do to patch over gaps in "the raw coding muscle memory of the model," such as teaching it to explore the codebase, to work persistently for a long time "with things like slash goal," and to run tests to check its work: "The model has natively absorbed these skills."

## 5. The data flywheel

![The data flywheel](../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/15.jpg)
*"The model eats the harness. The harness opens the next frontier." The flywheel built up over several slides.*

Why is this happening? "The harness is actually making itself obsolete through what I call the data flywheel." The cycle on the slide has five steps: the harness unlocks capabilities; agents take on new tasks; tasks become training data; the model absorbs the ability; the harness deletes the scaffolding. In Katz's words, the harness brings harder coding tasks within reach, more users attempt them with agents, "more coding data is shared with the labs," the models learn the skills natively, and eventually the harness "can actually delete the things it needed to do to make those tasks work in the first place."

![Why care about the harness?](../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/11.jpg)
*Two reasons the harness isn't dead.*

"Is the harness really dead? I say no, and there's two reasons why." The slide: (1) "The harness unlocks the next frontier for the model to solve." (2) "Parts of the harness can never be absorbed." (On stage Katz said the model "cannot absorb" those parts, "especially outside the context layer," without the slide's "never.")

## 6. Reason one: the harness opens the next frontier

Katz's first example was Grok Bot, "our autonomous colleague who gets work done for you. Not just coding, but all kinds of knowledge work." It has "already created a new category," Katz said, before turning to its harness. Rather than one-off chats, Grok Bot's harness runs "long-lived coordinator bots that orchestrate a fleet of worker agents," like "hiring a high-powered lawyer who deals with you directly and owns strategy, but they farm out their tasks to a team of paralegals."

At Cursor, the team removed prompting and guardrails as models got smarter. "But, curiously in Grok Bot, we've actually found ourselves putting back a lot of these things, adding new prompting, adding new guardrails," because the models "have not nailed yet" many tasks in its new domains.

![Key website flows, Communication, Acting vs asking, Learning over time](../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/13.jpg)
*Four areas the Grok Bot harness is working on now.*

Katz listed four. Key website flows, such as making slide decks and booking travel. Communication: "An autonomous colleague needs to keep you up to date on what it's been doing without overwhelming you." Acting versus asking: Grok Bot can do risky things like emailing colleagues, and "you only want us to do it in the way that you intended." And learning how the user works over time through memories. None of these is new to people who have pushed coding agents, but "for Grok Bot, they're much more important because everybody is needing them," and right now "it is the harness that is making these tasks feasible."

That completes the flywheel: "When a capability like coding gets commoditized by the model, it's actually the harness which unlocks a new frontier for the model to then solve and kicks off that flywheel again." The new frontier can be outside coding, as with Grok Bot, or within software engineering beyond coding: "the more senior you get, the less coding you're actually doing." Katz called this "the core idea that I would like you to understand."

The second example was the software factory, "multiple agents coordinating together to solve the full software development life cycle," which Katz called "far from a solved vision." Internally, someone reports a bug in a channel; an automation triages it; a cloud agent tries to fix it and uses internal tooling to record before-and-after videos proving the fix; once the fix is merged and deploying, a rollouts product starts another agent to watch production for issues. "You can think of all this as almost like a meta harness, which is orchestrating many agents together."

## 7. Reason two: what the model can't absorb

The second reason covers parts of the harness outside the model's reach. Katz named three.

![Robots working on a cloud while a person sleeps](../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/16.jpg)
*Infrastructure: agents keep working in the cloud while their owner sleeps.*

**Infrastructure and runtime.** Over the past year, agents have gone from working on a typical task for minutes to hours to days and longer. "There's just no way that this can work on one person's laptop, which might go to sleep. It has to be running in the cloud if you really want an autonomous colleague."

![Three permission dialogs for an agent](../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/17.jpg)
*Scoped permissions: Slack channels you choose, commands on this Mac, pull requests in repositories you pick.*

**Trust and security.** Bash plus a file of API keys can solve coding tasks, "but you probably don't want to give all your API keys in a file to the model… I'm sure that your head of security would have something to say." The slide showed three permission prompts: an agent asking to access Slack ("read and post in the channels you choose"), to control a Mac ("run commands on this Mac"), and to access GitHub ("open pull requests in the repositories you pick"). Companies need permissions scoped to specific agents and people, and guardrails matter "for anyone who doesn't want to wake up one morning to a really catastrophic action or to a $10,000 API fee."

![Tweets about Grok Bot's efficiency and speed](../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/19.jpg)
*Katz's posts on Grok Bot usage and speed, with user reactions.*

**Efficiency.** In both studies where the harness barely changed scores, "there was a significant difference in cost per task," Katz said. Katz had spent the past month optimizing the Grok Bot harness, tuning "cache hit rate… token efficiency… model mix," to bring down cost and latency "without any hit to quality." The specific numbers were on the slide, in posts Katz had published:

| Date | Post | Figure |
| --- | --- | --- |
| September 5 | Improvements to routing, caching, dynamic context usage and token efficiency | Usage goes further: 10% on average, up to 35% for some heavy users |
| October 1 | "Major improvements to make Grok Bot 3x faster" | Time to reply: P50 down 65%, P90 down 72% |

Two user posts sat alongside: "WAIT A SECOND. Why is Grok bot so fast today??" and "feel like grok bot is fast AF now." The takeaway: "The more and more that you use agents to do your work… the more valuable it will be for you to have the most efficient harness."

## 8. What's next

"That flywheel is speeding up, and right now we've entered a new age where the product is once again moving faster than the model," Katz said. So the harness "is going to get more messy before it gets cleaned up," and harnesses should be built so features are easy to ship and unship.

![Designing to be deleted](../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/20.jpg)
*Two design rules: hide inner workings, build extensible primitives.*

The slide gave two rules. First, "Show users outcomes, not harness inner workings (Hyrum's law)." Grok Bot and similar products expose less and less of the harness to users, because under Hyrum's law "anything that you expose to users, they will come to depend on." Users get less visibility, but there is "less change management to deal with, and you can iterate much faster." Second, "Design extensible primitives for agents": building blocks that let you add features without complicating things for the agent and remove them without a complicated refactor. "It's just good engineering."

![Designing agent primitives: subscriptions](../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/21.jpg)
*One subscriptions primitive routes many kinds of events to the right agent.*

Katz's example primitive is subscriptions. Agents need to wake up in response to events: an email to the bot, a Slack mention, a PR review, a PagerDuty alert, a weekday 9am schedule, or CI going red on a PR the agent cares about. Instead of separate integrations for each, there is "one framework and one tool" the agent uses to subscribe. On the slide, a subscriptions layer handles permissions, routes to the right agent, deduplicates, filters noise and wakes the agent, exposed as `subscribe(trigger)` and `wake(event)`.

![Firings by shim](../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/22.jpg)
*A dashboard of how often each shim fires, over about 45 minutes.*

The second example was shims: deterministic guardrails that patch over bad model behavior, such as nudging Grok Bot with "Hey, user's waiting on you" when it doesn't reply fast enough. "We'd rather not do these things. They're kind of like hacks, but we would rather fix them now than… wait for the next model checkpoint." Every shim is tracked centrally and logged whenever it fires. The dashboard on the slide stacked firings by shim over time, with names such as `mcp_missing_required_arguments`, `mcp_schema_key_spelling`, `mcp_argument_envelope`, `await_task_id_alias` and `legacy_update_state_forwa…`, plus 16 more. A bot watches the dashboard, and "whenever a shim stops firing, that model or that agent will put up a PR and say this shim can be cleaned up."

![What's next for the harness?](../slides/10-14-1155-the-model-ate-the-coding-harness-now-wha/23.jpg)
*Three predictions.*

The closing slide made three predictions: (1) "Harnesses will be designed to be deleted." (2) "The harness moves up a level: agents are the new building blocks." Yesterday's harnesses "were built from composing a bunch of LLM calls"; Grok Bot and the software factory use agents as the core building block. (3) "Within 6 months, 90% of agent work runs off the laptop."

## 9. Conclusion

"Is the harness dead? The parts that made it smarter at coding, yes, that is the flywheel at work," Katz said. But the harness is how agents unlock the next frontier of work, and "the trust, the efficiency, the infrastructure that the harness brings, those things are not going anywhere. So, yesterday's harness may be dead, but long live the agent harness of tomorrow."

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=29083884-87dc-44f8-b412-b494afca8856) · [Video: "Is the Agent Harness Dead?"](https://www.youtube.com/watch?v=7Y4sbZgBdnc) · [Talk page](https://ai.engineer/talks/7Y4sbZgBdnc) · [Continually improving our agent harness](https://cursor.com/blog/continually-improving-our-agent-harness) (Stefan Heule, Jediah Katz) · [How Much of a Harness Does a Strong Agent Need for Autonomous ML Engineering?](https://machinelearning.apple.com/research/harness-autonomous-ml-engineering) (Apple ML Research) · [HarnessTax study](https://arena.ai/blog/coding-agents-harness-tax/) (Arena blog, the Berkeley study) · [Cursor cloud agent docs](https://cursor.com/docs/cloud-agent) · [Cursor is now a part of SpaceX](https://cursor.com/blog/joining-spacex) · [Scaling long-running autonomous coding](https://simonwillison.net/2026/Jan/19/scaling-long-running-autonomous-coding/) (Simon Willison)

This write-up is based on the official video captions, the author's room recording of the same session, 17 of the author's 23 slide photos (the rest are build-up steps of the flywheel, a blurry shot and near-duplicates), and the sources above. The talk ended with applause and no Q&A. The [precise notes](precise/10-14-1155-the-model-ate-the-coding-harness-now-wha.md) (Chinese) keep timestamps and the full record of where the slides and sources disagree.
