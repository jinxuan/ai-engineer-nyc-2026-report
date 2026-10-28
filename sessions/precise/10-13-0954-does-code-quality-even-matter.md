# Does Code Quality Even Matter?：精确版

> Precise notes for this session, compiled from the official video captions, the Day 1 livestream captions, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：单独视频字幕逐句读完，和直播切片对过；16 张照片逐张核对 banner 和内容。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | 作者拍的 16 张（`01.jpg`–`16.jpg`），都是直播画面的手机翻拍，下方 banner 都是「Does Code Quality Even Matter? · Max Kanat-Alexander / Principal Software Engineer · Capital One」，全部属于这场。原始文件夹里按时间分进来的另外几张 banner 是上一场 Two Sigma（John Feminella）的，不用。讲者讲到的「怎样让软件更 disposable」和「降低 rigor 成本」两张列表页没有照片 |
| 视频 | [bU5gDVyu_28](https://www.youtube.com/watch?v=bU5gDVyu_28)，改名「Does Code Quality Still Matter? It Depends on This — Max Kanat-Alexander, Capital One」，16:23，上传 2026-10-21。字幕是主要文本。视频描述带章节（见下） |
| 直播 | Day 1 官方直播 [akEqiXFo3GM](https://www.youtube.com/watch?v=akEqiXFo3GM&t=2596s)，章节「43:51 Does Code Quality Even Matter? — Max Kanat-Alexander (Capital One)」，到 1:00:43 下一场。主持人介绍在直播 43:16，讲者开口在 43:53。**视频时间 + 43:41 ≈ 直播时间**（视频 00:12 = 直播 43:53）。直播和视频内容一致，视频只剪掉了主持人介绍 |
| 官方 talk 页 | `ai.engineer/talks/bU5gDVyu_28` 返回 404（2026-10-27） |
| 其他一手资料 | Chromium 源码 [url/third_party/mozilla/url_parse.cc](https://chromium.googlesource.com/chromium/src/+/refs/heads/main/url/third_party/mozilla/url_parse.cc)（核对 slide 上的函数名）；讲者网站 [codesimplicity.com](https://www.codesimplicity.com/) |

视频章节（资料：YouTube 描述）：00:00「It depends」— on what? · 01:14 Understanding and execution · 02:42 Why structure matters · 04:47 The two risks of every change · 06:56 Disposable software · 07:59 Durable software · 09:54 Making software more disposable · 12:05 Lowering the cost of rigor · 15:50 The answer

## 基本信息

- 时间地点：2026-10-13（周二）9:54–10:12am，Simons Room (Metropolitan East)，Featured Keynote，Main Stage（资料：[日程](https://ai.engineer/nyc/2026/schedule?session=5191589c-a70e-4806-99dc-86f7794f46b9)）
- 讲者：Max Kanat-Alexander，Principal Software Engineer，Capital One（日程、banner）
- 资料：日程 bio：Capital One 的 AI-Assisted Software Development 技术负责人；之前是 Bugzilla 项目的 Chief Architect、Google 的 Code Health 技术负责人、LinkedIn 的 Developer Productivity and Happiness 技术负责人；著有 *Code Simplicity* 和 *Understanding Software*，发布过 Google 的 code review guidelines 和 LinkedIn 的 Developer Productivity and Happiness Framework；「He spends about 5 hours a day talking to his best friends Claude and Codex.」
- 直播 43:16 主持人介绍：「principal software engineer at Capital One and author of Code Simplicity」
- 官方简介（日程）：网上关于「should we read the code?」的争论，一边喊「Everybody should build everything with a dark factory!」，一边喊「The slop will destroy us all!」；缺的是做这类决定的原则。讲者会讲他认为在 AI coding 下什么是对的
- 资料：视频描述把结论概括为：code quality 只为两件事存在（理解系统、安全地修改它）；AI 让 disposable software 变得常见；对它质量不重要，对 durable software「it matters more than ever」；以及怎样让系统更 disposable、怎样用 standards、in-workflow tooling、easy testing、maximum strictness 让 rigor 变便宜。（我的理解：「matters more than ever」是描述的措辞，讲者在台上没有这么说）

## 1. 开场：「it depends」到底取决于什么

- slide 01：左白底「ABSOLUTELY NO」，右黑底「ABSOLUTELY YES」
- 讲者说（视频 00:12）：反讽开场，「a nice uncontroversial topic that nobody has any opinions about」，社交媒体上每个观点都「really deeply nuanced」，「maybe there's a little bit more to it than like yes or no」
- 讲者说（视频 00:38）：senior engineer 对每个问题的标准答案是「it depends」，「that's not a very helpful answer though, frankly, because what does it depend on?」
- 讲者说（视频 00:57）：先问 code quality 当初为什么重要，「I see a lot of people be like, well, it made me feel good. That's not really a sufficient justification.」这场 talk 要搭一个能拿来推理的 framework

## 2. 一切取决于理解和执行

- slide 02：「1. Understanding　2. Executing on that understanding」
- 讲者说（视频 01:18）：想用软件完成的任何事都「depends on two factors and two factors alone」：理解，以及基于理解去执行，不论是「aligning a model, scaling a system, driving adoption」。debuggers、observability、evals、tests 都是为了提高理解或执行能力
- 讲者说（视频 01:48）：「Everything we have ever done in the field of code quality is exclusively and only for these two things.」和 correctness 之类别的东西无关
- slide 03：「1. Behavior　2. How to correctly modify it」
- 讲者说（视频 02:02）：软件工程师最重要的两种理解：系统的实际行为是什么（「not what does a doc say the behavior of the system is」），以及怎样正确、安全地修改这个行为

## 3. 为什么结构重要

- slide 04：`print("Hello World")` → `print("Hi World")`
- 讲者说（视频 02:30）：很小的系统，理解就是它做什么：打印 hello world，要改成 hi world 就改
- slide 05：一张 Chromium 代码库的全景可视化（密集的连线和区块）
- 讲者说（视频 02:52）：「This is a real map of the code base of Chromium, which is the open-source project behind Chrome.」组件之间的交互方式使它有「essentially infinite possible behaviors」，「I could not describe to you in words the behavior of Chromium as a whole」。这张图的出处讲者没说，【待确认】
- slide 06：URL 解析调用树，顶部 `https://example.com:443/articles?id=7#summary`，节点 ParseStandardUrl → DoExtractScheme、DoParseAfterSpecialScheme → DoParseAuthority、FindNextAuthorityTerminator、ParsePath → ParseUserInfo、ParseServerInfo、FindQueryAndRefParts
- 资料：这些函数名在 Chromium 的 [url_parse.cc](https://chromium.googlesource.com/chromium/src/+/refs/heads/main/url/third_party/mozilla/url_parse.cc) 里都存在（ParseStandardUrl、DoParseAfterSpecialScheme、DoParseAuthority、FindNextAuthorityTerminator、ParsePath、ParseUserInfo、ParseServerInfo、FindQueryAndRefParts；DoExtractScheme 也在）
- 讲者说（视频 03:17）：整体说不清，但「I can successfully describe to you how a component of the system behaves」，比如 URL parser；「this is the whole reason that code exists … for structure」
- slide 07：左 `print("Hello World")`，右一段 x86-64 汇编（`mov eax, 1` / `syscall` / `mov eax, 60` / `msg: db "Hello World", 10`）
- 讲者说（视频 03:54）：两边在 Linux 上都打印 Hello World；左边「has the identity and structure of Hello World」，右边只是「a series of instructions with no particular identity」，如果有几百万、几十亿行，「it would be much harder to grok」
- 讲者说（视频 04:25）：过去讲 readability、testability 这些像是「human concepts about code」，「but what actually mattered the most always was that the system had some sort of understandable structure」

## 4. 每次修改的两类风险

- slide 08：「1. Probability of introducing a problem　2. Making it more likely to have future problems」
- 讲者说（视频 04:51）：所有修改都有两类风险：引入问题（不只是 bug，也包括项目拖期、不符合用户需要、预算超支），以及让以后的修改更难，即增加未来风险
- slide 09：在两条下面分别加上「→ difficulty of understanding」「→ making it harder to understand」
- 讲者说（视频 05:25）：两者都源于理解的问题；「I can reduce every postmortem I have ever read for any incident to these two factors」，比如生产配置太难懂、有人写了没法懂的东西、系统的 emergent properties 没法理解
- 讲者说（视频 05:56）：第二点反过来喂第一点，「a vicious cycle that compounds in a non-linear fashion」；所有系统最终都会走到「every modification involves some sort of problem」，比如「giant 100 million 500 million line mainframe code bases」
- 讲者说（视频 06:41）：也许未来模型能「infinite ability to deal with complexity」，但今天不讨论，因为模型正在做另一件更有意思的事

## 5. Disposable software

- 讲者说（视频 06:56）：模型在创造「a new category of software. This category has always existed, but it's about to become a hundred thousand times more common than any other form of software. And that is disposable software.」
- slide 10：「Consequences of failure · People/systems that depend on it (change management) · Cost of replacement · Cost of maintenance」
- 讲者说（视频 07:17）：设想失败的后果很小或没有、几乎没人和别的系统依赖它、change management 几乎不存在、替换成本接近零（「replace the whole thing in hours or days」）、维护成本几乎为零：「In that world, code quality does not matter.」因为要防的复利风险不存在

## 6. Durable software

- slide 11（两栏）：

| Durable Software | Disposable Software |
| --- | --- |
| Lives for years | Replaceable in hours |
| Depended on by millions | Depended on by a few, or many who don't care if it changes |
| Major consequences of failure | If it doesn't work, I'll just make my own |
| Requires continuous maintenance | Maintenance is unimportant or nearly free |

- 讲者说（视频 08:00）：传统上我们一直在造 durable software，「depended on by millions or thousands of people」，失败可能是「business catastrophic」；「Most of the software engineers in the world spend most of their time maintaining these giant durable systems.」
- 讲者说（视频 08:21）：「I expect this to continue to exist」，这是社交媒体讨论跑偏的地方。durable 系统可能仍是世界上最重要的，而且可能更多，「But the most numerous systems will be disposable software.」
- 讲者说（视频 08:50）：「if it doesn't work, you will give everybody agents. They'll make a version of it themselves.」例子：「Don't like the calculator on your OS? Make your own. Want to plan a party for your kids? Write a party planner software just for that one party. Throw it away. Code quality doesn't matter. Nobody cares.」

## 7. 按 disposability 决定 rigor

- slide 12：横向双箭头，左「Less Rigor / More Disposable」，右「More Rigor / More Durable」
- 讲者说（视频 09:12）：「In the future, we will apply engineering rigor to the degree of disposability of the software.」左端「None of our practices of engineering rigor matter at all」，右端「all the practices of engineering that we have built up over the last many decades matter greatly」
- slide 13：同一条轴，左 Disposable 下「Consequences / Cost of replacement」，右 Durable 下「Dependencies / Cost of maintenance」
- 讲者说（视频 09:31）：位置由四个因素决定：consequences、cost of replacement、dependencies、cost of maintenance；全低是 disposable，全高是 durable。「you should err on the side of it being durable. You should err on these all being higher than you expect them to be.」
- 我的理解：slide 13 把四个因素分放在轴的两端只是排版，讲者口头说的是四个因素一起决定位置

## 8. 让软件更 disposable

- 讲者说（视频 09:54）：新做法是「You can make software more disposable」。没有这一页的照片，以下按讲者口述：
  - 隔离环境（better isolated environments）：只能部署到只接触你自己数据的环境，「No customer data, no personally identifiable information」
  - 强制独立（enforce independence）：「no inbound API calls from other production systems are allowed to this system in this environment」，于是部署到那里的东西都是 disposable 的
  - 冗余（redundancy）：保留一个「old clunky backup system」，大家可以 vibe code 自己更高效的前端，坏了还有旧系统可用，「Which it will fail, right? That's fine. But, it doesn't matter if it fails.」
  - 限制范围（limit scope）：只给一个人的计算器、只为一场派对
- 讲者说（视频 11:00）：「I think this is a whole new discipline of software engineering」：不是提高 durable 系统的 rigor，而是怎样让系统允许、鼓励 disposability

## 9. Durable software engineering 不会消失，降低 rigor 的成本

- slide 14：「Durable software engineering is not going away.」
- 讲者说（视频 11:24）：「this is the biggest confusion that I see on social media today. In fact, I believe the absolute amount of durable software in the world will increase because AI coding tools make it easier and easier to make more durable software.」
- 讲者说（视频 11:46）：disposable 和 durable 不是非黑即白，是光谱；沿着光谱变化的是投入多少工程 rigor 的努力，「both immediate effort and effort over time」
- slide 15：曲线图，纵轴 Engineering Effort，横轴 More Disposable → More Durable；上面一条「Without better foundations」陡升，下面一条「With better foundations」平缓，两线之间阴影
- 讲者说（视频 12:05）：最该关心的是「reducing the cost of effort, reducing the cost of rigor」；rigor 越便宜，越不用担心自己在光谱的哪里。判断错了（以为 disposable，后来要 durable）时，「The cheaper you make it to make it durable, the easier it is to change your mind or to be wrong because you're never going to be 100% right all the time on that estimate.」
- 讲者说（视频 12:43）：这需要另一套实践，「not engineering rigor themselves, but that are about how to decrease the cost of engineering rigor」；「I don't have 10 hours. I have about 5 minutes left on that timer.」
- 我的理解：slide 15 的「better foundations」就是下面讲的这些降低 rigor 成本的实践，讲者没有口头解释这张图

## 10. 降低 rigor 成本的四个做法

没有这一页的照片，以下按讲者口述（视频 13:05 起）：

- **Standardization**：「we do standardization because it removes possibilities」。系统里没有 PHP，就不用操心 PHP 的 linter、实践和安全漏洞；框架、RPC 协议同理：「Yes, you could still have the agent code up all your tools, but you have to do it every single time.」
- **In-workflow tooling**（讲者说视频 13:54）：「tooling that just occurs. The programmer doesn't have to choose to run it. They don't have to understand how it runs.」要可靠（false positive 少）、要快；「If it's free and fast, who cares?」这样也能用在 disposable software 上
- **Making testing easier**（视频 14:19）：不是怎样做更好的测试，而是「how do you make it so that nobody has to think about how to do better testing?」每个测试都快且确定性，不需要人人是测试专家。他作为 test owner 一直的梦想是每个团队都提供 in-memory fake 版本的服务，「Guess what? The coding agent can do that. So, you can just do that now, right? But you have to choose to do it and make it available to everybody.」
- **Maximum strictness**（视频 15:00）：所有 lint 规则开到最大，「You never compromise on anything」，和 standardization 一样省掉大量讨论；只要 false positive 率低、后果不严重，偶尔 lint 错了也无所谓。反例：在百万行代码库里突然打开 linter，「here's my 100,000-line change that I need to make」，不如一开始就对所有人打开
- 讲者说（视频 15:36）：「This is just a few areas, but also a whole area of software engineering discipline that you can apply on the other axis.」

## 11. 结论

- slide 16：「Does code quality even matter? / For disposable software? No / For durable software? Yes / Does it have to be expensive? No」
- 讲者说（视频 15:50）：「not just it depends, but yes or no」：disposable software 不重要，durable software 重要；第三个更有意思的问题，「does it have to be expensive? No.」「Thank you very much.」

## Q&A 精确版

没有 Q&A。视频在 16:15 掌声后结束；直播里讲者 59:54 说完「Thank you very much」，掌声和音乐后主持人在 1:00:16 直接介绍下一位讲者 Aparna Dhinakaran。

## 勘误表（YouTube 字幕 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| Max Canat Alexander（直播主持人介绍） | Max Kanat-Alexander | 日程、banner |
| Capital 1 | Capital One | 日程、banner |
| inward flow tooling（视频）/ inworkflow tooling（直播） | in-workflow tooling | 视频描述 |
| rigger（直播） | rigor | 上下文 |
| llinter（直播） | linter | 上下文 |
| URL parser 函数名（字幕无） | ParseStandardUrl 等 | slide、Chromium 源码 |

## 出入

1. **标题**：日程和直播 banner 是「Does Code Quality Even Matter?」，slide 16 也是「even」；YouTube 视频改名「Does Code Quality Still Matter? It Depends on This」
2. **durable software 的依赖人数**：slide 11 写「Depended on by millions」，口头是「depended on by millions or thousands of people」
3. **视频描述的措辞比讲者强**：描述说对 durable software「it matters more than ever」；讲者在台上只说「yes」，以及 durable software 的绝对数量会增加
4. **「a hundred thousand times more common」**：讲者口头说法，没有给出处或计算，是修辞性的估计
5. **官方简介里的「dark factory」「should we read the code?」**：讲者在台上没有用这两个说法，只泛指社交媒体上的争论

## 【待确认】清单

1. slide 05 Chromium 代码库全景图的出处（哪个可视化工具、哪一年）
2. 「让软件更 disposable」和「降低 rigor 成本」两页 slide 的原文（没有照片，只有口述）
