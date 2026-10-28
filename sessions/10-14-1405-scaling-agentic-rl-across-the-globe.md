# Scaling Agentic RL Across the Globe：精确版

> Precise notes for this session, compiled from the official video captions where available, the author's room recording, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材整理完、术语和数字核对完。**录音在 [14:59] 截断**（讲到 "we ran the training cluster and"），之后约 3–4 分钟（slide 19→25）只有照片，没有口述。没有录到 Q&A，也不确定现场有没有 Q&A。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | 作者拍的 24 张（IMG_0675–0695、0698–0700）。翻页器显示总共 27 页，拍到第 3、4、5、6、7、8、9、10、11、12、13、14、16、17、18/19、22（或 24，照片模糊）、23、25 页。**没有外来照片**：第一张 IMG_0675 是本场第 3/27 页（"A losing strategy once you compete on agent economics"），拍摄时间比录音开始早 5 秒 |
| 视频 | 没有。本场未找到上传。背景：同事 Nan Jiang 八月在 AI Engineer 讲过 "Taking Reinforcement Learning Cross Datacenter — Nan Jiang, Modal"（[YouTube](https://www.youtube.com/watch?v=maRzp4kImJ4)，只读了元数据，没看内容） |
| 一手资料 | [Stitch repo](https://github.com/modal-projects/stitch)（README）；Modal 博客 [Toward RL on mixed-precision, mixed-accelerator samplers](https://modal.com/blog/toward-rl-on-mixed-precision-mixed-accelerator-samplers)（Nan Jiang，2026-10-13，即演讲前一天）——**这篇就是 slide 14、22、23 的实验**，数字以它为准；slide 上引用的 5 篇 arXiv 论文（用 arXiv API 核对了标题、作者、日期） |

## 基本信息

- 时间地点：2026-10-14 2:05–2:23pm，Shannon Room (NY Ballroom E)，Expo Session（18 分钟）。按录音和照片时间推算，录音从约 2:06pm 开始，最后一张照片（第 25/27 页）约 2:24pm【待确认：时间来自导入时的 take 起点，可能有一两分钟误差】
- 讲者：Jason Mancuso，Modal，Member of Technical Staff, Research（session.json；X `@jvmncs`，GitHub `jvmncs`）
- 官方简介要点：企业为了控制成本、质量、性能、可靠性，转向 post-train 并部署开源模型；「Reinforcement learning is the most critical component of the posttraining stack」；会介绍 **Stitch**（开源框架，把 RL rollout 拆到多云、多区域、异构 GPU 上）和「a stable recipe for posttraining agentic models on off-policy rollout data」
- **注意**：录音截断前讲者没说出 "Stitch" 这个词；拍到的 slide 里也没有 Stitch 的页面。Stitch 是否在最后几分钟讲到，【待确认】

## 1. 为什么要 fine-tune

- slide 3/27「A losing strategy once you compete on agent economics」（前一页没拍到，【待确认】"losing strategy" 指的是什么；按讲者说 [02:11] 推测是「等下一个更强的闭源模型」，我的理解）
- slide 4/27「Why do people finetune at all?」三张卡片：
  1. **Pareto-optimal latency, quality and cost**，plus real reliability SLOs（图：一条曲线，一个点被拉到曲线上方）
  2. **Proprietary data formats**，absent from every pretraining set
  3. **Own your intelligence**，compete on task-intelligence per $（卡片里画了三层：Agent harness / Model / Infra）
- 讲者说 [00:00]–[01:01]：最早做 fine-tuning 的是有 agent 部署、需要可靠性 SLO 的客户；要 SLO 就得自己掌控 inference，而开源模型「out of the box」撑不住他们的部署质量；fine-tuning 是「the only path to owning your own inference」，并且能同时把 latency、quality、cost 往上推
- 讲者说 [01:01]：第二类是专有数据集，预训练里没有，「Claude, GPT just like would not work at all for these tasks」（转写为 "CLAWd, QPT"）
- 讲者说 [01:37]：第三类是一种新态度：为了做出差异化，越来越多人在整个栈上做垂直整合；最常见于已经在比「task intelligence per dollar」的行业，比如 coding；他预期这个趋势会扩散到 coding 以外
- 讲者说 [02:11]：很多工程师还没完全接受，「because the latest model comes out and it's better than any model they could fine-tune from the last generation」，但长期看 fine-tuning 只会更常见

## 2. 为什么是 RL 而不是 SFT

- slide 5/27「Why RL, and not SFT?」：一棵树，task → turn 1 … turn 5，每一轮分叉；其中一条路径加粗，标注「SFT: train on a recorded trace」
- 讲者说 [02:22]–[03:43]：SFT 是在生产里录下来的 trace 上训练；但 agent 按轮次有分支结构，部署后模型可能走到「a completely different branch」，所以「SFT is quite brittle in this regard」。RL 在训练时让模型探索这些分支，「At least that's kind of a hope」；人们说 RL「generalizing better」，通常指的就是这个

## 3. 生产里的 agent 已经有 RL 需要的东西

- slide 6/27「But isn't it hard to do RL?」：
  - YOU ALREADY HAVE：**Agent harness**（deployed in prod）✓、**Evals**（for that deployment）✓、**Prod traces**（from that deployment）✓
  - RL NEEDS：**Environment**（harness + evals）← 前两项；**Tasks**（train / test split）← prod traces
- 讲者说 [03:43]：「if you have an agent deployment in production, you probably already have the ingredients you need」。evals 不一定叫 evals，A/B test 或任何对这个 harness 做变更管理的方式都行；harness + evals 合起来就是一个 RL environment（转写为 "a gnarle environment"）
- 讲者说 [04:19]：训练任务最好从真实 production traces 合成：「finding examples that are failing, mining those, and creating tasks around them」，这样 fine-tuning 才可能改进产品在这些地方的表现
- 讲者说 [04:53]–[05:29]：RL 名声难，但「over the last year or last several months like we have kind of figured out the science of RL」；今天讲一个「particular RL recipe that we've had a lot of success with」

## 4. RL 循环、异步化和 mismatch 的来源

- slide 7/27、8/27「Isn't the science hard?」：列了 6 个来源，第二版用箭头汇成「**A recipe** that we've found success with」（arXiv 编号来自 slide；标题、作者、日期用 arXiv API 核对）：

| slide 上的名字 | slide 上的出处 | 核对结果（资料） |
| --- | --- | --- |
| GRPO | arXiv 2402.03300 | DeepSeekMath: Pushing the Limits of Mathematical Reasoning in Open Language Models（Zhihong Shao 等，2024-02-05） |
| Masked importance sampling | arXiv 2510.18855 | Every Step Evolves: Scaling Reinforcement Learning for Trillion-Scale Thinking Model（Ling Team，Ring-1T，2025-10-21）。摘要里对应的方法叫 **IcePop**：「stabilizes RL training via token-level discrepancy masking and clipping」 |
| Score centering | arXiv 2609.20807 | Score Centering Stabilizes Off-policy Reinforcement Learning（Martin Marek、Max Ryabinin，2026-09-17） |
| Top-p mask replay | arXiv 2512.02556 | DeepSeek-V3.2: Pushing the Frontier of Open Large Language Models（DeepSeek-AI，2025-12-02）。对应报告里的「Keep Sampling Mask」（据 [vLLM 文档](https://docs.vllm.ai/en/stable/training/sampling_mask/) 转述 Section 3.3，没读到报告原文） |
| Token-in, token-out | LMSYS blog, 2026 | 我的理解：应是 LMSYS 的 [No Token Left Behind: Demystifying Token-In-Token-Out in Miles](https://www.lmsys.org/blog/2026-05-13-no-token-left-behind)（2026-05-13），只看到搜索摘要，【待确认】是否就是这篇 |
| IMPALA | arXiv 1802.01561 | IMPALA: Scalable Distributed Deep-RL with Importance Weighted Actor-Learner Architectures（Lasse Espeholt 等，2018-02-05） |

- slide 9/27「How does RL work?」：多个 SAMPLER 把「trajectories + log-probabilities」送到 TRAINER，TRAINER 回传「new weights」
- 讲者说 [05:50]：sampler 就是正在训练的模型，在 environment 里用 harness 生成 rollouts；trainer 用梯度下降更新模型，再把新权重广播回 samplers
- slide 10/27「The math of RL prefers sequential steps」：INFERENCE 行 rollouts v0 / v1 / v2，TRAINER 行 train → v1 / v2 / v3，两行交替，中间是空闲的斜线格
- 讲者说 [06:23]：「the math of RL prefers this to be done sequentially」，但「this wastes a ton of idle compute」
- slide 11/27「We want both sides async」：INFERENCE 连续跑 rollouts v0, v0, v1, v2, v3, v4；TRAINER 连续跑 train → v1 … v5
- 讲者说 [07:03]：对 agent 来说，两边并行异步「pack your GPUs, get high utilization」；代价是 rollouts 很可能来自旧版本模型，这就是 **staleness**：「rollouts were generated by v1, the current model that you're training is v3」
- 讲者说 [07:36]：第二个不稳定的原因纯在系统层：「what the trainer thinks is training and what the samplers think」在栈的各层都有小差别；RL 的数学假设两边在学同一个分布，实际上略有不同
- slide 12/27、13/27「Why does RL have a reputation for being hard?」：

| MISMATCH SOURCE | EFFECT | FIX（13/27 加的一列） |
| --- | --- | --- |
| Kernels | Different outputs for the same input | Off-policy correction（MIS + SC） |
| Precision | BF16, FP8 and NVFP4 round differently | 同上 |
| Accelerator | Different kernels and number formats per chip | 同上 |
| Policy staleness | Rollouts come from older weights | 同上 |
| Routing | Different experts picked for the same token | 同上 |
| Top-p / top-k sampling | Mismatched action spaces | Top-p mask replay |
| Multi-turn context | Trainer scores context the model never saw | Token-in, token-out |

（FIX 列的框和行的对应关系是从照片上框的位置读的，Off-policy correction 的框覆盖前 5 行。）

## 5. 稳定配方：GRPO + MIS + score centering + top-p mask replay

- 讲者说 [08:35]：「we start from GRPO, this is what DeepSeek introduced for DeepSeek R1」（**出入**见下）
- 讲者说 [08:35]：加 **masked importance sampling**，修正 token 级的 mismatch，既包括系统层的差异，也包括 staleness：旧版本模型在某个位置会采样出不同的 token，两者概率之比可能差很多。他说「I won't go too much into details here about how these mechanisms work」
- 讲者说 [09:10]–[09:45]：第二是 **score centering**：逐 token 修正之后，剩余的 mismatch 会在训练中累积进权重；score centering「removes this drift over the course of training, not just at each token or at each step」，「builds up a term that accumulates with the drift and removes it」
- 资料（Score centering 论文摘要，arXiv 2609.20807）：训练与推理引擎的差异叫 training-inference mismatch（TIM）；不稳定主要来自 **drift**，即「a persistent bias between training and inference engines that accumulates with every training step」；score centering 是一个加性修正项，能和 importance sampling 叠加
- 讲者说 [09:45]–[10:50]：最后是 **top-p sampling with mask replay**：top-p 只从分布的头部采样，去掉很罕见的 token；如果两边对「能采哪些 token」的认定不一致，「this mismatch can be basically catastrophic if you don't correct for it」。他还说：做了 top-p 之后，score centering 的修正是 **exact** 而不是 approximate；不做 top-p 时「it's actually only an approximate correction for drift」（这一点我没在论文摘要里看到，【待确认】）
- 资料（[Modal 博客](https://modal.com/blog/toward-rl-on-mixed-precision-mixed-accelerator-samplers)）对各组件的说明：
  - MIS：按每个 token 的 importance ratio 重新加权，ratio 超出固定范围的 token 被 mask 掉；「Without the mask, a few tokens the samplers rarely produce could carry very large ratios and dominate the update.」
  - SC：在每个位置减去采样策略下 log-probability 梯度的期望，以消除 drift
  - Top-p mask replay：samplers 只从 top-p 候选集合采样，trainer 在同一个集合上重新归一化；「Runs without it sample from the full vocabulary.」
  - Token-in-token-out：用来对齐 multi-turn context

## 6. 消融实验：Qwen3.6-35B-A3B 上的 agentic coding

- slide 14/27「Training **Qwen3.6-35B-A3B** on agentic coding」：两张图，左「pass@4 on HARD-51」（纵轴 0–80%），右「pass@4 on the full set」（纵轴 50–100%），横轴 Training step 0–200。图例：Samplers = Mixed（实线）/ Homogeneous（虚线）；Method = GRPO、GRPO + MIS、GRPO + top-p、SC、SC + MIS、SC + MIS + top-p。图上多条线中途断崖下跌，绿色（SC + MIS、SC + MIS + top-p）一直保持
- 讲者说 [10:53]：「an ablation that we did on Qwen3.6-35B-A3B MoE. It's trained on the MiMo code environments with evals on SWE-bench Pro and the HARD-51 subset」，「putting them all together showing much more stable training than any of the alternatives that we've tested here」
- 资料（Modal 博客）的实验设置和结果：
  - 任务：agentic coding，最长 256K context；模型多轮写 patch，通过 Modal Sandbox 里的 bash 工具提交；在新 sandbox 里过测试 reward = 1，否则 0
  - 数据：Xiaomi 随 MiMo-V2.6 发布的 2.7K 个 repository 任务，去掉了泄露参考答案的 repo history
  - 评测：**SWE-bench Pro V2**（和训练集无重叠）及其 hard 子集 HARD-51；每个 checkpoint 都在 BF16 下评测
  - 栈：Miles（RL 框架）、SGLang（samplers）、Megatron-LM（trainer，B200 上 BF16）
  - 完全异步，不限 staleness；每次更新 128 个 prompt × 8 条轨迹；平均 staleness 约 4
  - Mixed：8 组、6 种加速器（B200/B300 NVFP4；H100/H200 FP8 和 BF16；A100、RTX PRO 6000 BF16；都用 FP8 KV cache）。Homogeneous：只用 B200，BF16 权重 + BF16 KV cache
  - 「Without correction, training collapses on both setups」；只有 SC + MIS（带不带 top-p）稳定训练到最后
  - SC + MIS + top-p：HARD-51 pass@4 升到约 65–75%；full set 从 89% 升到约 94%
  - Mixed samplers 的 sampler–trainer KL 平均是 homogeneous 的 **4.7×**；homogeneous 的 GRPO 起始 KL 低约 4 倍，仍在约 step 100 崩溃
  - 崩溃的特征：training reward 下降，KL 涨 3 倍以上，多数 episode 以重复的格式错误结束
  - 「stability depends less on the magnitude of the mismatch than on whether it keeps growing.」

## 7. 为什么在 Modal 上做 agent RL：碎片化的容量

- 讲者说 [11:30]：「how far can we push this recipe? Does it actually scale?」——但先讲 Modal 上做 RL 有什么不同
- slide 16/27「Why Modal for agent RL?」：只拍到第一张卡片「Training CLUSTERS」（后面的卡片是逐个出现的，没拍到）
- 讲者说 [11:30]–[12:02]：Modal 做 training，「just released clusters as GA last week or week before」；inference 有很多 endpoints；environments 跑在 Modal Sandboxes 里（转写为 "most sandboxes"）。所以「we have all of the … infra primitives」能组合成完整的生产级 agent RL 训练系统。唯一的问题是「our capacity model is very different」
- 资料（二手）：多篇报道称 Modal Clusters 于 2026-10-01 GA（如 [Futurum](https://futurumgroup.com/insights/modal-clusters-ga-serverless-multi-node-gpus-for-every-enterprise/)），与「上周或上上周」吻合；没找到 Modal 自己的公告原文，【待确认】
- slide 17/27「Modal's capacity model」：Provider A/B/C，每家两个 region，各有不同数量的 B200、H100、A100、H200、B300、RTX 6000。第二版加了 REQUESTS：「4 × B200 fits」「16 × B200 no fit」「RL: 2:1 inference to trainer GPUs」
- 讲者说 [12:33]–[13:13]：图上画了三家，「in reality, it's like 15 or 20 at this point」，包括所有 hyperscaler 和一批 neocloud；Modal 负责 GPU 健康和可靠性。这对可靠性、快速灵活调度好，但「it also fragments our compute pool」
- 讲者说 [13:13]：能较快调度到 4 节点或 8 节点的集群；到 16、32 节点就「pretty painful, it takes a really long time to find a contiguous block」
- 讲者说 [13:13]–[13:48]：agent RL 里通常「two inference GPUs for every training GPU」，约三分之二的集群在做 inference；8 节点集群里 trainer 可能只用两三个节点

## 8. 稀疏的权重更新：把 inference GPU 移出集群

- slide 18/27、19/27「The weight update is super sparse」：weights, step t+1 − weights, step t = 「diff: mostly zeros」
- 讲者说 [13:48]–[14:52]：想把 inference GPU 移出训练集群；依据是 RL 的权重更新「end up being incredibly sparse」（他说有两个原因，录音里没讲出具体是哪两个）。T+1 减 T 得到 delta，「100x or 1,000x as sparse」，用现成的压缩就能让 trainer 到 rollout engines 的通信量「goes down by a factor of a thousand」，所以 inference GPU 可以放到集群外
- **【待确认】** 100×–1000× 和「降到千分之一」只有讲者口述，slide 上没有数字；Modal 博客没有这组数字；Stitch README 也没有给 delta 的压缩比
- 讲者说 [14:52]：「for the run that I mentioned of Qwen3.6, we ran the training cluster and」——录音到此截断
- 资料（[Stitch README](https://github.com/modal-projects/stitch)）：「Stitch is the versioned control plane for disaggregated reinforcement learning.」trainer 发布 policy 版本到 store；请求可以要求最低或精确的 policy 版本，不兼容的 replica 返回可重试的 `409`；replica 在服务的同时预取并校验下一个完整 checkpoint 或 delta，激活时短暂暂停引擎；新 replica 追上当前版本才进入轮转；「Version bytes become durable before the shared pointer advances.」不规定训练算法、推理引擎、存储或算力提供方

## 9. 录音截断后的 slide（只有照片）

- slide 22/27（或 24，照片上的页码模糊）「Many GPU types, many precisions」：8 行，标题「Share of requests served」，右侧一列「Overall」：
  - A100（BF16 · FlashInfer）、RTX PRO 6000（BF16 · FlashInfer）、H100（FP8 · FA3）、H100（BF16 · FA3）、H200（FP8 · FA3）、H200（BF16 · FA3）、B200（NVFP4 · TRT-LLM）、B300（NVFP4 · TRT-LLM）
  - 每行的百分比照片太糊，读不出，【待确认】。资料（Modal 博客）只给了两个数：H100 FP8 提供 10% 的样本；B200 + B300 NVFP4 合计 47%
- slide 23/27「Stable training despite massive mismatch」：四张小图 Training reward、KL(sampler ‖ trainer) per token、Gradient norm、Entropy；两条线「SC + MIS + top-p on the mixed samplers」与「… on the homogeneous samplers」。KL 一图里两条线明显分开（mixed 更高），其他三张图两条线基本重合。资料（博客）：mixed 的 KL 平均是 homogeneous 的 4.7×，修正后两者的 pass@4、reward、gradient norm 保持接近
- slide 25/27「Competitive agents need the full stack」：Agent harness / Model / Infra 三层
- 资料（博客）：训练的大部分时间里，mixed samplers 每次更新的 sampler 成本比 homogeneous 低约 **15%**，作者说还有更多节省空间
- 第 20、21、24（或 22）、26、27 页没拍到，内容【待确认】

## Q&A 精确版

没有录到。录音在 [14:59] 截断，最后一张照片是第 25/27 页；之后有没有 Q&A，【待确认】。

## 勘误表（Whisper 转写 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| out of the bug | out of the box | 上下文 |
| CLAWd, QPT | Claude, GPT | 上下文 |
| a gnarle environment | an RL environment | 上下文 |
| AGE deployment | agent deployment | 上下文 |
| aGIFS | agents【待确认】 | 上下文 |
| DeepSeq / DeepSeq R1 | DeepSeek / DeepSeek R1 | 上下文 |
| top-peat / top-beat sampling | top-p sampling | slide |
| mass replay | mask replay | slide |
| Plan 3.635b of IMOE | Qwen3.6-35B-A3B MoE | slide 14 |
| coin 3.6 | Qwen3.6 | slide 14 |
| MIMO code environments | MiMo（Xiaomi MiMo-V2.6 的 repository 任务） | 资料（博客） |
| Sweebench Pro | SWE-bench Pro（博客写 SWE-bench Pro V2） | 资料（博客） |
| Hard 51 | HARD-51 | slide 14 |
| on modal | on Modal | 上下文 |
| most sandboxes | Modal Sandboxes | 上下文、资料（博客） |
| neoclots | neoclouds | 上下文 |
| it gets us the wrong compute charges | 【待确认】（可能是 "low compute charges"） | 听不清 |
| infra-primitive layers | infra primitives | 上下文 |

## 出入（来源之间不一致的地方）

1. **GRPO 的出处**：讲者说 [08:35]「what DeepSeek introduced for DeepSeek R1」；slide 引用的是 arXiv 2402.03300，即 2024 年 2 月的 DeepSeekMath。GRPO 首次提出是在 DeepSeekMath，R1 沿用了它（资料：arXiv）。以 slide / 论文为准
2. **top-p 的解释**：讲者说 [09:45]「only the tokens that have probability mass greater than 95, or greater than 5%」。我的理解：这不是 top-p 的定义；top-p = 0.95 是保留累计概率达到 0.95 的最小 token 集合，不是「单个概率大于 5% 的 token」。讲者口误或简化，写的时候不要照搬
3. **"masked importance sampling" 的名字**：slide 把它挂在 arXiv 2510.18855（Ring-1T）上，论文里的方法名是 IcePop（token-level discrepancy masking and clipping）。不是矛盾，是叫法不同（我的理解）
4. **评测集名称**：讲者说 "SWE-bench Pro"，博客写 "SWE-bench Pro V2"。以博客为准
5. **provider 数量**：slide 画了 3 家（示意），讲者说实际「15 or 20」。slide 只是示意图
6. **权重稀疏度**：讲者说「100x or 1,000x as sparse」「a factor of a thousand」，slide 没有数字，博客和 README 都没有，无法核对，【待确认】
7. **score centering 在 top-p 下是 exact**：只有讲者口述，论文摘要没提，【待确认】

## 【待确认】清单

1. slide 3/27「A losing strategy」指的是什么（前一页没拍到）
2. 录音截断后（slide 20–27）讲了什么，尤其是否正式介绍了 Stitch、有没有 Q&A
3. slide「Many GPU types, many precisions」的页码（22 还是 24）和每行百分比
4. 权重 delta「100×–1000× 稀疏」「通信量降到千分之一」的数字来源
5. score centering「在 top-p 下 exact、否则 approximate」的说法出处
6. slide 上 "Token-in, token-out — LMSYS blog, 2026" 是否就是 2026-05-13 的 "No Token Left Behind"
7. Modal Clusters GA 日期（二手资料称 2026-10-01，没读到 Modal 原文）
8. 讲者说的「two reasons」稀疏的两个原因（录音没讲出来）
9. 转写里 "aGIFS" 和 "the wrong compute charges" 原话
11. 录音起止时间与日程的对应（来自导入时的 take 起点）
