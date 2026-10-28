# Anthropic: Accelerating frontier AI in Financial Services：精确版

> Precise notes for this session, compiled from slide photos, the official session description and the primary sources linked inline. There is no recording of this talk. Written in Chinese with English terms and quotes. Source tags: slide, 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材整理完。**这场没有任何录音或视频**，只有 slide 照片和官方简介，下面所有内容都只到 slide 写了什么为止，讲者口头讲了什么一概不知。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| slide | 作者拍的 17 张（01–17），全部是这场：直播画面的手机翻拍，下方横幅「Keynote · Belinda Neal · Managing Director, Financial Services · Keon Shahab · Applied AI Architect · ANTHROPIC」（01 页还没出横幅，但 slide 本身是讲者介绍）。10 和 11 是同一页。slide 上没有页码，不知道总共多少页、漏拍了哪些 |
| 视频 | 没有单独视频（截至 2026-10-27 未找到） |
| 直播 | **第一天直播 VOD 里没有这场**。[akEqiXFo3GM](https://www.youtube.com/watch?v=akEqiXFo3GM&t=413s) 里：直播 6:53 主持人旁白「Coming up next, we're delighted to welcome two speakers from Anthropic. Please put your hands together for Belinda Neil, managing director of financial services, and Keon Shahab, applied AI architect.」，但接下来 7:24 开始的是 Bridgewater 那场；Bridgewater 在 23:44 结束，23:57 直接是 Two Sigma 那场的介绍 |
| 现场录音 | 没有 |
| 其他一手资料 | Anthropic [Claude for Financial Advisors](https://claude.com/resources/articles/claude-for-financial-advisors)（最初 2026-09-14 发布，后来更新）；Future Proof 现场报道 [InvestmentNews](https://investmentnews.com/practice-management/anthropics-big-deal-claude-for-financial-advisors-feels-the-love-future-proof/268189) |

我的理解：照片里能看到直播画面的观众人数角标（167–219），说明这场当时在直播里播出过，只是事后剪掉了；VOD 里 6:53 那句介绍 Anthropic 的旁白大概是剪辑时留下的。照片顺序上，这场照片的观众数（167–219）介于 Bridgewater（161–181）和 Two Sigma（204–227）之间，和日程顺序一致。【待确认】实际开讲时间。

## 基本信息

- 时间地点：2026-10-13（周二）9:18–9:36am，Simons Room (Metropolitan East)，Main Stage，Featured Keynote（资料：[日程](https://ai.engineer/nyc/2026/schedule?session=68a1e78b-569e-4314-a56f-f20508123cbe)）
- 题目「Anthropic: Accelerating frontier AI in Financial Services.」；直播横幅只写「Keynote」
- 讲者（slide 01「Who you are hearing from」）：
  - Belinda Neal，Managing Director, Financial Services, Anthropic；「Prior: 19+ years at Goldman Sachs across Engineering, Global Markets and Equity Research」
  - Keon Shahab，Applied AI Architect, Anthropic；「Prior: 5 years at Databricks, Lead Solutions Architect for Financial Services」
- 分工（我的理解，依据照片里的讲者小窗）：照片 01–06、14–17 台上是 Neal；照片 07–13（agent foundation 五层）台上是 Shahab
- 官方简介（资料：日程）要点：前沿 AI 进步得比金融服务业采用得快，「that gap is the opportunity」；每家公司要到达两个前沿：「the frontier of intelligence, what the best model can do today, and the frontier of adoption, how much of the firm's real work runs on it」；讲怎样把 connectors、skills 和 controls「built once」，从少数 agent 走到很多 agent，以及整个行业都这么做会是什么样，用 Claude for Financial Advisors 做实例；「You will leave with three questions to take back to your own firm.」

## 1. 三个前沿

- slide（照片 02）「Taking the frontier to the industry」三张卡：

  | # | 小标题 | 标题 | 说明 |
  | --- | --- | --- | --- |
  | 1 | THE FRONTIER OF INTELLIGENCE | Applying intelligence | What the best model can do today. An alpha opportunity. |
  | 2 | THE FRONTIER OF ADOPTION | Scaling agents | Building the agent foundation and the operating model. A scale opportunity. |
  | 3 | APPLIED TO FINANCIAL SERVICES | Compounding impact | The same foundation, built once for all of wealth. An industry opportunity. |

  底部：「This talk: how to take on the frontier opportunity across these three」

## 2. 前沿 1：模型能做什么

- slide（照片 03）「Frontier AI changes what's possible」：三条曲线 Coding（白）、Cyber（蓝）、Finance（橙），纵轴底部「Threshold crossed」、顶部「Autonomous fleets of agents」；横轴 2025 · Nov '25 · Apr '26 · Today。Coding 在 Nov '25 越过阈值，Cyber 在 Apr '26，Finance 当前标注「Early agents」。引语框：「"Our investment professionals live in data and analytical models. Claude for Excel meets them there and gives them a step-change in how fast they can build, test, and trust their work." Atte Lahtiranta, Citadel」。脚注：「Illustrative curves, time-shifted. Thresholds: Claude Opus 4.5 for coding, Claude Mythos Preview for cyber. Quote: Citadel, from Anthropic's financial services keynote, The Briefing.」
- 资料：Claude Mythos Preview 随 Project Glasswing 于 2026-04-07 公布，不公开发布（多家报道，如 [Constellation Research](https://www.constellationr.com/insights/news/anthropic-launches-project-glasswing-claude-mythos-preview-ai-cybersecurity-push)），和曲线上的 Apr '26 对得上
- 资料：2026 年 5 月 Anthropic 金融服务发布的报道里，Citadel 的 Atte Lahtiranta（Head of Core Engineering）有一段关于 Claude 的引语，含「step-change in efficiency」（[CFOtech](https://cfotech.news/story/anthropic-launches-ai-agents-for-financial-services)），措辞和 slide 上的不同。【待确认】slide 上这句的原始出处和原话
- slide（照片 04）「The Claude model family · Working across financial services to bring each model to the right use cases.」从左到右「Faster and lower cost」→「More capable」：

  | 模型 | 定位 | 用例 | 客户引语 | 客户 |
  | --- | --- | --- | --- | --- |
  | Haiku 5.5 | Fastest and lowest cost | Classification and routing; Field extraction, chat | "Ask in Document is one of our big sources of spend, doing about 8M calls a week in production." | AlphaSense |
  | Sonnet 5.5 | For everyday work at volume | Analysis and reporting; Filings at volume | "Claude Sonnet 5.5 scored ahead of Sonnet 5 and used about 121k tokens per answer, where Sonnet 5 used 497k." | Balyasny |
  | Opus 5.5 | The daily driver for complex finance work | Decks and merger models; Credit agreement redlines; Day-to-day agentic coding | "At its lowest effort setting, Claude Opus 5.5 beat Opus 5 at high effort on our BigFinance Bench with about 60% fewer output tokens." | Rogo |
  | Fable 5.1 | For the hardest work | Quant and trading research; The hardest coding problems; Long unattended runs | "A particular piece of code had an extremely rare crash, about one in a million runs, that nobody on our team had explained in four to five years. … Claude Fable 5.1 was the first to find it." | Millennium |

- slide（照片 05）「Smarter, cheaper and safer with every release」三格：

  | 格 | 大数字 | 说明 | 细项 |
  | --- | --- | --- | --- |
  | INTELLIGENCE IS GOING UP | 16+ hrs | of work a model can finish on its own | In 2023: minutes |
  | COST PER TASK IS GOING DOWN | 40% | less to run: Opus 5.5 against Opus 5 | Haiku 5.5: 75% less；Sonnet 5.5: up to 30% less；Fable 5.1: 25% less |
  | AND IT IS GETTING SAFER | 1.0% | of prompt-injection attacks succeed on Fable 5.1 | Fable 5, the release before: 6.5%；Other frontier models: 9% to 53% |

  脚注：「Capability: METR task-length measurements. Cost: Anthropic's launch pages, each against the model it replaced. Safety: Gray Swan indirect prompt injection benchmark, attack success within 15 attempts. Figures to confirm before the talk.」
- 我的理解：脚注最后一句「Figures to confirm before the talk」留在了投影的 slide 上，说明这页的数字在准备时还没最终核对
- 资料：BenchLM 汇总的 Gray Swan IPI（15 次尝试）表里，Claude Fable 5.1 和 Opus 5.5 都是 1.0%（[BenchLM](https://benchlm.ai/benchmarks/gray-swan-ipi)，汇总自厂商自报，非独立测试）。Fable 5 的 6.5% 和「其他前沿模型 9% to 53%」没有核到
- 资料：一篇报道说 Opus 5.5 于 2026-09-22 发布，token 单价比 Opus 5 低 20%（[AI Weekly](https://aiweekly.co/alerts/anthropic-ships-claude-opus-55-at-20-lower-token-prices)）；slide 的 40% 是「cost per task」，口径不同（每任务成本还受 token 用量影响），【待确认】Anthropic 原始发布页上的数字
- 【待确认】METR 测得「16+ hrs」对应的是哪个模型、哪次测量

## 3. 前沿 2：从少数 agent 到很多 agent

- slide（照片 06）「Agents work. Now scale them.」一条上升曲线，四个阶段：

  | 时间 | 阶段 | slide 说明 | 示例 |
  | --- | --- | --- | --- |
  | 2023 · Firms built chatbots | Claude assists | Ask it things. Marvel a bit. Move on. | 问「What's our policy for booking international travel?」答「Travel policies usually cover class of service and approvals.」 |
  | 2024 · Firms ran pilots | Claude collaborates | RAG, a knowledge base, a pilot or three. | 「Found it in the 2024 travel policy (p. 4): international trips need director approval.」（Knowledge base） |
  | 2025 to now · A few agents, built by hand | Claude works | Agents doing real work. | 「Done. I fixed the billing bug across 12 files, added tests, and opened a PR for your review.」（Your files / Your tools）；标「We are here」 |
  | Next · Many agents, one foundation | Claude operates | Fleets of agents run whole processes. | |

  底部：「The model is no longer the constraint. Building every agent by hand is. Next: how to build the agent foundation.」

- slide（照片 07）「The agent foundation: five layers, built once」：

  | 分组 | 层 | 问题 |
  | --- | --- | --- |
  | WHAT LETS YOU TRUST IT | 5 Traces and evals | How do you know it still works? |
  | | 4 Sandbox and approval | Who signs off before it acts? |
  | | 3 Identity and permissions | Who is it acting as? |
  | WHAT IT CAN DO | 2 Skills | Does it know your business and your processes? |
  | | 1 Connectors | Can it reach your systems and data? |
  | （底座） | The model | Can it do the work? Most teams start here. |

  左下「Illustrative example」

### Layer 1 Connectors

- slide（照片 08）「Layer 1 · Connectors · Can it reach your systems and data?」图：上排 AGENTS（Trade breaks · KYC review · Credit memo · Onboarding · Surveillance），中间一条「MCP」，下排 SYSTEMS（Orders · Market data · Payments · Documents · CRM）。对比两个数：灰色「5,000 · 100 agents × 50 systems · One system changes. Every agent breaks.」；橙色「150 · 100 agents + 50 systems · One system changes. One connector to fix.」左下：「Wire each system once. Control it in one place. Any agent can use it.」「Illustrative example」

### Layer 2 Skills

- slide（照片 09）「Layer 2 · Skills · Does it know your business and your processes?」示例文件 `break-investigation/SKILL.md`「How this firm investigates a trade break」：1「Match both sides of the trade.」2「Find the field that disagrees.」3「Propose the fix. Never apply it.」标签：Owned by Operations · Reviewed like code · Version 14。「Any agent that needs the method picks it up.」左下：「Write the method down once. The model becomes a specialist.」

### Layer 3 Identity and permissions

- slide（照片 10、11）「Layer 3 · Identity and permissions · Who is it acting as?」示例 `identity: trade-break-investigator`、`role: ops-analyst`：

  | 操作 | 资源 | 结果 |
  | --- | --- | --- |
  | read | Orders | Allowed |
  | read | Market data | Allowed |
  | read | CRM | Denied |
  | write | Orders | Propose only |

  「Every call is checked against the role, and logged.」左下：「No agent is a super-user. Each one is scoped to its job.」

### Layer 4 Sandbox and approval

- slide（照片 12）「Layer 4 · Sandbox and approval · Who signs off before it acts?」四步：1「Agent proposes」（a booking amendment）2「Staged as a dry run」（nothing real moves）3「A person approves」（the operations lead，高亮）4「Change is applied」（exactly as approved）。锁图标：「The skill asks. A hook enforces. Code that runs before every action. The model cannot skip it.」左下：「Nothing real moves until a named person approves.」

### Layer 5 和小结

- Layer 5 Traces and evals 没有单独拍到的页（只在照片 07、13 出现）【待确认】是否有单独一页
- slide（照片 13）「A strong foundation lets you move fast · The controls a risk committee asks for are the same ones that let a team ship the next agent without starting over.」「THE FIRST THREE QUESTIONS A RISK COMMITTEE ASKS」：Layer 3「Who is it acting as? Identity and permissions」；Layer 4「Who signs off before it acts? Sandbox and approval」；Layer 5「How do you know it still works? Traces and evals」。底部橙条：「Regulated does not mean slow. Every new agent inherits all three.」

## 4. 前沿 3：整个行业用同一套地基——Claude for Financial Advisors

- slide（照片 14）「Claude for Financial Advisors: the orchestrator across the stack」：中心 Claude 标志，周围 CRM · Custodian · Portfolio & reporting · Planning · Alternatives · Meeting capture · Estate & docs · Market data。右侧：「<20% of an advisor's time is spent with clients. The rest goes to 10+ disconnected tools.」三层：「LAYER 1 · CONNECTORS · Partner tools calculate · Deterministic numbers from the systems you trust.」「LAYER 2 · SKILLS · Claude orchestrates · Runs each workflow end to end across the tools.」「LAYER 4 · APPROVAL · The advisor approves · Nothing is saved or sent without sign-off.」底部：「LAUNCHED 14 September 2026 at Future Proof. Metric source: time with clients from Kitces Research.」
- 资料：Anthropic 文章 [Claude for Financial Advisors](https://claude.com/resources/articles/claude-for-financial-advisors) 称最初发布于 2026-09-14；它引用 Kitces research：典型顾问公司只有约六分之一的时间在客户会议上（slide 写「<20%」，口径接近）。文章说 Claude 准备 briefs、summaries 和分析草稿，CRM 更新、客户沟通草稿等行政操作「stages … for the advisor's approval」，「Investment recommendations, client communications, compliance determinations, and other regulated activities remain subject to human review and approval」
- 资料：Future Proof 在 Huntington Beach 举办，多家报道发布日期为 9 月 14 日（[InvestmentNews](https://investmentnews.com/practice-management/anthropics-big-deal-claude-for-financial-advisors-feels-the-love-future-proof/268189)）；Schwab 是首个托管方
- slide（照片 15）「Layer 1 · Connectors: the industry connects once」logo 墙：

  | 类别 | slide 上的 logo |
  | --- | --- |
  | Custody & investments | BlackRock · Charles Schwab · iCapital · Vanguard |
  | Portfolio, planning & reporting | Addepar · Envestnet · Orion · SS&C Black Diamond · wealth.com |
  | Data & research | FactSet · Morningstar · S&P Global · Snowflake |
  | CRM & client meetings | HubSpot · Redtail · Salesforce · Wealthbox · Zocks |
  | Core software | Box · Docusign · Dropbox · Google Workspace · Microsoft · Slack · Zoom |

  （照片模糊，SS&C Black Diamond、Snowflake、Google Workspace 是按 logo 轮廓辨认的）
- 资料：Anthropic 文章的合作方名单含 Addepar、BlackRock、Charles Schwab、Envestnet、FactSet、iCapital、Orion、SS&C Black Diamond、Wealthbox、Wealth.com、Vanguard、Zocks 等，已有 connector 含 Microsoft 365、Salesforce、DocuSign、Box、S&P Global、Morningstar。slide 上的 HubSpot、Redtail、Snowflake、Dropbox、Google Workspace、Slack、Zoom 不在该文章的名单里（可能是通用 connector）；文章里的 CAIS、Franklin Templeton、Invesco、J.P. Morgan、MSCI 等不在 slide 上（资料：文章注明最初 2026-09-14 发布，后来补充了更多 connector）
- slide（照片 16）「Layer 2 · Skills: eight workflows give time back」：Pre-meeting prep · Meeting follow-up · Rebalance review · Alternatives brief · Estate and tax brief · Prospect intake · Advisor onboarding · Compliance review。脚注：「The eight skills that ship with Claude for Financial Advisors. Use them as they come, or adapt them to your firm.」
- 资料：Anthropic 文章列的八个 skill：Advisor onboarding、Alternative investments brief、Compliance and AI policy、Estate and tax brief、Portfolio rebalance review、Post-meeting notes and follow-up、Pre-meeting prep、Prospect intake，和 slide 一一对应（名字略有简写）

## 5. 结尾：带回去的三个问题

- slide（照片 17）「Take the frontier to your firm」：

  | # | 对应前沿 | 标题 | 问题 |
  | --- | --- | --- | --- |
  | 1 | THE FRONTIER OF INTELLIGENCE | One workflow | Which use case are you applying the frontier models to? |
  | 2 | THE FRONTIER OF ADOPTION | One foundation | How are you building the agent foundation to scale from a few to many? |
  | 3 | APPLIED TO FINANCIAL SERVICES | One connector | How are you publishing connectors and skills for your users to build on? |

  底部：「How will you take on the frontier opportunity?」
- 我的理解：这就是官方简介说的「three questions to take back to your own firm」

## Q&A 精确版

无法得知。没有录音，照片里也没有 Q&A 画面。

## 勘误表

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| Belinda Neil（直播主持人旁白字幕） | Belinda Neal | slide、日程 |

## 出入

1. **直播**：VOD 里主持人介绍了 Anthropic 讲者，但演讲本身不在 VOD 里；照片却是直播画面（有观众数角标）
2. **Kitces 数字**：slide「<20%」，Anthropic 文章「a sixth」（约 17%）
3. **Opus 5.5 成本**：slide「40% less to run」（cost per task），一篇报道说 token 单价降 20%，口径不同
4. **Citadel 引语**：slide 上的措辞和 5 月报道里 Lahtiranta 的引语不同
5. **connector 名单**：slide 的 logo 墙和 Anthropic 文章当前名单不完全一致
6. **slide 自带的提醒**：照片 05 的脚注写着「Figures to confirm before the talk」

## 【待确认】清单

1. 讲者口头讲了什么、谁讲哪部分（只能从照片小窗推断）、有没有 Q&A
2. 这场实际几点开讲、为什么不在直播 VOD 里
3. 照片 05 的全部数字（16+ hrs、40%/75%/30%/25%、6.5%、9% to 53%）
4. 照片 04 四条客户引语的原始出处
5. 照片 03 Citadel 引语的原话和出处（slide 标「The Briefing」）
6. Layer 5 Traces and evals 是否有单独一页
7. 照片 15 中模糊 logo 的辨认
