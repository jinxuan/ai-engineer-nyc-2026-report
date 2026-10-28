**English** | [中文](zh/10-14-1706-everything-will-be-simulated.md)

# Runway Bets That Next-Frame Prediction Will Turn Into a Simulator for Everything on a Screen

*At AI Engineer NYC 2026, Runway co-founder Cristóbal Valenzuela argued that video models trained only to predict the next frame are learning physics, becoming interactive in real time, and could eventually render the computer interface itself, and showed early research demos of an operating system called Continuum.*

**Featured Keynote · Wednesday, October 14, 2026 · 5:06–5:24pm · Simons Room (Metropolitan East), Main Stage**
**Speaker:** Cristóbal Valenzuela, Co-Founder and Co-CEO, Runway

---

Cristóbal Valenzuela gave the last talk of the conference's main stage. "Thank you for staying for the last talk," Valenzuela said. "Welcome to my talk about the matrix and how we're living through it." Valenzuela said they had been working on Runway "for almost a decade," starting in New York "maybe around 10 years ago," and wanted to share what the company had learned building a research lab in the city, and where its work in video and pixel generation is heading.

## 1. From "The Orange" to video nobody can tell from real

Valenzuela started with what they called "prehistoric ancient ruins": Runway's first AI film, made in 2019 and titled *The Orange*, in which an orange falls from a tree and travels through different parts of the world. The model behind it generated frames at 128 by 128 pixels, which the team "forcibly upsample[d]" to screen resolution. "I think of this as art now," Valenzuela said. "This is like looking at a film from the 1900s."

The company was started "in 2018, 2017," Valenzuela said (Wikipedia gives 2018), and at the time the team was trying to convince itself that scaling such models would eventually produce something close to what Valenzuela showed next: "state-of-the-art video today. This is all generated. None of these characters are real. None of those locations are real." Valenzuela added: "When I look at stuff like this, I still believe this will look like the orange in just a couple more years."

## 2. Art, science and the illusion of movement

Runway sits "at the intersection of both art and science," Valenzuela said, with "deep roots in artistic and creative communities" and "some of the best researchers in the world working on diffusion models and large video models": "a team of scientists and artists thinking about the problem of scaling video and world models."

The company's core idea, Valenzuela argued, was discovered by cinema 120 to 130 years ago. "Video is an optical illusion. Video is the illusion of movement": stitch frames together fast enough and you get motion pictures. Runway takes that idea further: "How do we do next frame prediction fast enough, scale enough," so that the result is more than pretty creative videos.

## 3. Scaling laws and what next-frame prediction learns

All of this is "powered by scaling laws," Valenzuela said. Runway believed scaling laws applied to video as well as language, "and this has been true. We're now really ramping up the amount of compute." What interests Valenzuela is "the emerging properties that appear, the intelligence that appears" as the models scale.

Take an image of two people who do not exist and keep predicting the next frame from the previous ones. "If I do it fast enough and if I do it with a network that's big enough, I'll get movement," Valenzuela said, something that looks so real humans cannot tell. "I've been doing this for 10 years. I can't tell you if that's real or not." Runway measures this, Valenzuela said: "We actually measure and try to quantify if people can distinguish between real and generated videos. And I think that Turing test we already passed. People are not able to distinguish." Valenzuela did not describe the measurement.

The more important point, Valenzuela said, is that predicting frames teaches a model much more than frames. "You're learning about physics. You're learning about object permanence. You're learning about gravity. You're learning about occlusion. You're learning about movements," all from video and next-frame prediction. To render a complex scene consistently, a model has to understand what each subject is, how they relate, "how a camel might probably stand on a ball like that, what water in a container looks like." That is why many of the benchmarks Runway uses to evaluate its video models are not about whether people like the output but about "physics, geometries, how objects behave in the real world."

## 4. A physics engine with no physics laws

Run the same next-frame prediction in a 3D or game-like environment, Valenzuela said, and "you can start actually getting closer to a physics engine, a simulation system. But the simulation system has no physics laws. We didn't describe what the behavior of the object should be." The model learns it through the brute force of scale. One demo looked like a video game: "This is not GTA. This is someone playing in an environment and a software that doesn't exist. It's all simulation of a simulation." "I'm not kidding when I said welcome to my matrix simulation," Valenzuela said.

Valenzuela did not name a model. Runway released its first world model family, GWM-1, in December 2025; [TechCrunch reported](https://techcrunch.com/2025/12/11/runway-releases-its-first-world-model-adds-native-audio-to-latest-video-model) that it builds its simulation through "frame-by-frame prediction" and comes in Worlds, Robotics and Avatars variants.

## 5. Real time and interactive

Everything shown so far took seconds or minutes to generate. For the next three to four years, Valenzuela said, the question is what happens when the models are not only scaled up but made interactive. Keep predicting frame after frame, but allow an action at any point, "which means I can simulate any possible divergence or action in that world." A viewer can play the video, "but I can also come and touch every part, any aspect of that image and make it interactive to affect the consequences of that world. And it works."

One use is robotics. "We're now spinning an entire robotics lab in Runway," Valenzuela said. "If you come to our office in New York, half of it is what looks like a Hollywood studio. Half of it is a bunch of robots walking through and moving through the office." The aim is to make these models "the brains of how autonomous and embodiment systems walk and reason about the world." (GWM-Robotics, per TechCrunch, uses synthetic data to train and test robot policies.)

## 6. Any pixel on a screen: the world interface model

Beyond video and robotics, Valenzuela said, the more interesting case is "what happens when you think about anything, any pixel on a screen as a video model."

The first demo started from an image of a car. Running one of Runway's video models in real time turned it into an interactive application: Valenzuela clicked to turn on the lights, turned the car around and back, and opened the hood to look inside. "Everything you just saw here has no physical 3D representation. There's no Unity. There's no Unreal. There's no Blender. It's just a video model," trained "to understand actions and clicks in an interface." "We call this a world action model," Valenzuela said, then corrected: "sorry, a world interface model." (Secondary coverage reports that Runway calls its interface model Solaris, the first of a family of "Interface World Models"; Runway's own announcement could not be checked.)

In a second demo, Valenzuela moved objects around inside a photo of a room and they moved "as if I was actually in that room." "Remember there's no 3D representation of this there. This environment might not actually have been modeled before. It's just an image and the model that understands how objects move in a space," all because "next frame prediction scaled well enough can become a world model or a simulation engine."

## 7. Continuum: an operating system streamed from a video model

"With the eight minutes I have left," Valenzuela said, the next step was to push the idea to an extreme. Extrapolating, Valenzuela said, "might be one of the most important skills these days in AI, in company building": what happens over the next six, twelve, eighteen months. Runway asks what happens if it keeps scaling models, adding data and making them "universal simulation systems that can predict anything on a screen, literally anything on a screen."

The result is a research project called Continuum. "This is an operating system," Valenzuela said. "You might open your computer at some point in the near future and what you see streaming to your screen might be coming from a video model." Valenzuela stressed that it "is still very early research." [Runway Labs](https://labs.runway.com/) describes Project Continuum as "an operating system research application designed for real-time video interfaces" and lists four interactions, which match the four demos:

| Runway Labs name | Description on the Labs page | Demo on stage |
| --- | --- | --- |
| Portals | "Select anything; a place opens over it" | San Francisco fog; batteries |
| Visual Thinking | "An agent's steps, drawn as it works" | A coding agent designing a bike |
| Responsive interfaces | "One interface, re-rendered at every size" | Photosynthesis, resized |
| Worlds | "Step through a portal into a world" | A Mars outpost |

**Portals.** Valenzuela had been researching fog in San Francisco on an ordinary website. In Continuum, "I can select any part of content in that website and make it a portal." A portal "is a real-time video. It's fully interactive and fully based on the content you're experiencing reading through." "It's too much text for me. So I just want to make a video." Portals can be 3D visualizations or other content, and a panel on the right keeps every portal the user has opened, "basically a memory." Going back to an earlier portal about batteries, Valenzuela clicked and moved parts of it and asked questions, and "the videos and the content itself will change based on my interactions."

**Responsive interfaces.** "When the team showed it to me it was like, blew my mind," Valenzuela said. Because the interface is a real-time video, resizing the window changes what it shows. In a portal about photosynthesis, "if I resize the application, I get a different experience," and Valenzuela could click through parts of the cell. "I don't know how to call this," Valenzuela said. "It doesn't feel like a visualization. It doesn't feel like a video. It doesn't feel like a game. Something in between." Naming these interactions, Valenzuela said, is collective work still to be done.

**Visual thinking.** Valenzuela called this the most under-explored idea. Runway connected Continuum to a coding agent so the agent can visualize a problem. Asked to create a 3D model of a bike, "instead of the model trying to just think through that via code, the model can spin a version of Continuum to think through the problem visually," the way a visual-thinking person does. "Agents can spin up a sort of imagination." The agent interacts with it, and the user can watch it too and "guide the agent in much better ways" than through text alone. "It's a completely new paradigm," Valenzuela said.

**Live, reactive worlds.** In the last demo Valenzuela searched for Mars. Continuum inferred the intent and "suggests interfaces that are specifically designed in real time just for that," in this case a Mars outpost that "should be accurate with the data about the actual robots that are in Mars right now or with the ones that are projected maybe in 2030." Valenzuela moved around inside it as in a 3D system. "And it works. Just fascinating."

## 8. "Everything will be simulated"

The title, Valenzuela said, is an attempt to help Runway's team and the world get to the point where much of what was shown "will sound obvious." "Hopefully in a few more years or months, we'll look back at Continuum and it might feel very close to the video of the orange that we just showed you." Continuum and Runway's simulation work "are still in very early days, but we're getting to a point where it actually works and it really scales." Valenzuela ended with a release plan: "For the next 8 to 12 months, we're releasing most of what I showed here for everyone to use." (Secondary coverage of the October 2026 Continuum announcement reports no download, pricing or API yet.)

---

**Links:** [Schedule](https://ai.engineer/nyc/2026/schedule?session=af8422e8-d5f8-4dbd-a918-6360f5bfba80) · [Livestream at 8:29:11](https://www.youtube.com/watch?v=jiUC6IUMdvc&t=30551s) · [Runway Labs: Project Continuum](https://labs.runway.com/) · [Runway releases its first world model](https://techcrunch.com/2025/12/11/runway-releases-its-first-world-model-adds-native-audio-to-latest-video-model) (TechCrunch, GWM-1)

This write-up is based on the official livestream captions and the sources above. The author has no slide photos of this talk, whose demos were mostly video, and no separate video of the talk had been published at the time of writing. The talk ended without Q&A. The [precise notes](precise/10-14-1706-everything-will-be-simulated.md) (Chinese) keep livestream timestamps and list the details that could not be verified, including the name and date of *The Orange*.
