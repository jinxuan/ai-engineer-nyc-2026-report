[English](../10-13-1130-your-financial-agent-passed-every-eval-c.md) | **中文**

# Eval 拿了 97% 也不能当辩护：金融 agent 的四道控制

*在 AI Engineer NYC 2026 上，Fieldguide 的 Anirudh Veeraragavan 提出，一个金融 agent 的决定只有在四道控制（授权、执行控制、证据溯源、评估完整性）同时成立时才站得住，并为每一道控制各讲了一个出问题的调节项（reconciling item）。*

**Breakout（Evaluations track）· 2026 年 10 月 13 日（周二）· 11:30–11:48am · Turing Room (Riverside Ballroom)**
**讲者：** Anirudh Veeraragavan，Lead Security Engineer，Fieldguide（Fieldguide 的首位安全工程师；之前在 Plaid 工作五年，离开时是 Staff Software Security Engineer）

---

Anirudh Veeraragavan 从一个看起来已经完工的系统讲起。第一张幻灯片「Let's build a financial agent」展示了一个审计 agent 的典型四步流程：输入（客户的账簿、银行余额调节表、银行证据）、agent（负责匹配，prompt 里写着方法论，以往年度的审计作为示例）、eval（以往审核过的审计）和人（审核人签字）。

![Four-Step Process](../../slides/10-13-1130-your-financial-agent-passed-every-eval-c/01.jpg)
*典型的搭法：输入、agent、eval、人工签字。*

下一张幻灯片「Ship to GA: Everyone Wins」显示了三个绿框：evals 97%、指标全绿、用户满意。然后下面冒出一行字：「Selected for inspection: why was that sufficient?」（被抽中检查：凭什么认为这就够了？）（作者的录音从几张幻灯片之后才开始，所以 Veeraragavan 在开头这几张幻灯片上说了什么，这里没有记录。）

![Everyone Wins](../../slides/10-13-1130-your-financial-agent-passed-every-eval-c/03.jpg)
*Evals 97%、指标全绿、用户满意，然后是检查人员的问题。*

## 1. 为什么金融行业在乎「站得住」

![Fraud is real: Peregrine Financial Group](../../slides/10-13-1130-your-financial-agent-passed-every-eval-c/04.jpg)
*Peregrine Financial Group，2012 年 7 月：「On paper: $223.8M. At the bank: ~$5M.」（账面上 $223.8M，银行里约 $5M。）*

讲风险的这张幻灯片回顾了 Peregrine Financial Group。这家公司在 2012 年 7 月倒闭，此前「伪造银行对账单将近 20 年」。「他们声称有两亿多美元，实际上银行里只有 500 万，」Veeraragavan 说。（CFTC 2012 年 7 月的起诉书说，声称的客户资金超过 2.2 亿美元，实际余额约 510 万美元；幻灯片上 2.238 亿这个精确数字出处不明。）「我不是来追究是谁、怎么回事、为什么发生的，但这件事告诉我们，在金融行业，出错的后果相当严重。所以我们做出的决定背后要有底气，万一将来出了问题，我们得能为这些决定负责。」

至于标准，Veeraragavan 借用了审计行业的说法：「一个有经验、但之前没参与过这项工作的审计师，应该能看懂你做了哪些测试、收集了哪些证据、得出了什么结论。换句话说，一个刚接手的有经验的审计师能跟得上。」（这个想法和 PCAOB 的审计工作底稿准则 AS 1215 一致，不过 Veeraragavan 没有说出处。）用到 agent 上，标准就变成：「一个刚接手的有经验的审计师能跟得上这个 agent」。

要跟得上 agent，需要四道控制：授权、执行控制、证据溯源和评估完整性。「而归根结底，这一切都建立在知道什么时候 agent 该停下、改由人来决定。」Veeraragavan 称之为「a pattern for decision assurance」（一种决策保障的模式），并说它「适用的领域远不止」审计。后面每张幻灯片顶部都有五个标签，每道控制一个，再加一个「Human Decides」。

## 2. 例子：一份年末银行余额调节表

![Bank Reconciliation, 12/31/2026](../../slides/10-13-1130-your-financial-agent-passed-every-eval-c/05.jpg)
*一个虚构客户的年末银行余额调节表，以及 41 个调节项中的四个。*

贯穿始终的例子是一个虚构客户 Calder Freight Lines 在做 2026 年的年终结账。银行显示约 290 万美元，客户账簿显示约 270 万美元；「差额由 41 个调节项解释」。幻灯片上的调节表：

| 项目 | 金额 |
| --- | --- |
| Balance per bank | 2,914,600 |
| + Deposits in transit | 580,150 |
| − Outstanding checks | (731,420) |
| **Adjusted bank balance** | **2,763,330** |
| Balance per client | 2,763,330 ✓ |

Veeraragavan 挑了其中四项，「两张支票、两笔存款」，逐一走过这几道控制：

| 编号 | 类型 | 对方 | 金额 |
| --- | --- | --- | --- |
| OC-14 | 未兑付支票 | Brightline Fuel | 62,500 |
| OC-22 | 未兑付支票 | Rook Logistics | 96,400 |
| DIT-02 | 在途存款，12/31 | Norvale Foods | 61,900 |
| DIT-03 | 在途存款，12/31 | Lakeshore Retail | 480,000 |

## 3. 授权：同样的调用，不同的状态（OC-14）

![Same Call, Different State](../../slides/10-13-1130-your-financial-agent-passed-every-eval-c/06.jpg)
*Agent 可以以自己的身份起草未审核的行；对已审核的行只能提出修改建议，而且永远不能替换人标出的问题。*

第一个场景里，一位人工审核人逐项检查时，把其中一张支票标了出来：这里可能有问题。几天后，来了更多信息，说明它为什么不算例外。这触发了一次 agent 运行，agent 把这一项改写成「无例外」。

「问题在于，人已经审过了，也已经签字了。但他们审过的东西，背后的内容现在变了，」Veeraragavan 说。按通常的标准，agent 没做错什么：「它有权限，有证据，也得出了正确的结论。但在这个具体情况下，它不该覆盖任何东西，因为现在审核人声称审过的东西，和他们实际看过的东西对不上了。」

教训是：「授权不只是『这个 tool call 允许吗』，而是『在这个时间点的状态下，这个 tool call 对这个对象允许吗』。」幻灯片把规则写清楚了。Agent 可以以自己的身份起草未审核的行。它不能写入已审核的行，只能提出建议。它也不能替换人标出的问题。它的输出「必须作为它自己的建议存在，而不是作为权威的事实」。

## 4. 执行控制：要记录，不只是日志（OC-22）

!["We Had Logs."](../../slides/10-13-1130-your-financial-agent-passed-every-eval-c/08.jpg)
*OC-22：2027 年是 $96,400、无例外；2028 年重跑变成 $69,400、有例外。日志对比记录。*

第二个场景里，agent 在 2027 年审核 OC-22，结论是无例外。一两年后这一项被抽中复查，重跑一遍，结果不一样了。幻灯片的标题行：「OC-22 · 2027 run: $96,400 no exception · 2028 rerun: $69,400 exception.」这是怎么发生的？

「我们有日志。我们知道 agent 取了证据，做了一些推理，输出了最终答案。但这不足以还原发生了什么，」Veeraragavan 说。幻灯片做了对比：

| 我们有的：日志 | 我们需要的：记录 |
| --- | --- |
| `fetch_evidence("January statement")` | Claim |
| Final text | Evidence @hash + page + values read |
| A "reasoning" paragraph | Rules applied |
| | Run · model version · config |
| | Human decision |

![A Run Is a Chain of Recorded Steps](../../slides/10-13-1130-your-financial-agent-passed-every-eval-c/09.jpg)
*Fetch（代码）、extract（模型）、match（代码）、conclude（模型）：每一步都连同输入和输出一起保存。*

一次运行应该是一串被记录下来的步骤，每一步有一个 ID（run · item · step），输入和输出都保存下来。幻灯片上 OC-22 的链条：

| 步骤 | 执行方 | 记录内容 |
| --- | --- | --- |
| Fetch | code | statement STMT-JAN-4471, p.3, hash …9c1e |
| Extract | model | check #20977 · 01/08 · 96,400.00 |
| Match | code | 96,400 = 96,400 ✓ |
| Conclude | model | "Cleared 01/08. No exception." |

这条链还把确定性的步骤（「用代码取一项数据或做匹配」）和模型步骤（「用 agent 从 PDF 里抽一个字段，或者得出最终结论」）分开了。有了它，「你就可以单独重跑某一步，而不是整个重来」，把新的运行和旧的对比，并且「找出到底是哪一步出了错」。

![Rerun One Step, Not the Run](../../slides/10-13-1130-your-financial-agent-passed-every-eval-c/10.jpg)
*只针对同一个固定页面重跑 extract 这一步，就能看出哪次运行是对的。*

幻灯片上的代码只用当前模型在同一个固定输入上重跑 extract 这一步，并把结果存成一条关联的新记录：

```python
orig = steps.get(run="R-0031", item="OC-22", step="extract")
new  = extract(orig.input.page_sha, model=CURRENT)   # same pinned input
steps.put(new, parent=orig.id)                       # new record, linked
```

结果：「Today's model: 69,400 · Pinned page: 96,400 → the original stands.」（今天的模型读出 69,400 · 固定页面上是 96,400 → 原结论成立。）新模型读错了金额，把两位数字颠倒了。「我们最初那次运行其实是对的，反而是新的这次错了，」Veeraragavan 说，「不过还是那句话，重点不在于你对不对，而在于你能非常有把握地证明你知道自己是对的。不是碰巧模型挑中了正确答案。」

## 5. 证据溯源：引用是一个身份（DIT-02）

![Right Bank, Right Month, Wrong Account](../../slides/10-13-1130-your-financial-agent-passed-every-eval-c/11.jpg)
*Agent 引用了「January bank statement, p.2」，但那笔 $61,900 的入账在工资账户里，不在被测的运营账户里。*

第三个场景里，模型对 DIT-02 得出了错误的结论。「它说这一项有效，但它指向的其实是错的账户。」Agent 引用的是「January bank statement, p.2」，在那里找到 01/05 来自 Norvale Foods 的 $61,900，账户是 -4488（工资账户）。被测的账户是 -4471（运营账户），这个账户 1 月份没有 $61,900 的入账。

问题就出在引用本身。Veeraragavan 说，「January bank statement, page 2」没什么用，因为一个大的审计项目里可能有很多同名文档。「就算文档名是唯一的，模型看的是哪个版本？谁上传的？它到底从哪儿来的？」而且「这跟 hallucination 都没关系。它可能是一份真实存在的文档。」

![A Citation Is an Identity, Not a String](../../slides/10-13-1130-your-financial-agent-passed-every-eval-c/12.jpg)
*从检索记录而不是模型的文字里解析来源，再检查这条论断依赖的东西。*

「引用是一个身份，不只是一个字符串，」Veeraragavan 说，「你有一个非常具体的 ID、一个非常具体的文档版本、背后内容的一个 hash。」幻灯片上的代码从这次运行的检索记录里查来源，而不是从模型的文字里查，然后断言它和这一项相符：

```python
src = run.retrieval_record[claim.source_ref]   # id + version + hash, not model text
assert src.account == item.account             # check what the claim depends on
assert required_rules(item) <= claim.rules     # and every rule it had to apply
```

一旦论断和一个具体的来源绑定，就可以对证据施加规则和质量约束。比如，「我们可能只要直接来自银行的证据，而不要客户上传的东西，因为客户有可能篡改。」

## 6. 评估完整性：审计 gold（DIT-03）

![Audit the Gold, Not Just the Agent](../../slides/10-13-1130-your-financial-agent-passed-every-eval-c/13.jpg)
*Gold = 去年审核过的审计。有些 gold 会惩罚对的 agent；有些会奖励错的 agent。*

Eval 是用去年审核过的审计建的。「审计被审核过，不代表它一定是对的，」Veeraragavan 说。数据集里的错误「可能惩罚一个对的 agent，也可能奖励一个错的 agent」。幻灯片把对 gold 数据的审计分成两半：

| 审计 miss：惩罚对的 agent 的 gold | 审计 hit：奖励错的 agent 的 gold |
| --- | --- |
| **Superseded：** 最终结论写在一条审核备注里 | **Outdated：** 方法论有变化 |
| **Inquiry-only：** 没有任何文档能证明 | |

「你底层的 gold 数据，可能没有你以为的那么 gold。」

![The Score Hid It](../../slides/10-13-1130-your-financial-agent-passed-every-eval-c/14.jpg)
*总体 97%；九个 miss 里有四个是 gold 有问题；一个 12 项的延迟清算切片。*

幻灯片展示了实际情况：

| V1 总体 | 9 个 miss | 延迟清算切片 |
| --- | --- | --- |
| 97% ✓ ships | 4 个是 gold 有问题，不是 agent 的错 | 0 / 12 被判为「correct」 |

「九个 miss 里，有四个是因为 eval 本身就是错的。所以我们的 agent 其实比预期做得好，」Veeraragavan 说，「但有 12 个例子，eval 说应该是无例外，可是因为一些业务规则或方法论的变化，这些现在是错的。所以这些是 agent 实际上漏掉了、做错了的例子。」也就是说，按口头的解释，旧的 gold 把 12 个延迟清算项都标成了正确，而按现行规则，一个都不对。结论是：「我们不知道 agent 是比预期做得好还是做得差。这个分数掩盖了很多重要的背景。」

## 7. Agent 什么时候该停下来问人

![Stop and Ask a Human When…](../../slides/10-13-1130-your-financial-agent-passed-every-eval-c/16.jpg)
*五个升级给人的触发条件，以及一条 agent 发给编制人的示例消息。*

第五个标签「Human Decides」列出了 agent 该停下来问人的情况：

1. 它会改动人已经签过字的工作
2. 重跑的结果和记录不一致
3. 证据和这一项对不上
4. 一条必需的规则没通过
5. 不同来源说法不一

每一条都对应前面的一个场景。出现其中任何一种，「就说明出了问题，需要人介入搞清楚发生了什么，」Veeraragavan 说。

升级消息怎么写很重要。「这些升级应该容易核实，但不一定容易批准。我们知道 automation bias（自动化偏差）是真实存在的：如果你不停地 ping 人，他们就会开始对什么都点 yes。」所以 agent 只应该标出真正的例外情况，并且说清楚哪里有问题、哪里需要人介入。幻灯片上 agent 发给编制人的示例消息：「DIT-03: cleared 01/19, 11 business days out, from an affiliate. Investigate before concluding.」（DIT-03：01/19 清算，晚了 11 个工作日，来自关联方。下结论前请调查。）

人审这些可能要花点时间，「但没关系，因为我们知道 80% 的情况下 agent 会做好它该做的事，而那 20% 需要人介入的情况，我们要确保人介入在关键的地方，并且有他们需要的背景信息。」（80/20 这个比例是口头说的，不在幻灯片上，也没有给出处。）

## 8. 站得住是一个 AND

![Defensibility Is an AND](../../slides/10-13-1130-your-financial-agent-passed-every-eval-c/17.jpg)
*跳过任何一道控制，四项里就有一项会漏过去。*

「Defensibility is an AND（站得住是一个 AND），意思是四个都得有，否则总会有东西漏过去，」Veeraragavan 说。幻灯片把每道控制和它对应的失败对应起来：

| 跳过…… | ……结果 |
| --- | --- |
| Authorization | OC-14：签字的审核人看到的是另一个答案 |
| Execution control | OC-22：答案对，但你没法证明跑了什么 |
| Evidence provenance | DIT-02：引用了错误的账户 |
| Evaluation integrity | DIT-03：一笔延迟存款，97% 的 eval 判它「correct」 |

跳过执行控制，「你可能得出了正确答案，但没法说明是怎么得出的。那时候你还不如说是运气。」跳过证据溯源，答案错了，你也不知道它从哪儿来，「某个你没法追溯到具体来源的引用」。跳过评估完整性，「你可以拿到一个 97% 的漂亮数字，［但］你完全不知道实际情况是什么，是做得更好还是更差。」

## 9. 审计之外

![Same Four Questions, Different Nouns](../../slides/10-13-1130-your-financial-agent-passed-every-eval-c/18.jpg)
*信贷、反洗钱和基金估值，问的是同样的问题。*

Veeraragavan 说，这个模式也适用于其他金融决策，「只是背后的名词换了」。幻灯片给了三个：

| 领域 | 决策 | 问题 |
| --- | --- | --- |
| Credit | 贷款被拒 | 哪份征信报告、哪个模型版本、哪条信贷政策？ |
| AML | 警报被关闭 | 针对哪些交易、由谁关闭、依据什么证据？ |
| Fund valuation | 持仓被定价 | 来自哪个价格源、以哪个时间点为准？ |

![The model said so is not a defensible answer](../../slides/10-13-1130-your-financial-agent-passed-every-eval-c/19.jpg)
*收尾幻灯片。*

「任何时候，只要你做的金融决策影响可能非常大，你就需要能说明你是怎么得出这个结论的，」Veeraragavan 说，「如果你的辩护是『模型说的』，那真的不行。」收尾幻灯片写着：「'The model said so' is not a defensible answer.」（「模型说的」不是一个站得住的回答。）Veeraragavan 提到 Fieldguide 正在招人，并邀请大家在会议期间继续交流。

---

**链接：** [Schedule](https://ai.engineer/nyc/2026/schedule?session=30afe596-abe2-417c-bd5a-313fa4bc54b3) · [CFTC complaint against Peregrine Financial Group (Release 6300-12)](https://www.cftc.gov/PressRoom/PressReleases/6300-12) · [Peregrine Financial Group (Wikipedia)](https://en.wikipedia.org/wiki/Peregrine_Financial_Group) · [PCAOB AS 1215: Audit Documentation](https://pcaobus.org/oversight/standards/auditing-standards/details/AS1215) · [Auditing an audit agent](https://www.fieldguide.com/blog/auditing-an-audit-agent)（Fieldguide 博客，作者 Kunal Sinha；相关主题，不是讲者写的）

截至撰写时，这场 session 还没有公开的视频。本文依据作者现场的录音（一段手机录音，从 Peregrine 那张幻灯片开始，到讲者致谢之后结束，中间一段识别混乱的部分用手机音频重新转写过）、幻灯片照片和上面列出的资料。之后继续录着的麦克风没有录到任何能听清的内容，所以无法确定有没有 Q&A。[精确笔记](../precise/10-13-1130-your-financial-agent-passed-every-eval-c.md)（中文）保留了时间戳，并完整记录了对转写稿的修正。
