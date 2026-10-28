**English** | [中文](zh/10-13-0954-does-code-quality-even-matter.md)

# Code Quality Depends on How Disposable the Software Is, and Rigor Can Be Made Cheap

*At AI Engineer NYC 2026, Max Kanat-Alexander of Capital One argued that code quality exists only to help people understand and safely change software, so it stops mattering for the throwaway programs AI is making common, while durable systems still need it and can get it more cheaply.*

**Featured Keynote · Tuesday, October 13, 2026 · 9:54–10:12am · Simons Room (Metropolitan East), Main Stage**
**Speaker:** Max Kanat-Alexander, Principal Software Engineer, Capital One

---

Max Kanat-Alexander opened with sarcasm. "Today I figured I'd just pick a nice uncontroversial topic that nobody has any opinions about," Kanat-Alexander said, adding that every take on social media was "really deeply nuanced" and that "maybe there's a little bit more to it than like yes or no."

![Absolutely no, absolutely yes](../slides/10-13-0954-does-code-quality-even-matter/01.jpg)
*The two camps of the online debate.*

Kanat-Alexander leads AI-assisted software development at Capital One and was previously Chief Architect of the Bugzilla project and tech lead for Code Health at Google; the host introduced Kanat-Alexander as the author of the book *Code Simplicity*. The talk set out to replace every senior engineer's favorite answer. "It depends," Kanat-Alexander said, but "that's not a very helpful answer though, frankly, because what does it depend on?" The starting question was why code quality ever mattered. "I see a lot of people be like, well, it made me feel good. That's not really a sufficient justification."

## 1. Everything depends on understanding

![Understanding and executing on that understanding](../slides/10-13-0954-does-code-quality-even-matter/02.jpg)
*The two factors behind any software outcome.*

Anything you want to accomplish with software, Kanat-Alexander said, "depends on two factors and two factors alone": understanding, and executing on that understanding, whether the goal is "aligning a model, scaling a system, driving adoption." Debuggers, observability, evals and even tests exist to improve one or the other. "Everything we have ever done in the field of code quality is exclusively and only for these two things." It has nothing to do with correctness or anything else.

![Behavior and how to correctly modify it](../slides/10-13-0954-does-code-quality-even-matter/03.jpg)
*The two understandings a software engineer needs most.*

For an engineer, two understandings matter most: the actual behavior of the system ("not what does a doc say the behavior of the system is") and how to correctly and safely modify that behavior.

## 2. Why structure matters

![Hello World to Hi World](../slides/10-13-0954-does-code-quality-even-matter/04.jpg)
*In a tiny system, understanding is just knowing what it does.*

In a tiny program there is little to understand: it prints hello world, and if you want it to say hi world, you change it. Real systems are different.

![A map of the Chromium code base](../slides/10-13-0954-does-code-quality-even-matter/05.jpg)
*"A real map of the code base of Chromium."*

The next slide showed a dense visualization that Kanat-Alexander described as "a real map of the code base of Chromium, which is the open-source project behind Chrome." With all the ways its components can interact, it has "essentially infinite possible behaviors." "I could not describe to you in words the behavior of Chromium as a whole." (The source of the visualization was not given.)

![Chromium's URL parser](../slides/10-13-0954-does-code-quality-even-matter/06.jpg)
*The call tree that parses `https://example.com:443/articles?id=7#summary`.*

What an engineer can describe is a component. The slide traced how Chromium's URL parser splits a URL: `ParseStandardUrl` calls `DoExtractScheme` and `DoParseAfterSpecialScheme`, which calls `DoParseAuthority` (in turn `ParseUserInfo` and `ParseServerInfo`), `FindNextAuthorityTerminator` and `ParsePath` (in turn `FindQueryAndRefParts`). The function names match Chromium's `url_parse.cc`. "This is the whole reason that code exists," Kanat-Alexander said: "for structure."

![Hello World in Python and in assembly](../slides/10-13-0954-does-code-quality-even-matter/07.jpg)
*Same output, different structure.*

Both programs on the slide print Hello World on Linux: a one-line `print("Hello World")` and a block of x86-64 assembly that loads registers and makes system calls. The first "has the identity and structure of Hello World"; the second is "a series of instructions with no particular identity." At millions or billions of lines, "it would be much harder to grok." Readability and testability sounded like human concerns, Kanat-Alexander said, "but what actually mattered the most always was that the system had some sort of understandable structure."

## 3. The two risks of every change

![Two categories of risk](../slides/10-13-0954-does-code-quality-even-matter/08.jpg)
*Every modification carries both.*

Structure matters most when you modify a system, and every modification carries two risks. The first is introducing a problem, which is not just a bug: a project that runs long, that doesn't do what users wanted, or that fails on budget overruns. The second is making future changes harder, so that future problems become more likely.

![Both risks come from understanding](../slides/10-13-0954-does-code-quality-even-matter/09.jpg)
*The same list with its root causes: difficulty of understanding, and making it harder to understand.*

Both come down to understanding: the first from how hard the system is to understand, the second from making it harder. "I can reduce every postmortem I have ever read for any incident to these two factors," Kanat-Alexander said: a production configuration too hard to understand, someone who wrote something impossible to understand, emergent properties no one could understand. The second feeds the first in "a vicious cycle that compounds in a non-linear fashion," until a system reaches the point where "every modification involves some sort of problem," as in "giant 100 million, 500 million line mainframe code bases."

Future models might gain "infinite ability to deal with complexity," Kanat-Alexander allowed, but that was not the subject, because models were doing something else that was interesting right now.

## 4. Disposable software

Models are creating "a new category of software," Kanat-Alexander said. "This category has always existed, but it's about to become a hundred thousand times more common than any other form of software. And that is disposable software."

![Four factors](../slides/10-13-0954-does-code-quality-even-matter/10.jpg)
*Consequences, dependents, cost of replacement, cost of maintenance.*

Imagine software where the consequences of failure are minimal or nonexistent, few people and almost no other systems depend on it, change management barely exists, it can be replaced "in hours or days," and it costs almost nothing to maintain. "In that world, code quality does not matter," because the compounding risks it guards against don't exist.

## 5. Durable software is not going away

![Durable versus disposable software](../slides/10-13-0954-does-code-quality-even-matter/11.jpg)
*The two kinds of software side by side.*

| Durable software | Disposable software |
| --- | --- |
| Lives for years | Replaceable in hours |
| Depended on by millions | Depended on by a few, or many who don't care if it changes |
| Major consequences of failure | If it doesn't work, I'll just make my own |
| Requires continuous maintenance | Maintenance is unimportant or nearly free |

Traditionally the industry has built durable software, with failures that can be "business catastrophic" (on stage Kanat-Alexander said "depended on by millions or thousands of people"). "Most of the software engineers in the world spend most of their time maintaining these giant durable systems." Kanat-Alexander expected that to continue, and said this was where the online conversation goes astray: durable systems will probably remain the most important in the world, and there will probably be more of them. "But the most numerous systems will be disposable software."

If a disposable tool doesn't work, "you will give everybody agents. They'll make a version of it themselves." "Don't like the calculator on your OS? Make your own. Want to plan a party for your kids? Write a party planner software just for that one party. Throw it away. Code quality doesn't matter. Nobody cares."

## 6. Rigor scaled to disposability

![Less rigor to more rigor](../slides/10-13-0954-does-code-quality-even-matter/12.jpg)
*A spectrum from more disposable and less rigor to more durable and more rigor.*

"In the future, we will apply engineering rigor to the degree of disposability of the software," Kanat-Alexander said. At the disposable end, "none of our practices of engineering rigor matter at all"; at the durable end, all the practices built up over decades "matter greatly."

![The four factors on the spectrum](../slides/10-13-0954-does-code-quality-even-matter/13.jpg)
*Consequences, cost of replacement, dependencies and cost of maintenance place a system on the line.*

The four factors place a system on that line: consequences, cost of replacement, dependencies and cost of maintenance. All low means disposable; all high means durable. Kanat-Alexander added a caution from experience: "you should err on the side of it being durable. You should err on these all being higher than you expect them to be."

## 7. Making software more disposable

There is also something new teams can do: "You can make software more disposable." (No photo of this slide was taken; the list follows the spoken talk.) Kanat-Alexander named four levers.

**Isolated environments.** A place you can deploy that has access only to your own data: "No customer data, no personally identifiable information."

**Enforced independence.** "No inbound API calls from other production systems are allowed to this system in this environment," so everything deployed there is disposable.

**Redundancy.** Keep "an old clunky backup system." People can vibe code more efficient front ends of their own, because the durable system is still there when the "crappy little vibe coded thing" fails. "Which it will fail, right? That's fine. But, it doesn't matter if it fails."

**Limited scope.** A calculator for one person; a planner for one party.

Kanat-Alexander called this "a whole new discipline of software engineering": not improving the rigor of durable systems, but encouraging and allowing more disposability.

## 8. Lowering the cost of rigor

![Durable software engineering is not going away](../slides/10-13-0954-does-code-quality-even-matter/14.jpg)
*The point Kanat-Alexander re-emphasized.*

"This is the biggest confusion that I see on social media today," Kanat-Alexander said. "I believe the absolute amount of durable software in the world will increase because AI coding tools make it easier and easier to make more durable software." Disposable and durable are not black and white; across the spectrum, what changes is how much effort goes into rigor, "both immediate effort and effort over time."

![Engineering effort with and without better foundations](../slides/10-13-0954-does-code-quality-even-matter/15.jpg)
*Engineering effort rises steeply toward the durable end without better foundations, and gently with them.*

The chart plotted engineering effort against the disposable-to-durable axis: one curve labeled "without better foundations" climbed steeply, while one labeled "with better foundations" rose gently. What to care about most, Kanat-Alexander said, is "reducing the cost of rigor." The cheaper rigor is, the less it matters where a system sits, and the easier it is to recover from a wrong guess: "The cheaper you make it to make it durable, the easier it is to change your mind or to be wrong because you're never going to be 100% right all the time on that estimate." That takes a different set of practices, "not engineering rigor themselves," but practices "about how to decrease the cost of engineering rigor."

With "about 5 minutes left on that timer," Kanat-Alexander covered four (again without a photo of the slide):

**Standardization.** "We do standardization because it removes possibilities." With no PHP in the system, nobody has to work out PHP's linters, practices and security holes. The same goes for frameworks and RPC protocols: "Yes, you could still have the agent code up all your tools, but you have to do it every single time."

**In-workflow tooling.** "Tooling that just occurs. The programmer doesn't have to choose to run it. They don't have to understand how it runs." It must be reliable, with few false positives, and fast; then it can be applied to disposable software too. "If it's free and fast, who cares?"

**Making testing easier.** Not better testing, but "how do you make it so that nobody has to think about how to do better testing?", with every test fast and deterministic without anyone being an expert. Kanat-Alexander's longtime dream as a test owner was for every team to provide in-memory fake versions of their services. "Guess what? The coding agent can do that. So, you can just do that now, right? But you have to choose to do it and make it available to everybody."

**Maximum strictness.** Set every lint rule to maximum and "never compromise on anything." Like standardization, it removes many conversations; an occasional wrong lint is acceptable while false positives stay rare and consequences small. The alternative is turning on a linter late in a million-line code base and facing "my 100,000-line change."

These were only a few areas, Kanat-Alexander said, of "a whole area of software engineering discipline that you can apply on the other axis."

## 9. The answer

![Does code quality even matter?](../slides/10-13-0954-does-code-quality-even-matter/16.jpg)
*The closing slide answers three questions.*

The framework turns "it depends" into a yes or no. Does code quality matter for disposable software? No. For durable software? Yes. And a third, "even more interesting" question: does it have to be expensive? "No," Kanat-Alexander said, and closed the talk.

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=5191589c-a70e-4806-99dc-86f7794f46b9) · [Video: "Does Code Quality Still Matter? It Depends on This"](https://www.youtube.com/watch?v=bU5gDVyu_28) · [Day 1 livestream at 43:16](https://www.youtube.com/watch?v=akEqiXFo3GM&t=2596s) · [Chromium url_parse.cc](https://chromium.googlesource.com/chromium/src/+/refs/heads/main/url/third_party/mozilla/url_parse.cc) · [Code Simplicity](https://www.codesimplicity.com/)

This write-up is based on the official video captions (checked against the Day 1 livestream) and the author's slide photos of the livestream. The YouTube video was retitled "Does Code Quality Still Matter?"; the schedule and slides say "Even." The talk ended without Q&A. The [precise notes](precise/10-13-0954-does-code-quality-even-matter.md) (Chinese) keep timestamps and caption corrections.
