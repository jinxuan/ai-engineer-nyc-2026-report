# AI and the Future of Fundamental Investing：精确版

> Precise notes for this session, compiled from the official video captions where available, the author's room recording, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：录音整段读过，几处关键数字和听不清的句子用手机录音重新转写核对过。这场是 fireside chat，没有 slide，也没有录到观众提问；全文按主持人的 9 个问题分节。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | 无（台上对谈） |
| 视频 | 官方 Day-1 livestream 有这一章：[5:29:04](https://www.youtube.com/watch?v=akEqiXFo3GM&t=19744s)「AI and the Future of Fundamental Investing — Jack Woodruff and Vinoo Ganesh (Kepler)」（YouTube 描述的 chapters，用 yt-dlp 只读了元数据）。livestream 没有字幕，没有下载 |
| 讲者 Q&A | DEV 上 The Daily Context 的会前采访 [Kepler's Vinoo Ganesh on Building AI You Can Trust](https://dev.to/dailycontext/keplers-vinoo-ganesh-on-building-ai-you-can-trust-3p0f)（已存 `links/1.md`）。是会前采访，不是这场的内容，只在相关处作背景引用 |
| 其他 | Vinoo 另一场 talk「Do Not Let Models Invent Numbers」（https://www.youtube.com/watch?v=dZgA6btnb_s）不是本场，未引用 |

## 基本信息

- 时间地点：2026-10-13 2:55–3:13pm，Simons Room (Metropolitan East)，Stage Talk (18 mins)，Main Stage（session.json）
- 形式：fireside chat，Vinoo Ganesh 提问，Jack Woodruff 回答
- Jack Woodruff：Kepler Senior Advisor（session.json）。bio：Candlestick Capital Management 创始人、前 CIO（2019 起到 2025）；2010–2019 在 Citadel 的 equity 部门 Surveyor Capital 做 Portfolio Manager，「built and led an 18-person investment team and invested in more than 400 global equities」；更早在 Sigma Capital（SAC 旗下）等；从 Ernst & Young 起步，持加州 CPA（inactive）
- 资料：[Hedgeweek 2025-12-10](https://www.hedgeweek.com/citadel-veteran-to-close-candlestick-capital/) 转述 Bloomberg：Woodruff 关闭 2019 年创立的 $2.1bn 对冲基金 Candlestick
- Vinoo Ganesh：Kepler CEO。bio：Palantir 七年；Veraset CTO（被收购）；Citadel Head of Business Engineering
- 官方简介要回答的问题：AI 在 fundamental investing 里改变了什么、没改变什么；他绝不交给模型的是什么；「Will the biggest funds win everything? If everyone has the same models, where does alpha come from? And when does a number have to be right, not just plausible?」

## 1. 五年后的 fundamental investing 是什么样？[00:00]

- Vinoo 开场 [00:00]：他们之前聊过；这场「really as a fireside chat」（转写为 "fireside shop"）
- Jack 讲者说 [00:26]：「long-term stock picking, I don't believe is really going to change much at all. There's always been a mosaic and there's always been alpha creation by finding a divergent view.」变的是怎么综合信息、table stakes 变成什么：「how people synthesize all the information and where table stakes go to … is going to change completely」
- Jack [01:00]：PM 和分析师的传统角色「are going to be completely thrown up on their heads」，但「it's like too early to say」，工具太新

## 2. 一个优秀分析师能在 2030 年覆盖 400 只股票吗？[01:29]

- Vinoo 提问 [01:29]：「at Citadel you had 18 people covering 400 stocks and at Candlestick you had 30 stock [pickers]. Could one great analyst cover all 400 by 2030?」（"stock figures"，【待确认】是 pickers 还是别的词）
- Jack [01:44]：「the biggest issue is humans are still the bottleneck … you have to get the information into your head」；「investment management is a time management exercise」：只研究一只股票会比谁都懂，但同时覆盖的名字越多，每只能花的时间越少，边际收益递减
- Jack [02:51]：AI 可能提高「general knowledge」的 table stakes：「that work used to take me hours to prepare … now maybe an AI program can help me prepare a lot of that work in five minutes, which used to take me six to eight hours」
- Jack [03:10]：竞争很激烈，省下的时间要拿去做更多 proprietary research、channel checks、行业背景研究；「if I do the exact same thing I was doing before and … I can get my work done in two hours, someone else is going to be still working 16 hours a day trying to find that extra thing」
- Jack [03:58]：一两年前付钱让分析师做的事，「is not what we're going to be paying an analyst to be doing, you know, in a year」

## 3. 今天初级分析师做的哪件事，五年后还在做就显得荒唐？[04:13]

- Jack [04:13]：常见的说法是不用再读 10-K 和 transcript，「I actually don't believe that to be true」：真正跟一只股票跟得近，还是要自己消化，摘要能省时间，「but you still need to get that information into your head」
- Jack [04:45]：过去很多分析师在处理 big data，这部分「definitely … can be outsourced」
- Jack [04:59]：「What I believe the AI is going to do … is going to be pushing you in the right direction to work on the right things. … It's all about how do I know to work on the right things.」赢家是能用工具把时间花在找 divergent view 上的人

## 4. 找数据、清数据、核数据的时间省下来，分析师会更聪明还是只是更忙？[05:30]

- Vinoo 提问 [05:30]：分析师每周大量时间在「finding, cleaning, and checking data」（转写为 "clearing"），这部分消失后会怎样
- Jack [05:40]：省下的时间会用来找更多数据、做更多 proprietary research calls、industry builds，「it's just going to raise the bar on the amount of work that everybody's doing」；大家都做同样的事，就成了 table stakes
- Jack [06:13]：「The investment mosaic is all about finding three or four sort of data points that all sort of are lining up, that aren't necessarily coming from the same place. And that's generally, I think, the widely accepted view of how alpha is created.」

## 5. 你会在看不到出处的情况下签认一个数字吗？[06:50]

- Vinoo 提问 [06:50]：Jack 从 CPA 起步，「Would you ever sign off on a number or model produced without seeing where it came from?」
- Jack [07:00]：「No, no, no. Absolutely not.」「the most exciting thing about what's going on at Kepler is just our ability to audit every single number」
- Jack [07:11]：过去人工录入出错「was very rare」，「but still mistakes were made from time to time and … millions of dollars could be lost off of a single sort of like data entry mistake」；「there's no way we're gonna let AI tell us exactly what to do, run trading algorithms … if you can't trust the information」
- Jack [07:41]：「the accuracy of the numbers and your ability to audit it is absolutely the most important part of this entire process」；他认识的很多人用 AI 帮忙搭模型，「then they still have to go through and audit the numbers … so it's like, are we really saving that much time by automating this」，「I think it will get better」
- 资料（DEV 会前采访，Vinoo）：Kepler 的做法是预处理时抽取 fact 并给 identifier，模型只能通过确定性工具查这些 fact；「if the model can't resolve its output against the lookup, it rejects it」；每一步变换写进 provenance ledger

## 6. 你的投资流程里，哪部分绝不让 AI 工程师看到？[08:35]

- Vinoo 提问 [08:35]：「for a lot of the AI companies here, Jack is the buyer … what part of your investment process would you refuse to let an AI engineer see?」（重转版为 "AI industry"，原转写为 "AI engineer"，【待确认】）
- Jack [08:55]：「I'm the wrong guy to ask this question … this is a total CTO question」，他有个很好的 CTO 来管；他自己「would be very nervous about any sort of internal data … you don't want anything to get out」；他不知道术语，「sound like an idiot in front of you engineers」
- Jack [09:39]：他和 Vinoo 把现在叫做「1.0」：先做自动化、把工具和数据都建起来，「then we can actually like really start doing interesting things」，用工具告诉分析师该把时间花在哪
- Jack [10:00]：「investment management isn't about doing earnings previews. It's not about building DCFs. … It's making money. And so to make money, you need divergent views」；工具帮你找 divergent view，「but they're not the outcome」
- Jack [10:30]：竞争性的 moat / 套利「generally last pretty short」，「when someone else is making a ton of money, other people sort of figure it out, arbitrage gone」
- 资料（DEV 会前采访，Vinoo）：他觉得很怪的一点是「a lot of people are very okay sending private information to the labs」；对敏感数据的建议是「I would deploy on-prem」，在自己的 VPC 里跑模型；「Your own data is your alpha」

## 7. 每家基金都有同样的模型后，edge 来自更好的数据、更好的问题还是更好的判断？[10:56]

- Jack [11:05]：「I think all of it.」大家最终都会有同样的信息，「it's all going to be about how you prompt it, how fast you're able to … combine man versus machine」
- Jack [11:20]–[12:02]：过去大量的工作（他说现在可以自动化）「didn't tell us, hey, go buy this stock. It said, hey, this is something you should go spend a lot of time on」，告诉你该去深挖哪个 project，别人都在看别处；「That was the purpose of all the field work」
- Jack [12:20]：「If AI can just do it, then I don't know. I mean, what do we need us for?」工具的用处是找出该做更多研究的地方，「there's an unlimited amount of things one can do」，「It's about identifying facts before everybody else」

## 8. 2027 年开一个新基金，build list 和 buy list 上各有什么？[12:50]

- Vinoo 提问 [12:50]：「If you launch a fund in 2027, what's on your build list versus your buy list?」（原转写为 "bot list"，重转版为 "buy list"）
- Jack [13:01]：「I'm not sure」；人员角色会变：「I still need senior stock pickers. I may not need as many analysts」，但可能需要别的技能的人：「investigative journalists, people who can do like really deep project work … maybe different sort of data engineers」
- Jack [13:50]：过去的结构是金字塔：很多分析师 → senior stock pickers → 几个 PM 和最上面的 CIO；他设想「a little bit more vertical」：「I want the makers … the people who are really there to make the money. I don't know if I'm going to need as many of the preparers」，但也可能需要他们去找更多 proprietary information「to feed the beast」
- 注：Jack 实际没有分别列出 build 和 buy 的工具，答的是团队结构（我的理解）

## 9. AI 更帮谁：能当 50 人用的 5 人基金，还是 5000 人的机构？[14:32]

- Jack [14:54]：每家基金策略和 mandate 不同，有人「turn over my book [many] times a year」做高频（转写为 "9,000 times"，两次转写一致，但数字存疑，【待确认】），有人「buy and hold for five years」；「everyone's going to be able to do more with what they have」
- Jack [15:26]：5 人团队「slower idea velocity」，所以多半是更集中的策略。可选的 idea 越多，理论上 hit rate 越高：「if I cover 100 stocks, I gotta have 20 names in my portfolio, so I'm picking 20%. Well, if all of a sudden I'm covering 200 names, then … 10% … I'm covering 400 names, only 5% are going into that book」，但「the research for those 400 has to be just as solid as the research on the 100」
- Jack [16:15]：5000 人的机构多半是 multi-strat、覆盖各种资产，情况很不同；「idea velocity and the number of good ideas you have in your pipeline is paramount」：一个 idea 兑现了就换出去，放新的进来，「that's how you build alpha and build returns over time」

## 10. 大家看好但你认为行不通的 AI 点子，和没人谈但你认为能成的？[17:03]

- Jack [17:12]：「I wish I knew the thing that nobody's talking about because then I can make some money.」
- Jack [17:18]：大家在谈的是「this no analyst hedge fund」，全用 AI agents；他认为这样的基金「will be able to replicate a fund that existed a year ago; it's just not going to be as good as what today's hedge funds are going to be next year」
- Jack [17:40]：「I don't think you're ever going to really be able to take humans out of the loop.」quant 基金「have taken humans out of the loop for decades, so that's nothing new」；但在 fundamental 上如果能把人完全拿掉，「they're just going to be whoever's got the most [compute], the most information. And I just don't think that that's how it's going to go.」（"compute" 只在原转写里出现，重转版没有，【待确认】）
- Vinoo [17:55]：「Thank you so much, Jack.」录音结束

## Q&A 精确版

## 勘误表（Whisper 转写 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| fireside shop | fireside chat | 上下文 |
| 18 people covering 400 stops | 400 stocks | 上下文、bio |
| 30 stock figures | 30 stock [pickers]，【待确认】 | 上下文 |
| clearing (data) | cleaning | 上下文 |
| Was you ever sign off | Would you ever sign off | 重转 |
| KI | AI | 上下文 |
| run a mock | run amok | 重转 |
| animals | analysts | 上下文 |
| DCS | DCFs | 上下文 |
| parietary | proprietary | 上下文 |
| competitive modes | competitive moats | 上下文 |
| go by this stop | go buy this stock | 上下文 |
| a couple hundred stocks | 100 stocks | 重转（后文 "the research on the 100"） |
| Vinu | Vinoo | session.json |
| today's has funds | today's hedge funds | 上下文 |
| 说话人标签 | 提问 = Vinoo，回答 = Jack；transcript 在 [01:29]、[04:13]、[05:23]、[06:50]、[08:35]、[10:56]、[12:44]、[14:32]、[17:03] 处把问答混在同一标签下 | 内容 |

## 出入

1. **livestream 时长**：官方 chapter 从 5:29:04 到下一章 5:36:54，**只有约 7 分 50 秒**；手机录音 18:15，议程是 18 分钟。livestream 可能只播了一部分或剪过，【待确认】。另外 `session.json` 里存的链接 `&t=22028s`（约 6:07:08）是按议程时间估算的，落在下一个 chapter「Everyone Has AI. Who Has the Advantage?」（6:02:04 起）里，**不对**；以 YouTube chapter 的 `&t=19744s` 为准
2. **Jack 的姓**：议程、session.json、YouTube chapter、Hedgeweek 都写 **Woodruff**；DEV 采访写「Jack Woodroffe」，是 DEV 的笔误（我的理解）
3. **「18 people covering 400 stocks」**：Vinoo 口述为同时覆盖 400 只；bio 原文是带 18 人团队、2010–2019 年间「invested in more than 400 global equities」，是九年累计投过的数量，不一定是同时覆盖。Candlestick「30」没有找到出处
4. **时间对比**：Jack 说 AI 能把「six to eight hours」的准备工作缩到「five minutes」，随后又说「I can get my work done in two hours」，两个数说的是不同的工作量，都是口述举例，没有出处
5. **「9,000 times a year」**：两次转写都是这个数，作为换手率举例偏离常理，可能是口误或听错，【待确认】

## 【待确认】汇总

1. 「30 stock figures」的原词
3. 第 6 问是「AI engineer」还是「AI industry」
5. 「turn over my book 9,000 times a year」的数字
6. 第 10 问回答里「the most compute」是否说过
8. livestream 这一章为什么只有约 8 分钟
