# Your Financial Agent Passed Every Eval. Can You Defend Its Decision?：精确版

> Precise notes for this session, compiled from the official video captions where available, the author's room recording, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材核对完，slide 19 张逐张读过，录音中段的乱码已重新转写。没有 Q&A 可整理（见素材盘点）。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| 录音 / transcript | 两路。手机录音 `audio/import-3.ogg`（10:51，从 Peregrine 那张 slide 开始，到讲者致谢结束），质量好；Mac 内置麦 take-1/take-2 很差。`transcript.md` 把两路拼在一起，**[04:54]–[06:42] 一段是 Mac 那路的幻听乱码**（"the state of New York City" 反复出现、"We got it." 等），这一段我用同样的 Whisper 模型（`large-v3-turbo-q5_0`，`-mc 0`，同样的音量处理）对手机录音 270–420s 重新转写，文中 [04:30]–[06:55] 的引文以重转版为准。时间戳按 `transcript.md` 的时间轴（与手机录音的秒数基本一致） |
| Q&A | **没有可用录音**。`transcript.md` 里 [12:10]–[13:10] 标成「Q&A」的三段来自 Mac 麦，内容不可辨；我把 Mac take-2 末尾（500s 以后）重转了一遍，只得到一句不成句的话和 "Thank you."。手机录音在讲者致谢后结束。场次 11:30–11:48，只有 18 分钟，可能本就没有或只有一两个问题，【待确认】 |
| slide | 作者拍的 19 张（IMG_0388–0406），全部属于这场（标题、页眉 "Let's build a financial agent" / "Pattern for assurance" 一致，没有混入上一场的照片）。前 3 张（IMG_0388–0390）拍摄时间早于手机录音开头，**没有对应的录音** |
| 视频 | 没有 |
| 讲者资料 | 没找到讲者本人关于这场的文章或 slide（prep 阶段搜过）。公司博客 [Auditing an audit agent](https://www.fieldguide.com/blog/auditing-an-audit-agent)（Kunal Sinha，Fieldguide ML Engineer，2026-09-17）主题相近，但不是讲者写的，也没有覆盖四项控制 |

## 基本信息

- 时间地点：2026-10-13 11:30–11:48am，Turing Room (Riverside Ballroom)，Breakouts，Evaluations Track（session.json）
- 讲者：Anirudh Veeraragavan，Lead Security Engineer，Fieldguide（slide IMG_0406）。session.json 的 bio：Fieldguide 的 founding security hire；之前在 Plaid 五年，最后是 Staff Software Security Engineer，做过 agentic penetration-testing 系统
- 官方简介：「Defensibility requires four distinct controls: authorization, execution control, evidence provenance, and evaluation integrity」；用「a synthetic audit decision trace」走一遍；结尾「"the model said so" is not a defensible answer」
- 结构：前置（典型做法和「全绿」）→ 案例设定 → 四项控制，每项一个出错的对账条目 → 何时停下问人 → 四项缺一不可 → 推广到其他金融场景。slide 顶部有五个页签：Authorization / Execution Control / Evidence Provenance / Evaluation Integrity / Human Decides

## 0. 前置：一个「全绿」的金融 agent（无录音）

- slide「Let's build a financial agent · A typical build — Four-Step Process」：
  - INPUTS：Client's books / Bank reconciliation / Bank evidence
  - AGENT：Does the matching / Methodology in prompt / Prior-year audits as examples
  - EVAL：Previously reviewed audits
  - HUMAN：Reviewer signs off
- slide「Ship to GA — Everyone Wins」：Evals 97% ✓ / Metrics Green ✓ / Users Happy ✓；下一张加了一行「Selected for inspection: **why was that sufficient?**」
- 这三张没有录音，讲者口述内容【待确认】

## 1. 开场：Peregrine 和「新来的审计师能跟上」

- slide「Fraud is real · Peregrine Financial Group · July 2012」：「On paper: $223.8M / At the bank: ~$5M」，「Forged bank statements, for nearly 20 years.」
- 讲者说 [00:00]：「They claimed they had over $200 million, and in reality, at the bank, there's only $5 million.」他明确说「I'm not here to litigate who or what or why this happened」，要说明的是「in the financial industry, the consequences of being wrong are quite severe」
- 资料：CFTC 2012-07 的起诉公告（[Release 6300-12](https://www.cftc.gov/PressRoom/PressReleases/6300-12)）：NFA 2012 年 7 月审计时，PFG 声称持有超过 $220 million 客户资金，实际约 $5.1 million。[Wikipedia](https://en.wikipedia.org/wiki/Peregrine_Financial_Group)：创始人 Russell Wasendorf 用伪造的银行对账单，约 20 年间挪用最多约 $215 million；公司 2012 年 7 月关闭。slide 上的 **$223.8M** 这个精确数字我没在一手资料里找到原文，见出入
- 讲者说 [00:29]–[00:54]：借审计行业的一个定义：「an experienced auditor who hasn't done the work before should be able to understand the test you performed, the evidence you collected, the decision that was reached. In other words, an experienced auditor new to the work can follow it.」搬到 agent 上：「an experienced auditor new to the work can follow the agent」
- 资料：这条定义出自 PCAOB 审计准则 [AS 1215 Audit Documentation](https://pcaobus.org/oversight/standards/auditing-standards/details/AS1215) 第 .06 段：documentation 必须「contain sufficient information to enable an experienced auditor, having no previous connection with the engagement」理解所做的工作和结论（讲者没有说出处，出处是我核对的；PCAOB 页面对抓取返回 403，条文据搜索结果摘录，【待确认】逐字核对）
- 讲者说 [00:55]–[01:14]：要「follow the agent」，需要四项控制：authorization、execution control、evidence provenance、evaluation integrity；「all of this rests upon knowing when the agent should stop and the human should decide instead」。这是「a pattern for decision assurance」，「it really applies to many more domains than just this one」

## 2. 案例设定：Calder Freight Lines 年末银行对账

- slide「Pattern for assurance · Calder Freight Lines (fictional) — Bank Reconciliation · 12/31/2026」：

| 项目 | 金额 |
| --- | --- |
| Balance per bank | 2,914,600 |
| + Deposits in transit | 580,150 |
| − Outstanding checks | (731,420) |
| **Adjusted bank balance** | **2,763,330** |
| Balance per client | 2,763,330 ✓ |

- slide「Four of 41 reconciling items」：

| 编号 | 类型 | 对方 | 金额 |
| --- | --- | --- | --- |
| OC-14 | check | Brightline Fuel | 62,500 |
| OC-22 | check | Rook Logistics | 96,400 |
| DIT-02 | 12/31 deposit | Norvale Foods | 61,900 |
| DIT-03 | 12/31 deposit | Lakeshore Retail | 480,000 |

- 讲者说 [01:22]–[01:50]：虚构客户，「closing the books for 2026」；银行余额约 2.9、账上约 2.7，差额「explained by 41 reconciling items」；挑其中四条「two checks, two deposits」看 agent 怎么做
- 我的理解：OC = outstanding check，DIT = deposit in transit（slide 左表的两类调节项）。2,914,600 + 580,150 − 731,420 = 2,763,330，算术成立

## 3. Authorization：同一个调用，不同的状态（OC-14）

- slide「Same Call, Different State」：
  - ✓ Draft unreviewed rows, as itself
  - ✗ Write to a reviewed row: **propose** instead
  - ✗ Replace a human's flag
- 讲者说 [01:45]–[02:59]：人工 reviewer 把一张支票标成可能有问题；几天后来了补充信息，触发 agent 重跑，agent 把它改写成 no exception。问题在于人已经 review 并签字了，「what they reviewed, the content behind it has now changed」。agent 本身没做错：「It had the permission, it had the evidence, and it arrived at the right conclusion」，但不应该覆盖
- 讲者说 [02:17]：「authorization isn't just about is this tool call allowed. It's is this tool call allowed for this object given the state at this point in time.」agent 可以以自己的身份起草未审核的行，「but it cannot overwrite what a human reviewed」，只能作为「its own suggestion, as opposed to its own authoritative truth」
- slide（IMG_0404）对应的失败：「OC-14: signed by a reviewer who saw a different answer」

## 4. Execution control：记录，而不只是日志（OC-22）

- slide「"We Had Logs."」：「OC-22 · 2027 run: $96,400 no exception · 2028 rerun: $69,400 exception」
  - WHAT WE HAD: A LOG — `fetch_evidence("January statement")` / Final text / A "reasoning" paragraph
  - WHAT WE NEEDED: A RECORD — Claim / Evidence @hash + page + values read / Rules applied / Run · model version · config / Human decision
- 讲者说 [02:59]–[03:30]：2027 年跑出 no exception，一两年后被抽中复查，重跑得到完全不同的结果。有日志（取了证据、做了推理、给了答案），「but that's not enough to reconstruct what happened」
- slide「A Run Is a Chain of Recorded Steps」：FETCH（code，STMT-JAN-4471 p.3 · …9c1e）→ EXTRACT（model，#20977 · 01/08 · 96,400.00）→ MATCH（code，96,400 = 96,400 ✓）→ CONCLUDE（model，"Cleared 01/08. No exception."）；「step ID = run · item · step → inputs + output, saved」
- 讲者说 [03:30]–[04:20]：每一步有 ID，记下输入和输出；区分确定性步骤（用代码取数、匹配）和模型步骤（从 PDF 抽字段、得出结论）；有了这条链，就能单独重跑某一步，而不是整条重跑，并能定位新旧两次运行在哪一步分叉
- slide「Rerun One Step, Not the Run」（代码）：

```
orig = steps.get(run="R-0031", item="OC-22", step="extract")
new  = extract(orig.input.page_sha, model=CURRENT)   # same pinned input
steps.put(new, parent=orig.id)                       # new record, linked
```

  结论框：「Today's model: **69,400** · Pinned page: **96,400** → the original stands」
- 讲者说（重转版 [04:30]）：「our original run was actually correct, even though the run of this newer run is wrong. But again, it's not so much about whether you're correct or not. It's about being able to demonstrate with a high level of conviction that you know you're right. You didn't just get lucky that the model picked the right answer.」
- 我的理解：69,400 和 96,400 是两位数字对调，slide 用它演示新模型在 extract 这一步读错
- slide（IMG_0404）：「OC-22: right answer, but you can't show what ran」

## 5. Evidence provenance：引用是身份，不是字符串（DIT-02）

- slide「Right Bank, Right Month, Wrong Account」：
  - DIT-02 · What the agent cited：「"January bank statement, p.2"」/ **$61,900** · Norvale Foods · 01/05 / Account **-4488 (payroll)**
  - The account under test：Account **-4471 (operating)** / January: **no $61,900 credit**
- 讲者说（重转版 [04:57]–[05:30]）：模型结论错了，「It said that this was valid, but it actually pointed to the wrong account」。引用「January bank statement, page 2」没用：大项目里同名文档很多，「Even if this is a unique document name, what version of the model look at, who uploaded this, where did this even come from?」，「this isn't even related to hallucination. It can be a real document」
- slide「A Citation Is an Identity, Not a String」（代码）：

```
src = run.retrieval_record[claim.source_ref]   # id + version + hash, not model text
assert src.account == item.account             # check what the claim depends on
assert required_rules(item) <= claim.rules     # and every rule it had to apply
```

- 讲者说（重转版 [05:35]–[06:07]）：「a citation is identity. It's not just a string. You have a very specific ID, a very specific version of the document, a hash of the contents behind it」；能确切绑定出处后，才能对证据加规则或质量约束，例：「we may only want evidence that's sourced directly from the bank as opposed to something that the client uploaded where they may be able to tamper with it」
- slide（IMG_0404）：「DIT-02: cites the wrong account」

## 6. Evaluation integrity：审计 gold，不只审计 agent（DIT-03）

- slide「Audit the Gold, Not Just the Agent」，「Gold = last year's reviewed audits」：
  - AUDIT THE MISSES — Gold that punishes a right agent：**Superseded:** final call in a review note；**Inquiry-only:** no document shows it
  - AUDIT THE HITS — Gold that rewards a wrong agent：**Outdated:** changes in methodology
- 讲者说（重转版 [06:07]–[06:42]）：「Just because the audit was reviewed doesn't necessarily mean it's correct」，数据集里的错误「could punish a right agent or it could reward a wrong agent」；例子有被后来推翻的结论、基于不完整证据做的决定、方法论变更，「your underlying gold data may not be as gold as you thought it was」
- slide「The Score Hid It」：

| V1 · Overall | The 9 misses | Late-clearing slice |
| --- | --- | --- |
| **97%** ✓ SHIPS | **4** were bad gold, not the agent | **0 / 12** scored "correct" |

- 讲者说（重转版 [06:51]–[07:29]）：「out of nine of the misses, four was because the eval itself was just wrong. So our agent is actually doing better than expected. But there are 12 examples where [the eval] said this should be no exception, but because of some business rule changes or because of some … methodology changes, these are now wrong. So these are examples where the agent actually missed and was incorrect.」结论：「we don't know if our agent is doing better than expected or doing worse than expected. The score is hiding a lot of important context」
- 我的理解（【待确认】）：「0 / 12 scored "correct"」slide 上的意思似乎是：这 12 条被旧 gold 判为「正确」，但按现行规则其实都错，即 12 条里真正正确的是 0 条。讲者口述的意思与此一致，但 slide 措辞本身有歧义
- slide（IMG_0404）：「DIT-03: a late deposit the 97% eval scored "correct"」

## 7. Human decides：什么时候停下来问人

- slide「Stop and Ask a Human When…」：
  1. It would change work a human signed
  2. A rerun disagrees with the record
  3. The evidence doesn't match the item
  4. A required rule fails
  5. Sources disagree
- slide 下方「Agent → Preparer」消息示例：「"DIT-03: cleared 01/19, 11 business days out, from an affiliate. Investigate before concluding."」
- 讲者说 [07:29]–[08:00]：这几条对应前面几项控制里的情形，出现时「something has gone awry and a human needs to step in」
- 讲者说 [08:00]：「these escalations, they should be easy to verify, but not necessarily easy to approve. We know automation bias is real: if you continually ping the human, they'll just start hitting yes to everything.」所以只标异常，并且清楚指出哪里错、人要在哪里介入
- 讲者说 [08:30]：人审核要花时间，「but that's okay because we know 80% of the time the agent will do what it needs to do, and for those 20% of cases where we want the human to step in, we want to make sure they're stepping in where it counts」（80/20 是口述，slide 上没有，出处【待确认】）

## 8. Defensibility is an AND，以及推广到其他领域

- slide「Pattern for assurance · All or nothing — Defensibility Is an AND」：

| Skip… | …Then |
| --- | --- |
| Authorization | OC-14: signed by a reviewer who saw a different answer |
| Execution Control | OC-22: right answer, but you can't show what ran |
| Evidence Provenance | DIT-02: cites the wrong account |
| Evaluation Integrity | DIT-03: a late deposit the 97% eval scored "correct" |

- 讲者说 [08:56]–[09:27]：跳过 execution control，「you can arrive at the right answer but you can't show how. You may as well attribute it to luck at that point」；跳过 evaluation integrity，「you can arrive at a great flagship number of 97%, you have no idea what the reality is」
- slide「Beyond audit — Same Four Questions, Different Nouns」：
  - CREDIT：Loan declined: which bureau file, which model version, which credit policy?
  - AML：Alert closed: on which transactions, by whom, with what evidence?
  - FUND VALUATION：Position marked: from which price source, as of what time?
- 讲者说 [09:27]–[10:00]：credit 里批或拒一笔贷款、AML 里关闭或升级一条 alert，都可以问同样四个问题，「where you may change the nouns behind them」（转写为 "balance" / "load"）
- 结尾 slide：「"The model said so" is not a defensible answer.」讲者说 [10:00]：「if your defense is the model said so, that's not really going to cut it」；Fieldguide 在招人

## Q&A 精确版

无。手机录音在致谢后结束，Mac 麦的录音在 Q&A 时段不可辨（见素材盘点）。是否有观众提问、问了什么，【待确认】（作者现场印象）。

## 勘误表（Whisper 转写 → 实际）

| 转写 | 实际 | 依据 |
| --- | --- | --- |
| [04:54]–[06:42] 的 "the state of New York City" / "We got it." / "we're going to say" 等重复句 | 幻听，不是讲者原话；正文以手机录音重转版为准 | 重转、slide |
| evidence providence | evidence provenance | slide |
| their freight lines | Calder Freight Lines（虚构客户） | slide |
| going back to bank with affiliation | bank reconciliation | slide |
| four and two checks two deposits | these four: two checks, two deposits | 重转 |
| 20.7 against what happened in 2028 | 2027 run vs 2028 rerun | slide |
| draft unreviewed roast | draft unreviewed rows | slide |
| eagle said / evil said | eval said | 上下文 |
| goal data | gold data | slide（"Audit the Gold"） |
| past few decisions / past recent scores | past few [controls]【待确认】 | 上下文 |
| approving or declining a load | a loan | slide（CREDIT: Loan declined） |
| change the nouns → "change the balance" | nouns | slide 标题 |
| model SENSO | the model said so | slide |
| how you're riding at that point | 听不清（可能是 "how you're arriving at that point"），【待确认】 | — |
| in today's game model / in today's video | 听不清，【待确认】 | — |
| FieldGuide | Fieldguide | slide |

## 出入

1. **Peregrine 的数字**：slide 写「On paper: $223.8M」；讲者说「over $200 million」；CFTC 公告是「in excess of $220 million」声称 vs「approximately $5.1 million」实际；NFA 当时的说法是约 $225 million 中只有约 $5 million（据搜索结果中的二手报道）；Wikipedia 写挪用「as much as $215 million」。$223.8M 的出处【待确认】。slide 的「July 2012」与 CFTC 公告日期一致
2. **银行余额的单位**：slide 的 Balance per bank 是 2,914,600（无单位，按上下文是美元，即约 $2.9M）。原 transcript 转成「$2.9 million / $2.7 million」，重转版成了「billion」。两次转写不一致，按 slide 数字是 **million 级**
3. **80/20**：讲者口述「80% of the time the agent will do what it needs to do」，slide 上没有这个数，也没有给出处
4. **结构**：官方简介说「four distinct controls」；slide 和讲者实际是四项控制 + 第五个页签「Human Decides」，简介里作为「where the agent should stop and a human should decide」出现，两者一致，只是 slide 把它单列成一节

## 【待确认】汇总

1. IMG_0388–0390（Four-Step Process、Everyone Wins）三张 slide 讲者口述的内容（无录音）
2. 是否有 Q&A、问了什么
4. Peregrine「$223.8M」的出处
5. AS 1215 .06 段的逐字原文（PCAOB 页面无法抓取）
6. 「0 / 12 scored "correct"」的准确含义
7. 80/20 的出处
8. 转写里两处听不清的结尾措辞（"how you're riding at that point"、"today's game model"）
9. "past few decisions / past recent scores" 的原话
