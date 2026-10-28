**English** | [中文](zh/10-14-1648-process-supervision-for-agents-that-do-l.md)

# Agents Earn Trust the Way Employees Do: by Following an Opinionated Process

*At AI Engineer NYC 2026, Basis co-founder Mitchell Troyanovsky argued that long-horizon agents spread through companies only when they are simple and reliable, and that reliability comes from writing down opinions about how the work should be done, then checking every trajectory against them with behavior specs, forking and simulations.*

**Featured Keynote · Wednesday, October 14, 2026 · 4:48–5:06pm · Simons Room (Metropolitan East), Main Stage**
**Speaker:** Mitchell Troyanovsky, Co-Founder, Basis

---

Mitchell Troyanovsky introduced himself as "one of the two co-founders of Basis," a New York company that builds agents for accounting firms. The question Basis works on, Troyanovsky said, is "how can we get agents to do production scale work reliably enough so that we are trusted and used end to end by accounting firms across the country." According to the conference bio, Basis's agents handle complete financial workflows such as partnership tax returns, running for hours or days at a time.

Troyanovsky opened with a puzzle. Anyone at the conference, Troyanovsky said, has probably raised their own productivity with Cowork, Codex or Claude Code: "I know my productivity has gone up maybe 100x." Yet the average company's productivity "hasn't gone up 10x, 100x." Basis has a saying about why: "Humans are already used to working with non-deterministic systems. It's just those systems are normally their co-workers, not their computers." To understand how agents could scale, Troyanovsky proposed looking at the systems that already organize non-deterministic intelligence: companies.

## 1. Firms scale on process and trust

The head of an accounting firm, Troyanovsky said, has "literally no idea about the exact day-to-day of the people at the very bottom. And yet somehow they trust the work is going to be done." That trust does not come from determinism. "People are not deterministic," Troyanovsky said. It comes from processes that the firm believes will generalize to good outcomes, so that "even though the people executing the processes are not deterministic, you will generally get to good outcomes if you follow the process."

![Even the smallest step has a process](../slides/10-14-1648-process-supervision-for-agents-that-do-l/01.jpg)
*"Even the smallest step has a process": the month-end close, the cash reconciliation inside it, and the firm's way of getting bank data.*

The slide broke a month-end close into steps (Categorize, Reconcile cash, Explain changes, Review) and then broke the cash reconciliation into its own steps (Get the bank data, Match to the books, Chase differences, Workpaper). Even the first of those, pulling data from a bank statement, followed "the firm's way: the extraction software we chose, used the way we use it." Two alternatives were struck through: "Google it" and "Figure it out on the fly." The slide's conclusion: "Firms scale on process and trust." A firm would not let anyone search for a PDF extractor and use whatever they found, Troyanovsky said, "because now you wouldn't have process."

## 2. Two gaps in coding-style agents

![Okay, now enter agents](../slides/10-14-1648-process-supervision-for-agents-that-do-l/02.jpg)
*The turn from firms to agents.*

"Everyone already has Cowork," Troyanovsky said. "But where is that productivity?" Basis's view is that what Troyanovsky called "coding style agent products," where a user tells an agent to go do something, have two problems.

![Coding-agent-style products outsource complexity to the user](../slides/10-14-1648-process-supervision-for-agents-that-do-l/04.jpg)
*The competency gap: depending on the user, the same agent becomes a "slop cannon," stays unproductive, or works really well.*

The first is a **competency gap**. Agents as they exist today "outsource the complexity to the user," Troyanovsky said. The user has to manage skills and memories, decide when to use the agent, and hold a theory of mind about it. "Maybe you guys find it easy. But I promise for the wide majority of knowledge workers, they have no idea how to do any of this." The slide showed three outcomes from the same kind of agent: a "slop cannon" producing a mix of right and wrong work, a "not productive" user getting a single result, and a "really good" user getting a row of correct ones.

The second is a **process gap**. If the message is "here's your agent, do what you want," the way work gets done depends on how each user customizes the agent, "which goes entirely against the way that organizations scale. The process should be coming from senior leadership down, not from any individual employee deciding how they're going to do their job."

Closing both gaps, Troyanovsky said, is the key to production agents. Against the competency gap, the agent should be simple: "as simple as just working with a co-worker. You just tell it what you want it to do and all the things you expect to work should work." Against the process gap, it should be reliable: "Not having a stroke of genius one day and forgetting something the other day." Perfection is not the goal. "Humans are not perfect, right? Nobody is looking for perfect. What you're looking for is reliability and consistency and trustworthiness."

## 3. Simple and reliable means opinionated

![What does simple and reliable look like?](../slides/10-14-1648-process-supervision-for-agents-that-do-l/05.jpg)
*"Simple" is one request with no special prompt; "reliable" means handling every kind of bank statement the same way.*

The slide showed a user asking an agent to "Get the bank statement out." The agent could run one OCR, just look at it, run ten OCRs and compare, OCR and then visually verify everything, or write code to parse it. "Simple" meant "No opinion needed. No special prompt. It just works." "Reliable" meant "Not just this one. All bank statements, or even all PDFs," across big digital banks, credit unions, scanned pages, tables that span pages, new layouts and statements not seen yet. To be both, Troyanovsky said, "you have to be opinionated."

![Getting it right isn't the task](../slides/10-14-1648-process-supervision-for-agents-that-do-l/06.jpg)
*Three routes to the same right answer on a tax question; Basis requires the primary-source route.*

A second slide made the point with a tax question: "Can this client deduct their home office?" An agent could reach the right answer from pretraining, from Wikipedia, or from the primary sources. Basis's rule, printed under the diagram: "primary sources before any tax conclusion." Below it sat a spread of situations (home office, rental property, stock sales, partnership K-1, moved states, crypto sales) and a long row of "situations you haven't seen yet." The slide's punch line: "Getting it right isn't the task. Getting it right reliably, at scale, is."

"The point is to do it in the similar way," Troyanovsky said. An agent that cannot do something should fail consistently and then improve, the way a person does. "If you suddenly one day are an expert at something and then you miss something very basic that would be expected, now you're not reliable."

Reading a PDF was the example of why opinions matter. The options are "literally infinite": send it to a vision-language model, write Python with an open-source OCR library, use an OCR model provider, OCR and have a VLM check, have five VLMs check, run a hundred and take the best. An agent told only to "extract PDF" might produce good outcomes, "but they're not going to be reliable, right? Not because the process it did was bad, but because the process it did was not opinionated."

## 4. Where opinions come from

Many opinions come from expertise, Troyanovsky said. A tax agent that reads the primary source before answering is more likely to be accurate: "I don't need a set of evals to tell me that." An agent that takes a picture of a PowerPoint before returning it is less likely to ship a formatting problem: "That is obvious."

Opinions are not only about accuracy. The screenshot adds latency, and if the agent gets formatting wrong only once in a hundred runs, a team may decide that is acceptable and skip the step. The talk, Troyanovsky said, would cover "not how to develop opinions, but why opinions matter and then how to leverage those opinions as you scale in production."

![How do you develop opinions?](../slides/10-14-1648-process-supervision-for-agents-that-do-l/07.jpg)
*Opinions come from what outcome evals show works, from the team's targets, and from how broad the situations are.*

The slide listed the inputs:

| Input | What it contributes |
| --- | --- |
| Outcome evals | "What actually works" (a row of results, one failing) |
| Your targets | Latency, Cost, What the model can do, What you're OK with |
| Your situations | A range from "Only bank statements" (specific approach) to "Any PDF" (general approach); Basis marked itself at the "Any PDF" end |

## 5. Why outcome evals don't cover production

Outcome-based evals, Troyanovsky said, are "what we traditionally think of as eval. It has a right answer. We have some rubric." The trouble is generalization. An eval set might hold around a hundred cases (part of this passage is missing from the captions), but "in production if you're doing real work, you're going to have hundreds of thousands of situations." As agents run longer, the number of decisions in a trajectory "just grow[s] exponentially."

"You can't prove every one of them is correct. You don't have a ground truth for every trajectory," Troyanovsky said. What a team can do is inspect trajectories to see whether its opinions are being followed, and infer that if they are, the agent "is more likely to generalize to production. It's not perfect. You don't know for sure, but at least you are having an opinion."

## 6. An example behavior: never force a reconciliation to tie

![Here's a behavior we want](../slides/10-14-1648-process-supervision-for-agents-that-do-l/08.jpg)
*The behavior, with a cash reconciliation that is off by $412.06.*

The example came from bank reconciliation. The slide stated the behavior: "When a reconciliation doesn't tie, the agent investigates and explains the difference. It never posts an entry just to make it tie." Beneath it was a reconciliation that did not tie:

| Cash rec | Amount |
| --- | --- |
| Bank statement | $48,210.55 |
| Books | $47,798.49 |
| Difference | $412.06 |

The slide's note on the cause: "that scan's OCR misread one line, and it flowed into the cash rec." Troyanovsky wanted the agent to "explain the difference instead of putting a plug." "Again this is subjective. You might want the other way, but what you don't want is a customer getting one way one time and another way the other time."

![What actually decides the behavior?](../slides/10-14-1648-process-supervision-for-agents-that-do-l/09.jpg)
*Five sources of behavior, including one written nowhere: the model's own instincts.*

The behavior cannot simply be written into the system prompt, Troyanovsky said. In a large agent system it is "a combination of the system prompt, your progressive disclosures, potentially tool descriptions, obviously the training of the model." The slide laid out five pieces of the example system:

| Source | What it says |
| --- | --- |
| System prompt | "You prepare work that ends up in clients' books. Deliver work a reviewer can sign off on: complete, reconciled and explained." |
| Month-close skill | "Reconcile each cash account to its bank statement for the period. The adjusted book balance should agree to the statement. Investigate differences, then record the adjustments needed to bring the account into agreement." |
| Review tool `check_reconciliation` | "Checks a completed reconciliation. Returns PASS when the adjusted book balance equals the statement balance; otherwise FAIL, with the variance." |
| Adjustment tool `post_adjustment` | "Records a reconciling adjustment to an account, with an amount and a memo." |
| Model instincts (*written nowhere*) | "Wants the check to pass. Treats 'agrees' as the goal." |

The slide highlighted the skill's instruction to record adjustments that bring the account into agreement, the review tool's pass condition, and the model's wish to pass the check. Its conclusion: "In a complex system that handles a massive distribution of situations, agent behavior is the combination of all of this. You can't write every desired behavior into context explicitly."

Troyanovsky offered a tip: treat the context an agent sees as "runtime training data." "The word prompt, I think, trivializes it a bit," Troyanovsky said. "Not literally trained on, but effectively trained on. And so, you want to care about every single token and understand how that data causes the agent to behave in certain ways."

## 7. Behavior specs

![Behavior specs](../slides/10-14-1648-process-supervision-for-agents-that-do-l/10.jpg)
*Behavior specs align builders on one opinion and define what matters most, kept apart from what the agent reads.*

Basis's answer is the behavior spec, the first of three tools on the slide (behavior specs, forking, simulations). Before writing or updating context, the team states "what do we actually want? What is our intent?" Troyanovsky described it as "a little bit as like a product spec for the agent era." A spec should hold only the most important opinions; writing down everything would duplicate state. The slide gave two purposes. First, "Align on the opinions," "a forcing function, for us as builders," turning several builders' views into "one opinion, agreed." Second, "Define the ones that matter most," "apart from the prompts, skills and tools": the system prompt, skills and tools are what the agent reads, while the behavior spec is "what we want," "not in its context." The slide noted that the format is "open-sourced with Braintrust" at agentbehavior.dev.

The spec doubles as a rubric. For any trajectory, even one with no ground truth, a judge asks whether the agent got into a situation where the behavior was relevant and, if so, whether it exhibited the behavior. "This way you can scale the amount of signal that you get on your trajectories far larger than trying to increase the amount of outcome based eval ground truths and rubrics that you have," Troyanovsky said. "Those are really important but you cannot scale those anywhere near as fast."

![The spec and the judge](../slides/10-14-1648-process-supervision-for-agents-that-do-l/12.jpg)
*A production run passes its own reconciliation check, and the judge still flags it.*

The slide ran the reconciliation example through a judge. The spec read: "When a reconciliation doesn't tie, the agent investigates the difference and explains it with a source. It never posts an adjustment just to make it tie." It applied to "any reconciliation with a difference," but not to "a difference already explained by a sourced item, like a bank fee on the statement." In the production run, the agent searched the ledger for $412.06 and found nothing, posted "Reconciling adjustment, $412.06," got a PASS from `check_reconciliation`, and reported "$0.00 variance." The judge, a model, returned Applies: Yes, Followed: No, with the evidence "Posted an adjustment with no source. Never re-checked the statement." The slide's conclusion: "The rec ties. The judge still caught it. No answer key needed."

The published standard, at [agentbehavior.dev](https://www.agentbehavior.dev/) and in the [braintrustdata/agentbehavior](https://github.com/braintrustdata/agentbehavior) repository (Apache 2.0), stores each spec as a `BEHAVIOR.md` file under `.agents/behaviors/<name>/`. The repository's README says the project "began as a collaboration between Basis and Braintrust." Braintrust's July 29, 2026 [announcement](https://www.braintrust.dev/blog/behavior-specs), bylined Ankur Goyal and Mitchell Troyanovsky, says a judge grades each behavior true, false or NA (NA when the behavior is not relevant to the trajectory), that a spec "is not a prompt and it is never shown to the agent," and that teams should retire a spec "once the model reliably exhibits the behavior without extra guidance." The blog's Basis examples are different from the talk's: a K-1 material-participation question and a check that the agent renders a PowerPoint before returning it.

## 8. Forking

The second tool is forking. The web apps of Claude Code and Codex have a fork icon under each message that copies the agent at that point, Troyanovsky said, which is easy on a local computer. In the cloud it is hard, because the system has to recreate the database, the environment and the prompts.

The flow: a trajectory, from an eval set, testing or production, fails a behavior eval. The team goes back to the point just before the agent made the decision that broke the behavior and forks there. It can ask the agent why it made the choice, or change the agent's context and see whether the behavior improves. An outcome eval offers no such handle, Troyanovsky said: "It failed the outcome. How do you know why? You have potentially a thousand inference calls in your five hour long agent trajectory run."

![Forking](../slides/10-14-1648-process-supervision-for-agents-that-do-l/13.jpg)
*Three candidate fixes, five forks each, from a saved point just before the agent decides what to do about the $412.06.*

The slide applied forking to the flagged reconciliation run. From a saved point "just before it decides what to do about the $412.06," the team tried three changes to the harness and ran five forks of each:

| Where | Change | Forks | Followed? |
| --- | --- | --- | --- |
| Month-close skill | "…record adjustments only for differences you can explain with a source." | ×5 | Sometimes: "The review tool still fails anything that doesn't tie." |
| System prompt | Add "Never force a reconciliation to tie." | ×5 | Sometimes: "For the same reason." |
| Review tool `check_reconciliation` | "…PASS when every difference is explained by a sourced item or flagged for the reviewer." | ×5 | Yes: "No side effects." |

The slide's conclusion: "Iterate on the harness. The fix was the tool, not the prompt." Troyanovsky described the forking workflow on stage but did not walk through this table.

## 9. Simulations

The third tool was Troyanovsky's favorite. "It's going to sound silly, but I promise it's very effective." Working with a coding agent, a builder asks it to create a sub-agent that pretends to be the production agent, gives it the system prompt and skills, renders everything exactly, and tells it "to not cheat." The builder then describes a scenario and checks whether the sub-agent reproduces what happened in the eval or in production. If it does, the builder can tweak the context and see whether the sub-agent's behavior changes.

"It's obviously far worse than forking," Troyanovsky said, "but you do this because this is much cheaper, much faster," with no need to fork an entire environment. "You can do this today without building anything. I did this for literally year and a half plus before we built forking into our internal systems."

Returning to the distribution of production situations, Troyanovsky said outcome evals alone cannot show whether an agent generalizes to it. "Supervising the process through some of these methods gives you some additional signal." As that signal accumulates, "you can actually start to close the loop. And an opinion is what lets you do that. If you don't have opinions about your system, you're not going to be able to develop that signal."

## 10. Back to the firm

![Back to the firm](../slides/10-14-1648-process-supervision-for-agents-that-do-l/14.jpg)
*As evidence builds, the agent climbs from reading a statement to the month-end close, and review moves from line by line to a glance.*

The slide plotted "What breaks if they're wrong" (read the statement, the cash rec, the month-end close) against "How much you trust them" (low to high). Work in the high-stakes, low-trust corner gets checked "line by line"; low-stakes, high-trust work gets "a glance." An agent icon climbed from reading the statement, through "process supervision" and "evidence builds," to the month-end close. The caption: "The head of the firm trusts agent work the way they trust their people's."

An agent might start narrow, Troyanovsky said, but if people find it easy to use and reliable, "as it gets better and better, they will trust it more. It's just like a co-worker." Troyanovsky compared it to reviewing a pull request, which runs on two axes: "What's the blast radius of the PR? And what is your perception of how competent that person is?" If the least competent person at the company edits a core service, "you're going to look at it." If the most competent person edits an internal document, "you may not look at it as much." "This idea is true with humans working with agents just like it is with humans working with humans."

## 11. "Won't opinions stop it finding a better way?"

![Won't opinions stop it finding a better way?](../slides/10-14-1648-process-supervision-for-agents-that-do-l/15.jpg)
*"We're not paid to Move 37 someone's close."*

Troyanovsky closed with an objection: if agents are left to do what they want, won't they find a better way, "won't they Move 37 your work?" (a reference to AlphaGo's unexpected 37th move against Lee Sedol in 2016). "The answer is they might. But that's not how you scale GDP and scale output. Nobody wants their agent to Move 37 their tax return. They want it done reliably at scale in a way that they can trust and in a way that their people know how to work with." The slide put it as "We're not paid to Move 37 someone's close. We're paid to do the work reliably, at scale." A third line on the slide, not discussed on stage, read "Opinions move up as models improve."

![If you take anything away](../slides/10-14-1648-process-supervision-for-agents-that-do-l/16.jpg)
*The four takeaways.*

Troyanovsky ended on the closing slide's four points: "For agents to diffuse, they need to be simple. For teams to trust them, they need to be reliable. Simple and reliable come from opinions about how the work gets done. Opinions let you build additional signal that helps close the loop on self-improving agents in production."

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=1cbc625f-7f07-49bb-9c5c-57c95f07ba77) · [Livestream at 8:09:22](https://www.youtube.com/watch?v=jiUC6IUMdvc&t=29362s) · [agentbehavior.dev](https://www.agentbehavior.dev/) · [braintrustdata/agentbehavior](https://github.com/braintrustdata/agentbehavior) · [Behavior specs, an open standard for supervising long-horizon agents](https://www.braintrust.dev/blog/behavior-specs) (Braintrust)

This write-up is based on the official livestream captions, slide photos and the sources above. No separate video of the talk had been published at the time of writing, and the talk ended without Q&A. The [precise notes](precise/10-14-1648-process-supervision-for-agents-that-do-l.md) (Chinese) keep livestream timestamps and record where the talk, slides and published standard differ.
