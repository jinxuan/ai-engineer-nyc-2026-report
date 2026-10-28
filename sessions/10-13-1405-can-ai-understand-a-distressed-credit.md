# Two Frontier Models Disagreed on a Distressed Credit Until Their Reasoning Was Structured

*At AI Engineer NYC 2026, Axim Intelligence founder Harneet Kaur showed two frontier models giving opposite outlooks for iHeartMedia's debt, then converging once their analysis was broken into checkable layers, and asked whether agreement is the same as being right.*

**Breakout · Tuesday, October 13, 2026 · 2:05–2:23pm · Turing Room (Riverside Ballroom)**
**Speaker:** Harneet Kaur, Founder and CEO, Axim Intelligence (previously Morgan Stanley and a distressed credit investor)

---

Harneet Kaur introduced Axim Intelligence in one line: "we're building the intelligence layer for distressed credit investing." Kaur had "spent 10 years in finance," the last few "as a distressed credit and special situations investor" at a distressed private-equity and hedge fund. The talk asked whether a model can understand a company in financial trouble well enough to say what happens next.

## 1. What a distressed credit is

![What is a distressed credit?](../slides/10-13-1405-can-ai-understand-a-distressed-credit/01.jpg)
*A credit's slide from performing to distress, and the three ways it can end.*

"Credit is just a bond or loan … of a corporation," Kaur said. "The bond of Coca-Cola is a credit." A credit is stressed when the company risks two things: to "run out of money" and to "not be able to pay back their debt."

The overview slide drew a curve falling through three stages, then splitting into three outcomes:

| Stage | Slide description |
| --- | --- |
| 01 Performing | Debt is serviced. |
| 02 Deterioration | Liquidity tightens; covenants are tested. |
| 03 Distress | Unable to refinance debt or liquidity challenged. |

| Outcome | Slide description |
| --- | --- |
| Negotiated exchange | Out-of-court exchange of debt |
| Liability management exercise (LME) | Up-tier or drop-down exercise |
| Bankruptcy | Files for in-court Chapter 11 |

Some part of the credit market is in stress or distress every year, Kaur said, for cyclical reasons (a downturn in construction or real estate), operational ones (a food recall at a restaurant company) or secular disruption: "if you're a software business today, and maybe seeing some AI disruption." And distress has more than one ending. "What does distress mean? It's actually not straightforward. Bankruptcy is one outcome of distress"; others are a liability management exercise or an "out-of-court restructuring, negotiated exchange."

## 2. Two old worlds: statistical models and human analysts

![Distressed analysis has historically lived in 2 worlds](../slides/10-13-1405-can-ai-understand-a-distressed-credit/02.jpg)
*Statistical default models on one side, credit managers on the other, each with its limitation.*

The next slide divided the history of the field in two:

| | Machine learning models | Credit managers |
| --- | --- | --- |
| Task | Predict deterioration / default | Analyze credit investments |
| Tools | Altman Z-Score; Chava–Jarrow; modern ML | Underwriting; watchlists; investment committee |
| Limitation | Strong on statistical risk. Weak on context. | Rich context. Highly manual and hard to scale. |

"Historically, the engineering question for distress was pretty binary," Kaur said: default or no default. "Today, that's become a lot more complex." Scores that mattered 10 or 20 years ago, "the big one is the Altman Z-score," are too static for a situation that "changes, you know, sometimes monthly, quarterly, yearly." Markets have also become less transparent: "the rise of private credit and some of these other areas have caused, you know, a lack of pricing." (The Z-score comes from Edward Altman's 1968 paper in *The Journal of Finance*; Chava–Jarrow refers to Sudheer Chava and Robert Jarrow's 2004 bankruptcy-prediction model in the *Review of Finance*.)

The other world is qualitative: reading financial statements, understanding the lenders' side, reading the credit documents. "This process is super manual, very hard to scale."

## 3. The question: what does it mean to understand a credit?

![Can AI understand a distressed credit?](../slides/10-13-1405-can-ai-understand-a-distressed-credit/03.jpg)
*Ten questions an investor asks about iHeartMedia's credit situation.*

When Kaur started Axim, the question was "how far can AI get at understanding a distressed … credit and the type of paths you can take. Um and can you systematize what is effectively a pretty complex process." The slide put iHeartMedia's logo at the center of ten questions: What is the capital structure? What is structurally senior? What changed post-LME? How much liquidity is there? How does the ABL (asset-based loan) maturity affect the situation? What would recoveries be? What is the next event? What changed since last quarter? What are the key near-term catalysts? How does today compare to 2018?

Kaur grouped such questions as financial (liquidity, capital structure), legal ("what's the structural priority of the debt") and operational (will revenue rise or fall). Companies such as "Rogo or Harvey or some of these, you know, big AI companies" benchmark models question by question, for instance "what part of the debt is structurally senior?", in what Kaur called "the big finance bench."

Kaur's view was different: "the true understanding, right, of credit, and particularly distressed credit, is can you predict what happens next?" That means having "a probability distribution of what path this takes in the future."

## 4. The test case: iHeartMedia, and two models that disagreed

![There's no "ground truth" for predicting stress or distress](../slides/10-13-1405-can-ai-understand-a-distressed-credit/04.jpg)
*Two models' probabilities for iHeartMedia's path through 2029, with an empty ground-truth column.*

Kaur chose iHeartMedia, "a company that filed for a bankruptcy in 2018 … it's been through the rigmarole. Um it also went and did … another stressed transaction back in 2024. So, this is a live situation … there's no ground truth." (Public filings show iHeartMedia filed for Chapter 11 in March 2018 and emerged in May 2019; in December 2024 it completed an exchange in which holders of about $4.8 billion, 92.2% of its existing debt, swapped into new notes and term loans, extending most maturities by three years.)

"Let's throw it into the smartest, you know, models," Kaur said. "I did Astra 6 on Max Intelligence um and Opus 5.5." Given "a structure[d] context of the data," the two models disagreed about iHeartMedia's path through December 31, 2029:

| Path | Astra 6 | Opus 5.5 | Ground truth |
| --- | --- | --- | --- |
| Negotiated restructuring | **35%** | 20% | ? |
| Refinancing | 30% | 15% | ? |
| Bankruptcy | 25% | **62%** | ? |
| Aggressive non-pro-rata LME | 10% | 3% | ? |
| Total | 100% | 100% | ? |

Astra 6's most likely outcome was a negotiated restructuring; Opus 5.5's was bankruptcy. "How do you say which is right or wrong? You can't cuz I don't know what the probabilities are," Kaur said. "I have my best judgment on things. I I would say I lean towards Astra 6, but you know, we're all human." Kaur did not describe what the structured context contained or whether the two models received identical prompts.

## 5. DistressedBench: check each layer of the reasoning

![DistressedBench: eval framework for distressed credit analysis](../slides/10-13-1405-can-ai-understand-a-distressed-credit/07.jpg)
*Four layers, each evaluated before the next: extraction, reconciliation/derivation, judgment, prediction.*

If the final answer can't be graded, the steps leading to it can. "How do I improve at least an assessment of their process," Kaur asked, so that "I can look at … whether I agree or disagree with the different assumptions." The result was "this sort of benchmark … we benchmark each layer." The slide named it DistressedBench and showed four steps: Extraction → Reconciliation/Derivation → Judgment → Prediction.

A model can go wrong at every layer. Extraction "with public companies it's … pretty solved," Kaur said, but "with private loans and … when you have like Excel documents and PDFs, that can … still be a challenge." Derivation builds on it: "extraction would be you got EBITDA … derivation would be, okay, now you have EBITDA and you have debt, now you can have leverage." Judgment asks: "now you have the leverage. Is this a distressed credit or not?"

At this point Kaur's laptop died ("I think my computer died"), and the talk paused for about a minute. Resuming, Kaur explained that by the time the model reaches prediction, "you've assured the data points are good, you've assured that … the reconciliation layer is good, you've assured that there's some sort of reasonable judgment." That mirrors how human analysts work: "If you build a judgment on a bad data point, that's not going to be a good judgment." Kaur did not say how many questions DistressedBench contains, who labels them or how each layer is scored; Axim's website lists it as "coming soon."

## 6. The result: the models converged

The results slide was not photographed. Kaur reported that "once we actually created the structured reasoning process, … both the models actually converge in terms of what they believed the future probabilities of the business are going to be." Instead of "62% bankruptcy with Opus and 25% bankruptcy with Astra, now both these models are saying there's actually only a quarter or 25% chance of bankruptcy and both models agreed that the negotiated restructuring path is actually the more likely path." Kaur did not give the converged probabilities for the other three paths. The converged bankruptcy figure equals Astra 6's original estimate, the model Kaur had said they leaned toward.

## 7. Agreement is not the same as being right

Kaur drew two conclusions. First, on "non-verifiable questions, right? With the same structured context, organized context as a human … do these models have a predictive edge? And you have to say no." In Kaur's words, "in effect, even agents are human, right? In in some sense of the word and I won't get into the controversy there." Second, "a structured reasoning process can get the two models … or different intelligences to converge on effectively a same view of the future."

"But is that the correct path? We don't know, right?" A live situation is "the truest measure," Kaur said, because "otherwise any sort of historical analysis would create some sort of look ahead bias." The convergence itself was the point of interest: "if you can systematically build a process that actually converges on the same probabilistic outcomes. That is effectively what is a price in a market."

![Learn more about Axim Intelligence](../slides/10-13-1405-can-ai-understand-a-distressed-credit/08.jpg)
*The closing slide: what Axim does, Kaur's background, and the logos of firms where the founder, team and advisors have worked.*

The closing slide described Axim as building "the intelligence layer for stressed & distressed credit" and listed Goldman Sachs, Morgan Stanley, Meta and Glean under "experience across founder, team & advisors." Kaur ended with "that's all I had. Thank you"; there was no Q&A.

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=adca6e26-1ddc-4f77-8e35-59bf1d1de81d) · [Video](https://www.youtube.com/watch?v=WOszaC1WWbQ) · [Axim Intelligence](https://aximintelligence.com) · [iHeartMedia 2024 exchange completion (SEC 8-K exhibit)](https://www.sec.gov/Archives/edgar/data/1400891/000119312524283675/d854532dex991.htm) · [iHeartMedia Q2 2026 10-Q (SEC)](https://www.sec.gov/Archives/edgar/data/0001400891/000162828026055252/ihrt-20260630.htm) · [Chava & Jarrow (2004)](https://ideas.repec.org/a/oup/revfin/v8y2004i4p537-569..html)

This write-up is based on the official video captions, the author's room recording, slide photos and the public filings above. The session description promised a discussion of "analytical provenance," a term that did not come up on stage. The recording ends without a Q&A. The [precise notes](precise/10-13-1405-can-ai-understand-a-distressed-credit.md) (Chinese) keep timestamps and iHeartMedia's public capital-structure facts in more detail.
