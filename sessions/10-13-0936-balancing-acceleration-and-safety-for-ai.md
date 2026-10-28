**English** | [中文](zh/10-13-0936-balancing-acceleration-and-safety-for-ai.md)

# Brakes Let You Go Fast: Five Controls for Running AI Agents at a Hedge Fund

*At AI Engineer NYC 2026, Two Sigma's John Feminella used Citibank's $900 million Revlon payment error to show how controls fail when everyone follows procedure, then walked through five composable controls that let agents act with real autonomy without locking everything down.*

**Featured Keynote (Main Stage) · Tuesday, October 13, 2026 · 9:36–9:54am · Simons Room (Metropolitan East)**
**Speaker:** John Feminella, SVP Architecture, Two Sigma

---

John Feminella opened with a legal disclaimer slide: a promise not to sell or market any securities, and a statement that everything in the talk was Feminella's own opinion and not Two Sigma's. "If I don't say those magic words, the 5G microchips implanted in me will explode," Feminella said.

The disclaimer, Feminella went on, was a small example of how regulated the industry is. In a company full of controls, "one thing that's always interesting is to explore what happens when controls fail, because that gives you a good idea about why the controls were there in the first place." So the talk began with a story about a failure.

## 1. How Citibank sent $900 million by mistake

After August 2020, Feminella said, a new clause began appearing in commercial loan agreements. It guards against a strange scenario: the bank in the middle of a loan sends the lenders everything the borrower owes, by mistake, and the lenders reply, "No, thanks. You paid us back and we're all good, so we'd rather keep the money." (The clause is the erroneous-payment provision drafted by the Loan Syndications and Trading Association, known in the market as a "Revlon blocker.")

![Failure of controls](../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/01.jpg)
*The parties: Revlon as borrower, Citibank as agent, and two groups of lenders.*

Revlon, the cosmetics company, had borrowed under a large 2016 term loan, split among more than 300 lenders: hedge funds, credit funds, CLOs and other banks. Citibank was the administrative agent, "performing the plumbing that connects the borrowers with the lenders." Revlon paid Citi, and Citi paid the lenders; "Citi's not lending its own money, it's just in the middle moving people's money around." (According to the court's later findings, the loan was a seven-year, $1.8 billion syndicated loan, and there were 315 lenders.)

![August 11, 2020: the plan](../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/02.jpg)
*"Pay everyone their interest. Move no principal." The principal was meant to go to an internal wash account.*

The plan for August 11, 2020, was simple. Revlon would pay about $7.8 million of accrued interest to both groups of lenders. One group's principal would roll into a new facility, "an exchange of paper, not cash," and the rest would stay in the 2016 loan. "Only interest should be going out, principal should not be moving at all."

Citi processed the transaction in internal software called Flexcube ("Isn't that a fun enterprise software name?"). The easiest way to book it in Flexcube was to record it as if Revlon were paying off the entire loan, which produced the right interest calculations. The principal portion was supposed to go to an internal "wash account" owned by the bank, "so that it's booked and recorded correctly, but it never leaves the bank's perimeter."

![The screen](../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/03.jpg)
*Figure 1 from the court opinion: the Flexcube screen, with only the PRINCIPAL field set to the wash account.*

![What the screen needed](../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/05.jpg)
*Three fields needed the wash account number; only one had it, and $893.9 million went out.*

Three fields, labeled FRONT, FUND and PRINCIPAL, all needed the wash account number and a checked box. "For various arcane reasons," only PRINCIPAL was set. Flexcube showed a warning that money was about to leave the bank. But money was supposed to leave the bank, since the interest was being paid. "So the three approvers who were supposed to approve that looked at it and said, 'Yep, that's the right thing that's supposed to happen,'" and the operators went past the warning.

The court record fills in the detail. Citibank used a "six-eye" procedure: a maker entered the transaction, a checker verified it, and a senior manager approved it. All three believed, wrongly, that setting the PRINCIPAL field alone was enough, although the bank's internal manual said all three fields had to be set. The approver replied by email, "Looks good, please proceed. Principal is going to wash." The warning read, "Account used is Wire Account and Funds will be sent out of the bank. Do you want to continue?" It did not show an amount, and the checker clicked yes.

![One of the biggest blunders in banking history](../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/06.jpg)
*The wash account was skipped, and $893.9 million of Citi's own money went to the lenders.*

Flexcube sent the entire outstanding principal to every lender: "about $900 million instead of $8 million," Feminella said. (The slide says $893.9 million; the court gives $893,944,008.52.) The slide's title quoted the federal judge who later heard the case, who called it "one of the biggest blunders in banking history." Each lender received exactly what it would have been owed if Revlon had repaid the loan in full, which Revlon had no intention of doing.

For the lenders it was a windfall. Revlon was in trouble, so its loans traded well below face value, and "getting all your money back on a deal that you thought was only worth 60 cents on the dollar is a great deal for you," Feminella said. (The appeals court later described the loan as trading at 20 to 30 cents on the dollar.)

![Please give the money back](../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/07.jpg)
*Citi asks for the money back; the lenders answer "NO."*

When Citi called and asked for the money back, many lenders refused, arguing that a loan is meant to be repaid and an early repayment is still a repayment. Hence the new clause. Feminella did not cover how the case ended: in February 2021 the district court let the lenders keep roughly $500 million they had not returned, and in September 2022 the Second Circuit reversed that ruling.

The lesson Feminella drew: "the bank had this procedure, it had these controls, and everybody did what they thought the right thing to do was. That's the problem." Firms now give real agents real autonomy over systems like Flexcube every day. The question is how to prevent accidents like this one "without locking everything down or just hoping we'll get lucky."

## 2. Safety and innovation are not a trade-off

"You can't really know in advance what an agent will do," Feminella said. That sounds alarming "until you remember that humans are also non-deterministic systems, and they're going to make mistakes, too." Firms don't lock employees in rooms to stop them erring; they give people autonomy along with boundaries, visibility, and systems that let them do their jobs. "Safety and innovation are not opposite ends of a spectrum that you must trade off on … I think you can have your cake and eat it, too."

![Is safety a trade-off?](../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/08.jpg)
*"Innovate without permission slips." "Too little safety: existential risk. Too little innovation: slow death."*

![What usually doesn't work](../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/09.jpg)
*Four approaches Two Sigma rejected.*

Feminella listed the approaches that did not work for Two Sigma:

| Approach | What the slide says |
| --- | --- |
| Sandbox everything | Safer, but an agent's value often comes from what it can touch and combine |
| Lock everything down | Very safe but also not very useful; encourages shadow agents you can't see |
| Let agents go wild | "Gas Town-alikes" (after Steve Yegge's multi-agent orchestrator) have poor containerization out of the box and aren't suitable for enterprise use |
| Policy docs alone | Helpful for staking a claim, but not enforceable at runtime by themselves |

A sandbox with no real access "is safe, but it's not very interesting or useful." Letting agents do whatever they want leaves isolation "very weak," which does not suit highly regulated firms. Policy documents in a prompt are "not very enforceable at runtime."

What worked was a mix of controls: "the guardrails and shared capabilities that let teams move in ways that don't hinder what they're trying to do." Feminella stressed that "these controls are composable. Each one does one job," and together they let agents "run with real autonomy at an appropriate level of safety."

![Controls: composable](../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/10.jpg)
*The five controls, each with one job.*

| Control | What the slide says it does |
| --- | --- |
| Agent Registry | Catalog every agent: tools, endpoints, owner, product |
| Risk Factor Scoring | Score each agent's risk at registration and on every update |
| Activity Controls | Prevent anticipated risky egress or actions |
| Evaluations | Use shared approaches for common evaluations |
| Auto Mode Classifiers | Prevent unanticipated risky egress or actions |

Each control was introduced the same way: a problem, a tempting "wrong control," and the fix.

## 3. Control 1: an agent registry

![Problem 1: agent sprawl](../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/11.jpg)
*Agents across teams, and one with no team: who owns it, what tools, what access?*

Agents were appearing everywhere: big teams with many, small teams with a few, individuals "running stuff in an uncontrolled way." They took every shape: "random chatbots," "MCP servers glued together in this Frankenstein hotchpotch," notebook experiments, useful web tools. Without answers to simple questions (who owns this, what tools does it have, what should it be doing), the firm could neither govern nor reuse them, and teams rebuilt the same thing.

The tempting fix is a gate: nothing runs until it is registered and approved through an onerous process. Then even a notebook experiment waits behind something genuinely risky, and most people give up or move to "some shadow IT, less governed path," where the firm sees nothing at all.

![Control: Agent Registry, automatic front door](../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/12.jpg)
*Agents register their endpoints, tools and metadata; the registry ties them to products and is queryable by people and agents.*

"You can't govern what you can't see," Feminella said. The registry records each agent's tools, the endpoints it talks to, the product it belongs to, and its owner. People, programs and other agents query it to find what exists and use it as a source of truth. Registration costs nothing extra: building with one of the firm's approved agent frameworks registers the agent automatically, and manual registration is available. (Feminella's accompanying article adds that bespoke agents are detected heuristically, registered automatically, and their owners notified.)

## 4. Control 2: risk scoring

![Problem 2: one-size review](../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/13.jpg)
*A summarizer, a document drafter and a production deployer all wait weeks in the same review queue.*

![Wrong control: committee review for every agent](../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/14.jpg)
*Security, legal, risk and committee, two weeks each: "8+ weeks to ship" for a trivial agent. (The photo is partly covered by the video player's controls.)*

Agents carry very different risks. A bot that summarizes Jira or Linear tickets is not a deployment agent that can touch a running system. Run both through the same review and every agent waits weeks, so people route around the process. Heavy manual review by every constituency "feels safe, but now we've got a trivial summarizer waiting 2 months for the same sign-offs as an agent that is touching production systems." Most valuable agent ideas start as small experiments, and "the reviewers are drowning in these like low-risk requests" while the risky ones compete for the same attention.

The way out is that most enterprise agents are purpose-built. Open-ended coding agents "like Pi or Claude Code" don't know what you will ask next, but agents deployed in a firm usually have "very specific purposes," and can be told apart by what they declare up front: tools, endpoints and controls.

![Control: Risk scoring applies differentiated scrutiny](../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/15.jpg)
*A risk assessor inside the registry scores each agent at registration and on every update.*

The slide showed the flow: an agent is registered or updated, a risk assessor in the registry scores it, the score is validated, and the agent is published. Agents are scored "by tools requested, purpose, and systems touched," and very high-risk agents may be flagged before publishing. (Per Feminella's article, low-risk agents publish without human involvement.)

## 5. Control 3: tools follow purpose

![Problem 3: differentiated access](../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/16.jpg)
*Six kinds of access an agent might want, from role accounts to the external web.*

An agent rarely touches just one thing. It might use a role account, call internal API endpoints, run tools like web search or SQL, call other agents, read data stores, or reach the external web. "Each one of those is … a different door to a different kind of risk." An entitlement to a role account can do far more damage than reading a public web page. "The question is not can this agent do X, but what's the granularity" it needs.

![Wrong control: block all outbound access](../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/17.jpg)
*Blocking all outbound access: "Safe, but useless."*

Giving every agent the same short allow list and blocking everything else "is safe, but it's also useless." Teams give up or find a way around the wall, and "both outcomes are costly." Feminella quoted the author John Shedd: "A ship in harbor is safe, but that is not what ships are for." (The line comes from Shedd's 1928 collection *Salt from My Attic*, where it reads "built for.")

![Fix: access depends on resource and agent purpose](../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/18.jpg)
*The same tool gets a different answer depending on the agent's declared purpose.*

The fix is to let agents declare their purpose and the tools they need:

| Agent (purpose) | Agent tool | External web | Data store | API endpoint | Other agent | Role account |
| --- | --- | --- | --- | --- | --- | --- |
| summarizer (research) | Allow | Allow | Allow | Deny | Allow | Deny |
| deployer (release ops) | Gated | Deny | Gated | Allow | Gated | Gated |

A research summarizer may use a web tool; a release deployer has no reason to, so it doesn't get one by default. "As the agent author, you're in the best position to say, 'I need these four things out of the 500 that are available.'"

![Control: Activity controls](../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/19.jpg)
*Inside the container, an egress proxy lets the agent reach approved destinations and blocks the rest.*

Enforcement happens at runtime. When an agent starts in a managed environment, it looks up its policy in the registry, and the policy becomes enforceable rules: which endpoints and hosts it may reach. The slide showed an egress proxy beside the agent in its pod, passing traffic to approved destinations and blocking others. "Nobody's writing a ticket every time they need to change the access." As long as an agent conforms to its risk score and is in the registry, "you're governed, you're approved." (The article adds that the proxy does not trust agent code and fails closed.)

## 6. Control 4: shared evaluations

Teams had built many different ways to evaluate agents, and some had built none. "We'd be upset if teams were not writing unit tests, right? And we consider evals to be kind of the same thing." A model is like a runtime dependency; when an agent changes models, only evals show what happens to its output quality.

![Wrong control: central eval gate for every change](../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/20.jpg)
*Every typo, prompt, tool and config change waiting weeks at a central gate: "Innovation stalls."*

A central methodology that waits for a full suite and committee sign-off means "every like prompt tweak, every one-line change is going to sit in that same queue."

![Control: Shared eval framework](../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/21.jpg)
*Left: every agent rebuilds eval processing, integration and trace ingestion. Right: a shared framework does the common work.*

Most of what goes into a good eval is the same for every agent: eval processing, integration, trace ingestion. So Two Sigma runs "one shared approach to how we do evals … run by the firm and available to everybody," and "all you've got to do is just write what's true about your agent." (The article adds that results are aggregated across teams to show how models perform in the firm's own context, and that evals run on every change, like CI/CD.)

## 7. Control 5: classifiers instead of approval fatigue

![Problem 5: approval fatigue](../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/22.jpg)
*A stack of "Allow?" prompts, a row of approvals, and one risky action approved by reflex.*

Approving individual agent actions works at small scale. Once agents run in parallel or in production, "review requests come in faster than anybody can be expected to approve them," and reviewers fall into fatigue: "yes, yes, yes, yes, yes, and eventually you click yes on something that's, you know, activating a new nuclear bomb."

Feminella gave a recent example. Earlier in the year, a director at an AI research lab told an OpenClaw agent to suggest emails to delete and wait for approval. The inbox was so large that the agent compacted its context and lost the instruction to wait, then deleted hundreds of emails and did not stop when told to from a phone. (Feminella did not name the person; the incident was reported in February 2026 by Summer Yue, an alignment director at Meta Superintelligence Labs, who said more than 200 emails were deleted.)

![Fix: let classifier decide, escalate only what matters](../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/23.jpg)
*Many calls allowed, a few sent to a human, rare dangerous ones blocked.*

The better approach is to let "independent, conservative classifiers" decide routine calls and send people only the requests most likely to matter. "If you're saying yes 99.99999% of the time, then that's probably just a rule that the agent should follow, not a request you need to ask the human for permission for." Unclear or possibly dangerous requests go to a human; clearly dangerous ones are denied outright. Where explicit rules apply, a deterministic fast path decides; where they don't, a classifier does.

![Control: Auto-mode classifier — allow, soft or hard deny](../slides/10-13-0936-balancing-acceleration-and-safety-for-ai/24.jpg)
*A fast deterministic path from settings, a two-stage classifier for everything else, and a prompt-injection check on results.*

| Step | What the slide shows |
| --- | --- |
| Trigger | The main model proposes a tool call in auto permission mode |
| Fast path (deterministic) | Explicit rules in `~/.claude/settings.json`: allow or deny |
| Slow path (nondeterministic) | A classifier with stage 1, fast triage that errs toward blocking, and stage 2, deep review with "thinking" adjudication for uncertain cases; outcomes are allow, soft deny ("agent may clarify") or hard deny |
| Execute and check | The tool runs if allowed; a prompt-injection probe checks the result before it is added to the transcript |

The diagram uses the file path and command names of Claude Code's auto mode, though Feminella did not name it on stage. Anthropic's engineering write-up of auto mode describes the same design: a fast first-stage filter "instructed to err toward blocking," a second reasoning stage for flagged actions, and a probe that screens tool output for prompt injection.

## 8. Why cars have brakes

Feminella put the five pieces back together. A registry makes every agent visible from the day it is created. Risk scores put human review where it belongs. Guardrails let agents touch only what they need. Shared evals and metrics hold everyone to the same bar without reinventing the wheel. And governing the runtime ensures classification happens there "and not somewhere else."

"Why do cars have brakes?" Feminella asked. "It's not just so that you can stop, it's so that you can go fast and stop safely. So controls for our agents works the same way."

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=73badd10-6f8c-4c95-b5e7-55fc3d7bafc1) · [Video](https://www.youtube.com/watch?v=iniryju23Sk) · [Livestream (Day 1, from 23:57)](https://www.youtube.com/watch?v=akEqiXFo3GM&t=1437s) · [Brakes Let You Go Fast](https://dev.to/dailycontext/brakes-let-you-go-fast-balancing-acceleration-and-safety-for-ai-agents-49p9) (Feminella's article) · [In re Citibank August 11, 2020 Wire Transfers](https://www.nysd.uscourts.gov/sites/default/files/2021-02/20cv6539%20Citibank%20Opinion.pdf) (S.D.N.Y. opinion, 2021) · [Revlon decision reversed on appeal](https://www.torys.com/our-latest-thinking/publications/2022/09/revlon-erroneous-payment-decision-reversed-on-appeal) (Torys) · [How we built Claude Code auto mode](https://www.anthropic.com/engineering/claude-code-auto-mode) (Anthropic) · [OpenClaw inbox incident](https://techcrunch.com/2026/02/23/a-meta-ai-security-researcher-said-an-openclaw-agent-ran-amok-on-her-inbox) (TechCrunch) · [Quote origin: "A ship in harbor is safe"](https://quoteinvestigator.com/?p=7781) (Quote Investigator)

This write-up is based on the official video captions, the Day 1 livestream, slide photos and the sources above. The talk ended without Q&A. The [precise notes](precise/10-13-0936-balancing-acceleration-and-safety-for-ai.md) (Chinese) keep timestamps and the full record of where the talk, slides and court record disagree.
