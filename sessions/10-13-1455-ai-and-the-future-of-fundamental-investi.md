# AI Won't Pick the Stocks; It Will Tell Analysts Where to Look

*At AI Engineer NYC 2026, former hedge fund CIO Jack Woodruff argued that AI will raise the baseline every fund works from, not replace the search for a divergent view, and that no number should be trusted unless it can be audited.*

**Stage Talk (fireside chat) · Tuesday, October 13, 2026 · 2:55–3:13pm · Main Stage, Simons Room (Metropolitan East)**
**Speakers:** Jack Woodruff, Senior Advisor, Kepler (founder and former CIO of Candlestick Capital Management); Vinoo Ganesh, CEO, Kepler (moderator)

---

The session was not a talk but a conversation with no slides. Vinoo Ganesh, CEO of Kepler, asked the questions; Jack Woodruff answered them. Woodruff ran a team at Surveyor Capital, Citadel's equity unit, from 2010 to 2019, then founded Candlestick Capital Management in 2019 and led it through 2025. Woodruff started out at Ernst & Young and holds an inactive California CPA license. Ganesh spent seven years at Palantir, was CTO of the geospatial data startup Veraset, and headed business engineering at Citadel before founding Kepler.

Ganesh explained that the two had talked before and wanted to treat the session "really as a fireside chat." The official description listed the questions it would circle: what AI changes in fundamental investing and what it doesn't, whether the biggest funds will win everything, where alpha comes from if everyone has the same models, and "when does a number have to be right, not just plausible?"

The conversation ran through nine questions, and this write-up follows them in order.

## 1. Stock picking stays; the work around it changes

Ganesh opened by asking what fundamental investing looks like five years from now. Woodruff split the answer in two. "Long-term stock picking, I don't believe is really going to change much at all," Woodruff said. "There's always been a mosaic and there's always been alpha creation by finding a divergent view." What will change is the material underneath: "how people synthesize all the information and where table stakes go to in terms of what everybody has … is going to change completely."

The traditional roles of portfolio manager and analyst, Woodruff said, "are going to be completely thrown up on their heads." But it was "too early to say" what they will look like, because the tools are so new, especially to the people using them.

## 2. Humans are still the bottleneck

Ganesh noted that Woodruff's team at Citadel had 18 people covering 400 stocks, and mentioned a team of 30 at Candlestick (the recording is unclear on how that figure was described). Could one great analyst cover all 400 by 2030? (Woodruff's published bio describes leading an 18-person team that "invested in more than 400 global equities" over 2010–2019, a cumulative count rather than necessarily a simultaneous one.)

"The biggest issue is humans are still the bottleneck," Woodruff said. "At the end of the day, you have to get the information into your head." In Woodruff's view, "investment management is a time management exercise." An analyst who spent all their time on one stock would know it better than anyone in the world; covering two halves the time for each, and returns on time diminish as the list grows.

What AI might do is raise the table stakes on general knowledge. Preparation that "used to take me six to eight hours," Woodruff said, an AI program might now help prepare "in five minutes." That frees time, but not for rest. "It's a very competitive industry," Woodruff said, so the freed hours go to more proprietary research, channel checks and industry background work, all in search of divergent views. "If I do the exact same thing I was doing before and … I can get my work done in two hours, someone else is going to be still working 16 hours a day trying to find that extra thing that's going to make you money." What funds paid analysts to do a year or two ago, Woodruff added, "is not what we're going to be paying an analyst to be doing, you know, in a year."

## 3. Analysts will still read the filings

Asked what a junior analyst does today that will look absurd in five years, Woodruff rejected the common answer that analysts will stop reading 10-Ks and earnings call transcripts. "I actually don't believe that to be true." An analyst truly close to a name still has to process that information; summaries can cut the time, "but you still need to get that information into your head."

The work that can go, Woodruff said, is the large-scale data processing many analysts used to do: that "definitely … can be outsourced." The bigger change is direction: "What I believe the AI is going to do … is going to be pushing you in the right direction to work on the right things. Like I said, this is time management. It's all about how do I know to work on the right things." The winners will be the people who use the tools to spend their time finding a divergent view.

## 4. Saved time raises the bar for everyone

Analysts spend a large part of the week "finding, cleaning, and checking data," Ganesh said. When that goes away, will they get smarter or just busier?

Woodruff expected the time to be spent on more of the same hunt: more data, more proprietary research calls, more industry builds. "It's just going to raise the bar on the amount of work that everybody's doing." Once everyone does the same things, they become table stakes, and the competition moves to the next layer of information.

Woodruff described how alpha is found: "The investment mosaic is all about finding three or four sort of data points that all sort of are lining up, that aren't necessarily coming from the same place. And that's generally, I think, the widely accepted view of how alpha is created." No single data point is the edge.

## 5. "No, no, no. Absolutely not."

Ganesh pointed to Woodruff's start as a CPA: "Would you ever sign off on a number or model produced without seeing where it came from?"

"No, no, no. Absolutely not," Woodruff said, and called "our ability to audit every single number" the most exciting thing happening at Kepler. When humans entered data, mistakes were "very rare," Woodruff said, "but still mistakes were made from time to time and … millions of dollars could be lost off of a single sort of like data entry mistake." There is "no way we're gonna let AI tell us exactly what to do, run trading algorithms … if you can't trust the information," because then it "could run amok."

"The accuracy of the numbers and your ability to audit it is absolutely the most important part of this entire process," Woodruff said. Many people Woodruff knows use AI tools to help build models "and then they still have to go through and audit the numbers," which raises the question: "are we really saving that much time by automating this?" Woodruff's answer: "I think it will get better."

In a pre-conference interview with The Daily Context on DEV, Ganesh described Kepler's approach: facts are extracted and given identifiers during preprocessing, the model can reach them only through deterministic lookup tools, and "if the model can't resolve its output against the lookup, it rejects it." Every transformation is recorded in a provenance ledger.

## 6. Data security is a CTO question; today is "1.0"

Ganesh told the audience that for many AI companies in the room, Woodruff is the buyer: the person who has to be convinced before a hedge fund buys software. Which part of the investment process would Woodruff refuse to let them see?

"I'm the wrong guy to ask this question," Woodruff said. "This is a total CTO question," and Woodruff has a CTO who handles it. Woodruff would "be very nervous about any sort of internal data. You don't want anything to get out," and joked about not knowing the terminology and sounding "like an idiot in front of you engineers." (In the DEV interview, Ganesh said it was strange that "a lot of people are very okay sending private information to the labs," and advised that for sensitive data "I would deploy on-prem," running models inside one's own cloud network: "Your own data is your alpha.")

Woodruff then described where Kepler stands. Woodruff and Ganesh have been calling the current stage "1.0": building the automation, getting the tools in place and the data built up. After that, "we can actually like really start doing interesting things," using the tools to tell analysts where to spend their time.

"Investment management isn't about doing earnings previews. It's not about building DCFs," Woodruff said. "It's making money. And so to make money, you need divergent views." The tools help find them, "but they're not the outcome." And any edge is temporary: competitive moats and arbitrage "generally last pretty short," because the industry is full of very smart, motivated, highly paid people, and "when someone else is making a ton of money, other people sort of figure it out, arbitrage gone."

## 7. Where the edge comes from when everyone has the same models

Once every fund has the same models, does the edge come from better data, better questions or better judgment? "I think all of it," Woodruff said. Everyone will end up with the same information, "so it's all going to be about how you prompt it, how fast you're able to sort of … combine man versus machine."

Woodruff described what the old research process was for. Funds did an enormous amount of work, much of which Woodruff said could now be automated, and that work "didn't tell us, hey, go buy this stock. It said, hey, this is something you should go spend a lot of time on." It pointed to a project worth deep study while everyone else looked elsewhere: "you're looking for that sort of needle." "That was the purpose of all the field work," Woodruff said.

"If AI can just do it, then I don't know. I mean, what do we need us for?" Instead, Woodruff expected the tools to show where more research is needed, because "there's an unlimited amount of things one can do." The goal stays the same: "It's about identifying facts before everybody else."

## 8. A fund launched in 2027: fewer preparers, more makers

"If you launch a fund in 2027, what's on your build list versus your buy list?" Ganesh asked.

"I'm not sure," Woodruff said, and answered about people rather than tools. "I still need senior stock pickers. I may not need as many analysts," but the fund might need people with other skills in analyst-type roles: "investigative journalists, people who can do like really deep project work … maybe different sort of data engineers."

The old structure was a pyramid: many analysts presenting to senior stock pickers, who presented to a few portfolio managers, with the CIO on top. Woodruff pictured something "a little bit more vertical": "I want the makers, you know, the people who are really there to make the money. I don't know if I'm going to need as many of the preparers." Then Woodruff hedged: the fund might still want them, to find more proprietary information "to feed the beast."

## 9. Small funds and giant firms: idea velocity decides

Who does AI help more, Ganesh asked: a five-person fund that can act like 50, or a 5,000-person institution?

Woodruff said every fund has a different strategy and mandate. Some trade their book at high frequency many times a year; others "buy and hold for five years." Either way, "everyone's going to be able to do more with what they have."

A five-person team has "slower idea velocity," so it probably runs a more concentrated strategy. Woodruff explained why more coverage matters: more ideas to choose from should mean a better hit rate.

| Names covered | Names in a 20-stock portfolio | Share of coverage that makes the book |
| --- | --- | --- |
| 100 | 20 | 20% |
| 200 | 20 | 10% |
| 400 | 20 | 5% |

The catch: "the research for those 400 has to be just as solid as the research on the 100."

A 5,000-person firm is probably a multi-strategy fund across every asset class, a different situation. But for both, "idea velocity and the number of good ideas you have in your pipeline is paramount." When an idea works, the first thing to do is take it out and put a new one in: "that's how you build alpha and build returns over time."

## 10. The no-analyst hedge fund

The last question: what AI idea is everyone in finance excited about that Woodruff thinks won't work, and what idea is no one talking about that will?

"I wish I knew the thing that nobody's talking about, because then I can make some money," Woodruff said.

The idea everyone is talking about is "this no analyst hedge fund," run entirely by AI agents. Woodruff's view: such a fund "will be able to replicate a fund that existed a year ago; it's just not going to be as good as what today's hedge funds are going to be next year."

"I don't think you're ever going to really be able to take humans out of the loop," Woodruff said. Quantitative funds have run without humans in the loop for decades, so that is nothing new. But if humans could be taken out of fundamental investing entirely, the winners would simply be whoever has the most resources and "the most information. And I just don't think that that's how it's going to go."

"Thank you so much, Jack," Ganesh said, and the session ended.

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=a244839c-239c-4789-b621-32a3568a218e) · [Day 1 livestream, chapter at 5:29:04](https://www.youtube.com/watch?v=akEqiXFo3GM&t=19744s) · [Kepler's Vinoo Ganesh on Building AI You Can Trust](https://dev.to/dailycontext/keplers-vinoo-ganesh-on-building-ai-you-can-trust-3p0f) (The Daily Context, pre-conference interview) · [Hedgeweek on Candlestick's closure](https://www.hedgeweek.com/citadel-veteran-to-close-candlestick-capital/)

The session appears as a chapter of the official Day 1 livestream, which has no captions; the chapter runs only about eight minutes against an 18-minute session, so the stream may carry only part of it. This write-up is based on the author's room recording, a second transcription of a phone recording used to check key numbers and unclear sentences, and the sources above; the DEV interview predates the conference and is quoted only as background. There were no slides and no audience questions: the recording ends after the moderator's last question. The [precise notes](precise/10-13-1455-ai-and-the-future-of-fundamental-investi.md) (Chinese) keep timestamps and the list of unclear passages.
