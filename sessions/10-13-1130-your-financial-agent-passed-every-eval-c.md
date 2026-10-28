# A 97% Eval Score Is Not a Defense: Four Controls for Financial Agents

*At AI Engineer NYC 2026, Fieldguide's Anirudh Veeraragavan argued that a financial agent's decision is defensible only when four controls hold together (authorization, execution control, evidence provenance and evaluation integrity), and walked through one failing reconciling item for each.*

**Breakout (Evaluations track) · Tuesday, October 13, 2026 · 11:30–11:48am · Turing Room (Riverside Ballroom)**
**Speaker:** Anirudh Veeraragavan, Lead Security Engineer, Fieldguide (Fieldguide's founding security hire; previously five years at Plaid, ending as Staff Software Security Engineer)

---

Anirudh Veeraragavan began with a build that looks finished. The first slide, "Let's build a financial agent," showed a typical four-step process for an audit agent: inputs (the client's books, the bank reconciliation, bank evidence), an agent (which does the matching, with the methodology in its prompt and prior-year audits as examples), an eval (previously reviewed audits) and a human (a reviewer signs off).

![Four-Step Process](../slides/10-13-1130-your-financial-agent-passed-every-eval-c/01.jpg)
*A typical build: inputs, agent, eval, human sign-off.*

The next slide, "Ship to GA: Everyone Wins," showed three green boxes: evals 97%, metrics green, users happy. Then a line appeared under them: "Selected for inspection: why was that sufficient?" (The author's recording starts a few slides later, so what Veeraragavan said over these opening slides is not captured here.)

![Everyone Wins](../slides/10-13-1130-your-financial-agent-passed-every-eval-c/03.jpg)
*Evals 97%, metrics green, users happy, and then the inspector's question.*

## 1. Why defensibility matters in finance

![Fraud is real: Peregrine Financial Group](../slides/10-13-1130-your-financial-agent-passed-every-eval-c/04.jpg)
*Peregrine Financial Group, July 2012: "On paper: $223.8M. At the bank: ~$5M."*

The stakes slide recalled Peregrine Financial Group, which collapsed in July 2012 after "forged bank statements, for nearly 20 years." "They claimed they had over $200 million, and in reality, at the bank, there's only $5 million," Veeraragavan said. (The CFTC's July 2012 complaint put the claimed customer funds at more than $220 million and the actual balance at about $5.1 million; the source of the slide's exact $223.8 million is unclear.) "I'm not here to litigate who or what or why this happened, but what this shows us is that in the financial industry, the consequences of being wrong are quite severe. So we need to have confidence behind the decisions that we make, and we need to be able to stand behind them in case things do go awry in the future."

For a standard, Veeraragavan borrowed from the audit profession: "An experienced auditor who hasn't done the work before should be able to understand the tests you performed, the evidence you collected, the decision that was reached. In other words, an experienced auditor new to the work can follow it." (The idea matches the PCAOB's audit documentation standard, AS 1215, though Veeraragavan did not name a source.) Applied to agents, the test becomes: "an experienced auditor new to the work can follow the agent."

Following the agent requires four controls: authorization, execution control, evidence provenance and evaluation integrity. "And ultimately, all of this rests upon knowing when the agent should stop and the human should decide instead." Veeraragavan called it "a pattern for decision assurance" that "really applies to many more domains" than audit. Each of the remaining slides carried five tabs across the top, one for each control plus "Human Decides."

## 2. The example: a year-end bank reconciliation

![Bank Reconciliation, 12/31/2026](../slides/10-13-1130-your-financial-agent-passed-every-eval-c/05.jpg)
*A fictional client's year-end bank reconciliation, and four of its 41 reconciling items.*

The running example was a fictional client, Calder Freight Lines, closing its books for 2026. The bank shows about $2.9 million and the client's books about $2.7 million; "the difference is explained by 41 reconciling items." The slide's reconciliation:

| Line | Amount |
| --- | --- |
| Balance per bank | 2,914,600 |
| + Deposits in transit | 580,150 |
| − Outstanding checks | (731,420) |
| **Adjusted bank balance** | **2,763,330** |
| Balance per client | 2,763,330 ✓ |

Veeraragavan picked four of the items, "two checks, two deposits," to follow through the controls:

| Item | Type | Counterparty | Amount |
| --- | --- | --- | --- |
| OC-14 | outstanding check | Brightline Fuel | 62,500 |
| OC-22 | outstanding check | Rook Logistics | 96,400 |
| DIT-02 | deposit in transit, 12/31 | Norvale Foods | 61,900 |
| DIT-03 | deposit in transit, 12/31 | Lakeshore Retail | 480,000 |

## 3. Authorization: same call, different state (OC-14)

![Same Call, Different State](../slides/10-13-1130-your-financial-agent-passed-every-eval-c/06.jpg)
*The agent may draft unreviewed rows as itself; it may only propose changes to reviewed rows, and never replace a human's flag.*

In the first scenario, a human reviewer going through the items flags one of the checks: there may be something wrong here. A few days later, more information arrives explaining why it is not an exception. That triggers an agent run, and the agent rewrites the item to "no exception."

"The thing here is that a human already reviewed it, and they already signed it. But what they reviewed, the content behind it has now changed," Veeraragavan said. The agent did nothing wrong in the usual sense: "It had the permission, it had the evidence, and it arrived at the right conclusion. But in this specific instance, it shouldn't have overwritten anything, because now what the reviewer has claimed to review doesn't match what they actually looked at."

The lesson: "Authorization isn't just about 'is this tool call allowed.' It's 'is this tool call allowed for this object, given the state at this point in time.'" The slide spelled out the rules. The agent may draft unreviewed rows, as itself. It may not write to a reviewed row; it should propose instead. And it may not replace a human's flag. Its output "has to exist as its own suggestion, as opposed to its own authoritative truth."

## 4. Execution control: a record, not a log (OC-22)

!["We Had Logs."](../slides/10-13-1130-your-financial-agent-passed-every-eval-c/08.jpg)
*OC-22: $96,400 with no exception in 2027; $69,400 as an exception on the 2028 rerun. A log versus a record.*

In the second scenario, the agent reviews OC-22 in 2027 and finds no exception. A year or two later the item is selected for review, rerun, and comes out differently. The slide's header line: "OC-22 · 2027 run: $96,400 no exception · 2028 rerun: $69,400 exception." How could that happen?

"We have logs. We know that the agent fetched evidence, it did some reasoning, and outputted its final answer. But that's not enough to reconstruct what happened," Veeraragavan said. The slide compared the two:

| What we had: a log | What we needed: a record |
| --- | --- |
| `fetch_evidence("January statement")` | Claim |
| Final text | Evidence @hash + page + values read |
| A "reasoning" paragraph | Rules applied |
| | Run · model version · config |
| | Human decision |

![A Run Is a Chain of Recorded Steps](../slides/10-13-1130-your-financial-agent-passed-every-eval-c/09.jpg)
*Fetch (code), extract (model), match (code), conclude (model): each step saved with its inputs and output.*

A run should be a chain of recorded steps, each with an ID (run · item · step) and its inputs and output saved. The slide's chain for OC-22:

| Step | Done by | Recorded |
| --- | --- | --- |
| Fetch | code | statement STMT-JAN-4471, p.3, hash …9c1e |
| Extract | model | check #20977 · 01/08 · 96,400.00 |
| Match | code | 96,400 = 96,400 ✓ |
| Conclude | model | "Cleared 01/08. No exception." |

The chain also separates deterministic steps, "where you're using code to fetch an item or to do a match," from model steps, "where you're using the agent to extract a field from a PDF or to arrive at the final conclusion." With it, "you can then rerun individual steps as opposed to the whole thing," compare a new run against the old one, and "identify the exact step where something went wrong."

![Rerun One Step, Not the Run](../slides/10-13-1130-your-financial-agent-passed-every-eval-c/10.jpg)
*Rerunning only the extract step against the same pinned page shows which run was right.*

The slide's code reran just the extract step on the same pinned input with the current model and stored the result as a new linked record:

```python
orig = steps.get(run="R-0031", item="OC-22", step="extract")
new  = extract(orig.input.page_sha, model=CURRENT)   # same pinned input
steps.put(new, parent=orig.id)                       # new record, linked
```

The result: "Today's model: 69,400 · Pinned page: 96,400 → the original stands." The newer model had misread the amount, transposing two digits. "Our original run was actually correct, even though this newer run is wrong," Veeraragavan said. "But again, it's not so much about whether you're correct or not. It's about being able to demonstrate with a high level of conviction that you know you're right. You didn't just get lucky that the model picked the right answer."

## 5. Evidence provenance: a citation is an identity (DIT-02)

![Right Bank, Right Month, Wrong Account](../slides/10-13-1130-your-financial-agent-passed-every-eval-c/11.jpg)
*The agent cited "January bank statement, p.2," but the $61,900 credit was in the payroll account, not the operating account under test.*

In the third scenario, the model reached the wrong conclusion about DIT-02. "It said that this was valid, but it actually pointed to the wrong account." The agent cited "January bank statement, p.2," where it found $61,900 from Norvale Foods on 01/05, in account -4488 (payroll). The account under test was -4471 (operating), which had no $61,900 credit in January.

The citation itself was the problem. "January bank statement, page 2" isn't useful, Veeraragavan said, because a large engagement can have many documents with that name. "Even if this is a unique document name, what version did the model look at, who uploaded this, where did this even come from?" And "this isn't even related to hallucination. It can be a real document."

![A Citation Is an Identity, Not a String](../slides/10-13-1130-your-financial-agent-passed-every-eval-c/12.jpg)
*Resolve the source from the retrieval record, not the model's text, then check what the claim depends on.*

"A citation is identity. It's not just a string," Veeraragavan said. "You have a very specific ID, a very specific version of the document, a hash of the contents behind it." The slide's code looked up the source from the run's retrieval record rather than from the model's text, then asserted that it matched the item:

```python
src = run.retrieval_record[claim.source_ref]   # id + version + hash, not model text
assert src.account == item.account             # check what the claim depends on
assert required_rules(item) <= claim.rules     # and every rule it had to apply
```

Once a claim is tied to a concrete source, rules and quality constraints can be applied to the evidence. For example, "we may only want evidence that's sourced directly from the bank, as opposed to something that the client uploaded where they may be able to tamper with it."

## 6. Evaluation integrity: audit the gold (DIT-03)

![Audit the Gold, Not Just the Agent](../slides/10-13-1130-your-financial-agent-passed-every-eval-c/13.jpg)
*Gold = last year's reviewed audits. Some gold punishes a right agent; some rewards a wrong one.*

The eval was built from last year's reviewed audits. "Just because the audit was reviewed doesn't necessarily mean it's correct," Veeraragavan said. Mistakes in the dataset "could punish a right agent, or it could reward a wrong agent." The slide split the audit of the gold data in two:

| Audit the misses: gold that punishes a right agent | Audit the hits: gold that rewards a wrong agent |
| --- | --- |
| **Superseded:** the final call was in a review note | **Outdated:** changes in methodology |
| **Inquiry-only:** no document shows it | |

"Your underlying gold data may not be as gold as you thought it was."

![The Score Hid It](../slides/10-13-1130-your-financial-agent-passed-every-eval-c/14.jpg)
*97% overall; four of the nine misses were bad gold; a late-clearing slice of 12.*

The slide showed what that looks like in practice:

| V1 overall | The 9 misses | Late-clearing slice |
| --- | --- | --- |
| 97% ✓ ships | 4 were bad gold, not the agent | 0 / 12 scored "correct" |

"Out of nine of the misses, four was because the eval itself was just wrong. So our agent is actually doing better than expected," Veeraragavan said. "But there are 12 examples where the eval said this should be no exception, but because of some business rule changes or because of some methodology changes, these are now wrong. So these are examples where the agent actually missed and was incorrect." In other words, per the spoken explanation, the old gold marked all 12 late-clearing items as correct when, under current rules, none were. The conclusion: "We don't know if our agent is doing better than expected or doing worse than expected. The score is hiding a lot of important context."

## 7. When the agent should stop and ask a human

![Stop and Ask a Human When…](../slides/10-13-1130-your-financial-agent-passed-every-eval-c/16.jpg)
*Five triggers for escalation, and an example message from the agent to the preparer.*

The fifth tab, "Human Decides," listed when the agent should stop and ask:

1. It would change work a human signed
2. A rerun disagrees with the record
3. The evidence doesn't match the item
4. A required rule fails
5. Sources disagree

Each ties back to one of the scenarios. When one occurs, "something has gone awry and a human needs to step in to determine what's happening," Veeraragavan said.

The way escalations are written matters. "These escalations, they should be easy to verify, but not necessarily easy to approve. We know automation bias is real: if you continually ping the human, they'll just start hitting yes to everything." So the agent should flag only exceptional cases and say clearly what is wrong and where someone needs to step in. The slide's example message from agent to preparer: "DIT-03: cleared 01/19, 11 business days out, from an affiliate. Investigate before concluding."

The review may take the human some time, "but that's okay, because we know 80% of the time the agent will do what it needs to do, and for those 20% of cases where we want the human to step in, we want to make sure they're stepping in where it counts and they have the context they need." (The 80/20 split was spoken, not on a slide, and no source was given.)

## 8. Defensibility is an AND

![Defensibility Is an AND](../slides/10-13-1130-your-financial-agent-passed-every-eval-c/17.jpg)
*Skip any one control, and one of the four items slips through.*

"Defensibility is an AND, in the sense that you need all of them; otherwise things can always slip through," Veeraragavan said. The slide tied each control back to its failure:

| Skip… | …then |
| --- | --- |
| Authorization | OC-14: signed by a reviewer who saw a different answer |
| Execution control | OC-22: right answer, but you can't show what ran |
| Evidence provenance | DIT-02: cites the wrong account |
| Evaluation integrity | DIT-03: a late deposit the 97% eval scored "correct" |

Skip execution control and "you can arrive at the right answer but you can't show how. You may as well attribute it to luck at that point." Skip evidence provenance and the answer is wrong with no idea where it came from, "some citation that you can't tie back to a concrete source." Skip evaluation integrity and "you can arrive at a great flagship number of 97%, [but] you have no idea what the reality is, if you did better than that or you did worse than that."

## 9. Beyond audit

![Same Four Questions, Different Nouns](../slides/10-13-1130-your-financial-agent-passed-every-eval-c/18.jpg)
*The same questions for credit, anti-money-laundering and fund valuation.*

The pattern applies to other financial decisions, "where you may change the nouns behind them," Veeraragavan said. The slide gave three:

| Domain | Decision | Questions |
| --- | --- | --- |
| Credit | Loan declined | Which bureau file, which model version, which credit policy? |
| AML | Alert closed | On which transactions, by whom, with what evidence? |
| Fund valuation | Position marked | From which price source, as of what time? |

![The model said so is not a defensible answer](../slides/10-13-1130-your-financial-agent-passed-every-eval-c/19.jpg)
*The closing slide.*

"Anytime you're making a financial decision where the impact could be quite tremendous, you need to have some defensibility behind how you're arriving at that point," Veeraragavan said. "And if your defense is 'the model said so,' that's not really going to cut it." The closing slide read: "'The model said so' is not a defensible answer." Veeraragavan noted that Fieldguide is hiring and invited people to continue the conversation during the conference.

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=30afe596-abe2-417c-bd5a-313fa4bc54b3) · [CFTC complaint against Peregrine Financial Group (Release 6300-12)](https://www.cftc.gov/PressRoom/PressReleases/6300-12) · [Peregrine Financial Group (Wikipedia)](https://en.wikipedia.org/wiki/Peregrine_Financial_Group) · [PCAOB AS 1215: Audit Documentation](https://pcaobus.org/oversight/standards/auditing-standards/details/AS1215) · [Auditing an audit agent](https://www.fieldguide.com/blog/auditing-an-audit-agent) (Fieldguide blog, by Kunal Sinha; related topic, not by the speaker)

No video of this session had been published at the time of writing. This write-up is based on the author's room recording (a phone recording that begins at the Peregrine slide and ends after the speaker's thanks, with a garbled middle stretch re-transcribed from the phone audio), slide photos and the sources above. The microphone that kept running afterwards picked up nothing intelligible, so whether there was a Q&A could not be determined. The [precise notes](precise/10-13-1130-your-financial-agent-passed-every-eval-c.md) (Chinese) keep timestamps and the full record of the transcript corrections.
