**English** | [中文](zh/10-13-0900-vault-making-unstructured-data-work-for.md)

# Bridgewater's Vault: Agents Are Only as Good as the Data and Connectors Behind Them

*At AI Engineer NYC 2026, Bridgewater's Sam Green and Aaron Linsky showed how the firm built the Vault, a repository of more than 100 million documents tuned for both human investors and agents, and argued that the real payoff comes when agents stop waiting for questions and start pointing investors to what matters.*

**Featured Keynote (Main Stage) · Tuesday, October 13, 2026 · 9:00–9:18am · Simons Room (Metropolitan East)**
**Speakers:** Sam Green, Team Lead, Applied AI (AIA Labs), Bridgewater Associates; Aaron Linsky, Head of Product Engineering, Bridgewater Associates

---

The day's first Main Stage keynote came from Bridgewater Associates. Sam Green introduced the Vault as "the data platform that we've built at Bridgewater to turn unstructured data into a first-class asset in our investment process," and set out three lessons from building it.

![The Vault turns unstructured data into a first-class asset](../slides/10-13-0900-vault-making-unstructured-data-work-for/02.jpg)
*The three lessons, stated up front.*

First, humans and agents need very different things from a data platform, "and so you have to build for those different needs separately." Second, the greatest impact comes when a single tool meets both sets of needs at once. Third, "the real prize" is making agents "a proactive participant in your investment process" rather than "a reactive tool for your humans to use."

## 1. Fifty years of structured data

Green started with Bridgewater's history. For 50 years, the firm has pursued "the world's best understanding of how markets and economies work," turned into "a systematic encoding that we can use to stress test our logic over time." What began as "a set of if-then rules on a yellow legal pad" grew into a continuous, compounding process with humans at the center. More recently the firm has been building AIA, "the artificial investment associate, which puts a machine at the center of this continuous process."

![Fifty years of compounding](../slides/10-13-0900-vault-making-unstructured-data-work-for/03.jpg)
*From a yellow legal pad labeled "The 1980 bond system," to a human-centered learning process, to AIA's reasoning engine.*

The slide reduced the philosophy to three steps: track what is going on in the world; find what the firm's encoded understanding has not yet captured; improve and compound.

![From tabular data to unstructured data](../slides/10-13-0900-vault-making-unstructured-data-work-for/04.jpg)
*Before 2023, time-series models on structured data; since 2023, language models on unstructured content.*

For most of that history, the work meant time-series models on structured data, carefully built so it was "safe for use in our research process and aligned to our internal ontology and world model." Language models opened up a much wider world: news, research, transcripts, policy documents, and the internal conversation "that was previously just in the ether," such as memos, meeting transcripts and debate inside the firm.

That data needed the same treatment, Green said: "carefully cleaned, governed, and made ready for use in both point-in-time research questions today and also our systematic study over time." Looking at data vendors and platform providers, "there was nothing that provided us with all of the control that we needed." (The official session description adds that the team's first instinct had been to buy a solution; that part was not covered on stage.) So Bridgewater built the Vault. It now holds more than 100 million documents, with thousands added every day from 135 external sources, plus Bridgewater's internal data. Each document is aligned to the firm's ontology with enrichments such as countries, concepts and markets, "and most importantly, time period discussed in the document."

## 2. The same question, two very different answers

To show why this matters, Green asked an enterprise chat client a "fairly basic representative investment question": "How is AI infrastructure investment impacting the Korean won, and do market participants expect this to continue?" Neither the client nor the model was named; the slide says only "same question, same LLM."

![Standard tool: out of date, under-sourced](../slides/10-13-0900-vault-making-unstructured-data-work-for/05.jpg)
*The open-web run searches 15 websites.*

![The standard tool builds its answer on one broker note](../slides/10-13-0900-vault-making-unstructured-data-work-for/06.jpg)
*The answer leans on a single broker note.*

The agent searched the web and found 15 relevant-looking sources, but its answer drew on only one or two, and at the end it acknowledged that it hadn't found enough to establish a market consensus. Worse, the note it relied on most "was published way back in July of 2026," while the question was asked in early October. "In the age of AI and in investing, … three months is a massive amount of time," Green said.

![The Vault: searching with time as part of the query](../slides/10-13-0900-vault-making-unstructured-data-work-for/07.jpg)
*Connected to the Vault, the same model narrows to October 1–8, runs three queries and cites seven sources.*

Connected to the Vault, the same query behaved differently. The connection prompted the agent to look for a specific time window, October 1 to 8. Three targeted queries returned 52 candidate documents, and the final answer drew on seven trusted sources instead of one or two. "It takes great data and the right connectors to make agents impactful," Green said, and handed over to Linsky.

## 3. Search for two kinds of reader

"Search is hard," Linsky began, "but it gets even harder if we're designing for two different consumers."

![Search is hard](../slides/10-13-0900-vault-making-unstructured-data-work-for/08.jpg)
*"And harder when designing for two very different consumers."*

![Two very different consumers](../slides/10-13-0900-vault-making-unstructured-data-work-for/09.jpg)
*Humans read about 300 words a minute; agents, 30,000-plus.*

| | Humans | Agents |
| --- | --- | --- |
| Sensitive to | Precision | Recall |
| Need | High signal-to-noise ratio | Performant data access |
| Scale | Small | Massive |
| Consumption | Slow | Very quick |
| Reading speed | ~300 words per minute | 30,000+ words per minute |

Humans need precision because they can only read so much: "Even a fast reader manages about 300 words a minute, and personally, I'm much slower." Agents are "far less sensitive to precision. If the data is there, they'll find it," and they read roughly a hundred times faster.

![Five dimensions](../slides/10-13-0900-vault-making-unstructured-data-work-for/10.jpg)
*Recall, precision, filtering, entitlements and scale.*

Linsky named five dimensions that matter (recall, precision, filtering, entitlements and scale) and went deep on three: precision for people, scale for agents, and filtering for both.

## 4. Precision: benchmark the complaint

Once the Vault was full, "a heroic effort by our data and legal teams," recall stopped being the pressing issue. The number one complaint was that "Vault search is bad." "As a product owner, it's painful to hear that, but my response was let's benchmark it," Linsky said.

![#1 Complaint: "Vault search is bad"](../slides/10-13-0900-vault-making-unstructured-data-work-for/11.jpg)
*About 200 hand-labeled examples, and the five improvements they made measurable.*

The team collected questions and found the best documents by hand, including ones that looked helpful but weren't. About 200 examples took less than a week: "It was not fun or very glamorous, but it was very helpful." The key metric was precision at 10, the share of the top 10 results that are the right documents. It started around 60%: "the people were right, it was bad."

With a benchmark in place, the team could measure each improvement. The slide listed five: fixing document ingestion and parsing issues, tuning Reciprocal Rank Fusion weights, adding a reranker, query expansion, and agentic search. By the end, Linsky said, they were "up in the 90-95%." (On stage Linsky said "recall at 10 or precision at 10"; the metric defined earlier was precision at 10, and the video description says "over 90%." The talk did not break down how much each change contributed.)

## 5. Filtering: between pre- and post-filtering

![We want the most relevant documents](../slides/10-13-0900-vault-making-unstructured-data-work-for/12.jpg)
*Typical filters: an as-of date, a source such as the U.S. Federal Reserve, and currencies.*

Production traces showed that almost every query carried a condition: as of a certain date, from a certain source, about a specific asset. "All of that requires advanced filtering, and that's where we ran into our first major performance issue."

![Postfiltering v Prefiltering](../slides/10-13-0900-vault-making-unstructured-data-work-for/13.jpg)
*Post-filtering: search the whole index, then filter the top results by metadata.*

Linsky described the two classic ways to filter a vector database. Post-filtering uses the vector index first, then filters the top results by metadata; it is fast, but "recall could be pretty low because the globally relevant results might not match your filters." Pre-filtering filters first and then computes distances for every match; recall is very good, but it can be very slow. Bridgewater needed something in between and switched to turbopuffer, whose "native filtering" "looks only in the nearest clusters that contain matches and then scores just those." The result is "fast like post filtering, but with close to the same recall as pre-filtering." turbopuffer's own write-up of the technique describes the same design: attribute indexes tied to the clusters of the vector index, so the planner visits only the nearest clusters that hold matching documents.

## 6. Scale: agents query by the second

![Need to serve at Agent Scale](../slides/10-13-0900-vault-making-unstructured-data-work-for/15.jpg)
*100 queries an hour for people; 100-plus a second for agents.*

Human scale is "pretty pedestrian," Linsky said: about 100 queries an hour from 300 users, who take minutes to read what comes back. Agents are a few orders of magnitude beyond that, "easily 100 plus queries a second, and agent swarms can hit thousands easily," and they read documents in seconds. turbopuffer "handled that easily, too."

Linsky skipped entitlements, promising to cover the topic later that day. (The schedule lists a second session with Linsky at 11:55am, "Can Your AI Analyst Keep a Secret?", co-presented with turbopuffer on enforcing permissions at the retrieval layer.)

## 7. Same habits, new home

![Vault covered all five dimensions](../slides/10-13-0900-vault-making-unstructured-data-work-for/17.jpg)
*"It unlocked new workflows. It didn't change how people work."*

The Vault now covered all five dimensions and unlocked new workflows, "but it didn't change how our researchers worked. They still weren't coming to Vault every morning." That was Linsky's second lesson: "no matter how amazing the tools are, we can't just drop them on our investors' desks and expect them to be used. We need to migrate their old habits."

![Same habits, new home → new habits](../slides/10-13-0900-vault-making-unstructured-data-work-for/19.jpg)
*Each email habit got a Vault equivalent, then a new capability on top.*

Many researchers lived in email. Research arrived straight from brokers, they subscribed to lists, and rules filtered everything into folders. "It worked. It was familiar. And they knew that they weren't missing anything." The Vault rebuilt each habit:

| In email | In the Vault | New habit it opened up |
| --- | --- | --- |
| Research received by email | Searchable in the Vault | Ask questions across all of it |
| Subscribed lists | Saved searches | Dynamic feeds by topic |
| Folders | Saved collections | Hand feeds and collections to agents |

"It's the same habits, just a new home," Linsky said. Once those were in place, users could ask questions across all the data, follow topic feeds and hand collections to their agents, "and that's when we clearly had product market fit."

![Wherever the work happens](../slides/10-13-0900-vault-making-unstructured-data-work-for/20.jpg)
*Access points for people and for agents.*

Then users wanted the Vault everywhere. It went into chat, a phone app, Slack and the IDE for people, and to agents through an API and Python client, an MCP server, and a command-line tool with a skill for coding agents, "because agents love CLIs."

## 8. What the Vault powers

![What agent access makes possible](../slides/10-13-0900-vault-making-unstructured-data-work-for/21.jpg)
*Human accelerants, agent sandboxes and systematic decision-making, all drawing on the Vault.*

| Group | System | What the slide says |
| --- | --- | --- |
| Human accelerants | Pocket Analyst | "Our first AI analyst. Hours of research in minutes, using the Vault." |
| | Researcher | "The next generation of our Pocket Analyst capable of performing long time horizon financial research projects around our entire research circle." |
| Agent sandboxes | Custom RL models | "Models we train with reinforcement learning to make better use of the Vault." |
| Systematic decision making | Forecaster | "AI-generated macro forecasts." |
| | Investment signals | "Systematic strategies that turn documents into structured signals, sourcing every document through the Vault." |

The Vault helps the firm "move around that research circle faster," Linsky said. The Pocket Analyst, Bridgewater's first AI analyst, "can do hours of research in just a few minutes for a few dollars," relying on the Vault. The Researcher stretches the same idea "across days or weeks." Custom-trained models reason over Vault data alongside foundation models to feed systematic decisions: the Forecaster produces "AI-generated macro-relevant predictions," and investment signals turn "unstructured data into structured, tradable strategies." (The slide specifies reinforcement learning for the custom models; Linsky said only that they train custom models.)

## 9. From reactive to proactive: the Investor Workbench

Green returned for the third lesson. The real prize is to turn agents from a resource that answers a human's question into "proactive participants helping to steer your high-value professionals towards the most important areas for their attention."

![Building the Investor Workbench](../slides/10-13-0900-vault-making-unstructured-data-work-for/22.jpg)
*Reactive answers when asked; proactive steers attention. The interface asks "What am I missing?"*

Bridgewater's answer is the investor workbench, where an "agent is always on tracking what's important to that investor and presenting them with the areas that are most in need of their attention." A topic is proposed with a few pieces of information, and the investor decides whether to dig deeper or ignore it.

![Summary and evidence, ready for action](../slides/10-13-0900-vault-making-unstructured-data-work-for/23.jpg)
*A topic card on "AI impact on currencies," with an agent summary and an activity feed.*

![Internal and external evidence highlighted](../slides/10-13-0900-vault-making-unstructured-data-work-for/24.jpg)
*The same card, with the evidence column highlighted.*

The example card returned to the question from the start of the talk, AI's impact on currencies. It carries an agentic summary for a quick scan and "the exact evidence that the agent was drawing on, so they can assess for themselves what was leading to this recommendation," with a direct link into the research tools Linsky described.

![The Vault captures the human + agent collaboration](../slides/10-13-0900-vault-making-unstructured-data-work-for/25.jpg)
*An agentic analysis titled "Understanding AI's Impact on FX Markets."*

When an investor digs deeper alongside the agent, "the results of their collaboration are automatically captured back in the vault."

## 10. Compounding

![The Vault enables compounding](../slides/10-13-0900-vault-making-unstructured-data-work-for/26.jpg)
*Gather the right information; contribute synthesis back to the corpus.*

"The vault enables compounding," Green said, because it puts the right internal and external information at the fingertips of investors and agents, "and it automatically captures the synthesis that they generate and puts it back in the corpus for the next turn." What it takes to make an agent work, Green concluded, is "the right context and the right understanding of what's going on in your world." Green closed with a question for the audience: "Does your AI know what's going on in the world? And does it remember what your firm has learned about it? Because we are teaching ours too."

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=a84ca3a8-cc2f-4b50-a635-fc380f6f04f6) · [Video](https://www.youtube.com/watch?v=CLve4Nx_RwI) · [Livestream (Day 1, from 7:24)](https://www.youtube.com/watch?v=akEqiXFo3GM&t=444s) · [Native filtering for high-recall vector search](https://turbopuffer.com/blog/native-filtering) (turbopuffer) · [How Bridgewater's AIA Labs Built PAT](https://www.bridgewater.com/aia-labs/how-bridgewaters-aia-labs-built-pat-the-ai-pocket-analyst-tool) · [Can Your AI Analyst Keep a Secret?](https://ai.engineer/nyc/2026/schedule?session=88f71c9f-f0a9-4f42-96c0-0e1492bad1c2) (Linsky's later session)

This write-up is based on the official video captions, the Day 1 livestream, slide photos and the sources above. The talk ended without Q&A. The [precise notes](precise/10-13-0900-vault-making-unstructured-data-work-for.md) (Chinese) keep timestamps and the full record of where the talk, slides and descriptions disagree.
