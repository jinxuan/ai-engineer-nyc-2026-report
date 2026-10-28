**English** | [中文](zh/10-14-1220-ab-testing-ai-products-is-weird-what-cha.md)

# A Winning A/B Test Is Where an AI Feature's Ship Decision Starts, Not Where It Ends

*At AI Engineer NYC 2026, Bumble product manager Supriya Subudhi used the dating app's AI photo feedback feature to show five ways a positive A/B test can still leave the question of whether to ship unanswered.*

**Breakout · Wednesday, October 14, 2026 · 12:20–12:38pm · Turing Room (Riverside Ballroom)**
**Speaker:** Supriya Subudhi, Staff Product Manager, Bumble Inc

![Title slide: A/B Testing AI Products Is Weird](../slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/01.jpg)
*The title slide: "What changes when the product is probabilistic."*

---

Supriya Subudhi opened with a definition: A/B testing is "a method used to evaluate two versions of something with each other and see which is better." Then a show of hands: "Who thinks that if an A/B test comes back positive, that the feature should actually ship?" Judging by the response, Subudhi said, "a lot of you are smart and already know that when it comes to AI, things can get a little weird."

In an article published alongside the talk, Subudhi described the rule of thumb behind that question: "If an A/B test wins, you ship it. I've followed it for most of my career." With generative AI, it "stopped holding up." The tests were well designed and the results accurate, "but they just didn't answer the question I needed answered, which was whether we should ship it!"

## The setup: Bumble's AI photo feedback

![When it comes to A/B testing AI features, it gets weird](../slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/02.jpg)
*"A positive result is where the decision journey begins, not where it ends," followed by the five problems.*

The overview slide put the thesis on top: "A positive result is where the decision journey begins, not where it ends." Below it were five problems: users asked for it, then ignored it; the average win hides the tail; green dashboard, red bill; swap the model, flip the result; and there may not be a global rollout. These were problems "that I specifically discovered," Subudhi said, "that we have to overcome." (The session description had listed a different set of failure modes, including novelty effects and premature conclusions; the talk did not cover those.)

Subudhi framed the job of a product manager as representing users and solving their problems "while still being responsible for the P&L of the company, because we don't have unlimited resources." The example throughout was a feature Bumble shipped: AI photo feedback. Everyone on a dating app needs a profile and adds pictures without knowing how they come across. Bumble took its own knowledge of what works in a dating profile and used AI to generate suggestions for improving the pictures people add.

## 1. Users asked for it, then ignored it

![1. Users asked for it, then ignored it](../slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/03.jpg)
*"General openness to AI doesn't predict whether people will use a specific AI feature."*

| What happened | What to do |
| --- | --- |
| Our research showed that users were open to AI tips from Bumble. | Carefully integrate AI into the new user flows with education and awareness as well. |
| When we shipped the AI photo feedback, not many actually used it. | Always measure what people do, not what they say they will. |

Product work starts with discovery through surveys and customer interviews. In Bumble's research, one almost unanimous complaint was that people "didn't know whether their dating profile was good enough" and got no help from Bumble in finding out. When the team showed users examples of AI guidance, "almost 78% of the people that we spoke to came back and said they would love getting feedback." But in the A/B test, with half of users getting the feature, "we only saw 10% to 15% of people actually using the feature." (Those two figures come only from the talk; the slide and the article say "not many.") "Openness to AI doesn't actually predict whether people are going to use the feature."

The fix was "product management 101." Instead of a standalone feature people had to find, Bumble put it into flows people already used. When users added pictures, the app asked whether they wanted feedback. When they were "hitting a wall," not finding matches, it suggested checking the feedback on their profile. "That drove up adoption." In the article, Subudhi wrote that "what made the difference wasn't the model. It was where the feature showed up in the flow," and that AI features need to be "even more descriptive about what the feature will do" to earn trust.

## 2. The average win hides the tail

![2. The average win hides the tail](../slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/04.jpg)
*"A positive average can sit on top of individual outputs that are wrong. So success evaluation itself changes."*

| What happened | What to do |
| --- | --- |
| The top-line results looked fine. | Be strict where the feature can do harm if wrong. |
| Some people got weird feedback, like being told to smile more in a photo where they were already smiling. | Read real outputs (evals can be very helpful here). |
| | Check the long tail of data and split by segments, not just the average. |

The top line was good: more people added pictures, and people who got feedback made more changes to their profiles than those who didn't. In the details, "there were weird things happening." The AI told people to smile more in pictures where they were already smiling.

Subudhi contrasted two products to show how the stakes differ. With Spotify's AI DJ, which generates playlists, "it's okay if two out of four of the songs that are generated by AI are not good. But it's not okay if [a banking] app is telling you to spend $2,000 when you really can't."

Bumble started with guardrails on what the feedback could and could not say: "We couldn't be telling people to change things that they can't control, their skin color, their height. These are things that the AI should not touch." Next came reading real outputs, using eval tools to see what real users were receiving and whether the prompt was doing a good job. The success criteria shifted from the overall comparison to the long tail: "Are women responding the same way to our feedback as men?" Do people in different countries respond the same way, "because maybe the language generated in French is different from the language generated in English"? The article explained why this is new: "With a normal feature, everyone in a test arm sees the same thing. With a generative one, everyone sees something slightly different."

## 3. Green dashboard, red bill

![3. Green dashboard, red bill](../slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/05.jpg)
*"With generative AI, success makes the bill bigger." The chart shows the shape of the two cost curves, with no numbers.*

The slide's chart plotted cost against users with two lines: a steep one labeled "Generative feature: cost grows with every use" and a shallow one labeled "Recommendation model: cost grows slowly with users." Beside it (the right edge of the photo is cut off):

| What happened | What to do |
| --- | --- |
| The LLM route worked, but it cost about 10x more than a simpler approach. | Don't jump to using the best model from the start. |
| Add to that the cost of running evals on the gen AI feedback. | Measure the return on short term and long term together. |

Because the problem mattered so much to users, Bumble wanted the best quality and accuracy and "went with the best LLM," which was "about 10x more expensive than a simpler approach." Add the cost of running evals: "That was just too much."

Bumble already uses AI in its matching algorithm, but traditional ML has a different cost structure: high upfront compute and training costs, after which "the incremental cost for every user to use it is not that much." With generative AI, "the more successful your product, the more people that use it, and the more often they use it, the more your bill increases."

Subudhi argued that teams need to know why they are using AI at all; many peers "have just gotten mandates from their companies to use AI." The example was Amazon's shopping assistant. In the short term, good recommendations may lead to more purchases. In the long term, "having an assistant that you can rely on might make people choose Amazon over other e-commerce companies," a matter of brand equity and keeping users on the platform. The budget follows from that purpose: for a short-term benefit, the ROI can be a quick decision; for a long-term one, "maybe it's worth getting that extra budget." The article described the moment bluntly: "That's an uncomfortable meeting to sit in, because the dashboard is green and the answer is still no."

## 4. Swap the model, flip the result

![4. Swap the model, flip the result](../slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/06.jpg)
*"A winning result belongs to the model, not the feature."*

| What happened | What to do |
| --- | --- |
| We switched to a cheaper LLM, and alternative ML models. | Test cheaper and non-AI versions from the start. |
| Costs went down, and so did the impact to results. | Re-test every time you swap the model. |
| | Find the cheapest option that keeps the result. |

The cost of the best model couldn't scale, so Bumble switched to a cheaper one. "As the cost went down, so did the impact of the results because the accuracy went down as well." That showed the A/B result "was due to the model that we were testing, not the concept [of] feedback itself."

The lesson was to test a lot and stay data-driven. Eval setups let the team compare how different models performed on different data sets and what they cost, and retest versions and prompts quickly. On stage, Subudhi said that "ultimately we resulted in the cheapest option which helped us get the good result." The article tells it differently: after the switch, "we were back to square one to find and test a version that would fit within budget and also test positively," without saying such a version was found.

## 5. There may not be a global rollout

![5. There may not be a global rollout](../slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/07.jpg)
*"Regulations are constantly changing and can stop a feature that works in one country from shipping in another."*

| What happened | What to do |
| --- | --- |
| Photo feedback was live in the US. | Bring legal and compliance in before the experiment. |
| It was blocked in most other countries until we had cleared them legally. | Decide early which markets and versions you are building for. |
| | Be ready to steer clear of some markets till the end. |

Subudhi pointed to Meta's recently launched Muse AI: "I'm from London, so I haven't yet [used it]," because it is "only available in the U.S. and Canada." AI regulation differs around the world and keeps changing, and non-compliance carries financial and legal risk. (Reports describe a U.S. launch for Meta's Muse app in September 2026; a Canadian expansion appears in only one report.)

Bumble's feature followed the same path: "We shipped in the U.S., but then that's it. Like, we had to stop." The feature could not ship as-is to other countries. (TechCrunch's February 2026 coverage of Bumble's AI tools also described photo feedback as U.S.-only.) The conclusion: "Whether you're a product manager or not, whether you're an aspiring product engineer, a forward deployed engineer, everyone needs to start being a generalist and thinking about compliance and legal… early on. It can't come later."

Subudhi described a spectrum of choices. A team can plan to ship worldwide from the start and build within every market's restrictions. It can build the best possible version for the U.S., where the impact is greatest, "and then maybe not anywhere else." Or it can pay the extra cost of a version that behaves differently by country. "If you build the best thing, which you can't get in the hands of users, it doesn't mean anything." The article added a point about learning: "You can only learn where you're allowed to test, so a result in one market doesn't tell you much about the next."

## Five questions before scaling

![To scale AI features, ask these 5 questions](../slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/08.jpg)
*The closing slide's five questions, one per problem (edges cut off in the photo).*

Subudhi closed with one question for each problem:

| Problem | Question |
| --- | --- |
| Users asked for it, then ignored it | Are people using it, or only saying they want it? |
| The average win hides the tail | Have you read real outputs, not only the average? |
| Green dashboard, red bill | Does it still pay off after factoring in all costs? |
| Swap the model, flip the result | Is a win on the best model something you can afford? |
| There may not be a global rollout | Are you clear legally on which markets you can ship? |

"Because the question is not just did the feature win A/B tests anymore," Subudhi said. The article ended on the same note: "'Did it win?' is still where I start. It's just not where I stop anymore."

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=11fef189-97e8-4c8e-87e8-dac43d800846) · [A/B testing AI products is weird](https://dev.to/dailycontext/ab-testing-ai-products-is-weird-576h) (Supriya Subudhi, DEV) · [Bumble adds AI-powered photo feedback and profile guidance tools](https://techcrunch.com/2026/02/26/bumble-adds-ai-powered-photo-feedback-and-profile-guidance-tools) (TechCrunch) · [Providing feedback on your photos](https://support.bumble.com/hc/en-us/articles/32422047589277-Providing-feedback-on-your-photos) (Bumble help center)

No video of this session had been published at the time of writing. This write-up is based on the author's room recording, all eight of the author's slide photos, and the speaker's own article. The recording ends with Subudhi's thanks and contains no Q&A. A user-count figure Subudhi gave for Bumble is omitted because the transcript's version of it cannot be right. The [precise notes](precise/10-14-1220-ab-testing-ai-products-is-weird-what-cha.md) (Chinese) keep timestamps and the record of where the talk and the article differ.
