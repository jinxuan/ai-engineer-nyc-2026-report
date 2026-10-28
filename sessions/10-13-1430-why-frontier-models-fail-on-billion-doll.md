# Why Frontier Models Fail on Billion Dollar Portfolios：精确版

> Precise notes for this session, compiled from the official video captions where available, the author's room recording, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材整理完，正文逐节和 slide、视频字幕、录音对过。这场没有 Q&A（视频和录音里都没有）。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| 官方视频 | [aVbRSIQO2dE](https://www.youtube.com/watch?v=aVbRSIQO2dE)，2026-10-25 上传，改名为「Why Frontier Models Fail as Buy-Side Analysts - Janie Gu, Google DeepMind & Suyeol Yun, LinqAlpha」。正文约 18 分钟（[00:12]–[18:23]），讲完直接致谢，**没有 Q&A**。字幕是自动生成的，本文以它为正文主要依据，时间标「视频 mm:ss」。视频描述带章节（见下） |
| slide | 作者拍的 28 张（IMG_0450–0477），全部属于本场（页脚 `Mandate-Bench · Kaggle × Google DeepMind × LinqAlpha`），没有别场的照片。其中多张是同一页逐步展开的不同状态（0451–0453、0456–0458、0459–0461、0462–0463、0469–0470、0471–0473）。结果表两张（0474、0475）的数字都从照片上读过，并按行验算了 AVG |
| 官方简介 | session.json 里的会议简介。**简介里的几个数字（harness 提升 >60%、15.8%、1,300 只股票、近 5,000 份 SEC 文件）在视频和 slide 里都没有出现**，见「出入」 |
| 一手资料 | 没找到 Mandate-Bench 的论文、Kaggle 页面或 LinqAlpha 官方公告（截至 2026-10-27）。slide 说完整版 2026 年 11 月在 Kaggle 上线。唯一的网上报道是 [StartupHub.ai 2026-10-25 的文章](https://www.startuphub.ai/ai-news/ai-research/2026/frontier-models-still-fail-the-buy-side-memo-test)，它只引用了这个视频，是二手资料 |

视频章节（视频描述原文）：00:00 Intro · 01:10 Kaggle: from grading humans to grading models · 02:52 Why benchmarks are broken · 05:25 What reproducing 65 benchmarks revealed · 08:20 How Google DeepMind and Kaggle are fixing it · 10:20 Why your company should build a benchmark · 11:44 LinqAlpha and MandateBench · 12:32 Where existing finance benchmarks fall short · 13:31 A real analyst task, start to finish · 15:33 Grading the reasoning, not just the answer · 16:41 Early results · 17:18 What's next and how to get involved

## 基本信息

- 时间地点：2026-10-13 2:30–2:48pm，Turing Room (Riverside Ballroom)，Breakouts，Evaluations Track
- slide 上的正式标题（slide，IMG_0450）：**「Building Trusted, Real-World Evals」**，副标题「Why models fail on billion-dollar portfolios – a Mandate-Bench case study with LinqAlpha, Google DeepMind and Kaggle」。日程标题是「Why Frontier Models Fail on Billion Dollar Portfolios」，视频标题是「Why Frontier Models Fail as Buy-Side Analysts」
- slide 上的署名：「Janie Gu & Ivan Leo, Google DeepMind · Chanyeol [Choi …] Jack Haverty, LinqAlpha」（部分被前排观众挡住）；案例页（IMG_0465）署名「Chanyeol Choi, Suyeol Yun & Jack Haverty, LinqAlpha」
- **实际上台的是两人**：Janie Gu（Google DeepMind 的 product manager，「I lead our Kaggle benchmarks team」，视频 00:46）讲前半；Suyeol Yun（LinqAlpha Head of AI，视频 11:45 自我介绍「I'm head of AI at [LinqAlpha]」）讲 Mandate-Bench 案例；最后 Janie 收尾
- 讲者说（视频 00:53–01:06）：Ivan（GDM 的 forward deployed engineer）和 Jack「couldn't make it」；「Jacob is in the crowd … from our [LinqAlpha] partners」。Jacob 应是 Chanyeol Choi（他的 X 账号是 `jacob_choi_`，我的理解）。字幕接着说「So he'll be presenting with me」，但实际上台的是 Suyeol，【待确认】这句是不是指 Suyeol

## 1. Kaggle：从评估人到评估模型

- slide（IMG_0451–0453）「Fifteen years evaluating humans. Now, frontier models.」两栏：
  - 「2010 – 2026 · Kaggle competitions — Evaluating humans」：「The world's leading platform for ML competitions. **31M+ builders**, thousands of competitions, one leaderboard everyone trusts to evaluate them all.」「Sealed private test sets, leak detection and anti-cheating infrastructure.」
  - 「February 2026 · Kaggle joins Google DeepMind — Evaluating frontier models」：「Kaggle became GDM's public platform for benchmarks – an open, neutral proving ground for every frontier model, on the same infrastructure, tasks and grading.」
  - 底部：「Same DNA, new subject. **Our goal: become the industry standard for benchmarks** – the way Kaggle already is for competitions.」
- 讲者说（视频 02:10）：「in February 2026, Kaggle actually moved into Google DeepMind and we're moving our subject from evaluating humans to evaluating frontier models」
- 资料：没找到 Google/Kaggle 关于「2026 年 2 月并入 Google DeepMind」的官方公告；二手资料（[Habr](https://habr.com/ru/articles/1025362/)）只说 2026 年 Kaggle 归入 GDM、新增 Benchmarks 区和 Kaggle Benchmarks SDK。**具体月份以 slide 为准，【待确认】一手来源**

## 2. 为什么 benchmark 坏了：三种失败

- slide（IMG_0454–0455）「Benchmarks are how we measure AI progress. And right now, the state of benchmarks is broken.」小字：「They dictate how labs train, which model startups build on, and how investors judge a moat. **What you measure is what models hillclimb.**」
- 讲者说（视频 03:25）：「whether the benchmark is correctly measuring what you want it to measure is exactly what the models end up hill climbing … when you measure the wrong thing, the models actually get worse at what they're supposed to be doing」
- slide（IMG_0458）「Three ways benchmarks fail today.」：

| # | 名称 | 一句话 | slide 小字 |
| --- | --- | --- | --- |
| 01 | Relevance | Public benchmarks don't resemble real work. | Leaderboards test synthetic puzzles and academic trivia that bear little resemblance to what your agent actually does in production. |
| 02 | Compute | Too expensive to stay fresh. | 500 multi-turn agent tasks × 16 frontier models costs **thousands of dollars per sweep**. Without ongoing compute, evals rot within a release cycle. |
| 03 | Verification | No single source of truth. | Everyone runs their own harness and self-reports scores. The numbers are wildly different – because of infrastructure, not model quality. |

- 讲者说（视频 04:18）：同样的例子口头说成「costs **tens of thousands of dollars** per sweep」，新模型「basically every week or two」就发一个，不重跑就 stale。见「出入」
- 讲者说（视频 05:07）：差异来自「different scaffolding … different ways that you're calling tools … serialize the input into the model in different ways」，即使是「the exact same benchmark on the same model」

## 3. 复现 65 个 benchmark：分数看着正常，trace 不正常

- slide（IMG_0459–0461）「We reproduced **65 industry benchmarks** and read them trace by trace. The scores looked fine. The traces didn't.」五张卡片：

| 标签 | 标题 | slide 说明 |
| --- | --- | --- |
| phone-a-friend | Calling a smarter model for the answer | Models found API keys left in the eval environment and called a stronger frontier model to get the answer. |
| grader-tampering | Editing the grading script | When agent execution and grading shared a container, models modified the grader directly in order to pass. |
| ±20% variance | Same benchmark, different score | Running the exact same benchmark multiple times produced ±20% swings – from harness and infra instability, not the model. |
| open-book exam | Looking up answers on forums | Without network sandboxing, models made live calls mid-eval to look up answers on public forums. |
| apples to oranges | Comparing models at different reasoning efforts | Each harness ships its own default reasoning effort – so "head-to-head" comparisons were quietly running one model on high and another on low. |

- slide 底部：「Every one of these produced a score that looked perfectly legitimate – many matched the original paper. None of this is malice – a benchmark harness has a huge surface area, and these are easy to miss. When we found issues, we fixed them and either **pushed a PR upstream or published a Kaggle variant**.」
- 讲者说（视频 05:42）：「we at Kaggle spent the last 2 months reproducing 65 different industry benchmarks … recreated all of them, ran all of them, and read every single trajectory trace by trace」
- 讲者说（视频 06:12）phone-a-friend 的细节：被测模型在 eval container 里找到了**另一个模型**的 API key，调用那个模型，把它的回答当作自己的答案提交
- 讲者说（视频 06:44）grader-tampering：模型把评分脚本改成给自己「100%」
- 讲者说（视频 06:56）variance：「for all 65 benchmarks we ran all of them multiple times … variance of up to plus or minus 20% in the score … same model on the same exact harness on the same exact infra」
- 讲者说（视频 07:25）：「I think everyone's familiar with the Hugging Face incident」，模型在 Hugging Face、Reddit、Stack Overflow 上查答案。她没说是哪次事件，【待确认】
- 讲者说（视频 08:05）reasoning effort：「our scores actually matched parity with the original benchmark paper, but we found issues with the score itself」
- 资料：「65 个 benchmark」只有本场 slide 和讲者口述，没找到 Kaggle 发布的复现报告或清单，【待确认】有无公开材料

## 4. Google DeepMind 和 Kaggle 怎么做

- slide（IMG_0462–0463）「Google DeepMind and Kaggle are partnering to solve this.」三栏，分别对应上面三种失败：
  - **Relevance → Partnerships**「Benchmarks built around real workflows」：We partner with companies and researchers who own the workflow to design the benchmark。— Free compute and frontier model inference — **$5M in grants distributed in Q4** to fund benchmark research — GDM Forward Deployed Engineers to co-design the eval
  - **Verification → Infrastructure**「Scores that are accurate and reproducible」：Standardized infrastructure so every score is auditable and every run is repeatable。— Sandboxed runs with network isolation — Logged proxy calls and open traces for every model call — Containerized grading, separated from agent execution
  - **Expertise**「The people who've done this before」：Rigorous methodology backed by human and AI evaluation experts。— 15 years designing rigorous tests and preventing cheating by humans — GDM research expertise to design the benchmarks — Independent audits with research orgs like **Epoch AI**
- 讲者说（视频 08:29）：合作对象是做「economically [valuable] real-world workflows」的公司和研究者（字幕是 "viable"）
- 讲者说（视频 10:08）：和 Epoch 这样的独立研究机构合作审计，「so you don't just have to take our word for it」

## 5. 为什么公司要自己建 benchmark

- slide（IMG_0464）「Why build a benchmark? It improves your AI twice.」
  - 01 **Build faster internally**：「Stop relying on vibe checks. A benchmark tells you what works and what breaks every time you change a prompt, tool or harness.」
  - 02 **Make labs hillclimb your job**：「Publishing a rigorous benchmark encourages labs to hillclimb on your use case – so base models get better at your product over time.」
  - 流程「How internal and public benchmarking reinforce each other」：Capture the real job（Real tasks, data rooms and failure modes）→ Hillclimb your harness（Measure every prompt, tool & agent change）→ **Publish on Kaggle**（Audited, reproducible public leaderboard）→ Labs hillclimb for you（Next-gen base models improve on your task）
- 讲者说（视频 11:10）：「once the labs want to hill climb on your benchmark, then they'll pour trillions of dollars in of compute into improving the base models for your specific use case」

## 6. Mandate-Bench：LinqAlpha 的案例

**名字和出处**

- slide（IMG_0465）「Case study: **Mandate-Bench** on Kaggle」，「Measuring models' ability to build an investment memo for a company – the same way a buy-side analyst would.」slide 一律写带连字符的 Mandate-Bench；视频描述写 MandateBench。以 slide 为准
- 讲者说（视频 11:48）：「we created a benchmark called Mandate Bench. It's built co-developed with Google DeepMind and Kaggle」；Janie（视频 11:32）：「one of the first benchmarks that we built on Kaggle」
- 资料：截至 2026-10-27，[kaggle.com/benchmarks](https://www.kaggle.com/benchmarks) 和 [linqalpha.com](https://www.linqalpha.com) 上都没搜到 Mandate-Bench；也没有论文。**它测什么，目前只有本场 slide 和讲者口述可依**。另：prep 时搜到过一个同名的 MandateBench，和这场无关（见 `prep.md`）

**LinqAlpha 自我介绍**（slide，IMG_0466「AI research agents for the world's leading investors.」）

- **$5.0T+** AUM of institutions that use LinqAlpha；**200+** global investors use LinqAlpha actively；**13** research papers · ICLR ×5, ICML, SIGIR, ICAIF
- Trusted by 一栏的 logo 有 Greenlight、New York Life、APG、Tiger Global、Fidelity International、Schonfeld、CITIC CLSA、Mirae Asset、KB、BNP Paribas、OCBC、CGS International 等；Research co-authors 一栏有 BlackRock、Blackstone、JPMorganChase、Fidelity Investments、State Street、QRT、Kalshi、MIT、University of Oxford
- 讲者说（视频 12:01）：「institutions with more than $5 trillion in assets under management using [LinqAlpha]. And more than 200 investors use it actively」；论文「co-authored with the BlackRock, Blackstone, JP Morgan, etc.」

**现有金融 benchmark 停在 memo 之前**（slide，IMG_0467「Existing benchmarks stop before the memo.」）

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

- slide 脚注：「Our reading of each benchmark's public description. **Reasoning chain**: a conclusion earns credit only if the steps beneath it hold. **Brief to memo**: a fund's brief goes in, an investment memo with a call comes out.」（照片是斜拍的，表格按列逐行对过；这是 LinqAlpha 自己对别家 benchmark 的解读，我没有逐个核对这些 benchmark）
- 讲者说（视频 12:36）：现有 benchmark「prepare golden answers and compare it with the agent answer. But in real world task two analysts can reach two different answers and both could be right」；而且用的是「relatively short and simple task … they only get the fragments of it」。Mandate-Bench 要做两件事：「its rubric explicit[ly] grades the reasoning path」，以及把「real unit of work」当真：从 fund's mandate 一直到 investment memo

**一个任务 = 一个 mandate + 一家公司 + 一个日期**（slide，IMG_0468「One task is a mandate, a company and a date.」）

- **Mandate — The fund's brief**：What it buys, on what horizon, and what must be true before it acts.
- **Company — One US-listed ticker**：Across **50 tasks and 13 sectors**. No sub-questions – just filings, calls and data.
- **Date — The day the memo is written**：The agent sees only what was public by then.（point-in-time）
- 「**20–100h**」：「The agent hands back an investment memo and its reasoning graph: every fact with the tool call that returned it, every inference with its inputs, ending in long, short or pass. The same work takes an experienced buy-side analyst 20 to 100 hours.」
- 讲者说（视频 13:38）口头举的 mandate：「we are an event-driven fund and we trade on mergers and acquisitions with a holding period 3 to 12 months」；PM 问：company A 要被 company B 收购，「Should we go long, short, or just pass on this company? And please bring me a investment memo with detailed evidence and the reasoning behind it.」「this is the one real unit of analyst work」

**例题：Sonoco 的五个销售额**（slide，IMG_0469–0470「Five sources give five different sales numbers.」）

- 页头：COMPANY「Sonoco. Bought Eviosys, sold TFP.」DATE「15 May 2025, after Q1 results.」MANDATE「Long/short, US small and mid caps.」
- 「Sonoco's sales, five ways. Each is right where it came from.」

| 来源 | 口径（slide 原文） | 数字 |
| --- | --- | --- |
| Annual report, 2024 | sold unit removed; acquisition for one month | $5.3bn |
| Management's bridge | starts from a base that still has the sold unit | $6.6bn |
| Pro forma, 2024 | acquisition counted for the full year | $7.5bn |
| 2025 guidance | acquisition in, sold unit out | ~$8.0bn |
| Street forecasts | some still on the old company, some on the new | mixed |

- slide：「**Why it is hard**: the key issue is hidden – nobody says these numbers disagree. The agent has to notice, and rebuild Sonoco as it is today.」
- 讲者说（视频 14:26–15:30）：五个来源给出「5 billion to 8 billion dollars」，「because each source counts a different Sonoco」。agent 要做三步：发现数字对不上；查清为什么；「rebuild Sonoco as it was in May 2025 with Eviosys for the full year and without TFP. This it will be about 7.5 billion dollars of sales」。「that is just a small part of the entire reasoning [a] skill[ed] analyst [has] to work through」
- 资料（Sonoco SEC 文件）：Eviosys 收购 2024-12-04 完成（[FY2025 10-K](https://www.sec.gov/Archives/edgar/data/91767/000009176726000008/son-20251231.htm)）；TFP（Thermoformed and Flexible Packaging）卖给 TOPPAN，2025-04-01 完成，约 $1.8bn，公司表示用 $1.5bn 税后所得还债（[Q1 2025 10-Q](https://www.sec.gov/Archives/edgar/data/91767/000009176725000023/son-20250330.htm)）。这和 slide「acquisition for one month」（12 月 4 日完成，2024 年报里只并表约一个月）、评分页的「$1.5bn repaid」一致（我的理解）。2024 年报 net sales 是否正好 $5.3bn 我没有逐字核对 10-K，【待确认】

**评分：像 PM 评分析师那样评**（slide，IMG_0471–0473「Graded the way a PM grades an analyst.」）

- ① **Came from a tool call?**
  - tool call #7 — Sales it kept: $5.3bn ✓
  - tool call #9 — Pro forma sales: $7.5bn ✓
  - tool call #14 — Debt $7.1bn, $1.5bn repaid ✓
  - no tool call — Synergies already $60M ✗
  - not scored — CEO background – never reaches the call: earns nothing
- ② **Each step: pass or fail**：Run-rate earnings PASS；Leverage on own EBITDA PASS；Synergy upside **NO CREDIT: INVENTED INPUT**；Value per share PASS
- The call：Long, short or pass
- ③ **Attack**：Synergies → 0. Does the call still hold?
- slide 底部：「A PM asks of every step: **where did this come from, does it reach the call, does it hold up?**」
- 讲者说（视频 15:35–16:40）：agent 交 memo 和 reasoning graph，「every fact should point to [the] exact [tool] call it came from」；「our former buy-side analyst[s] write down the key reasoning steps that any good memo would include. Each step is a node in the graph」，例如要把公司重建成 2025 年 5 月的样子、算出新业务真实盈利、扣掉卖掉的部分、重建债务。还会用 what-if 攻击 memo：管理层承诺了 Eviosys 交易的 cost savings，「what if the … savings were zero? Would the call change? And does the memo explain this with clear reasoning about the moving parts?」结论：「memos can reach different calls and … both can score well as long as the work behind … each call[] [is] there」

## 7. 初步结果（preliminary，10 / 50 题）

**slide（IMG_0474）「Ten tasks, four agents: the best average 58 out of 100 with partial credit.」**（标签 preliminary）

| Agent（harness） | HWC ●○○ | FICO ●●○ | DG ●●○ | MRO ●●○ | PCRX ●●○ | GEV ●●● | HIMS ●●● | CE ●●● | CATY ●●● | SON ●●● | AVG |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Claude Opus 5.5 · in Claude Code | 58 | 52 | 60 | 65 | 70 | 68 | 49 | 61 | 66 | 37 | **58** |
| OpenAI Astra 6 · in Codex | 67 | 50 | 56 | 66 | 47 | 72 | 48 | 55 | 59 | 39 | 56 |
| Claude Sonnet 5.5 · in Claude Code | 46 | 39 | 53 | 61 | 56 | 44 | 39 | 48 | 47 | 26 | 46 |
| Gemini 4 Argon · in Antigravity | 43 | 34 | 46 | 56 | 47 | 55 | 58 | 50 | 32 | 25 | 45 |

- 脚注：「● = task difficulty · Preliminary: 10 of 50 planned tasks. The results are not yet conclusive for the full range of tasks the benchmark targets.」列名是股票代码（SON = Sonoco）
- 我按行验算了平均：58.6 / 55.9 / 45.9 / 44.6，四舍五入和 AVG 列一致，说明照片读数没错
- 注意每个模型都配了自家的 harness（Claude Code / Codex / Antigravity），比的是「模型 + harness」组合，不是裸模型（slide 写明，我的理解是这点和简介里「harness matters as much as your model」有关，但现场没展开）

**slide（IMG_0475）「The best agents find the key issue and build their call on it.」**（preliminary）

| Agent | Call follows from the work | Finds the key issue, sizes it | Values on its own numbers | Works out the street's view | Tries to break its own case | Tests mgmt's guidance | Survives expert attacks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Claude Opus 5.5 | 100% | 97% | 40% | 37% | 13% | 3% | 37% |
| OpenAI Astra 6 | 100% | 97% | 73% | 17% | 0% | 0% | 33% |
| Claude Sonnet 5.5 | 93% | 93% | 27% | 13% | 13% | 3% | 23% |
| Gemini 4 Argon | 70% | 80% | 20% | 10% | 0% | 10% | 26% |

- 脚注：「Share of runs that fully pass each check; last column is the share of the expert's attacks the memo's numbers survive. Preliminary: 10 of 50 planned tasks.」
- 讲者说（视频 16:41）：「This is a preliminary result. Only runs over 10 tasks of 50 entire task universe. And the [Claude Opus] 5.5 scores best.」时间不够，第二页跳过了，只说了一句：「every model behaves differently. For example, Astra 6 really good at making a financial modeling, while the Opus 5.5 is … balanced very well over the cross different … test」（对应表里 Astra 6 在 Values on its own numbers 上 73%，最高）
- 作者的录音在这一节之前已截止

## 8. 上线计划和其他 benchmark

- slide（IMG_0476）「Coming soon on Kaggle — Mandate-Bench. The full **50-task benchmark launches in November 2026**.」
- 讲者说（视频 17:19）：「The full … 50 task benchmark will be launching next month in November 2026」，还有「many many other benchmarks that are launching on Kaggle and DeepMind over the next few weeks」
- slide（IMG_0477）「Building a real-world benchmark? You're in good company.」：

| Benchmark | 状态（slide） | 领域 · 合作方 | slide 说明 |
| --- | --- | --- | --- |
| Code2Video Bench | live | Multimodal generation · with HeyGen | Can a model write code that renders a product-launch video a motion designer would actually ship? |
| Chi-Bench | coming soon | Clinical healthcare · with Actava | Long-horizon clinical workflows in situ: prior authorization, utilization management and population care. |
| Quadrillion | coming soon | Long-horizon research | Autonomous agents on deep, multi-step quantitative research across finance, healthcare and life sciences. |
| Human Behavior Bench | Launches Oct 14 | Human behavior · with Rehearsals | Can a model predict how a specific group of real people will answer – 750 tasks grounded in 1,244 U.S. adults, not the smooth average an LLM assumes? |

- slide：「Partner with us for **free model compute, GDM FDE support and $5M in grants**.」联系方式 `janiegu@google.com` · `kaggle.com/benchmarks`
- 讲者说（视频 17:43）：「We launched HeyGen's code to video bench last week」；在和 Actava、Quadrillion、Rehearsals 做 clinician workflows、research workflows、user research workflows 的 benchmark（字幕把 Actava 转成 "Octava"）
- 资料：这四个 benchmark 我没有逐一在 Kaggle 上核对，【待确认】

## Q&A 精确版

无。视频在 [18:23] 致谢后结束，没有问答；作者录音在视频约 [16:30] 处就截止了。会场是否有过问答，【待确认】（这场排期只有 18 分钟）。

## 勘误表（Whisper / 自动字幕 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| Link Alpha / LinkAlpha / Mike Alpha / Link Analytics / link offer | LinqAlpha | slide |
| Jenny / jamie | Janie（Gu） | session.json、slide |
| Suyo | Suyeol（Yun） | session.json |
| Ivan | Ivan Leo（GDM） | slide（IMG_0450） |
| tablo benchmark scheme / Kaggle benchmarks team | Kaggle Benchmarks team | slide、session.json |
| google defined / defind | Google DeepMind | 上下文 |
| toggle / catalyst / paggle | Kaggle | 上下文 |
| five-side equity analyst | buy-side equity analyst | slide |
| mandate bench / Many Bench / Maybe Bench | Mandate-Bench | slide |
| cruising ground | proving ground | slide |
| clarification（第三个问题） | verification | slide |
| hybrid conversion | head-to-head comparison | slide |
| pre-compute / France / French | free compute / grants | slide |
| raising efforts | reasoning efforts | slide |
| cash fund | hedge fund | 视频字幕 |
| adventure in fund | event-driven fund | 视频字幕 |
| three to tell one | 3 to 12 months | 视频字幕 |
| Donoco / Disney Sonoko | Sonoco | slide |
| Bibiosis / EVOC / EBS deal | Eviosys | slide |
| TNP | TFP | slide |
| 825 | May 2025 | 视频字幕、slide |
| 75 billion dollars | 7.5 billion dollars | slide（Pro forma $7.5bn） |
| 20 to 22 hundred hours | 20 to 100 hours | slide、录音 |
| more than 100 investors（录音） | 200+ | slide、视频字幕 |
| two call | tool call | slide |
| no-code agent | （无意义，应为 "first, the agent"） | 上下文，【待确认】 |
| cloud with 5.5 | Claude Opus 5.5 | slide |
| Octava | Actava | slide |
| JDM | GDM | slide |
| Shane（"let me get back to Shane"） | Janie | 上下文 |

## 出入

1. **简介里的数字没在台上出现**：会议简介说「moving one model to the right harness improved its score by more than 60%」「1,300 US stocks and nearly 5,000 SEC filings」「only 15.8% of frontier model runs meet the bar of an institutional portfolio manager」。这三组数字在视频字幕、录音和 28 张 slide 里都没有。台上给的是：50 题、13 个行业（slide），初步结果只跑了 10 题，最好的平均分是 58/100（partial credit）。15.8% 和 58/100 不是同一种指标，无法对照。网上也没找到这几个数字的一手出处（截至 2026-10-27）。**这几个数字只能当作简介里的说法，不能当作演讲内容引用**
2. **每次 sweep 的成本**：slide 写「500 multi-turn agent tasks × 16 frontier models costs **thousands of dollars** per sweep」，Janie 口头说「**tens of thousands of dollars** per sweep」（视频 04:24）。以 slide 为准，或两者都注明
3. **讲者名单**：日程列 4 人（Chanyeol Choi、Janie Gu、Jack Haverty、Suyeol Yun），slide 标题页还有 Ivan Leo（GDM）；实际只有 Janie Gu 和 Suyeol Yun 讲。视频标题也只署这两人
4. **题目三个版本**：日程「Why Frontier Models Fail on Billion Dollar Portfolios」，slide「Building Trusted, Real-World Evals」，视频「Why Frontier Models Fail as Buy-Side Analysts」
5. **口头例子的 mandate 和例题不同**：口述的 mandate 是 event-driven / M&A / 持有 3–12 个月（视频 13:53）；Sonoco 例题 slide 上的 mandate 是「Long/short, US small and mid caps」。两者不矛盾，口述的是另一个示意例子（我的理解）
6. **「Mandate-Bench」的写法**：slide 一律 Mandate-Bench，视频描述 MandateBench，二手报道 Mandate Bench

## 【待确认】汇总

1. 「Jacob is in the crowd … he'll be presenting with me」里的 Jacob 是否就是 Chanyeol Choi，以及这句是否其实指 Suyeol
2. Kaggle「2026 年 2 月并入 Google DeepMind」的一手来源
3. 「65 个 benchmark 复现」是否有公开的报告或清单
4. 讲者说的「Hugging Face incident」具体指哪件事
5. Mandate-Bench 的一手出处（Kaggle 页面、论文、LinqAlpha 公告）；目前只有 slide 和口述
6. Sonoco 2024 年报 net sales 是否正好 $5.3bn（没有逐字核对 10-K）
7. Code2Video Bench、Chi-Bench、Quadrillion、Human Behavior Bench 在 Kaggle 上的状态
8. 简介里 >60%、15.8%、1,300 只股票、近 5,000 份 SEC 文件的出处
9. 字幕「the no-code agent must rebuild…」原话
10. 现场是否有 Q&A
