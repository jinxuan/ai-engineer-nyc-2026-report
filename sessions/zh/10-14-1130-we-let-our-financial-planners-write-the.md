[English](../10-14-1130-we-let-our-financial-planners-write-the.md) | **中文**

# 在 Range，agent skill 由理财规划师来写：五步，不写代码

*在 AI Engineer NYC 2026 上，Range 的理财规划师 Matt Ragone 讲了没有工程背景的规划师们怎样搭起、并一直维护自己每天在用的 agent skill，以及为什么悬而未决的问题在于归属和测试，而不在模型能力。*

**Breakout · 2026 年 10 月 14 日（周三）· 11:30–11:48am · Turing Room (Riverside Ballroom)**
**讲者：** Matt Ragone，CFP®，CRPC®，Financial Planner，Range

![标题页：We Let Our Financial Planners Write the Agents](../../slides/10-14-1130-we-let-our-financial-planners-write-the/01.jpg)
*标题页。*

---

大会上的多数 talk 来自搭 agent 平台的人。Matt Ragone 把自己定位成另一边。「我不是工程师。我是一名 certified financial planner，」Ragone 在为大会录制的线上版 talk 里说。Ragone 在财富管理公司 Range 负责理财规划团队的 AI 落地，这场 talk 是一份搭建指南：规划师们是怎么开始自己写每天在用的 agent skill 的。

Ragone 讲了两遍。线上版约九分钟，基本照稿念，在大会前发布。10 月 14 日的现场版用的是同样的幻灯片和结构，但讲了约 13 分钟，是即兴讲的，有线上版里没有的例子。本文以现场版为准。作者的现场录音从开讲约两分钟后才开始，所以开头几张幻灯片这里用线上版的措辞来描述，并做了标注。

## 1. 不是一场厂商 talk

![About me: I'm a financial planner, not an engineer](../../slides/10-14-1130-we-let-our-financial-planners-write-the/02.jpg)
*"This isn't a vendor talk"：来自 AI 工程师们把工作交过去的那些人的一套方法。*

"About me" 这张幻灯片讲了两点：Ragone 负责「Range 这家财富管理公司的理财规划团队」的 AI 落地，以及 "This isn't a vendor talk. It's a method, from the people AI engineers hand their work to."（这不是一场厂商 talk。这是一套方法，来自 AI 工程师们把工作交过去的那些人。）在线上版里，Ragone 是这样说的：「你听到的大多数 agent talk 来自搭平台的人。这场 talk 来自另一边的人，就是你们把平台交过去的那些人。我们在一个受监管的行业工作。产出是给真实客户的理财建议。而审核这一步不是可选的。」

![Where everyone starts: AI as a chat box](../../slides/10-14-1130-we-let-our-financial-planners-write-the/03.jpg)
*起点：润色给客户的消息。*

Range 的起点和大多数公司一样，是「AI as a chat box」，用来「收紧给客户的消息。有用。但真正重要的变化是后来才来的」。在线上版里，Ragone 描述了把一条客户消息贴进去，让它变得「更清楚一点或更友好一点」：「所有活我还是在手动做。只是做得快了一点。」

## 2. 瓶颈从来不是打字

![A planner's constraint was never typing speed](../../slides/10-14-1130-we-let-our-financial-planners-write-the/04.jpg)
*"It's that we can only be in one place at a time."*

幻灯片写着："A planner's constraint was never typing speed. It's that we can only be in one place at a time."（规划师的瓶颈从来不是打字速度，而是我们一次只能在一个地方。）在现场，Ragone 说不管规划师打字是「每分钟 300 字还是 200 字」，活都还是手动的，和一位客户通话 30 或 45 分钟，这段时间别的客户就得不到。

![What changed: the work happens while I do other work](../../slides/10-14-1130-we-let-our-financial-planners-write-the/05.jpg)
*三步：和一位客户通话，一个 agent 为另一位客户起草，然后审核。*

幻灯片列出了现在一天的样子：（1）「我在和一位客户通话」；（2）「一个 agent 为另一位客户起草一份完整的现金流或保险分析」；（3）「通话结束：我审核、调整、发送。」

在现场，Ragone 说出了转折点：「一月底、二月初」，那时「Claude Cowork 彻底改变了局面」。通话期间，Ragone 可以让一个 Cowork agent「把一份现金流分析、一份保险分析、一份税务审查完整地起草出来」。（幻灯片上只提到了现金流和保险。）通话结束后，规划师得「审核、调整，真正看清我们的 agent 做了什么，确保它在发出去之前无懈可击」。媒体报道称 Claude Cowork 的 research preview 在 2026 年 1 月中旬发布；Ragone 给的日期可能是 Range 开始用它的时候。

## 3. 光有好 prompt 不够

![Getting there: It takes more than a good prompt](../../slides/10-14-1130-we-let-our-financial-planners-write-the/06.jpg)
*两个条件：在工作所在的地方工作，知道公司是怎么做的。"That's what a skill is."*

幻灯片说，要做到这一步，有两件事必须成立。第一，"Work where the work lives: in our systems, on our clients' real data."（在工作所在的地方工作：在我们的系统里，在客户的真实数据上。）第二，"Know how we do it: not how financial planning generally works."（知道我们是怎么做的：而不是理财规划一般是怎么做的。）两者合起来："That's what a skill is."（这就是 skill。）

关于第一点，Ragone 在现场解释说，Range 的客户——Range 称之为 member——把自己所有的财务数据都接到平台上：保险文件、投资、房产、房贷。「我们手上有他们完整连通的全貌。」agent 得知道该拉哪些数据、不该拉哪些。关于第二点，agent「不能只给出通用的建议，因为我们可能比 Claude Cowork 或者别的什么 agent 想给的更保守一点」。

Ragone 说，Range「幸运地走在了前面」。「我们团队大约 20 名规划师。」在 Cowork 出现之前，团队就已经把保险、投资和退休建议的交付方式标准化了，因为规划师们背景各异，从大型财富管理公司到小型 RIA 都有。这些参考文档放在 Notion 页面和 Google Docs 里。Cowork 来了之后，团队没有重新造轮子，指令很简单：「Claude，在启动任何工作流之前，把这份文档作为你审核流程的一部分来参考。」这「真的帮我们省了大量时间」。

## 4. 搭一个 skill 的五步

![The build guide: How to build a skill](../../slides/10-14-1130-we-let-our-financial-planners-write-the/07.jpg)
*Pick、Teach、Pack、Test、Ship。"None of these steps require writing code."*

搭建指南有五步：Pick、Teach、Pack、Test、Ship。"None of these steps require writing code,"（这些步骤都不需要写代码。）幻灯片写道。Ragone 在现场补充说，每一步都「需要非常了解这份工作。所以做这份工作的人应该是搭它的人」。Ragone 对「skill」和「agent」用得比较随意：这些步骤是用来搭「一个 skill 或 agent，我觉得它们基本可以互换」。

### 第 1 步：Pick

这一步没有幻灯片照片。「选一个你经常重复的工作流。我说的是每天、每周，」Ragone 说，「你的第一反应会是选最炫的、最复杂的那个。我凭经验告诉你，别这么做。」对的选择是那种很费时间、但能简单自动化的活，「你马上就能知道输出是好是坏」。复杂的工作流要迭代更久。Range 从现金流分析和保险审查开始。线上版把检验标准分成两条："You do it often and you know how to judge it."（你经常做，而且你知道怎么评判它。）

### 第 2 步：Teach

![Step 2: Show it one real case](../../slides/10-14-1130-we-let-our-financial-planners-write-the/08.jpg)
*"Narrate the judgment out loud: the why, not just the steps."*

幻灯片写着 "Show it one real case"（给它看一个真实案例）和 "Narrate the judgment out loud: the why, not just the steps."（把判断大声讲出来：讲为什么，而不只是步骤。）在台上，Ragone 修正了数字：「现在我大概会给它两个、三个、四个、五个真实案例。」（线上版也说的是「几个」。）除了那些标准化文档，Ragone 还把真实案例加进 skill 的参考资料里，尤其是 edge case 和「我不得不纠正 agent 工作的」案例，这样下一次运行能处理得更好。

讲解最重要。Ragone 的例子是：为了做现金流或保险分析上传客户的工资单时，告诉 skill 为什么，比如是为了看有没有团体定期寿险的扣款。「不用搞得太夸张。但这一点额外的讲解后来帮我们省了大量时间。」线上版把这种讲解称为 "the whole game"（全部的关键）。

### 第 3 步：Pack

![Step 3: Pack it into a skill](../../slides/10-14-1130-we-let-our-financial-planners-write-the/09.jpg)
*步骤之外的两样东西：它必须问的问题，它必须遵守的限制。*

skill 做得不错之后，就把它打包。幻灯片点出了步骤之外的两样东西："The questions it must ask"（它必须问的问题）和 "The limits it must respect"（它必须遵守的限制）。

关于问题，Ragone 在现场举了例子：这是 2025 年还是 2026 年的税务审查，适用哪部税法，哪个州？有些答案可以从 member 的 Range 档案里拉出来，但有些不行：这位 member 更关心房屋保险还是残疾险，有没有一份终身寿险保单要审查？

关于限制：「我们受监管。我们的建议会对真实生活产生影响。所以每一个 skill 都不允许给 member 发任何东西。」所有东西都由一位 CFP 审核，「老实说，往后我觉得也会一直是这样」。Ragone 提到 Range 有一个内置 AI 功能叫 Rai，被描述为「CFP compliant」，但当 member 要求和人沟通时，仍然由规划师来确认答案。线上版这样总结这一步：「如果你只写步骤，你得到的是一个自己跑起来的东西。如果你写下问题、设好限制，你就能得到一个可以信任的东西。」

### 第 4 步：Test

![Step 4: Run it 5–10 times on real cases](../../slides/10-14-1130-we-let-our-financial-planners-write-the/10.jpg)
*"Before anyone else sees it."*

幻灯片说，在 skill 交给团队之前，先在真实案例上跑五到十次。在现场，Ragone 说这组案例应该包括标准案例和一些 edge case，可以请一两个同事帮忙做压力测试。每次运行之后，Ragone 都会让 agent「把你学到的东西重新打包进一个 skill」。上线之前，Ragone 还会问最后一个问题：「你怎样能让自己更好？具体你会做什么？」然后再更新一次 skill。线上版把测试称为「大家都想跳过的那一步」，并说跑五到十次的工作量，远小于把一个半生不熟的 skill 交给「15、20、100 个人」之后再去收拾残局。

### 第 5 步：Ship

![Step 5: Ship it with an owner and a reviewer](../../slides/10-14-1130-we-let-our-financial-planners-write-the/11.jpg)
*一个负责让它保持最新的 owner，一个在团队依赖它之前检查它的 reviewer。*

上线「不是工作的终点。我倒希望有那么简单，」Ragone 说。税法会变，一个上线的 skill 需要一个让它保持最新的 owner，以及一个在团队依赖它之前检查它的 reviewer。在小公司，这两个角色可能是同一个人。在 Range，如果 Ragone 是某个 skill 的 owner，可能由一位叫 Joe 的同事来审，「因为 Joe 在保险方面的专业知识可能更好一点」。线上版给出了要另设 reviewer 的理由："Same as a plan, you don't grade your own work."（和理财方案一样，你不给自己的作业打分。）

## 5. Range 现在走到了哪里

![Where we are: 6 months, 24 skills, 17 in financial planning](../../slides/10-14-1130-we-let-our-financial-planners-write-the/12.jpg)
*"It spread to sales, product and marketing without a mandate."*

| 指标 | 数值 |
| --- | --- |
| 开始至今 | 6 个月 |
| 全公司的 skill | 24 |
| 理财规划的 skill | 17 |

（幻灯片上写的是六个月；Ragone 在现场说的是「六到八个月」。）幻灯片还写着它「没有任何强制要求，就扩散到了销售、产品和市场团队」。在现场，Ragone 说规划师、市场、team success 和 member success 团队都在彼此的成果上继续搭。下一步是一个叫 Ranger 的内部 agent（「我们是 Range，这个是 Ranger」），它住在 Slack 里，能在 Range 的 dashboard 里干活、帮忙处理 member 的事务，目前还在搭建中。

Ragone 还主张专家应该搭自己领域的 skill：保险专家能发现通才规划师可能注意不到的东西。Ragone 的比喻是四分卫：certified financial planner 有时「一把长传 50 码」，有时把球交给 CPA 或投资团队。对税务 skill，Range 的 CPA 和税务团队提供内容，Ragone 帮忙解决「怎么搭出真正的 skill」。

## 6. 瓶颈已经转移了

![Why this matters now: The bottleneck has moved](../../slides/10-14-1130-we-let-our-financial-planners-write-the/13.jpg)
*哪些已经解决，哪些还悬而未决。*

最后一张幻灯片把问题分成两类。已经解决的："Can a model draft a cash flow analysis?"（模型能不能起草一份现金流分析？）仍然悬而未决的："Who writes down how your firm does it? Who tests it before it ships? Who owns it once fifteen other people depend on it?"（谁把你们公司的做法写下来？谁在上线前测试它？等十五个人都依赖它之后，谁对它负责？）

瓶颈「过去是我能打多快」，Ragone 说。现在的问题是谁来确保每一个上线的 skill 都经过测试、都在更新：「我们怎么知道每个 skill 都是完全最新的？」Ragone 预计规划师会变成「几乎是 AI agent 的管理者」，同时「人的温度还在」。目标是：「不再是每天给三四个人提供建议……而是每位规划师每天给 100 个人提供建议。」Ragone 说，各家公司正在分化成采用这套做法的和没采用的，最后指出「这些话出自一个没有工程背景、没有编程背景的人」。Range 正在招人。

session 介绍曾提出一个问题：「当产出是判断时，怎样规模化地检查质量」。两个版本的 talk 都没有超出 owner 加 reviewer 这一步，以及最后一张幻灯片上那几个悬而未决的问题。

## 只有现场版才有的内容

和线上录制版相比，现场版多了这些：Claude Cowork 作为转折点、约 20 名规划师的团队规模、放在 Notion 和 Google Docs 里的标准化参考文档、工资单的例子、关于税务年度和州的问题、Rai、同事当 reviewer 的例子、内部 agent Ranger、四分卫的比喻、每位规划师每天服务 100 位客户的目标，以及上线前最后那个「你怎样能让自己更好？」的问题。反过来，线上版也有一些现场版没有的、照稿念的句子，比如「一个三月份还对的 skill，到十二月可能就错了」。

---

**链接：** [日程](https://ai.engineer/nyc/2026/schedule?session=9b002687-09cf-42e2-9282-532ed9d0c184) · [talk 的线上版](https://www.youtube.com/watch?v=gpmze-6ueGU)（YouTube，8:53，单独录制）· [Introducing Rai: The First AI Wealth Advisor](https://www.range.com/blog/introducing-rai-the-first-ai-wealth-advisor)（Range）

本文依据作者对现场版的录音（从开讲约两分钟后开始）、13 张幻灯片照片，以及单独录制的线上版字幕写成；线上版字幕用于开头部分，引用处已标注。Ragone 在最后两分钟表示可以回答问题，但录音结束时没有人提问。[精确笔记](../precise/10-14-1130-we-let-our-financial-planners-write-the.md)（中文）逐节对比了两个版本。
