# Frontier Models for Acting in Space and Time：精确版

> Precise notes for this session, compiled from the official livestream captions, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：直播字幕逐段读完，两张照片核对完，公司信息对过官网和报道。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | 作者在现场拍的 2 张（IMG_0534 = `01.jpg`、IMG_0536 = `02.jpg`），都是会场大屏，不是直播画面。01「Get Early Access」二维码页，02「Thank you!」团队合照页。讲者口头提到的其他 slide（如「on the left」的模型示意图）没有照片 |
| 视频 | 没有单独剪辑的视频。Day 1 官方直播 [akEqiXFo3GM](https://www.youtube.com/watch?v=akEqiXFo3GM&t=25339s)，章节「7:02:19 Frontier Models for Acting in Space and Time — Pim de Witte (General Intuition)」，到 7:21:20 下一场开始。下文时间都是直播时间 |
| 字幕 | YouTube 自动字幕，ASR 错误较多（见勘误表）。提问人的问题没有收音 |
| 其他一手资料 | [General Intuition 官网](https://generalintuition.com)；MIRA 世界模型（官网列出）；融资报道见下 |

## 基本信息

- 时间地点：2026-10-13（周二）4:48pm–5:06pm，Simons Room (Metropolitan East)，Featured Keynote，Main Stage（资料：[日程](https://ai.engineer/nyc/2026/schedule?session=cada670e-c5ea-4977-b3c2-9a68dda26abb)）
- 讲者：Pim de Witte，Co-Founder & CEO，General Intuition
- 资料：日程 bio：General Intuition 是「the frontier lab for acting in space and time」；此前创办 Medal（游戏片段平台）；做游戏和 AI 之前在 Médecins Sans Frontières（无国界医生）工作过，14 岁时运营过最大的 RuneScape 私服；公司「recently raised $650M+ from Khosla and General Catalyst」
- 资料：官网写「over $650M in the past year」。报道：2025 年 10 月约 $134M 种子轮（Khosla Ventures、General Catalyst 领投），2026 年 6 月 $320M Series A，估值 $2.3B（[Dealroom](https://dealroom.co/news/136129-general-intuition-raises-320m-series-a-at-2-3b-valuation/)、[GamesBeat](https://gamesbeat.com/?p=327574)）。另有 GamesBeat 标题称再融 $220M、估值 $6.2B，未核对，【待确认】
- 官方简介即上面的 bio，没有单独的 talk 摘要

## 1. 开场：用互联网数据解决互联网上没有的问题

- 讲者说（直播 7:02:20）：如果有人告诉你，把互联网上所有信息拿来训练模型，再用它解决互联网上没有的科学问题，「I think a lot of people would tell you that that's crazy」；「what we're doing has a lot of parallels to that story」
- 讲者说（直播 7:02:53）：Frozen 里的 Olaf：从山上滚下来撞到石头，身体散开又拼回去。「Olaf knows how to do that because Olaf can observe as he is acting」；和「autoregressively predict tokens」的 LLM 不同，它「constantly observing as it is acting」
- 讲者说（直播 7:03:36）：这是 LLM 做机器人和「native architectures like what we're working on」的大区别；Twitter 上的 demo 让人容易觉得 LLM 能解决一切，「There's fundamental limitations … where purpose-built models are a much better fit」

## 2. Medal：游戏录像就是「互联网」

- 讲者说（直播 7:04:00）：「for us it all started with Medal. We built the largest video game recorder in the world」，「we think of that as the internet」：人们用 Medal 录自己玩上万款不同游戏
- 讲者说（直播 7:04:17）：人可以用玩游戏的同一个大脑去 teleoperate 机器人，很多情况下还用同一个手柄；所以游戏的 action space（因为手柄）和 teleoperation/现实世界的 action space 常常是同一个。「every single robot below certain degrees of freedom ships with the game controller. A majority of the supply chains already work this way」
- 讲者说（直播 7:04:52）：录像器把输入/动作和画面帧对应起来；他们给动作打语义标签，「so we understand semantic meaning behind every action」，用来训练 vision-based model
- 讲者说（直播 7:05:10）：屏幕左边（slide，无照片）是模型的直观示意：给模型一组可选按键，「essentially a computer use model」；输入是画面帧，也可以加 text prompt，通过 API 调用。例子：有人朝你挥手，模型知道有「greet」动作可用，就输出对应动作（字幕「a greed action」）
- 资料：官网称 Medal 是「the world's largest and fastest-growing platform for gamer moments」，玩家每年上传「billions of gameplay clips」，作为 action-labeled video 训练数据（[generalintuition.com](https://generalintuition.com)）

## 3. 和 LLM 的对比：turn-based vs 连续帧

- 讲者说（直播 7:05:42）：LLM 用大规模互联网数据训练预测下一个 text token，现在用来写软件；这些 workload 适合「futures that are not simultaneously unfolding as you are predicting」。「Code is turn-based」，所以很难泛化到「environments … that include space and time」
- 讲者说（直播 7:06:24）：他们的模型持续接收画面帧（机器人摄像头或屏幕），「predicts everything as if it were just a video game」
- 讲者说（直播 7:06:44）：类比 Anthropic：「they were never frontier in just base text generation. They always focused on being the best at generating code」，赌的是更多人会把问题带进 coding space。General Intuition 的赌注：「as humans we are generally pixel-to-action converters plus senses」，其他感官很多也能转成像素；输出是键鼠或手柄动作，「essentially equivalent to that of an enhanced computer use model with more actions on the output space」。「you send frames and you get actions」
- 讲者说（直播 7:07:42）：人不会预测电机扭矩，手柄这种设备替人做了这件事；多数机器人公司在 end-to-end 层面预测精确姿态或电机数值，「our approach is let's just use that」（即用手柄这层现成的抽象）
- 我的理解：讲者对 Anthropic 的描述是他的概括，不是 Anthropic 的说法

## 4. 数据规模

- 讲者说（直播 7:08:26）：「Medal at the moment sees about a billion videos uploaded per year. This year we're on track for three billion」，带来「over a trillion and a half action tokens per month」
- 讲者说（直播 7:08:45）：「the total amount of data that frontier labs use today to train robotics models we collect in at the moment half a day」
- 讲者说（直播 7:09:08）：「We just bet on scaling pixel space the same way that Anthropic scaled coding space」，并赌随着数据和算力进入 pixel space，人们会把更多问题带进 pixel space；结果是「an incredibly general agent that can go and solve problems on computers [and] in the real world」
- 资料：这些数字只有讲者口头说法，官网只写「billions of gameplay clips」每年，【待确认】具体数字

## 5. World models 生成训练环境

- 讲者说（直播 7:09:39）：Anthropic 做 coding environments，「We focus on world models, which most of you know us for the MIRA release」，用 world model 生成更多环境让模型学习（字幕「mirror release」）
- 资料：官网的 world model 叫 **MIRA**，「a playable multiplayer world model」，与 Kyutai 合作、和 Epic Games 协作，用 10k 小时 bot 生成的 Rocket League 数据学四人对战（[generalintuition.com](https://generalintuition.com)）；官网把产品分为「action models that decide what actions to take」和「world models that predict the outcomes of actions」
- 讲者说（直播 7:09:53）：LLM 公司要给模型生成学习环境有两个问题：一是要花算力模拟环境，越逼真越贵；二是想让机器人在人的同一 input space 学东西（看着学），不想经过「an LLM, for a digital twin to have to be created, for the model to have to get deployed, … a bunch of verifier checks」之后机器人才学到。「when you can learn in the native modality of the human, you can all of the sudden start solving a lot more problems a lot faster」
- 讲者说（直播 7:10:56）：模型出来后会「significantly faster than any LLM at doing these things」

## 6. 游戏的动作密度（APM）

- 讲者说（直播 7:11:06）：「games are so action dense」；模型可以同时预测多个动作（字幕此处「use soft for a model predicting multiple actions」听不清，【待确认】）
- 讲者说（直播 7:11:22）：例子：操控无人机要做很多微调，「you really want to start with games, you don't want to start with teleoperations because the human will just take a lot less actions」；「you want a model that can predict really high APM」，数据集的 APM 让很多问题变得可解
- 讲者说（直播 7:11:48）：模型不只知道预测的是哪个键，还理解这个键的语义，「really important for things like interpretability」

## 7. 发布计划和用法

- 讲者说（直播 7:12:00）：「we will be releasing the models this year」；「we're stunned by the fact of what these models can do」。直播里不能放发布视频，但团队在纽约，可以找团队成员看现场 demo
- 讲者说（直播 7:12:35）：「we have essentially one model which can play video games at the highest level and control over 10 types of robots using the exact same primitives. So pixels in, actions out, completely real time」
- 讲者说（直播 7:12:49）：未来几个月陆续开放 early access；已有一些客户在用，但还是私下的；扫二维码报名，按 waitlist 报名先后放号
- slide（`01.jpg`，IMG_0534）：「The frontier lab for acting in space and time」「Get Early Access」二维码，general intuition logo，右侧一只像素化的羚羊/山羊图形
- 资料：官网写已接入「a small number of partners」做商用 API，覆盖 games、simulation、robotics，其他人走 waitlist
- 讲者说（直播 7:13:11）：类比 JEPA（字幕「Jev」，【待确认】是否指 JEPA）和「a lot of these decision-making models」打开了人们对新架构原语的眼界：「People are ready to kind of do away with the unpredictability of an LLM and put something in that is more reliable and faster」
- 讲者说（直播 7:13:40）：用例：探索用 LLM 生成的环境；UX 测试（模型像人一样操作界面，判断界面是否直观）。「all these problems that LLMs can't solve because they can't actually explore like a human」
- 讲者说（直播 7:14:12）：发布形态：仍然预期你用一个 orchestrator model 做长期规划，「something like Gemini or Astra」，把这些模型插进自己的 workload。字幕「Gemini or Astra」；Astra 应指 OpenAI 的 GPT-6 Astra（2026 年 9 月发布，[Simon Willison](https://simonwillison.net/2026/sep/5/introducing-gpt-6-astra-for-developers)）【待确认】讲者指的是不是它
- 讲者说（直播 7:14:26）：「it is genuinely going to feel like the early LLM days when you have a new toy」；早点理解这些原语，可以在上面建很多生意
- 讲者说（直播 7:15:03）：例子：科学实验室，能可靠地在空间和时间里编排机器人、接受文字指令；「Robotics is interesting because one of the big problems of solving robotics is making more robots」，很多被智能卡住的生意会因为解决 pixels to actions 冒出来

## 8. 无障碍用例

- 讲者说（直播 7:15:45）：因为模型能像人一样用电脑，可以帮助需要无障碍辅助的人。例：只能用一只手的人，自己控制鼠标，模型预测键盘动作；模型把用户的动作当输入（「it's observing as it's acting」），所以不会去预测用户已经在做的动作，只补键盘
- 讲者说（直播 7:16:30）：「If you're interested in building in this space, this is going to be the most interesting toy that comes out this year … I'm fairly certain」
- 讲者说（直播 7:16:43）：初期用途很多会在 simulation：如果你在用「Astra or Gemini」生成环境，现在有东西能替你探索（字幕「generating environments with Astra or with Gemini」）。我的理解：环境生成更常提的是 Google 的 Genie，字幕「Gemini」可能是 Genie 的误识别，【待确认】

## 9. 团队、招聘、时间表和 hackathon

- 讲者说（直播 7:17:04）：团队在纽约（「The Medal team and the General Intuition team is here in New York City」），想看 demo 或加入都可以联系
- 讲者说（直播 7:17:22）：想招「people who want to understand these models as much as we do」，擅长生成新环境；有研究背景更好，「but I don't have a research background so that's okay」；理想是有 end-to-end 交付 AI 产品经验的人
- 讲者说（直播 7:18:02）：三个团队：world modeling、policy（agent）、data generation 和 environment generation。「If you're an engineer, the best one to join is probably the environment generation team」
- 讲者说（直播 7:18:24）：「I'm very very excited for November」；「it will be either late November, early December is the current planning」
- 讲者说（直播 7:18:36）：要办「three hackathons」，第一个在纽约，11 月中；报名 partner waitlist 的人会收到邮件；之后说「there will be one in New York City and one in London」
- slide（`02.jpg`，IMG_0536）：「Thank you!」团队合照（约 14 人坐在沙发前）；「Stay in touch! pimdw@generalintuition.com」；「We're hiring · Scan for open roles.」二维码
- 资料：官网也写在招 video、computer vision、policy learning、world models、robotics、infrastructure、large-scale training 方向

## Q&A 精确版

讲者说「I have time for one or two questions」（直播 7:19:11），实际只答了一个，之后说「one more and then I got to run」，但没有第二个问题收进字幕就结束了（直播 7:20:49「All right. Thank you everyone」）。

**Q1（提问没有收音，从回答看是问和 JEPA 有什么不同）**
- 讲者说（直播 7:19:26）：JEPA「doesn't train alongside the action labels. So they bet on inferred action labels for the most part」；「JEPA I think in the limit is the more correct approach but it's just a lot less practical in the short term because you need to bet on a separate scaling law on the action inference itself」
- 讲者说（直播 7:19:46）：「We really like JEPA as a concept … we use some of the things in the models … but at the end of the day it's an approach, not a product」，两边会互相借鉴
- 讲者说（直播 7:20:04）：例：开飞机时动后面的方向舵，从视频帧里几乎看不出来。「our moat on having the action data really transfers better to action sequences than it does to world models and to generating the pixels」，所以全押在训练动作预测的通用 foundation model 上；world model 也用各种技术做，也乐见其他公司做

## 勘误表（YouTube 字幕 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| metal / Metal | Medal | 日程 bio、官网 |
| Tanthropic | Anthropic | 上下文 |
| autogressively | autoregressively | 上下文 |
| tell operate / tele operations | teleoperate / teleoperation | 上下文 |
| a greed action | a greet action（挥手打招呼） | 上下文 |
| LMS / LM | LLMs / LLM | 上下文 |
| mirror release world models | MIRA release | 官网 |
| skilled coding space | scaled coding space | 上下文 |
| Jeppa / JPA / Jetpa | JEPA | 上下文 |
| our mode on having the action data | our moat | 上下文 |
| weight lists | waitlists | 上下文 |
| purposebuilt / endto-end | purpose-built / end-to-end | 上下文 |

## 出入

1. **hackathon 数量**：讲者说「three hackathons」，但只点了纽约（11 月中）和伦敦两个
2. **发布时间**：先说「we will be releasing the models this year」「excited for November」，又说「late November, early December is the current planning」
3. **融资数字**：日程 bio「$650M+ from Khosla and General Catalyst」，官网「over $650M in the past year」；报道的轮次（$134M 种子、$320M Series A）加起来约 $454M，另有 $220M 一轮的报道未核对
4. **数据量**：讲者说每年约 10 亿条视频、今年有望 30 亿；官网只写「billions of gameplay clips」每年
5. **直播链接时间**：data 里旧链接按日程估算到 7:33:08；直播章节实际是 7:02:19

## 【待确认】清单

1. 「use soft for a model predicting multiple actions」（直播 7:11:13）原话
2. 「similar to how Jev and a lot of these decision-making models」（直播 7:13:16）里的「Jev」是否 JEPA
3. 「Gemini or Astra」是否指 Google Gemini 和 OpenAI GPT-6 Astra；环境生成那句的「Gemini」是否 Genie
4. Medal 视频量（10 亿/30 亿）、「1.5 trillion action tokens per month」、「half a day」对比的出处
5. 「control over 10 types of robots」没有公开资料
6. 第三场 hackathon 的地点
7. 提问人的原问题（没有收音）
