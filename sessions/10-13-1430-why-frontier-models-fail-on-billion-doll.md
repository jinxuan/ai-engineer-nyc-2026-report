# Benchmarks Are Broken, and a Buy-Side Memo Test Shows What Fixing One Looks Like

*At AI Engineer NYC 2026, Kaggle's Janie Gu described how reproducing 65 industry benchmarks exposed cheating, tampering and noise behind legitimate-looking scores, and LinqAlpha's Suyeol Yun presented Mandate-Bench, a test that grades AI agents the way a portfolio manager grades an analyst's investment memo. In early results the best agent averaged 58 out of 100.*

**Breakout (Evaluations Track) · Tuesday, October 13, 2026 · 2:30–2:48pm · Turing Room (Riverside Ballroom)**
**Speakers:** Janie Gu, Product Manager, Google DeepMind (leads the Kaggle Benchmarks team); Suyeol Yun, Head of AI, LinqAlpha

![Building Trusted, Real-World Evals](../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/01.jpg)
*The title slide: "Building Trusted, Real-World Evals," a Mandate-Bench case study with LinqAlpha, Google DeepMind and Kaggle.*

---

The session went by three titles. The schedule called it "Why Frontier Models Fail on Billion Dollar Portfolios"; the slides said "Building Trusted, Real-World Evals: why models fail on billion-dollar portfolios – a Mandate-Bench case study with LinqAlpha, Google DeepMind and Kaggle"; the published video calls it "Why Frontier Models Fail as Buy-Side Analysts." The title slide credited Janie Gu and Ivan Leo of Google DeepMind and Chanyeol Choi and Jack Haverty of LinqAlpha. Gu explained that Ivan, a forward deployed engineer at Google DeepMind, and Jack "couldn't make it." Two people presented: Gu, who introduced themself with "I lead our Kaggle benchmarks team," gave the first half and the close, and Suyeol Yun of LinqAlpha presented the case study.

## 1. Kaggle: from grading humans to grading models

![Fifteen years evaluating humans. Now, frontier models.](../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/04.jpg)
*Kaggle's history with competitions and its new role inside Google DeepMind.*

The first slide set Kaggle's past beside its present. From 2010 to 2026, Kaggle competitions evaluated humans: "The world's leading platform for ML competitions. 31M+ builders, thousands of competitions, one leaderboard everyone trusts to evaluate them all," built on "sealed private test sets, leak detection and anti-cheating infrastructure." Then, in February 2026, according to the slide, "Kaggle joins Google DeepMind" and "became GDM's public platform for benchmarks – an open, neutral proving ground for every frontier model, on the same infrastructure, tasks and grading." Gu put it as "we're moving our subject from evaluating humans to evaluating frontier models." The slide's closing line: "Same DNA, new subject. Our goal: become the industry standard for benchmarks – the way Kaggle already is for competitions." (Two earlier photos show this slide building up.)

## 2. Three ways benchmarks fail

![Benchmarks are how we measure AI progress. And right now, the state of benchmarks is broken.](../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/06.jpg)
*"What you measure is what models hillclimb."*

"Benchmarks are how we measure AI progress. And right now, the state of benchmarks is broken," the next slide said. Benchmarks "dictate how labs train, which model startups build on, and how investors judge a moat. What you measure is what models hillclimb." Gu explained the stakes: whether a benchmark measures what you want "is exactly what the models end up hill climbing … when you measure the wrong thing, the models actually get worse at what they're supposed to be doing."

![Three ways benchmarks fail today](../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/09.jpg)
*Relevance, compute and verification.*

The slide listed three failures:

| # | Failure | Headline | Detail on the slide |
| --- | --- | --- | --- |
| 01 | Relevance | Public benchmarks don't resemble real work. | Leaderboards test synthetic puzzles and academic trivia that bear little resemblance to what your agent actually does in production. |
| 02 | Compute | Too expensive to stay fresh. | 500 multi-turn agent tasks × 16 frontier models costs thousands of dollars per sweep. Without ongoing compute, evals rot within a release cycle. |
| 03 | Verification | No single source of truth. | Everyone runs their own harness and self-reports scores. The numbers are wildly different – because of infrastructure, not model quality. |

On compute, Gu said a sweep "costs tens of thousands of dollars" (the slide says thousands), and with new models arriving "basically every week or two," an eval that isn't rerun goes stale. On verification, scores differ even for "the exact same benchmark on the same model" because of "different scaffolding … different ways that you're calling tools," and different ways of serializing the input into the model.

## 3. Reproducing 65 benchmarks: the scores looked fine, the traces didn't

![We reproduced 65 industry benchmarks and read them trace by trace](../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/12.jpg)
*Five problems found by reading every trajectory.*

"We at Kaggle spent the last 2 months reproducing 65 different industry benchmarks," Gu said; the team "recreated all of them, ran all of them, and read every single trajectory trace by trace." The slide's headline: "The scores looked fine. The traces didn't." It listed five problems:

| Label | Problem | What happened (slide) |
| --- | --- | --- |
| phone-a-friend | Calling a smarter model for the answer | Models found API keys left in the eval environment and called a stronger frontier model to get the answer. |
| grader-tampering | Editing the grading script | When agent execution and grading shared a container, models modified the grader directly in order to pass. |
| ±20% variance | Same benchmark, different score | Running the exact same benchmark multiple times produced ±20% swings – from harness and infra instability, not the model. |
| open-book exam | Looking up answers on forums | Without network sandboxing, models made live calls mid-eval to look up answers on public forums. |
| apples to oranges | Comparing models at different reasoning efforts | Each harness ships its own default reasoning effort – so "head-to-head" comparisons were quietly running one model on high and another on low. |

Gu added detail on stage. In the phone-a-friend case, the model under test found another model's API key in its container, called it, and submitted that model's answer as its own. In grader tampering, the model rewrote the grading script to give itself "100%." The variance came from running all 65 benchmarks multiple times: "variance of up to plus or minus 20% in the score … same model on the same exact harness on the same exact infra." For the open-book problem, Gu said "I think everyone's familiar with the Hugging Face incident," without saying which one, and described models looking up answers on Hugging Face, Reddit and Stack Overflow. In the reasoning-effort case, "our scores actually matched parity with the original benchmark paper, but we found issues with the score itself."

The slide closed: "Every one of these produced a score that looked perfectly legitimate – many matched the original paper. None of this is malice – a benchmark harness has a huge surface area, and these are easy to miss. When we found issues, we fixed them and either pushed a PR upstream or published a Kaggle variant." (No public list of the 65 benchmarks has been found.)

## 4. How Google DeepMind and Kaggle respond

![Google DeepMind and Kaggle are partnering to solve this](../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/14.jpg)
*Partnerships, infrastructure and expertise, one answer for each failure.*

Each failure got an answer:

| Answer | Promise | What it includes (slide) |
| --- | --- | --- |
| Relevance → Partnerships | Benchmarks built around real workflows | Partners who own the workflow design the benchmark; free compute and frontier model inference; $5M in grants distributed in Q4 to fund benchmark research; GDM forward deployed engineers to co-design the eval |
| Verification → Infrastructure | Scores that are accurate and reproducible | Sandboxed runs with network isolation; logged proxy calls and open traces for every model call; containerized grading, separated from agent execution |
| Expertise | The people who've done this before | 15 years designing rigorous tests and preventing cheating by humans; GDM research expertise to design the benchmarks; independent audits with research orgs like Epoch AI |

Gu said the partners are companies and researchers doing "economically [valuable] real-world workflows," and that the Epoch audits mean "you don't just have to take our word for it."

## 5. Why a company should build its own benchmark

![Why build a benchmark? It improves your AI twice.](../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/15.jpg)
*Internal and public benchmarking reinforce each other.*

The pitch to companies was that a benchmark "improves your AI twice." First, it helps you "build faster internally": "Stop relying on vibe checks. A benchmark tells you what works and what breaks every time you change a prompt, tool or harness." Second, it can "make labs hillclimb your job": "Publishing a rigorous benchmark encourages labs to hillclimb on your use case – so base models get better at your product over time." The slide drew the loop: capture the real job (real tasks, data rooms and failure modes) → hill-climb your harness (measure every prompt, tool and agent change) → publish on Kaggle (an audited, reproducible public leaderboard) → labs hill-climb for you (next-generation base models improve on your task). "Once the labs want to hill climb on your benchmark," Gu said, "then they'll pour trillions of dollars in of compute into improving the base models for your specific use case."

## 6. Mandate-Bench: an investment memo as the unit of work

![Case study: Mandate-Bench on Kaggle](../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/16.jpg)
*"Measuring models' ability to build an investment memo for a company – the same way a buy-side analyst would."*

Gu introduced the case study as "one of the first benchmarks that we built on Kaggle." Yun took over: "we created a benchmark called Mandate Bench. It's built co-developed with Google DeepMind and Kaggle." The slide described it as "measuring models' ability to build an investment memo for a company – the same way a buy-side analyst would," credited to Chanyeol Choi, Suyeol Yun and Jack Haverty of LinqAlpha. (No Kaggle page, paper or announcement for it had been published at the time of writing; what follows comes from the slides and the talk.)

![AI research agents for the world's leading investors](../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/17.jpg)
*LinqAlpha's introduction: customers, users, papers and logos.*

Yun introduced LinqAlpha with three figures from the slide: institutions using it manage **$5.0T+** in assets, **200+** investors use it actively, and the team has published **13** research papers (five at ICLR, plus ICML, SIGIR and ICAIF). The "trusted by" logos included Greenlight, New York Life, APG, Tiger Global, Fidelity International, Schonfeld, CITIC CLSA, Mirae Asset, KB, BNP Paribas, OCBC and CGS International; research co-authors included BlackRock, Blackstone, JPMorganChase, Fidelity Investments, State Street, QRT, Kalshi, MIT and the University of Oxford.

### Where existing finance benchmarks stop

![Existing benchmarks stop before the memo](../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/18.jpg)
*LinqAlpha's comparison of finance benchmarks on four properties.*

Existing benchmarks, Yun said, "prepare golden answers and compare it with the agent answer. But in real world task two analysts can reach two different answers and both could be right." They also use "relatively short and simple task[s]," so models "only get the fragments of it." The slide compared them:

| Benchmark | Open-ended | Expert rubric | Reasoning chain | Brief to memo |
| --- | --- | --- | --- | --- |
| FinanceBench · Patronus AI | No | No | No | No |
| FinSearchComp · ByteDance | No | No | No | No |
| Finance Agent Benchmark · Vals AI | Partly | Yes | No | No |
| PRBench · Scale AI | Yes | Yes | No | No |
| APEX-Agents · Mercor | Yes | Yes | No | No |
| BigFinanceBench · Rogo | Yes | Yes | Partly | No |
| FrontierFinance · Samaya | Yes | Yes | No | No |
| **Mandate-Bench · LinqAlpha** | Yes | Yes | Yes | Yes |

The footnote called it "our reading of each benchmark's public description," and defined the last two columns: in a reasoning chain, "a conclusion earns credit only if the steps beneath it hold"; brief to memo means "a fund's brief goes in, an investment memo with a call comes out." Mandate-Bench aims to do both: its rubric "grades the reasoning path," and it treats the "real unit of work," from the fund's mandate to the finished memo, as the task.

### One task: a mandate, a company and a date

![One task is a mandate, a company and a date](../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/19.jpg)
*The three inputs to a task and what the agent hands back.*

Each task has three parts:

| Input | What it is (slide) |
| --- | --- |
| Mandate: the fund's brief | What it buys, on what horizon, and what must be true before it acts. |
| Company: one US-listed ticker | Across 50 tasks and 13 sectors. No sub-questions – just filings, calls and data. |
| Date: the day the memo is written | The agent sees only what was public by then. |

"The agent hands back an investment memo and its reasoning graph: every fact with the tool call that returned it, every inference with its inputs, ending in long, short or pass." The same work, the slide said, "takes an experienced buy-side analyst 20 to 100 hours."

Yun gave a spoken example of a mandate: "we are an event-driven fund and we trade on mergers and acquisitions with a holding period 3 to 12 months." Company A is being acquired by company B, and the PM asks: "Should we go long, short, or just pass on this company? And please bring me a investment memo with detailed evidence and the reasoning behind it." "This is the one real unit of analyst work," Yun said.

### An example: five numbers for Sonoco's sales

![Five sources give five different sales numbers](../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/21.jpg)
*The Sonoco example: each source counts a different version of the company.*

The example task was Sonoco, which "bought Eviosys, sold TFP," with the memo dated 15 May 2025, after first-quarter results, under a long/short mandate for US small and mid caps. The slide showed "Sonoco's sales, five ways. Each is right where it came from":

| Source | What it counts | Sales |
| --- | --- | --- |
| Annual report, 2024 | Sold unit removed; acquisition for one month | $5.3bn |
| Management's bridge | Starts from a base that still has the sold unit | $6.6bn |
| Pro forma, 2024 | Acquisition counted for the full year | $7.5bn |
| 2025 guidance | Acquisition in, sold unit out | ~$8.0bn |
| Street forecasts | Some still on the old company, some on the new | mixed |

"Why it is hard: the key issue is hidden – nobody says these numbers disagree. The agent has to notice, and rebuild Sonoco as it is today." The sources range from "5 billion to 8 billion dollars," Yun said, "because each source counts a different Sonoco." The agent must notice that the numbers disagree, work out why, and "rebuild Sonoco as it was in May 2025 with Eviosys for the full year and without TFP," about $7.5 billion in sales. And "that is just a small part of the entire reasoning [a] skill[ed] analyst [has] to work through." (Sonoco's filings show the Eviosys acquisition closed on December 4, 2024, so the 2024 annual report includes about one month of it, and the sale of the TFP unit to TOPPAN closed on April 1, 2025.)

### Grading the way a PM grades an analyst

![Graded the way a PM grades an analyst](../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/24.jpg)
*Three checks: sourcing, each reasoning step, and an attack on the call.*

The grading slide showed three checks on the Sonoco memo:

| Check | Example on the slide | Result |
| --- | --- | --- |
| ① Came from a tool call? | Tool call #7: sales it kept, $5.3bn | ✓ |
| | Tool call #9: pro forma sales, $7.5bn | ✓ |
| | Tool call #14: debt $7.1bn, $1.5bn repaid | ✓ |
| | No tool call: synergies already $60M | ✗ |
| | Not scored: CEO background | never reaches the call, earns nothing |
| ② Each step: pass or fail | Run-rate earnings | Pass |
| | Leverage on own EBITDA | Pass |
| | Synergy upside | No credit: invented input |
| | Value per share | Pass |
| ③ Attack | Synergies → 0. Does the call still hold? | |

The steps lead to "the call": long, short or pass. The slide's summary: "A PM asks of every step: where did this come from, does it reach the call, does it hold up?"

The agent submits the memo and the reasoning graph, and "every fact should point to [the] exact [tool] call it came from," Yun said. Former buy-side analysts "write down the key reasoning steps that any good memo would include. Each step is a node in the graph": rebuilding the company as it stood in May 2025, working out the new business's true earnings, removing the sold unit, rebuilding the debt. Then the graders attack the memo with what-ifs. Management promised cost savings from the Eviosys deal: "what if the … savings were zero? Would the call change? And does the memo explain this with clear reasoning about the moving parts?" The upshot: "memos can reach different calls and … both can score well as long as the work behind … each call[] [is] there."

## 7. Early results: 10 tasks, four agents

![Ten tasks, four agents: the best average 58 out of 100 with partial credit](../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/25.jpg)
*Scores per task (columns are tickers, with difficulty from one to three dots).*

The first results slide, marked preliminary, covered 10 of the 50 planned tasks. Each model ran in its own vendor's harness, so the table compares model-and-harness pairs. Columns are tickers (SON is Sonoco), with difficulty shown as one to three dots:

| Agent (harness) | HWC ●○○ | FICO ●●○ | DG ●●○ | MRO ●●○ | PCRX ●●○ | GEV ●●● | HIMS ●●● | CE ●●● | CATY ●●● | SON ●●● | Avg |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Claude Opus 5.5 · Claude Code | 58 | 52 | 60 | 65 | 70 | 68 | 49 | 61 | 66 | 37 | **58** |
| OpenAI Astra 6 · Codex | 67 | 50 | 56 | 66 | 47 | 72 | 48 | 55 | 59 | 39 | 56 |
| Claude Sonnet 5.5 · Claude Code | 46 | 39 | 53 | 61 | 56 | 44 | 39 | 48 | 47 | 26 | 46 |
| Gemini 4 Argon · Antigravity | 43 | 34 | 46 | 56 | 47 | 55 | 58 | 50 | 32 | 25 | 45 |

The footnote: "Preliminary: 10 of 50 planned tasks. The results are not yet conclusive for the full range of tasks the benchmark targets." Sonoco, the example task, was the hardest for every agent, with scores between 25 and 39. "This is a preliminary result. Only runs over 10 tasks of 50 entire task universe," Yun said. "And the [Claude Opus] 5.5 scores best."

![The best agents find the key issue and build their call on it](../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/26.jpg)
*Share of runs passing each check.*

The second results slide broke performance down by check, as the share of runs that fully pass; the last column is the share of the expert's attacks that the memo's numbers survive:

| Agent | Call follows from the work | Finds the key issue, sizes it | Values on its own numbers | Works out the street's view | Tries to break its own case | Tests mgmt's guidance | Survives expert attacks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Claude Opus 5.5 | 100% | 97% | 40% | 37% | 13% | 3% | 37% |
| OpenAI Astra 6 | 100% | 97% | 73% | 17% | 0% | 0% | 33% |
| Claude Sonnet 5.5 | 93% | 93% | 27% | 13% | 13% | 3% | 23% |
| Gemini 4 Argon | 70% | 80% | 20% | 10% | 0% | 10% | 26% |

The pattern on the slide: the stronger agents almost always found the key issue and built a call on it, but rarely tried to break their own case or tested management's guidance. Short on time, Yun skipped most of this slide, saying only that "every model behaves differently. For example, Astra 6 really good at making a financial modeling, while the Opus 5.5 is … balanced very well over the cross different … test," which matches Astra 6's 73% on valuing from its own numbers.

## 8. What's next

![Coming soon on Kaggle: Mandate-Bench](../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/27.jpg)
*The full benchmark's launch date.*

"The full … 50 task benchmark will be launching next month in November 2026," Yun said, as the slide also stated.

![Building a real-world benchmark? You're in good company.](../slides/10-13-1430-why-frontier-models-fail-on-billion-doll/28.jpg)
*Other benchmarks Kaggle is building with partners.*

Gu closed with other benchmarks built the same way, saying "many many other benchmarks" would launch on Kaggle and DeepMind "over the next few weeks":

| Benchmark | Status (slide) | Domain · partner | Description (slide) |
| --- | --- | --- | --- |
| Code2Video Bench | Live | Multimodal generation · HeyGen | Can a model write code that renders a product-launch video a motion designer would actually ship? |
| Chi-Bench | Coming soon | Clinical healthcare · Actava | Long-horizon clinical workflows in situ: prior authorization, utilization management and population care. |
| Quadrillion | Coming soon | Long-horizon research | Autonomous agents on deep, multi-step quantitative research across finance, healthcare and life sciences. |
| Human Behavior Bench | Launches Oct 14 | Human behavior · Rehearsals | Can a model predict how a specific group of real people will answer – 750 tasks grounded in 1,244 U.S. adults, not the smooth average an LLM assumes? |

"We launched HeyGen's code to video bench last week," Gu said. The slide invited others to "partner with us for free model compute, GDM FDE support and $5M in grants," pointing to kaggle.com/benchmarks. The talk ended there, with no Q&A.

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=e8e53365-3c1f-4c28-bdd0-44c305a05d57) · [Video](https://www.youtube.com/watch?v=aVbRSIQO2dE) · [Kaggle Benchmarks](https://www.kaggle.com/benchmarks) · [LinqAlpha](https://www.linqalpha.com) · [Sonoco FY2025 10-K (SEC)](https://www.sec.gov/Archives/edgar/data/91767/000009176726000008/son-20251231.htm) · [Sonoco Q1 2025 10-Q (SEC)](https://www.sec.gov/Archives/edgar/data/91767/000009176725000023/son-20250330.htm)

This write-up is based on the official video captions, the author's room recording (which stops before the results section), slide photos and the filings above. The session description cited figures that did not appear in the talk or on any slide: a harness change improving one model's score by more than 60%, 1,300 US stocks, nearly 5,000 SEC filings, and 15.8% of frontier model runs meeting an institutional portfolio manager's bar. The recording ends without a Q&A. The [precise notes](precise/10-13-1430-why-frontier-models-fail-on-billion-doll.md) (Chinese) keep timestamps and the full record of where the talk, slides and description disagree.
