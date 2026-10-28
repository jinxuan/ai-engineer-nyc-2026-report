**English** | [中文](zh/10-12-1315-beyond-done-building-verification-loops.md)

# An Agent Isn't Done Until It Can Prove It

*At AI Engineer NYC 2026, Nick Nisi and Zack Proser of WorkOS argued that the most valuable thing to build for coding agents is an app-specific tool that lets them check their own work, so that a failure drives the next fix instead of landing on a human's desk.*

**Workshop · Monday, October 12, 2026 · 1:15–2:15pm · Sutton Room (NY Ballroom W)**
**Speakers:** Nick Nisi, Engineering Manager, WorkOS; Zack Proser, AI Engineer (Applied AI team), WorkOS

---

Nick Nisi opened with an incident many in the room had seen on social media: an agent "just started deleting production databases. And in 9 seconds, everything was gone." Nisi didn't name the company, but the details match the PocketOS incident from April 2026, in which a Cursor agent running Claude Opus 4.6 wiped a production database and its backups with one call. The workshop's aim, Nisi said, was to prevent that kind of failure "while still trusting your agents and not having to look at every line, even look at every PR," and to "build that trust over time."

Proser added that verification is not only about preventing errors: "this is also … foundational to achieving true scale with agents."

Nisi introduced himself as an engineering manager who has not written code since July 30, 2025: "that's the day I got back from Laracon and I saw PHP and I was like, 'Never again.'" Proser works on WorkOS's Applied AI team. The workshop ran from a repository they called the "workbench," published at talks.workos.cloud/beyonddone (it now sits behind a WorkOS sign-in). According to Nisi, it holds the slides, a step-by-step page for each exercise, a glossary, and an "ask anything" button backed by a retrieval system trained on the deck.

## 1. Why reading the code no longer works

The first way to trust an agent's work is to review its code like any pull request. But agents write code faster than people can read it, Nisi said, so "a signal-to-noise ratio … starts coming into play and we just have too much code to review." Developers who no longer write code also grow less familiar with it. Browser tools such as Playwright can confirm that a form submitted and a success message appeared, and agents can report those results themselves. "You can trust that often times, but not all the time."

The demo app showed the gap. Running `pnpm snack up` (Nisi aliases `pnpm` to `p`) opened the "Midnight Snack Club," a waitlist page. Nisi entered an email, the page said success and listed the address, and a test that only checked the page would have passed. Then Nisi reloaded the page and the list was empty. The UI had updated, but nothing was saved. "We didn't check the other half."

Proser asked for a show of hands. More than half the room was building heavily with agents, and the same people were running several terminals, chat sessions or agents at once. "If this is the 18th … lane of work that you're driving today," Proser said, attention is thin, and that is where subtle bugs creep in: the agent says "the code looks good, it builds, it runs, the test pass, and it's still wrong."

Nisi then quoted Lauren Tan, a "famous potato" on X (the handle is poteto): "If an agent can't verify its own work, nothing else matters." (The original post was not found; the wording is as Nisi read it.) Proser said the field is converging on verification, reliability and evals as foundational, especially as harnesses build in recursive self-improvement: "we may all be maintaining multiple self-evolving code bases eventually." Nisi described trust as "a gradient," not a switch from zero to one. On WorkOS's Applied AI team, Proser said, working in a codebase with "a baseline that can be run and trusted" is "night and day."

## 2. Give agents goals they can check

Telling an agent to "make no mistakes" doesn't work, Nisi said. "You need to give it clear, defined goals to get to and give it a way to observe and check those goals." Every outcome left unspecified gets inferred or made up, and the agent's assumptions may not match yours. The system has to let the agent "observe, understand what you want, observe that, repair that and then recheck that."

Proser called the old approach "tired": one giant PR, one agent session running for three or four days, and repeated requests to "fix this, make it better." What works is decomposing tasks cleanly and telling each agent exactly how it will know it is done. "We have people on our team that are 900 PRs a month and … no incidents and … improving KPIs for the software," Proser said. "I'm almost gardening multiple code bases and letting them self evolve."

Both speakers had built their own tools, and Nisi said that is the right stage to be at: "It's too early to really classify on anything, so experimentation is key." A line from Nisi's own tooling summed up the approach: "status comes from records, not model prose." Nisi wants screenshots, videos, test results and smoke tests, not an agent saying it looks good. "At the end of the day, I don't care about the code as much. I care about the result." Nisi's March 2026 post "Case Statement: Building a Harness," linked from the talk page, describes a similar design in WorkOS's open-source "case" harness: a separate verifier agent, and evidence markers that scripts create only from real test output or Playwright screenshots.

**CTX.** Proser's tool, CTX ("context"), began as a memory layer shared across harnesses (Proser was trying ChatGPT, Claude Code, Pi and others, with "18 or 30 terminal tabs" on an average day). It is a simple web app on Cloudflare, vibe-coded at first, that can also hand secrets to agents without them seeing the credential. The newest experiment is centralized verification. An agent finishes its work and asks CTX over MCP, "Am I done? Please verify it." For a workflow step that requires a pull request, CTX runs a known verifier, which can start as a simple CLI and harden into a reusable piece, that checks through the GitHub API that the PR exists, is healthy and passes all merge checks. Only then does it mark that step satisfied; otherwise it tells the agent what is still failing. In the demo, one gate required "all pull requests were merged," and the sidebar showed the status alongside the repository and PR it had checked. Proser imagined standard verifiers shared across an organization in a library, skills or tooling.

**Riker.** Nisi's tool, Riker, is "an engineering-focused Grok Bot" that takes on a piece of work and then babysits it: fixing tests, answering review comments, and checking that fixes still serve the goal of working software. It builds a check, decides which tools to run, runs them, keeps the evidence, shows it in different ways, and decides when to stop. The point, Nisi said, is to stay out of "the minutia of shipping the final 10%."

## 3. From human checks to a deterministic CLI

Proser asked how developers verified a web app before LLMs. The room listed refreshing the page, opening a new tab, checking API response codes, checking local storage or other persistence, reading console logs and network traffic, reading what's on the page and interacting with it. Those checks were part of the job when developers were "hand chiseling code." Early LLM use replaced them with "three paragraphs of text. Go execute these instructions." The difference between the two, Proser said, is the agent's ability to verify that it did what was asked in a real application without breaking anything. Without that, "you're just getting the confabulations that LLMs are designed to give you."

An agent can't do any of this from a terminal alone; it needs tools and instructions on what to prove. Nisi walked up a ladder of prompt instructions, from "always verify the change in a browser" ("whatever that means") through "remember to reload and check," "check that the data persists" and "look at the console for errors." Many skills and saved prompts work this way, but anything forgotten or changed is left to interpretation. A better option is a script that runs "a deterministic check across a number of things" and feeds its output back, so the agent can loop until there are no errors.

Proser added that a script is also uniform. The same prompt given to 30 agents produces varied results; one command backed by bash calls does not. Developers like Lauren Tan, "achieving thousands of PRs ostensibly a month," have said this was the first thing they focused on and what "actually unlocked the velocity." Proser's advice: if your web app doesn't already have "a bespoke CLI that allows any agent to fully flex the functionality that you care about verifying," you will have "a much rougher time" relying on prompts alone.

The workshop's version of that CLI was `snack`. Nisi said it is not meant to be reused, only to serve as a template for building one for your own app:

| Group | Commands as described |
| --- | --- |
| Health | `up` and down; `doctor` (is the database running, is there a network connection) |
| Drive | `join` (join the waitlist), `reload`, `reset` (clear all data) |
| Observe | `snapshot`, network, `storage` (local storage), console logs, `screenshot`, run evals |
| Prove | `prove` (a deterministic check) and `judge` (a "system one" model that judges whether the result is right without needing an exact match) |

Proser said the gains from adding such a tool go to the whole team, which can ship "significantly more PRs significantly faster and significantly safer." Nisi went further: the way you verify an app "might even be more important than … the actual application." With verifiers in place, an app can be rewritten in another language and proven to work, as the Bun team did when it moved to Rust (Bun's LLM-assisted rewrite from Zig was merged in May 2026) and as TypeScript did with Go. "If you have all of those verifiers in place, then your source code doesn't matter as much."

Tools give an agent its body, Nisi said: "The LLM is the brain," and tools are how it reaches out and acts on the computer. In the demo, `p snack snapshot` returned the page as JSON, and after Nisi added an address it appeared in the snapshot; `p snack screenshot` captured the page. The end goal is "proving back to the human": screenshots and videos attached to the pull request, which the GitHub CLI can now do. Proser added Chrome's debugging tools: "Don't guess and … spin your wheels for 3 hours. Like actually figure out which part was causing the render to be slow." What matters, Nisi said, is "what a user sees": whether the page renders correctly "in this browser at this width with dark mode, with light mode," checked in a loop as many times as needed.

Nisi then showed the sequence an agent could run on its own: `doctor`, join `ada@example.test`, check storage (the address is there), reload, check storage again (empty), and confirm with a snapshot. The agent learns "I'm not actually persisting this" before it ever reports back.

## 4. Make everything feedback-loopable

Nisi pointed to a post from the Amp team (Nisi couldn't recall the title) with an example of a ball bouncing inside a circle on a canvas, where a slit in the circle lets the ball escape and end the game. The model can't see the canvas. But if the program prints the ball's coordinates on every frame and the ball jumps from one side of the screen to the other, a move of more than one pixel per frame signals a bug the model can find. The idea is "making things feedback loopable." Once that exists, Proser said, you can spin up 30 variants of a feature against the same eval and pick one, and if tokens keep getting cheaper, teams may start the week by asking for "35 versions of this new feature."

Proof can't be generic, Nisi said; it has to be specific to your application. For the snack page, `prove` uses Playwright: confirm the waitlist is empty, fill in an email, click the "join waitlist" button, expect the status element to say "waitlisted," and check that its text contains the address. Anyone who has written an end-to-end test with Playwright or React Testing Library has done this. Handed to an agent as a tool, it becomes reproducible evidence and a regression check that runs whenever the page changes.

Nisi added a caution: "A successful screenshot isn't proof." The screenshot must show the expected state reached by actually running the steps; otherwise "it might just be like another lie," produced by a path that isn't possible in the UI or with side effects.

Once such loops are durable and repeatable, Proser said, "a failure starts to drive future work." An agent that fails retries and verifies again while you are busy elsewhere, and you don't come back at the end of the day to push a PR the rest of the way.

Nisi asked who writes good error messages. Almost nobody raised a hand. They matter more than ever, Nisi said, because the best reader of an error is now an agent: make failures explicit about why they happened and how the app got there, and the agent can heal itself.

Proser stressed that the goal is not to wrap determinism around the model. Agents are capable and should be creative, and "constraints can … equal freedom." That is why you give them the outcome, which Nisi is exploring under the name "outcome-driven development." Being overly prescriptive "can actually lead to lower performance in some cases"; instead, "you must get here. You'll know you're here when this command returns this." Nisi said the latest frontier models, among them Opus 5.5 and Fable, are good at finding their own path: given a destination, they may wander, catch more edge cases or ask a question that unlocks something, while a fixed route keeps them from straying. That, Nisi said, has become practical only with "the latest batch of frontier models." Nisi framed the loop as red-green-refactor: "Verify this, reproduce it, show me … fix it, and then show me the proof of that." How the agent will show proof matters as much as the fix.

## 5. The moment the agent knows it isn't done

Nisi ran `p snack doctor`, which checked that Node and Chromium were installed and that a session existed, was running and had built. Then `snack join ada@example.test` ("Sorry, we don't type anymore") and `snack storage`, which came back empty. "This is the point at which in the real world … you've just gotten the value out of this entire system," Proser said, "cause at this moment your agent realizes I am not done and I'm not going to drop this work on the floor." A follow-up run of `prove` opened a browser and failed; Nisi said the persistence fix hadn't actually been applied yet.

Proser introduced "receipts": a record of each run, such as how a person handles a support ticket well. Every run an agent makes is valuable data now, because it can be fed to LLMs for fixes, and more valuable later, as self-improvement tooling matures, because it becomes a dataset for comprehensive evals that agents can hill-climb.

## 6. LLM judges and the saboteur game

With persistence fixed, Nisi said, the app could still lie. After a reload the status says "waitlisted," but the screenshot copy says "you're in, your seat is confirmed." Running `p snack judge` failed because "the confirmation text breaks the contract." The question was how to prove the page works without tying tests tightly to its exact text. The answer, Nisi said, is a "system one" model.

Proser explained the borrowed psychology. System 1 is the fast, automatic correction when someone swerves at you on the road; System 2 is "I need to figure out what I'm going to do with my life. So, I'm going to go book an Airbnb and like think about it for 3 days." For LLMs, "now we've got Jev and then immediately after we've got Clef from Cloudflare," Proser said, "even faster." Jev is TypeSafe AI's "System One model," launched September 15, 2026, which returns typed decisions (yes/no, a choice, a score) with confidence rather than text. Cloudflare's Clef models were released later (October 1, according to secondary reports, not two days after Jev). Nisi gave a simpler version: asked what color a light is, you say "yellow" immediately; asked why it is yellow, you start thinking.

Then came the game. A saboteur plants a random bug in the snack app (`pnpm snack up --bug random`), and you, the detective, use only `snack` to find it, name it and prove it. The live run didn't go to plan. `doctor` reported that the last receipt had failed but pointed to "proof … for code that no longer exists," left over from an earlier run. Someone in the audience suggested a wrong email; Nisi suspected the port, then decided the port was fine. "I need some verification tests around my … contrived verification examples," Nisi said.

Nisi listed the possible outcomes of a round: the detective named the bug with output; said it works but it didn't; said it works and it did; blamed a bug that wasn't there; or found the right bug with no evidence. The planted bugs included storing data only in memory, saving the wrong email address, losing the confirmation, over-promising (which the screenshot or judge can catch), a console error that appears in the receipt but not the console, and no bug at all, "and that's a real answer as well." Every miss, Nisi said, is "a new line that can go in the map."

Scoring will differ by team, Proser said: "The most important thing to do is to start and to create one, create an eval baseline and to use it consistently and to evolve it together as a team," until every PR exercises it and shows whether a change is safe to land.

## 7. See everything a developer would see

Proser summarized the workshop as moving what developers used to do in Chrome late at night into a deterministic CLI. If your web app doesn't have "a custom verifier CLI," that is "one of the highest leverage things you could start with," along with a foundational eval that shows "what good looks like," and access to performance, network traffic and rendering through Chrome DevTools, or equivalent projects for iOS and macOS apps. "You need to give your agent the ability to see absolutely everything that you would normally see as a developer."

Nisi added that the agent also needs intuition about when to use each tool, based on how you would reproduce a bug yourself. At WorkOS, much of it is sign-on: "Is it properly setting a cookie? Is it setting an expiration?"

Proser came back to receipts. For an e-commerce app, that means the whole user journey: did the user see an error, did the catalog take 7 seconds to appear, was the cart abandoned. A system running overnight could aggregate them, notice "eight failures in the caching module," open a PR and prove the fix. Teams shipping at high volume also attach a recording of the agent walking through the app, so a reviewer facing "15 important PRs" in the morning can watch it first. When self-evolving tooling becomes the default, Proser said, teams sitting on clean datasets will be able to move "significantly faster" than competitors without them. Nisi closed the section: unit tests, evals and smoke tests that the model can run let a team "climb that evidence ladder," and that evidence "really becomes the spec that shows what our app does."

## Q&A

The workshop took questions twice mid-session and once at the end. Most questions were not audible on the recording; they are reconstructed here from the speakers' answers.

### Determinism and scope

**Are you using models to write JSON to make the code deterministic?** Nisi said yes, with unit tests around it to catch edge cases and confirm the output really represents what a system one model should return. Nisi keeps those tests so the check doesn't drift over time.

**Does this work beyond the front end?** Proser said the team builds mostly TypeScript full-stack apps deployed to Cloudflare, and "it works perfectly," including evals focused on the back end or database operations. Nisi said that is why they aren't handing out a tool: "You kind of have to build up the tools that fit the app." Proser added that evals will "wildly differ" by business, from interior design to domains where only speed matters.

**Do you need an API key for the exercise?** (Inferred from the answer.) "For that one, yeah, you do," Nisi said. Proser said the tools make it optional, so every exercise runs without one, and the calls are cheap. Nisi said new accounts get $5 free, "and I think that'll last you like 3 years."

### Making the checks run

**Skills depend on the model choosing to use them. Shouldn't CI enforce the checks?** Nisi listed options: CI; a git hook, "probably the last one I would reach for"; a Claude Code hook; or a Pi extension running in the background. The easiest way to reach everyone, Nisi said, is to "just put an AGENTS.md, not a CLAUDE.md" in the repo that says, for example, verify by running this on every significant change or before opening a PR; people "have to go out of their way to ignore that." Proser added that hooks can be shared across the team for local development.

**What's the benefit of running checks locally rather than in CI?** Nisi said CI is part of it, but the real gain is the fast local loop: give the agent a destination and a way to verify it. That lets Nisi run several agents in parallel, three that day ("a very low day for me, but I'm at a conference"), normally 10 to 15, without coming back to an agent that says it's done when it isn't. Proser described the goal as returning at the end of the day to every lane started that morning and finding it correctly finished, without having to "turn by turn nudge" any of them. Nisi relies on CI and automated review: the team uses Greptile, and "if it gives me a five out of five and all of my CI is passing I feel extremely confident." The point of verification is reaching that state faster and with fewer turns, and keeping the agent on track, since an agent could add "10,000 lines of absolutely nothing" that pass review and change nothing.

### Production

**How do you trust agent changes in production?** (Inferred from the answer, which the captions don't attribute clearly; it appears to be Proser's.) The answer suggested deploying to a preview environment first, verifying there, then going to production and monitoring immediately. A pattern emerging for mostly agent-built deployments is at least one agent dedicated to the rollout, watching metrics, Datadog and feature flags, and able to say "this is actually bad and we're reverting it." "I truly understand and get the … queasiness around it, … but … it seems inevitable." The speakers then pointed attendees to WorkOS's booth 6 in the expo hall.

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=edb4f982-0cb8-48ee-8bf7-410255a40cb3) · [Video](https://www.youtube.com/watch?v=q4WC4TowkBo) · [Talk page](https://ai.engineer/talks/q4WC4TowkBo) · [Workshop workbench](https://talks.workos.cloud/beyonddone) (sign-in required) · [Case Statement: Building a Harness](https://nicknisi.com/posts/case-statement/) (Nick Nisi) · [Agent Experience: Build without leaving your terminal](https://workos.com/blog/agent-experience) (WorkOS) · [Introducing System One models and Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev) (TypeSafe AI) · [Rewriting Bun in Rust](https://simonwillison.net/2026/Jul/8/rewriting-bun-in-rust/) (Simon Willison)

This write-up is based on the official video captions and the sources above; no slide photos were taken at this session, so slides and demos are described from what the speakers said. The [precise notes](precise/10-12-1315-beyond-done-building-verification-loops.md) (Chinese) keep timestamps, caption corrections and the list of unverified items.
