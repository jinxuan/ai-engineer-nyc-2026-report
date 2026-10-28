# Hermes gives you wings: Build on open foundations.：精确版

> Precise notes for this session, compiled from the official livestream captions, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：直播字幕逐段读完，4 张照片核对完（都是这一场），公司信息对过报道。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | 作者拍的 4 张（IMG_0537–0540 = `01.jpg`–`04.jpg`），都是直播画面的翻拍，下方 banner「Keynote · Dillon Rolnick / CEO」，全部属于这一场。没拍到的 slide 不少（讲者有多处停顿翻页） |
| 视频 | 没有单独剪辑的视频。Day 1 官方直播 [akEqiXFo3GM](https://www.youtube.com/watch?v=akEqiXFo3GM&t=26480s)，章节「7:21:20 Hermes Gives You Wings: Build on Open Foundations — Dillon Rolnick (Nous Research)」，到 7:36:49 闭幕致辞。下文时间都是直播时间 |
| 字幕 | YouTube 自动字幕（见勘误表）。7:31:14–7:31:32 讲者停顿（「sorry」），像是忘词或翻页 |
| 其他资料 | 融资报道（[The Next Web](https://thenextweb.com/news/nous-research-90m-1-5bn-hermes-agent-wsj)、[Dealroom](https://dealroom.co/news/160592-nous-research-raises-90m-at-1-5b-valuation-for-open-source-ai/)）；Hermes Agent Bot Mode 的报道（[律动 BlockBeats via AICoin](https://www.aicoin.com/en/news-flash/3029829)） |

## 基本信息

- 时间地点：2026-10-13（周二）5:06pm–5:24pm，Simons Room (Metropolitan East)，Featured Keynote，Main Stage，当天最后一场 talk（资料：[日程](https://ai.engineer/nyc/2026/schedule?session=871cd5c8-13f4-4fe9-ac49-c8b25d970add)；主持人介绍「for our final talk of the day」，直播 7:21:06）
- 讲者：Dillon Rolnick，CEO，Nous Research
- 官方简介：「Modern businesses require open agents.」
- 资料：Nous Research 2026 年 10 月宣布融资 $90M，Robot Ventures 领投，Nvidia、M12 参与；WSJ 报道估值 $1.5B，公司未官方确认；资金用于 Hermes 的企业版（Hermes Cloud、Hermes Business、Hermes Enterprise）（[The Next Web](https://thenextweb.com/news/nous-research-90m-1-5bn-hermes-agent-wsj)、[Dealroom](https://dealroom.co/news/160592-nous-research-raises-90m-at-1-5b-valuation-for-open-source-ai/)）。演讲本身没提融资
- 讲者说（直播 7:21:31）：「I'm Dillon Rolnick. I'm the CEO of Nous Research and today we're going to talk about building on foundations and platforms that you can actually own.」

## 1. 模型在商品化，巨额投入怎么收回

- 讲者说（直播 7:21:51）：「How do you make back a [trillion]-dollar infrastructure and research bet?」（字幕「centillion」，【待确认】；后文多次说「trillion dollar」）。各家实验室各自花了数千亿美元，整个行业「has spent trillions perhaps」
- 讲者说（直播 7:22:16）：「the models have begun to commoditize」。在场的实践者凭直觉知道：你们会在「Astra, Fable, Opus, Kimi」之间随意切换，不太在意背后是谁，想试新模型
- 讲者说（直播 7:22:41）：数据上也看得到：这周 Twitter 上流传的图表显示 token 平均成本下降、token 总花费激增，而服务 token 的基础设施成本持平；原本高性能模型的利润空间在收窄
- 我的理解：Astra 应是 OpenAI 的 GPT-6 Astra，Fable、Opus 是 Claude 模型，Kimi 是 Moonshot 的模型；讲者没展开

## 2. 两条路：换一层竞争，或者 build God

- 讲者说（直播 7:23:12）：要收回投入，「two pretty obvious ways」：一是「find a different layer of the stack to compete at. You go down to the infrastructure or you go up to the user」；二是「you build God, and you build God faster than anyone else, and you monetize God」
- slide（`01.jpg`，IMG_0537）：「1. Compete at a different layer 2. Build god」，蓝色噪点背景，中间一个打坐人形的模糊图像，右下 Nous 标志
- 讲者说（直播 7:23:47）：「If you're in front of the user, you have an opportunity for stickiness. Stickiness means margin.」所以竞争主要转到了 agent layer（提到前面有讲者说 5% 的劳动力在用 agent）
- 讲者说（直播 7:24:08）：「Consumers though are fickle and they're terrible customers.」企业在 token 上花很多钱，而且有经济上有产出的用途，所以 user layer 的仗会打到企业里；最好的办法是嵌进企业，「when you're in a business, it's hard to take you out」
- 讲者说（直播 7:24:37）：两三年后，按产出算，你的劳动力有多少是 agentic 的？「70% conservatively, probably more」。如果 70% 的劳动力由一个可能和你利益相悖的人组织，「they can be organized into a strike」；要收回万亿投资，你会「use your asymmetric leverage and you would change the rules of the game」

## 3. 如果造出了 God：自己去解决最赚钱的问题

- 讲者说（直播 7:25:20）：「If you succeed at building God, what you would do is you would point God at the most lucrative problems in the world.」「The best shot at earning it back is not to sell it for an ever decreasing margin to others … The best shot is to solve them yourselves.」
- slide（`02.jpg`，IMG_0538）：「The best shot at earning it back is to point the models at the most lucrative problems in the world.」配一个大鼠标指针图形
- 讲者说（直播 7:25:48）：「Labs think that markets are solvable problems and that's exactly how they view the financial industry」，「a tractable problem at that, perhaps not even a very hard one」
- 讲者说（直播 7:26:06）：聪明的实验室知道自己没有足够的金融行业 context、know-how 和经验，所以会合作：雇 investment banking analyst，和全球最大的一些机构合作，在这里打地基。「My belief is that they will eventually go to the source because … that's where the money is」
- slide（`03.jpg`，IMG_0539）：六条新闻截图（从照片读，来源名和日期较模糊）：
  - CNBC「OpenAI targets work of Wall Street junior bankers with new ChatGPT for Financial Services」（「4 weeks ago」）
  - BleepingComputer「Anthropic wants Claude to analyze your bank account and financial data」（「3 weeks ago」）
  - The Verge「OpenAI now wants ChatGPT to access your bank accounts」（May 15, 2026）
  - Business Insider「Hot job alert: OpenAI is hiring an investment banker, paying up to $205K plus equity」（Jul 7, 2026）
  - OpenAI「MUFG aims to become AI-native with OpenAI」（Jul 7, 2026）
  - Banking Dive「Customers Bank deepens ties with OpenAI」（Apr 27, 2026）
  - 这些报道原文没有逐条核对，日期以照片为准【待确认】
- 讲者说（直播 7:26:37）：有人会说这不新鲜，一直有人设局把用户锁住再涨价；「But the difference is that the stakes are much higher. We're talking about meaningful [double-digit?] percentages of the Earth's economy」（字幕「deca percentages」，【待确认】）

## 4. 你需要 AI，所以选什么做地基很关键

- 讲者说（直播 7:26:58）：「you in this room need AI」，看过 agentic system 能做什么，「there's no going back」
- 讲者说（直播 7:27:11）：在场做决定的 VP、高管、工程师，现在就要决定在哪些工具和流程上「grow roots」；无论选什么都会固化下来，积累 know-how，组织围绕它建流程和系统
- 讲者说（直播 7:27:42）：「I implore you, do not build on quicksand foundations. Build on open foundations. Build on Hermes Agent.」
- slide（`04.jpg`，IMG_0540）：「Don't build on quicksand.」配一张流沙/水流般的抽象图

## 5. 为什么是 Nous：道德和哲学上的理由

- 讲者说（直播 7:27:53）：「I just spent a fair amount of time convincing you hopefully that the highest expected value move in AI right now is to either trap you and jack up the prices or replace you directly. So, why am I standing here?」
- 讲者说（直播 7:28:15）：「we built this company primarily for moral and philosophical reasons」；商业里道德和激励常常不一致，「it's probably easiest to change your morality」；但他们认为世界在变，最好的做法是双方利益一致
- 讲者说（直播 7:28:45）：公司是为了对抗「a seemingly inexorable trend towards centralization and centralized control of perhaps the world's most important technology」；Nous「was built by and staffed by nerds. Nerds who love science fiction」。「Skynet was, after all, a closed source company.」

## 6. 专有软件不再有护城河

- 讲者说（直播 7:29:16）：「there's no longer really a strong moat in proprietary software. The marginal cost to reproduce a piece of software has plummeted.」
- 讲者说（直播 7:29:28）：几个月前 xAI 发布了 Grok Bot，「we thought, wow, that's an intuitive user interface … So we pointed Hermes Agent at it and we reproduced it in a weekend and released it as a feature within Hermes Agent, open source of course」
- 资料（二手）：2026-08-14 律动 BlockBeats 报道 Nous Research 开始公开测试 Hermes Agent 的 Bot Mode，「offered as a standalone plugin」，「a Bot is essentially a Hermes Profile」；报道称设计「clearly follows the recently launched Grok Bot」，社区问是否补上了和 Grok Bot 的差距，Hermes 联合创始人 Teknium 回复「Yep」（[AICoin](https://www.aicoin.com/en/news-flash/3029829)）。「一个周末」只有讲者说法，【待确认】
- 讲者说（直播 7:29:47）：以前做软件很难，要协调的工程团队花很长时间把开源库拼成专有软件，再作为 SaaS 卖；「But now the opposite is true. Anything can be reverse engineered」，很多有动力的开源开发者把 agent 对准各种软件。「The moats of yesterday were in proprietary IP. The moats of tomorrow are in network effects, reputation, and reliability.」

## 7. 模型层同样：distillation

- 讲者说（直播 7:30:32）：实验室总是低估自己的工具，或者低估用户的才智和动力；模型层也一样，「models can fairly easily teach another model to be nearly as smart as the original model」；模型的优势很少在架构或专有的新系统，「it's usually in the quality of the data and the scale」
- 讲者说（直播 7:31:32）（停顿后）：模型公司意识到可以用补充的高质量数据、让一个模型教另一个模型来追上，「So they labeled the teaching process distillation and they labeled illicit distillation theft. You cannot call that copyright infringement because copyright infringement would imply that you somehow took every living human being's written or creative work and then trained on top of it.」「they also trained the models to not assist in the process of training other models and they labeled that safety.」
- 我的理解：这一段是讲者对头部实验室的批评，没有点名

## 8. Agent 更像操作系统，而不是 chatbot

- 讲者说（直播 7:32:26）：「agents are like operating systems and they're like frameworks and they're a lot more like those than they are like chatbots」；只把 agent 当私人助理，「you will forever live on the surface」
- 讲者说（直播 7:32:51）：引用当天稍早一场讲金融机构如何用 AI 的演讲（数据源、routine code、人的观点、judgment 如何汇成投资决策）。「Agents are really at their heart a system that combines determinism and non-determinism」；新的应用不再必须是确定性的「cold hard code」，可以注入 intelligence、reasoning、judgment
- 我的理解：指的应是同一直播 6:47:46 的「A Hedge Fund as a Function」（Frank Long，Coatue），那场讲每个环节由 deterministic code、LLM、statistical method、human 组合运行；讲者没点名，【待确认】
- 讲者说（直播 7:33:46）：你应该在自己拥有的地基上建这些系统，「be able to swap in and out the models that you want」，在 determinism、non-determinism、code、数据源、judgment、reasoning 这些原语上叠功能
- 讲者说（直播 7:34:09）：但「you should not be spending your entire lives being intelligence companies」；多数公司不是 agent 公司、不是 AI 公司，应专注自己擅长的事；state of the art 变化时，「you should inherit that, not rebuild it and maintain it」
- 讲者说（直播 7:34:37）：「these are tools that should make you better. They are not drop-ins to replace you. Do you know how much energy it takes to create a human?」

## 9. Hermes Agent 的主张和收尾

- 讲者说（直播 7:34:47）：「you can run it however you want, wherever you want. Modify it as you see fit. If you're not having fun anymore, you can take your ball and you can go home. The moat has changed. The moat is no longer entrapping you. The moat is in reputation.」
- 讲者说（直播 7:35:11）：「I want to build a standard. Hermes Agent today is the most used and downloaded open source agent in the world. It is order[s] of magnitude more downloaded than some of its closed competitors.」（字幕「order of of magnitude」，单复数听不清；这两个说法没有找到独立数据，【待确认】）
- 讲者说（直播 7:35:27）：只要有一方愿意开源这些工具，「water will continue to flow downhill. And I want to be downhill so that I can work with you to provide support, safety, scalability to go along with the world's best open agent」。护城河在「Network effects, reliability, and reputation」
- 讲者说（直播 7:35:57）：「Somebody is going to earn back the trillion dollar investments that we've made. Make sure that it's you.」「You can have your cake and eat it, too. You can have state-of-the-art features and reliability without sacrificing your autonomy. Build on something you own. Build with us. Thank you.」

## Q&A 精确版

没有 Q&A。讲者在直播 7:36:24 说「Thank you」，掌声后主持人 Alex Lieberman 上台做闭幕致辞（直播 7:36:37）。

## 勘误表（YouTube 字幕 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| Dylan Rolnik / Dylan Rolnick | Dillon Rolnick | 日程、直播 banner |
| News Research / Noose research | Nous Research | 日程、slide logo |
| centillion dollar | trillion-dollar（【待确认】） | 后文「trillion dollar investment」 |
| Kimmy | Kimi | 上下文 |
| attractable problem | a tractable problem | 上下文 |
| quicksound found quicksand | quicksand | slide「Don't build on quicksand.」 |
| inexurable | inexorable | 上下文 |
| XAI red released Grockbots | xAI released Grok Bot | 上下文、报道 |
| piler open source | pile up open source（【待确认】） | 上下文 |
| SAS | SaaS | 上下文 |
| moes | moats | 上下文 |
| dropins | drop-ins | 上下文 |
| 10X（主持人介绍） | Tenex | 直播章节「Alex Lieberman (Tenex)」 |

## 出入

1. **「point God」vs「point the models」**：口头是「point God at the most lucrative problems」，slide `02.jpg` 写「point the models at the most lucrative problems in the world」
2. **Grok Bot 的复制**：讲者说「reproduced it in a weekend」并作为 Hermes Agent 的功能开源发布；能找到的二手报道说 Bot Mode 以独立插件形式公开测试，没提用时
3. **「most used and downloaded open source agent」**：只有讲者说法，没有找到下载量数据
4. **当天稍早的金融机构演讲**：讲者没点名，按直播顺序推断是 Coatue 那场

## 【待确认】清单

1. 「centillion dollar」原话是否「trillion-dollar」
2. 「deca percentages of the Earth's economy」原话（可能是「double-digit percentages」）
3. slide `03.jpg` 六条新闻的来源和日期（照片模糊，未逐条打开原文）
4. Hermes Agent 下载量和「order[s] of magnitude more downloaded than some of its closed competitors」的出处
5. 「reproduced it in a weekend」
6. 讲者引用的「5% of a workforce」出自哪位讲者
7. 引用的金融机构演讲是否 Coatue 的「A Hedge Fund as a Function」
