**English** | [中文](zh/10-12-0900-orchestrating-claude-managed-agents.md)

# Let Claude Draw Its Own Org Chart: Flows Bring Up to 1,000 Agents to Claude Managed Agents

*At AI Engineer NYC 2026, Anthropic's CJ Avilla and Harrison Stall introduced flows, the dynamic-workflows feature of Claude Managed Agents that lets Claude plan and run its own multi-agent topology through the API, and argued that running hundreds of agents safely is mostly a matter of budgets, isolation and keeping credentials out of the sandbox.*

**Workshop · Monday, October 12, 2026 · 9:00–11:00am · Sutton Room (NY Ballroom W)**
**Speakers:** CJ Avilla, Member of Technical Staff, Anthropic · Harrison Stall, Member of Technical Staff, Anthropic

---

CJ Avilla opened with operating-system history. In 1969, "just about 30 miles from here," Ken Thompson had a month alone at Bell Labs while his wife took their one-year-old son to visit family in California. Avilla said Thompson split the month into weeks, one each for a kernel, an editor, an assembler and "one other thing," and ended it with Unix. What made that possible, Avilla said, was "conceptual integrity": "one person can hold the entire project in their head and execute the entire project end to end."

The counterexample was IBM's OS/360 in the same years: a bet-the-company project that, by Avilla's account, IBM sized at about 5,000 man-years and staffed with as many as 1,000 engineers. Fred Brooks wrote down the lessons in *The Mythical Man-Month*, including the "surgical team": one chief who holds the whole design, surrounded by about ten specialists. "We also want to take that same sort of structure and scale it up much further," Avilla said.

Linux showed how. Avilla cited the recent Linux 7.2 release, with "2,600 something different developers across 300 different companies" (LWN counted 2,652 developers; the company count wasn't checked). It didn't start that way. In 1998 Linus Torvalds, swamped by emailed patches, told the kernel list he was going on vacation, and Larry McVoy replied with the line Avilla quoted: "The problem is that Linus doesn't scale." By around 2002 Torvalds had moved from a star, with everyone sending patches to him, to "layers of trust," with trusted maintainers reviewing code and only the exceptions reaching him. The slide contrasted the two: about 36 actors pointing at one person, against a tree with at most four people at each level.

Avilla drew the parallel to today: engineers who manage many agents "might start to feel this feeling of … being the bottleneck," sitting in GitHub approving a flood of agent-written pull requests. The workshop was about giving that structure to Claude.

## 1. Flows: Claude orchestrates its own topology

Harrison Stall announced the feature. "On Friday" (October 9, per Anthropic's release notes), Claude Managed Agents, the hosted agent API Anthropic launched in April, gained what the speakers called flows: "the next version of multi-agent orchestration, where you can let Claude fully orchestrate the entirety of its own multi-agent topology." Anthropic's documentation calls the feature dynamic workflows and each execution a workflow run.

The idea comes from Claude Code, where typing "ultracode" (or asking for a workflow) has Claude write a JavaScript program that fans work out to many subagents. Instead of drawing a "classify and act or fan out and synthesize adversarial verification style" topology by hand, "you can literally just tell Claude … and Claude will just know based on model training … how to effectively orchestrate itself."

Stall offered what were, in Stall's words, "hand waved numbers" for why this matters: a single agent "keels over" after about an hour; a coordinator with a roster of subagents, the first multi-agent design in Managed Agents, lasts a little longer; dynamic workflows allow "arbitrarily complex nested sets of agents operating all in parallel."

Teams building their own multi-agent harnesses, Stall said, kept hitting the same problems: restarting containers ("what if one of your agents out of a hundred dies … two hours into the task"), a single coordinator trying to track 60 agents ("Linus doesn't scale like that"), and no way to see the state of the whole graph. Stall also recalled the meme from after Opus 4.5, when people carried half-open laptops so Claude Code wouldn't die and sat in parks pressing "approve, approve, approve." "That's just not going to scale for our ambitions."

About half the room had heard of Claude Managed Agents. Stall described it as an API that lets Anthropic manage the agent life cycle: you define an agent (skills, tools, MCP servers), run it in a session, and watch it through a durable event log. The first multi-agent version, shipped in May, required defining every subagent up front and listening to each agent's thread, like threads in an operating system that share state by passing messages. "It wasn't the most trivial thing to set up."

With flows, a lead agent writes a plan (Stall's example: "migrate my million line mono repo from Python to Rust"), fans out to agents it starts and stops itself, and works through phases of "discrete, verifiable units of work." A single event stream shows everything the agents do; a budget caps spend; and the API can interrupt a run that is going the wrong way. Unlike Claude Code, this runs behind an API, so it can be triggered on a schedule, from a webhook or from CI. Stall's example was a bug-hunter workflow on GitHub.

Anthropic's documentation sets the limits. A workflow run can start up to 1,000 agents over its life, with up to 64 working at once (not guaranteed), and runs 24 hours by default. Only the session's primary agent can start a run, so runs don't nest. It is enabled with the `multiagent_20261001` type.

## 2. Demo: a stock watchlist scanner

"Because I recognize there is a … bunch of finance oriented folks in the room," Stall ran the stock watchlist scanner template from the Claude Console. Under "Managed agents," the Quick start page lists one-click templates, with a dynamic-workflow tab at the bottom. The template creates an agent on Claude Opus 5.5 with the multiagent type and a persona system prompt.

The agent also needs an environment, "a computer essentially for Claude to operate on." Stall said that over the past 10 or 12 months, giving Claude a computer had made it "way way way more capable just across the board." Using an environment prepared that morning, Stall started a session with Apple, Nvidia, Amazon, JPM, XOM and COST. The Sessions pane showed the lead agent, "the person at the top of the pyramid, the CEO," and its session threads, each "the single context window, the single instantiation of an agent," spinning up in parallel. A finished run from earlier had used "only like six concurrent agents or so at each step"; Claude scales the count to the task.

## 3. From the console to production

Avilla took over to show where flows go after prototyping. The usual next step is to wire the agent into an application: run the scanner every weekday five minutes before market open, or trigger it from Slack. In code, enabling flows means passing the multiagent configuration when creating the agent and telling the lead agent, in its first event, to "run a workflow." "That's all you need to do."

By default Claude invents the subagents it needs. Teams can also pass preconfigured subagents, for example ones that specialize in Yahoo Finance, news sources or the SEC's EDGAR. Agents can use custom skills, MCP servers and custom tools, and the environment can be a self-hosted sandbox in your own infrastructure.

For the scanner, a flow hands each ticker to its own scan agent, "an isolated context where all it cares about is its one ticker," so other tickers' search results can't sway it. A review phase, also isolated, follows. The speakers ran the scanner both ways. Their comparison slide (its exact figures weren't read out) showed the flow costing "much more" but being "much more effective at finding all of the details." Flows are for when "I want to go really deep and make sure that I'm leaving no stone unturned, and I'm willing to spend the money."

The prompt can also shape where the money goes. Avilla's example: scan with Haiku, "a really fast, efficient model," two agents per ticker, then review with Fable, "two or three agents per ticker," because the review is where details get missed. Each flow can "expand and contract based on how much you want to invest in that point in the process." For important tasks, Avilla recommended a full eval set.

The templates also ship in Claude Code's Claude API skill: `/claude-api managed-agents-onboard` followed by the template name sets one up, using Anthropic's `ant` CLI to call the API. Avilla said they often build managed agents from Claude Code rather than the console. The team found that open prompts work best at the start ("use a workflow, one ticker per agent … this is what I'm kind of looking for"), letting Claude design the flow before you fine-tune it.

Attendees got about $50 in API credits from a QR code. Avilla also pointed to that week's news that Max and Team subscriptions now include API credits: $100 a month on Max 5x and up to $500 for Team. (Anthropic's documentation confirms those figures and that the credits cover the API and Managed Agents but not Claude Code.)

## 4. Bug Hunter: closing the review loop

After a first round of questions, Avilla returned to a theme from the previous AI Engineer conference: "How can we close these loops" so systems run more on their own. Bug Hunter is a flow Anthropic uses on its own pull requests, available as a template and in Claude Code.

A single agent reviewing a PR does worse than this pipeline. The code is split into slices, each handed to an agent with its own context that notes "this might be a problem." A second phase reviews the same slice and agrees or disagrees with each candidate. A third picks the candidates that might be real. Then a prover phase downloads the repository and tries to reproduce each bug. In practice the results land as inline PR comments with a severity. The value, Avilla said, is what a good human reviewer brings: knowledge of "this distant corner of the code base" that the PR never mentions. The template is set up with `/claude-api managed-agents-onboard bug-hunter`.

## 5. Explainer Studio: a flow that makes videos

Few attendees followed @ClaudeDevs on X ("not enough of you"), whose feature announcements often come with animated explainer videos. Many of those are made by a flow. It researches the details, plans, scouts and fact-checks, writes a design brief, then hands off to production phases that build and animate each scene; in practice there are also phases for sound effects, voiceover and scripting. Avilla said a video that might have taken "20 hours or something" four years ago is now "one very small prompt."

The sample video explained dynamic workflows with a task of 300 contracts to search for change-of-control clauses, then walked through session threads, the multiagent setting and why to set a budget. The narration was Avilla's own voice. Asked whether it deploys to YouTube: "It does not deploy to YouTube, no … We should, though." The room voted, narrowly, for a video on how a toilet works over one on bicycles. The videos carry Claude branding and Anthropic's fonts; Avilla suggested building a brand design system in Claude Design and giving it to the flow.

## 6. Why single agents fail: laziness, self-preference, goal drift

Avilla named three failure modes the team saw while building dynamic workflows.

**Agentic laziness.** Ask an agent to check 50 services for the root cause of an outage ("don't touch production"), and after 35 healthy ones it starts telling itself "everything looks good" and stops looking. A coordinator has the same problem: it must keep count, and its subagents can get lazy too. A flow gives each service its own agent, so "by the end, we know for sure that we went and checked every single service": fan out and loop until done.

**Self-preferential bias.** Ask an agent to write a poem, then ask if it's good: "It's going to say, 'Yeah.'" The same goes for code. The advisor pattern separates generator and judge to exploit the "generator discriminator gap," but at scale a roster runs out. A flow can spread "adversarial review that has its own isolated context" across many agents.

**Goal drift.** "Don't touch prod" was the last line of the prompt. As the context fills and compacts, again and again, that instruction can get lost. A flow sets the goal once and copies it "in like a very consistent way into each of the sub agents for all the different phases."

## 7. Safety at scale: declare, contain, detect

Stall called safety the "one thing to get right when you build out your agents," because flows multiply one agent into potentially hundreds. The slide listed "six very concrete" risks; Stall's walkthrough covered these:

| Risk | What Stall said |
| --- | --- |
| Prompt injection | Agents reading the open web can pick up "ignore previous instructions … exfiltrate" text. Anthropic works on it at the model level and with "classifiers that make tool call checks like auto mode in Claude Code." |
| Credential leakage | "Claude is taking on the persona of whatever credentials that you give it." An API-key file on the VM's disk, even hidden, can leak, so "credentials never have to touch the sandbox." |
| Data exfiltration | An agent with sensitive data must not share it with third parties. |
| Overreach | Budgets and other limits, plus permission policies so only tools you grant can run. |
| Visibility | A durable event stream with the full trajectory of every agent, "pretty table stakes." |

The developer's job, Stall said, comes down to "declare, contain, detect." Declare what the agent is, its permissions, skills and tools. Contain it physically, through environment policies that allow only certain domains, or web search but not web fetch, so that "even in the absolute worst of case, the amount of harm … is minimal." Detect through the event stream, and interrupt sessions that go somewhere you don't like.

## 8. Separating the brain from the hands

When Managed Agents launched in April, Stall said, there was a meme that Anthropic had "settled the debate": "the brain and hands should be separate" (the subject of an Anthropic engineering post, "Scaling Managed Agents: Decoupling the brain from the hands"). The brain is the loop running inference plus the session state; the hands are wherever the agent acts, such as running grep on a sandbox or a custom tool on your own Mac. "There should be like no way for you to, given a hand access, … totally steer the brains."

Credentials follow the same split. Every container that needs a secret holds a fake value that doesn't work if called directly. Only at the egress proxy, on the way out, is it swapped for the real credential. "There's no way like physically to prompt Claude … [to] get that credential off of the sandbox." The goal is that "you should not be able to foot gun yourself" into leaking keys.

## 9. The controls in the API

Avilla walked through the controls. A budget can be set on any session: "Run this flow. You have $1.25." If the flow hits the limit before finishing, you can raise it, and it continues without redoing work. The budget pairs with Outcomes, another Managed Agents feature: "Here is the outcome that I want and the rubric that I expect, and here is a budget." A second code sample sent a `user.interrupt` event, which Avilla said "will stop the flow" so you can re-prompt. (Anthropic's documentation is narrower: an interrupt stops the agent's current turn but "ends no run"; to stop a run you send a message asking the agent to stop it.) Avilla suggested a practice exercise: run a flow with a deliberately low budget, then raise it and watch it resume.

Because every event is logged, a second managed agent can audit the first: pull the transcripts through the API and ask, "tell me if my agents did anything that was unexpected or accessed anything … they weren't supposed to."

The "lowest-hanging fruit," Avilla said, is to switch the permission policy from always-ask to auto mode. People clicking "approve, approve, approve" in the park say yes "like 97% of the time"; Claude, using auto mode, is "much more patient" in judging whether each tool call is safe or destructive. (Anthropic's August post on making auto mode the default in Claude Code gives the same 97% figure, and reports that in a controlled study human review caught 13.6% of dangerous commands against auto mode's 89%. Managed Agents added an `auto` permission policy in September.)

## 10. Vaults, minimal tools and network allowlists

Stall's last example used the Twilio API. The naive approach uploads a `.env` file with the key so a Python script in the container can call Twilio, leaving the key in plain text. Instead, the key goes into a vault as an environment-variable credential. An injection-location setting says where the secret should appear in outbound traffic (a bearer auth header or the request body), and an allowed-hosts list says which domain it may be substituted for, because "we don't want like our Twilio API key to ever go out to … mydomain.com." The session is created with the vault, and the platform swaps placeholders for real values at the egress layer.

Tools should be the smallest set possible. Web fetch and web search can be disabled. MCP tools deserve care because their list isn't known in advance: "what if something happened and [GitHub] were compromised and an attacker added like … my exfiltration tool?" So MCP tools can be enabled one by one. Finally, the environment's network can be limited, for example to `api.twilio.com` only; a call anywhere else "would like physically fail at the … firewall layer."

Avilla closed by returning to the opening: "We're … in many cases trying to like reinvent the wheel of these organizational learnings that we had" in the 1960s and '70s. "Start from a template, shape it … to be your own. Tell us what you find."

## Q&A

Questions came in two rounds, one after the demo (about half an hour) and one at the end. Most questions were inaudible on the recording and are given as the speakers restated them. Where the recording doesn't make clear which speaker answered, the answer is attributed to both.

### Scale and ambition

**Is this how the recent burst of scientific results, like on Navier-Stokes, gets done?** "Maybe in our … demos our stock ticker is not ambitious enough," Stall said. Flows are "a way … to throw test time compute at hard problems"; Stall couldn't describe Anthropic's internal research harnesses, but making this an API is a step toward making such work "broadly doable by the general public." Avilla added that the old coordinator-and-roster design capped subagents, while flows allow "I think it's 1,000 agents," and a lead agent can run several flows at once or one after another. On Friday, Avilla had started a flow with the prompt "Ken Thompson made Unix in a week. Can you try and make an operating system that's Unix compatible with a Python interpreter? Use flows … feel free to be brave and use up to 1,000 agents," plus a GitHub repository. Avilla had hoped to show it finished by Monday at 9am. It was still running after "like 85 hours," three or four flows in, with about 1,000 commits.

**Do more agents bring diminishing returns, like coordination costs in a growing company?** Stall had no number: the field is new, and the "capability curve per subagent" may become calculable later. Avilla added that the same 100 agents do better in a flow "because there's some organization and isolation between those contexts with gating and these different phases."

**How does Claude know what scale to work at?** "Evals and lower defaults," Stall said. A stock scan shouldn't spawn 1,000 agents; a Navier-Stokes problem might need you to push the flow explicitly toward more parallelism.

**How deep can topologies nest?** "Just one layer of nesting" today, with no arbitrarily complex fan-outs; deeper nesting may come once "the safety and stuff is really locked in." (Anthropic's documentation says the same: runs don't nest.)

**Should a big project get a pre-built dependency graph?** "Totally." Upload it as a file into the session and tell Claude where it is.

**What if new information arrives halfway through a two-day run?** Interrupt the session and add the information ("lemma X is actually probably useful for you"). The primary agent knows the state of the flow and can start a new one that uses both the new information and the history. What isn't supported is inserting a step into the original plan and rerunning later steps.

### Running, watching and steering flows

**Does the Claude API skill in Claude Code run on my machine or in Managed Agents?** Avilla: Claude Code runs locally or in the cloud depending on your setup, and it is a separate system; it makes API calls to create the agent, session and other resources.

**Can I see the topology while it runs?** Yes, in the console, Stall said, and every subagent's session thread is available through the API for building your own UI. Avilla showed the console drilling into individual subagents and their events, including the payload the lead agent passed to each. Stall noted that the dashboard is built on the public API: "Anything that you see here, you could totally hand roll yourself."

**Does the model know its budget?** Not today. The budget is a guardrail against runaway spend, and "if you tell it the budget, Claude can get a little bit anxious," Stall said. Avilla described the mechanics: when a session hits, say, $5, you get a budget-reached event, can raise the budget while the session is live, and the flow picks up where it left off without losing completed work.

**Can subagents message each other?** Not directly through the API, Stall said, but they share a file system and can pass notes through it; the API shows agent message sent and received events.

**How are contexts kept apart?** Each agent runs in its own session thread, "physically in the database like separate context windows," with its own system prompt, and inter-agent messages are visible "if you want to make sure that … [a] middle management agent isn't like doing something with … its underlings that … you don't want."

**Can the shared file system be partitioned?** Only by prompting today; there is no directory-level permission layer, which Stall called "pretty useful" and promised to take back. Asked about hooks, Stall pointed to tool permissions: a read request can be routed to your system, which checks which agent asked and denies access to, say, `/mount/foobar`.

**How do you check intermediate state on a long run like the operating system?** Avilla's prompt had asked for a team of documentation writers ("use Sonnet 5.5 … I love the … documentation that comes out of that") to document each phase in the GitHub repo. You can also interrupt, or have Claude keep a report file updated. Stall added that anything written to the outputs directory is stored durably and downloadable through the Files API.

**Can a human give feedback mid-flow?** Give Claude a custom tool such as "request human feedback," and answer it when called.

**How do you stop infinite loops or cascading errors?** "Yes, sort of," Stall said. Set a budget; beyond that, Claude is "pretty good at detecting subagents that are misbehaving" and cutting them out, and learning from past runs helps.

**Can a flow run only part of itself when nothing has changed?** Not on the platform today, other than telling Claude to act conditionally. Stall would layer that detection into the application. A task a single agent can do in 30 seconds isn't a fit for flows, which are "for … ambitious, complex tasks."

**Can behavior vary with the type of work rather than the system prompt?** Stall pointed to per-session effort settings and per-session overrides of agent fields.

### Improving flows over time

**How do you cut token spend over time?** It is a hill-climbing problem, Stall said: use the hill-climb skill in Claude Code on the prompt, or give Claude the run transcripts and ask where the flow does too much or too little. Avilla added Dreams, a Managed Agents feature that takes a set of sessions and improves the attached memory store. After, say, ten runs, the dream can "crystallize and improve the memory from … session to session." Stall: "I basically described hand rolling your own dreaming."

**Can flows improve their own prompts?** Not automatically. Deployments run sessions on a schedule, but nothing schedules self-improvement, though Stall could imagine building it from these parts. Avilla described the `/claude-api hillclimb` command as "taking … Andrej Karpathy's auto research and … baking it into a skill." When a new Haiku model came out, Avilla built a charades game for the family (a webcam, a card, Haiku guessing the mime) and ran hill-climb on its prompt. Claude generated an artifact for recording clips of Avilla rowing a boat or playing tennis, and used them to tune details such as frames per second.

**How do you iterate on output that only taste can judge, like a video?** Avilla suggested starting in Claude Code: work the process out by hand or with dynamic workflows, have Claude encode it as a skill, use that skill in a Managed Agents flow, then hill-climb the prompt with a human choosing between pairs of outputs.

**Is markdown the right way to define agents and watch them?** "It depends," Stall said, but "if you throw in your API key to a markdown file … that's definitely bad. Do not do that." Avilla added a technique: a main document that indexes others (kernel here, assembler there, editor there) so each subagent reads only what it needs.

### Integrations and environments

**Can a workflow be packaged as a plugin?** Not today. Individual plugin components such as skills drop into the API already. A workflow's generated prompt is a text file you can check into Claude Code "and it'll … probably be pretty pretty close."

**Can some agents run on a Mac or another machine?** Use custom tools: the model asks your code to act, you run the action on that machine, and return the result.

**What about data behind a VPN?** MCP tunnels give agents private MCP access, using Cloudflare tunnels today according to Stall, without exposing anything to the open internet.

**Does self-hosting change anything?** Some features aren't fully supported on self-hosted sandboxes, but flows are.

**How does Bug Hunter handle dependencies across repositories?** It is "kind of up to Claude": every repository you grant is mounted in the container, and Claude uses a clean Bazel build if there is one, or searches for symbols if not. Each PR review mounts the repositories again, like a CI checkout.

**What about in-game telemetry?** The event stream can be streamed live or listed. Managed Agents accepts images, but sending game screenshots for real-time action "would be a pretty bad user experience" because of latency; debugging a crash from a screenshot is "totally feasible."

**What are good uses of the event stream?** Stall mentioned an internal competitive Catan simulation that used the events to explain each player's choices. Avilla suggested chat interfaces whose tool calls expand into generative UI, such as rendering a chart, with technical analysis, whenever the watchlist scanner finds something interesting.

**How do you share agents across an organization?** Stall cited a Terraform provider and the console's export features but no one-click sharing. Avilla said most customers build a product layer instead: dragging a card on "very popular project management tool['s]" Kanban board, for example, can start a managed-agent session or flow behind the scenes.

**How do Claude Tag and flows relate?** Claude Tag, a Slack bot that connects a team's information, is "a slightly orthogonal product surface," Stall said; you could build something like it with flows. Avilla said the seams between Anthropic's products will keep disappearing, and that Claude Tag is "a huge daily driver" internally. At the end, asked how Anthropic works, the speakers said they are "very like Claude Tag pilled": it reviews Jira activity from the past day and proposes ticket changes, and "if you just say at Claude like every … day at 6:00 a.m. like do this … it would work out of the box."

**When should you use ultracode and when flows?** Ultracode is for sitting at a laptop working in Claude Code interactively; flows are for something repeatable, triggered through an API from any product. "We actually took the Ultra Code … abstraction or methodology, and generalized it into what flows is on the Claude Managed Agents API."

**Who is liable if something malicious happens?** "Unfortunately, I am not a lawyer." Like any platform business, it calls for "defense-in-depth": every layer of the stack should do its best. The session ended there.

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=c415d432-5b6f-4b4a-9d2c-2e02cfde99db) · [Video](https://www.youtube.com/watch?v=kiClTcJUnhc) · [Talk page](https://ai.engineer/talks/kiClTcJUnhc) · [Multiagent orchestration docs](https://platform.claude.com/docs/en/managed-agents/multiagent-orchestration) · [Workflow runs docs](https://platform.claude.com/docs/en/managed-agents/workflow-runs) · [Claude Platform release notes](https://platform.claude.com/docs/en/release-notes/overview) · [Claude Code dynamic workflows](https://code.claude.com/docs/en/workflows) · [Scaling Managed Agents: Decoupling the brain from the hands](https://www.anthropic.com/engineering/managed-agents) · [Auto mode is now the default in Claude Code](https://claude.com/blog/auto-mode-default-in-claude-code) · [API credits for Max and Team plans](https://platform.claude.com/docs/en/about-claude/api-credits-for-subscribers) · [Latent.Space report on the workshop](https://latent.space/p/claude-dynamic-workflows) · [Larry McVoy, "A solution for growing pains" (1998)](https://lkml.rescloud.iu.edu/9809.3/0957.html)

This write-up is based on the official video captions and the sources above; there are no slide photos for this session, so slides are described from what the speakers said. The [precise notes](precise/10-12-0900-orchestrating-claude-managed-agents.md) (Chinese) keep timestamps, the full list of questions and where the talk and the documentation disagree.
