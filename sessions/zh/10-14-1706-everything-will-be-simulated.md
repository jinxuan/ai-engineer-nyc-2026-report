[English](../10-14-1706-everything-will-be-simulated.md) | **中文**

# Runway 押注：next-frame prediction 会变成屏幕上一切的模拟器

*在 AI Engineer NYC 2026 上，Runway 联合创始人 Cristóbal Valenzuela 认为，只学预测下一帧的视频模型正在学会物理、变得可以实时交互，最终可能直接渲染电脑界面本身；Valenzuela 还展示了一个叫 Continuum 的操作系统的早期研究 demo。*

**Featured Keynote · 2026 年 10 月 14 日（周三）· 5:06–5:24pm · Simons Room (Metropolitan East)，Main Stage**
**讲者：** Cristóbal Valenzuela，Co-Founder and Co-CEO，Runway

---

Cristóbal Valenzuela 讲的是大会主舞台的最后一场。「谢谢大家留到最后一场，」Valenzuela 说，「欢迎来听我讲 matrix，以及我们怎么活在里面。」Valenzuela 说自己做 Runway「快十年了」，「大概十年前」从纽约起步；想分享公司在这座城市做研究实验室的经验，以及 Runway 在视频和像素生成上往哪走。

## 1. 从「The Orange」到真假难辨的视频

Valenzuela 先放了一段自称「史前古迹」的东西：Runway 做的第一部 AI 影片，2019 年完成，名叫 *The Orange*，讲一个橙子从树上掉下来，穿过世界各地。背后的模型生成 128×128 像素的画面，团队再「硬生生上采样」到放映分辨率。「我现在把它当艺术看，」Valenzuela 说，「就像在看一部 1900 年代的电影。」

Valenzuela 说公司是「2018、2017 年」成立的（Wikipedia 写 2018 年）。当时团队想说服自己：把这类模型 scale 上去，总有一天能接近 Valenzuela 接下来放的东西，「今天 state-of-the-art 的视频。全部是生成的。这些人物没有一个是真的，这些地方没有一个是真的」。Valenzuela 又说：「我看着这些东西，仍然相信再过几年，它们看起来也会像那个橙子一样。」

## 2. 艺术、科学和运动的错觉

Valenzuela 说，Runway 处在「艺术和科学的交叉点」，「深深扎根于艺术和创作社区」，也有「一些全世界最好的研究者在做 diffusion model 和大型视频模型」，是「一群科学家和艺术家，在想怎么 scale 视频和 world model」。

Valenzuela 认为，公司的核心想法电影在 120 到 130 年前就发现了。「Video is an optical illusion. Video is the illusion of movement.」（视频是一种视错觉，是运动的错觉。）帧拼得够快，就有了 motion picture。Runway 把这个想法再往前推：「怎么把 next frame prediction 做得够快、scale 得够大」，让结果不只是好看的创意视频。

## 3. Scaling laws，以及 next-frame prediction 学到了什么

Valenzuela 说，这一切「靠 scaling laws 驱动」。Runway 相信 scaling laws 不只适用于语言，也适用于视频，「事实证明是这样。我们现在正在大幅加大算力」。Valenzuela 感兴趣的是模型 scale 之后「涌现出来的性质、涌现出来的智能」。

拿一张两个不存在的人的图片，根据前面的帧不断预测下一帧。「做得够快、网络够大，我就能得到运动，」Valenzuela 说，得到人分辨不出真假的东西。「我做这个十年了，我也没法告诉你那是不是真的。」Valenzuela 说 Runway 会测这件事：「我们确实在测量、量化人能不能区分真实视频和生成视频。我觉得那个 Turing test 我们已经过了，人已经分辨不出来。」Valenzuela 没有讲具体怎么测。

Valenzuela 说，更重要的是，预测帧让模型学到的远不止帧。「你在学物理，学 object permanence，学重力，学遮挡，学运动」，全部来自视频和 next-frame prediction。要把一个复杂画面渲染得前后一致，模型得理解每个主体是什么、它们之间什么关系，「一头骆驼大概怎么站在那样一个球上，容器里的水长什么样」。所以 Runway 评估视频模型用的很多 benchmark，看的不是人喜不喜欢，而是「物理、几何、物体在真实世界里怎么运动」。

## 4. 没有物理定律的物理引擎

Valenzuela 说，把同样的 next-frame prediction 放到 3D 或类似游戏的环境里，「就开始接近一个物理引擎、一个模拟系统。但这个模拟系统没有物理定律。我们没有描述物体应该怎么运动」。模型靠 scale 的蛮力学会。有一个 demo 看起来像电子游戏：「这不是 GTA。这是有人在一个不存在的环境、不存在的软件里玩。全是模拟的模拟。」「我说欢迎来到我的 matrix simulation，可不是开玩笑，」Valenzuela 说。

Valenzuela 没有点名是哪个模型。Runway 2025 年 12 月发布了第一个 world model 系列 GWM-1；[TechCrunch 报道](https://techcrunch.com/2025/12/11/runway-releases-its-first-world-model-adds-native-audio-to-latest-video-model)说它通过「frame-by-frame prediction」构建模拟，有 Worlds、Robotics、Avatars 三个变体。

## 5. 实时和交互

到这里放的东西都要几秒到几分钟才能生成。Valenzuela 说，未来三四年的问题是：模型不只是 scale 上去，还变得可交互，会怎样。一帧接一帧地预测，但允许在任意时刻插入一个动作，「也就是说我可以模拟这个世界里任何可能的分岔或动作」。观众可以播放视频，「也可以过来碰这张图的任何部分、任何地方，让它可交互，去影响这个世界接下来发生什么。而且它真的能用」。

一个用途是 robotics。「我们现在在 Runway 内部搭了一整个 robotics lab，」Valenzuela 说。「如果你来我们纽约的办公室，一半看起来像好莱坞摄影棚，另一半是一堆机器人在办公室里走来走去。」目标是让这些模型成为「自主系统和具身系统怎么行走、怎么理解世界的大脑」。（按 TechCrunch 的报道，GWM-Robotics 用合成数据来训练和测试机器人策略。）

## 6. 屏幕上的任何像素：world interface model

Valenzuela 说，在视频和 robotics 之外，更有意思的是「把任何东西、屏幕上的任何像素都当成一个视频模型」。

第一个 demo 从一张汽车图片开始。实时跑 Runway 的一个视频模型，这张图就成了一个交互应用：Valenzuela 点击打开车灯，把车转过去再转回来，打开引擎盖看里面。「你刚才看到的一切都没有物理意义上的 3D 表示。没有 Unity，没有 Unreal，没有 Blender。就是一个视频模型」，训练成「能理解界面里的动作和点击」。「我们管这个叫 world action model，」Valenzuela 说，然后改口：「抱歉，是 world interface model。」（二手报道称 Runway 把它的界面模型叫 Solaris，是「Interface World Model」系列的第一个；Runway 的官方公告没能核对。）

第二个 demo 里，Valenzuela 在一张房间照片里移动物体，物体的移动就「像我真的在那个房间里一样」。「记住，这里没有任何 3D 表示。这个环境可能从来没被建模过。只有一张图，和一个理解物体在空间里怎么运动的模型」，都是因为「next frame prediction scale 得够好，就能变成 world model 或模拟引擎」。

## 7. Continuum：由视频模型串流出来的操作系统

「用我剩下的八分钟，」Valenzuela 说，下一步是把这个想法推到极端。Valenzuela 说，外推「可能是如今在 AI、在创业里最重要的技能之一」：接下来六个月、十二个月、十八个月会发生什么。Runway 常问：如果一直 scale 模型、加数据，把它们做成「能预测屏幕上任何东西的 universal simulation system，真的是屏幕上的任何东西」，会怎样。

结果是一个叫 Continuum 的研究项目。「这是一个操作系统，」Valenzuela 说。「也许不久的将来你打开电脑，屏幕上串流出来的东西就来自一个视频模型。」Valenzuela 强调它「还是很早期的研究」。[Runway Labs](https://labs.runway.com/) 把 Project Continuum 描述为「an operating system research application designed for real-time video interfaces」，列了四种交互，和台上的四个 demo 对得上：

| Runway Labs 名称 | Labs 页面的描述 | 台上的 demo |
| --- | --- | --- |
| Portals | 「Select anything; a place opens over it」 | 旧金山的雾；电池 |
| Visual Thinking | 「An agent's steps, drawn as it works」 | coding agent 设计自行车 |
| Responsive interfaces | 「One interface, re-rendered at every size」 | 光合作用，调整窗口大小 |
| Worlds | 「Step through a portal into a world」 | 火星前哨站 |

**Portals。** Valenzuela 在一个普通网站上查旧金山的雾。在 Continuum 里，「我可以选中网站里任何一段内容，把它变成一个 portal」。portal「是一段实时视频，完全可交互，完全基于你正在读的内容」。「字太多了，我只想要一段视频。」portal 可以是 3D 可视化或其他内容，右边一栏保存着用户打开过的所有 portal，「基本上就是一份记忆」。回到之前一个关于电池的 portal，Valenzuela 点击、移动其中的部分并提问，「视频和内容本身会随着我的交互而变化」。

**Responsive interfaces。** 「团队第一次给我看的时候，我真的被震住了，」Valenzuela 说。因为界面是实时视频，调整窗口大小会改变它显示的内容。在一个关于光合作用的 portal 里，「我调整应用的大小，就得到不同的体验」，Valenzuela 还能点进细胞的各个部分。「我不知道该怎么叫它，」Valenzuela 说。「它不像可视化，不像视频，也不像游戏，介于它们之间。」Valenzuela 说，给这类交互起名字是大家还要一起做的事。

**Visual thinking。** Valenzuela 说这是最没被充分探索的想法。Runway 把 Continuum 接到一个 coding agent 上，让 agent 能把问题可视化。让它给一辆自行车做 3D 模型，「模型不只是用代码去想，而是能起一个 Continuum 实例，用视觉去想这个问题」，就像擅长视觉思考的人那样。「Agent 能起一种想象。」agent 和它交互，用户也能看到，从而「比只靠文字更好地引导 agent」。「这是一个全新的 paradigm，」Valenzuela 说。

**实时响应的世界。** 最后一个 demo 里，Valenzuela 搜索火星。Continuum 推断出意图，「实时给出专门为此设计的界面」，这次是一个火星前哨站，「应该和现在火星上真实的机器人数据一致，或者和预计 2030 年左右会有的那些一致」。Valenzuela 像在 3D 系统里一样在里面走动。「而且它能用。太神奇了。」

## 8. 「Everything will be simulated」

Valenzuela 说，这个标题是想帮 Runway 的团队和外界走到一个点：今天展示的很多东西到时候「会听起来理所当然」。「希望再过几年或几个月，我们回头看 Continuum，会觉得它很像刚才那个橙子的视频。」Continuum 和 Runway 的模拟工作「还在非常早期，但我们正走到它真的能用、真的能 scale 的地步」。Valenzuela 最后给了发布计划：「接下来 8 到 12 个月，我们会把今天展示的大部分东西发布出来，给所有人用。」（关于 2026 年 10 月 Continuum 公布的二手报道称，目前还没有下载、定价或 API。）

---

**链接：** [Schedule](https://ai.engineer/nyc/2026/schedule?session=af8422e8-d5f8-4dbd-a918-6360f5bfba80) · [直播 8:29:11 处](https://www.youtube.com/watch?v=jiUC6IUMdvc&t=30551s) · [Runway Labs: Project Continuum](https://labs.runway.com/) · [Runway releases its first world model](https://techcrunch.com/2025/12/11/runway-releases-its-first-world-model-adds-native-audio-to-latest-video-model)（TechCrunch，GWM-1）

本文依据官方直播字幕和上面列出的资料。作者没有这场演讲的幻灯片照片（演讲的 demo 以视频为主），截至撰写时也没有单独发布的视频。演讲结束后没有 Q&A。[精确笔记](../precise/10-14-1706-everything-will-be-simulated.md)（中文）保留了直播时间戳，并列出了没能核实的细节，包括 *The Orange* 的片名和年份。
