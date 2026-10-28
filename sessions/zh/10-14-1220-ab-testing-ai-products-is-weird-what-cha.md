[English](../10-14-1220-ab-testing-ai-products-is-weird-what-cha.md) | **中文**

# A/B 测试赢了，AI 功能的上线决策才刚开始，而不是结束

*在 AI Engineer NYC 2026 上，Bumble 产品经理 Supriya Subudhi 用这款约会 app 的 AI 照片反馈功能，讲了五种情况：A/B 测试结果是正的，但「该不该上线」的问题仍然没有答案。*

**Breakout · 2026 年 10 月 14 日（周三）· 12:20–12:38pm · Turing Room (Riverside Ballroom)**
**讲者：** Supriya Subudhi，Staff Product Manager，Bumble Inc

![标题页：A/B Testing AI Products Is Weird](../../slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/01.jpg)
*标题页：「What changes when the product is probabilistic.」（当产品是概率性的，什么变了。）*

---

Supriya Subudhi 先给了一个定义：A/B 测试是「一种把某样东西的两个版本放在一起评估、看哪个更好的方法」。然后请大家举手：「谁觉得 A/B 测试结果是正的，这个功能就应该上线？」从现场反应看，Subudhi 说，「你们很多人都很聪明，已经知道一碰上 AI，事情就会变得有点奇怪」。

在和这场 talk 同步发布的文章里，Subudhi 讲了这个问题背后的经验法则：「A/B 测试赢了，就上线。我职业生涯的大部分时间都在遵循这条规则。」到了生成式 AI，它「不再站得住了」。测试设计得没问题，结果也准确，「但它们就是回答不了我真正需要回答的问题：我们到底该不该上线！」

## 背景：Bumble 的 AI 照片反馈

![When it comes to A/B testing AI features, it gets weird](../../slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/02.jpg)
*「A positive result is where the decision journey begins, not where it ends」，下面是五个问题。*

概览幻灯片把论点放在最上面：「A positive result is where the decision journey begins, not where it ends.」（正向结果是决策的起点，不是终点。）下面列了五个问题：用户要了，然后不用；平均值的胜利掩盖了长尾；dashboard 是绿的，账单是红的；换个模型，结果就翻转；可能没法全球上线。Subudhi 说，这些是「我自己具体踩到的、必须克服的」问题。（session 简介里列的是另一组失败模式，包括新奇效应和过早下结论；talk 里没讲这些。）

Subudhi 把产品经理的工作描述为代表用户、解决用户的问题，「同时还要对公司的 P&L 负责，因为我们的资源不是无限的」。贯穿全场的例子是 Bumble 上线过的一个功能：AI 照片反馈。约会 app 上每个人都需要一份资料，上传照片时却不知道别人看了会是什么感觉。Bumble 把自己对「什么样的约会资料有效」的了解拿出来，用 AI 针对用户上传的照片生成改进建议。

## 1. 用户要了，然后不用

![1. Users asked for it, then ignored it](../../slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/03.jpg)
*「General openness to AI doesn't predict whether people will use a specific AI feature.」（对 AI 的普遍开放态度，预测不了人们会不会用某个具体的 AI 功能。）*

| 发生了什么 | 该怎么做 |
| --- | --- |
| 我们的调研显示，用户愿意接受 Bumble 给的 AI 建议。 | 把 AI 谨慎地融入新用户流程，同时做好教育和认知。 |
| AI 照片反馈上线后，真正用的人不多。 | 永远衡量人们做了什么，而不是他们说会做什么。 |

产品工作从调研开始：问卷和用户访谈。在 Bumble 的调研里，一个几乎一致的抱怨是，大家「不知道自己的约会资料够不够好」，而 Bumble 也没帮他们弄清楚。团队给用户看 AI 指导的示例时，「我们聊过的人里，差不多 78% 回来说他们很想得到反馈」。但在 A/B 测试里，一半用户拿到了这个功能，「我们只看到 10% 到 15% 的人真正在用」。（这两个数字只出现在 talk 里；幻灯片和文章里写的是「不多」。）「对 AI 的开放态度，其实预测不了人们会不会用这个功能。」

解法是「产品管理入门课」。Bumble 没有把它做成一个需要用户自己去找的独立功能，而是放进了用户已经在走的流程里。用户上传照片时，app 问他们要不要反馈。用户「碰了壁」、一直没有匹配时，app 建议他们去看看资料的反馈。「采用率就上去了。」Subudhi 在文章里写道：「起作用的不是模型，而是这个功能出现在流程里的位置。」还写道，AI 功能需要「更加清楚地说明这个功能会做什么」，才能赢得信任。

## 2. 平均值的胜利掩盖了长尾

![2. The average win hides the tail](../../slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/04.jpg)
*「A positive average can sit on top of individual outputs that are wrong. So success evaluation itself changes.」（正向的平均值下面，可能压着一个个错误的输出。所以，评估成功的方式本身也变了。）*

| 发生了什么 | 该怎么做 |
| --- | --- |
| 顶层结果看起来不错。 | 在功能出错会造成伤害的地方，要严格。 |
| 有些人收到了奇怪的反馈，比如照片里明明已经在笑了，还被建议多笑一点。 | 读真实的输出（eval 在这里很有帮助）。 |
| | 检查数据的长尾，按细分人群拆开看，而不只看平均值。 |

顶层数据不错：上传照片的人变多了，收到反馈的人比没收到的人改资料改得更多。但细看之下，「发生了一些奇怪的事」。AI 会建议用户在已经在笑的照片里多笑一点。

Subudhi 拿两个产品做对比，说明风险的高低不同。Spotify 的 AI DJ 会生成歌单，「AI 生成的四首歌里有两首不好，没关系。但如果［一个银行］app 叫你花 2,000 美元，而你其实根本花不起，那就不行了」。

Bumble 先给反馈能说什么、不能说什么设了 guardrail：「我们不能叫人去改他们控制不了的东西，比如肤色、身高。这些是 AI 不该碰的。」接下来是读真实的输出，用 eval 工具看真实用户收到的是什么、prompt 做得好不好。成功的标准从整体对比转到了长尾：「女性对我们反馈的反应和男性一样吗？」不同国家的人反应一样吗？「因为也许生成的法语和生成的英语不一样。」文章解释了为什么这是新问题：「普通功能，测试组里每个人看到的都一样。生成式功能，每个人看到的都略有不同。」

## 3. dashboard 是绿的，账单是红的

![3. Green dashboard, red bill](../../slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/05.jpg)
*「With generative AI, success makes the bill bigger.」（用生成式 AI，越成功账单越大。）图里画的是两条成本曲线的形状，没有数字。*

幻灯片上的图以用户数为横轴、成本为纵轴，画了两条线：一条陡的，标着「Generative feature: cost grows with every use」（生成式功能：每用一次成本都在涨）；一条平缓的，标着「Recommendation model: cost grows slowly with users」（推荐模型：成本随用户数缓慢增长）。旁边是（照片右边缘被截掉了）：

| 发生了什么 | 该怎么做 |
| --- | --- |
| LLM 路线行得通，但成本大约是更简单方案的 10 倍。 | 不要一开始就直接上最好的模型。 |
| 再加上对生成式 AI 反馈跑 eval 的成本。 | 同时衡量短期和长期的回报。 |

因为这个问题对用户太重要，Bumble 想要最好的质量和准确率，就「用了最好的 LLM」，结果「大约比更简单的方案贵 10 倍」。再加上跑 eval 的成本：「那就实在太多了。」

Bumble 的匹配算法里本来就用了 AI，但传统 ML 的成本结构不同：前期算力和训练成本高，之后「每多一个用户使用的增量成本并不高」。用生成式 AI，「你的产品越成功，用的人越多，用得越频繁，你的账单就涨得越多」。

Subudhi 认为，团队得先想清楚自己为什么要用 AI；很多同行「只是从公司那里接到了必须用 AI 的指令」。例子是 Amazon 的购物助手。短期看，好的推荐也许能带来更多购买。长期看，「有一个靠得住的助手，可能会让人们选 Amazon 而不是别的电商」，这关乎品牌资产和用户留存。预算由这个目的决定：如果是短期收益，ROI 可以很快拍板；如果是长期收益，「也许值得多争取一些预算」。文章对那个时刻写得很直白：「那是一场坐着很难受的会，因为 dashboard 是绿的，答案却仍然是不。」

## 4. 换个模型，结果就翻转

![4. Swap the model, flip the result](../../slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/06.jpg)
*「A winning result belongs to the model, not the feature.」（赢下来的结果属于模型，不属于功能。）*

| 发生了什么 | 该怎么做 |
| --- | --- |
| 我们换成了更便宜的 LLM，以及其他 ML 模型。 | 从一开始就测试更便宜的版本和非 AI 版本。 |
| 成本降了，对结果的影响也降了。 | 每次换模型都要重新测试。 |
| | 找到能保住结果的最便宜方案。 |

最好的模型成本无法规模化，Bumble 就换成了更便宜的。「成本降下来了，结果的影响也跟着降了，因为准确率也降了。」这说明 A/B 结果「来自我们测的那个模型，而不是反馈这个概念本身」。

教训是多测试、坚持用数据说话。有了 eval 体系，团队可以比较不同模型在不同数据集上的表现和成本，也能快速重测不同版本和 prompt。Subudhi 在台上说：「最后我们落到了最便宜的方案上，它帮我们拿到了好结果。」文章的说法不同：换模型之后，「我们回到了原点，要去找、去测一个既在预算内、测试结果又是正的版本」，没有说找到了这样的版本。

## 5. 可能没法全球上线

![5. There may not be a global rollout](../../slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/07.jpg)
*「Regulations are constantly changing and can stop a feature that works in one country from shipping in another.」（监管在不断变化，可能让一个在某国行得通的功能没法在另一国上线。）*

| 发生了什么 | 该怎么做 |
| --- | --- |
| 照片反馈在美国上线了。 | 在实验之前就把法务和合规拉进来。 |
| 在大多数其他国家，直到法务审核通过之前都被挡着。 | 尽早决定要做哪些市场、哪些版本。 |
| | 准备好在某些市场一直避开到最后。 |

Subudhi 提到 Meta 最近推出的 Muse AI：「我来自伦敦，所以还没［用过］。」因为它「只在美国和加拿大可用」。世界各地的 AI 监管不一样，而且一直在变，不合规会带来财务和法律风险。（报道称 Meta 的 Muse app 于 2026 年 9 月在美国上线；扩展到加拿大的说法只出现在一篇报道里。）

Bumble 的功能也是同样的路径：「我们在美国上线了，然后就到此为止。我们不得不停下。」这个功能没法原样上线到其他国家。（TechCrunch 2026 年 2 月关于 Bumble AI 工具的报道也说照片反馈只在美国提供。）结论是：「不管你是不是产品经理，不管你是想做产品工程师，还是 forward deployed engineer，每个人都得开始成为通才，尽早……考虑合规和法律。这件事不能往后放。」

Subudhi 描述了一系列选择。团队可以从一开始就计划全球上线，在所有市场的限制之内去做。也可以为影响最大的美国做出最好的版本，「然后也许别的地方都不上」。或者多花一笔成本，做一个按国家表现不同的版本。「如果你做出了最好的东西，却没法交到用户手里，那就毫无意义。」文章还补充了一点关于学习的：「你只能在允许测试的地方学到东西，所以一个市场的结果对下一个市场说明不了太多。」

## 规模化之前的五个问题

![To scale AI features, ask these 5 questions](../../slides/10-14-1220-ab-testing-ai-products-is-weird-what-cha/08.jpg)
*最后一张幻灯片上的五个问题，每个对应一个问题点（照片边缘被截掉）。*

Subudhi 最后给每个问题配了一个提问：

| 问题 | 提问 |
| --- | --- |
| Users asked for it, then ignored it | 人们是在用它，还是只是说想要它？ |
| The average win hides the tail | 你读过真实的输出吗，而不只是看平均值？ |
| Green dashboard, red bill | 把所有成本算进去之后，它还划算吗？ |
| Swap the model, flip the result | 在最好的模型上赢了，你负担得起吗？ |
| There may not be a global rollout | 你在法律上弄清楚能在哪些市场上线了吗？ |

「因为问题已经不只是这个功能有没有赢下 A/B 测试了。」Subudhi 说。文章的结尾也是同一个意思：「『赢了吗？』仍然是我的起点。只是它不再是我的终点。」

---

**链接：** [日程](https://ai.engineer/nyc/2026/schedule?session=11fef189-97e8-4c8e-87e8-dac43d800846) · [A/B testing AI products is weird](https://dev.to/dailycontext/ab-testing-ai-products-is-weird-576h)（Supriya Subudhi，DEV）· [Bumble adds AI-powered photo feedback and profile guidance tools](https://techcrunch.com/2026/02/26/bumble-adds-ai-powered-photo-feedback-and-profile-guidance-tools)（TechCrunch）· [Providing feedback on your photos](https://support.bumble.com/hc/en-us/articles/32422047589277-Providing-feedback-on-your-photos)（Bumble 帮助中心）

截至写作时，这场 session 还没有发布视频。本文依据作者在现场的录音、作者拍的全部 8 张幻灯片照片，以及讲者本人的文章。录音以 Subudhi 的致谢结束，没有 Q&A。Subudhi 提到的一个 Bumble 用户数被省略了，因为转录稿里的那个数字不可能是对的。[精确笔记](../precise/10-14-1220-ab-testing-ai-products-is-weird-what-cha.md)（中文）保留了时间戳，并记录了 talk 和文章之间的出入。
