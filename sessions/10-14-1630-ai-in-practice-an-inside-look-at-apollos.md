**English** | [中文](zh/10-14-1630-ai-in-practice-an-inside-look-at-apollos.md)

# At Apollo's CLO Arm, a Three-Day Portfolio Build Now Takes Minutes

*At AI Engineer NYC 2026, Jack McDonald of Apollo's Redding Ridge told Rowspace co-founder Michael Manapat how AI agents working over the firm's own reconciled data cut portfolio construction from days to minutes, and why trust came from engineers sitting with the front office every day.*

**Featured Keynote · Wednesday, October 14, 2026 · 4:30–4:48pm · Simons Room (Metropolitan East), Main Stage**
**Speakers:** Michael Manapat, Co-Founder, Rowspace; Jack McDonald, Principal, Apollo Global Management, and COO, Redding Ridge

---

The session was a conversation rather than a presentation: Michael Manapat asked the questions, Jack McDonald answered, and no slides were shown. The official description called it a "Rowspace customer co-presenting keynote."

Manapat introduced Rowspace as "an agentic platform that uses AI to help financial services partners like Apollo deal with messy, fragmented data." The company uses agents "to do the work of data engineering," reconciling data across many systems "so that portfolio managers, traders, and so forth can make decisions faster and more accurately." Rowspace launched publicly in February 2026 with $50 million in funding led by Sequoia and Emergence Capital; Fortune reported that Manapat previously held senior technical roles at Stripe and Notion, and that the platform processes data inside customers' own cloud systems.

McDonald introduced himself as a principal at Apollo and COO of Redding Ridge, "our structured credit and CLO manager that is captive to Apollo," a "fully separate entity, operating company that manages about 50 billion in assets," mainly in collateralized loan obligations (CLOs). McDonald joined Apollo in 2020 and worked in its financial institutions and asset-backed groups before Redding Ridge. (Redding Ridge's own recruiting page gives assets under management of about $43.9 billion as of March 31, 2026.)

## 1. When AI becomes mission-critical

Manapat framed the conversation around a gap Rowspace sees across financial firms: "there's a pretty wide gap between when AI is nice to have and when AI can actually become mission critical to a business." At Apollo, Manapat said, AI was proving transformative to how the firm operates.

McDonald described the starting point. Redding Ridge manages many fund entities and receives "thousands and thousands of data points" a day across its investments, with what McDonald described as "thousand name loan pools" and credits across several businesses that it assesses daily. "CLO management is very active and it's traded in the broadly syndicated loan market," so information has to be at hand. The goal is data that is "accurate, consistent, and in a format that we can utilize."

The difficulty is that this information is private. Unlike public equities or public fixed income, it is "very fragmented in various documents." So Redding Ridge brought in Rowspace to "come in and actually sit with our teams," learn the actual calculations and identify "what's actually the golden source of truth," then keep improving the AI to understand and repeat that work.

## 2. Portfolio construction: from days to minutes

Manapat asked for a concrete example of the "fire drills" that used to occupy a team for a week or more. McDonald gave two.

The first was portfolio construction. CLOs "are very test specific" and "very rules based," with a specific waterfall governing where money flows, all of it written into "deep indenture documents and multiple different legal agreements." A portfolio manager building a mock portfolio had to review those documents, build Excel models and assemble "anywhere from 200 to 400 single credit names." Before Rowspace, that took "anywhere from two to three days in a manual Excel." Now, with real-time information pulled from document libraries, trade and compliance systems and live pricing, "a portfolio that would take somebody days" takes "a matter of minutes."

The tool does not finish the job. It gets a portfolio to "80, 90% completion," McDonald said, and the PM applies judgment to refine "that last 10% to make it theirs."

## 3. The weekly trade reconciliation

The second example was trade reconciliation for the firm's warehouse purchases. Redding Ridge uses bank financing to buy loans in the fixed-income market at any time. Each week a team reconciled what had been bought and sold: "literally going into bank accounts to understand statements, where things actually settled," then emailing a recap to management. That took "many hours of the week for two to three individuals."

Now the same report has been rebuilt in Rowspace as a visualization. It is "repeatable in the span of a minute" and automated; the team's job is to QA it each day. McDonald called the result "a much more rinse and routine type process."

| Workflow | Before | After |
| --- | --- | --- |
| Mock portfolio of 200–400 credits | 2–3 days in manual Excel | minutes, to 80–90% complete; PM finishes the last 10% |
| Weekly trade reconciliation | many hours a week for 2–3 people | report regenerated in about a minute; daily QA |

## 4. Why not just use Claude or ChatGPT?

Redding Ridge's team makes trading and portfolio management decisions, Manapat noted, so accuracy matters more than for someone reading a chatbot's answer. Manapat put two questions together: "Why can't you just plug this stuff into Claude and or ChatGPT and see what happens?" And how does a team come to trust an AI product enough to "make trades, build portfolios"?

McDonald said general models are "very beneficial, but it's all general information … and none of your own actual data." They are good for research, new ideas and getting up to speed on a product or investment quickly. Redding Ridge's approach to AI has been "owning our own data and having some type of platform and technology that sits on top of that that we know is to be true."

Trust, McDonald said, came from how Rowspace worked, which differed from other technology vendors: "they're boots on the ground. Michael sits with our team on a daily basis." The reason is that "one piece of data … might come from 20 different sources and there's only one that's actually right." Rowspace's team sat with portfolio managers and finance staff to learn the calculations, which data is correct and which sources should be overridden. Because Redding Ridge's own people validated the results, drawn from the trade order management system, the performance system, Bloomberg and pricing indexes, the team can rely on them. Elsewhere in the company, McDonald said, systems remain fragmented and data is still moved by hand, "whereas Rowspace is now just automating that all for us."

## 5. How Apollo became "AI-pilled" early

Manapat said that "even early last year before Claude Code became a thing, when Cursor was still in the tab completion era," Apollo was among the most "AI-pilled" firms Rowspace spoke with, both in its view of how to invest in an AI world and operationally: it said early on that it "must rebuild the firm in an AI native way, must have a platform, not point solutions." Where did that come from?

McDonald pointed to culture. Apollo prides itself on being "the leader of the alternative asset management space" and on being "very innovative." Management stresses building the team's skills and staying nimble. "We appreciated that the AI world is here and we need to adapt and adopt it, not … discredit it," McDonald said, bringing new technology into workflows to free people to think and to replace manual processes. McDonald described Apollo as "a very innovative firm that's very flat and open culture." Then: "I'm blanking on the second part of your question. Sorry." Manapat moved on.

## 6. How early is finance?

Software engineering has changed completely as a discipline, Manapat said, and law and customer service may be close. Where is finance, between "AI is here, it's made a difference" and not yet past "the bottom of the first inning"?

"I still think it's very early," McDonald said, at least in asset management, investment banking and institutional finance. Large banks have extensive middle- and back-office operations with room to improve, and Apollo itself is "extremely complicated." Today's value lies in building models, getting up to speed on a company and aggregating research, but "that's lower value-add at least of where we stand today, and that's going to evolve," and some functions may need "less junior support over time." At Redding Ridge the focus is on making routine tasks efficient, which frees capacity "cuz we kind of have like endless things to do." The second half of Manapat's question, what it would take to change finance, did not get a separate answer.

## 7. Commodity or competitive edge?

Manapat laid out two possibilities. In one, AI speeds up tasks and saves operating expense, but gives no firm an edge, just as no firm gains an advantage from using Excel. In the other, AI "materially makes a competitive advantage or helps reify Apollo's lead in structuring and creative deals."

"If it's adapted and utilized properly, it will make you more competitive," McDonald said. For firms in actively traded markets, success comes down to process and information: knowing available liquidity, where things are pricing and what they are worth at any moment. That already exists in public markets; "in the private markets, it's evolving," and Apollo has been pushing it forward. Over time, firms with better information that their teams know is accurate will pull ahead, because "the last thing we want to do is … make a decision that was misinformed," with errors flowing through to the firm and its investors.

## 8. What has to be true next

Asking about Apollo in five years would be unfair, Manapat said: "I can't even predict if my team's going to use Codex or Claude Code next week." So Manapat asked what McDonald personally hoped to see.

McDonald named two things. First, full confidence in every piece of information that goes to investors, without hours of manual checking; Redding Ridge has "over 400 global investors." Second, a more "tech enabled … mindset." The team has not lacked it, but technology has been "an afterthought." With routine work automated, people can ask "why does this process exist and how does it work today and how can we actually enhance that," and whether to bring in new technology. Ideally, McDonald said, that becomes staff's first thought when tackling a problem at Redding Ridge.

## 9. Advice for people building AI for finance

Manapat's last question drew on what Manapat had learned working with Redding Ridge: how "unimaginably complex and creative the work is." What should people building AI for finance keep in mind?

"We are extremely complex organizations," McDonald said. Things that look "novel or strange and out of place" are "probably actually there for a reason." So "spending the time and really putting boots on the ground to sit with whatever team or front office professional that you're working to build a tool for … is time well spent." McDonald credited exactly that for Rowspace's product being "ahead of our roadmap and expectations": "Michael and the team has spent ample time sitting on the ground with our team like pretty much every single day."

Manapat closed with what Manapat called a shameless plug: both Rowspace and Apollo's technology group are hiring people who want to apply AI to finance.

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=44e9d04a-e8b4-418c-8a85-02fbb9263dd0) · [Video](https://www.youtube.com/watch?v=BOWYujhIQuo) ("AI Agents Cut 3-Day Portfolio Builds to Minutes") · [Day 2 livestream at 7:50:03](https://www.youtube.com/watch?v=jiUC6IUMdvc&t=28203s) · [Fortune on Rowspace's launch](https://fortune.com/2026/02/25/rowspace-sequoia-ai-financial-platform-private-equity-investment-venture-capital/) · [Redding Ridge recruiting page](https://job-boards.greenhouse.io/reddingridge/jobs/4994805008)

This write-up is based on the official video captions, checked against the Day 2 livestream captions. No slides were shown and the author took no photos. The conversation ended without audience questions. The [precise notes](precise/10-14-1630-ai-in-practice-an-inside-look-at-apollos.md) (Chinese) keep timestamps and caption corrections.
