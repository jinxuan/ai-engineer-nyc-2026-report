**English** | [中文](zh/10-13-1648-frontier-models-for-acting-in-space-and.md)

# Pixels In, Actions Out: General Intuition Bets Game Footage Can Teach Models to Act

*At AI Engineer NYC 2026, Pim de Witte of General Intuition argued that models trained on gameplay video and controller inputs, rather than on text, will act in real time on screens and robots, and announced early access and hackathons for the company's first models.*

**Featured Keynote · Tuesday, October 13, 2026 · 4:48–5:06pm · Simons Room (Metropolitan East), Main Stage**
**Speaker:** Pim de Witte, Co-Founder & CEO, General Intuition

---

Pim de Witte opened with a thought experiment. If someone had said that the industry would take everything on the internet, train a model on it and use that model to solve scientific problems that were not on the internet, "I think a lot of people would tell you that that's crazy," de Witte said. "What we're doing has a lot of parallels to that story."

The illustration was Olaf, the snowman from Frozen. In one scene Olaf rolls down a mountain, hits a stone, comes apart and reassembles himself. "Olaf knows how to do that because Olaf can observe as he is acting," de Witte said. An LLM, by contrast, autoregressively predicts tokens. That difference, in de Witte's account, separates LLMs attempting robotics from "native architectures like what we're working on." Demos on Twitter make it easy to see LLMs as the thing that will solve everything, but "there's fundamental limitations … where purpose-built models are a much better fit."

General Intuition describes itself as "the frontier lab for acting in space and time." According to the conference schedule, de Witte previously founded Medal, a platform for sharing gaming clips; before gaming and AI, de Witte worked with Médecins Sans Frontières and, at 14, ran the largest RuneScape private server. The company says it has raised more than $650 million in the past year, with Khosla Ventures and General Catalyst among its backers.

## 1. A game recorder as "the internet"

"For us it all started with Medal," de Witte said. "We built the largest video game recorder in the world, and we think of that as the internet." People use Medal to record themselves playing tens of thousands of different games.

The argument rested on the controller. A person can teleoperate a robot with the same brain used to play a video game, and often with the same controller. So the action space of games and the action space of teleoperation, or of the real world, are in many cases the same. "Every single robot below certain degrees of freedom ships with the game controller," de Witte said. "A majority of the supply chains already work this way."

Medal's recorder ties inputs and actions to the frames they produced. General Intuition labels those actions "so we understand semantic meaning behind every action," and uses the data to train a vision-based model. De Witte described the model as "essentially a computer use model": it is given a set of keys to choose from, receives frames (and optionally a text prompt) through an API, and outputs actions. If someone waves at it and it knows a greeting action is available, it outputs that action. (The slide de Witte referred to here was not photographed.) The company's website calls Medal "the world's largest and fastest-growing platform for gamer moments," with players uploading "billions of gameplay clips" a year.

## 2. Turn-based text versus continuous frames

LLMs, de Witte said, took internet text, learned to predict the next token and are now used to write software. That work suits "futures that are not simultaneously unfolding as you are predicting." Code is turn-based: you say something, the model does something, and so on. That makes it "very, very difficult for it to generalize to environments … that include space and time."

General Intuition's models instead receive a continuous stream of frames, from a robot's camera or from a screen, and predict "everything as if it were just a video game." De Witte drew a comparison with Anthropic, which in de Witte's telling was never at the frontier of plain text generation but always focused on being best at code, betting that people would bring more problems into coding. General Intuition's bet is that "as humans we are generally pixel-to-action converters plus senses," and that many other senses can also be turned into pixels. The outputs are keyboard-and-mouse or controller actions, making the model roughly "an enhanced computer use model with more actions on the output space." In short: "you send frames and you get actions."

Humans, de Witte added, do not know how to predict motor torque. The controller is the device that translates intent into those values. Most robotics companies try to predict exact poses or motor values end to end; General Intuition's approach is to use the controller layer that already exists.

## 3. The scale of the data

De Witte gave the numbers behind the bet:

| Measure | Figure given on stage |
| --- | --- |
| Videos uploaded to Medal | about 1 billion a year; on track for 3 billion this year |
| Action tokens | over 1.5 trillion a month |
| Comparison | the total data frontier labs use to train robotics models, collected "at the moment" in half a day |

These figures come from the talk; the company's website says only that players upload billions of clips a year. "We just bet on scaling pixel space the same way that Anthropic scaled coding space," de Witte said, expecting that as data and compute flow into pixel space, people will bring more problems there. The result would be "an incredibly general agent that can go and solve problems on computers [and] in the real world."

## 4. World models as the training environment

Where Anthropic builds coding environments, General Intuition builds world models, "which most of you know us for the MIRA release," to generate more environments for its models to learn in. The company's website describes MIRA as "a playable multiplayer world model," built with Kyutai in collaboration with Epic Games and trained on 10,000 hours of bot-generated Rocket League play. The site separates "action models that decide what actions to take" from "world models that predict the outcomes of actions."

De Witte argued that an LLM company generating learning environments has two problems. Simulation costs compute, and the more realistic it gets, the more it costs. And learning should happen in the human's own input space: if a person is showing a robot partner something, the robot should observe the same thing, without going through an LLM, a digital twin, a deployment and "a bunch of verifier checks" before it learns. "When you can learn in the native modality of the human, you can all of a sudden start solving a lot more problems a lot faster." When the models come out, de Witte said, they will be "significantly faster than any LLM" at these tasks.

## 5. Why game data is action-dense

Games are "so action dense," de Witte said, and the model can predict several actions at once. That density transfers to demanding robotics work. Flying a drone takes many small adjustments: "you really want to start with games, you don't want to start with teleoperations, because the human will just take a lot less actions." A model needs to predict "really high APM" (actions per minute), and the APM of the dataset puts such problems within reach. Because the model also knows the semantic meaning of the key it is pressing, de Witte said, this matters for interpretability.

## 6. Release plans and early access

"We will be releasing the models this year," de Witte said. "We're stunned by the fact of what these models can do." Release videos could not be shown on a livestream without spoiling the surprise, but the team is in New York and has begun giving live demos.

"We have essentially one model which can play video games at the highest level and control over 10 types of robots using the exact same primitives. So pixels in, actions out, completely real time." Early access will roll out over the next few months; some customers already use the models privately. Places are allocated by the time people join the waitlist, reached through a QR code on the slide. The company's website says it has onboarded "a small number of partners" to its commercial API across games, simulation and robotics.

![Get Early Access](../slides/10-13-1648-frontier-models-for-acting-in-space-and/01.jpg)
*"The frontier lab for acting in space and time": the early-access slide, photographed in the room.*

De Witte expects developers to tire of the unpredictability of LLMs once a new primitive is available: "People are ready to kind of do away with the unpredictability of an LLM and put something in that is more reliable and faster." Uses de Witte listed included exploring environments generated with LLMs and testing user interfaces, since the model acts as a human would on an interface: "all these problems that LLMs can't solve because they can't actually explore like a human."

The models are not meant to replace planners. De Witte still expects users to run an orchestrator model, "something like Gemini or Astra," for long-term planning and plug General Intuition's models into their workloads. (The captions read "Astra," most likely OpenAI's GPT-6 Astra; this is not confirmed.) De Witte compared the moment to "the early LLM days when you have a new toy" and said early adopters could build businesses on it. One example was a science lab that can reliably orchestrate robots from text instructions. "Robotics is interesting because one of the big problems of solving robotics is making more robots," de Witte said; many businesses bottlenecked by intelligence would emerge from solving "pixels to actions."

## 7. An accessibility use case

One use case de Witte singled out as most exciting was accessibility. Because the models use computers like a human, a person who can use only one arm could control the mouse while the model predicts the keyboard actions. And because the model observes the user's actions as input, "it's observing as it's acting," it will not predict what the user is already doing; it fills in only the keys.

"If you're interested in building in this space, this is going to be the most interesting toy that comes out this year," de Witte said. "I'm fairly certain." Much early use would be in simulation: people already generating environments with other labs' models would now have something that can explore those environments for them.

## 8. Team, timeline and hackathons

The Medal and General Intuition teams are both in New York City. De Witte asked attendees to get in touch for demos, and especially to join. The company wants "people who want to understand these models as much as we do," ideally good at generating novel environments; a research background helps, "but I don't have a research background, so that's okay." The ideal hire has shipped AI products end to end.

There are three teams: world modeling, policy (the agent), and data and environment generation. "If you're an engineer, the best one to join is probably the environment generation team."

On timing, de Witte said "I'm very, very excited for November," and that the current plan is "either late November, early December." The company will also organize hackathons. De Witte announced "three hackathons," the first in New York City in mid-November, with invitations going to people on the partner waitlist, then named only two cities: "there will be one in New York City and one in London."

![Thank you slide](../slides/10-13-1648-frontier-models-for-acting-in-space-and/02.jpg)
*The closing slide: a team photo, de Witte's contact address and a QR code for open roles.*

The closing slide showed a team photo, the line "Stay in touch!" with de Witte's General Intuition email address, and "We're hiring · Scan for open roles."

## Q&A

De Witte said there was time for "one or two questions"; one was answered. The question itself was not picked up by the microphone.

### Action labels versus JEPA

**How does this differ from JEPA?** (inferred from the answer) JEPA "doesn't train alongside the action labels," de Witte said. "They bet on inferred action labels for the most part." De Witte called JEPA "in the limit … the more correct approach," but "a lot less practical in the short term because you need to bet on a separate scaling law on the action inference itself." General Intuition likes JEPA as a concept and uses some of its ideas, "but at the end of the day it's an approach, not a product," and the two will borrow from each other.

De Witte gave an example of why action labels matter. A pilot moving the rudder at the back of a plane makes a change that is almost impossible to see in the video frames. "Our moat on having the action data really transfers better to action sequences than it does to world models and to generating the pixels." That is why the company went all in on general foundation models for action prediction, while still using many techniques for world models and welcoming other companies that build them. De Witte said "one more and then I got to run," but the session ended there.

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=cada670e-c5ea-4977-b3c2-9a68dda26abb) · [Day 1 livestream at 7:02:19](https://www.youtube.com/watch?v=akEqiXFo3GM&t=25339s) · [General Intuition](https://generalintuition.com)

No standalone video of this keynote had been published at the time of writing. This write-up is based on the official Day 1 livestream captions, the author's two slide photos from the room and the company's website. The [precise notes](precise/10-13-1648-frontier-models-for-acting-in-space-and.md) (Chinese) keep livestream timestamps, caption corrections and the open questions.
