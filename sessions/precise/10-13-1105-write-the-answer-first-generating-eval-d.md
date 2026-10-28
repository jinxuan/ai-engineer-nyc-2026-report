# Write the Answer First：精确版

> Precise notes for this session, compiled from the official video captions where available, the author's room recording, slide photos and the primary sources linked inline. Written in Chinese with English terms and quotes. Source tags: slide, 讲者说 (said in a recording), 资料 (primary source), 我的理解 (inference), 【待确认】 (unverified).

状态（2026-10-27）：素材整理完、slide 数字逐一读过。**没有 Q&A**（视频在致谢处结束）。

## 素材盘点

| 素材 | 情况 |
| --- | --- |
| 录音 / transcript | 有录音（take 1，约 19 分钟），但 Whisper 转写**不可用**：只有 "Thank you"、"we are going to be able to do that"、"We will be back to the beginning of the day" 的重复幻觉。本文不引用 |
| 视频 | 有：[How to Test AI Agents When No Benchmark Exists — Nathanael Weill, Air Canada](https://www.youtube.com/watch?v=c_SPW__jF9w)，2026-10-22 上传，19:50。自动字幕是正文主要来源。视频在 [19:39] 致谢处结束，无 Q&A |
| slide | 作者拍的 26 张（IMG_0362–0387），全部属于本场（第一张「Air Canada has lots of agents and no benchmarks」是本场开头，不是上一场）。IMG_0378 和 IMG_0379 是同一页「From four weeks to five minutes」 |
| 讲者资料 | 无本场 slide 或仓库。讲者 Medium：[medium.com/@naweill](https://medium.com/@naweill) |
| 其他一手资料 | YouTube 视频描述与章节；日程 session 简介；[RAGAS 论文](https://arxiv.org/abs/2309.15217)；HyPE 论文（见第 2 节） |

## 基本信息

- 时间地点：2026-10-13 11:05–11:23am，Turing Room (Riverside Ballroom)，Breakouts，Evaluations Track
- 讲者：Nathanael Weill，Air Canada 的 Director, Data Science & AI（日程）。讲者说 [00:46]：「I joined Air Canada in June of this year」；他的团队负责给 operation、corporate、commercial、loyalty 四块业务交付 AI 方案
- 开场的比喻（讲者说 [00:18]–[00:35]）：像戒瘾互助会自我介绍，「My name is Nathaniel. And I'm a non-evaluator」，讲「how I quit this addiction of shipping an agent without any evaluation」。结尾 slide 呼应：「The 24-hour chip is the hardest one」「720 days. Keep coming back.」
- 日程简介：RAG 评测曾经各团队自己摸索、要几个月，RAGAS 出来后 faithfulness、context relevance、answer relevance 成了现成 playbook，「the same evaluation now takes a week」；自定义 agent（审合同、分拣发票、内部 workflow）没有对应物。「It won't be RAGAS for agents, but it will be enough to get started on Monday.」
  - 资料：RAGAS 论文 Es et al., [arXiv:2309.15217](https://arxiv.org/abs/2309.15217)（2023）
- 视频章节（资料，视频描述）：00:00 Intro / 00:47 No benchmark, no experts / 01:41 A lesson from a telecom RAG project / 04:43 Weekly expert reviews were too slow / 05:38 Flip it: write the answer first / 06:33 From one test a week to dozens a day / 07:30 Supplier contract review / 10:08 How the contract agent works / 10:39 Five ways to build a test set / 12:31 Planting rule violations / 13:39 Removing clauses / 14:42 Changes that shouldn't matter / 15:38 Templates with blanks / 16:39 Avoid LLM judges when code can check / 18:07 Use expert feedback to build better tests
- slide 顶部有一条进度导航：Problem › Tried › Stuck › New method › Result，两段故事（2024、2026）各走一遍

## 1. 问题：没有 benchmark，也没有专家

- slide「Air Canada has lots of agents and no benchmarks」：Operations / Corporate / Commercial / Loyalty；Chatbots · AI workflows · ML and optimization pipelines with agents inside
- 讲者说 [01:04]–[01:49]：任务太具体，「we don't have any public benchmark we can rely on」；更重要的是「we are data scientists that can build agents … but we are not domain experts」，专家在别的业务部门，很忙，接触有限

## 2. 2024 的前车之鉴：telecom RAG

- slide「I hit this problem once before, in 2024」：The agent：A Q&A assistant；The documents：Telecom standards and vendor documents；The task：Answer a technical question；A good result：The right answer, from the right page。**The hard part**：We were data scientists, with limited a[ccess to] the subject-matter experts who could grade our answ[ers]。底注被挡（「…telecom co… …ed Air Canada.」，我的理解：「在一家 telecom 公司，加入 Air Canada 之前」，【待确认】）
- 讲者说 [02:08]：约 10,000 份文档，外加大量 vendor 文档，找对文档本身有逻辑；[02:22]「2024, everybody wanted to build RAGs … on LinkedIn it's super easy. It's not」
- slide「Four teams, same challenge. Only one made it work」：Team A / Team B / Team C didn't work，Us worked。「Why the difference? I still don't know why the other teams didn't succeed. I only kno[w]…」，底注「…same task… team. Neither the company nor the other team[s]…」（被挡）
  - 讲者说 [02:50]：竞争的是「two external, one internal. It's not like in parallel, but in segments」；[03:01]「our solution actually worked. What mean worked is actually delivering the value. The people liked the solution, actually used it」
- **Tried**：slide「Every week we added a part. Quality didn't move」：起点 Question → Retrieve → Answer，之后逐周加 + Better parsing、+ Chunker、+ Re-ranker、+ HyPE、+ Query rewriter、+ Reverse HyPE、+ Reasoning step 1、+ Reasoning step 2；Answer quality 曲线 Week 1–7「still flat」。底注：「One [try a] week: that's how fast we could test. Every fix broke something else.」
  - 讲者说 [04:13]：论文里的技巧「on paper it worked for them … but for us it didn't really help」；[04:22]「when we add complexity, we found that like we break something else … that's actually the real problem」
  - 资料（HyPE）：Hypothetical Prompt Embeddings，Domen Vake、Jernej Vičič、Aleksandar Tošić，[Bridging the Question-Answer Gap in RAG: Hypothetical Prompt Embeddings](https://arxiv.org/pdf/2607.29402)，发表于 IEEE Access；做法是索引时为每个 chunk 预生成假想问题并嵌入，检索变成问题对问题的匹配。「Reverse HyPE」没找到出处，【待确认】
- **Stuck**：slide「Each change waited a week for an expert」：1 We change the agent（minutes）2 A telecom expert reads its answers（the only way to know if it helped）3 …in their one review slot a week（experts have day jobs）**= 1 try per week**（右侧日历：每周只有周三一格）
  - 讲者说 [04:58]：每周一次大会，「a giant giant screen with five different chatbot, ask the same question to all of them」，请专家选更喜欢哪个；「funny enough they're not always agreeing … you put [two] experts [in] the room you get three opinions」
- **New method**：slide「So we wrote the answer first」：BEFORE: the expert grades the answer：Question → Agent answers → Expert reads → 1 week；NOW: we pick the answer, then write the question：Pick a page → LLM writes a question → Agent searches → Right page?。**Why it works**：We know which page holds the answer before the test runs. Code checks it in seconds. No expert needed. 底注：「"[Righ]t page" = the passage of the documents that holds the answer. The test: does the system fin[d it]…」
  - 讲者说 [05:51]：「there is nothing new here … we reverse the problem」；[06:28]「for those that work in rags, there is plenty of implementation of this thing. I didn't invented it」
- **Result**：slide「Then we could test every change, every day」：1 try per week（one expert review）→ **dozens** tries per day（automatic, on generated questions）；「Every change got a score. Keep it if the score goes up, drop it if it goes down.」曲线 W1–W4 上升。底注：「…[argumen]ts in meetings ended the same way: let's just run it.」
  - 讲者说 [06:45]：「we went from one evaluation a week to dozens a day」，专家打分的准确率逐周上升；「removing a lot of complexity help」，重点是「the right embedding model, the right chunking, the right alpha in the BM25」（我的理解：指 hybrid search 里 BM25 与向量检索的权重 alpha）
- slide「Why the difference? I treat it as an axiom」：Team A / B / C no test set，Us had a test set。**MY AXIOM**：「If you can't tell whether a change helped, you can't make the agent better.」「An axiom: you can't prove it, but every time you apply it, it holds.」底注：「[A test set] is the one thing I'm fairly sure we had and the others didn't.」
  - 讲者说 [07:15]：「I'm pretty sure the other team didn't had any test set. So they were using this kind of long loop with expert」

## 3. 2026：Air Canada 的供应商合同审查 agent

- slide「Now, in 2026 at Air Canada: a different challenge」：Supplier contract → **Agent checks each section against our rules** → Suggested changes, each with a reason → A reviewer accepts or rejects each one。SAME AS IN 2024：Lots of documents / Busy experts / No benchmark。WHAT'S NEW：It spots problems, not answers / Each one needs the rule that proves it / It writes a fix, or leaves it alone。底注：「[Inspired] by a true project. Names and details changed. Numbers are real.」
- 讲者说 [07:57]–[08:35]：专家逐条看条款、给反馈（不合规、需重写）；慢则供应商交付延误，「if we miss a clause, it's also very costly … it can backfire to us」；[08:56]「You can imagine the price of a lawyer by hour」
- slide「So we tried the RAG fix again. It kept us on the flat line」：Where we were（W1–W8 平线）→ Where we want to be（上升线）。「How can we test every change when the agent's job is spotting problems, not finding pages?」底注：「[Tes]t: we know which page holds the rule. Does the agent fetch it?」
- slide「We were stuck because each job needs its own test」（the contract agent 四步）：**Spot**（Is there a problem in this clause?）needs a new test；**Find**（Which rule or past contract proves it?）✓ the search test covers it；**Write**（Draft the change）needs a new test；**Decide**（Flag it, or leave it alone）needs a new test。底注：「[The search] test c[overs Find, not]… Spot, Write or Decide.」
  - 讲者说 [10:10]：「it's a bit simplified, but actually the agent has four steps」；Spot 是把合同各节分到具体 domain，再判断有没有缺陷或需要改的地方

## 4. 五种不靠专家造测试集的方法

- slide「Working the steps: five ways, no expert needed」：

| # | 方法 | STARTS FROM |
| --- | --- | --- |
| 1 | Use past decisions as the answers | a decision someone already made |
| 2 | Choose the answer, then write the contract | a rule |
| 3 | Take a good contract and break it | a good contract |
| 4 | Check how the answer moves | no answer at all |
| 5 | Turn one example into many | one example |

WHO DOES WHAT：You（the human）/ Test writer（an AI that writes tests）/ Agent（the one we test）/ Code（ch[ecks]…）

- 讲者说 [11:09]：「My goal is not to show off. My goal is to get you inspired in how to reapply those techniques」

**方法 1：用过去的决定当答案**（slide「Step 1 of 5」）
- 1 You：As received: cap = $500K → 2 You：As signed: cap = $2M → 3 Code：diff，Answer: raise the cap → 4 Agent：agent? Does the agent say it?
- Provide：old contracts, before and after；Generate：nothing: the past is the answer；Check：exact match
- **What we found：we asked for these pairs. None arrived.**
- 讲者说 [12:19]：团队不保留所有版本，「they don't keep the original one, they just keep the last one」，还有谈判过程，「so this thing doesn't work」

**方法 2：先选答案，再写合同**（Step 2 of 5）
- 1 You：cap ≥ $1M（Take a rule）→ 2 You：cap = $500K → flag（Pick the answer）→ 3 Test writer：Generate the contract → 4 Agent：§8? Does it flag §8?
- Provide：rules and their answers；Generate：contracts built around them；Check：exact match
- **What we found：43 rules → 275 answers, known before any test ran.**
- 讲者说 [12:56]：「we have the cap above $1 million … every contract that Air Canada should follow」；规则来自专家或现有文档，「cannot guarantee the coverage of everything. So we … also came up with our own rules」；[13:45]「we generate at the end 275 tests that we know can run and we know the answer … we planted the problem, and we try to find it back」

**方法 3：拿一份好合同，把它弄坏**（Step 3 of 5）
- 1 You：A good contract → 2 Test writer：Delete one clause → 3 Code：missing: breach clause（Now you know the answer）→ 4 Test writer：Mix in clean ones
- Provide：a good, real contract；Generate：broken copies + clean ones；Check：code, by planted ID
- **What we found：deleted sections were noticed 1 time in 3.**
- 讲者说 [14:12]：难，因为供应商模板不同，「Knowing what should be in what contract require a bit of thinking」；[14:34]「we found that our agents are missing most of the clause that has been removed. So that's a very very strong benchmark」

**方法 4：没有正确答案时，看答案怎么动**（Step 4 of 5）
- The test writer makes a small edit → Code compares the agent's two answers：Rename a party = same flags；Reorder the clauses = same flags；Run it again = same flags
- Provide：one contract；Generate：small edits to it；Check：code, two runs
- **What we found：same contract, 5 runs: 1 in 3 judgments changed.**
- 讲者说 [14:57]：改供应商名称、地址这类「should not have any impact on the number of flags」；[15:27]「You don't need to do any LLM as a judge. The previous speaker showed like it's very hard task. So, we stay away from LLM as a judge as much as possible」

**方法 5：一个例子变很多个**（Step 5 of 5）
- 1 You：Notice: 30d / Pay: net 60（One example）→ 2 You：Notice: [N] / Pay: [terms]（Make blanks）→ 3 Test writer：15d · net 30 / 90d · net 45 / 45d · net 90（Fill them many ways）→ 4 Code：notice < 60d → flag（The check reads the blanks）
- Provide：one example with blanks；Generate：many fillings；Check：code reads the blanks
- **What we found：more varied contracts, checked the way reviewers work.**
- 讲者说 [15:44]：专家本来就用带空白的模板，「we generate an array for each of those blanks」，有的组合违反规则、有的不违反，事先知道；[16:18]「our expert already use templates, so we can reuse them」

## 5. 结果：从四周到五分钟；先用最便宜的检查

- slide「From four weeks to five minutes」：BEFORE **4 weeks**：Beta feedback came back after the code had already moved on。NOW **5 min**：A scripted run. Code matches each finding to a planted problem。下方阶梯：[Ch]eapest check first：Exact match → Code → AI judge → Hu[man]。底注：「[Because we] wrote the [answer] first, the check is just code. No AI judge needed for any of the five.」
- 讲者说 [16:29]：「4 weeks to get a feedback, and we can get a script to run and get the evaluation back in 5 minutes」
- 讲者说 [16:51]：「As soon as we step into LLM as a judge … we got so much noise from the LLM as a judge compared to the noise with the agent that we [are] building. Solving this problem is actually almost as hard as solving the right problem」；可以做 fact checking 等，「but actually it require so much work and we could not found the right one for this project」

## 6. 同一招用在合同之外

- slide「The same trick works far beyond contracts」（1. Write the answer → 2. Generate the input → 3. Run and check）：
  - **Trajectory**：Tool calls, in order: `get_weather` → `book_court` → `send_invite` → Write the scenario that needs those calls → An LLM plays the user. Code checks the order
  - **Text to SQL**：A slice of data, pulled by a query you wrote → Generate the question in plain text → Run the agent's query. Compare rows
  - **Your agent**：Pick the answer → Generate the input that leads to it → Code checks i[t]
  - 底注：「[Sa]me pattern every time: [write the answ]er first, then build the test around it.」
- 讲者说 [18:04]：「Doesn't apply to everything. And sometime we had to re-engineer our solution to fit this paradigm … With the original design we could not evaluate it. We switched to something that we can actually evaluate」
- slide「Real feedback makes the tests better over time」：Generated tests → Agent → Real feedback → New real tests → 回到 Generated tests，中间「One loop at a time」；右下角 **arize** logo
- 讲者说 [18:36]：专家反馈「we don't use this feedback to fix the agent. We use this feedback to generate better test. Better prompt. Better ideas how to remove clauses」；[18:54]「that can be done by [A]rize. There's a lot of tools there. Thanks for the invitation」（我的理解：Arize 是本 track 的邀请方/赞助方，【待确认】）

## 7. 结论

- slide「No benchmark? Write the answer first」：1 **Write the answer first.** Then the check is just code, and you can test every change. 2 **Generated tests are a starting point, not a ship decision.** A person still checks every change. Real feedback takes over.
- 讲者说 [19:10]：「it's not enough to know if you can ship the product. Just a starting point … It's not the go no go for the solution」
- slide「Are you ready to quit delivering agents without evaluation too?」：「The 24-hour chip is the hardest one. Pick one agent. Write ten answers before you write the tests. Run them tomorrow.」「720 days. Keep coming back.」— Nathanael Weill
  - 我的理解：24-hour chip 是戒酒互助会的「清醒 24 小时」纪念币；720 天约等于从 2024 年那个项目到现在的两年（讲者说 [07:40]「has been 2 years now」）

## Q&A 精确版

无。视频在 [19:39] 讲者致谢处结束（总长 19:50）；作者录音转写是幻觉文本，无法确认现场是否有问答【待确认】，按 18 分钟 slot 和 19 分钟的视频长度看，应该没有时间问答（我的理解）。

## 勘误表（字幕 → 实际）

| 字幕 | 实际 | 依据 |
| --- | --- | --- |
| Thank you for joining this station | this session | 上下文 |
| Nathaniel | Nathanael | 日程、slide |
| Arcada | Air Canada | 上下文 |
| rug / rags | RAG / RAGs | 上下文 |
| 19 90% of the time | 90% of the time | 上下文 |
| abiding that is well known | 【待确认】（可能是 "a paradigm that is well known"） | 上下文 |
| Rise | Arize | slide logo |
| the agent to eat some hand points | to hit some endpoints | slide（Trajectory 行） |
| Test to SQL | Text to SQL | slide |
| the 24 hour chip | the 24-hour chip | slide |
| ask the expert to explain the room you get three opinions | put [two] experts in the room（【待确认】人数） | 上下文 |

## 出入（来源之间不一致的地方）

1. **方法 3 是不是「最强」**：讲者在列五种方法时说第 5 种「is actually the most powerful. It's very hard」（[11:49]），讲方法 3 时又说「that's the most powerful approach … that we had today」（[14:29]）。slide 没有给哪一种排序
2. **方法 3 的结果措辞**：slide「deleted sections were noticed 1 time in 3」；讲者说「our agents are missing most of the clause that has been removed」。两者一致（漏掉约 2/3），但讲者没有口头给出比例
3. **方法 4 的一致性数字**只在 slide 上（「same contract, 5 runs: 1 in 3 judgments changed」），讲者口头没有提
4. **反馈周期**：2024 项目是「1 try per week」（slide、讲者说）；2026 项目 slide 是「4 weeks」（Beta feedback）。两个项目不同，不矛盾，但写文章时不要混用
6. **HyPE 的时间**：slide 把 HyPE 放在 2024 项目的周历里；能找到的 HyPE 论文是 Vake 等人的版本，arXiv 编号 2607.29402，另有页面显示 2025 年发表于 IEEE Access，都晚于 2024。【待确认】讲者当时用的是不是同一方法，或者这页周历是事后示意（slide 底注只说「Names and details changed. Numbers are real」挂在 2026 那页）
7. **2024 竞争团队**：slide「Four teams」（A、B、C + Us）；讲者说「two external, one internal」，合起来一致

## 【待确认】汇总

1. 「I hit this problem once before」页底注（推测是「在一家 telecom 公司，加入 Air Canada 之前」）
2. 「Four teams」页底注全文
3. 「Reverse HyPE」的出处；HyPE 与 2024 时间线的关系
4. 字幕 "abiding that is well known" 的原话
5. 「experts in the room you get three opinions」里的人数
6. Arize 与本 track 的关系（讲者「Thanks for the invitation」）
7. 现场是否有 Q&A
