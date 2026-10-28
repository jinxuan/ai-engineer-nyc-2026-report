**English** | [中文](zh/10-12-1530-unhobbling-the-frontier.md)

# Your App Is Probably Sitting Inside the Frontier

*At AI Engineer NYC 2026, Anthropic's Lance Martin argued that most Claude applications leave cost and quality on the table through small misconfigurations. Martin walked through five fixes in 23 minutes, then spent 48 minutes on questions about evals, hill-climbing and effort.*

**Workshop · Monday, October 12, 2026 · 3:30–4:30pm · Shannon Room (NY Ballroom E)**
**Speaker:** Lance Martin, Member of Technical Staff, Anthropic (previously at LangChain)

![Lance Martin on stage](../slides/10-12-1530-unhobbling-the-frontier/15.jpg)

---

Lance Martin opened with a single chart. On a plot of cost against performance, a curve marked the efficient configurations: the Pareto frontier. A red dot labeled "your application" sat well inside it, and a dashed arrow moved it left onto the curve, labeled "a 'free' win (lower cost, no performance drop)."

"Many times, you're not actually on this frontier," Martin said. "Your application is actually sitting inside this frontier that you could be on, and it's because of minor misconfigurations, which I'm going to show you today how to fix."

![Most applications sit inside the frontier](../slides/10-12-1530-unhobbling-the-frontier/01.jpg)
*"Most applications sit inside the frontier": moving onto the curve lowers cost without losing performance.*

That is the "unhobbling" of the title: the model is capable, and the configuration around it holds it back. Martin covered five themes, each paired with a command in Claude Code's `claude-api` skill: audit your prompts, use the prompt cache, design a good eval, hill-climb on it, and build harnesses for long-running work.

## 1. Prompts accumulate debt

"Prompts accumulate debt just like code," Martin said. Several model generations ago, developers added instructions such as "think step by step," "double-check your work" and "be maximally thorough," or gave the model scratchpads and templates for how to think. Frontier models no longer need them: "They naturally verify their work. They don't need to be [told to be] thoughtful." Keeping those instructions raises cost and can lower performance.

The slide sorted the debt into three kinds:

![Prompt instructions accumulate as debt](../slides/10-12-1530-unhobbling-the-frontier/02.jpg)
*Seven kinds of prompt debt, grouped by what they do to an application.*

| Effect | Pattern | What the slide says |
| --- | --- | --- |
| **Taxes cost** | Verification rituals | "verify twice before answering": measured **+48% cost per ticket, zero accuracy change** |
| | Thoroughness boosters | "be maximally thorough": unneeded tool calls per ticket went from **0.07 to 0.95**, old model vs new |
| | Procedure and scratchpad scaffolds | step-by-step templates stacked on top of the model's native thinking |
| | Worked-example stuffing | few-shot examples that teach long reasoning chains the model doesn't need |
| | Anti-laziness tool boosters | "always check the docs first": forced lookups with nothing to look up |
| | Dead weight | runbooks for features you no longer have, read and paid for on every request |
| **Breaks accuracy** | Contradictory rules | two rules added months apart; the model quotes both and freezes: **−9 accuracy points from one planted pair** |
| **Breaks outright** | Dated configuration | settings for an older API generation, rejected or silently reinterpreted |

Contradictory rules got the most attention. Older models were weaker at following instructions, so faced with two conflicting rules they would ignore one, and "it kind of still worked." Frontier models take both seriously: "they'll often get confused by contradicted rules." The better the model, the more a contradiction hurts.

![The two audit commands](../slides/10-12-1530-unhobbling-the-frontier/03.jpg)
*The same audit, reachable from two skills.*

The fix is one command. `/claude-api prompt-audit` checks code that calls the API; `/checkup prompt-audit` checks Claude Code's own configuration. Both call the same underlying skill, which scans prompts, skills and tool descriptions for these patterns and removes them. Martin said it is used widely inside Anthropic.

![Prompt audit improves performance and reduces cost](../slides/10-12-1530-unhobbling-the-frontier/04.jpg)
*A simulated migration on a 50-ticket customer-support eval.*

To show the effect, Martin built a 50-ticket customer-support eval, simulated a migration from Opus 4.8 to Opus 5.5, and injected each anti-pattern one at a time, averaging the results:

| Configuration | Cost per ticket | Accuracy |
| --- | --- | --- |
| Opus 4.8, legacy prompt | 2.45¢ | 88.1% |
| Opus 5.5, same prompt (model upgrade only) | 2.02¢ | 90.0% |
| Opus 5.5, after prompt-audit | 1.83¢ | 92.0% |

Upgrading the model alone helped; auditing the prompt moved it further up and to the left. The practical takeaway: every model migration is a good time to audit the prompt, not just change the model ID.

## 2. Use the prompt cache: it costs nothing in quality

Agents run for many turns, and each turn adds only a little, so most of the context repeats. "You don't want to pay for it twice." Cached tokens are billed at roughly a tenth of the base rate (Martin suggested checking the pricing page for exact figures) and are faster.

The difficulty is that the cache is a strict prefix match: "even a byte change in your prefix will invalidate the cache." Martin suggested ordering a prompt from what changes least to what changes most: system instructions and tools, cached globally across sessions; then project-level memory such as `CLAUDE.md`; then session state; then the messages that grow each turn. The Claude Console shows an application's cache hit rate, and the Claude Code team tracks its own "very, very religiously."

![cost-optimize can reduce cost by fixing prompt caching](../slides/10-12-1530-unhobbling-the-frontier/05.jpg)
*Running cost-optimize against internal benchmarks that used default harnesses.*

`/claude-api cost-optimize` checks cache usage (and also effort levels and prompts). Martin and colleagues ran it on internal benchmarks whose harnesses weren't caching properly:

| Benchmark | Cost change | Cost | Accuracy change (95% CI) |
| --- | --- | --- | --- |
| tau2-bench (retail), customer-service agents | −72.9% | $48.68 → $13.20 | +2.6pp (−2.1 to +7.3) |
| LegalBench, legal classification | −66.9% | $38.46 → $12.74 | −0.8pp (−1.3 to −0.3) |
| OfficeQA Pro, document Q&A | −71.7% | $252.14 → $71.26 | +1.5pp (−4.0 to +7.0) |
| SWE-bench Verified, agentic coding | −24.3% | $14.42 → $10.92 | +1.1pp (−2.6 to +4.8) |

Martin summarized it as "reduce costs up to 72%, but no meaningful change in performance," and called caching "just pure cost saving." The table mostly bears that out, with one nuance: LegalBench's accuracy change is small, but its whole confidence interval sits below zero.

## 3. Give Claude an eval it can climb

"Frontier Claude in particular is very good at self-correction," Martin said. "If you give Claude a well-structured evaluation, it can basically just hill climb in that eval." The condition is that the eval be climbable.

![The four elements of a good eval](../slides/10-12-1530-unhobbling-the-frontier/06.jpg)
*Score against tokens per attempt, for three model sizes at low, medium and high effort.*

The chart plots score against tokens spent for a frontier, a mid-size and a smaller model, each at low, medium and high effort. A good eval shows four things: (1) performance improves with a more capable model, (2) performance improves with more thinking, (3) there is headroom left below a perfect score, and (4) run-to-run variance is low. Martin's accompanying blog post adds a fifth that comes first: the tasks should mirror production, not just be easy to generate or grade.

The most common problem Martin sees is ambiguous tasks. An eval looks like it has 20% of headroom, "but no model can actually achieve it," so a team burns tokens climbing toward a ceiling that isn't there. The blog gives a test: a task that fails on every run, however many replicates, is probably ambiguous; a good task is one where two domain experts would reach the same verdict. Variance can also hide in configuration (effort not applied consistently) or in the environment (leftover files or git history from an earlier trial handing the agent the answer).

The command is `/claude-api build-eval`. It is "not opinionated at all about where your data lives" and works with any tracing setup: describe the problem, point it at raw data such as traces, and it will "show you the data, show you the evaluation, sanity check the grader [and] the criteria with you." Per the blog, it draws example inputs in this order of preference: production transcripts (after asking about retention and sensitive data), bug reports and support tickets, five to ten hand-written cases, and cases synthesized from the codebase. It then proposes the cheapest grader that fits: a code check (exact match, a fixed label set, a JSON schema, passing tests) when outputs are constrained, or an LLM judge with a rubric "written as checkable claims (not a 1-to-5 scale)" when they are open-ended. The judge should not be the model under test. Before trusting the grader it scores a few cases with you, runs it twice on the same output to check it gives the same verdict, checks for timeouts and cut-off answers, and warns if the baseline already scores around 95% or higher.

## 4. Hill-climb without fooling yourself

With a good eval, "frontier Claude models are very, very effective at hill climb … without you in the loop." The danger is overfitting: tuning the system to the eval's quirks rather than to production. "You run into production and performance is not better at all. That's frustrating because you spend a lot of money."

![Beware of overfitting](../slides/10-12-1530-unhobbling-the-frontier/07.jpg)
*How features of a benchmark shape a harness. The dashed arrow is an outright leak.*

The slide paired features of a benchmark with what they do to a harness:

| The benchmark has… | …so the harness gains |
| --- | --- |
| a task mix that needs OCR | an OCR tool |
| tasks that live in `/app` | "always cd /app, run pytest" |
| distinctive phrasings | a prompt tuned to those phrasings |
| failures you've read | one patch per failure |
| a public repo with answers | an agent that curls the reference solution (the outright leak) |

`/claude-api hillclimb` borrows a fix from classical statistics, a train/test split. Martin was careful to say it is "of course, not perfect. You have to audit this carefully."

![The hill-climbing loop](../slides/10-12-1530-unhobbling-the-frontier/08.jpg)
*One change per round, a held-out test split, and explicit revert rules.*

The loop on the slide edits one surface (for example, a prompt) while the model and harness stay fixed. Before starting, a pre-flight runs the baseline several times to set the noise floor. Each round, an analyzer reads failures from the train split only, forms a hypothesis, and proposes one change. Both splits are scored every round, and the decision follows three rules: if train and test both improve, keep the change; if train improves but test stays flat, treat it as overfitting and revert; if either gets worse, treat it as a regression and revert. "If you see train improving but test flat, it's a signal that you might be overfitting."

The blog adds two habits beyond the split: never paste failure content into the prompt, and keep answers structurally out of the model's reach, since models can "reward hack" by finding them. It also suggests where to aim: surfaces that are cheap to change, changes whose effect on the score can be attributed to them (skill descriptions and skill trigger rates are a good pair), and an objective with real headroom. When the score stalls for two or three rounds, the climber stops editing, sorts every remaining failure by cause, and often finds ambiguous cases or a broken grader. At the end it reports the test result with confidence intervals and recommends against merging if the gain is within noise.

### Case one: hill-climbing the skill itself

![claude-api skill hill-climbing](../slides/10-12-1530-unhobbling-the-frontier/09.jpg)
*The claude-api skill's pass rate over about two dozen rounds, in three phases.*

Martin hill-climbed the `claude-api` skill against an eval built from Anthropic's documentation, giving Claude Code access to the skill text and the docs. The pass rate went from 66.1% to 87.9% (68% on stage; the slide and blog say 66.1%):

| Phase | What changed | Pass rate |
| --- | --- | --- |
| 1 | Adding missing sections and type tables (eight features had no coverage; C# and Java type tables had errors) | 66.1% → about 77% |
| 2 | Fixing how the skill tells Claude to write code: after stalling, the climber found Claude wrote older API forms from its training, so it added an old-to-new lookup table at the top of the skill | → 80.1% |
| 3 | Fixing graders, plus more skill edits (tasks that never improved pointed to flawed examples or graders) | → 87.9% |

"Simple text-based surfaces are very, very good for this kind of process," Martin said.

### Case two: hill-climbing for cost

![Cost-focused hill-climbing](../slides/10-12-1530-unhobbling-the-frontier/10.jpg)
*The path the cost-focused climber adopted on the customer-support benchmark.*

Told to "reduce cost," the climber searches model, effort and prompt combinations that cut cost without falling below the baseline. The blog gives the numbers for the support benchmark (44 tickets: 30 for the search, 14 held out):

| Step | Configuration | Accuracy (search tickets) | Cost per ticket |
| --- | --- | --- | --- |
| 1 | Opus 4.8, high effort (baseline) | 74.4% | 4.6¢ |
| 2 | Opus 5.5, low effort, audited prompt | 87.8% | 1.9¢ |
| 3 | Sonnet 5, low effort | 88.9% | 1¢ |
| 4 | Sonnet 5, low effort, improved prompt (routing rules, refund-cap cross-reference) | 98.9% | 1¢ |

On the 14 held-out tickets, the final setup scored 90.5% against the original's 78.6%, at about a fifth of the cost. On stage Martin called it "a 90% reduction in cost"; the blog's figures work out closer to 78%. (The slide labels step 2 "Opus 5"; the blog says Opus 5.5.)

Two points stood out. Cost works as an objective even when an eval is saturated: "I don't care about performance, I just want to reduce cost." And a stronger model at lower effort often beats a weaker model working harder, "like giving someone who's smarter [permission to] try less," because it spends fewer tokens. That heuristic is now built into the skill.

## 5. Build harnesses for agents that run for days

The last theme looked ahead.

![Task duration over time](../slides/10-12-1530-unhobbling-the-frontier/11.jpg)
*Length of tasks Claude models can complete autonomously, measured in time a human would take.*

The chart, in the style of METR's time-horizon plots, shows three eras: autocomplete and chat, synchronous local agents, and asynchronous remote agents. The data points climb from Claude Sonnet 3.5 (21 minutes) to Sonnet 3.7 (1 hour), Opus 4 (1.7 hours), Opus 4.5 (4.9 hours) and Opus 4.6 (12 hours). Claude 3 Opus "was capable of only, like, four minutes of autonomous work," Martin said; now task durations are "exceeding, like, a day to two days," beyond what METR's method can easily measure. At that length, reliability and security matter in new ways.

![Managed Agents architecture](../slides/10-12-1530-unhobbling-the-frontier/12.jpg)
*The harness at the center, with session, sandbox, tools and orchestration as separate components.*

Claude Managed Agents is built on what Martin called "decoupling the brain from the hands," the title of an engineering post Martin co-wrote in April. The harness is the brain; sandboxes are the hands where code runs. "Imagine you have an agent that you spin up overnight … and the sandbox dies midway through the night. You lose state, you lose your session log, everything." So the session log is its own append-only object, separate from both. If the sandbox dies, the harness gets a tool-call error and a new sandbox is started; if the harness dies, a new one resumes from the log. Secrets stay in a vault outside the sandbox: Claude can use them but "never [has] direct access," which matters for an agent "working for, say, days on end."

![An agent automation built on Managed Agents](../slides/10-12-1530-unhobbling-the-frontier/13.jpg)
*The reference automation from Martin and CJ Avilla's blog post.*

Martin closed with an example from a post written with CJ Avilla. A schedule wakes the agent. It reads its sources read-only, through MCP or HTTPS, with credentials from the vault. Guardrails cap turns, time and spend, and a heartbeat lets someone notice if it stops. Memory is split in two: state the agent writes (a bookmark per source, what it has already said, a log of every run) and preferences the reader writes (scope, length, who gets notified, quiet days, when to stop). Before posting, it checks whether it already posted, then sends a brief to one destination approved in advance. In the published version it reads Slack channels and GitHub pull requests on weekday mornings and posts to a single Slack channel.

![The onboarding command](../slides/10-12-1530-unhobbling-the-frontier/14.jpg)
*Running this in Claude Code sets up the example.*

Running `/claude-api managed-agents-onboard` with the post's URL builds the example. Martin ended the talk with how colleagues at Anthropic now work: give an agent an eval, let it climb overnight, and "come back, for example, the morning, and it's significantly improved."

## Q&A

Questions ran for 48 minutes. They are grouped by topic below.

### Who builds the eval: humans or Claude?

**How automatable is eval design?** Martin cited feedback from Hamel Husain, who tried `build-eval` publicly on a customer-support app: the skill should show the user the raw data first. Martin agreed and had a pull request open to add a data viewer. Claude is good at scanning offline logs and flagging suspicious cases, "effectively like acting as an online evaluator but looking at your offline logs," but "there is still quite a role for the human in the loop."

**How will the split between humans and Claude change?** Humans steer: "What are you actually trying to evaluate? And why is it important? So it's kind of taste." Once that is clear, Claude reads raw traces and flags where the app falls short, and a human reviews those cases and turns them into an evaluator. "That's typically a very nice marriage between Claude and human."

**Who owns evals, product or engineering?** "At Anthropic, everyone is empowered to build evals," usually along product ownership. Skills like this let many more roles build evals, since Claude can handle the plumbing such as eval runners.

**Which eval or tracing tool should we use?** Martin, who worked at LangChain before Anthropic, declined to pick one. At small scale "you obviously can roll your own," and Claude can cheaply build a custom data viewer for a specific problem.

**How big must an eval set be?** Martin hadn't studied it closely: "frankly, just ask Claude," which has heuristics for sizing (the skill estimates cases × repeats × models up front). Small sets overfit more easily, are noisier and may not represent production.

**Single-turn or full-trajectory evals for agents?** Agents usually need multi-turn evals. `build-eval` can promote traces into the eval set; whether to grade the whole trajectory or only the final outcome is a choice, and the command includes guidance.

### Judging fuzzy outputs

**How do you evaluate products without clear success metrics, like UX?** Use a deterministic check if you can. Otherwise, for an LLM judge: write a rubric a human could grade with; "don't use arbitrary scales … grade it from zero to five, it's very ambiguous," and prefer binary judgments, splitting multiple criteria into several binary classifiers ("from one to ten, rate this, that's extremely noisy"); test across models ("Have Sonnet, Opus, and Fable all grade using your rubric. If they all disagree, that's a bad sign"); and run replicates to confirm low variance.

**What about pairwise comparisons and Elo?** "Pairwise comparisons can actually be quite good." It is far easier to say "I like this webpage better than this one" than to score one page out of five.

**Why insist on binary if models are smart enough for graded scales?** "I'm being a little bit overly prescriptive. You obviously can." A scale is fine if variance is low, results reproduce across models and a human can tell adjacent grades apart. Binary is a shortcut, and graders are often small, cheap models.

**An example from a less verifiable domain?** Evals for generative UI, using a pairwise judge to compare configuration A against B over many trials. LLM judges are also common for retrieval, writing and labeling traces.

### Hill-climbing in practice

**Can you hill-climb on human feedback?** Yes: "Hill climbing only requires that you have an eval and you have some signal that Claude can use to judge good versus bad." Turn the feedback into that signal.

**Which surface should you start with?** Text: prompts and skills are constrained and "highly coupled to the output." Harness code produces large diffs that may not connect to what is being measured. The catch is that prompts overfit easily, even with a train/test split: "Claude's very good at wiggling the prompt into a shape that does well in your eval that isn't really representative of what's in production."

**When should a prompt or SOP become a skill?** "I actually think you should just have Claude do that": go through the workflow, then ask Claude to turn it into a skill. For hill-climbing, Martin starts from a crude skill and lets Claude climb from there; the `claude-api` skill began as little more than a map of the docs.

**How do you think about the eval's distribution?** Look at the data and make sure the cases represent production. Avoid "adversarial sampling," building the eval only from cases one model fails: "models are jagged," so the next model may solve them all and the eval ends up shaped around one model's weak spots. Pick cases that are "hard or interesting from first principles."

**Is hill-climbing guaranteed to converge?** "It's not guaranteed to converge at all." When stuck, the climber clusters the remaining failures; often the last 20% are faulty grader examples or ambiguous instructions, "not because Claude can't do better, it's because the eval is poorly structured." That is what happened in phase 2 of the skill example.

**Is there a role for DSPy?** Martin respects the DSPy team and Omar Khattab. The skill is open source and is "just some very simple, general instructions for setting up the hill climbing loop," so it could likely work alongside DSPy, though Martin hasn't used it.

**What about budgets running out of hand?** The skill asks for a budget and tracks it. Runaway spending is another argument for good eval design: if the last 20% is impossible, "you burn a huge number of tokens" on it.

**Can you optimize several metrics at once?** Yes, the cost example optimized cost and performance together. Martin usually considers cost, performance and speed, "a pretty reasonable trifecta"; more objectives make the problem harder, so consider "changing one thing at once, doing it serially."

**What about tasks that take 14 hours to run?** That is why Martin favors fast surfaces like prompts. Long tasks can be climbed, but it needs a harness that stays reliable for many hours (internally, Managed Agents): "it just becomes a bigger infrastructure lift."

**Is this like backprop on a loss function?** Similar to training, down to the train/test split, but "these are kind of run-time modifications" made to prompts and skills after a model is deployed.

**How much does eval awareness matter?** Anthropic has published on it as a training concern, but here "the bigger concern is typically more overfitting": Claude finds prompt tricks that make the number go up without helping production, not because it knows it is being tested.

**How does this relate to Karpathy's autoresearch?** Much of the motivation comes from it: models self-correct well against a clear, verifiable objective. Autoresearch targets something like training loss, while this command is built for evals with a split, but the loop is the same. "If you had a nanoGPT repo, you could use this skill with it, I'm sure."

### Effort and prompt audits

**How do you choose among five effort levels?** With an eval, let the climber search model and effort together. The principle: "the stronger model at lower efforts is … more token efficient and therefore cheaper than a weaker model at high effort." The climber tries models at low effort first, until performance drops below the baseline; Haiku 4.5 at low effort regressed badly in Martin's test. Without an eval: "Opus 5.5, low effort. Try that." Asked "Low, not medium?", Martin said to start low and dial up; many people start at max, which is "extremely token hungry." (Anthropic's docs note that Opus 5.5 defaults to medium, so low is one step below default.)

**What does effort actually mean?** Roughly "how hard to think." A smarter model at low effort spends fewer thinking tokens from a higher baseline than a cheaper model at high effort.

**Is effort a hard cap?** "It is not a hard cap." Even at high effort a simple problem won't consume huge numbers of tokens. The difference shows on frontier-level problems, where xhigh or max can use far more tokens than low or medium for similar results "because Claude will do more work to verify the result." Anthropic's documentation says the same: effort is "a behavioral signal, not a strict token budget."

**Is the prompt audit model-specific?** These anti-patterns are definitely unnecessary for the 5.5 generation; the cutoff is fuzzy, perhaps around Opus 4.5. Migrating to Opus 5.5 is the time to run it. The skill includes some model-specific guidance; Haiku might still benefit from a few boosters, "I don't know if that's true."

### Reliability and keeping up

**What actually improves reliability in context engineering?** The idea from the Recursive Language Models work (Zhang, Kraska and Khattab): keep the context as an object separate from the harness, which queries it through tools. If context is just a list of messages, "you do a compaction event, and everything you compacted is gone." Managed Agents' session log works this way.

**Are we designing around how models were optimized?** An attendee quoted an Opus 4.8 agent: "I'm optimized to produce fluid, forward-moving, plausible output, and plausible is not verified." Martin's answer tied the session together. Many tasks aren't easily verifiable, so the eval must carry the weight: good objectives, clear grading, cross-model agreement, low variance. Give a model a poorly structured eval and "it won't go anywhere, and it's not the model's fault."

**How do teams keep up with models that change so fast?** As models improve, eval scores should rise; "if you don't see that, I would go back and question the eval structure." Writing evals that challenge Fable or Opus 5.5 is itself hard, "a recurring challenge."

**Is it worth optimizing the eval itself in an outer loop?** Martin described a pattern from tracing companies: deployed agents produce traces, an online evaluator scores them, low scorers move into an offline eval set, the system hill-climbs on that set, and passing cases become regression tests. "That's a very good pattern." The recording ends here.

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=6a4e7cfd-334a-4d3b-94ed-282acc905448) · [Automating eval design and hillclimbing with Claude](https://claude.dev/blog/automating-eval-design-and-hillclimbing/) (Lance Martin) · [Scaling Managed Agents: Decoupling the brain from the hands](https://www.anthropic.com/engineering/managed-agents) · [Building effective agent automations](https://claude.dev/blog/building-effective-agent-automations/) · [Effort documentation](https://platform.claude.com/docs/en/build-with-claude/effort) · [Recursive Language Models](https://arxiv.org/abs/2512.24601)

No recording of this workshop had been published at the time of writing. This write-up is based on the author's room recording, slide photos and the sources above. The [precise notes](precise/10-12-1530-unhobbling-the-frontier.md) (Chinese) keep timestamps and the full record of where the talk, slides and blog disagree.
