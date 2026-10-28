[English](../10-14-1040-the-hidden-costs-of-a-faster-model.md) | **中文**

# 模型更快，只有整件活都更快才算数

*在 AI Engineer NYC 2026 上，MiniMax 的 Morgan Suo 拆解了公司新模型 M3.1 Flash 里的各项提速手段，从 sparse attention 到 4-bit 运算，并主张每一项都要拿一个完成的、可核查的结果所花的成本和时间来衡量。*

**Breakout · 2026 年 10 月 14 日（周三）· 10:40–10:58am · McCarthy Room (Riverside Suite)，Curated track**
**讲者：** Morgan Suo，Head of US Business Development，MiniMax

---

「新模型出来的时候，」Morgan Suo 说，「它生成任务更快，但……我该换过去吗？」Suo 描述了一个常见的结局：一份研究报告很快就回来了，「但漏了一个很重要的来源。于是得有人去找到那个来源，修好那份报告……那么用了这个更快的模型，这活儿真的变快了吗？」

Suo 在 MiniMax 快三年了，回忆说公司刚进入美国市场时，「几乎没人认识我们」；那时公司只有第一个视频生成模型。这场 talk 讲的是公司最新的语言模型 M3.1 Flash，Suo 说它将在下周发布。计划是先看 MiniMax 怎么减少模型的计算量，再问怎样测试这些改动值不值得。（官方视频改了标题，叫 "Is a Faster Model Actually Faster? Test the Whole Job."）

## 1. 研究任务：增长有没有变成现金？

Suo 说，一条快速的聊天回复和一份研究报告需求不同，所以「任务决定了什么叫好的表现」。整场 talk 都用同一个研究任务。分析师的问题是：「AI 服务器需求在增长，给它们供应电路板的公司，有没有把这份增长变成现金？」（PCB，即 printed circuit board（印刷电路板），用来连接 AI 服务器等设备内部的电子元件。）

模型要做几件事：找到对的报告，对齐同样的报告期，把不同业务分开，并解释数字的含义。拿到报告的分析师需要「打开某个重要论断背后的来源」，核对一个计算，并「看到什么证据可能改变结论」。「这就是我们对一个完成的研究任务的标准，」Suo 说。（介绍这个任务的幻灯片没拍到。）

![Cash flow behind the growth story](../../slides/10-14-1040-the-hidden-costs-of-a-faster-model/01.jpg)
*对 M3.1 Flash 的一个 checkpoint 在电路板研究任务上的定性内部评审。*

幻灯片描述的是对 M3.1 Flash checkpoint 0918-e3（2026 年 9 月）的一次「定性内部评审」。它突出的发现是："Profit growth ≠ Cash conversion."（利润增长 ≠ 现金转化。）举的例子是：「利润可以上涨，而现金还压在应收账款和库存里。」验证步骤是：「把经营现金流和利润表、营运资本对上。」资料包是 "14 interim reports · 22 source URLs · 5 testable theses"。（Suo 在台上说的是「14 份公司报告」。）

「利润可以增长，但现金流讲的是另一个故事，」Suo 说。一家公司卖得更多、报出更高的利润，但客户还没付钱，或者买来的原材料还堆在仓库里。所以模型应该指向现金流量表和相关披露：「一份报告的论断能被核查，它才赢得信任。」这次评审描述的是研究产出；talk 后面的技术测试是另外的任务，用来理解具体的取舍。

## 2. M3.1 Flash 里改了什么

![What changed in M3.1 Flash](../../slides/10-14-1040-the-hidden-costs-of-a-faster-model/03.jpg)
*「attention 和精度改变的是引擎；serving 和 effort 决定研究预算。」*

幻灯片把改动和上一个配置 MiniMax M3 做了对比：

| 组件 | 之前的配置 | M3.1 Flash 的配置 |
| --- | --- | --- |
| 前三层 attention | Full attention | Sparse attention |
| Sparse indexer 和主 attention | Q8KV8 | Q8KV4 |
| Routed MoE experts | MXFP8 | W4A4 NVFP4 |
| 推理控制 | M3 可以关掉 thinking | 始终开启，默认 effort max |

被挡住一部分的页脚结尾是："…source pack; change the configuration, then compare completed work."（……资料包；改配置，然后比较完成的工作。）

Suo 把这些改动对应到四个问题：哪里可以减少 attention 的工作量，数据怎么存储和搬运，serving 配置怎样影响用户的等待时间，以及模型做多少推理。「每一项优化都得对完成的任务有帮助。」

MiniMax 的 model card 和 vLLM 的首日支持博客证实了表里的大部分内容：前三层 full attention 换成了 sparse attention；在 indexer 和 attention 路径里，KV cache 都以 NVFP4 存储，query 保持 FP8；routed MoE 权重以 NVFP4 存储，让 checkpoint 从 451.5 GB 缩到 250.1 GB，而 shared experts 保持 MXFP8。「W4A4」这个标签，意思是 activation 和权重都是 4 bit，只出现在幻灯片和 talk 里；公开来源只提到了权重。

## 3. Sparse attention：活少了，但还得找到那条脚注

![How sparse attention works](../../slides/10-14-1040-the-hidden-costs-of-a-faster-model/05.jpg)
*一个轻量的 indexer 挑出 block；主 attention 只在这些 block 上跑。*

「现在我觉得每个模型都说，哦，我的 context window 很大，比如我们说我们有 100 万，」Suo 说。问题在于模型能不能在里面又快又准地找到正确结果。

幻灯片画了两条分支。index 分支「给每个 query 的候选 block 打分。例子：定位现金流脚注」。主分支「在选中的 block 上跑 attention。局部上下文仍然包含在内」。一张图展示了两组 query，各自有一套选中和未选中的 key block 模式，合成一个 attention mask。底部一行给出预算："16 × 128 = 2,048 selected tokens"，并注明这不是整个 context window。选择随每个 query 而变，因为不同的问题需要不同的段落。（vLLM 的博客描述了同样的机制：KV cache 切成 128 token 的 block，由 indexer 打分，attention 在得分最高的 16 个 block 加上最近的一个 block 上运行，每个 query 最多 2,048 个 key。）

对这个研究任务来说，目的是减少跨多份文档的 attention 工作量。但「那里一条很短的脚注可能改变整个结论，」Suo 说，「对一个现金流问题，脚注可能比好几页行业展望更重要。」"Less attention work still has to find the important evidence."（attention 的活少了，也还得找到重要的证据。）

![Sparse attention in three more layers](../../slides/10-14-1040-the-hidden-costs-of-a-faster-model/06.jpg)
*一项跨不同输入长度和位置的检索测试，最长约 830,000 token。*

因为 M3.1 Flash 现在前三层也用 sparse attention，检索就成了一项重要检查。标题为 "Find the decisive footnote" 的幻灯片报告了另一项合成测试，把事实放在长输入的不同位置：

| 输入 token 数 | 单个事实，5 个位置 | 4 个事实求和 |
| --- | --- | --- |
| 7.6–7.9K | 5/5 | — |
| 27.1–32.7K | 5/5 | — |
| 112.9–119.8K | 5/5 | 3/3 |
| 233.8–236.6K | 5/5 | — |
| 465.3–497.7K | 5/5 | 3/3 |
| 817.0–830.5K | 5/5 | 3/3 |

最后一格在照片里被挡住了，但幻灯片上的总计 "39/39 pass"（30 次单事实请求加 9 次 4 事实请求）意味着是 3/3。Suo 说模型通过了全部 39 次请求，最长输入约 830,000 token，但「这个结果只覆盖这项测试里的情况」。对真实报告，问题不一样：模型能不能找到现金流脚注，用对报告期，发现文档之间的冲突？Suo 的建议是把重要段落挪到输入里的不同位置，再问同一个问题。

## 4. 4-bit KV cache：平均分稳住了，也可能藏着下滑

![Four-bit KV: check the workload behind the average](../../slides/10-14-1040-the-hidden-costs-of-a-faster-model/07.jpg)
*key 和 value 从 8 bit 降到 4 bit，平均分几乎没动，但有一个 benchmark 掉了 6.6 分。*

模型把前文的内部信息保存在所谓的 KV cache 里。M3.1 Flash「把 key 和 value 从 8 bit 改成 4 bit，但 query 还是 8 bit，」Suo 说。这减少了要存储和搬运的数据。幻灯片注明：8-bit query、4-bit key 和 value，每 16 个值一个 block scale。表格来自 9 月 21 日的一次内部工程评测，开启了 speculative decoding（DSpark，M3.1 Flash 的 draft 模型）：

| 评测 | Q8KV8 | Q8KV4 | 变化 |
| --- | --- | --- | --- |
| 宏平均 | 61.72 | 61.83 | +0.11 |
| GPQA-Diamond | 79.3 | 80.8 | +1.5 |
| SciCode | 48.6 | 42.0 | −6.6 |
| RISE-Fix1 | 34.0 | 39.25 | +5.25 |

幻灯片用红字警告：退化「can hide behind a stable average」（可能藏在稳定的平均分后面）。「平均分其实几乎一样，但那边的 SciCode 分数掉了大概 6.6 个百分点，」Suo 说，而其他分数在涨。「平均分其实可能藏着一个……你的用户在意的错误。」Suo 补充说，这张表比较的是两套完整配置，所以「它不能告诉我们每个分数是哪项改动造成的」。对研究任务来说，这意味着要再检查一遍输出：来源对不对，计算对不对，解释和证据对不对得上？

## 5. 4-bit experts：scale 在哪里还原很关键

![Routed experts move to NVFP4 W4A4](../../slides/10-14-1040-the-hidden-costs-of-a-faster-model/08.jpg)
*routed experts 改用 4-bit 权重和 activation；shared expert 不变。*

M3.1 Flash 是 mixture-of-experts 模型：它有一些叫 expert 的计算部件，每个 token 选其中一部分来用。「在 M3.1 Flash 里，这些被选中的 expert 用 4-bit 的权重和 activation。shared expert 不在这次改动之内，」Suo 说。权重是模型存储的值；activation 是模型算出来的值。降低精度减少了数据搬运，也支持更高效的计算，幻灯片也注明了代价的一面："Scaling and restoration still add work."（缩放和还原仍然会增加工作量。）

但计算分好几步：「我们缩放数字，跑计算，再把 scale 还原。这些步骤的位置真的很重要。」标题为 "Scale restoration changes the result" 的幻灯片表格说明了位置：

| 路径 | Activation scale 的处理 |
| --- | --- |
| FC1 | 按行动态。在 GEMM 之后、非线性 activation 之前还原 |
| FC2 | 固定编码因子：16。在 GEMM 之后乘 1/16 还原 |

「把这个顺序弄对，是把答案弄对的一部分。所以快的运算也还得是对的，」Suo 说。测试要覆盖完整的实现；对研究任务来说，就是把同样的问题再跑一遍，看答案为什么仍然过不了检查。幻灯片的红字是："verify derived numbers after changing the numerical path."（改了数值路径之后，要核对推导出来的数字。）

## 6. Speculative decoding：检查的时间也要算进去

![Draft faster. Verify. Measure accepted output.](../../slides/10-14-1040-the-hidden-costs-of-a-faster-model/09.jpg)
*起草、验证、接受或纠正：起草和验证都要花时间。*

从模型本身转到它怎么生成答案，Suo 讲了 speculative decoding。一个更快的 draft 过程先提出一小串 token，「然后 target 模型来检查，系统保留 target 接受的前一部分，需要时做纠正」。幻灯片展示了三步：draft、verify（target 模型打分）、accept or correct（保留前缀，然后继续）。

「这里的机会是一次 target 检查拿到好几个 token，」Suo 说，「但起草要时间，检查要时间，拒掉这些 token 也要时间。所以检查 draft 的时间也要算进去。」幻灯片说的是同一件事："Drafting and target verification both contribute to the cost of a completed answer."（起草和 target 验证都计入一个完成答案的成本。）这些检查验证的是 token，但「财务论断和来源需要它们自己的检查」。对研究任务来说，衡量还应该包括来源检索、tool call 和重试。「最终的衡量是拿到一份分析师真正能用的报告所需的时间。」

## 7. 吞吐量几乎翻倍；最慢的首 token 等待长了 7.9 倍

![1.9x throughput. 7.9x p95 first-token wait.](../../slides/10-14-1040-the-hidden-costs-of-a-faster-model/10.jpg)
*一项低推理 effort 下并发生成故事请求的压测。*

即使单个请求跑得很好，用户一多，体验就变了。另一项 serving 测试在低推理 effort 下并发跑生成故事的请求：

| 并发请求数 | 总输出 tokens/s | 首 token 等待中位数 | 首 token 等待 p95 |
| --- | --- | --- | --- |
| 32 | 1,092 | 0.99 s | 1.46 s |
| 64 | 2,043 | 1.03 s | 11.59 s |
| 128 | 4,138 | 11.29 s | 11.71 s |

从 32 个请求到 64 个，每秒总输出增长约 1.9 倍，但第 95 百分位的首 token 等待从约 1.5 秒变成约 11.6 秒，「长了八倍」，Suo 说。（幻灯片给的是 1.46 s 到 11.59 s，也就是 7.9 倍。）中位数几乎没动，「但我们不能只看中位数」。用户感受得到等待。（Suo 还提到「每秒 300 多」的吞吐量，这个数和幻灯片上的任何数字都对不上。）talk 没有讨论 128 个请求那一行，那里中位数等待也跳到了 11.29 秒。

被挡住一部分的页脚说 GPU「does not remove queueing」（并不能消除排队），建议先设一个 p95 的「time-to-usable-memo target」（拿到可用报告的时间目标），「then test throughput at that limit」（再在这个限度上测吞吐量）。Suo 说，首个 token 可能只是推理过程，所以关键是可用的报告什么时候到：先给报告定一个截止时间，再选一个能满足它的负载。

## 8. 推理控制：effort 留空，你拿到的就是 max

![Default max changes the work you request](../../slides/10-14-1040-the-hidden-costs-of-a-faster-model/11.jpg)
*在 M3.1 Flash 里，thinking 始终开启，省略的 effort 设置会变成 max。*

还有一个设置会改变模型被要求做的工作量。「thinking 一直开着，但如果 effort 留空，默认就是 max，」Suo 说，「所以一个默认值就能改变我们要求的工作量。」幻灯片说得很直白："M3.1 Flash requires adaptive thinking. Missing effort becomes max."（M3.1 Flash 必须开 adaptive thinking。不填 effort 就变成 max。）在 "What actually reaches the model" 下面列着：省略 effort 会变成 max；推理保持开启；记录实际生效的路由和 prompt。MiniMax 的 API 文档与此一致：`reasoning_effort` 接受 `low`、`medium`、`high`、`xhigh` 或 `max`，「省略时默认为 `max`」，而且如果关掉 thinking，M3.1 Flash 会返回错误，M3 则允许关掉。

幻灯片在配对样本上的评测展示了这个取舍（输出 token 包括推理）：

| 评测 | High | Max |
| --- | --- | --- |
| MMLU-Pro 准确率 | 76.9% | 78.5% |
| 平均输出 token | 1,005 | 1,851 |
| LiveCodeBench 准确率 | 68.0% | 80.0% |
| 平均输出 token | 7,703 | 10,443 |

在编程测试上，high effort「用的输出 token 比 max 少，但准确率从 80% 掉到了 68%，」Suo 说；幻灯片的说法是 "LCB high used fewer tokens but lost 12 accuracy points."（LCB 上 high 用的 token 更少，但准确率掉了 12 个点。）「给这个任务它真正需要的 thinking 预算。」对研究任务来说，这意味着在同样的报告上比较不同设置，承认快速的初稿和可以送审的报告可能需要不同的预算，并记录用的是哪个设置。

## 9. 这个研究工作流，你会上线吗？

![Would you ship this research workflow?](../../slides/10-14-1040-the-hidden-costs-of-a-faster-model/12.jpg)
*三项上线检查：证据和数字、成本和延迟、可复现性。*

幻灯片请观众「在同样的任务、来源和截止时间下，评估完整的 serving 配置」。Suo 的流程是：固定问题和来源报告，用同样的信息截止日期，在跑对比之前先就「什么样的报告算合格」达成一致。然后检查三件事。

第一，证据和数字（"Audit citations and calculations"）：分析师能不能信任这份报告，能不能打开重要论断背后的来源、核对报告期、重算财务数字、找出可能改变结论的证据？

第二，成本和延迟（"Cost per acceptable memo"）：完成的任务花了多少钱、用了多久？「把检索、tool call 和重试都算进去。如果报告在使用前必须有人修改，那部分工作也要算，」Suo 说，并把所有失败的尝试都计入产出合格报告的成本。在预期的用户数下测试，检查报告是否在承诺的时间内送达，「衡量一份你真正能用的报告的成本」。

第三，可复现性（"Pin versions; replay; roll back"）：保存模型和 runtime 版本、prompt 设置和来源；保留失败样例；「从小范围上线开始，并保留切回去的办法」。幻灯片的页脚列出了要固定的 checkpoint、runtime、prompt、effort 和来源，结尾是："Canary; compare; roll back."（灰度；对比；回滚。）

## 10. 衡量一个合格结果的成本

![Measure the cost of delivering an acceptable result at the required latency](../../slides/10-14-1040-the-hidden-costs-of-a-faster-model/13.jpg)
*收尾页："Traceable evidence. Correct calculations. A deadline the user can accept."*

结束前，Suo 播放了一段 talk 的简短回顾视频，是用 MiniMax 的 AI 视频模型从一条文字 prompt 生成的。然后 Suo 回到开场的问题：要不要换到更快的模型。对分析师来说，完成的任务是「一份带着他们能打开的来源、带着他们能核对的数字的报告；它必须在他们需要的时候到」。M3.1 Flash 改变了计算花在哪里，而完整的任务揭示这些改动对用户意味着什么。收尾页写着："Measure the cost of delivering an acceptable result at the required latency."（衡量在要求的延迟内交付一个合格结果的成本。）

「更快的模型确实重要，」Suo 说，「但最重要的是我们能用它们做成什么。」

---

**链接：** [日程](https://ai.engineer/nyc/2026/schedule?session=f3445fe7-308b-4e84-8490-fd78a13db4fb) · [视频](https://www.youtube.com/watch?v=YvQ4BVTWfHI) · [MiniMax API 文档（OpenAI 兼容）](https://platform.minimax.io/docs/api-reference/text-openai-api) · [MiniMax-M3.1-Flash model card](https://huggingface.co/MiniMaxAI/MiniMax-M3.1-Flash) · [MiniMax Agent: M3.1 Flash](https://agent.minimax.io/tools/m3-1-flash-preview) · [vLLM: Day-0 Support for MiniMax-M3.1-Flash](https://vllm.ai/blog/2026-10-23-minimax-m31-flash)

本文依据官方视频字幕（覆盖了作者现场录音漏掉的前六分钟）、现场录音、幻灯片照片和上面列出的来源写成；介绍研究问题的幻灯片没拍到。视频在 talk 结束后就停了，没有 Q&A。[精确笔记](../precise/10-14-1040-the-hidden-costs-of-a-faster-model.md)（中文）保留了时间戳，以及 talk、幻灯片和 MiniMax 文档之间所有出入的完整记录。
