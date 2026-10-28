[English](../10-14-1405-scaling-agentic-rl-across-the-globe.md) | **中文**

# 在跨云分布、型号不一的 GPU 上稳定做 agent RL 的配方

*在 AI Engineer NYC 2026 上，Modal 的 Jason Mancuso 认为，任何在生产环境跑着 agent 的团队，都已经具备 reinforcement learning 所需的东西。Mancuso 介绍了一套四部分的配方，让异步 RL 在 sampler 和 trainer 不一致时仍保持稳定，并解释了稀疏的权重更新如何让 inference GPU 可以放在训练集群之外。*

**Expo Session · 2026 年 10 月 14 日（周三）· 2:05–2:23pm · Shannon Room (NY Ballroom E)**
**讲者：** Jason Mancuso，Member of Technical Staff, Research，Modal

---

session 简介给出了前提：想掌控成本、质量、性能和可靠性的公司，正在对 open model 做 post-training 并部署，而「reinforcement learning 是 post-training 技术栈里最关键的部分」。简介还承诺会介绍 Stitch，也就是 Modal 的开源框架，用来把 RL rollout 分散到不同的云、区域和 GPU 型号上；以及「一套在 off-policy rollout 数据上对 agentic 模型做 post-training 的稳定配方」。

![A losing strategy once you compete on agent economics](../../slides/10-14-1405-scaling-agentic-rl-across-the-globe/02.jpg)
*27 张幻灯片中的第 3 张：「A losing strategy once you compete on agent economics.」（一旦在 agent 经济性上竞争，这就是必输的策略。）*

作者的现场录音从 27 张幻灯片中的第 3 张开始，上面写着「A losing strategy once you compete on agent economics」。前一张没有拍到，所以「必输的策略」具体指什么没有记录；从 Mancuso 一分钟后说的话推断，可能是指坐等下一个更好的闭源模型。

## 1. 公司为什么要 fine-tune

![Why do people finetune at all?](../../slides/10-14-1405-scaling-agentic-rl-across-the-globe/06.jpg)
*fine-tune 的三个理由，一张卡片一张卡片地出现。*

「Why do people finetune at all?」这张幻灯片逐步出现三张卡片：「Pareto-optimal latency, quality and cost, plus real reliability SLOs」（延迟、质量、成本做到 Pareto 最优，外加真正的可靠性 SLO）；「Proprietary data formats, absent from every pretraining set」（任何 pretraining 数据集里都没有的私有数据格式）；以及「Own your intelligence, compete on task-intelligence per $」（掌握自己的智能，在每美元的任务智能上竞争），画成三层堆叠，分别标着 agent harness、model 和 infra。

Mancuso 把每一条都说成 Modal 见到的一类客户。fine-tuning 最先火起来，是在那些需要可靠性 SLO 的 agent 部署团队里。要满足 SLO，就得自己掌握 inference，而 open model「开箱即用」的状态并不总能在不损失质量的前提下撑起这些部署。Mancuso 说，fine-tuning「是自己掌握 inference 的唯一途径」，它能把延迟、质量和成本一起往曲线上方推。

第二类有私有数据集，agent 需要在上面推理，而这些数据不在任何 pretraining 数据集里。Mancuso 说，在这些任务上，「Claude、GPT 根本就不行」，所以这些团队除了 fine-tune open model 别无选择。

第三类是「一种正在出现的态度」：为了用 agent 做出差异化的价值，越来越多公司在技术栈的每一层做垂直整合。这在已经按「每美元的任务智能」竞争的领域最常见，比如 coding；Mancuso 预计它会扩展到 coding 之外。很多工程师还没被说服，「因为最新的模型一出来，就比他们能从上一代 fine-tune 出来的任何模型都好」，但 Mancuso 说，从长远看，fine-tuning「只会越来越普遍」。

## 2. 为什么是 RL 而不是 SFT

![Why RL, and not SFT?](../../slides/10-14-1405-scaling-agentic-rl-across-the-globe/07.jpg)
*一棵跨五轮的 agent session 可能性树；SFT 只在一条录下来的路径上训练。*

幻灯片把一个 agent session 画成一棵在每一轮都分叉的树，从任务一直到第 5 轮，其中一条粗路径标着「SFT: train on a recorded trace」（SFT：在录下来的 trace 上训练）。Mancuso 说，supervised fine-tuning 是用生产环境里录下来的 trace 做训练。但 agent 每一轮都会分叉，所以一个在某条 trace 上训练出来的模型，部署后可能会走到可能性树上「一个完全不同的分支」。「在这方面，SFT 相当脆弱。」在 RL 里，模型在训练时就会探索这些分支：「至少这是一种期望。当人们说 RL 泛化得更好时，通常指的就是这个。」

## 3. 生产环境里的 agent 已经备齐了原料

![But isn't it hard to do RL?](../../slides/10-14-1405-scaling-agentic-rl-across-the-globe/08.jpg)
*一个已部署 agent 的团队手里已有的东西，对应到 RL 需要的东西。*

幻灯片把团队已有的东西对应到 RL 需要的东西：

| 你已经有 | RL 需要 |
| --- | --- |
| Agent harness, deployed in prod | Environment (harness + evals) |
| Evals, for that deployment | |
| Prod traces, from that deployment | Tasks (train / test split) |

「如果你在生产环境里有一个 agent 部署，你很可能已经有了需要的原料。」Mancuso 说。eval 不一定非得叫 eval；A/B 测试，或者「基本上你对这个 harness 做变更管理的任何方式」都行。harness 和 eval 合起来就是一个 RL environment。任务应该来自真实使用：从线上生产 trace 里合成，「找出失败的例子，把它们挖出来，围绕它们构造任务」，这样 fine-tuning 就有机会恰好在这些地方改进产品。

Mancuso 承认 RL 仍然有「难」的名声，但「在过去一年或者过去几个月里，我们差不多把 RL 的科学弄明白了」。talk 剩下的部分介绍的是「一套我们用得很成功的 RL 配方」。

## 4. RL 循环，以及为什么异步会带来不一致

![Isn't the science hard?](../../slides/10-14-1405-scaling-agentic-rl-across-the-globe/10.jpg)
*六个已发表的来源，组合成「a recipe that we've found success with」。*

「Isn't the science hard?」这张幻灯片列了六个来源，先单独列出，再用箭头连成「a recipe that we've found success with」（一套我们用得很成功的配方）。精确笔记逐条核对了引用：

| 幻灯片上的技术 | 幻灯片上的引用 | 指的是什么 |
| --- | --- | --- |
| GRPO | arXiv 2402.03300 | *DeepSeekMath*（Shao et al.，2024 年 2 月） |
| Masked importance sampling | arXiv 2510.18855 | *Every Step Evolves*，即 Ring-1T 论文（Ling Team，2025 年 10 月），方法名叫 IcePop：「token-level discrepancy masking and clipping」 |
| Score centering | arXiv 2609.20807 | *Score Centering Stabilizes Off-policy Reinforcement Learning*（Marek 和 Ryabinin，2026 年 9 月） |
| Top-p mask replay | arXiv 2512.02556 | DeepSeek-V3.2 技术报告（2025 年 12 月），里面叫「Keep Sampling Mask」 |
| Token-in, token-out | "LMSYS blog, 2026" | 可能是 LMSYS 2026 年 5 月的文章「No Token Left Behind」，但这个对应没有得到确认 |
| IMPALA | arXiv 1802.01561 | *IMPALA*（Espeholt et al.，2018） |

![How does RL work?](../../slides/10-14-1405-scaling-agentic-rl-across-the-globe/11.jpg)
*sampler 把 trajectory 和 log-probability 发给 trainer，trainer 再把新权重发回来。*

Mancuso 先回顾了机制。很多 GPU 运行 sampler，也就是正在训练的模型的副本，它们用 harness 在 environment 里生成 rollout。trainer 消费这些 rollout，用梯度下降更新模型，再把新权重广播回 sampler，这样 rollout 就一直来自一个不断改进的模型。

![The math of RL prefers sequential steps](../../slides/10-14-1405-scaling-agentic-rl-across-the-globe/12.jpg)
*同步 RL：inference 和训练轮流进行，一方工作时另一方空闲。*

「RL 的数学希望这件事按顺序来做。」Mancuso 说：用 v0 做 rollout，训练到 v1，用 v1 做 rollout，依此类推。幻灯片上 inference 和 trainer 两行轮流工作，中间是斜线标出的空闲块。「这浪费了大量空闲算力。」

![We want both sides async](../../slides/10-14-1405-scaling-agentic-rl-across-the-globe/13.jpg)
*异步 RL：两行都在持续运行。*

对 agent 来说，让两边持续、并行地运行要好得多，这样你能「把 GPU 塞满，拿到高利用率，不浪费钱」。代价是 staleness（数据陈旧）：rollout 很可能是由模型的一个旧副本生成的。「rollout 是 v1 生成的，而你正在训练的当前模型是 v3。」它们对 v3 还有用吗？

Mancuso 说，第二个不稳定来源纯粹在系统层面。在技术栈的每一层，「trainer 认为自己在训练的东西」和「sampler 认为的东西」之间，都可能有细微的差别。RL 的数学假设两边从同一个分布学习；实际上，分布略有不同。

![Why does RL have a reputation for being hard?](../../slides/10-14-1405-scaling-agentic-rl-across-the-globe/14.jpg)
*sampler 和 trainer 之间不一致的七个来源。*

![同一张表加上了修复方法](../../slides/10-14-1405-scaling-agentic-rl-across-the-globe/15.jpg)
*下一张幻灯片加了一列修复方法。*

两张幻灯片先列出了不一致的来源，再列出修复方法。（修复方法框的分组是根据它们在照片里的位置读出来的：一个框跨了前五行。）

| 不一致来源 | 影响 | 修复 |
| --- | --- | --- |
| Kernels | 相同输入得到不同输出 | Off-policy correction (MIS + SC) |
| Precision | BF16、FP8 和 NVFP4 的舍入方式不同 | Off-policy correction (MIS + SC) |
| Accelerator | 每种芯片的 kernel 和数值格式不同 | Off-policy correction (MIS + SC) |
| Policy staleness | rollout 来自更旧的权重 | Off-policy correction (MIS + SC) |
| Routing | 同一个 token 选中了不同的 expert | Off-policy correction (MIS + SC) |
| Top-p / top-k sampling | action space 不一致 | Top-p mask replay |
| Multi-turn context | trainer 给模型从没见过的 context 打分 | Token-in, token-out |

Mancuso 说，目标是「用最简单的一组修复来纠正这种不一致，同时不把信号淹没」。

## 5. 配方

配方从 GRPO 开始，Mancuso 说那是「DeepSeek 为 DeepSeek R1 提出的」。（幻灯片引用的是 DeepSeekMath 论文，GRPO 最早在那篇论文里提出；R1 是后来用了它。）

第一项补充是 **masked importance sampling (MIS)**，它纠正 token 级别的不一致，既包括系统差异带来的，也包括 staleness 带来的：如果模型的一个旧版本在某个位置会采样出不同的 token，两个概率之间的比值可能会差得很远。Mancuso 没有展开讲机制。Modal 关于这次实验的博客文章做了描述：每个 token 按它的 importance ratio 重新加权，ratio 落在固定区间之外的 token 被 mask 掉。「没有这个 mask，几个 sampler 很少产生的 token 可能带着非常大的 ratio，主导整个更新。」

第二项是 **score centering (SC)**。Mancuso 说，即使每个 token 都纠正过了，剩下的不一致仍会在训练过程中累积「在权重本身里」，于是最新的模型和它应有的样子略有偏差。score centering「在整个训练过程中消除这种漂移，而不只是在每个 token 或每一步上。它大概是构造出一个随漂移一起累积的项，再把它减掉」。score centering 的论文把这个问题称为 training-inference mismatch，并说不稳定主要来自漂移，即「训练引擎和推理引擎之间一种持续存在的偏差，随每一步训练累积」；它的解法是一个可以和 importance sampling 叠加使用的加性修正。

第三项是 **top-p sampling with mask replay**。top-p sampling 只保留概率分布的头部，去掉「一堆非常罕见、很可能很糟的 token」。如果 sampler 把自己限制在这个集合里，而 trainer 对「哪些 token 是可能的」看法不同，「这种不一致如果不纠正，基本上是灾难性的」。按博客的描述，sampler 只从 top-p 候选集合里采样，trainer 也在同一个集合上重新归一化。（Mancuso 在台上把 top-p 解释为保留「概率质量大于……5%」的 token；严格来说，top-p 为 0.95 时保留的是概率加起来达到 0.95 的最小 token 集合。）Mancuso 补充说，有了 top-p，score centering 的修正就从近似变成了精确，没有它的话，「其实只是对漂移的近似修正」。这个说法不在论文摘要里，无法核实。

修复列表里的第四项 token-in-token-out，按博客的说法，用来对齐 sampler 和 trainer 之间的多轮 context。

## 6. 消融实验：Qwen3.6-35B-A3B 做 agentic coding

![Training Qwen3.6-35B-A3B on agentic coding](../../slides/10-14-1405-scaling-agentic-rl-across-the-globe/16.jpg)
*六种方法在混合 sampler 和同构 sampler 上、200 个训练步内的 pass@4。*

「有了这套配方，情况会好得多得多，也稳定得多。」Mancuso 说。幻灯片展示了一个消融实验：用 mixture-of-experts 模型 Qwen3.6-35B-A3B，在 MiMo 代码 environment 上训练，在 SWE-bench Pro 及其困难子集 HARD-51 上评估。两张图画了 200 个训练步内 HARD-51 和全集上的 pass@4，共六种方法（GRPO、GRPO + MIS、GRPO + top-p、SC、SC + MIS、SC + MIS + top-p），每种方法分别在混合 sampler（实线）和同构 sampler（虚线）上跑。好几条线在中途断崖式下跌；SC + MIS 的线稳住了。Mancuso 说，结果显示「训练比我们在这里测过的任何替代方案都稳定得多」。

Modal 在 talk 前一天发布的博客文章给出了设置和数字：

| 设置 | 细节（来自博客） |
| --- | --- |
| 任务 | Agentic coding，context 最长 256K；模型通过 Modal Sandbox 里的 bash tool 分几轮写出一个 patch；在一个新的 sandbox 里测试通过 reward 为 1，否则为 0 |
| 训练数据 | Xiaomi 随 MiMo-V2.6 发布的 2.7K 个仓库任务，去掉了会泄露参考答案的仓库历史 |
| 评估 | SWE-bench Pro V2（与训练集无重叠）及其 HARD-51 子集；每个 checkpoint 都用 BF16 评估 |
| 技术栈 | Miles（RL 框架）、SGLang（sampler）、Megatron-LM（trainer，在 B200 上用 BF16） |
| 异步程度 | 完全异步，staleness 无上限，平均 staleness 约为 4；每次更新 128 个 prompt × 8 条 trajectory |
| 混合 sampler | 6 种加速器上的 8 组：B200/B300 用 NVFP4；H100/H200 用 FP8 和 BF16；A100 和 RTX PRO 6000 用 BF16；全部使用 FP8 KV cache |
| 同构 sampler | 只用 B200，BF16 权重和 BF16 KV cache |

按博客的说法，「不做修正的话，两种设置下训练都会崩溃」；只有 SC + MIS（加不加 top-p 都行）一直稳定训练到最后。用 SC + MIS + top-p，HARD-51 的 pass@4 升到大约 65–75%，全集 pass@4 从 89% 升到约 94%。崩溃的表现是：训练 reward 下降，sampler 和 trainer 之间的 KL 涨到三倍以上，大多数 episode 以反复的格式错误结束。混合 sampler 的 KL 平均是同构 sampler 的 4.7 倍，但同构的 GRPO 初始 KL 只有大约四分之一，仍然在第 100 步左右崩溃了。博客的结论是：「稳定性与其说取决于不一致的大小，不如说取决于它是否持续增长。」（Mancuso 说的是「SWE-bench Pro」；博客写明是 V2。）

## 7. 为什么是 Modal：碎片化的算力

「这套配方我们能推到多远？它真的能规模化吗？」Mancuso 问。在回答之前，Mancuso 先解释了在 Modal 上做 RL 有什么不同。

![Why Modal for agent RL?](../../slides/10-14-1405-scaling-agentic-rl-across-the-globe/17.jpg)
*只拍到了第一张卡片「Training clusters」。*

Modal 做训练，而且「上周还是上上周刚把 clusters 正式 GA」（二手报道说正式上线日期是 10 月 1 日）。它在大量 endpoint 后面大规模运行 inference，agent environment 跑在 Modal Sandbox 里。所以 Modal 具备组合出一整套生产级 agent RL 系统所需的全部基础设施 primitive。「唯一的问题是，我们的算力模式和别人很不一样。」

![Modal's capacity model](../../slides/10-14-1405-scaling-agentic-rl-across-the-globe/19.jpg)
*GPU 分散在不同的提供商和区域；4×B200 的请求能放下，16×B200 的请求放不下。*

幻灯片上画了三家提供商，每家两个区域，各有数量不等的 B200、H100、A100、H200、B300 和 RTX 6000 GPU。一个请求面板显示「4 × B200 fits」，而「16 × B200」则「no fit」，并注明「RL: 2:1 inference to trainer GPUs」（RL：inference 和 trainer GPU 之比为 2:1）。Mancuso 说，幻灯片上画了三家，但「实际上现在大概有 15 到 20 家」，包括所有 hyperscaler 和很多 neocloud，由 Modal 负责 GPU 的健康和可靠性。这对可靠性、对快速灵活的调度都有好处，「但它也让我们的算力池碎片化了」。

Modal 能比较快地调度出四节点或八节点的集群，但到了 16 或 32 个节点，「就相当痛苦，要花很长时间才能找到一块连续的资源」。这对 RL 尤其不利：agent RL 通常需要「每一块训练 GPU 配两块 inference GPU」，所以集群里大约三分之二在做 inference，一个八节点的集群里可能只有两三个节点在训练。

## 8. 稀疏的权重更新让 inference 可以离开集群

![The weight update is super sparse](../../slides/10-14-1405-scaling-agentic-rl-across-the-globe/21.jpg)
*第 t+1 步的权重减去第 t 步的权重：「diff: mostly zeros」。*

解法是把 inference GPU 移出训练集群。Mancuso 说，让这件事成为可能的，是 RL 的权重更新「最终会极其稀疏」（Mancuso 说有两个原因，但录音没有录到是哪两个）。幻灯片把第 t+1 步的权重减去第 t 步的权重，得到一个「mostly zeros」（大部分是零）的 diff。虽然完整的权重是稠密且庞大的，但 Mancuso 说更新「稀疏 100 倍或 1,000 倍」，所以用现成的压缩就能把从 trainer 发到 rollout 引擎的数据量「减少到千分之一」。这些数字只是口头说的，不在幻灯片上，也不在博客文章或 Stitch README 里。

「我提到的那次 Qwen3.6 的运行，我们把训练集群跑在……」作者的录音在这里结束，大约在 15 分钟处。

## 9. 最后几张幻灯片（只有照片，没有录音）

剩下的幻灯片拍了照片但没有录音，所以 Mancuso 在这些幻灯片上讲了什么无从得知。这一段里有几张幻灯片没有拍到，拍到的幻灯片都没有直接讲 Stitch，所以 talk 有没有点名介绍 Stitch 无法确认。

![Stable training despite massive mismatch](../../slides/10-14-1405-scaling-agentic-rl-across-the-globe/22.jpg)
*SC + MIS + top-p 在混合和同构 sampler 上的训练 reward、sampler-trainer KL、梯度范数和熵。*

「Stable training despite massive mismatch」（不一致很大，训练依然稳定）展示了四张小图（训练 reward、每个 token 上 sampler 和 trainer 之间的 KL、梯度范数和熵），对应 SC + MIS + top-p 在混合 sampler 和同构 sampler 上的表现。KL 那张图里两条线明显分开，混合的更高；另外三张图里两条线几乎重合。这和博客一致：混合 sampler 的 KL 平均高 4.7 倍，但 pass@4、reward 和梯度范数都很接近。

![Many GPU types, many precisions](../../slides/10-14-1405-scaling-agentic-rl-across-the-globe/23.jpg)
*训练过程中每种 GPU 型号和精度服务的请求占比。*

「Many GPU types, many precisions」（多种 GPU 型号，多种精度）画出了训练过程中八组 sampler 各自服务的请求占比：A100 (BF16, FlashInfer)、RTX PRO 6000 (BF16, FlashInfer)、H100 (FP8, FA3)、H100 (BF16, FA3)、H200 (FP8, FA3)、H200 (BF16, FA3)、B200 (NVFP4, TRT-LLM) 和 B300 (NVFP4, TRT-LLM)。照片里每一行的百分比看不清。博客给了两个：用 FP8 的 H100 产出了 10% 的样本，用 NVFP4 的 B200 加 B300 合计产出了 47%。博客还说，在这次运行的大部分时间里，混合 sampler 每次更新的成本比同构的低约 15%，而且还有进一步节省的空间。

![Competitive agents need the full stack](../../slides/10-14-1405-scaling-agentic-rl-across-the-globe/24.jpg)
*agent harness、model 和 infra：最后一张幻灯片重复了开头的三层。*

拍到的最后一张幻灯片「Competitive agents need the full stack」（有竞争力的 agent 需要完整的技术栈），重复了开头那张「own your intelligence」卡片里的三层：agent harness、model 和 infra。

Stitch 是简介里提到的那个框架，它的 README 把它描述为「the versioned control plane for disaggregated reinforcement learning」（解耦式 reinforcement learning 的版本化控制平面）。trainer 把 policy 版本发布到一个 store；请求可以要求最低或精确的 policy 版本，无法满足的副本会返回一个可重试的 `409`；副本在继续服务的同时预取并校验下一个完整 checkpoint 或增量，短暂暂停以激活它，只有追上之后才重新加入轮转。「版本的字节先落盘持久化，共享指针才会前移。」它不规定训练算法、推理引擎、存储或算力提供商。

---

**链接：** [日程](https://ai.engineer/nyc/2026/schedule?session=e6ccda8d-0613-44f8-b98f-53ba8037b518) · [Stitch](https://github.com/modal-projects/stitch) · [Toward RL on mixed-precision, mixed-accelerator samplers](https://modal.com/blog/toward-rl-on-mixed-precision-mixed-accelerator-samplers)（Nan Jiang，Modal）· [DeepSeekMath (GRPO)](https://arxiv.org/abs/2402.03300) · [Ring-1T / IcePop](https://arxiv.org/abs/2510.18855) · [Score Centering](https://arxiv.org/abs/2609.20807) · [DeepSeek-V3.2](https://arxiv.org/abs/2512.02556) · [No Token Left Behind](https://www.lmsys.org/blog/2026-05-13-no-token-left-behind)（LMSYS）· [IMPALA](https://arxiv.org/abs/1802.01561) · [Taking Reinforcement Learning Cross Datacenter](https://www.youtube.com/watch?v=maRzp4kImJ4)（Nan Jiang 8 月的 talk，背景资料）

截至写作时，这场 session 还没有发布视频。本文依据作者的现场录音（在大约 15 分钟处、一句话说到一半时中断）、幻灯片照片，以及上面列出的论文、博客文章和代码仓库。最后几张幻灯片只能根据照片描述；录音结束时没有 Q&A，之后有没有 Q&A 无从得知。[精确笔记](../precise/10-14-1405-scaling-agentic-rl-across-the-globe.md)（中文）保留了时间戳，以及未经核实的细节的完整清单。
