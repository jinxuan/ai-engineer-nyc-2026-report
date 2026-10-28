# Can Any Model Write Like Me?（ToneBench）：精确版

> Precise notes for this session, compiled from the official video captions where available, the author's room recording, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材整理完、数字逐一对过 slide 和讲者文章。**没有 Q&A**（视频在掌声处结束，现场 18 分钟的 slot 讲满）。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| 录音 / transcript | 有录音（take 1，约 18.5 分钟），但 Whisper 转写**基本不可用**：开头几句是乱码，[07:00]–[09:01] 和 [15:01] 起是重复幻觉（"I don't know what I'm going to tell you about"、"Speakers & I'm a member of the team"）。本文不引用 |
| 视频 | 有：[Why AI Writing Still Reads Like Slop — Louis-François Bouchard, Towards AI](https://www.youtube.com/watch?v=_dTF3NpPzGo)，2026-10-22 上传，19:02。自动字幕是正文主要来源（"slop" 被识别成 "slub"/"slap"，见勘误表）。视频在 [18:55] 掌声处结束，无 Q&A |
| slide | 作者拍的 31 张（IMG_0330–0361，缺 0341），全部属于本场，没有上一场的照片。讲者用 Google Slides 放映，标签页名 `ToneBench-AIE-NYC`，页脚 `benchmark.towardsai.com` |
| 讲者文章 | [What 9,050 AI-Written Scripts Taught Me About Slop](https://dev.to/dailycontext/what-9050-ai-written-scripts-taught-me-about-slop-1g5n)（DEV Daily Context，会前发布）。数字和 slide 有多处不一致，见「出入」 |
| 其他一手资料 | [ToneBench 首页](https://benchmark.towardsai.com/)、[Methodology](https://benchmark.towardsai.com/methodology.html)（**2026-10-23 更新**，已是会后重跑的版本，数字和现场不同）；YouTube 视频描述与章节；会议日程页的 session 简介 |

## 基本信息

- 时间地点：2026-10-13 10:40–10:58am，Turing Room (Riverside Ballroom)，Breakouts，Evaluations Track
- 讲者：Louis-François Bouchard，Towards AI 的 CTO & Co-Founder；运营 What's AI YouTube 频道；合著 *Building LLMs for Production*（资料：session.json 里的日程简介）
- 视频标题被改成「Why AI Writing Still Reads Like Slop」；章节（资料，视频描述）：00:00 What is slop? / 02:41 Measuring sloppy writing / 03:49 Why build another benchmark / 05:44 Can a model write like me? / 07:19 10 scripts, 5 drafts, 4 judges / 09:14 What gets measured / 11:10 How the judges score / 12:38 Judges prefer their own models / 13:46 Human A/B checks / 14:15 What it cost / 15:20 What we learned / 16:21 How to write better with AI
- 讲者说 [03:01]：「we are … today right now releasing a new benchmark that we called Tone-Bench」；[18:35]「it's live. We just like right now published it」
- slide「PLAN」：1 Why another benchmark → 2 How it works（what we measure, and how）→ 3 The cost（hours and $$）→ 4 What we learned（writing tips you can use now）→ 5 Your turn!

## 1. 什么是 slop

- slide（开场）：引语「I am bracing for 2026 as the year of the slopacolypse across all of github, substack, arxiv, X/instagram, and generally all digital media.」署名 **Andrej Karpathy, January 2026**；大字「Tired of the slopacolypse? Let's stop slop.」背景是一墙 AI 腔词（delve、tapestry、"It's not just X, it's Y"、In today's fast-paced world、pivotal、paradigm-shifting、unlock the real power、the real magic happens、harness 等）
  - 讲者说 [00:20]：「what Karpathy termed as the slop apocalypse that he thought would start in 2026」
  - **【待确认】** Karpathy 这条原话的出处没找到（搜索无结果），只能标为 slide 引用
- slide「What is slop?」列了六条外部定义：Simon Willison（"who made the word stick, May 2024"）、Merriam-Webster（Word of the Year 2025）、Oxford University Press（Word of the Year shortlist, 2024）、swyx（Latent Space, January 2026）、Samuel Paech et al.（the Antislop paper, October 2025），中间大字「It s*cks to read it」
  - 资料：Merriam-Webster 2025 年度词是 slop，2025-12-15 公布，释义是「digital content of low quality that is produced usually in quantity by means of artificial intelligence」（[AP 转载](https://abc7news.com/post/merriam-websters-2025-word-year-is-slop/18288780/)）；与 slide 上的引文一致
  - 其余几条（Willison、OUP、swyx、Antislop 论文）没有逐条核对原文，【待确认】
- slide「Let's delve deep into the intricacies of Agentic AI most companies miss..」署名「LLM，~~2023 2024 2025~~ 2026」
- slide「What slop is made of」六条：

| # | 类别 | slide 上的例子与说明 |
| --- | --- | --- |
| 1 | Overused words | "delve" · "tapestry" · "seamlessly"；they sound generated: some show up 1,000× more often than in human writing |
| 2 | Too general | "Most teams..." · "Most projects..."；broad claims, nothing specific |
| 3 | Says nothing | "In today's fast-paced world..." · "unlock the real power of AI"；nice words, very little information |
| 4 | Repeats itself | "In other words..." · "To recap..."；the same point, again and again |
| 5 | Hallucinations | "A 2023 Stanford study found..."；facts that aren't true |
| 6 | Wrong tone | "Buckle up!" · "We're thrilled to announce..."；it sounds like nobody, or like a press release |

- slide 底部：「My goal: measure **sloppy writing**, not just slop words.」
- 讲者说 [01:07]：模型在变强（「fable, GPT-6.1」），slop「doesn't seem to go away. It just keeps on scaling」；[02:50]「I don't want to rebuild EQ-Bench slop score」

## 2. 为什么要再做一个 benchmark

- slide：「So we built **ToneBench**. A benchmark that measures creative writing.」附二维码
- slide「We write for a living, so I care」：Since 2019: 500,000+ people taught, 100K+ members in our Discord；YouTube **74.2K** subscribers, 734 videos since 2020；**150K** followers, thousands of writers（Towards AI 的发布平台）；**10,000+** copies of our first book, 4.4★（*Building LLMs for Production*）；Towards AI Academy；「2nd book, coming soon」*AI Engineering for Production*（louisbouchard.ai/book）
- slide「Building evals is also our job」：2020–2022 Head of AI at a startup（+ master's in computer vision + creation of Towards AI for education）；2022–2024 PhD research at Mila（where running evaluations was most of the work）；2022–today Towards AI builds for clients（including custom eval pipelines）；**June 2026 ToneBench**。**15** AI engineers who build and deploy AI for companies；**4** specialized technical writers / editors。Partners 一栏有 OpenAI 等 logo；Clients include Europol、J.P.Morgan、New York Public Library、Intel、Veloce IT（logo 识读，【待确认】是否完整）
- 讲者说 [04:26]：「I published hundreds of videos. We published thousands of articles. We released a book. We are working on a second one」；[04:46] 在 X 上边做边分享，反馈说明「we did need some sort of more creative writing benchmark」

## 3. 怎么测：writer 侧

- 定义（讲者说 [05:54]）：「It's a way to measure if a model can write like me … if I can read it out loud like right now without cringing」
- slide「First, the writer」：Brief + research（what to write, and the facts）→ One model（with my voice and storytelling guides）→ A full video script（between 1000–5000 words. In my voice.）
- slide「What the writer model sees, and what it writes」，输入各部分字数：

| 输入 | slide 说明 | 字数 |
| --- | --- | --- |
| Voice guide | how I sound | 2,626 |
| Storytelling guide | hooks, pacing, cues | 3,243 |
| The contract | hard requirements | 1,045 |
| The brief | | 693 |
| The research | | 673 |

输出：One full script，~2,000 words + `[SHOW:]`（视觉提示标记）

- slide「Ten of my own scripts, picked to be different」：Kind of video：News & opinion 3 / Technical explainer 3 / Career guide 2 / Announcement 2；Length：Under 10 min 3 / 10 to 15 min 4 / 15 to 28 min 3；Research given：Short 2 / Medium 4 / Long, up to 7,500 words 4；Voice：Personal story 4 / Analysis and teaching 6；Published?：On YouTube / Never pu[blished]（数量被挡住，【待确认】）
- 讲者说 [07:35]：「Some of them are published, some are not. We don't say which ones are the benchmarks so that we check for contamination」
- 资料（Methodology）：10 个脚本各约 885–4,260 spoken words，一半来自已发布视频、一半未发布；由六位 Towards AI 成员审过，再由 content lead 终审

## 4. 整个系统：5 份草稿、4 个 judge

- slide「The whole system」：× **127** models and thinking levels (for now) × 10 scripts；One model（e.g. Sonnet 5.5 (max)）→ Generate 5×（Draft 1–5）→ **4 LLM judges**：Claude Opus 5.5、GLM-5.3 Flash、GPT-6.1、Jev (TypeSafe) → 1 score per script（from 9 metrics）→ Model score（10 scripts, 9 metrics）→ Leaderboard（ranked by Elo, head to head）
- 讲者说 [08:08]：「we have four different LLM judges … I recently added Jev as a test. Not sure if I will keep that」（字幕为 "Jeff"）
- 资料（Methodology）：Jev 1.13 (TypeSafe) 是「typed judge」，对每个 metric 回答固定的多选题、不写理由，给出五个质量档的概率再映射到 0–100；2026-09-29 加入；线性校准 2026-10-08 冻结。Claude Opus 5.5 与 GPT-6.1 Sol 都是 medium 档，GLM 5.3 Flash 经 OpenRouter；四个 judge 等权平均
- 资料（Methodology）：Writing Elo 起点 1500、K=16、400 点 logistic scale，每个脚本两两比较 Writing Score，25 种对局顺序平均，200 次 bootstrap 给 95% 区间，用 Bradley-Terry 交叉验证；Writing Score 是绝对分（0–100），Elo 是相对分
- **为什么 5 份草稿**（slide「First, why 5 drafts? Measuring intra-model variability」，*five drafts of the same script）：

| 模型（slide 左侧被裁，按可见部分） | 离散 | rank on best drafts | rank on all five | rank on worst drafts |
| --- | --- | --- | --- | --- |
| Claude Opus 5.5 (max) | ±0.9 | #1 | #1 | #3 |
| [Grok] 4.7 (high)，「…usters」（两簇） | ±3.4 | #5 | #29 | #67 |
| [Nemo]tron 3 Ultra (reasoning)，「all over the place」 | ±5.0 | #111 | #137 | #148 |

  - 讲者说 [08:40]：「as we see here with Grok 4.7, some of the generations are outliers … it's just as good as Opus 5.5, but on just one generation out of five」；「the worse the model is, the widest the variance」；[09:05]「five drafts seems to be the good middle ground in terms of cost」
  - 模型名 Grok、Nemotron 是按讲者说和 slide 残字补全，【待确认】

## 5. 测什么：9 个 metric 和权重

- slide「What we measure」，权重条：

| Metric | 权重 | 打分方 |
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

（合计 100%。打分方一列来自后面「What the judges send back」那页的标注和资料 Methodology。）

- slide 把 9 个归成 5 类：1 **A specific tone**: does it sound like me? 2 **Storytelling (pure "quality")**: hook, emotion, rhythm, clarity, real examples instead of generalities 3 **Instructions**: the length, and facts that match the research 4 **Research accuracy**: does it use what we gave it, and add its own knowledge where it helps? 5 **Slop**: our own checks, plus EQ-Bench's slop score
- 讲者说 [10:29]：「we measure for slop using EQ-Bench slop score, but also our own measurements」

## 6. Judge 怎么打分

- slide「What every judge* sees」：Rules, and what the packet holds 449 words / 5 score levels, how to pick a number 402 / Tone & voice 1,418 / Writing craft 1,421 / Substance & accuracy 1,344 / Hook 872 / YouTube best practices 1,054 / Continuity & emotion 894 / Anti-slop & number reframing 1,165 / Visual cues 855 / Evidence first, reply format 241 / The brief 693 / The research（the fact source）673 / **My final script**（the top of the scale for voice）1,390 / The draft（being judged）~1,300 / Computed stats（counted by code: cues, listed phrases, the opening）
  - 右侧：**~14,000** words per judgment on this task, up to ~25,000 on the longest；**Never shown**: which model wrote it; the expected word count (code scores length)；「25,400 times … (as of today)」（后半被挡，【待确认】完整句子）
  - 讲者说 [10:59]：「it's a 14,000 prompt that we send to every judge」；「doesn't know which model is writing」
- slide「Example with one metric: tone and voice」：
  - Four signals the judge counts：**First person**（real experience or judgment, at most one per paragraph, spread out）；**Reactions to major claims**（how many get my take, not just reported）；**Owned judgment**（what I would use, avoid, trust or test first）；**Tone violations**（corporate, hyped, theatrical or forced casual）。箭头注「large (detailed) descriptions!」
  - **Categorize first, then create a range**：90–100 first person in 3+ parts, 80%+ of claims get a reaction, no tone violations；80–89 spread out, 65–79% get a reaction, 1–2 isolated slips；70–79 uneven, 50–64%, or one tone problem recurs；50–69 shallow, 25–49%: any AI educator could say it；25–49 rare, under 25%, personality markers dominate；0–24 first person and judgment effects absent
  - …with scored excerpts：Mine · scored 95「I use coding agents daily… I love this stuff. But I would not drop a random cheap proxy between my agent and any repo I care about.」；Model draft · scored 66「I've watched agents rewrite the same helper function four different ways across 18 iterations.」；（另一份）draft · scored 40「…a podcast with Addy Osmani a[go]…」
  - 底注：「*worked best vs. direct % or binary or only categories」
  - 讲者说 [11:35]：「the judge will categorize between six categories first and after having found a category, it will give a rating … from just binary that we usually suggest to use or directly categories or directly percentage. We found that this actually works really well」
- slide「What the judges send back」（同一份 brief 下三个模型的单 judge 结果）：

| Metric | weight | Opus 5.5 | GPT-6.1 Sol | Haiku 5.5 |
| --- | --- | --- | --- | --- |
| Tone & voice | 19% | 86 | 76 | 73 |
| Substance & accuracy | 15% | 88 | 85 | 85 |
| Continuity & emotion | 14% | 88 | 77 | 75 |
| Writing craft | 13% | 85 | 73 | 71 |
| YouTube craft | 12% | 86 | 84 | 80 |
| Hook | 10% | 91 | 86 | 76 |
| Length · code | 8% | 100 | 97 | 98 |
| Slop · code + judge | 5% | 89 | 87 | 85 |
| Visual cues | 4% | 86 | 84 | 76 |
| **Draft score** | | **88.2** | **81.7** | **78.7** |

  - 「Draft score = each metric × its weight, added up. Then we average the [scores across the] five drafts.」（中间被挡）
  - 一个 judge 对 tone & voice 的回复：1 · one quote it highlighted「I'd much rather read a wrong assumption stated clearly than a right one I have to reverse-engineer from your code.」2 · score 86 3 · one sentence why「First person carries real hiring judgments from open to close (what he'd rather read, what answer he accepts, what he's hiring for)...」× 8 metrics
  - 我的理解（验算）：按权重加总，Opus 5.5 = 88.22、Haiku 5.5 = 78.68，与 slide 一致；GPT-6.1 Sol 算出 81.61，slide 写 81.7，差 0.1（可能是四舍五入前的原始分）
- 讲者说 [12:28]：「with 120 models, we generate five drafts each for 10 tasks. It quickly goes to the thousands of scripts」

## 7. 每个 judge 都偏爱某家实验室

- slide「Every AI judge has a favorite lab」（雷达图，轴是 Anthropic / OpenAI / Z.ai / DeepSeek / xAI / Alibaba / Google；「Points each judge … compared w[ith] … a lab's drafts」，底注部分被挡）：
  - Claude Opus 5.5：Anthropic **+2.3**，OpenAI **−2.8**
  - GPT-6.1 Sol：OpenAI **+3.9**，Anthropic **−2.1**
  - GLM-5.3 Flash：Alibaba **+1.4**，OpenAI **−1.4**
  - Jev, no model in the race：Alibaba **+3.7**，Google **−2.9**
- 讲者说 [12:44]：「some models prefer themselves … particularly OpenAI」；「Anthropic and OpenAI are really in a war here where they seem to have past training on hating each other」；「OpenAI really has a strong preference on its own family of models, not just one model, but all OpenAI models」；建议「use different model family」
- 资料（讲者文章）：「No judge is neutral. Use a panel, and never let your writer's lab be the only scorer.」（文章里的数字与 slide 不同，见「出入」）
- **人工 A/B**：讲者说 [13:50]：「right now we compared manually me and three of my teammates 1,000 AB pairs of the generated scripts, not the full scripts, but like a good chunk that is comparable between two scripts」；以后想请社区帮忙

## 8. 成本

- slide「The cost…」：
  - **~500 h**：4 people, one month: scripts, rubric, what to measure, AB comparisons
  - **4 months** of iterations（箭头标注：3 Claude 20x, 1 Codex 20x + 1000s $ in OpenRouter & other APIs）
  - **$2,350** to re-judge everything after one rubric change
  - **$3,750** to regenerate and re-judge the whole board
  - 「and going... (paper coming!)」；「Experimenting is expensive. Want to help? Reach out!」
- 讲者说 [14:19]–[15:25]：kickoff「around 500 hours」；「four different 20X subscription for 4 months」，「we just spent thousands of dollars there」；「if I just change one thing in the rubrics … it will cost us more than $2,000. And if I just change one line in an input prompt for a model … more than $4,000」

## 9. 学到了什么：用 AI 写作的建议

- slide「Tips for writing with AI, from ToneBench results」：
  - 左框：**Pick the model that gets you there with the fewest edits, without making you mad.** Not the one that writes "amazing" text. Right now: **At scale**: GLM-5.3 Flash · #12 · $0.022 a script；**One piece**: Claude Opus 5.5 xhigh · #1 · $1.003
  - 右侧六条：1 Very clear directions: long, detailed skills and instructions. The more the better. 2 Examples of what to do, and how 3 Negative examples of what not to do 4 Your best past pieces, in full 5 A review by a separate judge LLM, then an edit based on its feedback. This helps a lot. (1–3 reviews max) 6 (cheapest best option) Several drafts, especially with cheap open models + a smart, high-thinking judge picks or merges the best.
  - 底部：「Way more on the site: the best model for your needs and budget, cost vs quality, open vs closed, every thinking level, rankings per metric, the full methodology」→ benchmark.towardsai.com
- 讲者说 [15:43]：「you don't want to pick the model that will generate a one-shot amazing post. You just want to find a model that will be the least annoying to you」
- 讲者说 [16:04]：「at scale GLM 5.3 flash is extremely good, extremely cheap. It's 50 times cheaper than the best one, which is Opus 5.5 extra high … So, you don't want to use Max for writing, usually」（这里中间一句「2.5 times cheaper than Max thinking, but with same results」在字幕里指代不清，【待确认】他比的是哪两个配置）
  - 我的理解（验算）：$1.003 / $0.022 ≈ 46，与「50 times」相符
- 讲者说 [17:01]：「use a high-thinking judge just one time, one to three times, otherwise it will loop into slop」；「I don't have quantitative metrics on that, but it helps a lot」
- 讲者说 [17:24]：公司规模化的做法是用 GLM 5.3 Flash 生成约 10 份草稿，再让 Opus 之类的 high-thinking judge 挑选、合并各份最好的部分，「it will still land way way less expensive than just one generation by Opus 5.5」
- 讲者说 [18:04]：「you want a human review. You cannot really go around that」；「Doesn't mean Opus 5.5 is the best model」
- slide（结尾）：「This measures my voice. What about yours? One data point, not the truth. A strong signal for writing in a voice. We hope it helps everyone write with less slop. Need one on your own tasks? That's what we build at Towards AI.」

## Q&A 精确版

无。视频在 [18:55] 掌声处结束（总长 19:02）；作者录音的转写是幻觉文本，无法确认现场有没有问答，但 18 分钟的 slot 已经讲满（录音 take 时长约 18.5 分钟）。

## 勘误表（字幕 / 转写 → 实际）

| 字幕或转写 | 实际 | 依据 |
| --- | --- | --- |
| slub / slap（多处） | slop | slide |
| the slub apocalypse | the slopacolypse | slide |
| Jeff | Jev (TypeSafe) | slide、Methodology |
| fable | Fable（Claude 模型名） | 上下文 |
| GLM 5.3 flash | GLM-5.3 Flash | slide |
| Opus 5.5 extra high | Claude Opus 5.5 xhigh | slide |
| a 14,000 prompt | ~14,000 words per judgment | slide |
| 1,000 AB pairs of degenerated scripts | of the generated scripts | 上下文 |
| "let's delve in" | 讲者故意的玩笑（delve 是 slop 词） | 上下文 |
| transcript.md 全文 | 不可用（重复幻觉） | 与视频对照 |

## 出入（来源之间不一致的地方）

1. **测了多少个模型/配置**：日程简介 148 models；slide「The whole system」×127 models and thinking levels (for now)，但「why 5 drafts」那页排名到 #148；讲者说 [12:32]「120 models」；讲者文章「181 configurations」、「9,050 scripts」；Methodology（2026-10-23 重跑后）136 configurations、6,800 drafts、27,200 judgments。时间点不同，数字都不同，写文章时要注明引用的是哪个版本
2. **工时**：slide 和讲者说都是约 500 小时（4 人一个月）；讲者文章写「about 400 hours」
3. **订阅和花费**：slide 3 个 Claude 20x + 1 个 Codex 20x + OpenRouter 等「1000s $」；讲者说「four different 20X subscription for 4 months」；日程简介「two 20x subscriptions on Claude and Codex, and $80 in API credits」（简介说的是「benchmark 148 个模型」那两周，口径不同）
5. **judge 偏向的数字**：slide 上 GPT-6.1 Sol 给 OpenAI +3.9、Anthropic −2.1；讲者文章写 GPT judge 给 OpenAI +2.75、Anthropic −2.14。slide 上 Jev 对 Google −2.9，文章却说 Jev「a small preference for the Gemini models」，方向相反。文章还提到一个「DeepSeek judge」，但 slide 和 Methodology 的第三个 judge 是 GLM-5.3 Flash。【待确认】文章是否基于更早的 judge 组合
6. **Opus 5.5 哪一档最好、多少钱**：slide「why 5 drafts」是 Claude Opus 5.5 (max) #1；tips 页是 Opus 5.5 xhigh · #1 · $1.003；讲者文章：max 90.8 分 $2.75/篇、xhigh 90.0 分 $0.69/篇；Methodology 现在 xhigh 85.8 分 $1.003/篇、榜上没有 max。xhigh 的单价（$0.69 vs $1.003）互相矛盾，【待确认】
7. **GLM-5.3 Flash**：slide #12、$0.022/篇；文章 88.2 分、1.5 美分/篇，「Opus 5.5 at max costs nearly 190 times more」；讲者说「50 times cheaper than … Opus 5.5 extra high」。另外现在的 ToneBench 首页上 GLM-5.3 Flash 只作为 judge 出现，不在排名里（WebFetch 摘要，【待确认】）
8. **review 轮数**：slide「1–3 reviews max」，讲者说「one to three times」；讲者文章「Beyond two, I haven't seen much help」
9. **人工评审**：讲者说他和三个同事做了 1,000 对 A/B（视频描述也写 1,000 human A/B checks）；日程简介说他「rated the first few hundred pairwise comparisons myself. My evaluation was broken」；Methodology 现在写「The human arena is closed」，head-to-head 用的是 Claude Opus 5.5 (medium) 自动判
10. **日程简介承诺但演讲没讲到的**：「same drafts average 74 out of 100 with one judge and 88 with another」；「whether Elo was even the right tool」；「My evaluation was broken, and only doing the work by hand revealed it」。这几点在视频和 slide 里都没有展开
11. **讲者粉丝数**：slide 74.2K YouTube subscribers；日程 bio「70K+ subscribers」（时间差，不矛盾）

## 【待确认】汇总

1. Karpathy「slopacolypse」引语（January 2026）的原始出处
2. slide「What is slop?」里 Willison / OUP / swyx / Antislop 论文几条定义的原文
3. 「Ten scripts」页 Published / Never published 各几篇（被挡）
4. 「why 5 drafts」页的模型全名（Grok 4.7、Nemotron 3 Ultra 是补全的）
5. 「What every judge sees」页「25,400 times … (as of today)」整句
6. Antislop 论文的 Samuel Paech 与 EQ-Bench 作者 Samuel J. Paech 是否同一人
7. 讲者说「2.5 times cheaper than Max thinking, but with same results」指的是哪两个配置
8. Opus 5.5 xhigh 的单价（$0.69 还是 $1.003）以及 #1 是 max 还是 xhigh
9. 讲者文章里的「DeepSeek judge」与 slide 的 GLM-5.3 Flash judge 的关系
10. GLM-5.3 Flash 是否还在当前榜单上
12. Partners / Clients logo 是否识读完整
