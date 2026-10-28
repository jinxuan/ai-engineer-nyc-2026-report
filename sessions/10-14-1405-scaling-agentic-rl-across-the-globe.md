# A Stable Recipe for Agent RL on Mismatched GPUs Spread Across Clouds

*At AI Engineer NYC 2026, Modal's Jason Mancuso argued that any team with an agent in production already has what reinforcement learning needs, presented a four-part recipe that keeps asynchronous RL stable when samplers and trainer disagree, and explained how sparse weight updates let inference GPUs live outside the training cluster.*

**Expo Session · Wednesday, October 14, 2026 · 2:05–2:23pm · Shannon Room (NY Ballroom E)**
**Speaker:** Jason Mancuso, Member of Technical Staff, Research, Modal

---

The session's abstract set the premise: companies that want control over cost, quality, performance and reliability are post-training and deploying open models, and "reinforcement learning is the most critical component of the posttraining stack." It promised a look at Stitch, Modal's open-source framework for spreading RL rollouts across clouds, regions and GPU types, and "a stable recipe for posttraining agentic models on off-policy rollout data."

![A losing strategy once you compete on agent economics](../slides/10-14-1405-scaling-agentic-rl-across-the-globe/02.jpg)
*Slide 3 of 27: "A losing strategy once you compete on agent economics."*

The author's room recording begins on slide 3 of 27, which read "A losing strategy once you compete on agent economics." The previous slide was not photographed, so what the losing strategy was is not on record; from what Mancuso said a minute later, it may have been waiting for the next, better closed model.

## 1. Why companies fine-tune

![Why do people finetune at all?](../slides/10-14-1405-scaling-agentic-rl-across-the-globe/06.jpg)
*Three reasons to fine-tune, revealed one card at a time.*

The slide "Why do people finetune at all?" built up three cards: "Pareto-optimal latency, quality and cost, plus real reliability SLOs"; "Proprietary data formats, absent from every pretraining set"; and "Own your intelligence, compete on task-intelligence per $," drawn as three stacked layers labeled agent harness, model and infra.

Mancuso described each as a kind of customer Modal sees. Fine-tuning first took off with teams running agent deployments that needed reliability SLOs. Meeting an SLO means owning your inference, and open models "out of the box" could not always support those deployments without losing quality. Fine-tuning "was the only path to owning your own inference," Mancuso said, and it pushes latency, quality and cost up the curve together.

The second group had proprietary datasets that agents needed to reason over, data absent from any pretraining set. On those tasks, Mancuso said, "Claude, GPT just like would not work at all," so the teams had no alternative to fine-tuning open models.

The third is "an emerging attitude": to build differentiated value with agents, more companies are vertically integrating across every level of the stack. That is most common where companies already compete on "task intelligence per dollar," like coding, and Mancuso expected it to spread beyond coding. Many engineers are not convinced yet, "because the latest model comes out and it's better than any model they could fine-tune from the last generation," but over the long haul, Mancuso said, fine-tuning "is only going to become more common."

## 2. Why RL rather than SFT

![Why RL, and not SFT?](../slides/10-14-1405-scaling-agentic-rl-across-the-globe/07.jpg)
*A tree of possible agent sessions over five turns; SFT trains on one recorded path.*

The slide drew an agent session as a tree branching at every turn, from the task through turn 5, with one thick path labeled "SFT: train on a recorded trace." Supervised fine-tuning, Mancuso said, means training on traces recorded in production. But agents branch with every turn, so a model trained on one trace may, once deployed, find itself on "a completely different branch" of the tree of possible sessions. "SFT is quite brittle in this regard." In RL, the model explores those branches during training: "At least that's kind of a hope. And when people talk about RL like generalizing better, that's often what they mean."

## 3. A production agent already has the ingredients

![But isn't it hard to do RL?](../slides/10-14-1405-scaling-agentic-rl-across-the-globe/08.jpg)
*What a team with a deployed agent already has, mapped to what RL needs.*

The slide mapped what a team already has to what RL needs:

| You already have | RL needs |
| --- | --- |
| Agent harness, deployed in prod | Environment (harness + evals) |
| Evals, for that deployment | |
| Prod traces, from that deployment | Tasks (train / test split) |

"If you have an agent deployment in production, you probably already have the ingredients you need," Mancuso said. The evals need not be called evals; A/B tests or "basically any way you do change management for this harness" will do. Together, the harness and the evals make an RL environment. The tasks should come from real usage: synthesize them from live production traces, "finding examples that are failing, mining those, and creating tasks around them," so fine-tuning has a chance of improving the product exactly there.

RL still has a reputation for being hard, Mancuso acknowledged, but "over the last year or last several months, we have kind of figured out the science of RL." The rest of the talk presented "a particular RL recipe that we've had a lot of success with."

## 4. The RL loop, and why going async causes mismatch

![Isn't the science hard?](../slides/10-14-1405-scaling-agentic-rl-across-the-globe/10.jpg)
*Six published sources combined into "a recipe that we've found success with."*

The slide "Isn't the science hard?" listed six sources, first on their own and then joined by arrows into "a recipe that we've found success with." The precise notes checked each citation:

| Technique on the slide | Citation on the slide | What it refers to |
| --- | --- | --- |
| GRPO | arXiv 2402.03300 | *DeepSeekMath* (Shao et al., February 2024) |
| Masked importance sampling | arXiv 2510.18855 | *Every Step Evolves*, the Ring-1T paper (Ling Team, October 2025), where the method is called IcePop: "token-level discrepancy masking and clipping" |
| Score centering | arXiv 2609.20807 | *Score Centering Stabilizes Off-policy Reinforcement Learning* (Marek and Ryabinin, September 2026) |
| Top-p mask replay | arXiv 2512.02556 | The DeepSeek-V3.2 report (December 2025), where it is called "Keep Sampling Mask" |
| Token-in, token-out | "LMSYS blog, 2026" | Probably LMSYS's May 2026 post "No Token Left Behind," though that match is unconfirmed |
| IMPALA | arXiv 1802.01561 | *IMPALA* (Espeholt et al., 2018) |

![How does RL work?](../slides/10-14-1405-scaling-agentic-rl-across-the-globe/11.jpg)
*Samplers send trajectories and log-probabilities to the trainer, which sends back new weights.*

Mancuso first recapped the mechanics. Many GPUs run samplers, copies of the model being trained, which generate rollouts in the environment using the harness. A trainer consumes the rollouts, updates the model by gradient descent and broadcasts the new weights back to the samplers, so rollouts keep coming from a continuously improving model.

![The math of RL prefers sequential steps](../slides/10-14-1405-scaling-agentic-rl-across-the-globe/12.jpg)
*Synchronous RL: inference and training take turns, and each side idles while the other works.*

"The math of RL prefers this to be done sequentially," Mancuso said: roll out with v0, train to v1, roll out with v1, and so on. The slide drew the inference and trainer rows taking turns, with hatched idle blocks in between. "This wastes a ton of idle compute."

![We want both sides async](../slides/10-14-1405-scaling-agentic-rl-across-the-globe/13.jpg)
*Asynchronous RL: both rows run continuously.*

For agents, it is much better to run both sides continuously and in parallel, which lets you "pack your GPUs, get high utilization from them and not waste money." The cost is staleness: rollouts were likely produced by an older copy of the model. "Rollouts were generated by v1, the current model that you're training is v3." Are they still useful for v3?

A second source of instability is purely at the systems level, Mancuso said. At every level of the stack, small details can differ between "what the trainer thinks is training and what the samplers think." The math of RL assumes both sides are learning from the same distribution; in practice, it is slightly different.

![Why does RL have a reputation for being hard?](../slides/10-14-1405-scaling-agentic-rl-across-the-globe/14.jpg)
*Seven sources of mismatch between samplers and trainer.*

![The same table with fixes added](../slides/10-14-1405-scaling-agentic-rl-across-the-globe/15.jpg)
*The next slide added a column of fixes.*

Two slides catalogued the mismatches and then the fixes. (The grouping of the fix boxes is read from where they sit in the photo: one box spans the first five rows.)

| Mismatch source | Effect | Fix |
| --- | --- | --- |
| Kernels | Different outputs for the same input | Off-policy correction (MIS + SC) |
| Precision | BF16, FP8 and NVFP4 round differently | Off-policy correction (MIS + SC) |
| Accelerator | Different kernels and number formats per chip | Off-policy correction (MIS + SC) |
| Policy staleness | Rollouts come from older weights | Off-policy correction (MIS + SC) |
| Routing | Different experts picked for the same token | Off-policy correction (MIS + SC) |
| Top-p / top-k sampling | Mismatched action spaces | Top-p mask replay |
| Multi-turn context | Trainer scores context the model never saw | Token-in, token-out |

The aim, Mancuso said, was "the simplest set of fixes that will correct for this mismatch without drowning out the signal."

## 5. The recipe

The recipe starts from GRPO, "what DeepSeek introduced for DeepSeek R1," Mancuso said. (The slide cites the DeepSeekMath paper, where GRPO was first introduced; R1 used it later.)

The first addition is **masked importance sampling (MIS)**, which corrects token-level mismatch, both from the systems differences and from staleness: if an older version of the model would have sampled a different token at some position, the ratio between the two probabilities can be very different. Mancuso did not go into the mechanics. Modal's blog post on the experiment describes it: each token is reweighted by its importance ratio, and tokens whose ratio falls outside a fixed range are masked out. "Without the mask, a few tokens the samplers rarely produce could carry very large ratios and dominate the update."

The second is **score centering (SC)**. Even with every token corrected, Mancuso said, whatever mismatch remains accumulates over training "in the weights themselves," so the latest model is slightly different from what it should be. Score centering "removes this drift over the course of training, not just at each token or at each step. It kind of like builds up a term that accumulates with the drift and removes it." The score-centering paper calls the problem training-inference mismatch and says instability comes mainly from drift, "a persistent bias between training and inference engines that accumulates with every training step"; its fix is an additive correction that can be stacked with importance sampling.

The third is **top-p sampling with mask replay**. Top-p sampling keeps only the head of the probability distribution, which removes "a bunch of very rare, likely bad tokens." If the samplers restrict themselves to that set and the trainer's view of which tokens were possible differs, "this mismatch can be basically catastrophic if you don't correct for it." In the blog's description, samplers draw only from the top-p candidate set and the trainer renormalizes over that same set. (On stage Mancuso glossed top-p as keeping tokens with "probability mass greater than … 5%"; strictly, top-p of 0.95 keeps the smallest set of tokens whose probabilities add up to 0.95.) Mancuso added that with top-p in place, the score-centering correction becomes exact rather than approximate, and without it "it's actually only an approximate correction for drift." That claim is not in the paper's abstract and could not be checked.

The fourth item on the fix list, token-in-token-out, aligns multi-turn context between samplers and trainer, according to the blog.

## 6. The ablation: Qwen3.6-35B-A3B on agentic coding

![Training Qwen3.6-35B-A3B on agentic coding](../slides/10-14-1405-scaling-agentic-rl-across-the-globe/16.jpg)
*Pass@4 over 200 training steps for six methods on mixed and homogeneous samplers.*

"With this recipe, things get much, much better and much more stable," Mancuso said. The slide showed an ablation on the mixture-of-experts model Qwen3.6-35B-A3B, trained on MiMo code environments and evaluated on SWE-bench Pro and its hard subset, HARD-51. Two charts plotted pass@4 on HARD-51 and on the full set over 200 training steps, for six methods (GRPO, GRPO + MIS, GRPO + top-p, SC, SC + MIS, SC + MIS + top-p), each on mixed samplers (solid lines) and homogeneous samplers (dashed). Several lines dropped off a cliff partway through; the SC + MIS lines held. The result showed "much more stable training than any of the alternatives that we've tested here," Mancuso said.

Modal's blog post, published the day before the talk, gives the setup and numbers:

| Setting | Detail (from the blog) |
| --- | --- |
| Task | Agentic coding, up to 256K context; the model writes a patch over several turns through a bash tool in a Modal Sandbox; reward is 1 if tests pass in a fresh sandbox, else 0 |
| Training data | 2.7K repository tasks released by Xiaomi with MiMo-V2.6, with repository history that leaked reference answers removed |
| Evaluation | SWE-bench Pro V2 (no overlap with training) and its HARD-51 subset; every checkpoint evaluated in BF16 |
| Stack | Miles (RL framework), SGLang (samplers), Megatron-LM (trainer, BF16 on B200) |
| Asynchrony | Fully asynchronous, staleness unbounded, average staleness about 4; 128 prompts × 8 trajectories per update |
| Mixed samplers | 8 groups across 6 accelerator types: B200/B300 in NVFP4; H100/H200 in FP8 and BF16; A100 and RTX PRO 6000 in BF16; all with FP8 KV cache |
| Homogeneous samplers | B200 only, BF16 weights and BF16 KV cache |

Per the blog, "without correction, training collapses on both setups"; only SC + MIS, with or without top-p, trained stably to the end. With SC + MIS + top-p, HARD-51 pass@4 rose to roughly 65–75%, and full-set pass@4 went from 89% to about 94%. Collapse looked like falling training reward, a sampler-trainer KL more than tripling, and most episodes ending in repeated format errors. The mixed samplers' KL averaged 4.7 times the homogeneous samplers', yet homogeneous GRPO, starting with about a quarter of the KL, still collapsed around step 100. The blog's conclusion: "stability depends less on the magnitude of the mismatch than on whether it keeps growing." (Mancuso said "SWE-bench Pro"; the blog specifies V2.)

## 7. Why Modal: fragmented capacity

"How far can we push this recipe? Does it actually scale?" Mancuso asked. Before answering, Mancuso explained what is different about doing RL on Modal.

![Why Modal for agent RL?](../slides/10-14-1405-scaling-agentic-rl-across-the-globe/17.jpg)
*Only the first card, "Training clusters," was photographed.*

Modal does training, and "just released clusters as GA last week or week before" (secondary reports put general availability on October 1). It runs inference at scale behind many endpoints, and agent environments run in Modal Sandboxes. So Modal has all the infrastructure primitives to compose a full production agent RL system. "The only problem is that our capacity model is very different compared to other people."

![Modal's capacity model](../slides/10-14-1405-scaling-agentic-rl-across-the-globe/19.jpg)
*GPUs scattered across providers and regions; a 4×B200 request fits, a 16×B200 request doesn't.*

The slide showed three providers with two regions each, holding different numbers of B200, H100, A100, H200, B300 and RTX 6000 GPUs. A requests panel showed that "4 × B200 fits" while "16 × B200" has "no fit," and noted "RL: 2:1 inference to trainer GPUs." The slide shows three providers, but "in reality, it's like 15 or 20 at this point," Mancuso said, including all the hyperscalers and many neoclouds, with Modal handling GPU health and reliability. That is good for reliability and for fast, flexible scheduling, "but it also fragments our compute pool."

Modal can schedule clusters of four or eight nodes fairly quickly, but at 16 or 32 nodes "it's pretty painful, it takes a really long time to find a contiguous block." That hurts RL in particular: agent RL typically needs "two inference GPUs for every training GPU," so about two-thirds of a cluster does inference, and in an eight-node cluster only two or three nodes might be training.

## 8. Sparse weight updates let inference leave the cluster

![The weight update is super sparse](../slides/10-14-1405-scaling-agentic-rl-across-the-globe/21.jpg)
*Weights at step t+1 minus weights at step t: "diff: mostly zeros."*

The fix is to move the inference GPUs out of the training cluster. What makes that possible, Mancuso said, is that RL weight updates "end up being incredibly sparse" (Mancuso said there were two reasons, but the recording does not capture what they were). The slide subtracted the weights at step t from those at step t+1 to get a diff of "mostly zeros." Although the full weights are dense and large, Mancuso said the update is "100x or 1,000x as sparse," so off-the-shelf compression cuts the data sent from trainer to rollout engines "by a factor of a thousand." Those figures were given only verbally; they are not on the slide, in the blog post or in the Stitch README.

"For the run that I mentioned of Qwen3.6, we ran the training cluster and…" The author's recording ends there, at about 15 minutes.

## 9. The final slides (photos only, no audio)

The remaining slides were photographed but not recorded, so what Mancuso said over them is not known. Several slides in this stretch were not photographed, and none of the photographed slides covers Stitch directly, so whether the talk presented Stitch by name is not confirmed.

![Stable training despite massive mismatch](../slides/10-14-1405-scaling-agentic-rl-across-the-globe/22.jpg)
*Training reward, sampler-trainer KL, gradient norm and entropy for SC + MIS + top-p on mixed and homogeneous samplers.*

"Stable training despite massive mismatch" showed four small charts (training reward, KL between sampler and trainer per token, gradient norm and entropy) for SC + MIS + top-p on the mixed and on the homogeneous samplers. In the KL chart the two lines sat clearly apart, mixed higher; in the other three they nearly overlapped. That matches the blog: the mixed samplers' KL averaged 4.7 times higher, yet pass@4, reward and gradient norm stayed close.

![Many GPU types, many precisions](../slides/10-14-1405-scaling-agentic-rl-across-the-globe/23.jpg)
*Share of requests served by each GPU type and precision over training.*

"Many GPU types, many precisions" charted the share of requests served over training by eight sampler groups: A100 (BF16, FlashInfer), RTX PRO 6000 (BF16, FlashInfer), H100 (FP8, FA3), H100 (BF16, FA3), H200 (FP8, FA3), H200 (BF16, FA3), B200 (NVFP4, TRT-LLM) and B300 (NVFP4, TRT-LLM). The per-row percentages are not legible in the photo. The blog gives two: H100 in FP8 produced 10% of samples, and B200 plus B300 in NVFP4 produced 47% together. It also says that for most of the run, the mixed samplers cost about 15% less per update than the homogeneous ones, with room for more savings.

![Competitive agents need the full stack](../slides/10-14-1405-scaling-agentic-rl-across-the-globe/24.jpg)
*Agent harness, model and infra: the closing slide repeats the three layers from the start.*

The last photographed slide, "Competitive agents need the full stack," repeated the three layers from the opening "own your intelligence" card: agent harness, model and infra.

Stitch, the framework named in the abstract, is described in its README as "the versioned control plane for disaggregated reinforcement learning." The trainer publishes policy versions to a store; requests can demand a minimum or exact policy version, and a replica that can't serve it returns a retryable `409`; replicas prefetch and verify the next full checkpoint or delta while still serving, pause briefly to activate it, and join rotation only once caught up. "Version bytes become durable before the shared pointer advances." It does not prescribe the training algorithm, inference engine, storage or compute provider.

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=e6ccda8d-0613-44f8-b98f-53ba8037b518) · [Stitch](https://github.com/modal-projects/stitch) · [Toward RL on mixed-precision, mixed-accelerator samplers](https://modal.com/blog/toward-rl-on-mixed-precision-mixed-accelerator-samplers) (Nan Jiang, Modal) · [DeepSeekMath (GRPO)](https://arxiv.org/abs/2402.03300) · [Ring-1T / IcePop](https://arxiv.org/abs/2510.18855) · [Score Centering](https://arxiv.org/abs/2609.20807) · [DeepSeek-V3.2](https://arxiv.org/abs/2512.02556) · [No Token Left Behind](https://www.lmsys.org/blog/2026-05-13-no-token-left-behind) (LMSYS) · [IMPALA](https://arxiv.org/abs/1802.01561) · [Taking Reinforcement Learning Cross Datacenter](https://www.youtube.com/watch?v=maRzp4kImJ4) (Nan Jiang's August talk, background)

No video of this session had been published at the time of writing. This write-up is based on the author's room recording, which stops mid-sentence at about 15 minutes, slide photos, and the papers, blog post and repository above. The last few slides are described from photos only, and the recording ends without a Q&A; whether one followed is not known. The [precise notes](precise/10-14-1405-scaling-agentic-rl-across-the-globe.md) (Chinese) keep timestamps and the full list of unverified details.
