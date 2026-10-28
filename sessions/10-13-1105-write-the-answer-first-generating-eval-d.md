# No Benchmark and No Experts? Write the Answer First

*At AI Engineer NYC 2026, Air Canada's Nathanael Weill showed how a data science team without public benchmarks or easy access to domain experts built test sets for its agents by fixing the answer first and generating the input around it, so that plain code could do the grading.*

**Breakout (Evaluations track) · Tuesday, October 13, 2026 · 11:05–11:23am · Turing Room (Riverside Ballroom)**
**Speaker:** Nathanael Weill, Director, Data Science & AI, Air Canada

---

Nathanael Weill opened like a speaker at a support group. "My name is Nathanael, and I'm a non-evaluator," Weill said. "It's been a lot of time since the last agent I've been shipping without any evaluation. And today I want to tell you the story of how I quit this addiction of shipping an agent without any evaluation."

![Air Canada has lots of agents and no benchmarks](../slides/10-13-1105-write-the-answer-first-generating-eval-d/01.jpg)
*Air Canada's four business areas, each with chatbots, AI workflows and ML pipelines with agents inside.*

Weill joined Air Canada in June 2026. The company is large, spanning operations, corporate, commercial and loyalty, and Weill's team delivers AI solutions to all four: chatbots, AI workflows, and machine learning and optimization pipelines with agents inside. Two things make evaluating those agents hard. The tasks are so specific that "we don't have any public benchmark we can rely on." And, more importantly, "we are data scientists that can build agents … but we are not domain experts." The experts sit in other business units, are busy with their own work, and are hard to get time with.

The talk told two stories, from 2024 and 2026, each moving through the same stages marked across the top of the slides: Problem, Tried, Stuck, New method, Result.

## 1. 2024: a telecom RAG project

![I hit this problem once before, in 2024](../slides/10-13-1105-write-the-answer-first-generating-eval-d/02.jpg)
*The 2024 project: a Q&A assistant over telecom standards and vendor documents.*

Before Air Canada, Weill worked at another company on a Q&A assistant for telecommunications engineers. The slide set out the task: the documents were telecom standards and vendor documents, the task was to answer a technical question, and a good result was "the right answer, from the right page." The domain is large, with about 10,000 documents plus many vendor documents, and there is a logic to knowing which document answers which question. "2024, everybody wanted to build RAGs," Weill said. "On LinkedIn it's super easy. It's not." The hard part, in the slide's words: "We were data scientists, with limited access to the subject-matter experts who could grade our answers."

![Four teams, same challenge](../slides/10-13-1105-write-the-answer-first-generating-eval-d/03.jpg)
*Teams A, B and C didn't work; "Us" worked.*

Weill's team was competing with others on the same task, "two external, one internal. It's not like in parallel, but in segments." Only Weill's solution worked, meaning "actually delivering the value. The people liked the solution, actually used it." Weill was candid about why: "To be honest, I don't know why we actually made it work compared to the other teams." All had the same documents and the same task.

### Tried: adding parts every week

![Every week we added a part. Quality didn't move](../slides/10-13-1105-write-the-answer-first-generating-eval-d/05.jpg)
*Eight additions over seven weeks; answer quality "still flat."*

In RAG, a question goes to the system, which retrieves relevant documents and writes an answer from them. "If you found the right page or the right documents, 90% of the time you have actually the right answer," Weill said, so the challenge was retrieval. The team started with Question → Retrieve → Answer and, week by week, added parts: better parsing, a chunker, a re-ranker, HyPE (hypothetical prompt embeddings, which index generated questions for each chunk), a query rewriter, "reverse HyPE," and two reasoning steps. The answer-quality line stayed flat across seven weeks. The slide's footnote: "One try a week: that's how fast we could test. Every fix broke something else."

Techniques from papers "on paper … worked for them, … but for us it didn't really help," Weill said. "When we add complexity, we found that we break something else. Something that used to work now is not working anymore. And that's actually the real problem."

### Stuck: one try per week

![Each change waited a week for an expert](../slides/10-13-1105-write-the-answer-first-generating-eval-d/06.jpg)
*Changing the agent took minutes; grading it took the expert's one review slot a week.*

The slide laid out the bottleneck. The team could change the agent in minutes. A telecom expert reading its answers was "the only way to know if it helped." And experts had day jobs, so there was one review slot a week: "= 1 try per week," with a calendar showing only Wednesdays filled in.

Those reviews were weekly meetings. "We put a giant, giant screen with five different chatbots, ask the same question to all of them and ask the experts to grade them. I prefer this one, I prefer this one." The experts did not always agree, and sometimes didn't know the answer and had to look it up. "Our budget was we can try one solution a week, and this was not enough to go fast."

### New method: write the answer first

![So we wrote the answer first](../slides/10-13-1105-write-the-answer-first-generating-eval-d/08.jpg)
*Before: the expert grades the answer. Now: pick the page, then write the question.*

The fix reversed the direction of the test. Before: a question went to the agent, the agent answered, an expert read the answer, and a week passed. Now: pick a page, have an LLM write a question about it, let the agent search, and check whether it found that page. "Why it works," per the slide: "We know which page holds the answer before the test runs. Code checks it in seconds. No expert needed." The "right page" is the passage of the documents that holds the answer.

"There is nothing new here," Weill said. "We reverse the problem. … For those that work in RAGs, there is plenty of implementation of this thing. I didn't invent it. I just found it very, very smart, and it stuck to me. Like, why can't this principle be applied to other tasks?"

### Result: dozens of tries a day

![Then we could test every change, every day](../slides/10-13-1105-write-the-answer-first-generating-eval-d/09.jpg)
*From one try per week to dozens per day, and a rising quality line.*

"We went from one evaluation a week to dozens a day," Weill said, and the experts' grades of accuracy began rising week after week. The slide's rule: "Every change got a score. Keep it if the score goes up, drop it if it goes down." Its footnote added that arguments in meetings now ended the same way: "let's just run it."

Much of the gain came from taking things out. "Removing a lot of complexity helped. We didn't need a lot of those." What mattered was "the right embedding model, the right chunking, the right alpha in the BM25."

![Why the difference? I treat it as an axiom](../slides/10-13-1105-write-the-answer-first-generating-eval-d/10.jpg)
*The other three teams had no test set; Weill's team had one.*

Weill still didn't know for certain why the team succeeded, "but I'm pretty sure the other teams didn't have any test set. So they were using this kind of long loop with experts," perhaps with more expert access than Weill's team had. The slide stated the lesson as an axiom: "If you can't tell whether a change helped, you can't make the agent better." It defined the term: "An axiom: you can't prove it, but every time you apply it, it holds." The lesson "stuck to me until this day; it has been two years now," Weill said.

## 2. 2026: reviewing supplier contracts at Air Canada

![Now, in 2026 at Air Canada: a different challenge](../slides/10-13-1105-write-the-answer-first-generating-eval-d/11.jpg)
*The contract agent's flow, what carries over from 2024, and what's new.*

The second story was a current Air Canada project, presented on the slide as "inspired by a true project. Names and details changed. Numbers are real." Air Canada has many suppliers and many contracts. Experts go through each one clause by clause and give feedback: this clause is not compliant, this one needs to be rewritten. It is slow and costly both ways. If a supplier is needed now and the contract is late, "it causes a lot of delay in the chain. And if we miss a clause, it's also very costly because at the end of the day it can backfire to us." The business asked the data science team for an agent to automate the work.

In the agent's flow, a supplier contract goes in, the agent checks each section against Air Canada's rules, it produces suggested changes, each with a reason, and a reviewer accepts or rejects each one. The slide listed what was the same as in 2024 (lots of documents, busy experts, no benchmark; "you can imagine the price of a lawyer by the hour," Weill said) and what was new: the agent spots problems rather than answering questions; each problem needs the rule that proves it; and the agent writes a fix or leaves the clause alone.

![So we tried the RAG fix again](../slides/10-13-1105-write-the-answer-first-generating-eval-d/12.jpg)
*Where we were (flat) and where we want to be (rising): "How can we test every change when the agent's job is spotting problems, not finding pages?"*

Applying the 2024 trick directly kept the team "on the flat line." The search test (we know which page holds the rule; does the agent fetch it?) did not cover the agent's actual job.

![We were stuck because each job needs its own test](../slides/10-13-1105-write-the-answer-first-generating-eval-d/14.jpg)
*The contract agent's four steps; only Find is covered by the search test.*

"It's a bit simplified, but actually the agent has four steps," Weill said:

| Step | Question | Test coverage |
| --- | --- | --- |
| Spot | Is there a problem in this clause? | needs a new test |
| Find | Which rule or past contract proves it? | the search test covers it |
| Write | Draft the change | needs a new test |
| Decide | Flag it, or leave it alone | needs a new test |

Spot, for example, first classifies every section of the contract into a domain and then looks for a defect or a needed change. Finding the supporting documents was the 2024 problem, already solved. The other three steps had nothing.

## 3. Five ways to build a test set without an expert

![Working the steps: five ways, no expert needed](../slides/10-13-1105-write-the-answer-first-generating-eval-d/15.jpg)
*Five techniques, each with what it starts from, and the roles: you, a test writer AI, the agent, and code.*

Weill presented five techniques the team used or tried. "My goal is not to show off. My goal is to get you inspired in how to reapply those techniques in your own challenges." Each slide showed who does each step: you (the human), the test writer (an AI that writes tests), the agent under test, and code.

| # | Technique | Starts from |
| --- | --- | --- |
| 1 | Use past decisions as the answers | a decision someone already made |
| 2 | Choose the answer, then write the contract | a rule |
| 3 | Take a good contract and break it | a good contract |
| 4 | Check how the answer moves | no answer at all |
| 5 | Turn one example into many | one example |

### Technique 1: past decisions as answers

![1. Use past decisions as the answers](../slides/10-13-1105-write-the-answer-first-generating-eval-d/16.jpg)
*Diff the contract as received against the contract as signed; the agent should flag the same change.*

The idea: take a contract as received (cap = $500K) and as signed (cap = $2M), diff them in code to get the answer ("raise the cap"), and check whether the agent says the same thing by exact match. The slide's "What we found": "we asked for these pairs. None arrived." Teams keep only the final version, not the original, and contracts go through negotiation, Weill said. "So this thing doesn't work."

### Technique 2: choose the answer, then write the contract

![2. Choose the answer, then write the contract](../slides/10-13-1105-write-the-answer-first-generating-eval-d/17.jpg)
*Take a rule, pick a violating value, generate a contract around it, and see if the agent flags that section.*

Start from a rule, for example a cap of at least $1 million that every Air Canada contract should meet. Pick the answer (a cap of $500K, which should be flagged), have the test writer generate a contract around it, and check by exact match whether the agent flags that section (§8 on the slide). The catch is that you need to know the rules. Some came from experts or existing documentation, but that "cannot guarantee the coverage of everything," so the team also wrote its own. The result: "43 rules → 275 answers, known before any test ran." As with the RAG project, "we planted the problem, and we try to find it back," Weill said.

### Technique 3: break a good contract

![3. Take a good contract and break it](../slides/10-13-1105-write-the-answer-first-generating-eval-d/18.jpg)
*Delete one clause, record which, and mix the broken copies with clean ones.*

Take a real, signed contract, have the test writer delete one clause, record in code what is missing (for example, the breach clause), and mix the broken copies with clean ones that should raise no flags. Code checks by the planted ID. This is hard for the agent because suppliers use different templates: "Knowing what should be in what contract requires a bit of thinking." The finding: "deleted sections were noticed 1 time in 3." Weill said the agents were missing most of the removed clauses, "so that's a very, very strong benchmark," and called it "the most powerful approach that we had." (Earlier, introducing the list, Weill had called technique 5 "the most powerful.")

### Technique 4: check how the answer moves

![4. No right answer? Check how the answer moves](../slides/10-13-1105-write-the-answer-first-generating-eval-d/19.jpg)
*Edits that shouldn't matter should produce the same flags.*

When there is no right answer, test consistency instead. The test writer renames a party, reorders the clauses, or simply runs the same contract again; code compares the agent's two answers, which should have the same flags. Changing the supplier's name or address "should not have any impact on the number of flags." The slide's finding: "same contract, 5 runs: 1 in 3 judgments changed." All of it is checked by code. "You don't need to do any LLM as a judge. The previous speaker showed it's a very hard task. So we stay away from LLM as a judge as much as possible," Weill said.

### Technique 5: turn one example into many

![5. Turn one example into many](../slides/10-13-1105-write-the-answer-first-generating-eval-d/20.jpg)
*Turn an example into a template, fill the blanks many ways, and let the check read the blanks.*

Experts already work from contract templates with blanks. Start from one example (notice: 30 days; pay: net 60), turn it into a template (notice: [N]; pay: [terms]), and have the test writer fill it many ways (15 days and net 30, 90 days and net 45, 45 days and net 90). A code check reads the blanks, for example "notice < 60d → flag." "We generate an array for each of those blanks," Weill said. "Some tuples break some rules, some tuples don't. We know that." No LLM is involved, and "our experts already use templates, so we can reuse them." The finding: "more varied contracts, checked the way reviewers work."

## 4. From four weeks to five minutes

![From four weeks to five minutes](../slides/10-13-1105-write-the-answer-first-generating-eval-d/22.jpg)
*Before: beta feedback after four weeks. Now: a scripted run in five minutes. Below, the cheapest check first.*

Before, beta feedback took four weeks and "came back after the code had already moved on." Now, "a scripted run. Code matches each finding to a planted problem," in five minutes. A ladder along the bottom ordered the checks from cheapest: exact match, code, AI judge, human. The footnote: because the team wrote the answer first, "the check is just code. No AI judge needed for any of the five."

Weill dwelt on that point. Exact match and code are fast. "As soon as we step into LLM as a judge … it can be very fancy, it can be very useful. It's so hard. We got so much noise from the LLM as a judge compared to the noise with the agent that we're building. Solving this problem is actually almost as hard as solving the right problem." Fact checking and other techniques exist, "but actually it requires so much work, and we could not find the right one for this project."

## 5. The same trick beyond contracts

![The same trick works far beyond contracts](../slides/10-13-1105-write-the-answer-first-generating-eval-d/23.jpg)
*Write the answer, generate the input, run and check: for trajectories, text-to-SQL and your own agent.*

The slide applied the three-step pattern (write the answer, generate the input, run and check) to other kinds of agent:

| Agent | 1. Write the answer | 2. Generate the input | 3. Run and check |
| --- | --- | --- | --- |
| Trajectory | Tool calls, in order: `get_weather` → `book_court` → `send_invite` | Write the scenario that needs those calls | An LLM plays the user; code checks the order |
| Text to SQL | A slice of data, pulled by a query you wrote | Generate the question in plain text | Run the agent's query; compare rows |
| Your agent | Pick the answer | Generate the input that leads to it | Code checks it |

Another Air Canada project requires an agent to hit endpoints in a certain order; the team knows the expected order, generates a story that needs it, has an LLM play the user, and checks the trace. The pattern "doesn't apply to everything," Weill said. "Sometimes we had to re-engineer our solution to fit this paradigm, even if we move away slightly from the original design. With the original design we could not evaluate it. We switched to something that we can actually evaluate."

![Real feedback makes the tests better over time](../slides/10-13-1105-write-the-answer-first-generating-eval-d/24.jpg)
*Generated tests, agent, real feedback, new real tests: "One loop at a time." An Arize logo sits in the corner.*

Generated tests are not the end. The loop on the slide runs from generated tests to the agent, to real feedback, to new real tests and back. Experts' feedback is not used to fix the agent directly, Weill said: "We use this feedback to generate better tests. Better prompts. Better ideas on how to remove clauses." "That can be done by Arize. There's a lot of tools there. Thanks for the invitation."

## 6. Conclusion

![No benchmark? Write the answer first](../slides/10-13-1105-write-the-answer-first-generating-eval-d/25.jpg)
*The two takeaways.*

The conclusion slide had two points. First: "Write the answer first. Then the check is just code, and you can test every change." Second: "Generated tests are a starting point, not a ship decision. A person still checks every change. Real feedback takes over." Generated tests are "not enough to know if you can ship the product," Weill said. "It's not the go/no-go for the solution."

![Are you ready to quit delivering agents without evaluation too?](../slides/10-13-1105-write-the-answer-first-generating-eval-d/26.jpg)
*The closing slide, returning to the support-group framing.*

The final slide returned to the opening joke: "The 24-hour chip is the hardest one. Pick one agent. Write ten answers before you write the tests. Run them tomorrow." It was signed "720 days. Keep coming back. — Nathanael Weill." Weill offered to help anyone who wanted "to quit releasing agents without evaluation."

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=c4ba0138-6f6d-4780-93a1-4d8f0c478339) · [Video: How to Test AI Agents When No Benchmark Exists](https://www.youtube.com/watch?v=c_SPW__jF9w) · [RAGAS paper](https://arxiv.org/abs/2309.15217) (the schedule abstract's reference point for RAG evaluation) · [Hypothetical Prompt Embeddings (HyPE) paper](https://arxiv.org/pdf/2607.29402) · [Nathanael Weill on Medium](https://medium.com/@naweill)

The session abstract framed the talk against RAG evaluation, where RAGAS's faithfulness, context relevance and answer relevance metrics turned months of work into a ready playbook, and promised something that "won't be RAGAS for agents, but it will be enough to get started on Monday." The two stories used different feedback cycles (one expert review a week in 2024, four weeks for beta feedback in 2026). The video ends with Weill's thanks and no Q&A. This write-up is based on the official video captions, the author's slide photos and the sources above (the author's room recording was not usable). The [precise notes](precise/10-13-1105-write-the-answer-first-generating-eval-d.md) (Chinese) keep timestamps and a full record of where the sources disagree.
