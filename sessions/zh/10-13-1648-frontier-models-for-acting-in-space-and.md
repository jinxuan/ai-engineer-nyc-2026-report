[English](../10-13-1648-frontier-models-for-acting-in-space-and.md) | **中文**

# Pixels in, actions out：General Intuition 押注游戏录像能教会模型行动

*在 AI Engineer NYC 2026 上，General Intuition 的 Pim de Witte 认为，用游戏录像和手柄输入（而不是文本）训练出来的模型，能在屏幕和机器人上实时行动，并宣布了公司首批模型的 early access 和 hackathon。*

**Featured Keynote · 2026 年 10 月 13 日（周二）· 4:48–5:06pm · Simons Room (Metropolitan East)，Main Stage**
**讲者：** Pim de Witte，Co-Founder & CEO，General Intuition

---

Pim de Witte 用一个思想实验开场。假如有人说，业界会把互联网上的所有东西拿来训练一个模型，再用它解决互联网上没有的科学问题，「我想很多人会说这太疯狂了」，de Witte 说，「我们在做的事和这个故事有很多相似之处。」

例子是 Frozen 里的雪人 Olaf。有一场戏，Olaf 从山上滚下来，撞上一块石头，身体散开，又拼了回去。「Olaf 会这么做，是因为 Olaf can observe as he is acting」，de Witte 说。LLM 则是 autoregressive 地预测 token。在 de Witte 看来，这个区别把「用 LLM 做机器人」和「像我们这样的 native architecture」分开了。Twitter 上的 demo 很容易让人觉得 LLM 能解决一切，但「存在根本性的限制……这些地方 purpose-built 的模型合适得多」。

General Intuition 自称「the frontier lab for acting in space and time」。按大会日程的介绍，de Witte 之前创办了游戏片段分享平台 Medal；在做游戏和 AI 之前，de Witte 在 Médecins Sans Frontières（无国界医生）工作过，14 岁时运营过最大的 RuneScape 私服。公司说过去一年融了超过 6.5 亿美元，投资方包括 Khosla Ventures 和 General Catalyst。

## 1. 把游戏录像器当成「互联网」

「对我们来说，一切从 Medal 开始，」de Witte 说，「我们做了世界上最大的游戏录像器，we think of that as the internet。」人们用 Medal 录下自己玩上万款不同游戏的过程。

论证的关键是手柄。人可以用玩游戏的同一个大脑去 teleoperate 机器人，很多时候还用同一个手柄。所以游戏的 action space 和 teleoperation、乃至现实世界的 action space，很多情况下是同一个。「低于一定自由度的机器人，每一台出厂都配游戏手柄，」de Witte 说，「大部分供应链已经是这样运转的。」

Medal 的录像器把输入、动作和它们产生的画面帧对应起来。General Intuition 给这些动作打标签，「这样我们理解每个动作背后的语义」，再用这些数据训练 vision-based model。de Witte 说这个模型「本质上是一个 computer use model」：给它一组可选按键，通过 API 输入画面帧（也可以加 text prompt），它输出动作。如果有人朝它挥手，而它知道有「打招呼」这个动作可用，它就输出这个动作。（de Witte 这里提到的 slide 没有拍到。）公司官网称 Medal 是「the world's largest and fastest-growing platform for gamer moments」，玩家每年上传「billions of gameplay clips」。

## 2. Turn-based 的文本 vs 连续的画面帧

de Witte 说，LLM 拿互联网文本学会了预测下一个 token，现在被用来写软件。这类工作适合「预测的同时、未来并没有在同步展开」的场景。代码是 turn-based 的：你说一句，模型做一步，如此往复。所以它「很难很难泛化到……包含空间和时间的环境」。

General Intuition 的模型则持续接收画面帧，来自机器人的摄像头或屏幕，「把一切都当成一个电子游戏来预测」。de Witte 拿 Anthropic 作比：在 de Witte 的说法里，Anthropic 从来不是纯文本生成的 frontier，而是一直专注于把代码做到最好，赌的是人们会把更多问题带进 coding。General Intuition 的赌注是「as humans we are generally pixel-to-action converters plus senses」，而很多其他感官也能转换成像素。输出是键鼠或手柄动作，模型大致相当于「一个增强版的 computer use model，输出空间里有更多动作」。一句话：「你送进帧，拿回动作。」

de Witte 补充，人不知道怎么预测电机扭矩。手柄就是把意图翻译成这些数值的设备。多数机器人公司试图 end-to-end 地预测精确姿态或电机数值；General Intuition 的做法是直接用现成的手柄这一层。

## 3. 数据规模

de Witte 给出了这个赌注背后的数字：

| 指标 | 台上给出的数字 |
| --- | --- |
| Medal 视频上传量 | 每年约 10 亿条；今年有望到 30 亿 |
| Action tokens | 每月超过 1.5 万亿 |
| 对比 | frontier labs 训练机器人模型用的全部数据，「目前」半天就能采到 |

这些数字来自演讲本身；公司官网只说玩家每年上传数十亿条片段。「我们就是押注像 Anthropic scale coding space 那样去 scale pixel space，」de Witte 说，预期随着数据和算力进入 pixel space，人们会把更多问题带过来。结果会是「一个极其通用的 agent，能去解决电脑上[和]现实世界里的问题」。

## 4. 用 world model 当训练环境

Anthropic 做 coding environments，General Intuition 做 world model，「大家多半是因为 MIRA 的发布认识我们的」，用它生成更多环境让模型学习。公司官网介绍 MIRA 是「a playable multiplayer world model」，与 Kyutai 合作、和 Epic Games 协作，用 1 万小时 bot 生成的 Rocket League 对局训练。官网把两类模型分开：「action models that decide what actions to take」和「world models that predict the outcomes of actions」。

de Witte 认为，LLM 公司要生成学习环境会遇到两个问题。模拟要花算力，越逼真越贵。而且学习应该发生在人自己的 input space 里：一个人想给机器人搭档演示什么，机器人就应该看到同样的东西，而不是先经过一个 LLM、建一个 digital twin、部署模型、再跑「一堆 verifier 检查」之后才学到。「当你能用人的原生模态学习，你一下子就能更快地解决多得多的问题。」de Witte 说，这些模型发布后，做这类事会「比任何 LLM 都快得多」。

## 5. 游戏数据为什么动作密集

游戏「动作非常密集」，de Witte 说，模型可以同时预测好几个动作。这种密度能迁移到要求很高的机器人工作上。操控无人机需要很多细小的调整：「你真的应该从游戏开始，而不是从 teleoperation 开始，因为人在 teleoperation 里做的动作少得多。」模型需要能预测「非常高的 APM」（每分钟操作数），数据集的 APM 让这类问题变得可解。de Witte 说，因为模型还理解所按按键的语义，这对 interpretability 很重要。

## 6. 发布计划和 early access

「我们今年会发布这些模型，」de Witte 说，「这些模型能做到的事让我们自己都震惊了。」在直播里放发布视频会剧透，所以没放；但团队就在纽约，已经开始做现场 demo。

「We have essentially one model which can play video games at the highest level and control over 10 types of robots using the exact same primitives. So pixels in, actions out, completely real time.」（我们基本上有一个模型，能以最高水平玩游戏，并用完全相同的原语控制 10 种以上的机器人。像素进，动作出，完全实时。）early access 会在接下来几个月陆续开放；已经有客户私下在用。名额按加入 waitlist 的先后分配，入口是 slide 上的二维码。公司官网说已经有「a small number of partners」接入商用 API，覆盖 games、simulation 和 robotics。

![Get Early Access](../../slides/10-13-1648-frontier-models-for-acting-in-space-and/01.jpg)
*「The frontier lab for acting in space and time」：early access 页，在会场拍摄。*

de Witte 预期，一旦有了新的原语，开发者会想摆脱 LLM 的不可预测性：「人们已经准备好放下 LLM 的不可预测性，换上更可靠、更快的东西。」de Witte 列举的用途包括：探索用 LLM 生成的环境；测试用户界面，因为模型会像人一样操作界面。「所有这些 LLM 解决不了的问题，因为 LLM 没法真的像人一样去探索。」

这些模型不是用来取代规划的。de Witte 仍然预期用户用一个 orchestrator model 做长期规划，「比如 Gemini 或 Astra」，再把 General Intuition 的模型插进自己的 workload。（字幕写作「Astra」，最可能是 OpenAI 的 GPT-6 Astra，未经确认。）de Witte 把这个时刻比作「早期 LLM 的日子，你拿到一个新玩具」，说早期用户可以在上面建生意。一个例子是科学实验室：能用文字指令可靠地编排机器人。「机器人有意思的地方在于，解决机器人问题的一大难题是造出更多机器人，」de Witte 说；很多被智能卡住的生意，会因为解决了「pixels to actions」而出现。

## 7. 无障碍用例

de Witte 说最兴奋的用例之一是无障碍。因为模型像人一样用电脑，一个只能用一只手的人可以自己控制鼠标，由模型预测键盘动作。又因为模型把用户的动作当作输入，「it's observing as it's acting」，它不会去预测用户已经在做的动作，只补上按键。

「如果你想在这个领域做东西，这会是今年出来的最有意思的玩具，」de Witte 说，「我相当确定。」早期很多用途会在 simulation 里：已经在用其他实验室的模型生成环境的人，现在有东西能替自己去探索这些环境。

## 8. 团队、时间表和 hackathon

Medal 和 General Intuition 的团队都在纽约。de Witte 请大家联系团队看 demo，尤其欢迎加入。公司想招「和我们一样想搞懂这些模型的人」，最好擅长生成新环境；有研究背景更好，「但我自己也没有研究背景，所以没关系」。理想人选做过 end-to-end 交付 AI 产品。

公司有三个团队：world modeling、policy（即 agent）、数据和环境生成。「如果你是工程师，最适合加入的大概是环境生成团队。」

时间上，de Witte 说「我非常期待 11 月」，目前的计划是「11 月底或 12 月初」。公司还会办 hackathon。de Witte 宣布「三场 hackathon」，第一场在纽约、11 月中旬，partner waitlist 上的人会收到邀请邮件；但随后只点了两个城市：「一场在纽约，一场在伦敦」。

![Thank you 页](../../slides/10-13-1648-frontier-models-for-acting-in-space-and/02.jpg)
*结束页：团队合照、de Witte 的联系邮箱和招聘二维码。*

结束页上是团队合照，「Stay in touch!」和 de Witte 在 General Intuition 的邮箱，以及「We're hiring · Scan for open roles」。

## Q&A

de Witte 说还有时间回答「一两个问题」，实际回答了一个。提问本身没有收进麦克风。

### Action labels 和 JEPA

**这和 JEPA 有什么不同？**（从回答推断）de Witte 说，JEPA「不和 action labels 一起训练，基本上押的是推断出来的 action labels」。de Witte 认为 JEPA「在极限情况下是更正确的路线」，但「短期内实用性差很多，因为你得押注 action inference 本身另有一条 scaling law」。General Intuition 喜欢 JEPA 这个概念，模型里也用了一些它的思路，「但说到底它是一种方法，不是产品」，两边会互相借鉴。

de Witte 举例说明 action labels 为什么重要。飞行员动飞机尾部的方向舵，这个变化在视频帧里几乎看不出来。「我们在 action 数据上的 moat，迁移到动作序列上的效果，比迁移到 world model 和生成像素上要好。」所以公司全力投入训练通用的动作预测 foundation model，同时 world model 也用各种技术在做，也欢迎其他公司去做。de Witte 说「再来一个我就得走了」，但演讲在这里结束。

---

**链接：** [日程](https://ai.engineer/nyc/2026/schedule?session=cada670e-c5ea-4977-b3c2-9a68dda26abb) · [Day 1 直播 7:02:19 处](https://www.youtube.com/watch?v=akEqiXFo3GM&t=25339s) · [General Intuition](https://generalintuition.com)

截至撰写时，这场 keynote 还没有单独发布的视频。本文依据官方 Day 1 直播字幕、作者在会场拍的两张 slide 照片和公司官网。[精确版笔记](../precise/10-13-1648-frontier-models-for-acting-in-space-and.md)（中文）保留了直播时间戳、字幕勘误和待确认的问题。
