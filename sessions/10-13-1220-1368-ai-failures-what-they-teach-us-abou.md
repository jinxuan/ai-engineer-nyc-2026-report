# Evaluate the Whole AI System by Risk, Not by How Often Things Fail

*At AI Engineer NYC 2026, Vishal Srivastava and Tanmay Sah mapped 1,368 publicly reported AI incidents onto 13 failure dimensions and argued that the controls around a model, not the model itself, most often decide whether a failure turns into harm.*

**Breakout (Evaluations Track) · Tuesday, October 13, 2026 · 12:20–12:38pm · Turing Room (Riverside Ballroom)**
**Speakers:** Vishal Srivastava, PhD, Corporate VP and Lead Data Scientist, New York Life Insurance; Tanmay Sah, AI Researcher

---

Vishal Srivastava introduced the research as a habit: "A big part of my research is looking into AI failures, investigating it, and then learning from it." Tanmay Sah described themself as "a senior quantitative modeler and an AI researcher."

The talk started from a public dataset. "We looked at AI incident database. It's a publicly available AI incident database," Srivastava said, "and we looked at the AI failures and asked one simple question, what these failures teach us about how … the AI system should be evaluated." The speakers did not name the database or the date of the snapshot. (The numbering of the public AI Incident Database reached No. 1,368 in early 2026, which fits the title, but the talk never said so.)

AI failures, Srivastava said, "have been increasing exponentially," and over the last four years, the foundation-model era, "the jump is almost vertical." (There is no photo of that opening chart, so its figures are not reproduced here.) Inside a company, the number of AI use cases is "much larger than our capacity to review them each one by one," which calls for "a structured way to investigate these AI failures."

## 1. A framework of 3 questions and 13 dimensions

![Full AI System Evaluation Across 13 Dimensions](../slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/03.jpg)
*The framework as an inverted triangle: six technical dimensions, four deployment controls, three business impacts.*

The framework slide, titled "Full AI System Evaluation Across 13 Dimensions," drew an inverted triangle with layers of 6, 4 and 3. A bar down the left side said it applies to traditional ML, generative AI, RAG and agentic AI. Each layer asks one question, and each of the 13 sub-dimensions asks its own:

| Pillar | Question | Sub-dimension | Question on the slide |
| --- | --- | --- | --- |
| **Technical Failure** (6) | Where can the system break? | Data Foundations | Are the input data quality and resources reasonable? |
| | | System Design | Is the system designed appropriately for the task? |
| | | Predictive Validity | Does the system perform as claimed? |
| | | Fairness | How are outcomes and errors proportional to different groups? |
| | | Failure Modes | Where, when, how, and for whom does the claim break? |
| | | Explainability | Can behaviour and decisions be explained reasonably? |
| **Deployment Controls** (4) | Can that failure reach the real world? | AI Authority | What is the AI allowed to decide or act or inform? |
| | | Human Mediation | Can a human meaningfully intervene? |
| | | Exposure Control | Who is using the AI system, internal versus external? |
| | | Harm Detection | Can emerging harm be recognised in time? |
| **Business Impact** (3) | How much does it matter when AI fails? | Financial | What financial consequence could result? |
| | | Legal | What legal or regulatory consequence could result? |
| | | Scale | How large is the impact? |

Srivastava walked the layers as a lifecycle. Errors made in development, in data quality, tool architecture or system design, pass into deployment. There, "even if the model or the system has some faulty design issues, but if you have sufficient controls in deployment, those errors can be contained." What remains lands in a business context as reputational, legal or financial risk. "The model alone or the technical strength of the model is just one part of the system. We have to look at the system holistically."

Two of the deployment dimensions got extra explanation. AI Authority asks: "Is the AI for informing us or it is out there to act on our behalf?" Exposure Control distinguishes internal employees, who can see an error and contain it, from "external customers," where "any small issue can really propagate fast." Srivastava summed it up as "one framework which has 13 dimensions and three core questions." (Two earlier photos show the same slide building up layer by layer.)

## 2. Nearly every failure fits, and most fit several dimensions at once

![AI Failures Map to the Same 13 Dimensions](../slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/04.jpg)
*97.7% of the 1,368 incidents map to at least one of the 13 dimensions.*

The next slide tested the framework against the incidents. A ring chart showed that **97.7% of 1,368 incidents** map to at least one of the 13 dimensions, under the line "AI failures have structure. We can map it." On stage Srivastava rounded it to "almost 98%," adding that failures are associated with these dimensions, "in some cases multiple of these dimensions." By that figure, about 31 incidents fit none of them.

![AI Failures Occur Across Multiple Dimensions Simultaneously](../slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/05.jpg)
*How many of the 13 dimensions each incident touches; the median is six.*

A histogram counted how many dimensions each incident spanned, from 0 to 13. The tallest bar was at 7 (about 145 incidents), a dashed line marked the median at 6, and the caption read "The typical AI failure spans nearly 6 dimensions." Srivastava said "on average, there are six failure sub-dimensions" in each incident; the slide gives the median, not a mean. The session description had promised that "92% of incidents implicated multiple weaknesses simultaneously"; that number was not said on stage, though the histogram's bars at 0 and 1 together come to roughly 8% of incidents, consistent with it.

## 3. What fails most often is not what matters most

![What Fails Most Often Isn't Necessarily What Matters Most](../slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/06.jpg)
*Each dot is one dimension: how common it is (x, log scale) against how strongly it is linked to harm (y).*

This scatter plot was the center of the talk. Each dot is one of the 13 dimensions, measured across the 1,368 incidents, of which 206 involved material harm. The x-axis shows how common a severe rating on that dimension is, on a log scale; the y-axis shows lift, how much more often the dimension appears when harm occurs, from 1× to 5×. Two dots were labeled on the slide:

| Dimension | Frequency (severe rating) | Lift toward harm |
| --- | --- | --- |
| Human Mediation | rare: 0.95% of incidents | 4.8× |
| System Design | most common: 38% of incidents | about 1× |

The other dots were unlabeled. Read by eye, Harm Detection sat around 2.7× and Exposure Control around 2.3×, Fairness, Legal and Predictive Validity around 1.8–1.9×, AI Authority around 1.5×, and Financial, Scale, Data Foundations and the fifth technical dimension near or below 1×. (This chart calls that dimension "Failure Analysis"; the other slides say "Failure Modes.")

Srivastava described the top-left corner: human mediation, the "effectiveness of human in the loop," and harm detection, "whether we have monitoring control in place." Scale and system design are "present in many cases, but they are not associated so much with harm." The conclusion, on the slide as "Prioritize by potential harm, not frequency": "we need to prioritize these sub-dimensions by the potential of making harm, and then allocate appropriate resources … rather than looking at all the failure sub-dimensions equally."

![Deployment Controls Shape Whether Failure Reaches Harm](../slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/07.jpg)
*Lift of each pillar in harmful incidents compared with the rest.*

The same comparison at the level of the three pillars defined material harm as "fatality, legal action or discrimination," 206 of the 1,368 incidents. The bars showed how much more often each pillar appears in harmful incidents than in the others, where 1× means equally common:

| Pillar | Lift in harmful incidents |
| --- | --- |
| Technical Failure | 1.10× |
| **Deployment Controls** | **2.43×** |
| Business Impact | 1.46× |

"Controls that we have in deployment are more critically associated with harm, followed by the business impact, and followed by the technical failure," Srivastava said. "All these sub-dimensions are important, but deployment controls are far more important."

The speakers' companion article on DEV reports a different set of measures, against fatal outcomes rather than material harm: a 4.7× lift for severe monitoring-and-detection failures and 2.8× for high AI decision authority. The article also states that these are "associations within reported incidents, not causal estimates," subject to reporting bias, a caveat not repeated on stage.

## 4. Materiality decides how deep to evaluate

![Materiality Determines Evaluation Depth](../slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/08.jpg)
*A materiality score designed up front, then tested retrospectively on the incidents.*

If harm matters more than frequency, an organization needs to know which use cases could cause it. The slide had two halves. The design half described short heuristics "not fitted to harm data": six technical inputs, four deployment-control inputs and three business-impact inputs, combined by multiplication, each input scored 1–3 against written criteria, and "when unsure, rate higher." The tiers were Low (≤4), Medium (≤8), High (≤15) and Critical (>15). The left edge of the photo is cut off, so exactly how the inputs within each pillar are combined before multiplying is not visible.

The test half applied the score retrospectively to the 1,368 incidents:

| Materiality tier | Material-harm rate | Incidents |
| --- | --- | --- |
| Low | 9.1% | 36 of 396 |
| Medium | 15.9% | 87 of 548 |
| High | 17.7% | 66 of 373 |
| Critical | 33.3% | 17 of 51 |

The tiers add up to 1,368 incidents and 206 harmful ones, matching the earlier slides. Harm rises with each tier, and Critical, though only 51 incidents, has a harm rate more than three times Low's. "Materiality directly rank orders with respect to the harm associated with this AI use cases," Srivastava said, so a team can "allocate more resources to much higher material AI use cases." The slide's tagline: "Materiality sets how deep we evaluate."

## 5. Risk as a chain of four probabilities

![Decomposing AI System Level Risk](../slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/09.jpg)
*Expected harm as the product of four factors, three of them set by the deployment around the model.*

Tanmay Sah took over with a formula. Expected loss for a deployment setup A is the product of four factors:

| Factor | Meaning | Set by |
| --- | --- | --- |
| P(fail) | The model gets it wrong | Model quality, inputs |
| P(reach \| fail) | The error reaches a real decision | Authority, exposure |
| P(miss \| reach) | Nobody catches it in time | Human review, monitoring |
| Severity | How bad it is when it lands | Financial, legal, scale |

Here A is "the deployment setup around the model: authority · exposure · human review · monitoring." The slide cited the speakers' own preprint, arXiv:2602.18986, as "adapted"; that paper's abstract uses three factors (failure, failure becoming harm at a given level of automation, and severity), with the 2012 Knight Capital loss as its case study, and the slide splits the middle one in two.

Sah read the four factors as "how often the model fails, whether that failure reaches a real decision, whether it goes uncaught, and how severe the consequences are." Only the first belongs to the model. "This same model can create different risk depending on how we deploy it," Sah said; the slide put it as "Same model, different A, different risk." Hence: "if risk depends on the whole deployment system, our evaluation needs to cover that setup, too."

## 6. In agentic systems, the evaluator needs governing too

Sah then contrasted what and how to test. (There are no photos of these two slides.) Traditional model risk work already "challenges" models by testing assumptions, input data and behavior under stress. Extending that to a whole deployed system means asking: "does retrieval bring back the right information? Or what happens when a tool fails? Does human review catch mistake? Or does the agent stay within its permission or boundary? And can the system finally recover?" In Sah's words, "the logic stays the same. The boundary of what we test gets bigger."

On how to test, traditional ML computes accuracy or AUC-ROC on held-out data and uses that as evidence for a deployment decision, though "we still need to check the data and assumptions behind those metrics." An agentic system has too many combinations of inputs, tool responses and action sequences ("or trajectory, or rollout") to cover, so only selected scenarios can be tested, and often another agent or an AI judge generates the tests or grades the outcome, "but that judge can make mistakes, and the test can miss important failures." So the team must check "whether the evaluator is reliable, what the evidence covers, and whether it applies to the system we are actually deploying." Sah's conclusion: "That's why the evaluator is the part of the system we govern."

## 7. Good evals try to falsify: the 2-4-6 task

![Why good evals need to seek failure not confirmation](../slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/10.jpg)
*Peter Wason's 1960 rule-discovery task, used as an argument for failure-driven evals.*

Srivastava returned with a classic experiment from Peter Wason at UCL in 1960, whom Srivastava credited as "the one who coined this term confirmation bias." Participants are shown 2–4–6 and asked for the rule. The likely hypothesis is "ascending even numbers," and testing 8–10–12 and 20–22–24 confirms it: "more confirmation → more confidence." The actual rule was any three increasing numbers, which 1–2–3 or 3–8–100 would have revealed. According to the slide, 79% of participants "tested only triples that fit their hunch." (On stage Srivastava said "79% of people tested wrong," a different measure; which figure in Wason's paper the 79% refers to was not verified.)

The link to AI: "AI systems, as we know, are trained on human-generated data," and an evaluation "has to falsify, not to confirm," so the eval system "needs to be failure-driven." The slide's tagline: "Good evals try to falsify, not confirm."

## 8. Build, break, fix, break again

![How should we operationalize falsification?](../slides/10-13-1220-1368-ai-failures-what-they-teach-us-abou/11.jpg)
*A four-step loop around "System Evals."*

The loop on the next slide had four steps:

| Step | What it means |
| --- | --- |
| Build | Define the system claim and deployment boundary. |
| Break | Seek disproof across failure regions, controls, authority, and workflow. |
| Fix | Intervene at the right layer: model, workflow, scope, authority, or controls. |
| Break again | Verify the repair and search for new failure modes. |

Start from what the system claims to do, Srivastava said, and try to break it to find where the claim fails. A fix can take two forms: "don't let the AI system run in … the territory where there are failures," or add "supporting systems" the team is confident in. Iterate until the required confidence is reached. "This is essentially the first principles based approach." The output is a map of trusted, "uncertain" and "high-risk" regions, routed "based on the reliability and materiality"; for high-risk systems, "make sure that your AI is only being used for … the region that you're confident of."

## 9. A case: the airline chatbot

Sah closed the method with a case (no slide photo). A passenger whose family member had died asked an airline's chatbot about a discounted fare. The chatbot said to buy a regular ticket and "claim the discount afterwards within 90 days." The passenger did; the airline refused, citing its policy, and "the tribunal held the airline responsible." Sah did not name the airline; the facts match *Moffatt v. Air Canada* (2024 BCCRT 149), decided by British Columbia's Civil Resolution Tribunal in February 2024.

The failure chain: "incorrect advice reaches the customer, the customer acts on it, and the organization faces the consequences." Build–break–fix would set the requirement "advice must match the approved policy," challenge the chatbot with "questions about refunds after travel," add "a policy check or human escalation" where errors show up, and test again to see whether those controls stop the error before it reaches a customer. "Define the claim, actively challenge it, and use the evidence [to] guide changes and make a deployment decision."

## 10. Takeaways

Srivastava summarized (the closing slide was not photographed). Evals "should be risk-based … rather than frequency-based." Break the system into technical failure, deployment controls and business context; rate use cases on a materiality scale ("that could be your company specific scale"); stress-test at the system level; and "once you are confident about the region, then only make the deployment decision." The final line: "Evaluate the full AI system based on risk, not just one component." The speakers offered to "take any questions later"; there was no Q&A in the session.

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=9d72d7ac-a2a2-42cf-b880-5859316d840c) · [Video](https://www.youtube.com/watch?v=mMw3arwQsL0) · [What 1,368 Real-World AI Incidents Teach Us About Evaluation](https://dev.to/dailycontext/what-1368-real-world-ai-incidents-teach-us-about-evaluation-71p) (DEV / Daily Context) · [Srivastava & Sah, arXiv:2602.18986](https://arxiv.org/abs/2602.18986) · [AI Incident Database](https://incidentdatabase.ai/) · [CTV News on Moffatt v. Air Canada](https://www.ctvnews.ca/vancouver/article/air-canadas-chatbot-gave-a-bc-man-the-wrong-information-now-the-airline-has-to-pay-for-the-mistake/)

This write-up is based on the official video captions, the author's room recording, slide photos and the sources above. The recording ends without a Q&A. The [precise notes](precise/10-13-1220-1368-ai-failures-what-they-teach-us-abou.md) (Chinese) keep timestamps and the full record of where the talk, slides and article disagree.
