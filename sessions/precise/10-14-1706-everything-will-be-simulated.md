# Everything will be Simulated：精确版

> Precise notes for this session, compiled from the official livestream captions and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in the livestream), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：直播字幕逐段对照完。没有 slide 照片。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | **没有照片**。原先按拍摄时间归到这场的 6 张，横幅全是上一场「Process Supervision for Agents That Do Long-Horizon Work · Mitchell Troyanovsky · Basis」，已改归 Basis 那场。这场的 demo 以视频为主，内容只能靠讲者口述 |
| 视频 | 没有单独剪辑的视频。用的是第二天的官方直播 [jiUC6IUMdvc](https://www.youtube.com/watch?v=jiUC6IUMdvc&t=30551s)，章节「8:29:11 Everything will be Simulated — Cristóbal Valenzuela (Runway)」，到 8:45:54 Closing Remarks。字幕是自动生成的 |
| 一手资料 | Runway Labs 页面 [labs.runway.com](https://labs.runway.com/)（Project Continuum）；TechCrunch [Runway releases its first world model, adds native audio to latest video model](https://techcrunch.com/2025/12/11/runway-releases-its-first-world-model-adds-native-audio-to-latest-video-model)（2025-12-11，GWM-1）；[Wikipedia: Runway (company)](https://en.wikipedia.org/wiki/Runway_(company)) |

## 基本信息

- 时间地点：2026-10-14（周三）5:06–5:24pm，Simons Room (Metropolitan East)，Featured Keynote，Main Stage，当天最后一场（资料：[日程](https://ai.engineer/nyc/2026/schedule?session=af8422e8-d5f8-4dbd-a918-6360f5bfba80)）
- 讲者：Cristóbal Valenzuela，Runway Co-Founder and Co-CEO（日程）。日程 bio：Runway 是「a global AI research and technology company building foundational AI research models and tools to create real-world superintelligence」；之前是 NYU Tisch School of the Arts 的研究者
- 资料：Runway 2018 年成立，创始人 Cristóbal Valenzuela、Alejandro Matamala、Anastasis Germanidis，三人在 NYU Tisch 的 ITP 项目相识，总部在曼哈顿（[Wikipedia](https://en.wikipedia.org/wiki/Runway_(company))）
- 官方简介（日程）：下一个前沿是理解空间、运动和物理动态的模型；讲能实时模拟真实环境的 world model，以及它给 robotics、digital interfaces 和科学发现带来什么
- 主持人画外音介绍（直播 8:28:41）：「What if AI could do more than generate content and actually simulate the physical world?」

## 1. 开场：「欢迎来到 matrix」

- 讲者说（直播 8:29:11）：「Thank you for staying for the last talk. … welcome to my talk about the matrix and how we're living through it.」做 Runway「for almost a decade now」，「Started here in New York actually maybe around 10 years ago」；要讲在纽约做公司、做研究实验室的经验，以及 video、pixel generation 的方向

## 2. 从 2019 年的「The Orange」到今天

- 讲者说（直播 8:29:50）：「This is our first AI film we ever made. It was done in 2019. … it's called The Orange」，一个橙子从树上掉下来，穿过世界各地；当时的模型生成 128×128 像素，再「forcibly upsample」到放映分辨率；「I think of this as art now. This is like looking at a film from the 1900s」
- 讲者说（直播 8:30:34）：「we started the company in 2018, 2017」；当时要说服自己：只要 scale，就能接近今天的效果。接着放「state-of-the-art video today」：「None of these characters are real. None of those locations are real.」
- 讲者说（直播 8:31:04）：「when I look at stuff like this, I still believe this will look like the orange in just a couple more years.」
- 资料：没找到 Runway 官方关于「The Orange」的记录；一篇 Substack 文章提到 Runway 2018 年成立时 AI 只能生成「the crude sphere of an orange falling from a tree」（[Is Runway the one to bring an artist's soul to AI?](https://earlydaysbymerlin.substack.com/p/is-runway-the-one-to-bring-an-artists)，二手，只看到搜索摘要）。片名、年份和分辨率只来自讲者，【待确认】
- 资料：Wikipedia 写 Runway 成立于 2018 年；讲者说「2018, 2017」

## 3. Runway：艺术和科学的交叉；video 是 optical illusion

- 讲者说（直播 8:31:19）：Runway「at the intersection of both art and science」，「deep roots in artistic and creative communities」，也有「some of the best researchers in the world working on diffusion models and large video models」，是「a team of scientists and artists thinking about the problem of scaling video and world models」
- 讲者说（直播 8:31:49）：公司的核心想法电影「120, 130 years ago」就发现了：「video is an optical illusion. Video is the illusion of movement」，帧拼得够快就有运动。Runway 把它推到下一层：「How do we do next frame prediction fast enough, scale enough」，做的不只是好看的创意视频

## 4. Scaling laws 和涌现：next frame prediction 学到了物理

- 讲者说（直播 8:32:27）：「all is powered by … core scaling laws」，相信 scaling laws 不只适用于语言模型，也适用于视频，「this has been true」；「We're now really ramping up the amount of compute」；有意思的是 scale 之后的「emerging properties」
- 讲者说（直播 8:33:04）：拿一张图（两个不存在的人），不断预测下一帧，够快、网络够大，就有运动，「something that looks so real to us that we humans can [not] distinguish」；「I've been doing this for 10 years. I can't tell you if that's real or not.」
- 讲者说（直播 8:33:29）：「we measure that. We actually measure and try to quantify if people can distinguish between real and generated videos. And I think that Turing test we already passed … people are not able to distinguish.」（具体测量方法和数字没讲，【待确认】）
- 讲者说（直播 8:33:43）：预测下一帧学到的不只是好看的帧：「You're learning about physics. You're learning about object permanence. You're learning about gravity. … occlusion. … movements」，全部来自 video、images、next frame prediction
- 讲者说（直播 8:34:05）：举一个复杂画面：模型要生成一致的视频，就得理解各个主体、它们的关系、「how a camel might probably stand on a ball like that, what water in a container looks like」
- 讲者说（直播 8:34:30）：评估视频模型的 benchmark 不只是 creative（「do you like it or do you don't」），而是基于「physics, on geometries, on how objects behave in the real world」，还有 simulators / video worlds（字幕作「simulator or vido worlds」）

## 5. 从视频到可玩的世界：没有物理定律的物理引擎

- 讲者说（直播 8:35:01）：同样的 next frame prediction 放到 3D 环境或类似游戏的场景，「You can start actually getting closer to a physics engine, a simulation system. But the simulation system has no physics laws. We didn't describe what the behavior of the object should be.」模型靠「brute force of scale」学会
- 讲者说（直播 8:35:33）：「This is not GTA. This is someone playing … in an environment and a software that doesn't exist. It's all simulation of a simulation」，「I'm not kidding when I said welcome to my matrix simulation」
- 资料：Runway 2025-12-11 发布首个 world model GWM-1，用「frame-by-frame prediction」构建模拟；有 GWM-Worlds（24 fps、720p 的交互式场景）、GWM-Robotics、GWM-Avatars 三个变体（[TechCrunch](https://techcrunch.com/2025/12/11/runway-releases-its-first-world-model-adds-native-audio-to-latest-video-model)）。讲者没有点名 GWM，我的理解：这一段的 demo 对应这类 world model

## 6. 实时和交互

- 讲者说（直播 8:35:54）：前面看到的都「was not real time」，生成要几秒到几分钟。未来三四年的重点：不只把模型做大、获得更多涌现能力，还要让它「interactive」：在逐帧预测的任意时刻插入动作，「I can simulate any possible divergence or action in that world」；可以播放视频，也可以「touch every part, any aspect of that image and make it interactive」，「and it works」

## 7. Robotics lab

- 讲者说（直播 8:36:54）：「we're now spinning an entire robotics lab in Runway」；纽约办公室「half of it is what looks like a Hollywood studio. Half of it is a bunch of robots walking through and moving through the office」；目标是让这些模型成为「the brains of how autonomous and embodiment systems walk and reason about the world」
- 资料：GWM-Robotics 用合成数据（加入天气、障碍等变化）训练和测试机器人策略，计划以 SDK 提供（[TechCrunch](https://techcrunch.com/2025/12/11/runway-releases-its-first-world-model-adds-native-audio-to-latest-video-model)）

## 8. 屏幕上的任何像素都是视频：world interface model

- 讲者说（直播 8:37:23）：video 和 robotics 是两个明显的用例，更有意思的是「what happens when you think about anything, any pixel on a screen as a video model」
- 讲者说（直播 8:37:37）demo：一张汽车图片，实时跑视频模型变成交互应用：开车灯、把车转过来、再转回去、打开引擎盖看里面；「everything you just saw here has no physical 3D representation. There's no Unity. There's no Unreal. There's no Blender. It's just a video model」，训练成「understand actions and clicks in an interface」
- 讲者说（直播 8:38:14）：「We call this a world action model. Um, sorry, um, a world interface model.」
- 讲者说（直播 8:38:29）第二个 demo：拿一张房间图片，移动里面的物体，「expect them to move as if I was actually in that room」；「there's no 3D representation」，这个环境可能从没被建模过，「It's just an image and the model that understands how objects move in a space」
- 资料（二手）：Runway 2026-08-31 发布 Solaris，称为「Interface World Model」系列的第一个模型，逐帧渲染界面、没有 HTML/CSS/DOM（[OrcaRouter 博客](https://www.orcarouter.ai/blog/runway-project-continuum-early-look)、[pasqualepillitteri.it](https://pasqualepillitteri.it/en/news/23148/runway-project-continuum-computer-without-apps)）。Runway 官方公告没找到，名字和日期【待确认】；讲者现场说的是「world interface model」，没有提 Solaris

## 9. 外推：Project Continuum

- 讲者说（直播 8:39:03）：「with the eight minutes I have left」，把这个想法推到极端。「extrapolating might be one of the most important skills these days in AI, in company building」；Runway 常问：继续 scale、加数据、做成「universal simulation systems that can predict anything on a screen」会怎样
- 讲者说（直播 8:39:49）：「we call continuum」，「the idea of real time video models or world models taken really to the next degree. This is an operating system.」将来打开电脑，「what you see streaming to your screen might be coming from a video model」；「This is still very early research.」
- 资料：[Runway Labs](https://labs.runway.com/) 列出 Project Continuum（2026），「An operating system research application designed for real-time video interfaces.」四种交互：Portals「Select anything; a place opens over it」；Visual Thinking「An agent's steps, drawn as it works」；Responsive interfaces「One interface, re-rendered at every size」；Worlds「Step through a portal into a world」
- 资料（二手）：Continuum 2026-10-01 由 Runway Labs 公布，是研究原型，没有下载、价格或 API（[pasqualepillitteri.it](https://pasqualepillitteri.it/en/news/23148/runway-project-continuum-computer-without-apps)）；Runway 在 X 上的公告标题见搜索结果（「Introducing Project Continuum by @runwayml_labs, a new operating system research application built around real-time video interfaces」），原帖没能打开，日期【待确认】

### 9.1 Portals

- 讲者说（直播 8:40:25）：研究旧金山的雾，什么时候、怎么形成。「This is structural data. This is actually a website.」Continuum 可以选中网页里任何一段内容「and make it a portal」；portal「is a real-time video. It's fully interactive and fully based on the content you're … reading」；「It's too much text for me. So I just want to make a video」；portal 可以是 3D 可视化或任何内容
- 讲者说（直播 8:41:04）：右边列着看过的所有 visualization 和 portal，「basically it's a memory」；比如之前研究电池的 portal，可以回去点、移动、提问，「the videos and the content itself will change based on my interactions」

### 9.2 Responsive video interfaces

- 讲者说（直播 8:41:41）：「when the first time I saw it when the team showed it to me it was like blew my mind」：因为是实时视频，调整窗口大小也会改变行为；学 photosynthesis，「If I resize the application, I get a different experience」，还能点进细胞各部分
- 讲者说（直播 8:42:14）：「I don't know how to call this. … doesn't feel like a visualization. It doesn't feel like a video. It doesn't feel like a game. Something in between.」需要大家一起想怎么叫这种交互；也可以改 prompt 的结构来改变交互

### 9.3 Visual thinking

- 讲者说（直播 8:42:49）：「one of the ones that I feel has been the most under-explored」，「we call this visual thinking」：把 Continuum 接到一个 coding agent，agent 可以用 Continuum 把问题可视化。例子：让 coding agent 给你的自行车做 3D 模型，模型不只用代码思考，而是「spin a version of continuum to think through the problem visually」
- 讲者说（直播 8:43:35）：「Agents have the ability to do visual thinking. Agents can spin up a sort of imagination」，agent 能交互，人也能看到并「guide the agent in much better ways that I might have able to guide it just via text alone」；「It's a completely new paradigm.」
- 资料：Runway Labs 对 Visual Thinking 的描述「An agent's steps, drawn as it works」；讲者没提用的是哪个 coding agent

### 9.4 Live, reactive experiences（Mars）

- 讲者说（直播 8:44:04）：最后一个例子：搜索 Mars，Continuum 理解意图后「suggests interfaces that are specifically designed in real time just for that」；生成了一个 Mars outpost，「should be accurate with the data about the actual robots that are in Mars right now or with the ones that are projected maybe in 2030」；可以在里面走动，「in a 3D continuum」，「and it works, just fascinating」
- 我的理解：对应 Runway Labs 的第四种交互 Worlds；讲者没用这个词

## 10. 结论：「everything will be simulated」

- 讲者说（直播 8:45:06）：说 everything will be simulated，是想让团队和外界意识到，今天看到的很多东西会变得「obvious」；「we'll look back at Continuum and it will might feel very close to the video of the orange」
- 讲者说（直播 8:45:27）：Continuum 和 Runway 的模拟工作「are still in very early days, but we're getting to a point where it actually works and it really scales」；「for the next 8 to 12 months, we're releasing most of what I showed here for everyone to use. Thank you guys.」

## Q&A 精确版

没有 Q&A。直播在 8:45:44 掌声后进入 8:45:54 的 Closing Remarks。

## 勘误表（YouTube 自动字幕 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| Christoval Valenuela（主持人介绍） | Cristóbal Valenzuela | 日程 |
| scaling loss | scaling laws | 上下文 |
| delusion movement / the illusion | the illusion of movement | 上下文 |
| we humans can distinguish | we humans can't distinguish | 上下文（下一句「I can't tell you if that's real or not」） |
| real and generic videos | real and generated videos | 上下文 |
| touring test | Turing test | 上下文 |
| simulator or vido worlds | simulators or video worlds【待确认】 | 上下文 |
| lecture and prediction / an extra prediction | next frame prediction | 上下文 |
| poor force of scale | brute force of scale | 上下文 |
| metric simulation | matrix simulation | 开场「my talk about the matrix」 |
| board both art and science | both art and science | 上下文 |
| Continum | Continuum | Runway Labs 页面 |
| unsan | 【待确认】（可能是 uncanny） | 上下文 |
| the disk of the imaginations | 【待确认】 | 上下文 |
| visual visual thinking | visual thinking | Runway Labs 页面 |

## 出入

1. **模型名字**：讲者先说「world action model」，马上改口「world interface model」；二手报道里 Runway 的叫法是「Interface World Model」（Solaris），词序不同，且官方公告没核对到，【待确认】
2. **成立时间**：讲者说「2018, 2017」，又说「maybe around 10 years ago」「almost a decade」；Wikipedia 写 2018 年
3. **讲者头衔**：日程和主持人介绍是 Co-Founder and Co-CEO；Wikipedia 写 CEO
4. **四种交互的叫法**：Runway Labs 页面是 Portals / Visual Thinking / Responsive interfaces / Worlds；讲者口头用了 portal、visual thinking，resize 和 Mars 两个 demo 没有用页面上的名字
5. **发布时间**：讲者说「for the next 8 to 12 months, we're releasing most of what I showed here」；二手报道说 Continuum 目前没有下载、价格、API 或发布日期

## 【待确认】清单

- 「The Orange」（2019）的片名、年份、128×128 分辨率只来自讲者，没找到官方记录
- 「Turing test we already passed」背后的测量方法和数据
- Solaris / Interface World Model 的官方公告和日期
- Project Continuum 公布日期（二手报道写 2026-10-01）
- 字幕「unsan」「the disk of the imaginations」「simulator or vido worlds」的原话
