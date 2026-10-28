**English** | [中文](zh/10-13-1630-a-hedge-fund-as-a-function.md)

# Coatue's Frank Long: Write the Fund as a Function, Then Pick a Runner for Every Line

*At AI Engineer NYC 2026, Coatue's head of AI Frank Long described an investment firm as a function from external and internal context to what to buy, how much and when, and argued that the work is breaking it into checkable pieces and choosing whether code, a statistical model, an LLM or a person runs each one.*

**Featured Keynote · Tuesday, October 13, 2026 · 4:30–4:48pm · Simons Room (Metropolitan East), Main Stage**
**Speaker:** Frank Long, Head of AI, Coatue

![Frank Long on the Main Stage](../slides/10-13-1630-a-hedge-fund-as-a-function/20.jpg)

---

Frank Long introduced Coatue as "a technology investment firm across public and private markets," a business where "you really need to … be able to take high conviction bets on big themes and watch those play out." A career works the same way, Long said, and the bet Long made on joining the firm was "that agents will have an even bigger impact in investing than they did at coding. I know that's a big statement." The talk laid out the framework Coatue uses to pursue that.

## 1. A fund is a function

![def fund(c_external, c_internal)](../slides/10-13-1630-a-hedge-fund-as-a-function/03.jpg)
*The fund as a two-line function. The cover reads "An Investment Fund as a Function."*

"At a high level you can think about a hedge fund as a function that takes in two parameters, context external and context internal, and spits out one tuple: selection, size, timing. What stock, how much, when." The slide wrote it as code, `def fund(c_external, c_internal): return selection, size, timing`, with notes in the margin: "Takes external and internal context," and, in red, "People own the investment judgment."

Long called it a compression problem: how to take all the information in the world and produce one simple tuple. "The product that we provide is essentially a time series of this tuple that constructs a portfolio."

## 2. Everything a human sees in pixels, a model sees via API

![Everything a human can see in pixels, a model can see via API](../slides/10-13-1630-a-hedge-fund-as-a-function/06.jpg)
*What an analyst looks at on screen, mapped to API calls for external and internal context.*

The foundation is a simple philosophy: "every single thing that a human can see in pixels on the screen, an agent in your building should be able to see via API." Long's analogy: "Imagine you brought the smartest people ever, the greatest investors alive, and you brought them into your company, but you gave them a computer without login to 75% of your services. It's almost immaterial how smart the system is if it doesn't have the tools to actually action that."

The slide set a monitor showing a chart and a table beside the calls an agent would make instead, using Samsung Electronics (Korean ticker 005930.KS) as the example:

| Context | Call | Returns |
| --- | --- | --- |
| External | `news.search("Samsung", lang="ko")` | title, source, time |
| External | `prices.history("005930.KS")` | date, close |
| External | `filings.get("005930.KS", "Q3")` | revenue, operating margin, … |
| Internal | `positions.current()` | ticker, weight, cost |
| Internal | `liquidity.requirements()` | cash floor, redemptions |
| Internal | `case_studies.search("Samsung")` | memo, author, date |

External context (news, prices, filings) should be reachable "not via dragging and dropping, copy and pasting, like directly via API." Internal context "is just as important": the firm's own situation, such as current positions, liquidity and risk requirements around certain names, and past case studies or history with a sector, stock or management team.

Long credited the firm for this groundwork: "No credit to me, Coatue has been an amazing leader on this for over a decade. It's both technological and cultural." Alongside the data science team and systems, the culture is "super super rigorous": "For decades, we filed every single note about every single trade, about every single decision we made or didn't make, about what went well and what didn't go well."

(The livestream's captions drop out for about ten seconds here and at four later points in the talk, so a few sentences are missing from the record.) When they resume, Long was dismissing the idea that one could put everything into "a context window and get my magic numbers and go home. … It can't be that easy."

## 3. Break it down until each piece can be checked

![Break it down until each piece can be checked](../slides/10-13-1630-a-hedge-fund-as-a-function/09.jpg)
*The fund function split into gather, analyze and decide.*

The next slide grew the function line by line:

| Line | Note on the slide |
| --- | --- |
| `def fund(c_external, c_internal):` | Takes external and internal context |
| `info = gather(c_external, c_internal)` | Gather information |
| `view = analyze(info)` | Analyze it |
| `selection, size, timing = decide(view)` | Make the decision: a person |
| `return selection, size, timing` | People own the investment judgment |

Gathering means filtering what is relevant. "I pulled some news articles about the NBA that probably isn't material to stocks, but oh well, this is actually related to some local permitting that might actually impact data centers. Like there's a lot of nuance in this gather step." Analysis forms a view of "what that means for the markets, public and private," and the decision turns the view into the three-part tuple. Each step's output is the next step's input, which Long called a natural way for an engineer to think about it. "But even this, it really can't be as simple as writing three functions."

## 4. A case study: Korean news

To show the full complexity, Long zoomed into "a tiny subpiece of a tiny subpiece of just gathering information": interpreting the news. "News from Asia and Korean media outlets has been super important this year because semis [and] AI have been a huge huge theme in the market," Long said, and reading Korean-language news "better than our completely non-Korean staff" looked like a good use case.

![Interpreting news from Korean media outlets](../slides/10-13-1630-a-hedge-fund-as-a-function/12.jpg)
*The gather step for one source, with a runner assigned to each line.*

| Line | Runner | Note on the slide |
| --- | --- | --- |
| `def gather(question, korean_news):` | | One research question, one source |
| `articles = collect(korean_news)` | Code | Originals, with source and time |
| `relevant = filter(articles, question)` | LLM | Against the question, with reasons |
| `notes = translate_extract(relevant)` | LLM | Sourced observations, in English |
| `check(notes, against=articles)` | Code | Every quote matches its source |
| `return update` | Person | Accepts, revises or rejects it |

Even collecting has many degrees of freedom: "This could be an MCP. This could be a CLI. License the data feeds. Clean the data feeds." How to chunk the text for the model, whether to pass images as images or convert them to text: "I almost think about those knobs as like hyperparameters in a traditional machine learning system … something for me to tune and optimize." Filtering removes what doesn't touch the portfolio ("there's probably Korean NBA fans"). Then comes the question of whether to translate, extract, summarize or convert into a structured form, with "so many ways to experiment how to expose information to the model," and "one of the reasons this is hard is it keeps changing." Checking cross-references the results against other sources; on stage Long mentioned internal notes, views, past conversations and meeting recordings, and called cross-referencing disparate sources "super powerful." (The slide's note for this line is narrower: every quote matches its source.) Finally a person may update the firm's view.

## 5. Every line needs a runner

![You need a runner](../slides/10-13-1630-a-hedge-fund-as-a-function/13.jpg)
*Four kinds of runner, each with an example.*

"I think of each line as having a runner," Long said. The slide named four:

| Runner | Example on the slide | Why, per Long |
| --- | --- | --- |
| Code | `if pe < 15: keep()` | "It's fast. It's cheap. It's reliable." |
| ML (statistics, classical machine learning) | `model.fit(x, y)` | Models "much more complex relationships than just pure deterministic code" |
| LLM | `summarize(transcript)` | "It's probably totally impossible to build a bunch of if statements that can summarize better than a modern LLM." |
| Person | `meet(ceo)` | Judgment and "the ability to navigate the real world" |

A price-to-earnings screen belongs in deterministic code; fitting a regression to a pattern belongs to statistics. For meeting a company's management, "I probably don't want to send an LLM or an AI agent in to do that. I probably don't want to send an if statement into that meeting." In many cases, "a person is the best runner for a given piece of the function."

"There is no glory in automating things for the sake of automating things," Long said. "The goal is not to take as many things and make them LLM as possible for us." The sentence that followed, stating what the goal is, falls in one of the caption gaps.

## 6. One function, many ways to wire it

![gather() for the Korean news: one function, many ways to wire it](../slides/10-13-1630-a-hedge-fund-as-a-function/14.jpg)
*Three sources and internal notes flow through a graph of steps to a proposal and a review.*

A real version is "probably not just a linear statement." The slide drew `gather()` as a graph. Three sources labeled `korean_news`, `dart_filings` and `call_transcripts` enter `collect()`, then branch through `dedupe()`, `rank()`, `filter()` and `translate()` into `extract_quotes()`, `extract_figures()` and `entity_link()`, then into `check_sources()`, `compare_to_last_view()`, `key_stock_drivers()` and `compare_to_holdings()`, with the firm's own notes feeding in, before `propose()` and `review()`. Dashed loops send work back when a quote doesn't match its source and when a reviewer asks for a revision.

Long listed alternatives: translate and extract quotes, then check them "to make sure that these are actually things that people said"; or dedupe, filter and pull figures; or start from an existing note and look for confirmatory data points; "or maybe I'm trying a red team. There's a million different ways to configure this. So yet again more hyperparameters within the control flow."

![One wiring highlighted](../slides/10-13-1630-a-hedge-fund-as-a-function/15.jpg)
*Three named wirings; the third is traced in red.*

The slide named three of them: "Read the press, check the quotes"; "From the numbers to key stock drivers"; and, highlighted, "Rank first, compare to our last view," which runs from Korean news through collect, rank, translate, extract quotes and compare to last view, to propose and review.

## 7. The search space

![The same graph colored by runner](../slides/10-13-1630-a-hedge-fund-as-a-function/16.jpg)
*Each node recolored by its runner, following the legend of the runner slide.*

"Each of these boxes probably has an optimal runner." Collection is deterministic code: "I'm pulling last 24 hours for a given source." Following the shapes and colors of the runner slide, the graph marked these assignments:

| Runner | Nodes |
| --- | --- |
| Code | collect, dedupe, extract_figures, check_sources, compare_to_holdings |
| ML | rank, entity_link, key_stock_drivers |
| LLM | filter, translate, extract_quotes, compare_to_last_view, propose |
| Person | review |

Even that "probably understates the complexity," Long said. "The reality is most things are multi-runner": code plus an LLM plus a statistical method, or a person working with an LLM. "It's probably combinatorics between my four buckets." Within the LLM bucket alone there are more choices: a big, medium or small model; effort levels such as xhigh, high or medium; which harness; which skills load with the harness; which hooks load with the skills. "I have an absolutely enormous amount of hyperparameters within my search space. And this is just for this one tiny problem … I'm just filtering the news."

The way through is to break problems into pieces "that we can understand and evaluate." After another caption gap, Long described what that reveals: "Oh, no. That actually shouldn't be a model at all. That should be an if statement. Or, oh man, LLMs are really, really bad at translating that to portfolio impact. It should really be a human on that step," with a deterministic system helping that person search and pull the information. "And if you don't have a north star to hill climb against, you're just kind of … fidgeting around."

## 8. The real graph

"I cannot convey enough that this is just one tiny thing we do," Long said. Every data source passes through more than three transformations. "So the actual graph of code at funds like Coatue, it actually probably looks — oh, not like this. This is my screen saver."

![The full graph and Long's contacts](../slides/10-13-1630-a-hedge-fund-as-a-function/19.jpg)
*Thousands of input nodes converging on a single decision.*

The closing slide showed a fan of thousands of nodes of every shape and color, narrowing layer by layer to a single point, under Long's X handle (@fyxlong) and LinkedIn address. "The actual graph probably looks something like this, right? It's probably massively complex. In each of those nodes, we just talked about at least 10 hyperparameters with at least 20 different valid values for them." After a final caption gap, Long spoke of what the firm must "also absorb culturally, organizationally. That is the great challenge of applying AI [to] investing."

## 9. Beyond investing

"Beyond investing, what we're really talking about is creating a state machine that can take information from the world and understand it well enough to make coherent decisions," Long said, with uses in policy, nonprofits, drug discovery and corporate strategy. Long said the most exciting part of forums like this was talking to people in other fields whose problems "congeal on the same problem space," invited the audience to continue in the hallway, and thanked them.

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=9a9dfcd0-459f-43eb-b3f8-004864b4ad2a) · [Day 1 livestream at 6:47:46](https://www.youtube.com/watch?v=akEqiXFo3GM&t=24466s)

No standalone video of this keynote had been published at the time of writing. This write-up is based on the official Day 1 livestream captions and the author's slide photos of the livestream, plus one photo from the room. The captions have five gaps of about ten seconds each, so some sentences are missing. The schedule titles the talk "A Hedge Fund as a Function"; the slides say "An Investment Fund as a Function." The talk ended without Q&A. The [precise notes](precise/10-13-1630-a-hedge-fund-as-a-function.md) (Chinese) keep livestream timestamps, caption corrections and the open questions.
