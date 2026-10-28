# Measuring Slop Means Measuring Voice, Not Banned Words

*At AI Engineer NYC 2026, Towards AI's Louis-François Bouchard launched ToneBench, a benchmark that scores whether a model can write a video script in Bouchard's own voice, and shared what it cost to build and what it taught about writing with AI.*

**Breakout (Evaluations track) · Tuesday, October 13, 2026 · 10:40–10:58am · Turing Room (Riverside Ballroom)**
**Speaker:** Louis-François Bouchard, CTO and Co-Founder, Towards AI (runs the What's AI YouTube channel; co-author of *Building LLMs for Production*)

---

Louis-François Bouchard opened on a wall of words that readers have learned to distrust: delve, tapestry, pivotal, paradigm-shifting, harness, "the real magic happens," "In today's fast-paced world," "It's not just X, it's Y." Over them sat a quote the slide attributed to Andrej Karpathy in January 2026: "I am bracing for 2026 as the year of the slopacolypse across all of github, substack, arxiv, X/instagram, and generally all digital media." Below it, in large orange type: "Tired of the slopacolypse? Let's stop slop."

![Let's stop slop](../slides/10-13-1040-can-any-model-write-like-me-building-a-b/01.jpg)
*The opening slide: the "slopacolypse" quote over a cloud of AI-flavored phrases.*

"I want to try to help fixing what Karpathy termed as the slop apocalypse that he thought would start in 2026, and I think we've seen that on social media," Bouchard said. "To try to fix this and to stop slop, we first need to define it."

The talk followed a five-part plan: why another benchmark, how it works, what it cost, what was learned, and "your turn." Bouchard announced that the benchmark, ToneBench, was being released that day.

## 1. What slop is

![What is slop?](../slides/10-13-1040-can-any-model-write-like-me-building-a-b/02.jpg)
*Six outside definitions of slop, with "It s\*cks to read it" across the middle.*

The second slide collected definitions from people and institutions that have tried to pin the word down, credited on the slide to Simon Willison ("who made the word stick," May 2024), Merriam-Webster (Word of the Year 2025), Oxford University Press (Word of the Year shortlist, 2024), swyx (Latent Space, January 2026) and Samuel Paech and colleagues (the Antislop paper, October 2025). Merriam-Webster's, which did name slop its 2025 word of the year, reads: "digital content of low quality that is produced usually in quantity by means of artificial intelligence." Bouchard's own summary was printed across the middle of the slide. "What I think we can all agree is that it really sucks to read something that feels generated," Bouchard said.

![Let's delve deep](../slides/10-13-1040-can-any-model-write-like-me-building-a-b/03.jpg)
*"Let's delve deep into the intricacies of Agentic AI most companies miss.." signed "LLM," with 2023, 2024 and 2025 struck through.*

The joke slide made the point that the style has not aged out. "Even as models get better with Fable, GPT-6.1 and all the better models, slop doesn't seem to go away. It just keeps on scaling," Bouchard said. "We need something different than just training the models more, it seems."

![What slop is made of](../slides/10-13-1040-can-any-model-write-like-me-building-a-b/06.jpg)
*Bouchard's six ingredients of slop, and the goal of the project.*

Bouchard then broke slop into six parts:

| # | Ingredient | Examples on the slide | What it means |
| --- | --- | --- | --- |
| 1 | Overused words | "delve," "tapestry," "seamlessly" | they sound generated: some show up 1,000× more often than in human writing |
| 2 | Too general | "Most teams…," "Most projects…" | broad claims, nothing specific |
| 3 | Says nothing | "In today's fast-paced world…," "unlock the real power of AI" | nice words, very little information |
| 4 | Repeats itself | "In other words…," "To recap…" | the same point, again and again |
| 5 | Hallucinations | "A 2023 Stanford study found…" | facts that aren't true |
| 6 | Wrong tone | "Buckle up!," "We're thrilled to announce…" | it sounds like nobody, or like a press release |

Bouchard admitted to the second one: "I'm myself guilty of using that on LinkedIn as well, which I guess it's the LinkedIn meta." The worst, in Bouchard's view, was the last: slop "usually has the wrong tone or at least it has no tone and like everyone's tone at the same time. So it just doesn't really talk like you."

That framed the goal printed at the bottom of the slide: "measure sloppy writing, not just slop words." Bouchard said: "I don't want to rebuild EQ-Bench slop score. It's really about measuring how sloppy is a writing."

## 2. Why build another benchmark

![So we built ToneBench](../slides/10-13-1040-can-any-model-write-like-me-building-a-b/07.jpg)
*"So we built ToneBench. A benchmark that measures creative writing."*

![The plan](../slides/10-13-1040-can-any-model-write-like-me-building-a-b/08.jpg)
*The plan: why another benchmark, how it works, the cost, what we learned, your turn.*

Bouchard gave three reasons the field needs another benchmark. The first was the state of social media: "Just looking at LinkedIn and X replies, my replies on X, I think we do. People are spamming more and more generated content. So, at least have them be decently good."

The second was personal need. Towards AI is a small team with an education side that produces a lot of AI engineering content, and it uses AI to help. "It's been extremely annoying to review this slop constantly and try to fix it," Bouchard said. "I published hundreds of videos. We published thousands of articles. We released a book. We are working on a second one."

![We write for a living, so I care](../slides/10-13-1040-can-any-model-write-like-me-building-a-b/10.jpg)
*Towards AI's publishing footprint, with the second book on the right.*

The slide put numbers on that footprint: since 2019, more than 500,000 people taught and more than 100,000 members in the company's Discord; 74.2K YouTube subscribers and 734 videos since 2020; 150K followers and thousands of writers on the Towards AI publication; more than 10,000 copies of the first book, *Building LLMs for Production*, rated 4.4 stars; the Towards AI Academy; and a second book, *AI Engineering for Production*, "coming soon."

The third reason was demand: Bouchard had shared the project on X while building it, and the response suggested "we did need some sort of more creative writing benchmark like this one."

![Building evals is also our job](../slides/10-13-1040-can-any-model-write-like-me-building-a-b/11.jpg)
*Bouchard's evaluation background and the team behind ToneBench.*

Finally, Bouchard argued the team was suited to the work. The timeline on the slide ran from Head of AI at a startup (2020–2022, alongside a master's in computer vision and founding Towards AI) to PhD research at Mila (2022–2024, "where running evaluations was most of the work"), to Towards AI building AI solutions for clients, including custom eval pipelines (2022 to today), to ToneBench in June 2026. The company has 15 AI engineers who build and deploy AI for companies and four specialized technical writers and editors. "I've been evaluating AI models since my master's and then PhD. And now at Towards AI, we do build evaluation pipelines for our clients," Bouchard said.

## 3. The writer: can a model write like me?

"What we built is super subjective," Bouchard said. "It's a way to measure if a model can write like me. And how I define that is just that if I can read it out loud like right now without cringing because of the words that I wouldn't use myself."

![First, the writer](../slides/10-13-1040-can-any-model-write-like-me-building-a-b/12.jpg)
*A brief and research go to one model, which writes a full video script in Bouchard's voice.*

The task is simple in outline. A model receives a brief and research, plus Bouchard's voice and storytelling guides, and must produce a full video script of 1,000 to 5,000 words. "Obviously that doesn't happen. It really sucks usually, but that's the whole goal," Bouchard said.

![What the writer model sees](../slides/10-13-1040-can-any-model-write-like-me-building-a-b/13.jpg)
*The writer's input, section by section, and its output.*

The real prompt is much longer than a brief:

| Input section | Purpose | Words |
| --- | --- | --- |
| Voice guide | how I sound | 2,626 |
| Storytelling guide | hooks, pacing, cues | 3,243 |
| The contract | hard requirements | 1,045 |
| The brief | what to write | 693 |
| The research | the facts | 673 |

The output is one full script of about 2,000 words with `[SHOW:]` markers for visual cues. The guides include "a lot of details on how I talk, how I write, tons of examples of my previous content videos," as well as general material on storytelling, active sentences "and just how to write well," Bouchard said.

![Ten of my own scripts](../slides/10-13-1040-can-any-model-write-like-me-building-a-b/14.jpg)
*The ten benchmark tasks, chosen to vary in kind, length, research and voice.*

There are ten tasks, each based on one of Bouchard's own scripts and each with its own rubric:

| Dimension | Breakdown |
| --- | --- |
| Kind of video | News & opinion 3 · Technical explainer 3 · Career guide 2 · Announcement 2 |
| Length | Under 10 min 3 · 10 to 15 min 4 · 15 to 28 min 3 |
| Research given | Short 2 · Medium 4 · Long (up to 7,500 words) 4 |
| Voice | Personal story 4 · Analysis and teaching 6 |
| Published? | Some on YouTube, some never published (counts hidden in the photo) |

"Some of them are published, some are not. We don't say which ones are the benchmarks so that we check for contamination," Bouchard said. According to the ToneBench methodology page, the scripts run from about 885 to 4,260 spoken words, half from published videos and half unpublished, and were reviewed by six Towards AI members before a final review by the content lead.

## 4. The whole system: five drafts, four judges

![The whole system](../slides/10-13-1040-can-any-model-write-like-me-building-a-b/15.jpg)
*Each model writes five drafts per script; four LLM judges score them; scores roll up into a leaderboard.*

The pipeline on the slide runs once per model and thinking level ("× 127 models and thinking levels (for now)") and per script (× 10). One model, for example Sonnet 5.5 at max effort, generates five drafts. Four LLM judges score each draft: Claude Opus 5.5, GLM-5.3 Flash, GPT-6.1 and Jev (TypeSafe). The result is one score per script from nine metrics, then a model score across ten scripts and nine metrics, then a leaderboard ranked by Elo from head-to-head comparisons.

Jev was new. "I recently added Jev as a test. Not sure if I will keep that, but I added it just to see if it worked well," Bouchard said. According to the methodology page, Jev 1.13 (TypeSafe) is a "typed judge" that answers a fixed multiple-choice question for each metric without writing reasons, producing probabilities over five quality levels that are mapped to 0–100. The page also says the four judges are weighted equally, and that the Elo uses a 1500 starting rating, K=16, a 400-point logistic scale, 25 orderings averaged and 200 bootstrap runs for 95% intervals, cross-checked with Bradley-Terry; the Writing Score is absolute (0–100) and the Elo is relative.

![Why 5 drafts?](../slides/10-13-1040-can-any-model-write-like-me-building-a-b/16.jpg)
*Five drafts of the same script, scored, for three models: the spread grows as quality drops.*

Why five drafts? "It's to check for intra-model variability, which I highly suggest if you are building your own evaluations, because especially on a creative task, sometimes as we see here with Grok 4.7, some of the generations are outliers," Bouchard said. The slide (whose left edge was cut off in the photo) compared three models:

| Model | Spread | Rank on its best drafts | Rank on all five | Rank on its worst drafts |
| --- | --- | --- | --- | --- |
| Claude Opus 5.5 (max) | ±0.9 | #1 | #1 | #3 |
| Grok 4.7 (high), two clusters | ±3.4 | #5 | #29 | #67 |
| Nemotron 3 Ultra (reasoning), "all over the place" | ±5.0 | #111 | #137 | #148 |

(The first two names are partly cut off on the slide and were completed from what Bouchard said; the third is a reading of the visible "…tron 3 Ultra.") On one draft out of five, Bouchard said, Grok 4.7 was "just as good as Opus 5.5." Generally, "the worse the model is, the widest the variance." Five drafts "seems to be the good middle ground in terms of cost for us to execute and to measure this variability."

## 5. What gets measured

![What we measure](../slides/10-13-1040-can-any-model-write-like-me-building-a-b/19.jpg)
*Nine weighted metrics, grouped into five aspects.*

"How do you even quantify writing quality? That's been the challenge," Bouchard said. ToneBench uses nine metrics with fixed weights:

| Metric | Weight | Scored by |
| --- | --- | --- |
| Tone & voice | 19% | judges |
| Substance & accuracy | 15% | judges |
| Continuity & emotion | 14% | judges |
| Writing craft | 13% | judges |
| YouTube craft | 12% | judges |
| Hook strength | 10% | judges |
| Length | 8% | code |
| Slop | 5% | code + judge |
| Visual cues | 4% | judges |

Bouchard condensed them into five aspects. First, a specific tone: does it sound like me, meaning can the model adapt to one voice? Second, storytelling, or pure "quality": hook, emotion, rhythm, clarity and "real examples instead of generalities." Third, instructions: the expected length and format, and visual cues in the right places. Fourth, research accuracy: whether the draft uses the research it was given, adds its own knowledge where that helps, and avoids hallucinating. Fifth, slop, measured "using EQ-Bench slop score, but also our own measurements."

## 6. How the judges score

![What every judge sees](../slides/10-13-1040-can-any-model-write-like-me-building-a-b/20.jpg)
*The judge's packet, section by section, and what it is never shown.*

Each judge receives a long packet. Its sections, with word counts from the slide:

| Packet section | Words |
| --- | --- |
| Rules, and what the packet holds | 449 |
| 5 score levels, how to pick a number | 402 |
| Tone & voice | 1,418 |
| Writing craft | 1,421 |
| Substance & accuracy | 1,344 |
| Hook | 872 |
| YouTube best practices | 1,054 |
| Continuity & emotion | 894 |
| Anti-slop & number reframing | 1,165 |
| Visual cues | 855 |
| Evidence first, reply format | 241 |
| The brief | 693 |
| The research (the fact source) | 673 |
| My final script (the top of the scale for voice) | 1,390 |
| The draft (being judged) | ~1,300 |
| Computed stats (counted by code: cues, listed phrases, the opening) | – |

That comes to about 14,000 words per judgment on this task, and up to about 25,000 on the longest. Two things are never shown: which model wrote the draft, and the expected word count, since code scores length. "The judge just analyzes the writing quality. It doesn't focus on length and doesn't know which model is writing," Bouchard said. Bouchard's own finished script is included as the top of the scale for voice. (A further line on the slide, beginning "25,400 times," was hidden in the photo.)

![Example with one metric: tone and voice](../slides/10-13-1040-can-any-model-write-like-me-building-a-b/22.jpg)
*The tone-and-voice rubric: four signals, six score bands, and scored excerpts.*

Tone and voice, the main metric, illustrated how each rubric works. The judge counts four signals, each with a long description: first person (real experience or judgment, at most one per paragraph, spread out); reactions to major claims (how many get Bouchard's take rather than just being reported); owned judgment (what Bouchard would use, avoid, trust or test first); and tone violations (corporate, hyped, theatrical or forced casual). It then picks a band before giving a number:

| Score | Band description |
| --- | --- |
| 90–100 | first person in 3+ parts, 80%+ of claims get a reaction, no tone violations |
| 80–89 | spread out, 65–79% get a reaction, 1–2 isolated slips |
| 70–79 | uneven, 50–64%, or one tone problem recurs |
| 50–69 | shallow, 25–49%: any AI educator could say it |
| 25–49 | rare, under 25%, personality markers dominate |
| 0–24 | first person and judgment effects absent |

The rubric also carries scored excerpts. Bouchard's own line scored 95: "I use coding agents daily… I love this stuff. But I would not drop a random cheap proxy between my agent and any repo I care about." A model draft scored 66: "I've watched agents rewrite the same helper function four different ways across 18 iterations." Another draft, mentioning a podcast with Addy Osmani, scored 40.

"The judge will categorize between six categories first, and after having found a category, it will give a rating," Bouchard said. The team compared this against alternatives, "just binary that we usually suggest to use, or directly categories, or directly percentage. We found that this actually works really well." The slide's footnote said the same: it "worked best vs. direct % or binary or only categories."

![What the judges send back](../slides/10-13-1040-can-any-model-write-like-me-building-a-b/24.jpg)
*One judge's scores for three models on the same brief, and its reply for one metric.*

For each metric, a judge returns a quote it highlighted, a score and one sentence of reasoning. The slide showed one judge's scores for three models on the same brief:

| Metric | Weight | Opus 5.5 | GPT-6.1 Sol | Haiku 5.5 |
| --- | --- | --- | --- | --- |
| Tone & voice | 19% | 86 | 76 | 73 |
| Substance & accuracy | 15% | 88 | 85 | 85 |
| Continuity & emotion | 14% | 88 | 77 | 75 |
| Writing craft | 13% | 85 | 73 | 71 |
| YouTube craft | 12% | 86 | 84 | 80 |
| Hook | 10% | 91 | 86 | 76 |
| Length (code) | 8% | 100 | 97 | 98 |
| Slop (code + judge) | 5% | 89 | 87 | 85 |
| Visual cues | 4% | 86 | 84 | 76 |
| **Draft score** | | **88.2** | **81.7** | **78.7** |

The draft score is each metric times its weight, added up; the five drafts' scores are then averaged. (Recomputing from the table gives 88.2 and 78.7 for Opus and Haiku, and 81.6 for GPT-6.1 Sol, a tenth below the slide, probably from rounding.) For the Opus draft's tone and voice, the judge highlighted "I'd much rather read a wrong assumption stated clearly than a right one I have to reverse-engineer from your code," gave it 86, and explained: "First person carries real hiring judgments from open to close (what he'd rather read, what answer he accepts, what he's hiring for)…"

All of it is automated, Bouchard said, because the volume is too large to read by hand: "with 120 models, we generate five drafts each for 10 tasks. It quickly goes to the thousands of scripts."

## 7. Every judge has a favorite lab

![Every AI judge has a favorite lab](../slides/10-13-1040-can-any-model-write-like-me-building-a-b/25.jpg)
*How many points each judge gave or took from each lab's drafts, compared with the other judges.*

"The problem with LLM judges is that they have preferences," Bouchard said. "Some models prefer themselves, and particularly OpenAI." A radar chart across seven labs (Anthropic, OpenAI, Z.ai, DeepSeek, xAI, Alibaba, Google) showed each judge's largest bias:

| Judge | Most favored | Least favored |
| --- | --- | --- |
| Claude Opus 5.5 | Anthropic +2.3 | OpenAI −2.8 |
| GPT-6.1 Sol | OpenAI +3.9 | Anthropic −2.1 |
| GLM-5.3 Flash | Alibaba +1.4 | OpenAI −1.4 |
| Jev ("no model in the race") | Alibaba +3.7 | Google −2.9 |

"What I found quite funny when studying this is that it seems like Anthropic and OpenAI are really in a war here, where they seem to have past training on hating each other," Bouchard said. The practical advice: if you use an LLM judge, such as GPT, to pick the best model for your task, "be careful and use different model family," because "OpenAI really has a strong preference on its own family of models, not just one model, but all OpenAI models." Bouchard's accompanying article puts it as: "No judge is neutral. Use a panel, and never let your writer's lab be the only scorer." (The article's figures differ from the slide's, for example +2.75 rather than +3.9 for the GPT judge on OpenAI, and it describes Jev as slightly favoring Gemini models, the opposite of the slide's −2.9 for Google.)

People on X had suggested using humans for a creative task, which Bouchard called "ideal, but quite impractical." The team tried a scalable version: "right now we compared manually, me and three of my teammates, 1,000 A/B pairs of the generated scripts, not the full scripts, but a good chunk that is comparable between two scripts." Bouchard hoped to open this up to the community later.

## 8. What it cost

![The cost](../slides/10-13-1040-can-any-model-write-like-me-building-a-b/26.jpg)
*Hours, months and dollars, "and going… (paper coming!)."*

| Item | Figure on the slide |
| --- | --- |
| Kickoff effort | ~500 hours: 4 people, one month (scripts, rubric, what to measure, A/B comparisons) |
| Iteration | 4 months, on 3 Claude 20x and 1 Codex 20x subscriptions, plus thousands of dollars in OpenRouter and other APIs |
| One rubric change | $2,350 to re-judge everything |
| Full refresh | $3,750 to regenerate and re-judge the whole board |

The 500 hours went to building the ten scripts, defining the rubrics, finding examples and trying "to quantify quality, which is very difficult," Bouchard said. Then came four months of iterating "with four different 20X subscriptions" on the metrics and their weights. "If I just change one thing in the rubrics or how the judge rates generations, it will cost us more than $2,000. And if I just change one line in an input prompt for a model when it generates, it costs us more than $4,000." (The slide gives $3,750 for the full regeneration. Bouchard's article puts the kickoff at about 400 hours rather than 500.) The slide ended: "Experimenting is expensive. Want to help? Reach out!"

## 9. What it taught about writing with AI

![Tips for writing with AI](../slides/10-13-1040-can-any-model-write-like-me-building-a-b/30.jpg)
*Which model to pick, and six techniques, from the ToneBench results.*

The main lesson, Bouchard said, came from four months of looking at results: "You don't want to pick the model that will generate a one-shot amazing post. You just want to find a model that will be the least annoying to you to get to where you want to be." The slide put it as "Pick the model that gets you there with the fewest edits, without making you mad. Not the one that writes 'amazing' text." Its current picks:

| Use | Model | Rank | Cost per script |
| --- | --- | --- | --- |
| At scale | GLM-5.3 Flash | #12 | $0.022 |
| One piece | Claude Opus 5.5 xhigh | #1 | $1.003 |

"At scale, GLM 5.3 Flash is extremely good, extremely cheap. It's 50 times cheaper than the best one, which is Opus 5.5 extra high," Bouchard said (the slide's prices work out to about 46 times). Bouchard added that "you don't want to use Max for writing, usually." (The "why five drafts" slide had listed Opus 5.5 at max as #1; the tips slide names xhigh. The sources disagree on the exact ranks and prices, and the leaderboard has since been re-run.)

The slide's six techniques, which Bouchard walked through:

| # | Technique |
| --- | --- |
| 1 | Very clear directions: long, detailed skills and instructions. The more the better. |
| 2 | Examples of what to do, and how |
| 3 | Negative examples of what not to do |
| 4 | Your best past pieces, in full |
| 5 | A review by a separate judge LLM, then an edit based on its feedback. This helps a lot (1–3 reviews max). |
| 6 | (Cheapest best option) Several drafts, especially with cheap open models, plus a smart, high-thinking judge that picks or merges the best |

On the prompt: "It cannot be long enough, and the more the better." Add every example of what to do, and every negative example, "the words you don't want, everything." If you write newsletters, "you can share it in full, and it will just be better."

On review: use a high-thinking judge "just one time, one to three times, otherwise it will loop into slop." One review by another model with the same instructions, giving feedback to the writer, "helps a lot," though "I don't have quantitative metrics on that." (Bouchard's article says that beyond two reviews it hasn't seen much help.)

For companies working through APIs, Bouchard suggested generating around ten drafts with a cheaper model such as GLM-5.3 Flash, then having a smarter high-thinking judge such as Opus "pick and merge the best parts of each of these drafts," which "will still land way, way less expensive than just one generation by Opus 5.5" and, in Bouchard's experience, gives better results.

And last: "You want a human review. You cannot really go around that."

![This measures my voice](../slides/10-13-1040-can-any-model-write-like-me-building-a-b/31.jpg)
*The closing slide.*

Bouchard closed with a caveat: "All of this benchmark measures obviously my voice, so it's super subjective, and it's just one data point. You don't want to fully trust it blindly. Doesn't mean Opus 5.5 is the best model." The final slide read: "This measures my voice. What about yours? One data point, not the truth. A strong signal for writing in a voice." Bouchard encouraged the audience to build their own evaluation pipelines, offered help, and pointed to the site, live as of that moment, with its graphs, comparisons and full methodology.

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=736edd83-bb08-49f1-90ed-722b7ed07eb2) · [Video: Why AI Writing Still Reads Like Slop](https://www.youtube.com/watch?v=_dTF3NpPzGo) · [ToneBench](https://benchmark.towardsai.com/) · [ToneBench methodology](https://benchmark.towardsai.com/methodology.html) · [What 9,050 AI-Written Scripts Taught Me About Slop](https://dev.to/dailycontext/what-9050-ai-written-scripts-taught-me-about-slop-1g5n) (Bouchard's article) · [Merriam-Webster's 2025 word of the year (AP)](https://abc7news.com/post/merriam-websters-2025-word-year-is-slop/18288780/)

The number of models tested differs by source and date: the schedule listed 148, the system slide said 127 "for now," Bouchard said 120 on stage, the article counts 181 configurations and 9,050 scripts, and the methodology page, updated on October 23 after a re-run, lists 136 configurations. The methodology page also now describes the human arena as closed. The video ends with applause and no Q&A. This write-up is based on the official video captions, the author's slide photos and the sources above (the author's room recording was not usable). The [precise notes](precise/10-13-1040-can-any-model-write-like-me-building-a-b.md) (Chinese) keep timestamps and the full record of where the talk, slides and article disagree.
