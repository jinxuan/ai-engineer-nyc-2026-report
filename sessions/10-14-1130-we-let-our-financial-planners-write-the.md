# At Range, the Financial Planners Write the Agent Skills, in Five Steps and No Code

*At AI Engineer NYC 2026, Range financial planner Matt Ragone described how planners with no engineering background built and now maintain the agent skills they use every day, and why the open questions are about ownership and testing rather than model capability.*

**Breakout · Wednesday, October 14, 2026 · 11:30–11:48am · Turing Room (Riverside Ballroom)**
**Speaker:** Matt Ragone, CFP®, CRPC®, Financial Planner, Range

![Title slide: We Let Our Financial Planners Write the Agents](../slides/10-14-1130-we-let-our-financial-planners-write-the/01.jpg)
*The title slide.*

---

Most talks at the conference came from people who build agent platforms. Matt Ragone presented himself as the other side. "I'm not an engineer. I'm a certified financial planner," Ragone said in an online version of the talk recorded for the conference. Ragone leads AI adoption for the financial planning team at Range, a wealth management firm, and the talk was a build guide: how the planners came to write the agent skills they use daily.

Ragone gave the talk twice. An online version, about nine minutes long and read close to a script, was published before the conference. The in-room talk on October 14 used the same slides and structure but ran about 13 minutes and was delivered off the cuff, with examples that are not in the online version. This write-up follows the in-room talk. The author's room recording starts about two minutes in, so the opening slides are described here with wording from the online version, which is marked as such.

## 1. Not a vendor talk

![About me: I'm a financial planner, not an engineer](../slides/10-14-1130-we-let-our-financial-planners-write-the/02.jpg)
*"This isn't a vendor talk": a method from the people AI engineers hand their work to.*

The "About me" slide made two points: Ragone leads AI adoption "for the financial planning team at Range, a wealth management firm," and "This isn't a vendor talk. It's a method, from the people AI engineers hand their work to." In the online version Ragone put it this way: "Most agent talks you'll hear come from the people who built the platform. This talk comes from someone on the other side, the people you hand the platform to. We work in a regulated business. The output is financial advice to real clients. And the review step is not optional."

![Where everyone starts: AI as a chat box](../slides/10-14-1130-we-let-our-financial-planners-write-the/03.jpg)
*The starting point: polishing client messages.*

Range started where most firms start, with "AI as a chat box," used for "tightening up client messages. Useful. But the change that mattered came later." In the online version, Ragone described pasting a client message in and asking for it to be "a little clearer or friendlier": "I was still doing all of the work manually. I was just doing it a little bit faster."

## 2. The bottleneck was never typing

![A planner's constraint was never typing speed](../slides/10-14-1130-we-let-our-financial-planners-write-the/04.jpg)
*"It's that we can only be in one place at a time."*

The slide read: "A planner's constraint was never typing speed. It's that we can only be in one place at a time." In the room, Ragone said that whether a planner types "300 words per minute, 200 words per minute," the work is still manual, and a 30- or 45-minute call with one client is time no other client gets.

![What changed: the work happens while I do other work](../slides/10-14-1130-we-let-our-financial-planners-write-the/05.jpg)
*Three steps: a call with one client, an agent drafting for another, then review.*

The slide listed what a day looks like now: (1) "I'm on a call with one client"; (2) "An agent drafts a full cash flow or insurance analysis for another"; (3) "The call ends: I review, adjust, and send."

In the room, Ragone named the turning point: "late January, early February," when "Claude Cowork completely changed the game." While on a call, Ragone can have a Cowork agent "completely draft a cash flow analysis, an insurance analysis, a tax review." (The slide mentions only cash flow and insurance.) When the call ends, the planner has to "review, adjust, and really see what our agent built so that I can make sure it is bulletproof before it gets sent out." Press reports put Claude Cowork's research-preview release in mid-January 2026; the dates Ragone gave may be when Range started using it.

## 3. It takes more than a good prompt

![Getting there: It takes more than a good prompt](../slides/10-14-1130-we-let-our-financial-planners-write-the/06.jpg)
*Two conditions: work where the work lives, and know how the firm does it. "That's what a skill is."*

Two things had to be true, the slide said. First, "Work where the work lives: in our systems, on our clients' real data." Second, "Know how we do it: not how financial planning generally works." Together: "That's what a skill is."

On the first point, Ragone explained in the room that Range's clients, whom it calls members, connect all their financial data to the platform: insurance documents, investments, real estate, mortgages. "We have their entire picture fully connected." The agent has to know which data to pull and which not to. On the second, the agent "can't just be delivering generic recommendations because we might be a little bit more conservative than what Claude Cowork or any sort of other agent would like to deliver."

Range was "ahead of this curve, luckily," Ragone said. "Our team is about 20 planners." Before Cowork arrived, the team had standardized how it delivers insurance, investment and retirement recommendations, because planners came from different backgrounds, from large wealth management firms to small RIAs. Those reference documents lived in Notion pages and Google Docs. When Cowork arrived, instead of reinventing the wheel, the instruction was simply, "Claude, just reference this document as part of your review process before you kick off any of your workflows." That "actually really saved us so much time."

## 4. Five steps to build a skill

![The build guide: How to build a skill](../slides/10-14-1130-we-let-our-financial-planners-write-the/07.jpg)
*Pick, Teach, Pack, Test, Ship. "None of these steps require writing code."*

The build guide had five steps: Pick, Teach, Pack, Test, Ship. "None of these steps require writing code," the slide said. In the room Ragone added that each "requires knowing the work really well. So that's why the people doing the work should be the ones building it." Ragone used "skill" and "agent" loosely: the steps are for building "a skill or agent, they're pretty much interchangeable I feel."

### Step 1: Pick

There was no photo of a slide for this step. "Pick a workflow you repeat constantly. I'm talking daily, weekly," Ragone said. "Your first instinct is going to be to pick the flashiest thing, the most complex thing. I'm telling you from experience, do not do that." The right choice is work that takes a lot of time but can be automated simply, where "you will immediately know if the output is good, if it's bad." Complex workflows take longer to iterate on. Range started with cash flow analyses and insurance reviews. The online version gave the test in two parts: "You do it often and you know how to judge it."

### Step 2: Teach

![Step 2: Show it one real case](../slides/10-14-1130-we-let-our-financial-planners-write-the/08.jpg)
*"Narrate the judgment out loud: the why, not just the steps."*

The slide said "Show it one real case" and "Narrate the judgment out loud: the why, not just the steps." On stage, Ragone revised the number: "I would end up now probably doing it two, three, four, five real cases." (The online version also says "several.") Alongside the standardization documents, Ragone adds real cases to the skill's references, especially edge cases and cases "where I had to correct the agent's work," so the next run handles them better.

The narration matters most. Ragone's example: when uploading a client's pay stub for a cash flow or insurance analysis, tell the skill why, for instance to check whether there is a deduction for group term life insurance. "You don't have to go crazy with it. But that additional narration has saved us so much time down the road." The online version called this narration "the whole game."

### Step 3: Pack

![Step 3: Pack it into a skill](../slides/10-14-1130-we-let-our-financial-planners-write-the/09.jpg)
*Two things beyond the steps: the questions it must ask, the limits it must respect.*

Once the skill does a good job, pack it. The slide named two things beyond the steps: "The questions it must ask" and "The limits it must respect."

On questions, Ragone gave examples in the room: is this a 2025 or a 2026 tax review, which tax code applies, which state? Some answers can be pulled from the member's Range profile, but others can't: is the member more concerned with homeowner's insurance or disability, and is there a whole life policy to review?

On limits: "We're regulated. Our recommendations have real-life impacts. So every single skill is not allowed to send anything to a member." Everything is reviewed by a CFP, "and honestly, just moving forward, I feel like that's going to be the case." Ragone mentioned that Range has a built-in AI feature called Rai, described as "CFP compliant," but when a member asks for human interaction, a planner still validates the answer. The online version summed up the step: "If you only write the steps, you get something that simply just runs on its own. If you write the questions and set the limits, you can get something you can trust."

### Step 4: Test

![Step 4: Run it 5–10 times on real cases](../slides/10-14-1130-we-let-our-financial-planners-write-the/10.jpg)
*"Before anyone else sees it."*

Before the skill reaches the team, run it five to ten times on real cases, the slide said. In the room, Ragone said the set should include standard cases and some edge cases, and one or two colleagues can help stress-test it. After each run Ragone tells the agent to "take what you learned, repackage that into a skill." And before shipping, Ragone asks one final question: "How could you make yourself better? What would you specifically do?" and updates the skill again. The online version called testing "the step everyone wants to skip," and said five to ten runs is far less work than cleaning up after handing a half-baked skill "to 15, 20, 100 people."

### Step 5: Ship

![Step 5: Ship it with an owner and a reviewer](../slides/10-14-1130-we-let-our-financial-planners-write-the/11.jpg)
*An owner who keeps it current, a reviewer who checks it before the team relies on it.*

Shipping is "not where the work ends. I wish it was that easy," Ragone said. Tax code changes, and a shipped skill needs an owner who keeps it current and a reviewer who checks it before the team relies on it. At a small company the two may be the same person. At Range, if Ragone owns a skill, a colleague named Joe might review it "because he might have a little bit better subject matter expertise [in] insurance." The online version gave the reason for a separate reviewer: "Same as a plan, you don't grade your own work."

## 5. Where Range is now

![Where we are: 6 months, 24 skills, 17 in financial planning](../slides/10-14-1130-we-let-our-financial-planners-write-the/12.jpg)
*"It spread to sales, product and marketing without a mandate."*

| Metric | Value |
| --- | --- |
| Time since starting | 6 months |
| Skills across the company | 24 |
| Skills in financial planning | 17 |

(The slide says six months; in the room Ragone said "six to eight months.") The slide added that it "spread to sales, product and marketing without a mandate." In the room Ragone said planners, marketers, team success and member success had built on each other's work. The next step is an internal agent called Ranger ("we're Range, this is Ranger") that lives in Slack, can work in Range's dashboard to help with member tasks, and is still being built.

Ragone also argued that experts should build the skills in their own area: an insurance specialist will catch things a generalist planner might not. Ragone's metaphor was a quarterback: certified financial planners sometimes "air it out 50 yards," and sometimes hand the ball to the CPA or investment team. For tax skills, Range's CPA and tax team supply the content, and Ragone helps with "how to build the actual skill."

## 6. The bottleneck has moved

![Why this matters now: The bottleneck has moved](../slides/10-14-1130-we-let-our-financial-planners-write-the/13.jpg)
*What is settled and what is still open.*

The final slide split the questions in two. Settled: "Can a model draft a cash flow analysis?" Still open: "Who writes down how your firm does it? Who tests it before it ships? Who owns it once fifteen other people depend on it?"

The bottleneck "used to be how fast can I type," Ragone said. Now the question is who makes sure every shipped skill is tested and updated: "How do we know that every skill is fully up to date?" Ragone expects planners to become "almost be AI agent managers," while "the human touch is still there." The goal: "instead of providing advice for three to four people a day… we're providing advice to 100 people a day per planner." Ragone said firms are diverging between those adopting this and those that aren't, and closed by noting that "this is coming from someone with no engineering background, no coding background." Range is hiring.

The session description had posed the question of "how to check quality at scale when the output is judgment." Neither version of the talk went beyond the owner-and-reviewer step and the open questions on the last slide.

## What only the room version had

Compared with the online recording, the in-room talk added: Claude Cowork as the turning point, the team size of about 20 planners, the standardized reference documents in Notion and Google Docs, the pay-stub example, the questions about tax year and state, Rai, the colleague-as-reviewer example, the internal agent Ranger, the quarterback metaphor, the goal of 100 clients a day per planner, and the final "how could you make yourself better?" question before shipping. The online version, in turn, has the scripted lines quoted above, such as "a skill that was right in March can be wrong by December."

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=9b002687-09cf-42e2-9282-532ed9d0c184) · [Online version of the talk](https://www.youtube.com/watch?v=gpmze-6ueGU) (YouTube, 8:53, recorded separately) · [Introducing Rai: The First AI Wealth Advisor](https://www.range.com/blog/introducing-rai-the-first-ai-wealth-advisor) (Range)

This write-up is based on the author's room recording of the in-room talk (which starts about two minutes in), 13 slide photos, and the captions of the separately recorded online version, used for the opening and marked where quoted. Ragone offered to take questions in the final two minutes, but the recording ends without any. The [precise notes](precise/10-14-1130-we-let-our-financial-planners-write-the.md) (Chinese) compare the two versions section by section.
