**English** | [中文](zh/10-14-1455-build-your-own-team-of-managed-agents.md)

# One API Call Gets an Agent Its Own Computer

*At AI Engineer NYC 2026, Google DeepMind's Jack Wotherspoon showed how Gemini Managed Agents run a full agent loop in a cloud sandbox from a single API call, then chained three financial agents through one shared sandbox to research, value and write up a stock.*

**Expo Session · Wednesday, October 14, 2026 · 2:55–3:13pm · Shannon Room (NY Ballroom E)**
**Speaker:** Jack Wotherspoon, Member of Technical Staff (DevEx engineer), Google DeepMind

![The Shannon Room as the talk began](../slides/10-14-1455-build-your-own-team-of-managed-agents/01.jpg)

---

"I am a DevEx engineer at Google DeepMind," Jack Wotherspoon told the audience, and opened by asking the room who had heard of or used cloud or managed agents. The idea, Wotherspoon said, is simple: "instead of running a harness and running coding agent on your own machine, you can actually just run them in the cloud on a sandbox."

The 18-minute expo session made the case for doing that, walked through the building blocks of Google's version, Managed Agents in the Gemini API, and ended with three live demos built around financial analysis.

## 1. Why an agent needs its own computer

![With a sandbox, agents verify their own work](../slides/10-14-1455-build-your-own-team-of-managed-agents/02.jpg)
*Slide 2 of 19: the four-step loop an agent runs when it has a computer of its own.*

The first reason is verification. The slide, headed "With a sandbox, agents verify their own work," said that "instead of handing you untested code, an agent with its own computer runs the script, inspects the traceback, fixes the bug, and returns the verified output." It laid out four steps:

| Step | What happens | Example on the slide |
| --- | --- | --- |
| 01 Write code | Writes Python code to parse filings and compute cash flows | `write dcf.py` |
| 02 Execute code | Runs the script inside the Linux container and captures stderr | `python3 dcf.py` |
| 03 Test and validate | Spots a missing column or NaN row, patches the code, and re-runs | `fix KeyError → retry` |
| 04 Deliver artifact | Verifies the chart rendered cleanly before finishing the turn | `chart.png ✓` |

A footer read "Autonomous verification loop runs inside the cloud sandbox · Zero human prompts required." Running that loop on a laptop has side effects, Wotherspoon said: "sometimes it steals the browser. It starts to run things for you. You start getting pop-ups."

![Your laptop doesn't scale](../slides/10-14-1455-build-your-own-team-of-managed-agents/03.jpg)
*Slide 3: running locally means approving every command or exposing your machine.*

The second reason is that "your laptop doesn't scale." The slide contrasted the two settings. On a laptop, the agent is "blocked on permission prompts" (an `Allow bash execution & network access? [y/N]` prompt), shares your personal files, SSH keys and active sessions, stops when the lid closes, and can't scale to thousands of customer sessions. In a cloud sandbox it "runs & verifies autonomously": an isolated Linux container with "zero local blast radius," running asynchronously in the background, scaling "from your daily workflow to thousands of users."

Wotherspoon put it in terms of sharing: a developer who has tuned skills and a personal coding agent has no easy way to hand that setup to a colleague or build it into an app, and "you don't want your app that you're deploying to customers to be running with your own kind of subscription." Building the alternative by hand is tedious: spin up a container or VM, load it, manage it, do your own harness engineering, "and then let's not even start to talk about networking or setting up firewall rules or all the security stuff."

## 2. A single call to the Interactions API

![Now it's a single API call](../slides/10-14-1455-build-your-own-team-of-managed-agents/04.jpg)
*Slide 5: the whole setup in one `interactions.create` call.*

"Whether it's Gemini managed agents, Claude managed agents," Wotherspoon said, the product category "boils down to just being a single API call," and the talk would cover Google's offering in the Gemini API. The slide said the Interactions API "gives you a Google-hosted Linux sandbox, a persistent filesystem, and the Antigravity agent loop out of the box," and showed the call:

```python
from google import genai
client = genai.Client()
interaction = client.interactions.create(
    agent="antigravity-preview-09-2026",
    environment="remote",
    input="Pull Q3 earnings, model cash flow in Python, "
          "and export a chart to valuation.png.",
)
print(interaction.output_text)
```

Wotherspoon described the Interactions API as "our kind of new agent first version of our SDKs and client libraries." The older model interface was one input, one output, which "didn't really scale to agents"; an interaction can span many steps, write files and call MCP tools. The API "went GA earlier this year" and is "now our kind of home for managed agents." Two things change from an ordinary model call: you name an agent (here the base Antigravity agent) instead of a model, and you set `environment="remote"`, which tells it "we want a managed agent that runs in the cloud." Google's documentation adds that `environment` can also take the ID of an existing environment, to reuse its files and state, or a full configuration object with custom sources and network rules.

## 3. The architecture and its building blocks

![Gemini Managed Agents architecture](../slides/10-14-1455-build-your-own-team-of-managed-agents/05.jpg)
*Slide 6: the user's request goes to the Gemini API, which drives a sandbox.*

The architecture slide traced a request ("Analyze Q3 filings and create a valuation report…") from the user to the Gemini API, which holds the tools (`code_execution`, filesystem, skills, MCP tools) and the model, labeled Gemini 4 Argon. The API sends execute calls to the sandbox and gets results back. Inside the sandbox, the root filesystem holds `AGENTS.md`, `skills/` and `mcp_tools`, and a terminal panel reads "Isolated Linux VM · 4 vCPU · 16 GB" and "Egress proxy injects credentials." Four inputs feed the sandbox from above: GCS, a repository, inline content and credentials. (The slide's footer gives the machine as Debian, 4 vCPU and a 16 GB VM; Google's Antigravity agent documentation does not list these specs, so they rest on the slide alone.)

Wotherspoon said the API has built-in server-side tools, such as Google Search and filesystem tools for reading and writing files, and accepts custom functions and MCP tools. The sandbox runs "on kind of Google Cloud infrastructure," and can be loaded at startup with agent skills, an `agents.md` file or a GitHub repository.

![Building blocks of managed agents](../slides/10-14-1455-build-your-own-team-of-managed-agents/06.jpg)
*Slide 7: six building blocks, each with the API parameter that controls it.*

The building-blocks slide listed six pieces:

| Block | Parameter | What it does (slide wording) |
| --- | --- | --- |
| Harness | `agent_config` | "Plans the work, calls tools, and checks its own results." |
| Environment | `environment` | "Its own Linux computer with files and packages that persist." |
| Tools | `tools + mcp` | "Built-in bash, files, web search, and remote MCP servers." |
| Skills | `.agents/` | "Instructions in AGENTS.md and repeatable workflows in SKILL.md." |
| Credentials | `credentials` | "API keys injected outside the sandbox so secrets stay safe." |
| Schedule | `triggers` | "Cron schedules and background runs that work while you're away." |

On stage Wotherspoon spoke of five pillars (harness, environment, skills, credentials, triggers), and the video's chapter title lists the same five; the slide adds Tools as a sixth. The harness is the Antigravity harness, "the exact same harness that's running in our anti-gravity suite of products over at Google." The environment "is kind of its own entity," so several agents can point at the same sandbox and build on each other's files. Skills are like a company's "playbooks." Credentials are "a new piece of managed agent" that injects tokens or MCP OAuth into the sandbox. Triggers schedule runs; Wotherspoon's example was financial analysis "8:30 a.m. every morning before the market opens," with results sent by webhook to Slack, or an agent that "queries your team's Jira board every morning."

## 4. The harness and the models behind it

![The Antigravity harness runs the loop](../slides/10-14-1455-build-your-own-team-of-managed-agents/07.jpg)
*Slide 8: the model is one parameter in `agent_config`.*

The harness slide said it is "built on the same harness as Antigravity products. It plans multi-step work, executes tools, verifies its own output, and lets you configure the underlying model." The model is set in one line, `agent_config={"type": "antigravity", "model": "gemini-4-argon"}`, with the example input "Audit all 10-K footnotes and reconcile segment revenue across 5 years." Two models were offered: `gemini-4-argon` for "deep reasoning for complex coding and financial modeling," and `gemini-3.8-flash`, a "fast, low-latency default for everyday agent workflows." Wotherspoon framed the choice as Flash for speed and efficiency, Argon for deep reasoning or heavy analysis. Google's documentation confirms Flash is the default, so Argon must be chosen explicitly, and says a managed agent's model is fixed when the agent is created.

"Yesterday we just launched Gemini 4 Argon which is our new frontier model," Wotherspoon said. "It's especially good at financial tasks," as well as data analysis across many documents and multimodal work. The next slide (slide 9; the author's photo of it is not in the set) made three claims for Argon:

| Claim | Slide text |
| --- | --- |
| #1 on the Vals Index | "Leads frontier models across finance, coding, legal, and tax work, lifting agentic code submissions at Google by 35%." |
| Long decode up to 1M tokens | "Shared 1M context window (input + thinking + output) with tunable thinking_level (low, medium, high)." |
| Pair with 3.8 Flash for cost and speed | "Route your hardest reasoning to Argon and fast everyday turns to Gemini 3.8 Flash." |

Press coverage reports Argon at the top of the Vals Index; no source was found for the 35% figure, which appears only on the slide. Wotherspoon explained long decode: "normally models have an output token max of around 64K tokens," while with Gemini 4 "the 1 million kind of context window or token limit is now shared between input, thinking, and output," so for math reasoning or financial modeling "where the output might be pages and pages and pages long, the model can actually go and do that."

## 5. Conversations and sandboxes are separate state

![Interactions and Environments are stateful](../slides/10-14-1455-build-your-own-team-of-managed-agents/08.jpg)
*Slide 10: one agent writes to `/workspace`; a second picks up the same machine with a fresh conversation.*

Wotherspoon called interactions and environments "two different stateful things on our back end." An interaction holds the conversation (messages and reasoning turns); passing one interaction's ID as `previous_interaction_id` to the next makes a second turn. An environment is the sandbox's filesystem, the `/workspace` files, data and installed packages, and files can be uploaded or downloaded through the API.

Keeping them apart enables multi-agent work. In the slide's code, a `market-researcher` agent saves Q3 filings to `/workspace/q3.csv` in a new remote environment; a `quant-analyst` agent is then started with `environment=step1.environment_id` and asked to plot a valuation chart from the same file. The second agent "has a very fresh kind of context window and conversation, but it's reusing the environment," Wotherspoon said. "Kind of a multi-agent workflow."

## 6. Tools, playbooks and saved agents

![Built-in sandbox tools plus remote MCP](../slides/10-14-1455-build-your-own-team-of-managed-agents/10.jpg)
*Slide 11: four built-in tools, and a GitHub MCP server attached in a few lines.*

Every managed agent "starts with shell, filesystem, and web grounding enabled," the tools slide said:

| Tool | What it does |
| --- | --- |
| `code_execution` | Run Bash, Python and Node with stdout/stderr capture |
| `filesystem` | Read, write, edit and search files across `/workspace` |
| `google_search` | Live web search and `url_context` page fetching |
| `mcp_server` | Attach external Model Context Protocol servers over HTTP |

The code example attached GitHub's MCP server (`https://api.githubcopilot.com/mcp`) using a stored credential named `github-prod`, and asked the agent to open a tracking issue with DCF findings from `/workspace`. Remote MCP lets users "hook up to your favorite services," Wotherspoon said.

![Configure agents with your playbooks](../slides/10-14-1455-build-your-own-team-of-managed-agents/11.jpg)
*Slide 12: mount instructions, skills and data into the sandbox, then save the setup as an agent.*

The skills slide said you can "mount AGENTS.md, SKILL.md, Git repos, or Cloud Storage buckets directly into the sandbox — then promote your setup to a saved agent." Its file tree showed `.agents/AGENTS.md` (persona and rules), `.agents/hooks.json` (guardrails), `.agents/skills/dcf-model/SKILL.md`, and `/workspace/filings/` mounted from Git or GCS. The code put two sources into the environment: an inline `AGENTS.md` reading "You are a rigorous equity analyst," and a repository (`https://github.com/org/fin-skills`) mounted at `.agents/skills`. Three tabs above the code named the workflow: customize inline, save the agent, invoke it by ID.

The playbooks your agents follow on a personal machine can run the same way in the cloud, Wotherspoon said, without the risk of "overriding any files or messing up things on your machine." Once a workflow works, `agents.create` saves it, and "you can just easily invoke your personal agent by calling it with its agent ID."

## 7. Secrets never enter the sandbox

![Secrets never enter the sandbox](../slides/10-14-1455-build-your-own-team-of-managed-agents/12.jpg)
*Slide 13: an egress proxy attaches the token on the way out.*

The credentials slide said: "Store tokens and credentials once via client.credentials.create(). When the agent calls an allowlisted domain, the egress proxy injects the secret on the wire." Its diagram had three boxes. The agent sandbox calls `api.polygon.io` or `github.com` "with zero tokens in memory or env"; the egress proxy "verifies network.allowlist, attaches bearer_token or refreshes oauth2"; the external API "receives authenticated request; sandbox can never exfiltrate the key." The code stored a bearer token as `polygon-prod` and allowlisted `api.polygon.io` with that credential.

Wotherspoon described the proxy as a layer between sandbox and API that "puts a placeholder kind of dummy token into the sandbox so that the agent can never kind of go rogue and say, 'Oh, I see that I have this GitHub token laying around. Let me just see what else is on there.'" When the agent calls the GitHub API, the proxy matches the domain and injects the real token. (The spoken account mentions a placeholder token in the sandbox; the slide says "zero tokens." Both agree the real secret stays outside.)

## 8. Schedules, background runs and a team of agents

![Run on a schedule or fan out in the background](../slides/10-14-1455-build-your-own-team-of-managed-agents/13.jpg)
*Slide 14: a weekday cron trigger, and a non-blocking background job.*

The schedule slide showed two patterns. A trigger created with `client.triggers.create()` named `pre-market-briefing` runs the `quant-analyst` agent on cron `0 7 * * 1-5` in `America/New_York` (7:00am on weekdays; the spoken example was 8:30am) with the input "Refresh morning risk report," reusing a shared environment. A background job, `background=True`, asks the agent to "Run 10,000 Monte Carlo paths on NVDA" and is polled with `client.interactions.get(id=job.id)`. Wotherspoon suggested uses such as a daily briefing, scanning email and suggesting replies, or checking the market before it opens.

![One prompt, three specialized financial sandboxes](../slides/10-14-1455-build-your-own-team-of-managed-agents/14.jpg)
*Slide 15: a coordinator fans the work out to three specialist agents.*

Because a managed agent is just an API call, one agent can call others and fan work out. The team-architecture slide showed a coordinator, "Portfolio Lead," that "splits ticker analysis across three specialized managed agents in parallel," each saved with its own `AGENTS.md` persona, `SKILL.md` workflows and Python packages:

| Agent | Job | Output |
| --- | --- | --- |
| `sec-filing-researcher` | Extracts 10-K and 10-Q segment tables | `segments.json` |
| `quant-dcf-modeler` | Runs Monte Carlo and sensitivity analysis in Python | `valuation.png` |
| `exec-report-writer` | Synthesizes thesis and risk memo | `memo.pdf` |

The outputs go through the Files API into a "Verified Briefing" of "audited numbers, charts, and PDF ready for decision-makers." Wotherspoon argued for "niche and specific targeted agents that have one core purpose" over "a super super broad … task to an agent" with "no way of kind of measuring what it's actually doing."

## 9. Three demos

![Let's put it to the test](../slides/10-14-1455-build-your-own-team-of-managed-agents/15.jpg)
*Slide 16: the live build, with the researcher's output for NVIDIA streaming in.*

**Research, model, report.** The live build ran three agents in sequence against one shared sandbox for the ticker NVDA, under the line "Re-use environments to allow agents to pick up where others left off." Step 1, `sec-filing-researcher`, pulls quarterly revenue and segments and writes `/workspace/metrics.json`. Step 2, `quant-dcf-modeler`, reuses the environment ID, runs a Python DCF on that file and saves `valuation.md`. Step 3, `exec-report-writer`, reads both files and saves `investment_memo.md`.

The researcher's output on screen gave the fiscal quarter as Q2 FY2027 (period ended 2026-07-26, filed 2026-08-26, from Form 10-Q / Form 8-K), revenue of $96,221 million ($96.22B) against $46,743 million a year earlier, and year-over-year growth of 105.85%. These are the agent's demo output and were not checked against NVIDIA's filings.

![The valuation sensitivity table](../slides/10-14-1455-build-your-own-team-of-managed-agents/16.jpg)
*The quant agent's `valuation.md`: implied share price and upside across discount rates and growth rates.*

The quant agent's `valuation.md` (4,826 bytes) held a "Valuation Sensitivity Analysis." Its implied share price matrix crossed the discount rate (WACC) with the perpetual terminal growth rate:

| WACC \ growth | 2.5% | 3.0% | 3.5% | 4.0% | 4.5% |
| --- | --- | --- | --- | --- | --- |
| 8.0% | $287.48 | $311.49 | $340.84 | $377.52 | $424.69 |
| 8.5% | $262.89 | $282.53 | $306.11 | $334.92 | $370.93 |
| 9.0% | $242.10 | $258.42 | $277.70 | $300.85 | $329.13 |
| 9.5% | $224.29 | $238.02 | $254.04 | $272.98 | $295.70 |
| 10.0% | $208.86 | $220.55 | $234.03 | $249.77 | $268.36 |
| 10.5% | $195.37 | $205.41 | $216.89 | $230.13 | $245.58 |

A second matrix gave the implied upside, from −0.90% (9.5% WACC, 2.5% growth) to +87.65% (8.0% WACC, 4.5% growth) in the visible rows. Wotherspoon pointed out that every step is saved as a step trace (function results, file writes, code execution) that can be followed and optimized, and that the quant agent "can pick up its task and its job from that metrics.json" because it shares the sandbox, "doing a forecast for the share price."

![The report writer at work](../slides/10-14-1455-build-your-own-team-of-managed-agents/17.jpg)
*Step 3 running: the report writer reads `valuation.md` and `dcf_model.py` from the shared workspace.*

The third agent turned the "research and knowledge dump" into a report; its trace showed it viewing `/workspace/valuation.md`, listing `/workspace` and reading `dcf_model.py`. With MCP servers attached, Wotherspoon said, it could write to a Google Doc or post to Slack.

![A finance story built from Google Drive data](../slides/10-14-1455-build-your-own-team-of-managed-agents/18.jpg)
*Second demo: an interactive report built from a company's receipts in Google Drive.*

**A report from Google Drive.** The second demo showed a managed agent built into an app and connected to a Google Drive "where we dumped in a bunch of receipts of our annual kind of company data." A skill told it how to build an interactive report. The result on screen, "Nimbus Cloud SaaS · FY 2025 · Finance Story," was on panel 8 of 12, "Treasury Pulse": "24 settlements. $148,068 peak surge," with an operating settlement and cash-flow chart, a post-settlement pace of 1.5 and an October operating surge of 24. Anything you can do with a personal coding agent, Wotherspoon said, can become a managed agent run for the company daily or inside your own apps.

![Tickr in AI Studio](../slides/10-14-1455-build-your-own-team-of-managed-agents/19.jpg)
*Third demo: Tickr, a financial document analyzer from the AI Studio gallery.*

**Tickr in AI Studio.** "The easiest way to get started today is to go over to AI Studio and get started for free," Wotherspoon said, before a last demo from the AI Studio gallery, an app called "Financial Research Agent [AIE]." Its page read "Tickr, your intelligent financial document analyzer," which "automatically finds and synthesizes recent SEC filings and public disclosures," covering Forms 10-K and 10-Q, 8-K, and 3, 4, 5 and 13F, with the footer "Gemini can make mistakes, don't rely on it for financial advice." Wotherspoon entered VOO; "this ticker agent is a bunch of our different managed agents" that each look for a different analysis. The final agent produced a PDF report with analysis, key takeaways, charts and links to its sources: "it actually went to the Vanguard site, found the different documents, read them in, and then did analysis on it."

"And that is it on managed agents," Wotherspoon said. "Thank you very much."

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=970f672e-f351-46bb-9790-78c6b1baf81d) · [Video](https://www.youtube.com/watch?v=Y_p3y6zP5QE) · [Introducing Managed Agents in the Gemini API](https://blog.google/innovation-and-ai/technology/developers-tools/managed-agents-gemini-api) · [Interactions API: our primary interface for Gemini models and agents](https://blog.google/innovation-and-ai/technology/developers-tools/interactions-api-general-availability) · [Expanding Managed Agents in Gemini API](https://blog.google/innovation-and-ai/technology/developers-tools/expanding-managed-agents-gemini-api/) · [Antigravity agent documentation](https://ai.google.dev/gemini-api/docs/antigravity-agent)

This write-up is based on the official video captions, the author's room recording, slide photos (slides 1, 4 and 17–19 of the deck were not photographed) and Google's documentation. The talk ended without a Q&A. The [precise notes](precise/10-14-1455-build-your-own-team-of-managed-agents.md) (Chinese) keep timestamps and the full list of corrections and open questions.
