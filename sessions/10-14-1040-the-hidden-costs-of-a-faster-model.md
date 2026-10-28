# A Faster Model Is Only Faster If the Whole Job Is

*At AI Engineer NYC 2026, MiniMax's Morgan Suo walked through the speed-ups inside the company's new M3.1 Flash model, from sparse attention to 4-bit math, and argued that each one has to be judged by the cost and time of a finished, checkable result.*

**Breakout · Wednesday, October 14, 2026 · 10:40–10:58am · McCarthy Room (Riverside Suite), Curated track**
**Speaker:** Morgan Suo, Head of US Business Development, MiniMax

---

"When a new model arrives," Morgan Suo said, "it can generate a task faster, but … should I switch to that model?" Suo described a familiar outcome: a research report comes back very quickly, "but it misses a very important source. So someone has to find that source and fix that report … So did the job really get faster with this faster model?"

Suo has been at MiniMax for almost three years and recalled that when the company first entered the American market, "almost no one knows us"; at the time it had its first video generation model. The talk covered the company's latest language model, M3.1 Flash, which Suo said would be released the following week. The plan was to look at how MiniMax reduced the model's computation, then ask how to test whether those changes are worth it. (The official video was retitled "Is a Faster Model Actually Faster? Test the Whole Job.")

## 1. The research job: is growth turning into cash?

A quick chat reply and a research report have different needs, Suo said, so "the task tells us what good performance means." The talk used one research job throughout. The analyst's question: "As AI server demand is growing, are the companies supplying the circuit boards turning this growth into cash?" (PCB, or printed circuit board, connects the electronic components inside devices such as AI servers.)

The model had several jobs: find the right reports, line up the same reporting periods, keep different businesses separate, and explain what the numbers mean. The analyst who receives the report needs to "open the source behind an important claim," check a calculation and "see what evidence could change the conclusion." "That is our standard for a finished research job," Suo said. (The slides introducing the job were not photographed.)

![Cash flow behind the growth story](../slides/10-14-1040-the-hidden-costs-of-a-faster-model/01.jpg)
*A qualitative internal review of an M3.1 Flash checkpoint on the circuit-board research job.*

The slide described a "qualitative internal review" of M3.1 Flash checkpoint 0918-e3 from September 2026. The finding it highlighted: "Profit growth ≠ Cash conversion." Its illustrative example: "Profit can rise while cash remains tied up in receivables and inventory." The verification step: "reconcile cash from operations with the income statement and working capital." The source pack was "14 interim reports · 22 source URLs · 5 testable theses." (On stage Suo described "14 company reports.")

"Profit can grow but cash flow tells a different story," Suo said. A company sells more and reports higher profit, but customers haven't paid yet, or it has bought materials still sitting in storage. So the model should point to the cash flow statement and the relevant disclosures: "a report earns trust when its claims can be checked." The review described the research output; the technical tests in the rest of the talk were separate tasks, used to understand specific trade-offs.

## 2. What changed inside M3.1 Flash

![What changed in M3.1 Flash](../slides/10-14-1040-the-hidden-costs-of-a-faster-model/03.jpg)
*"Attention and precision change the engine; serving and effort determine the research budget."*

The slide summarized the changes against the previous configuration, MiniMax M3:

| Component | Previous configuration | M3.1 Flash configuration |
| --- | --- | --- |
| First three attention layers | Full attention | Sparse attention |
| Sparse indexer and main attention | Q8KV8 | Q8KV4 |
| Routed MoE experts | MXFP8 | W4A4 NVFP4 |
| Reasoning controls | M3 can disable thinking | Always on, default effort max |

The partly hidden footer ended: "…source pack; change the configuration, then compare completed work."

Suo tied the changes to four questions: where attention work can be reduced, how data is stored and moved, how the serving setup affects how long a user waits, and how much reasoning the model does. "Every optimization has to help the finished job."

MiniMax's model card and vLLM's launch-support blog post confirm most of the table: the first three full-attention layers were replaced with sparse attention; the KV cache is stored in NVFP4 while queries stay in FP8, in both the indexer and the attention path; and the routed MoE weights are stored in NVFP4, which shrinks the checkpoint from 451.5 GB to 250.1 GB, while the shared experts stay in MXFP8. The "W4A4" label, meaning activations as well as weights in 4 bits, appears only on the slide and in the talk; the published sources mention only the weights.

## 3. Sparse attention: less work, but it still has to find the footnote

![How sparse attention works](../slides/10-14-1040-the-hidden-costs-of-a-faster-model/05.jpg)
*A lightweight indexer picks blocks; main attention runs only on those.*

"Nowadays I think every model says, oh, I have a very big context window, like we said we have 1 million," Suo said. The question is whether the model can find the right result in it quickly and correctly.

The slide showed two branches. The index branch "scores candidate blocks for each query. Example: locate the cash-flow footnote." The main branch "runs attention on selected blocks. Local context remains included." A diagram showed two query groups, each with its own pattern of selected and non-selected key blocks, combined into an attention mask. The bottom line gave the budget: "16 × 128 = 2,048 selected tokens," noting that this is not the full context window. The selection changes with each query, since different questions need different passages. (vLLM's blog describes the same mechanism: the KV cache is split into 128-token blocks, an indexer scores them, and attention runs on the top 16 blocks plus the most recent one, capping each query at 2,048 keys.)

For the research job, the point is to reduce attention work across many documents. But "a very short footnote there may change the entire conclusion," Suo said. "For a cash flow question, the footnote may matter more than the pages of industry outlook." "Less attention work still has to find the important evidence."

![Sparse attention in three more layers](../slides/10-14-1040-the-hidden-costs-of-a-faster-model/06.jpg)
*A retrieval test across input lengths and positions, up to about 830,000 tokens.*

Because M3.1 Flash now uses sparse attention in its first three layers as well, retrieval is an important check. The slide, headed "Find the decisive footnote," reported a separate synthetic test that placed facts at different positions in long inputs:

| Input tokens | Single fact, 5 positions | 4-fact sum |
| --- | --- | --- |
| 7.6–7.9K | 5/5 | — |
| 27.1–32.7K | 5/5 | — |
| 112.9–119.8K | 5/5 | 3/3 |
| 233.8–236.6K | 5/5 | — |
| 465.3–497.7K | 5/5 | 3/3 |
| 817.0–830.5K | 5/5 | 3/3 |

The last cell is hidden in the photo, but the slide's total of "39/39 pass" (30 single-fact plus nine 4-fact requests) implies 3/3. The model passed all 39 requests, Suo said, and the longest inputs were about 830,000 tokens, but "the result covers the cases in this test." For real reports, the questions are different: can the model find a cash-flow footnote, use the right reporting period, and spot a conflict between documents? Suo's advice was to move the important passage to different places in the input and ask the same question again.

## 4. 4-bit KV cache: a stable average can hide a drop

![Four-bit KV: check the workload behind the average](../slides/10-14-1040-the-hidden-costs-of-a-faster-model/07.jpg)
*Moving keys and values from 8 to 4 bits barely moved the average, but one benchmark fell 6.6 points.*

The model keeps internal information from earlier text in what is called the KV cache. M3.1 Flash "changes its keys and values from eight bits to four bits, but the query still stays at eight bits," Suo said. That reduces the data stored and moved. The slide noted 8-bit queries, 4-bit keys and values, and block scales per 16 values. Its table came from an internal engineering evaluation dated September 21, with speculative decoding (DSpark, M3.1 Flash's draft model) enabled:

| Evaluation | Q8KV8 | Q8KV4 | Change |
| --- | --- | --- | --- |
| Macro average | 61.72 | 61.83 | +0.11 |
| GPQA-Diamond | 79.3 | 80.8 | +1.5 |
| SciCode | 48.6 | 42.0 | −6.6 |
| RISE-Fix1 | 34.0 | 39.25 | +5.25 |

The slide's warning in red: regressions "can hide behind a stable average." "The average scores actually is almost the same, but the SciCode score there, it drops like 6.6 percentage," Suo said, while other scores rose. "The average score actually can hide a mistake … your user cares about." The table compares two complete setups, Suo added, so "it does not tell us which changes caused each score." For the research job, that means checking the output again: are the sources right, are the calculations right, does the explanation match the evidence?

## 5. 4-bit experts: where the scale is restored matters

![Routed experts move to NVFP4 W4A4](../slides/10-14-1040-the-hidden-costs-of-a-faster-model/08.jpg)
*Routed experts move to 4-bit weights and activations; the shared expert stays unchanged.*

M3.1 Flash is a mixture-of-experts model: it has computing parts called experts and selects some of them for each token. "In M3.1 Flash, these selected experts use four-bit weights and activations. The shared expert is outside this change," Suo said. Weights are the model's stored values; activations are values the model calculates. Lower precision reduces data movement and supports more efficient computation, and the slide noted the cost side: "Scaling and restoration still add work."

But the calculation has several steps: "we scale the numbers, we run the calculation and restore the scale. The position of these steps really matters." The slide's table, "Scale restoration changes the result," showed where:

| Path | Activation-scale handling |
| --- | --- |
| FC1 | Dynamic per row. Restore after GEMM, before nonlinear activation |
| FC2 | Fixed encoding factor: 16. Restore with × 1/16 after GEMM |

"Getting that order right is the part of getting the answer right. So fast math still has to be correct," Suo said. The test has to cover the complete implementation, and for the research job, the same questions are run again to see why answers still miss the checks. The slide's red line: "verify derived numbers after changing the numerical path."

## 6. Speculative decoding: count the checking time too

![Draft faster. Verify. Measure accepted output.](../slides/10-14-1040-the-hidden-costs-of-a-faster-model/09.jpg)
*Draft, verify, accept or correct: both drafting and verification cost time.*

Moving from the model to how it generates answers, Suo described speculative decoding. A faster draft process proposes a short sequence of tokens, "and then the target model can check it, and the system keeps this first part that the target accepts and makes a correction when it's needed." The slide showed the three steps: draft, verify (target-model scoring), and accept or correct (keep the prefix, then continue).

"The opportunity here is to get several tokens from one target check," Suo said, "but drafting takes time, checking takes time, rejecting these tokens also takes time. So count the time spent checking the draft too." The slide made the same point: "Drafting and target verification both contribute to the cost of a completed answer." These checks verify tokens, but "financial claims and sources need their own checks." For the research job, the measurement should also include source retrieval, tool calls and retries. "The final measure is the time to a report that an analyst can really use."

## 7. Throughput nearly doubled; the slowest first tokens took 7.9 times longer

![1.9x throughput. 7.9x p95 first-token wait.](../slides/10-14-1040-the-hidden-costs-of-a-faster-model/10.jpg)
*A load test of simultaneous story-generation requests at low reasoning effort.*

Even when one request runs well, many users change the experience. A separate serving test ran simultaneous story-generation requests at low reasoning effort:

| Concurrent requests | Aggregate output tokens/s | Median first-token wait | p95 first-token wait |
| --- | --- | --- | --- |
| 32 | 1,092 | 0.99 s | 1.46 s |
| 64 | 2,043 | 1.03 s | 11.59 s |
| 128 | 4,138 | 11.29 s | 11.71 s |

Going from 32 to 64 requests, total output per second grew about 1.9 times, but the wait for the first token at the 95th percentile went from about 1.5 seconds to about 11.6, "eight times longer," Suo said. (The slide gives 1.46 s to 11.59 s, or 7.9 times.) The median barely moved, "but we cannot only look at the median time." The users feel the wait. (Suo also mentioned throughput of "more than 300 per second," a figure that doesn't match any number on the slide.) The talk did not discuss the 128-request row, where the median wait also jumped to 11.29 seconds.

The slide's partly hidden footer said a GPU "does not remove queueing" and suggested setting a p95 "time-to-usable-memo target, then test throughput at that limit." The first token may only be reasoning, Suo said, so what matters is when the usable report arrives: set a deadline for the report, then choose a load that meets it.

## 8. Reasoning controls: leave effort blank and you get max

![Default max changes the work you request](../slides/10-14-1040-the-hidden-costs-of-a-faster-model/11.jpg)
*In M3.1 Flash, thinking is always on, and an omitted effort setting becomes max.*

One more setting changes the work the model is asked to do. "Thinking stays on, but if we leave the effort blank, the default is max," Suo said. "So a default can change the amount of work we request." The slide stated it plainly: "M3.1 Flash requires adaptive thinking. Missing effort becomes max." Under "What actually reaches the model" it listed: effort omitted becomes max; reasoning remains on; record the effective route and prompt. MiniMax's API documentation matches: `reasoning_effort` accepts `low`, `medium`, `high`, `xhigh` or `max`, "defaults to `max` when omitted," and M3.1 Flash returns an error if thinking is disabled, which M3 allowed.

The slide's evaluation on matched items showed the trade-off (output tokens include reasoning):

| Evaluation | High | Max |
| --- | --- | --- |
| MMLU-Pro accuracy | 76.9% | 78.5% |
| Mean output tokens | 1,005 | 1,851 |
| LiveCodeBench accuracy | 68.0% | 80.0% |
| Mean output tokens | 7,703 | 10,443 |

On the coding test, high effort "uses fewer output tokens than the max, but the accuracy actually fell from 80% to 68%," Suo said; the slide put it as "LCB high used fewer tokens but lost 12 accuracy points." "Give this task the thinking budget it really needs." For the research job, that means comparing settings on the same reports, recognizing that a quick first pass and a report ready for review may need different budgets, and recording which setting was used.

## 9. Would you ship this research workflow?

![Would you ship this research workflow?](../slides/10-14-1040-the-hidden-costs-of-a-faster-model/12.jpg)
*Three release checks: evidence and numbers, cost and latency, reproducibility.*

The slide asked the audience to "evaluate the complete serving configuration on the same task, sources and deadline." Suo's procedure: fix the question and the source reports, use the same information cutoff, and agree on what makes a report acceptable before running the comparison. Then check three things.

First, evidence and numbers ("Audit citations and calculations"): can the analyst trust the report, open the sources behind important claims, check reporting periods, recalculate the financial numbers and look for evidence that could change the conclusion?

Second, cost and latency ("Cost per acceptable memo"): what did the finished job cost and how long did it take? "Include retrieval, tool calls and retries. So if a person must fix the report before use, count that work too," Suo said, and include all failed attempts in the cost of producing acceptable reports. Test at the number of users expected, check whether reports arrive within the promised time, and "measure the cost of a report you can actually use."

Third, reproducibility ("Pin versions; replay; roll back"): save the model and runtime versions, the prompt settings and the sources; keep examples of failures; and "start with a small rollout and keep a way to switch back." The slide's footer listed the checkpoint, runtime, prompt, effort and sources to pin, and ended: "Canary; compare; roll back."

## 10. Measure the cost of an acceptable result

![Measure the cost of delivering an acceptable result at the required latency](../slides/10-14-1040-the-hidden-costs-of-a-faster-model/13.jpg)
*The closing slide: "Traceable evidence. Correct calculations. A deadline the user can accept."*

Before finishing, Suo played a short recap of the talk generated with MiniMax's AI video model from a single text prompt. Then Suo returned to the opening question of whether to switch to a faster model. For the analyst, the finished job is "a report with sources they can open, and it has numbers they can check; it must arrive when they need it." M3.1 Flash changes how computation is spent, and the complete task shows what those changes mean for the user. The closing slide read: "Measure the cost of delivering an acceptable result at the required latency."

"Faster models really matter," Suo said, "but what matters most is what we can do with them."

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=f3445fe7-308b-4e84-8490-fd78a13db4fb) · [Video](https://www.youtube.com/watch?v=YvQ4BVTWfHI) · [MiniMax API documentation (OpenAI-compatible)](https://platform.minimax.io/docs/api-reference/text-openai-api) · [MiniMax-M3.1-Flash model card](https://huggingface.co/MiniMaxAI/MiniMax-M3.1-Flash) · [MiniMax Agent: M3.1 Flash](https://agent.minimax.io/tools/m3-1-flash-preview) · [vLLM: Day-0 Support for MiniMax-M3.1-Flash](https://vllm.ai/blog/2026-10-23-minimax-m31-flash)

This write-up is based on the official video's captions (which cover the first six minutes that the author's room recording missed), the room recording, slide photos and the sources above; the slides introducing the research question were not photographed. The video ends after the talk without a Q&A. The [precise notes](precise/10-14-1040-the-hidden-costs-of-a-faster-model.md) (Chinese) keep timestamps and the full record of where the talk, slides and MiniMax's documentation differ.
