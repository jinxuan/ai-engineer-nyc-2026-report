[English](../10-13-1040-can-any-model-write-like-me-building-a-b.md) | **中文**

# 衡量 slop，量的是语气，不是禁用词

*在 AI Engineer NYC 2026 上，Towards AI 的 Louis-François Bouchard 发布了 ToneBench。这个 benchmark 给模型打分，看它能不能用 Bouchard 自己的语气写一篇视频脚本。Bouchard 还讲了做这个 benchmark 花了多少钱，以及它让人对用 AI 写作有了哪些认识。*

**Breakout（Evaluations track）· 2026 年 10 月 13 日（周二）· 10:40–10:58am · Turing Room (Riverside Ballroom)**
**讲者：** Louis-François Bouchard，CTO 兼联合创始人，Towards AI（运营 What's AI YouTube 频道；*Building LLMs for Production* 合著者）

---

Louis-François Bouchard 开场放了满满一墙读者已经不再信任的词：delve、tapestry、pivotal、paradigm-shifting、harness、「the real magic happens」「In today's fast-paced world」「It's not just X, it's Y」。上面压着一句引语，幻灯片注明出自 Andrej Karpathy，2026 年 1 月：「I am bracing for 2026 as the year of the slopacolypse across all of github, substack, arxiv, X/instagram, and generally all digital media.」（我已经做好准备，2026 年将是 slop 末日之年，席卷 github、substack、arxiv、X/instagram，以及几乎所有数字媒体。）下面是橙色大字：「Tired of the slopacolypse? Let's stop slop.」（受够了 slop 末日？一起来终结 slop。）

![Let's stop slop](../../slides/10-13-1040-can-any-model-write-like-me-building-a-b/01.jpg)
*开场幻灯片：「slopacolypse」引语压在一团 AI 味短语上。*

「Karpathy 说的 slop 末日，他认为会从 2026 年开始，我想我们已经在社交媒体上看到了。我想试着帮忙解决它，」Bouchard 说，「要解决、要终结 slop，首先得给它下定义。」

演讲分五部分：为什么还要一个 benchmark、它怎么运作、花了多少钱、学到了什么，以及「轮到你了」。Bouchard 宣布这个 benchmark，ToneBench，当天发布。

## 1. Slop 是什么

![What is slop?](../../slides/10-13-1040-can-any-model-write-like-me-building-a-b/02.jpg)
*六个外部给出的 slop 定义，中间横着一行「It s\*cks to read it」。*

第二张幻灯片收集了一些人和机构给这个词下的定义，幻灯片上注明的来源有 Simon Willison（「让这个词流行起来的人」，2024 年 5 月）、Merriam-Webster（2025 年度词汇）、Oxford University Press（2024 年度词汇候选）、swyx（Latent Space，2026 年 1 月），以及 Samuel Paech 等人（Antislop 论文，2025 年 10 月）。Merriam-Webster 确实把 slop 选为 2025 年度词汇，它的定义是：「通常借助人工智能大量生产的低质量数字内容。」Bouchard 自己的总结印在幻灯片中间。「我想我们都同意的一点是，读一篇感觉是生成出来的东西，真的很难受，」Bouchard 说。

![Let's delve deep](../../slides/10-13-1040-can-any-model-write-like-me-building-a-b/03.jpg)
*「Let's delve deep into the intricacies of Agentic AI most companies miss..」，署名「LLM」，2023、2024、2025 都被划掉。*

这张玩笑幻灯片想说的是，这种文风并没有过时。「即使模型越来越好，有了 Fable、GPT-6.1 这些更好的模型，slop 好像也没有消失，反而一直在规模化，」Bouchard 说，「看来我们需要的不只是把模型训练得更多。」

![What slop is made of](../../slides/10-13-1040-can-any-model-write-like-me-building-a-b/06.jpg)
*Bouchard 列出的 slop 六种成分，以及这个项目的目标。*

接着 Bouchard 把 slop 拆成六个部分：

| # | 成分 | 幻灯片上的例子 | 含义 |
| --- | --- | --- | --- |
| 1 | Overused words | 「delve」「tapestry」「seamlessly」 | 一听就是生成的：有些词的出现频率是人类写作的 1,000× |
| 2 | Too general | 「Most teams…」「Most projects…」 | 宽泛的论断，没有具体内容 |
| 3 | Says nothing | 「In today's fast-paced world…」「unlock the real power of AI」 | 词很好听，信息很少 |
| 4 | Repeats itself | 「In other words…」「To recap…」 | 同一个观点翻来覆去 |
| 5 | Hallucinations | 「A 2023 Stanford study found…」 | 不真实的事实 |
| 6 | Wrong tone | 「Buckle up!」「We're thrilled to announce…」 | 听起来不像任何人，或者像新闻稿 |

第二条 Bouchard 自己也认了：「我自己在 LinkedIn 上也犯过，大概这就是 LinkedIn 的玩法吧。」Bouchard 认为最糟的是最后一条：slop「通常语气不对，或者至少没有语气，像是所有人的语气混在一起。所以它根本不像你在说话。」

这引出了印在幻灯片底部的目标：「measure sloppy writing, not just slop words」（衡量写得 sloppy 的文字，而不只是 slop 词）。Bouchard 说：「我不想重做一个 EQ-Bench 的 slop score。我真正想衡量的是一篇文字写得有多 sloppy。」

## 2. 为什么还要再做一个 benchmark

![So we built ToneBench](../../slides/10-13-1040-can-any-model-write-like-me-building-a-b/07.jpg)
*「So we built ToneBench. A benchmark that measures creative writing.」*

![The plan](../../slides/10-13-1040-can-any-model-write-like-me-building-a-b/08.jpg)
*演讲计划：为什么还要一个 benchmark、它怎么运作、成本、学到了什么、轮到你了。*

Bouchard 给出了这个领域还需要一个 benchmark 的三个理由。第一是社交媒体的现状：「看看 LinkedIn 和 X 上的回复，我在 X 上收到的回复，我觉得确实需要。大家在刷越来越多生成的内容。那至少让它们写得像样一点。」

第二是自己的需要。Towards AI 是个小团队，教育业务产出大量 AI engineering 内容，也用 AI 帮忙。「一直要审这些 slop、想办法改，烦透了，」Bouchard 说，「我发了几百个视频。我们发了几千篇文章。我们出了一本书，正在写第二本。」

![We write for a living, so I care](../../slides/10-13-1040-can-any-model-write-like-me-building-a-b/10.jpg)
*Towards AI 的出版规模，右边是第二本书。*

幻灯片给这些规模配上了数字：2019 年以来教过超过 500,000 人，公司 Discord 有超过 100,000 名成员；2020 年以来 74.2K YouTube 订阅、734 个视频；Towards AI publication 有 150K 关注者和数千名作者；第一本书 *Building LLMs for Production* 卖出超过 10,000 本，评分 4.4 星；Towards AI Academy；以及第二本书 *AI Engineering for Production*，「coming soon」。

第三是需求：Bouchard 在做这个项目时在 X 上分享过，反响说明「我们确实需要这样一个更偏创意写作的 benchmark」。

![Building evals is also our job](../../slides/10-13-1040-can-any-model-write-like-me-building-a-b/11.jpg)
*Bouchard 的 evaluation 背景和 ToneBench 背后的团队。*

最后，Bouchard 认为这个团队适合做这件事。幻灯片上的时间线从一家创业公司的 Head of AI（2020–2022，同时读计算机视觉硕士、创办 Towards AI），到 Mila 的博士研究（2022–2024，「跑 evaluation 是大部分工作」），到 Towards AI 为客户做 AI 方案、包括定制 eval pipeline（2022 年至今），再到 2026 年 6 月的 ToneBench。公司有 15 名为企业构建和部署 AI 的 AI 工程师，以及四名专职技术写作和编辑。「我从硕士、博士开始就在评估 AI 模型。现在在 Towards AI，我们也为客户搭 evaluation pipeline，」Bouchard 说。

## 3. 写手：模型能写得像我吗？

「我们做的东西非常主观，」Bouchard 说，「它衡量的是一个模型能不能写得像我。我的定义很简单：我能不能像现在这样把它念出来，而不会因为里面有我自己不会用的词而尴尬。」

![First, the writer](../../slides/10-13-1040-can-any-model-write-like-me-building-a-b/12.jpg)
*Brief 和 research 交给一个模型，它用 Bouchard 的语气写一篇完整的视频脚本。*

任务的轮廓很简单。模型拿到一份 brief 和 research，加上 Bouchard 的语气指南和讲故事指南，要写出一篇 1,000 到 5,000 词的完整视频脚本。「当然，做不到。通常写得很烂，但这正是目的，」Bouchard 说。

![What the writer model sees](../../slides/10-13-1040-can-any-model-write-like-me-building-a-b/13.jpg)
*写手模型的输入，按部分列出，以及它的输出。*

真正的 prompt 比一份 brief 长得多：

| 输入部分 | 作用 | 词数 |
| --- | --- | --- |
| Voice guide | how I sound | 2,626 |
| Storytelling guide | hooks, pacing, cues | 3,243 |
| The contract | hard requirements | 1,045 |
| The brief | what to write | 693 |
| The research | the facts | 673 |

输出是一篇约 2,000 词的完整脚本，用 `[SHOW:]` 标记视觉提示。Bouchard 说，指南里有「大量关于我怎么说话、怎么写作的细节，还有一大堆我以前视频内容的例子」，以及关于讲故事、主动句的通用材料，「也就是怎么把东西写好」。

![Ten of my own scripts](../../slides/10-13-1040-can-any-model-write-like-me-building-a-b/14.jpg)
*十个 benchmark 任务，在类型、长度、research 和语气上刻意错开。*

一共十个任务，每个基于 Bouchard 自己的一篇脚本，各有自己的 rubric：

| 维度 | 分布 |
| --- | --- |
| 视频类型 | News & opinion 3 · Technical explainer 3 · Career guide 2 · Announcement 2 |
| 长度 | Under 10 min 3 · 10 to 15 min 4 · 15 to 28 min 3 |
| 提供的 research | Short 2 · Medium 4 · Long (up to 7,500 words) 4 |
| 语气 | Personal story 4 · Analysis and teaching 6 |
| 是否发布过？ | 一部分在 YouTube 上，一部分从未发布（具体数量在照片里被挡住） |

「有些发布过，有些没有。我们不说哪些是 benchmark 用的，这样可以检查有没有污染（contamination），」Bouchard 说。据 ToneBench 的 methodology 页面，这些脚本的口播词数在约 885 到 4,260 之间，一半来自已发布的视频，一半未发布，先由六名 Towards AI 成员审过，再由内容负责人做最终审核。

## 4. 整个系统：五份草稿，四个 judge

![The whole system](../../slides/10-13-1040-can-any-model-write-like-me-building-a-b/15.jpg)
*每个模型对每篇脚本写五份草稿；四个 LLM judge 打分；分数汇总成 leaderboard。*

幻灯片上的 pipeline 对每个模型和 thinking 级别各跑一遍（「× 127 models and thinking levels (for now)」），对每篇脚本各跑一遍（× 10）。一个模型，比如 max effort 的 Sonnet 5.5，生成五份草稿。四个 LLM judge 给每份草稿打分：Claude Opus 5.5、GLM-5.3 Flash、GPT-6.1 和 Jev (TypeSafe)。结果先是每篇脚本一个分数（来自九个指标），然后是跨十篇脚本、九个指标的模型分数，最后是按两两对比算出的 Elo 排名的 leaderboard。

Jev 是新加的。「我最近把 Jev 加进来做个测试。不确定会不会留着，只是想看看效果好不好，」Bouchard 说。据 methodology 页面，Jev 1.13 (TypeSafe) 是一个「typed judge」，对每个指标回答一道固定的选择题，不写理由，输出五个质量等级上的概率，再映射到 0–100。页面还说四个 judge 权重相同；Elo 用 1500 作为初始分，K=16，400 分的 logistic 尺度，对 25 种排列取平均，用 200 次 bootstrap 算 95% 区间，并用 Bradley-Terry 交叉验证；Writing Score 是绝对分（0–100），Elo 是相对分。

![Why 5 drafts?](../../slides/10-13-1040-can-any-model-write-like-me-building-a-b/16.jpg)
*三个模型对同一篇脚本各写五份草稿的得分：质量越低，分布越散。*

为什么是五份草稿？「是为了检查模型内部的波动，如果你在搭自己的 evaluation，我非常建议这么做，因为尤其是创意类任务，有时候就像这里的 Grok 4.7，有些生成结果是离群值，」Bouchard 说。幻灯片（照片里左边缘被裁掉了）对比了三个模型：

| 模型 | 波动 | 最佳草稿的排名 | 五份全算的排名 | 最差草稿的排名 |
| --- | --- | --- | --- | --- |
| Claude Opus 5.5 (max) | ±0.9 | #1 | #1 | #3 |
| Grok 4.7 (high)，分成两簇 | ±3.4 | #5 | #29 | #67 |
| Nemotron 3 Ultra (reasoning)，「all over the place」（东一个西一个） | ±5.0 | #111 | #137 | #148 |

（前两个名字在幻灯片上被部分裁掉，根据 Bouchard 的口述补全；第三个是根据露出来的「…tron 3 Ultra」推断的。）Bouchard 说，五份里有一份，Grok 4.7「和 Opus 5.5 一样好」。总体上，「模型越差，方差越大」。五份草稿「在我们执行的成本和衡量这种波动之间，似乎是个不错的折中」。

## 5. 衡量什么

![What we measure](../../slides/10-13-1040-can-any-model-write-like-me-building-a-b/19.jpg)
*九个带权重的指标，归成五个方面。*

「写作质量到底怎么量化？这一直是难点，」Bouchard 说。ToneBench 用九个权重固定的指标：

| 指标 | 权重 | 打分方 |
| --- | --- | --- |
| Tone & voice | 19% | judges |
| Substance & accuracy | 15% | judges |
| Continuity & emotion | 14% | judges |
| Writing craft | 13% | judges |
| YouTube craft | 12% | judges |
| Hook strength | 10% | judges |
| Length | 8% | code |
| Slop | 5% | code + judge |
| Visual cues | 4% | judges |

Bouchard 把它们归成五个方面。第一，特定的语气：听起来像不像我，也就是模型能不能适应某一个人的语气？第二，讲故事，或者说纯粹的「质量」：hook、情绪、节奏、清晰度，以及「真实的例子而不是泛泛而谈」。第三，指令：预期的长度和格式，视觉提示放在对的地方。第四，research 准确性：草稿有没有用上给它的 research，在有帮助的地方补充自己的知识，并且不 hallucinate。第五，slop，「用 EQ-Bench slop score 来衡量，也加上我们自己的测量」。

## 6. Judge 怎么打分

![What every judge sees](../../slides/10-13-1040-can-any-model-write-like-me-building-a-b/20.jpg)
*Judge 拿到的材料包，按部分列出，以及永远不给它看的东西。*

每个 judge 拿到一份很长的材料包。各部分及幻灯片上的词数：

| 材料包部分 | 词数 |
| --- | --- |
| Rules, and what the packet holds | 449 |
| 5 score levels, how to pick a number | 402 |
| Tone & voice | 1,418 |
| Writing craft | 1,421 |
| Substance & accuracy | 1,344 |
| Hook | 872 |
| YouTube best practices | 1,054 |
| Continuity & emotion | 894 |
| Anti-slop & number reframing | 1,165 |
| Visual cues | 855 |
| Evidence first, reply format | 241 |
| The brief | 693 |
| The research (the fact source) | 673 |
| My final script (the top of the scale for voice) | 1,390 |
| The draft (being judged) | ~1,300 |
| Computed stats (counted by code: cues, listed phrases, the opening) | – |

在这个任务上，每次评判大约 14,000 词，最长的任务大约 25,000 词。有两样东西永远不给 judge 看：草稿是哪个模型写的，以及预期的词数，因为长度由代码打分。「Judge 只分析写作质量。它不管长度，也不知道是哪个模型写的，」Bouchard 说。Bouchard 自己的定稿也放在里面，作为语气维度的满分标杆。（幻灯片上还有一行以「25,400 times」开头的内容，在照片里被挡住了。）

![Example with one metric: tone and voice](../../slides/10-13-1040-can-any-model-write-like-me-building-a-b/22.jpg)
*Tone-and-voice 的 rubric：四个信号、六个分数段，以及打过分的片段。*

主要指标 tone and voice 展示了每个 rubric 是怎么运作的。Judge 统计四个信号，每个都有很长的描述：第一人称（真实的经历或判断，每段最多一处，分散开）；对主要论断的反应（有多少论断带上了 Bouchard 的看法，而不只是转述）；自己的判断（Bouchard 会用什么、避开什么、信任什么、先测什么）；以及语气违规（公关腔、炒作、戏剧化或刻意装随意）。然后先选一个分数段，再给具体分数：

| 分数 | 分数段描述 |
| --- | --- |
| 90–100 | 3 个以上部分有第一人称，80%+ 的论断有反应，没有语气违规 |
| 80–89 | 分布较散，65–79% 有反应，1–2 处孤立的失误 |
| 70–79 | 不均匀，50–64%，或者某个语气问题反复出现 |
| 50–69 | 浅，25–49%：任何一个 AI 教育者都能说出来 |
| 25–49 | 很少，低于 25%，个性标记占主导 |
| 0–24 | 没有第一人称，也没有判断 |

Rubric 里还带了打过分的片段。Bouchard 自己的一句得了 95 分：「I use coding agents daily… I love this stuff. But I would not drop a random cheap proxy between my agent and any repo I care about.」一份模型草稿得了 66 分：「I've watched agents rewrite the same helper function four different ways across 18 iterations.」另一份提到和 Addy Osmani 的一期播客的草稿得了 40 分。

「Judge 会先在六个类别里归类，找到类别之后再给出评分，」Bouchard 说。团队把这种做法和其他方案比较过，「就是我们平常建议用的二元打分，或者直接分类，或者直接给百分比。我们发现这种做法实际效果非常好。」幻灯片的脚注也这么写：它「worked best vs. direct % or binary or only categories」（比直接给百分比、二元或只分类效果都好）。

![What the judges send back](../../slides/10-13-1040-can-any-model-write-like-me-building-a-b/24.jpg)
*一个 judge 在同一份 brief 上给三个模型的分数，以及它对其中一个指标的回复。*

对每个指标，judge 返回它标出的一句引文、一个分数和一句理由。幻灯片展示了一个 judge 在同一份 brief 上给三个模型的打分：

| 指标 | 权重 | Opus 5.5 | GPT-6.1 Sol | Haiku 5.5 |
| --- | --- | --- | --- | --- |
| Tone & voice | 19% | 86 | 76 | 73 |
| Substance & accuracy | 15% | 88 | 85 | 85 |
| Continuity & emotion | 14% | 88 | 77 | 75 |
| Writing craft | 13% | 85 | 73 | 71 |
| YouTube craft | 12% | 86 | 84 | 80 |
| Hook | 10% | 91 | 86 | 76 |
| Length (code) | 8% | 100 | 97 | 98 |
| Slop (code + judge) | 5% | 89 | 87 | 85 |
| Visual cues | 4% | 86 | 84 | 76 |
| **草稿得分** | | **88.2** | **81.7** | **78.7** |

草稿得分是各指标乘以权重后相加；五份草稿的得分再取平均。（按表格重算，Opus 和 Haiku 是 88.2 和 78.7，GPT-6.1 Sol 是 81.6，比幻灯片低 0.1，大概是四舍五入造成的。）对 Opus 草稿的 tone and voice，judge 标出的是「I'd much rather read a wrong assumption stated clearly than a right one I have to reverse-engineer from your code」，给了 86 分，理由是：「第一人称从头到尾承载着真实的招聘判断（他更愿意读什么、接受什么样的回答、在招什么人）……」

Bouchard 说这一切都是自动化的，因为量太大，人工读不过来：「120 个模型，每个模型在 10 个任务上各生成五份草稿。很快就到了几千篇脚本。」

## 7. 每个 judge 都有偏爱的实验室

![Every AI judge has a favorite lab](../../slides/10-13-1040-can-any-model-write-like-me-building-a-b/25.jpg)
*和其他 judge 相比，每个 judge 给每家实验室的草稿多给或少给了多少分。*

「LLM judge 的问题在于它们有偏好，」Bouchard 说，「有些模型偏爱自己，尤其是 OpenAI。」一张覆盖七家实验室（Anthropic、OpenAI、Z.ai、DeepSeek、xAI、Alibaba、Google）的雷达图显示了每个 judge 最大的偏差：

| Judge | 最偏爱 | 最不偏爱 |
| --- | --- | --- |
| Claude Opus 5.5 | Anthropic +2.3 | OpenAI −2.8 |
| GPT-6.1 Sol | OpenAI +3.9 | Anthropic −2.1 |
| GLM-5.3 Flash | Alibaba +1.4 | OpenAI −1.4 |
| Jev（「no model in the race」，自家没有模型参赛） | Alibaba +3.7 | Google −2.9 |

「研究这个的时候我觉得挺好笑的，Anthropic 和 OpenAI 好像真的在打仗，似乎训练时就学会了互相讨厌，」Bouchard 说。实用的建议是：如果你用一个 LLM judge，比如 GPT，来给自己的任务挑最好的模型，「要小心，用不同家族的模型」，因为「OpenAI 对自家的模型家族有很强的偏好，不只是某一个模型，而是所有 OpenAI 模型。」Bouchard 配套的文章是这么说的：「No judge is neutral. Use a panel, and never let your writer's lab be the only scorer.」（没有哪个 judge 是中立的。用一个评审团，永远不要让写手所在的实验室成为唯一的打分方。）（文章里的数字和幻灯片不一样，比如 GPT judge 对 OpenAI 是 +2.75 而不是 +3.9；文章还说 Jev 略微偏爱 Gemini 模型，和幻灯片上 Google −2.9 正好相反。）

X 上有人建议创意任务应该用人来评，Bouchard 认为这「理想，但很不现实」。团队试了一个能扩展的版本：「目前是我和三个队友手工比了 1,000 组生成脚本的 A/B 对，不是整篇脚本，而是两篇脚本之间可以对比的一大段。」Bouchard 希望以后能向社区开放。

## 8. 花了多少钱

![The cost](../../slides/10-13-1040-can-any-model-write-like-me-building-a-b/26.jpg)
*小时、月份和美元，「and going… (paper coming!)」（还在增加……论文即将发布！）。*

| 项目 | 幻灯片上的数字 |
| --- | --- |
| 启动投入 | ~500 小时：4 个人，一个月（脚本、rubric、衡量什么、A/B 对比） |
| 迭代 | 4 个月，用 3 个 Claude 20x 和 1 个 Codex 20x 订阅，外加几千美元的 OpenRouter 和其他 API 费用 |
| 改一次 rubric | $2,350 重新评判全部内容 |
| 全量刷新 | $3,750 重新生成并重新评判整个榜单 |

Bouchard 说，这 500 小时花在搭十篇脚本、定义 rubric、找例子，以及尝试「量化质量，这非常难」。接下来是四个月的迭代，「用四个不同的 20X 订阅」打磨指标和权重。「只要我改一下 rubric 或者 judge 给生成结果打分的方式，就要花两千多美元。只要改一下模型生成时输入 prompt 里的一行，就要花四千多美元。」（幻灯片上全量重新生成是 $3,750。Bouchard 的文章说启动投入大约 400 小时，而不是 500。）幻灯片最后写着：「Experimenting is expensive. Want to help? Reach out!」（做实验很贵。想帮忙？联系我们！）

## 9. 它让人对用 AI 写作有了哪些认识

![Tips for writing with AI](../../slides/10-13-1040-can-any-model-write-like-me-building-a-b/30.jpg)
*根据 ToneBench 结果，该选哪个模型，以及六个技巧。*

Bouchard 说，最主要的教训来自四个月看结果的经验：「你不是要挑一个能一次生成出惊艳帖子的模型，而是要找一个让你在达到目标的过程中最不烦的模型。」幻灯片是这么写的：「Pick the model that gets you there with the fewest edits, without making you mad. Not the one that writes 'amazing' text.」（挑那个让你改得最少、不把你逼疯就能达到目标的模型，而不是能写出「惊艳」文字的那个。）当前推荐：

| 用途 | 模型 | 排名 | 每篇脚本成本 |
| --- | --- | --- | --- |
| 大规模 | GLM-5.3 Flash | #12 | $0.022 |
| 单篇 | Claude Opus 5.5 xhigh | #1 | $1.003 |

「大规模用的话，GLM 5.3 Flash 极其好、极其便宜。比最好的那个，也就是 Opus 5.5 extra high，便宜 50 倍，」Bouchard 说（按幻灯片的价格算大约是 46 倍）。Bouchard 还补充说，「写作通常不要用 Max」。（「why five drafts」那张幻灯片把 max 的 Opus 5.5 列为 #1；tips 幻灯片写的是 xhigh。各来源在具体排名和价格上不一致，leaderboard 之后也重跑过。）

幻灯片上的六个技巧，Bouchard 逐一讲了：

| # | 技巧 |
| --- | --- |
| 1 | 非常清晰的指示：又长又详细的 skill 和指令。越多越好。 |
| 2 | 该怎么做、怎么写的例子 |
| 3 | 不该怎么做的反例 |
| 4 | 你过去最好的作品，完整给出 |
| 5 | 让另一个 judge LLM 审一遍，再根据反馈修改。这很有帮助（最多审 1–3 次）。 |
| 6 | （最便宜的最佳方案）多写几份草稿，尤其用便宜的开源模型，再让一个聪明的、高 thinking 的 judge 挑出或合并最好的 |

关于 prompt：「怎么长都不嫌长，越多越好。」把每个该怎么做的例子、每个反例都加进去，「你不想要的词，全部」。如果你写 newsletter，「可以整篇给它，效果就是会更好。」

关于审稿：用一个高 thinking 的 judge，「就一次，一到三次，不然会循环进 slop 里。」让另一个模型用同样的指令审一遍、把反馈给写手，「帮助很大」，不过「我没有这方面的量化指标」。（Bouchard 的文章说，超过两次审稿就看不出多少帮助了。）

对于通过 API 工作的公司，Bouchard 建议用 GLM-5.3 Flash 这类便宜模型生成十份左右的草稿，再让 Opus 这类更聪明的高 thinking judge「挑出并合并每份草稿里最好的部分」，这样「仍然比让 Opus 5.5 生成一次便宜得多得多」，而且按 Bouchard 的经验效果更好。

最后：「你需要人工审核。这一步绕不过去。」

![This measures my voice](../../slides/10-13-1040-can-any-model-write-like-me-building-a-b/31.jpg)
*收尾幻灯片。*

Bouchard 最后提醒了一句：「这个 benchmark 衡量的显然是我的语气，所以非常主观，只是一个数据点。不要完全盲目地信它。这不代表 Opus 5.5 就是最好的模型。」最后一张幻灯片写着：「This measures my voice. What about yours? One data point, not the truth. A strong signal for writing in a voice.」（这衡量的是我的语气。你的呢？一个数据点，不是真理。对用某种语气写作来说，是一个很强的信号。）Bouchard 鼓励听众搭自己的 evaluation pipeline，表示愿意帮忙，并介绍了此刻已经上线的网站，上面有图表、对比和完整的 methodology。

---

**链接：** [Schedule](https://ai.engineer/nyc/2026/schedule?session=736edd83-bb08-49f1-90ed-722b7ed07eb2) · [Video: Why AI Writing Still Reads Like Slop](https://www.youtube.com/watch?v=_dTF3NpPzGo) · [ToneBench](https://benchmark.towardsai.com/) · [ToneBench methodology](https://benchmark.towardsai.com/methodology.html) · [What 9,050 AI-Written Scripts Taught Me About Slop](https://dev.to/dailycontext/what-9050-ai-written-scripts-taught-me-about-slop-1g5n)（Bouchard 的文章）· [Merriam-Webster's 2025 word of the year (AP)](https://abc7news.com/post/merriam-websters-2025-word-year-is-slop/18288780/)

测试过的模型数量因来源和时间而异：schedule 上写 148，系统幻灯片写 127「for now」，Bouchard 在台上说 120，文章统计了 181 个配置和 9,050 篇脚本，methodology 页面在 10 月 23 日重跑后更新为 136 个配置。Methodology 页面现在还写明人工 arena 已经关闭。视频以掌声结束，没有 Q&A。本文依据官方视频字幕、作者拍的幻灯片照片和上面列出的资料（作者现场的录音无法使用）。[精确笔记](../precise/10-13-1040-can-any-model-write-like-me-building-a-b.md)（中文）保留了时间戳，并完整记录了演讲、幻灯片和文章之间不一致的地方。
