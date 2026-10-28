# We Let Our Financial Planners Write the Agents：精确版

> Precise notes for this session, compiled from the official video captions where available, the author's room recording, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材整理完，现场版和线上视频版逐段对照完。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | 作者拍的 13 张（IMG_0610–0622），全部属于这场，没有别场的照片。IMG_0610 标题页拍于 11:29:40，早于录音开始 |
| 视频 | [gpmze-6ueGU](https://www.youtube.com/watch?v=gpmze-6ueGU)「We Let Our Financial Planners Write the Agents — Matthew Ragone, Range」，8:53，上传日期 2026-10-12，属 Online Talks 播放列表。字幕 `subs/gpmze-6ueGU.txt`。**这是另录的线上版，不是现场这一场**，见下一节 |
| 线上版阅读页 | [swyx 的 AIE NYC 2026 talks 站](https://aie-nyc-2026-talks.swyxio.workers.dev/talks/matt-ragone-financial-planners)：标为 Online track，9 分钟，10/13 线上直播 [akEqiXFo3GM](https://www.youtube.com/watch?v=akEqiXFo3GM&t=12408s) 的 3:26:48–3:35:39 |
| 其他一手资料 | Range 官方博客 [Introducing Rai: The First AI Wealth Advisor](https://www.range.com/blog/introducing-rai-the-first-ai-wealth-advisor)；[Range 顾问名单](https://www.range.com/public/about-us)（prep 里核对过，Matt Ragone 列为 Financial Planner, CFP） |

### 视频和现场是不是同一场？

不是同一次讲。**同一套 slide、同一个结构（五步法、6/24/17 的数字），但视频是照稿念的线上录制版（约 9 分钟），现场是脱稿讲的（约 13 分钟，加了不少例子）。**

- 时间：视频上传于 2026-10-12，线上直播 10/13；现场是 10/14 11:30–11:48，Turing Room（资料：日程 `session.json`）
- 视频的措辞和官方 description 几乎逐句对应（「A planner's constraint was never typing speed」「the people you hand the platform to」），现场没这么说
- 现场有、视频没有的内容：Claude Cowork 是转折点、约 20 位 planner、标准化文档放在 Notion 和 Google Docs、pay stub 上找 group term life 扣款的例子、skill 要问哪一年的税表和哪个州、Rai、内部 agent「Ranger」住在 Slack 里、同事 Joe 当 reviewer、四分卫比喻、「每位 planner 一天给 100 人提供建议」的目标、最后问 Claude「how could you make yourself better?」
- 视频有、现场录音里没有的：「It's a method and it's what actually worked for us」「Think of a skill as a recipe」「a skill that was right in March can be wrong by December」「Same as a plan, you don't grade your own work」「There isn't much field data on this yet」。其中开头部分现场可能讲了但没录到（录音缺前 2 分钟）
- 下面各节以 slide 为骨架，视频为主要措辞，现场独有的内容标「录音」

## 基本信息

- 时间地点：2026-10-14 11:30–11:48am，Turing Room (Riverside Ballroom)，Breakouts，Evaluations Track（资料：日程）
- 讲者：Matt Ragone（slide 写「Matt Ragone, CFP®, CRPC®」，视频标题和 YouTube 用 Matthew Ragone），Financial Planner，Range。slide「I lead AI adoption for the financial planning team at Range, a wealth management firm」
- 讲者说（视频 00:27）：「I'm not an engineer. I'm a certified financial planner」；Range「an all-in-one wealth management company」
- 官方简介的问题：「how to check quality at scale when the output is judgment」。**这个开放问题在两个版本里都没有展开回答**，现场只在结尾提了一句（见第 6 节）
- 资料：Range 的 AI 顾问 Rai 于 2025 年 10 月上线，基于客户组合和 Range 的投资方法训练（[Range 博客](https://www.range.com/blog/introducing-rai-the-first-ai-wealth-advisor)）

## 1. 开场：不是 vendor talk，是被交付平台的那一方

- slide「About me / I'm a financial planner, not an engineer」：「I lead AI adoption — for the financial planning team at Range, a wealth management firm」；「This isn't a vendor talk — It's a method, from the people AI engineers hand their work to」
- 讲者说（视频 00:46）：「Most agent talks you'll hear come from the people who built the platform. This talk comes from someone on the other side, the people you hand the platform to.」；「We work in a regulated business. The output is financial advice to real clients. And the review step is not optional.」
- slide「Where everyone starts / AI as a chat box」：「Tightening up client messages. Useful. But the change that mattered came later.」
- 讲者说（视频 01:12）：把客户消息粘进去让它「a little clearer or friendlier」，「I was still doing all of the work manually. I was just doing it a little bit faster.」

## 2. 瓶颈：planner 同一时间只能在一个地方

- slide：「A planner's constraint was never typing speed. It's that we can only be in one place at a time.」
- slide「What changed / The work happens while I do other work」：1. I'm on a call with one client；2. An agent drafts a full cash flow or insurance analysis for another；3. The call ends: I review, adjust, and send
- 讲者说（视频 02:19）：「The agent doesn't send anything. A planner reviews every single recommendation before a client sees it. What changed is that the first draft no longer needs me sitting in front of it.」
- 讲者说（录音 00:00–00:34）：打字再快「300 words per minute, 200 words per minute」也还是手工；一个 30–45 分钟的客户电话期间，别的客户都做不了
- 讲者说（录音 00:34）：转折点是「late January, early February」的 **Claude Cowork**，「completely changed the game」；agent 在他打电话时起草 cash flow analysis、insurance analysis、**tax review**（视频只说 cash flow 和 insurance）
- 资料：Claude Cowork 以 research preview 发布于 2026-01-12（先给 macOS 上的 Max 用户，随后扩到 Pro、Team、Enterprise）。来源是二手报道汇总（如 [AI Tools Review](https://aitoolsreview.co.uk/insights/what-is-claude-cowork)），Anthropic 一手公告没有核对，【待确认】。讲者说的「late January, early February」可能指 Range 开始用的时间，和发布日期不冲突（我的理解）

## 3. 前提：比好 prompt 多两件事，合起来就是 skill

- slide「Getting there / It takes more than a good prompt」：「Work where the work lives — In our systems, on our clients' real data」；「Know how we do it — Not how financial planning generally works」；「That's what a skill is.」
- 讲者说（视频 02:56）：「The model already knows that, but the agent needs to know how Range does it. What we look at first, what we always flag, how we phrase a specific recommendation.」skill 的定义：「Our firm's way of doing one specific piece of work written down so an agent can follow it.」
- 讲者说（录音 01:46）：会员（Range 把客户叫 members）把所有财务数据连到平台上：保险文件、投资、房产、房贷，「we have their entire picture fully connected」；agent 要知道哪些数据该拉、哪些不该拉
- 讲者说（录音 02:17）：不能给 generic 建议，「we might be a little bit more conservative than what Claude Cowork or any sort of other agent would like to deliver」
- 讲者说（录音 02:28）：「Our team is about 20 planners.」在 Cowork 之前就把保险、投资、退休建议的交付方式**标准化成参考文档**，因为 planner 来自大型财富管理公司或小 RIA，做法各不相同。这些文档在 Notion 和 Google Docs 里；Cowork 出来后只需告诉 Claude「just reference this document as part of your review process before you kick off any of your workflows」，「that actually really saved us so much time」

## 4. 五步法：Pick, Teach, Pack, Test, Ship

- slide「The build guide / How to build a skill」：1 Pick · 2 Teach · 3 Pack · 4 Test · 5 Ship；「None of these steps require writing code.」
- 讲者说（视频 03:27）：「None of them require writing code. Every one of them requires knowing the work really well, which is exactly why the people doing the work should also be the ones building it.」
- 讲者说（录音 03:20）：「five steps to build a skill or agent, they're pretty much interchangeable I feel」（skill 和 agent 两个词他混用）

### 4.1 Pick：选你常做、且一眼能判断好坏的

- slide 无单独页（五步总览之后直接是 Teach，Pick 页没拍到）
- 讲者说（视频 03:40）：别选最炫最复杂的；选「something you do every week where you already know what good looks like, where you can spot a bad draft at a glance」；起步选最不复杂、但手工活最多的。Range 选的是 cash flow analysis 和 insurance review。「If you can't judge the output quickly, you can't test it and you can't trust it. So look for two things. You do it often and you know how to judge it.」
- 讲者说（录音 03:57）：「I'm talking daily, weekly」；复杂的流程「gonna take you a little bit longer to really reiterate」
- 视频字幕 03:49「Don't pick something you do every week」，按上下文应为「Do pick」或断句问题（我的理解：意思是要选每周做的）

### 4.2 Teach：用真实案例，把判断讲出来

- slide「Show it one real case」：「Narrate the judgment out loud: the why, not just the steps.」
- 讲者说（视频 04:28）：「Step two, show it several real cases」，用过去交付过的方案，从头走到尾，「narrate your judgment out loud. Not just next I open the pay stub, why you're opening it, what you're looking for, what would make you stop and ask a question. That narration is the whole game.」
- 讲者说（录音 04:46）：「I would end up now probably doing it two, three, four, five real cases」；除了标准化文档，还把 edge case 和「where I had to correct the agent's work」放进 skill 的参考资料里
- 讲者说（录音 05:17）：例子是上传 pay stub 时告诉 skill 为什么：去看有没有 group term life（团体寿险）的扣款。「You don't have to go crazy with it. But that additional narration has saved us so much time down the road」。转写里扣款金额是「$3」，【待确认】
- 出入：slide 写「one real case」，两个版本讲者口头都说多个（视频「several」，录音「two, three, four, five」）

### 4.3 Pack：写进 skill 的是问题和禁区，不只是步骤

- slide「Pack it into a skill」：「The questions it must ask」；「The limits it must respect」
- 讲者说（视频 05:06）：「A skill isn't just a list of steps.」要问的：开始前要知道什么，「What should it confirm with me instead of simply guessing?」；禁区：「In our world, the big one is that it never sends anything to a client. It drafts and then a planner decides」；「If you only write the steps, you get something that simply just runs on its own. If you write the questions and set the limits, you can get something you can trust.」「Think of a skill as a recipe.」
- 讲者说（录音 06:14）：要问的例子：是 2025 还是 2026 年的税务审查、按哪年税法、哪个州；会员更关注房屋保险还是残疾险、有没有要审的 whole life 保单。部分数据可以直接从会员的 Range 档案里拉
- 讲者说（录音 06:45）：「we're regulated. Our recommendations have real-life impacts. So every single skill is not allowed to send anything to a member」，必须由 CFP 审，「honestly, just moving forward, I feel like that's going to be the case」
- 讲者说（录音 07:16）：Range 有内建的 AI 功能 Rai，他说是「CFP compliant」（后半句转写为「RA-confined」，听不清，【待确认】）；但会员要求和人交流时，仍要 planner 核对
- 资料：Range 称 Rai 在 CFP 练习题上内部测试得 95%，并注明不是 CFP Board 正式考题（据 [Range 博客](https://www.range.com/blog/introducing-rai-the-first-ai-wealth-advisor) 及其转述，二手搜索结果，原文未逐字核对，【待确认】）

### 4.4 Test：先在 5–10 个真实案例上跑

- slide「Run it 5–10 times on real cases」：「Before anyone else sees it.」
- 讲者说（视频 05:51）：「This is the step everyone wants to skip. Every run teaches you something the skill didn't know yet. An edge case, a question it should have asked, a spot where it sounded confident and was wrong.」修了存、再跑；比起把半成品交给「15, 20, 100 people」再收拾，工作量小得多
- 讲者说（录音 07:38）：要有标准 case 也要有 edge case；找一两位同事一起 stress test；每次跑完让它「take what you learned, repackage that into a skill」；发布前最后问 Claude「how could you make yourself better? What would you specifically do?」再更新 skill

### 4.5 Ship：要有 owner 和 reviewer

- slide「Ship it with an owner and a reviewer」：「An owner — who keeps it current」；「A reviewer — who checks it before the team relies on it」
- 讲者说（视频 06:28）：「Our processes change, tax rules change, and a skill that was right in March can be wrong by December.」reviewer「it's someone other than the author. Same as a plan, you don't grade your own work.」「That's the difference between a clever personal shortcut and something a team can build on.」
- 讲者说（录音 08:46）：小公司 owner 和 reviewer 可能是同一个人；他当 owner 时，可能请同事 Joe 当 reviewer，因为对方在保险上更专业

## 5. 现状：6 个月，24 个 skill，17 个在规划

- slide「Where we are」：**6** months in · **24** skills across the company · **17** in financial planning；「It spread to sales, product and marketing without a mandate.」
- 讲者说（视频 07:12）：「Nobody sent a memo. People saw what the planners were doing and started building their own agents.」「Every one of those skills was written and is maintained by planners, marketers, and salespeople, not developers or engineers.」
- 讲者说（视频 07:34）：专家给自己的领域建 skill 后，skill 更具体（保险专家写保险 skill，税务专家写税务 skill）；review 自然发生：「We already review each other's plans … So now we review each other's skills in that same way.」
- 讲者说（录音 09:17）：「six to eight months」；扩散到 planners、marketers、team success、member success
- 讲者说（录音 09:50）：下一步是内部 agent **Ranger**（「we're Range, this is Ranger」），住在 Slack，能上 dashboard 帮做会员相关的任务，还在建设中
- 讲者说（录音 10:27）：比喻：CFP 像四分卫，有时自己长传，有时把球交给 CPA、投资团队；税务 skill 由 CPA / 税务团队提供内容，他负责「how to build the actual skill」
- 出入：slide 和视频是「6 months」，现场说「six to eight months」

## 6. 为什么是现在：瓶颈移动了

- slide「Why this matters now / The bottleneck has moved」：Settled：「Can a model draft a cash flow analysis?」Still open：「Who writes down how your firm does it?」「Who tests it before it ships?」「Who owns it once fifteen other people depend on it?」
- 讲者说（视频 08:28）：「If you're being asked to push agents out past the engineering org, you're about to hit these exact questions. There isn't much field data on this yet, but this is how we've done it here at Range.」
- 讲者说（录音 11:38）：新问题是 skill 发布后谁来测、谁来更新，「how do we know that every skill is fully up to date」；预测 planner 会变成「AI agent managers」；人仍然在，「the human touch is still there」
- 讲者说（录音 12:08）：目标是「instead of providing advice for three to four people a day … to 100 people a day per planner」；行业里采用和不采用的公司正在分化
- 讲者说（录音 12:41）：结尾「this is coming from someone with no engineering background, no coding background」；Range 在招人
- 联系方式：视频 08:46 字幕为「you can reach me for at mattrange.com」，按读音可能是邮箱 matt@range.com，【待确认】

## Q&A 精确版

没有 Q&A。现场讲者说「We do have like two minutes if there's any questions, but if not, I'll be outside」（录音 12:41），之后录音只有一句「Thank you」（录音 13:20），到 13:40 结束，没有录到提问。线上视频版没有 Q&A。

## 勘误表（Whisper / YouTube 字幕 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| Matt Rigon（视频字幕） | Matt Ragone | slide、日程 |
| Claude Coler | Claude Cowork | 上下文（前后都是 Cowork） |
| flawed skill | Claude skill | 上下文 |
| a good client（录音 01:16） | a good prompt | slide「It takes more than a good prompt」、视频 |
| pick to teach path test and ship | Pick, Teach, Pack, Test, Ship | slide |
| education cases | edge cases | 上下文 |
| not uploading a pay stub | now / when uploading a pay stub | 上下文（我的理解） |
| pastub（视频字幕） | pay stub | 上下文 |
| a lip a list of steps（视频字幕） | a list of steps | 上下文 |
| a certain scale of agents | certain skills / agents | 上下文（我的理解） |
| 20 plans | 20 planners | 录音 02:28 同一数字 |
| an agency ranger | an agent, Ranger | 上下文「we're Range, this is Ranger」 |
| Rai … RA-confined | Rai（Range 的 AI 顾问）；后半句【待确认】 | Range 博客 |
| members … is why you keep your needs or change | 听不清，【待确认】 | — |
| mattrange.com（视频字幕） | 可能是 matt@range.com，【待确认】 | 读音 |

## 出入

1. **一个案例还是多个**：slide 标题「Show it one real case」；视频说「several real cases」，现场说现在会用「two, three, four, five」个
2. **多久**：slide 和视频「6 months in」；现场「six to eight months」
3. **Cowork 的时间**：现场说「late January, early February」；Cowork 发布于 2026-01-12（二手来源）。可能是 Range 开始用的时间，不算矛盾
4. **agent 起草的内容**：slide 和视频只说 cash flow 或 insurance analysis；现场多了 tax review
5. **官方简介的开放问题**（判断类输出如何规模化检查质量）在两个版本里都只提了问题，没有给方法

## 【待确认】清单

1. pay stub 例子里的扣款金额（转写「$3」）
2. Rai 那句的后半「RA-confined」原话
3. 录音 01:46「members … is why you keep your needs or change」原话
4. 联系方式「mattrange.com」是否是 matt@range.com
5. Claude Cowork 发布日期（只核对了二手报道）
6. Rai 95% CFP 练习题的说法（只看到搜索摘要）
7. 录音开头缺失的约 2 分钟里，现场是否讲了「not a vendor talk」「AI as a chat box」（slide 拍到了，录音没有）
