# AI in Practice: An Inside Look at Apollo's Approach：精确版

> Precise notes for this session, compiled from the official video and livestream captions and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：视频和直播字幕逐段对照完；公司信息对过报道和招聘页。没有照片。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | 没有。这是一场两人对谈，没有放 slide（官方 talk 页的「slide chapters」只有赞助商页和大会 logo）。相邻文件夹 `10-14-1648-process-supervision-for-agents-that-do-l` 的 16 张照片 banner 全是下一场「Process Supervision for Agents That Do Long-Horizon Work · Mitchell Troyanovsky · Basis」，没有这一场的 |
| 视频 | [BOWYujhIQuo](https://www.youtube.com/watch?v=BOWYujhIQuo)，改名「AI Agents Cut 3-Day Portfolio Builds to Minutes — Jack McDonald, Apollo & Michael Manapat, Rowspace」，18:57，上传 2026-10-16。主要文本来源，下文「视频 mm:ss」 |
| 直播 | Day 2 直播 [jiUC6IUMdvc](https://www.youtube.com/watch?v=jiUC6IUMdvc&t=28203s) 7:50:03–8:09:22。和视频是同一场、内容一致，没有被剪掉的段落；直播时间 ≈ 视频时间 + 7:49:52。直播字幕在几处专名上比视频字幕准（「agentic platform」「Rowspace」「tab completion」「Codex or Claude Code」），见勘误表 |
| 官方 talk 页 | [ai.engineer/talks/BOWYujhIQuo](https://ai.engineer/talks/BOWYujhIQuo)：没有列资源链接 |
| 其他资料 | Rowspace 发布报道（[Fortune, 2026-02-25](https://fortune.com/2026/02/25/rowspace-sequoia-ai-financial-platform-private-equity-investment-venture-capital/)）；Redding Ridge 招聘页（[Greenhouse](https://job-boards.greenhouse.io/reddingridge/jobs/4994805008)）；Irradiant 收购公告（[GlobeNewswire, 2025-02-21](https://www.globenewswire.com/news-release/2025/02/21/3030715/0/en/Redding-Ridge-Asset-Management-to-Acquire-Irradiant-Partners.html)） |

## 基本信息

- 时间地点：2026-10-14（周三）4:30pm–4:48pm，Simons Room (Metropolitan East)，Featured Keynote，Main Stage（资料：[日程](https://ai.engineer/nyc/2026/schedule?session=44e9d04a-e8b4-418c-8a85-02fbb9263dd0)）
- 官方简介：「Rowspace customer co-presenting keynote」
- 讲者：
  - Michael Manapat，Co-Founder，Rowspace（主持提问）。资料：Fortune 报道称 Manapat 曾在 Stripe 和 Notion 担任技术要职；Rowspace 2026-02-25 公开亮相，总融资 $50M（Sequoia 领投种子轮，Sequoia 和 Emergence Capital 共同领投 Series A）；平台把机构的结构化和非结构化数据（文档、会计系统、旧演示文稿等）汇到一起，在客户自己的云里处理数据
  - Jack McDonald，Principal at Apollo Global Management，COO of Redding Ridge（回答）
- 视频简介：「Apollo's CLO business used to spend 2–3 days building a single mock portfolio by hand in Excel. Now it takes minutes.」把 Redding Ridge 写作「Apollo's ~$50B structured credit and CLO manager」
- 视频章节：0:00 Intro: Rowspace & Redding Ridge · 1:33 When AI becomes mission-critical · 3:38 Portfolio construction: days → minutes · 5:31 Automating the weekly trade rec · 6:46 Why not just use Claude or ChatGPT? · 9:21 How Apollo became AI-native early · 11:17 How early is finance in AI adoption? · 13:24 Commodity or durable competitive edge? · 15:22 What has to be true next · 18:20 Advice for builders applying AI to finance

## 1. 开场：Rowspace 和 Redding Ridge

- Manapat 说（视频 00:12；直播 7:50:04）：「Rowspace is an agentic platform that uses AI to help financial services partners like Apollo deal with messy, fragmented data. We essentially use agents to do the work of data engineering, to rec information or reconcile data from across a variety of systems so that portfolio managers, traders, and so forth can make decisions faster and more accurately.」（「agentic platform」据直播字幕，视频字幕作「H cloud form」）
- Manapat 介绍 McDonald 是「chief operating officer of Redding Ridge, one of Apollo's credit units」
- McDonald 说（视频 00:53）：「I am a principal at Apollo and COO of Redding Ridge Capital, which is our structured credit and CLO manager that is captive to Apollo」；「fully separate entity, operating company that manages about 50 billion in assets」，主要做 CLO；2020 年加入 Apollo，先后在 financial institutions group、asset-backed group，现在在 Redding Ridge
- 资料：Redding Ridge 招聘页：「assets under management of approximately $43.9 billion as of March 31, 2026」，2016 年由 Apollo 创立并注资，主业是为美国和欧洲的 CLO 及相关 warehouse facility 做 collateral management（[Greenhouse](https://job-boards.greenhouse.io/reddingridge/jobs/4994805008)）。2025 年 2 月收购 Irradiant Partners，加入 $10.7B CLO AUM，合并后约 $38B（[GlobeNewswire](https://www.globenewswire.com/news-release/2025/02/21/3030715/0/en/Redding-Ridge-Asset-Management-to-Acquire-Irradiant-Partners.html)，新闻稿页面 403 没打开，数字来自搜索摘要和转载，【待确认】）
- 资料：官方名称是 Redding Ridge Asset Management（RRAM）；McDonald 口头说「Redding Ridge Capital」

## 2. 从 nice to have 到 mission critical

- Manapat 说（视频 01:34）：和大量金融机构交流后发现，「there's a pretty wide gap between when AI is nice to have and when AI can actually become mission critical to a business」；在 Apollo 的情况里 AI 能真正改变公司运作方式
- McDonald 说（视频 02:06）：Redding Ridge 管很多基金实体和投资，每天从所有投资进来「thousands and thousands of data points」；管着「probably … thousand name loan pools」，跨多个业务的信用敞口每天评估和投资。「CLO management is very active and it's traded in the broadly syndicated loan market」，所以信息要随手可得
- McDonald 说（视频 02:37）：目标是确保数据和数据质量「accurate, consistent, and in a format that we can utilize」。这些信息是私有的，不像公开股票或公开固收那样在市场上广泛流通，「very fragmented in various documents」
- McDonald 说（视频 03:13）：找 Rowspace 合作，让他们「come in and actually sit with our teams」，理解实际的计算、信息，以及「what's actually the golden source of truth」，再不断改进 AI 引擎去理解和复现

## 3. 例一：portfolio construction，从 2–3 天到几分钟

- Manapat 问（视频 03:38）：过去要一个团队干一周甚至更久的「fire drills」，举个具体例子
- McDonald 说（视频 04:07）：CLO「are very test specific」「very rules based」，有非常具体的 waterfall 决定钱怎么流，这些都写在「deep indenture documents and multiple different legal agreements」里
- McDonald 说（视频 04:25）：PM 做 portfolio construction 时要读这些文件、建 Excel 模型、搭 mock portfolio，「anywhere from 200 to 400 single credit names」；用 Rowspace 之前，「anywhere from two to three days in a manual Excel」。现在拿到最新的实时信息，「create a portfolio that would take somebody days to now do it in the matter of minutes」
- McDonald 说（视频 04:59）：PM 先做到「80, 90% completion」，再用自己的判断打磨「that last 10% to make it theirs」
- McDonald 说（视频 05:12）：数据来自 doc libraries、trade and compliance systems、real-time pricing 等

## 4. 例二：每周 trade rec 自动化

- McDonald 说（视频 05:31）：业务模式是有一批银行融资，随时可以在固收市场买贷款（warehouse purchase activity）
- McDonald 说（视频 05:45）：以前每周做一次 trade rec（reconciliation）：本周买了什么、卖了什么；「very manual」，「literally going into bank accounts to understand statements, where things actually settled」，一个团队对完账再给管理层发 recap 邮件
- McDonald 说（视频 06:16）：现在把同一份报告放进 Rowspace 做可视化，「That report was then created and now repeatable in the span of a minute」，全部自动化，团队只做 QA、每天检查；「a much more of a rinse and routine type process」，以前「many hours of the week for two to three individuals on our team」
- 我的理解：例一说的是「两三天变几分钟」，例二说的是「两三个人每周数小时变一分钟内可重复生成 + 每日 QA」

## 5. 为什么不直接用 Claude 或 ChatGPT，信任怎么建立

- Manapat 问（视频 06:48）：你们不是读 chatbot 输出了解情况，而是做交易决策和组合管理决策，对准确性和质量要求很高。两个问题：很多人会问「Why can't you just plug this stuff into Claude and or ChatGPT and see what happens?」；团队怎么建立足够的信任，「make trades, build portfolios」
- McDonald 说（视频 07:26）：Claude、OpenAI、ChatGPT 这些 LLM「very beneficial, but it's all general information … and none of your own actual data」，适合做研究、找新想法、快速了解某个产品或投资
- McDonald 说（视频 07:50）：Redding Ridge 的 AI 建设思路是「owning our own data and having some type of platform and technology that sits on top of that that we know is to be true」
- McDonald 说（视频 08:07）：和合作过的其他科技公司不同，Rowspace 团队「boots on the ground」，「Michael sits with our team on a daily basis」；「for one piece of data that might come from 20 different sources and there's only one that's actually right」。Rowspace 团队和 PM 团队、财务团队一起弄清计算、哪个数据是对的、哪些来源该被覆盖，再汇总
- McDonald 说（视频 08:43）：因此能信任系统：团队验证过，数据来自 trade order management system、performance system、Bloomberg 或其他 pricing index，知道是准确的；而公司里很多其他系统「very fragmented」，要手工搬数据，「whereas Rowspace is now just automating that all for us」
- 资料：Fortune 报道里 Rowspace 联合创始人 Yibo Ling 也说通用 chatbot 做尽调的问题是缺数据，「You need the right information in the right context」

## 6. Apollo 为什么早早「AI-pilled」

- Manapat 说（视频 09:21；直播 7:59:13）：「even early last year before Claude Code became a thing, when Cursor was still in the tab completion era」，Apollo 是他们接触过的机构里「most AI-pilled」的之一，既包括对 AI 世界里怎么投资的看法，也包括运营上：很早就说「we must rebuild the firm in an AI native way, must have a platform not point solutions」。问：这从哪来，Apollo 文化里什么让你们这么早就 AI-pilled
- McDonald 说（视频 10:07）：Apollo 一直以作为「the leader of the alternative asset management space」自居，「very innovative」；管理层一直强调提升团队能力、保持灵活、适应市场方向。「we appreciated that the AI world is here and we need to adapt and adopt it, not … discredit it」；把新技术带进工作流，腾出人的精力去思考，替代手工流程；「a very innovative firm that's very flat and open culture」
- McDonald 说（视频 11:11）：「I'm blanking on the second part of your question. Sorry.」Manapat：「Well, maybe we can keep on going.」

## 7. 金融业在 AI 上还很早

- Manapat 问（视频 11:17）：软件工程作为一门学科已经彻底改变，法律和客服也许快了；金融在「AI is here, it's made a difference」到「we haven't even gotten to the bottom of the first inning」之间处于什么位置？如果还早，要真正改变金融需要什么？（又是两部分问题，「remember the second part」）
- McDonald 说（视频 11:54）：大银行和大机构有大量「middle back office support」，有很多提升空间；Apollo 自己「extremely complicated」。在 asset management、投行、机构金融里，「I still think it's very early」；现在的价值在建模型、快速了解一家公司、汇总研究，但「that's lower value-add at least of where we stand today, and that's going to evolve」；某些职能「probably a need where there's less junior support over time」
- McDonald 说（视频 12:56）：Redding Ridge 目前是把技术用在 routine task 上，尽量高效，「That frees up capacity for … other projects cuz we kind of have like endless things to do」
- 我的理解：第二部分（需要什么）没有单独回答

## 8. 商品还是持久的竞争优势

- Manapat 问（视频 13:17）：一种世界里 AI 只是加速任务、省 OpEx，就像没有哪家公司因为用 Excel 而有优势；另一种世界里 AI「materially makes a competitive advantage or helps reify Apollo's lead in structuring and creative deals」。你怎么看？
- McDonald 说（视频 14:03）：「if it's adapted and utilized properly, it will make you more competitive」；对活跃交易市场里的资管和投资公司，核心是流程和随手可得的信息：知道可用流动性、价格在哪、某一时刻值多少。这在公开交易市场已经存在，「but in the private markets, it's evolving」，Apollo 一直在推动这件事；长期看，掌握更好、团队确信准确的信息的机构会走在前面
- McDonald 说（视频 15:04）：「the last thing we want to do is … make a decision that was misinformed」，错误会一路传到企业和投资人

## 9. 接下来要实现什么

- Manapat 说（视频 15:18）：问 Apollo 五年后会怎样不公平，「I can't even predict if my team's going to use Codex or Claude Code next week」（据直播字幕，视频字幕作「code extra cloud code」）；改问个人层面：希望看到工作、团队、Apollo 的 AI 使用怎么演进，技术要真正有价值需要什么条件
- McDonald 说（视频 15:52）：一是对流向投资人的所有信息有「full confidence」，准确，不用花几小时手工核对；投资人基础广，「over 400 global investors」
- McDonald 说（视频 16:21）：二是更「tech enabled in mindset」：团队不缺这个，但一直是「an afterthought」；routine task 更高效后，可以思考「why does this process exist and how does it work today and how can we actually enhance that」，是否引入新技术；理想是让这成为员工处理问题时的首要思路。「this is specifically at Redding Ridge」

## 10. 给做金融 AI 的人的建议，和收尾

- Manapat 问（视频 17:14）：和你合作学到的是这些工作复杂和有创造性得难以想象，要人和 AI 都花很大力气。对想在金融 AI 技术端或金融端工作的人，有什么要知道的？
- McDonald 说（视频 17:41）：「we are extremely complex organizations」，有些东西看起来「novel or strange and out of place, but it's probably actually there for a reason」；所以「spending the time and really putting boots on the ground to sit with whatever team or front office professional that you're working to build a tool for … is time well spent」
- McDonald 说（视频 18:14）：Rowspace 产品现在「ahead of our roadmap and expectations」，原因是「Michael and the team has spent ample time sitting on the ground with our team like pretty much every single day」
- Manapat 说（视频 18:34）：「shameless plug both Rowspace and Apollo technology are hiring people who want to apply AI to finance」

## Q&A 精确版

没有观众 Q&A。整场是 Manapat 提问、McDonald 回答的对谈，结尾致谢后掌声（视频 18:51；直播 8:08:50），主持人随即介绍下一位讲者 Mitchell Troyanovsky（Basis）。

## 勘误表（YouTube 字幕 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| Roam Space / Row Space / RoseAI space / Rose base / Rosebud / ropace / roerpace | Rowspace | 日程、视频标题 |
| H cloud form（视频） | agentic platform | 直播字幕 |
| direct information / wreck information | rec information（reconcile） | 上下文 |
| Reddington Ridge / Reading Ridge / Writing Ridge | Redding Ridge | 日程、视频简介 |
| CLLO / CLOS's | CLO / CLOs | 上下文 |
| trade wreck / wrecking that out | trade rec / reccing that out（reconciliation） | 上下文 |
| tap configuration era（视频） | tab completion era | 直播字幕 |
| AI pilled | AI-pilled | 上下文 |
| performing management decisions（视频） | portfolio management decisions | 直播字幕 |
| code extra cloud code（视频） | Codex or Claude Code | 直播字幕「codeex or cloud code」 |
| Michael Amanipat（直播） | Michael Manapat | 日程 |
| chat Scotch（直播） | ChatGPTs | 视频字幕 |

## 出入

1. **Redding Ridge 规模**：McDonald 说「about 50 billion」，视频简介「~$50B」；Redding Ridge 招聘页写约 $43.9B（截至 2026-03-31）
2. **公司名**：McDonald 说「Redding Ridge Capital」，公开资料是 Redding Ridge Asset Management
3. **未回答的问题后半部分**：AI-pilled 那题的第二部分 McDonald 说忘了；「改变金融需要什么」也没有单独回答
4. **视频章节 vs 官方 talk 页章节**：两套章节划分不同（YouTube 10 段，talk 页 6 段），内容一致

## 【待确认】清单

1. 「thousand name loan pools」的准确意思（约一千个贷款名字的池子？字幕原话）
2. 「over 400 global investors」指 Redding Ridge 还是更广的范围（上下文是 Redding Ridge 的投资人基础）
3. Rowspace 在 Apollo 内部的部署范围（只听到 Redding Ridge 的两个例子）
