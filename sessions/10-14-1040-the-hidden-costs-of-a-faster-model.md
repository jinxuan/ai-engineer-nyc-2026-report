# The Hidden Costs of a Faster Model：精确版

> Precise notes for this session, compiled from the official video captions where available, the author's room recording, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材整理完，正文逐节和 slide、视频字幕、录音、MiniMax 文档对过。这场没有 Q&A。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| 官方视频 | [YvQ4BVTWfHI](https://www.youtube.com/watch?v=YvQ4BVTWfHI)，2026-10-21 上传，改名为「Is a Faster Model Actually Faster? Test the Whole Job — Morgan Suo, MiniMax」。正文 [00:12]–[17:35]，**没有 Q&A**。自动字幕是正文主要依据，时间标「视频 mm:ss」。视频描述带章节（见下）。ai.engineer 的[讲座页](https://ai.engineer/talks/YvQ4BVTWfHI)是视频内容的摘要，没有新信息 |
| slide | 作者拍的 13 张（IMG_0576–0580、0582–0589；0581 不在文件夹里），全部属于本场，没有别场的照片。0577/0578、0579/0580 是同一页拍了两次。**开场、研究问题（PCB）那几页没拍到**，照片从「Cash flow behind the growth story」开始。数字都从照片读过 |
| 一手资料 | MiniMax 官方：[API 文档（OpenAI 兼容接口）](https://platform.minimax.io/docs/api-reference/text-openai-api)、[Hugging Face 模型卡 MiniMaxAI/MiniMax-M3.1-Flash](https://huggingface.co/MiniMaxAI/MiniMax-M3.1-Flash)、[MiniMax Agent 的 M3.1 Flash 页](https://agent.minimax.io/tools/m3-1-flash-preview)。第三方但技术细节最全：[vLLM 博客 Day-0 Support for MiniMax-M3.1-Flash](https://vllm.ai/blog/2026-10-23-minimax-m31-flash)（Inferact and the vLLM Team，2026-10-23） |

视频章节（视频描述原文）：00:00 Intro · 01:44 The research job · 03:20 Profit vs. cash flow · 04:22 What changed inside the model · 05:09 Sparse attention on long inputs · 07:53 4-bit KV cache · 08:59 Low-precision experts · 10:20 Speculative decoding · 11:39 Throughput vs. wait time · 13:09 Reasoning budgets · 14:41 How to evaluate a model switch · 16:12 Recap

## 基本信息

- 时间地点：2026-10-14 10:40–10:58am，McCarthy Room (Riverside Suite)，Breakout Session (18 mins)，Curated Track
- 讲者：Morgan Suo，MiniMax Head of US Business Development（日程）。讲者说（视频 00:18）：在 MiniMax「almost three years」，刚进美国市场时「almost no one knows us」，当时有第一个视频生成模型
- 讲者说（视频 00:38）：「our latest language model which will be released next week」（讲于 10-14）。资料：模型的发布日期，二手资料说是 2026-10-19，vLLM 的 day-0 支持博客是 10-23，MiniMax 官方页没写日期，【待确认】
- 题目：日程「The Hidden Costs of a Faster Model」，视频「Is a Faster Model Actually Faster? Test the Whole Job」
- 讲者说（视频 01:01–01:33）：新模型来了，「It can generate a task faster but … should I switch to that model?」报告很快回来，「but it misses a very important source. So someone has to find that source and fix that report … So did the job really get faster with this faster model?」

## 1. 研究任务：AI 服务器需求，PCB 供应商赚到现金了吗

- 讲者说（视频 02:06–03:20）：今天只讲 M3.1 Flash 和一个研究任务。分析师的问题：「as AI server demand is growing, are the companies supplying the circuit boards turning this growth into cash?」（PCB = printed circuit board）。「The internal review describes work across 14 company reports」，模型要找对报告、对齐 reporting period、把不同业务分开，再解释数字的含义。分析师要能「open the source behind an important claim … check a calculation … see what evidence could change the conclusion」，「that is our standard for a finished research job」（这几页 slide 没拍到）
- slide（IMG_0576）「FINANCIAL RESEARCH / INTERNAL REVIEW — **Cash flow behind the growth story**」，「M3.1 Flash checkpoint 0918-e3 · September 2026 · qualitative internal review」
  - 「Finding highlighted in the internal review」：**Profit growth ≠ Cash conversion**
  - 「Illustrative example: Profit can rise while cash remains tied up in receivables and inventory.」
  - 「Verification: reconcile cash from operations with the income statement and working capital.」
  - 底部：「**14 interim reports · 22 source URLs · 5 testable theses**」
- 讲者说（视频 03:26–04:21）：「profit can grow but cash flow tells a different story」，模型要指向现金流量表和相关披露；「a report earns trust when its claims can be checked」。这个 review 讲的是研究产出；后面的技术测试是「separate tasks」，用来理解具体的 trade-off

## 2. M3.1 Flash 改了什么（总表）

- slide（IMG_0577–0578）「**What changed in M3.1 Flash**」：「Attention and precision change the engine; serving and effort determine the research budget.」

| Component | Previous configuration | M3.1 Flash configuration |
| --- | --- | --- |
| First three attention layers | Full attention | Sparse attention |
| Sparse indexer and main attention | Q8KV8 | Q8KV4 |
| Routed MoE experts | MXFP8 | W4A4 NVFP4 |
| Reasoning controls | M3 can disable thinking | Always on, default effort max |

  底部（左侧被挡）：「…source pack; change the configuration, then compare completed work.」
- 我的理解：「Previous configuration」指 MiniMax M3（第四行写明「M3 can disable thinking」）
- 讲者说（视频 04:28–05:09）：四个问题：哪里能减少 attention 的计算；数据怎么存、怎么搬；serving 配置怎么影响用户等待；模型推理多少。「every optimization has to help the finished job」
- 资料核对（逐行）：
  - 前三层：HF 模型卡「the first three full-attention layers are replaced with sparse attention」，模型「uses MiniMax Sparse Attention (MSA) throughout the model」
  - KV：vLLM 博客「The KV cache is stored in NVFP4 while the query stays in FP8」，覆盖「both the indexer and the attention path」，模型「natively quantization-aware trained for an NVFP4 KV cache」；每 token 34,560 bytes，FP8 是 61,440 bytes
  - MoE：vLLM 博客「its routed MoE weights are stored in NVFP4」，checkpoint 从 451.5 GB（MiniMax-M3-MXFP8）降到 250.1 GB；shared experts 仍是 MXFP8（「fused MXFP8 shared experts」）。HF 模型卡说 quantization-aware training 覆盖「the routed MoE experts and the attention paths」。「W4A4」（权重和激活都是 4-bit）只见于 slide，资料里没写，【待确认】
  - Reasoning：见第 7 节，官方 API 文档完全一致

## 3. Sparse attention：少算 attention，但要找到那条脚注

- slide（IMG_0579–0580）「MINIMAX SPARSE ATTENTION — **How sparse attention works**」：「A lightweight indexer selects blocks for the main attention calculation.」
  - **Index branch**：Scores candidate blocks for each query. Example: locate the cash-flow footnote.
  - **Main branch**：Runs attention on selected blocks. Local context remains included.
  - 图：Query Group 1 / Query Group 2 各自的 block 选择 mask（Selected / Non-selected，Query → Key Block），汇总成 Attention Mask
  - 底部：「• P…d MSA: **16 × 128 = 2,048 selected tokens**」「…tokens ≠ the full context window.」（第一个词被挡，【待确认】）
- 讲者说（视频 05:15–06:17）：「every model says oh I have a very big context window like we said we have 1 million」，关键是在这 100 万里能不能又快又对地找到。sparse attention 先用一个很小的选择步骤给文本块打分，main attention 只算选中的块，「also keeps the nearby text in view」；选择随 query 变化
- 讲者说（视频 06:18–06:53）：目标是减少多个文档上的 attention 计算，但「a very short footnote there may change the entire conclusion. So for a cash flow question, the footnote may matter more than the pages of industry outlook」；「less attention work still has to find the important evidence」
- 资料（vLLM 博客）：KV cache 按 128 token 分块，indexer 每个 GQA group 用一个 index query 给所有 key 打分、max-pool 成每块一个分数，attention 只算 top 16 块，外加始终保留的最近一块，「This caps each query at 2,048 keys」。和 slide 的 16 × 128 = 2,048 一致
- 资料：MSA 论文 arXiv:2606.13392（HF 模型卡引用，2026-06-11），我没有读论文正文

**长上下文检索测试**

- slide（IMG_0582）「…ATTENTION COVERAGE — **Sparse attention in three more layers**」：「[M3].1 Flash changes the first three layers from full attention to sparse attention.」左：「[Fi]nd the decisive footnote」「Find the footnote that changes the thesis. [Test a]cross input lengths and positions.」右「**M3.1 Flash long-context retrieval**」：

| Input tokens | Single, 5 positions | 4-fact sum |
| --- | --- | --- |
| 7.6–7.9K | 5/5 | — |
| 27.1–32.7K | 5/5 | — |
| 112.9–119.8K | 5/5 | 3/3 |
| 233.8–236.6K | 5/5 | — |
| 465.3–497.7K | 5/5 | 3/3 |
| 817.0–830.5K | 5/5 | 3/3（照片右下角被挡，按总数推算） |

  底部：「[Ch]eck: retrieve the decisive filing. **Separate synthetic retrieval test: 39/39 pa[ss]**」
- 我核对了总数：6 × 5 + 3 × 3 = 39，和 39/39 一致，所以最后一格应是 3/3
- 讲者说（视频 06:55–07:23）：「M3.1 Flash also uses … this sparse attention in its first three layers that makes this retrieval an important check」；「in [one] run the model passes all 39 requests and the longest inputs were about 830k tokens」；「the result covers the cases in this test」
- 讲者说（视频 07:26–07:53）：对真实研究任务还要问：能找到现金流脚注吗？用对 reporting period 吗？能发现不同文档之间的矛盾吗？把关键段落挪到输入的不同位置再问一遍

## 4. 4-bit KV cache：平均分没变，单项掉了

- slide（IMG_0583）「ATTENTION PRECISION — **Four-bit KV: check the workload behind the average**」：「M3.1 Flash: Q8KV8 → Q8KV4 in the sparse indexer and main attention branch.」
  - 左「Smaller values, block scales」：「• 8-bit queries; 4-bit K/V.」「…per 16 values.」红字（左侧被挡）：「…[regres]sions can hide behind a stable average.」「…[−6].6 points; RISE-Fix1 +5.25 points.」
  - 右「**M3.1 Flash DSpark configurations**」，「Internal engineering evaluation · September 21」：

| Evaluation | Q8KV8 | Q8KV4 | 变化（我算的） |
| --- | --- | --- | --- |
| Macro average | 61.72 | 61.83 | +0.11 |
| GPQA-Diamond | 79.3 | 80.8 | +1.5 |
| SciCode | 48.6 | 42.0 | **−6.6** |
| RISE-Fix1 | 34.0 | 39.25 | +5.25 |

- 讲者说（视频 07:58–08:47）：「the model keeps the internal information from earlier text which we call this KV cache. And M3.1 Flash changes its keys and values from eight bits to four bits, but the query still stays at eight bits」；「the average scores actually is almost the same but the [SciCode] score there it drops like 6.6 percentage」，「the average score actually can hide a mistake … your user cares about」；这张表比的是两套完整配置，「does not tell us which changes caused each score」
- 讲者说（视频 08:50）：对研究任务要重新检查输出：来源对不对、计算对不对、解释和证据是否一致
- 资料：「per 16 values」的 block scale 和 NVFP4 的格式一致（NVFP4 每 16 个值共享一个 scale；我的理解，来自 NVIDIA 对 NVFP4 的公开说明，这里没有附原文链接，【待确认】）
- 资料：表名里的「DSpark」是 M3.1 Flash 的 speculative decoding drafter（HF 模型卡：「DSpark replaces the earlier EAGLE-like MTP approach」），说明这组 KV 对比是在开了 DSpark 的配置下测的（我的理解）

## 5. Routed experts 改成 NVFP4 W4A4：scale 在哪一步恢复很重要

- slide（IMG_0584）「ROUTED MOE EXPERTS — **Routed experts move to NVFP4 W4A4**」：「M3.1 Flash: MXFP8 → four-bit weights and activations. The shared expert stays unchanged.」
  - 左「Efficiency and implementation cost」：「[Less we]ight traffic; lower precision.」「[Scaling and] restoration still add work.」（左侧被挡）
  - 右「Scale restoration changes the result」：

| Path | Activation-scale handling |
| --- | --- |
| FC1 | Dynamic per row. Restore after GEMM, before nonlinear activation |
| FC2 | Fixed encoding factor: 16. Restore with × 1/16 after GEMM |

  - 底部：「[Ch]eck: verify derived numbers after changing the numerical path; the[n …]」（右侧被挡）
- 讲者说（视频 09:02–10:19）：MoE「selects some of them for each token. In M3.1 Flash, these selected experts use four bit [weights] and activations. The shared expert is outside this change」；weights 是存储的模型参数，activations 是模型算出来的值；低精度减少数据搬运、支持更高效的计算；但计算有几步：「we scale the numbers, we run the calculation and restore the scale. The position of these steps really matters」，「getting that order right is the part of getting the answer right. So fast math still has to be correct」；要测「the complete implementation」
- 资料：见第 2 节（vLLM 博客：routed MoE 权重 NVFP4，shared experts MXFP8）。FC1/FC2 的 scale 处理细节资料里没有，只见于 slide

## 6. Speculative decoding 和并发：吞吐翻倍，p95 首 token 等待 7.9 倍

**Speculative decoding**

- slide（IMG_0585）「SPECULATIVE DECODING — **Draft faster. Verify. Measure accepted output.**」：「Drafting and target verification both contribute to the cost of a completed answer.」三步：Draft（P[ropose a t]oken sequence）→ Verify（Target-model scoring）→ Accept / correct（Keep prefix; then co[ntinue]）。底部红字：「…[accep]ted tokens and end-to-end time. Financial facts still need separate checks.」
- 讲者说（视频 10:30–11:38）：「a faster draft process … propose a short sequence of tokens and then the target model can check it and the system keeps this first part that the target accepts and makes a correction when it's needed」；「the opportunity here is to get several tokens from one target check … but drafting takes time, checking takes time, rejecting these tokens also takes time. So count the time spent checking the draft too」；这些 check 验的是 token，「financial claims and sources need their own checks」；最终指标是「the time to a report that an analyst can really use」，要算上检索、tool calls 和 retries
- 资料：M3.1 Flash 的 drafter 叫 **DSpark**，取代之前「EAGLE-like MTP」，发布时附带 draft head（HF 模型卡）；vLLM 的 NVIDIA 示例配置用 7 个 speculative tokens，AMD 示例用 3 个（vLLM 博客）。讲者台上没说 DSpark 这个名字（只出现在第 4 节表名里）

**并发负载测试**

- slide（IMG_0586）「SERVING CAPACITY — **1.9x throughput. 7.9x p95 first-token wait.**」：「M3.1 Flash load test: simultaneous story-generation requests at low reasoning effort.」

| Concurrent requests | Aggregate output tokens/s | Median first-token wait | p95 first-token wait |
| --- | --- | --- | --- |
| 32 | 1,092 | 0.99 s | 1.46 s |
| 64 | 2,043 | 1.03 s | **11.59 s** |
| 128 | 4,138 | 11.29 s | 11.71 s |

  底部：「32 → 64 requests: 1.9x throughput; 7.9x p95 wait.」「…[G]PU does not remove queueing.」「…[p9]5 time-to-usable-memo target, then test throughput at that limit.」（左侧被挡）
- 我核对了：2,043 / 1,092 = 1.87；11.59 / 1.46 = 7.94，和标题一致
- 讲者说（视频 11:47–12:58）：这是另一个「separate serving test」，story generation、low reasoning effort，并发从 32 升到 64，「total output per second grows about 1.9 times」，但「the P95 wait for the first token … goes from about 1.5 seconds to 11.[6] seconds」，「eight times longer」；「the median stay[s] … we cannot only look at the median time」；「even when the throughput you see like oh we have [throughput] like more than 300 per second but still you need to care about the final wait time」（「300」和 slide 对不上，见「出入」）
- 讲者说（视频 12:58–13:09）：首 token 可能只是 reasoning，还要量「when the usable report arrives」；「set a deadline for that report, [then] we choose a [load] that meets it」
- 我的理解：128 并发时 median 也跳到 11.29 s，说明 64 并发时只有尾部请求排队，128 时大多数请求都在排队；台上没讲 128 这一行

## 7. Reasoning controls：不填 effort 就是 max

- slide（IMG_0587）「REASONING CONTROLS — **Default max changes the work you request**」：「[M]3.1 Flash requires adaptive thinking. Missing effort becomes max.」
  - 左「What actually reaches the model」：Effort omitted → max；Reasoning remains on；Record the effective route and prompt
  - 右「**M3.1 Flash reasoning evaluation · matched items**」：

| Evaluation | High | Max |
| --- | --- | --- |
| MMLU-Pro accuracy | 76.9% | 78.5% |
| Mean output tokens | 1,005 | 1,851 |
| LiveCodeBench accuracy | **68.0%** | **80.0%** |
| Mean output tokens | 7,703 | 10,443 |

  「Output tokens include reasoning.」底部红字：「…[se]t by evidence quality. **LCB high used fewer tokens but lost 12 accuracy points.**」
- 讲者说（视频 13:14–14:37）：「this is the default max changes … it can control how many tokens we use for this model thinking … thinking stays on but if we leave the effort blank the default is max. So a default can change the amount of work we request」；「one coding test there shows the tradeoff … the high uses fewer output tokens than the max but the accuracy actually fell from 80% to 68%」；「give this task the thinking budget it really needs」；同一批报告上比较不同设置，「the quick first pass [and] the report ready for review may need different budgets」，要记录用了哪个 reasoning 设置
- 资料（[MiniMax API 文档](https://platform.minimax.io/docs/api-reference/text-openai-api)）：`reasoning_effort` 仅 M3.1-Flash 支持，取值 `low`、`medium`、`high`、`xhigh`、`max`，「**Defaults to `max` when omitted**」，`none` 不支持、返回 400；thinking 的 `type` 可以是 `disabled` 或 `adaptive`，「`MiniMax-M3.1-Flash` always thinks and returns `400` if `disabled` is sent」，报错信息「model "MiniMax-M3.1-Flash" requires adaptive thinking」；MiniMax-M3 发 `disabled` 则「Skips thinking and answers directly」。**和 slide 的「requires adaptive thinking」「Missing effort becomes max」「M3 can disable thinking」完全一致**
- 资料（[MiniMax Agent 页](https://agent.minimax.io/tools/m3-1-flash-preview)）：「Reasoning is always on in M3.1 Flash and cannot be disabled」，五档 effort，「Max as the default recommendation」

- slide（IMG_0588）「RESEARCH WORKFLOW / RELEASE DECISION — **Would you ship this research workflow?**」：「Evaluate the complete serving configuration on the same task, sources and deadline.」三项：
  - **Evidence & numbers**：Audit citations and calculations.
  - **Cost & latency**：Cost per acceptable memo.
  - **Reproducibility**：Pin versions; replay; roll bac[k].
  - 底部：「…[chec]kpoint, runtime, prompt, effort and sources. Canary; compare; roll back.」
- 讲者说（视频 14:41–16:11）：先固定问题和源报告，用同样的 information cutoff，事先约定什么样的报告算合格，再比较。然后查三件事：
  1. 分析师能不能信这份报告：能打开重要结论背后的来源、核对 reporting period、重算财务数字、找可能推翻结论的证据
  2. 完成的工作花了多少、用了多久：「include retrieval, tool c[alls] and retries. So if a person must fix the report before use, count that work too … include all the failed attempts in the cost of producing … acceptable reports」；按预期用户数测试，检查报告是否在承诺时间内到达，「measure the cost of [a] report you can actually use」
  3. 能不能复现：保存模型和 runtime 版本、prompt 设置和来源，保留失败样例，「start with a small roll out and keep a way to switch back」
- slide（IMG_0589，收尾页）：「**Measure the cost of delivering an acceptable result at the required latency.**」「Traceable evidence. Correct calculations. A deadline the user can accept.」署名「[Morgan] Suo」「…@minimax.io」
- 讲者说（视频 16:12–16:48）：放了一段用 MiniMax 视频模型生成的回顾短片（字幕和录音都是「A3」），说「it's open[-weight]」「it's only a one prompt with the text to video」。资料：MiniMax 现有的视频模型是 H3（Hailuo 3，2026-07-31 发布，二手资料称开放了权重，如 [fal.ai 介绍](https://fal.ai/learn/tools/minimax-h3-explained)）；**「A3」可能是「H3」的 ASR 错误，【待确认】**
- 讲者说（视频 16:51–17:35）：回到开头问题，对分析师来说完成的工作是「a report with [sources] they can open and it has numbers they can check, it must arrive when they need it」；「measure what [it] cost[s] to finish [the] job and on time」；结尾：「**faster models really matter but what matter[s] most is what we can do with them**」

## Q&A 精确版

无。视频在 [17:35]「Thank you guys」后结束；录音最后一句是「if you want to interact you can start it」（指什么不清楚，【待确认】），之后没有问答。

## 勘误表（Whisper / 自动字幕 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| Minax / Miniax | MiniMax | slide |
| m3.1 flat / every point one flash | M3.1 Flash | slide |
| glass attention / this bar's attention | sparse attention | slide |
| passenger passages / tax | passages / text | 上下文 |
| in v run | in one run（台上数字见 slide：39/39） | 上下文，【待确认】 |
| kd cache | KV cache | slide |
| sign score | SciCode score | slide |
| MO ine architecture / mo in moe | MoE architecture | slide |
| four big weights / four bit ways | 4-bit weights | slide |
| speculative coding | speculative decoding | slide |
| parking model / targeting model | target model | slide |
| tool halls / tool costs | tool calls | 上下文 |
| storage generation | story generation | slide |
| 105 seconds（录音） / 1.5 seconds | 1.46 s | slide |
| 11.3 seconds … 6 seconds | 11.59 s | slide |
| weight（time） | wait | slide |
| medium state | median | slide |
| dial line / guideline | deadline | 上下文 |
| choose a low that miss it | choose a load that meets it | 上下文、slide（我的理解） |
| default master / default max changes | Default max changes the work you request | slide |
| awkward tokens / up output tokens | output tokens | slide |
| populations | calculations | 视频字幕 |
| information code | information cutoff | 视频字幕 |
| retracts | retries | 视频字幕 |
| short report / recap（片子） | recap | 视频字幕 |
| A3（视频模型） | 可能是 H3，【待确认】 | 资料 |
| opened way / open with | open-weight | 上下文 |
| 14 company reports | 14 interim reports | slide |

## 出入

1. **p95 首 token 等待**：slide 是 1.46 s → 11.59 s（7.9x）；讲者口头「about 1.5 seconds to 11.[3/6] seconds」「eight times」。以 slide 为准
2. **「more than 300 per second」**：讲者说「we have throughput like more than 300 per second」（视频 12:52），slide 上的总吞吐是 1,092 / 2,043 / 4,138 tokens/s，没有 300 这个数。每请求吞吐约 34（1,092 / 32）和 32（2,043 / 64）tokens/s 也不是 300。【待确认】讲者指的是什么
3. **报告数量的说法**：讲者说「14 company reports」，slide 是「14 interim reports · 22 source URLs · 5 testable theses」
4. **W4A4**：slide 写 routed experts 是「W4A4 NVFP4」（权重和激活都 4-bit），讲者也说「4-bit weights and activations」；vLLM 博客和 HF 模型卡只说 routed MoE weights 是 NVFP4，没提激活精度
5. **KV 的精度叫法**：slide 用 Q8KV4（8-bit query、4-bit KV）；vLLM 博客的说法是 KV cache NVFP4、query FP8。两者一致，只是叫法不同（我的理解）
6. **发布时间**：讲者 10-14 说「will be released next week」；二手资料说 10-19 发布，vLLM day-0 博客是 10-23。官方日期没找到
7. **简介和正文**：简介写「blockwise 4-bit attention and MoE quantization change scale selection and rounding」「how reasoning controls can alter the path from API request to prompt construction」，台上都讲到了；简介里「observability」一词台上只体现为「record the effective route and prompt」「pin versions; replay」

## 【待确认】汇总

1. M3.1 Flash 的官方发布日期
2. slide 的「W4A4」（激活也是 4-bit）在官方资料里的出处
3. IMG_0580 底部「P…d MSA: 16 × 128 = 2,048 selected tokens」第一个词
4. NVFP4「每 16 个值一个 scale」的一手出处链接（本文没附）
5. 「in one run … 39 requests」里 ASR「v run」的原话
6. 讲者「more than 300 per second」指的是什么
7. 回顾短片用的视频模型是「A3」还是 H3
8. 录音末句「if you want to interact you can start it」指什么
