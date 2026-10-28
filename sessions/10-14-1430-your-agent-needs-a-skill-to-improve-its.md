**English** | [中文](zh/10-14-1430-your-agent-needs-a-skill-to-improve-its.md)

# Score Every Agent Conversation, Then Let an Agent Fix the Skills

*At AI Engineer NYC 2026, Warp's Ben Holmes laid out a loop in which agents grade past coding-agent conversations against Markdown rubrics, collect the failures and propose edits to the skills that caused them, and demonstrated it with Warp's Skill Doctor.*

**Breakout, Evaluations track · Wednesday, October 14, 2026 · 2:30–2:48pm · Turing Room (Riverside Ballroom)**
**Speaker:** Ben Holmes, DevEx Lead, Warp

![The opening whiteboard](../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/01.jpg)
*The talk ran on a tldraw whiteboard: "Hi, I'm Ben, DevEx Engineering Lead at Warp. Today we're talking about self-improving agents."*

---

Ben Holmes gave the talk from a hand-drawn tldraw canvas rather than slides, switching to the Warp terminal for a demo. The subject, Holmes said, was "building self-improving agents. Agents that look through their past work and figure out how to improve their own setup."

Holmes works on Warp Factories, which Holmes described as "open and flexible infrastructure to help you all build software factories of your own." The talk set the larger picture aside to focus on one piece of it: having agents introspect the work they do inside such a system and suggest improvements, so they perform better over time.

The session abstract opened with a different example from the one Holmes used on stage: "We just found out our orchestration agent was wasting hundreds of tool calls this past month. The reason? One bad instruction in our message passing skill." That story does not appear in the recording; the talk used a related example about an agent hunting for port numbers, described below.

## 1. Two loops: improve the product, improve the factory

![Inner loop: improve the product](../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/03.jpg)
*The inner loop: work moves from assignment through triage, spec, implementation, review and shipping to monitoring.*

A software factory, Holmes said, means automating the software development life cycle. Some draw it as an assembly line of agents handing work off; Holmes drew it as a wheel labeled "Inner loop – improve the product": new work assigned, triage, spec writing, implementation, code review, ship, monitor usage, and back to new work. (One box between implementation and code review was blocked from view in every photo, so its label is not known.) Work starts from a task list, a board or Slack messages. Agents research or triage it, perhaps write a spec with a human, implement the change, use computer use to take screenshots or recordings so the result can be checked, and run "rounds of adversarial code review" before it ships and is monitored.

"But the more interesting part of this thing is all of the sort of meta engineering that goes around making that wheel run as efficiently as it can," Holmes said. Warp calls it **factory engineering**: "putting in engineering work to improve the system that is driving the agents themselves." That could mean improving the skills that drive the agents, or improving code architecture and running static analysis so agents work on the right parts of the codebase (Holmes mentioned SonarQube and other sponsors working on this). This talk's loop targets one thing: agent skills and configuration.

![Outer loop: improve the factory](../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/04.jpg)
*The outer loop wraps the core factory loop: review, score, isolate failures, propose improvements.*

The second drawing, "Outer loop – improve the factory," wrapped a red dashed circle around the core factory loop. Its four steps were: review agent conversations, score quality, isolate failures, and propose improvements, the last marked with an alarm clock and "Every 24 hr." Beside "score quality" sat a box of scoring rubrics with efficiency and code quality checked and test coverage crossed out. Beside "propose improvements" sat a diff card for `test-writing/SKILL.md` (+57 −20) adding a "Coverage pyramid" section on "when to add unit vs. integration tests." A red note at the bottom was partly blocked from view.

Holmes walked around the loop. Agents review their past conversations. Scoring agents grade how the conversations went: "think of this like an LLM as a judge, reviewing against rubrics that you define," on metrics such as code quality or test coverage. Once many conversations across the team's work are graded, "you can find all the failures, figure out reasons that those failures are happening, and then maybe an agent wakes up on a cron to suggest improvements." Then evaluation continues, to see how the change performed. The rest of the talk took the segments one at a time.

## 2. Get every conversation into one place

![Where agent conversations live](../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/06.jpg)
*Local agents store transcripts in `~/.claude`, `~/.codex` and Warp's conversation storage.*

The first step is getting all agent conversations in one place so an agent can reason about everything the team is doing. Local agents such as Claude Code, Codex or Pi keep their sessions on each machine; the whiteboard listed `~/.claude/**jsonl`, `~/.codex/*.jsonl` and Warp conversation storage. That is enough to optimize "your single player setup." Holmes had talked to teams that built an S3 bucket and an automation to zip up everyone's `.claude` folders and upload them, "just so we have everyone's conversations in one place."

With a software factory, Holmes said, centralizing "kind of comes for free," because agents run on shared machines rather than individual laptops: either a long-running compute instance or isolated sandboxes started for every run. In Warp's factory setup, each agent runs in an isolated Docker sandbox. Any cloud runner that all tasks flow through puts every agent trace in the same place.

![What a trace needs to contain](../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/07.jpg)
*Three things to capture: the transcript, the tool and MCP calls, and the code output.*

The conversation text alone is not enough. The traces should also hold the metadata: tool calls, file-system calls and MCP calls, "everything that connects the agent to the outside world." And they should hold the outputs: the code diffs inside the conversation, or the actual pull request if you pin the commit. Holmes summed it up as three things: "the transcript of what was said," the tool calls, "and then you have the output."

## 3. Score quality with an LLM judge

![Three example scorers](../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/10.jpg)
*Three scorers under "score quality": redundant tests, verbosity and efficiency.*

With the data in place, "you can have an agent start scoring the quality." Holmes drew three example scorers and introduced each with a show of hands.

**Redundant tests.** "How many people in here feel like their agents write too many tests?" About 40% of the room raised a hand, Holmes said; "On our team, it was about 100%." It depends on the model and the instructions, and teams hold different views on end-to-end versus integration versus unit coverage. A scorer can evaluate how often the agent writes too many tests in each conversation.

**Verbosity.** "How many people feel that their agents write too many comments?" "There's about everyone." Holmes declined to name the model responsible. This matters because "code review is kind of the biggest bottleneck right now": how an agent summarizes its changes and what comments it leaves in the code can stand in the way of getting a change reviewed quickly. A verbosity scorer checks whether the agent is overly verbose when talking to you or when writing code.

**Efficiency.** The third looks at tool calls and MCP calls. The longer agents run without a human in the loop, perhaps in a cloud sandbox where nobody watches the conversation, the easier it is to miss failed tool calls. "These agents are very persistent and will work around whatever roadblock that they run into, and they won't tell you about it. They'll just tell you that it worked." Holmes gave an example from Warp: in their test harness, the agents' skills carried many bad instructions, but the agent worked around them without saying so, trying port number after port number to figure out its environment until it succeeded. "The most that we knew was like, oh, the agent takes like two minutes longer than maybe we expect. But under the hood, there's actually a lot to be improved."

## 4. What a scorer looks like

"It's just markdown, as you would probably expect," Holmes said. In Warp's setup a scorer is "like a specialized skill definition" that defines what an LLM should score. Holmes opened `efficiency.md` from the `skill-doctor` skill in Warp's `common-skills` repository.

![efficiency.md: description and labels](../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/12.jpg)
*The top of the efficiency scorer: a one-line description and four labeled grades.*

The file's description reads: "Whether the agent worked directly toward its result, or wasted effort on redundant steps, avoidable rework, or unnecessary back-and-forth." It then defines four labels (descriptions as far as they were legible in the photo):

| Label | Score | Description |
| --- | --- | --- |
| `highly_efficient` | 1 | "The agent took a direct path: nothing re-read or re-run, independent steps batched…" |
| `mostly_efficient` | 0.8 | "The agent slipped once or twice: a duplicated read, an early retry, or a small correction…" |
| `mostly_inefficient` | 0.4 | "The agent wasted effort repeatedly, or caused a round of rework an earlier check would have prevented." |
| `highly_inefficient` | 0.2 | mostly hidden; mentions "the same defect reworked" |

The labels define what good and bad look like on that criterion, Holmes said, likening them to the rubric a high-school teacher hands out for a five-paragraph essay. You decide where pass and fail divide: "In our case, maybe the bound is halfway through this list. So if it's a 0.4 or 0.2, that's kind of a failing grade. Something went wrong in that conversation." The skill's published `SKILL.md` agrees: only conversations with raw scores below 0.5 count as failed, and only those feed skill suggestions.

![efficiency.md: what to assess](../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/14.jpg)
*The scorer judges against what a competent engineer with the same tools would have needed.*

![efficiency.md: the Assess list](../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/13.jpg)
*The list of common sources of waste.*

Below the labels, the rubric tells the judge to score against what a competent engineer with the same tools would have needed, not against what the agent managed. "A mistake that looks unavoidable in context still counts if better tooling, a skill, or a check would have prevented it — name that cause in the reason." The bullets that follow are "common sources of waste, not an exhaustive checklist":

| Source of waste | What the rubric says (excerpted) |
| --- | --- |
| Rework from mistakes | Work redone because the agent got it wrong the first time: a test or build failure a local check would have caught, edits to the wrong file, a misread requirement later reverted |
| Cost to the human | Repeated correction or steering from the user is the most expensive waste; a question asked up front is cheap, the same question after building the wrong thing is not |
| Information gathering | Re-reading, re-running or re-searching for something already found; reading a large file end to end when a targeted search would answer it |
| Routine-step overhead | A roundabout way of doing a standard, repeated part of the agent's job; weighted beyond its one-run cost, because it recurs in every future conversation until a skill or rule fixes it |
| Batching | Independent reads, searches or workstreams run serially across turns instead of together |
| Flailing; verification timing | Partly hidden in the photos |

Holmes's advice on writing these: "You should keep this as succinct as possible and just build it up as you go." A rubric can start as a few bullet points that you test yourself.

## 5. Grade each conversation, then isolate the failures

![One session fanned out to three scorers](../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/16.jpg)
*A session file is sent to each scorer; results land as pass or fail.*

The next drawing sent one session file, `update-auth-service.jsonl`, to three scorer files (`redundant-test.md`, `verbosity.md`, `efficiency.md`), with a pool of sessions below and a grades box of PASS and FAIL.

Holmes's hypothetical: an agent built a full-stack implementation of an auth service, the PR was merged, and now an agent grades how that conversation went, with "one agent run per conversation." You can grade a random sample or every conversation. Walking down the scorers, the grader might find that on tests the agent wrote many end-to-end tests when the team's guidelines prefer another kind, so it fails for not following instructions. On verbosity it wrote few comments and was easy to follow, so it passes. On efficiency it spent a lot of time exploring the test environment to work out how to test the change, when it should have discussed that up front, so it falls below the line.

The usual trigger, Holmes said, is a PR merge: an agent then reviews that conversation against all the criteria. Over time each record accumulates a pass or fail per criterion plus an agent's explanation of "why one of those failures or passes happened," which becomes context for the agent that decides what to improve.

![The full outer loop, step by step](../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/17.jpg)
*Sessions scored, failures isolated, improvements proposed.*

The full chain on the board ran from reviewing conversations through scoring to isolating failures (sessions such as `add-chat-mvp.jsonl` and `add-sentry-webhook.jsonl` marked fail or pass) and proposing improvements. In Warp's setup, conversations are collected as soon as they happen. A cron job runs every 24 hours over a sample of the past day's conversations ("maybe there's been like 200"), filters down to the failures, and hands the failure reports to a self-improvement agent with an instruction along the lines of "these were the 50 conversations that failed. Here are the notes that were left on each conversation. Go ahead and find commonalities," plus access to the skills repository and the codebase. (The 200 and 50 were illustrations, not measurements.) The coverage-pyramid diff on the outer-loop drawing is the kind of change that comes out.

## 6. Demo: Skill Doctor

"This is going to be the easiest one that you all will get to try," Holmes said, introducing **Skill Doctor**, available from Warp's site. It is a skill for a single-player setup: it crawls all of your agent conversations, from Claude, Codex or others, and runs a one-time evaluation.

![Running /skill-doctor in Warp](../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/19.jpg)
*An orchestrator agent and three scorer subagents reviewing Codex sessions.*

The photo of the run shows the command `/skill-doctor Review the codex sessions for hubble.md stored in …/Documents/hubble-md-codex-sessions`, with tabs for an orchestrator and three scorer agents (scorer-a, scorer-b, scorer-c). The orchestrator read the skill's list of supported harnesses and listed the session files. Skill Doctor "grades in batches for a little bit of token efficiency," Holmes said, and a self-improvement agent then reads the scores and writes a report.

![The Agent Skill Report](../slides/10-14-1430-your-agent-needs-a-skill-to-improve-its/20.jpg)
*The report card from the sample run: a B, 83 overall.*

The report, titled "Agent Skill Report" and generated October 14, 2026 at 4:09 PM UTC with harness "codex," gave an overall grade of B:

| Metric | Score |
| --- | --- |
| Efficiency | 73 |
| Code quality | 75 |
| Procedure compliance | 80 |
| Verbosity | 100 |
| Skill coverage | 100 |
| **Overall** | **83 (B)** |
| Conversations scored | 12 |
| Skills installed / used | 19 / 16 |

The overall figure follows the weighting in the skill's `SKILL.md`, 25% each for efficiency and code quality, 20% for procedure compliance and 15% each for verbosity and skill coverage, which works out to 83. Per the same file, skill coverage is not a rubric score but the fraction of sessions in which an installed skill was detected, and code quality is scored only on conversations that changed code. The findings section was mostly hidden in the photo; legible fragments mention agents pushing to the wrong PR (pushing to origin instead of a contributor's branch without asking) and a changelog skill.

"This is one shot," Holmes said; it is better run regularly, but "as a first go, you can just see what you can optimize right now." In the sample, the self-improvement agent read the scoring agents' notes on the weaker metrics and proposed two changes. The first matched the earlier example: the agent kept hunting for a free port on the test machine without understanding how the environment was set up, so it proposed changing the end-to-end testing skill to give better instructions for finding a port, "instead of just banging against the wall until it got something right," saving tool calls. The second came from Holmes constantly rewriting the agent's changelog entries; it rewrote the changelog skill with more prescriptive language so that back-and-forth would stop.

"These were things that I could have gone and found myself," Holmes said, but the port problem in particular was useful to surface automatically, because the agent's process usually sits collapsed behind a toggle or runs in the cloud where nobody watches it closely.

Warp's Skill Doctor page says it supports Claude Code, Codex and Warp, runs as `/skill-doctor`, and uses subagents to "score these conversations against tested rubrics for efficiency, code quality, and skill coverage." The `SKILL.md` adds that everything runs locally ("Never upload transcripts, session files, or any excerpt of them anywhere"), that it samples up to 12 sessions by default from the past 45 days, and that proposed changes go to a `proposed/<skill-name>/SKILL.md` with a diff rather than editing the user's real skill files, all rendered into a self-contained `report.html`.

## 7. Running it continuously: a score dashboard

Holmes meant to end by showing how to run the loop continuously, but the demo failed ("Oh, shoot … No!"), so Holmes described it instead: a set of charts of scores over time.

Holmes contrasted this with memory systems. Memory also has agents look back over conversations, but "it's very invisible what the agent actually decided or what takeaways that it's got." Scoring gives you a dashboard. Plot the efficiency score over time, say averaging 50–60%, and mark the changes you made to the agent setup; then you can see "there was an inflection point here, this is when I merged the skill." Not only do the agents improve themselves, Holmes said, "I actually know that when I merge that skill that it suggested something real happened on the other side."

The recording ends mid-sentence in the closing remarks: "By having scores in the loop with self-improvement, you can get…"

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=0a0cbd67-23fa-4a3d-a8dc-06e3caa3ab2f) · [Skill Doctor](https://www.warp.dev/skill-doctor) (Warp) · [skill-doctor SKILL.md](https://github.com/warpdotdev/common-skills/blob/main/.agents/skills/skill-doctor/SKILL.md) (`warpdotdev/common-skills`)

No video of this session had been published at the time of writing. This write-up is based on the author's room recording, which stops during the closing remarks at about 17¾ minutes, photos of the whiteboard and demo, and the Skill Doctor page and `SKILL.md` above. The recording ends without a Q&A; whether one followed is not known. The [precise notes](precise/10-14-1430-your-agent-needs-a-skill-to-improve-its.md) (Chinese) keep timestamps and the full list of unverified details.
