+++
title = "Air Launch Button"
date = 2026-11-09T11:00:00-07:00
draft = false
description = "3...2...1...Launch! Unnecessarily complicated button"
tags = ["electronics", "micro-controller", "music"]
categories = ["Projects", "Electronics"]
+++


As a developer there comes a time when a project makes its transition from development to production. It's a terrifying moment and often comes with a mix of celebration, nervousness and panic when something suddenly happens in production which never happened in testing and we need to franticly patch the bugs.

In this post I'd like to focus on the celebration aspect, and pretend the other bits don't happen. More specifically I'd like to focus on a little project I embarked upon to mark the occasion for the launch of our recent big project.

A few weeks before the set launch date as a team we discussed what we might to do mark the unveiling of our years long project on the world. Some mentioned food (a classic according to the wonderful Ologies podcast on [Party Psychology](https://www.alieward.com/ologies/partypsychology), others mentioned drink, then one offhand comment rose above the chatter for a second (I can't actually remember who said it), but _someone_ said "Oh, wouldn't it be cool if we could press a big red button to launch it". 

Well, that's all my brain needed to jump into action and dedicate all it's spare time and processing power to working on just that.

A one-time, completely pointless, completely unnecessary, surprisingly time-consuming project which at the end would be shelved away as junk.

Perfect! Just what I like.

I love the freedom of these sorts of projects. The temporary nature of the final thing means it can be held together with tape, blu tack, and a Canute-ish force of belief, and then allowed to collapse back into the pile of junk from whence it came ready to be born again as a new thingamabob.

As with many of my projects, hindsight often reveals how unnecessarily complicated the path I chose was, but through that comes incidental learning, frustration and forks in the road which make it feel more like a journey than a direct path from initial idea to implementation.

I'd just been in the land of microcontrollers, setting up the initial phase of my [electric concertina](https://applied-bodgery.co.uk/posts/electric-concertina-midi/) so this as a tool to realise harebrained schemes was top of mind. So I temporarily ripped the heart out of my concertina and put it to use as the brain for my passion of the week.

I now had the full concept in mind: a button which would play a little song as a celebratory marking of the launch. I did have grand ideas to have this actually 'launch' the platform firing off an API request, but it was technically already live (and had been for a while), this was more of a symbolic launch for when the first users would be given access.

The song I wanted to be a catchy little jingle about the launch, with some mention of the development process with maybe a bit of an in-joke in there. As explained in my [duck head](https://applied-bodgery.co.uk/posts/the-duck-head/) post, I believe these are the things which bring people together and form a 'tribe'. The silly in-jokes, the traditions, the ceremonies, the unwritten shared understandings which give us resilience when things get tough, but also give us joy when everything is ticking along smoothly.

Up until now I have avoided using AI for my projects. I use enough of it at work and it feels in direct conflict with the purpose of my projects and the motivation behind why I do these things. I speak more about this in my [why do I bodge](https://applied-bodgery.co.uk/posts/welcome-to-the-blog/) post, but the short of it is, I yearn for the hours of frustration banging my head against the table, the deep dives into corners of the internet to find one tiny little detail, the mistakes and real human errors.

Despite all that, I proceeded to use AI to slop out the song.

It was my first dabble in using an AI model for creating music, and it felt exploratory and new, unlike my day-to-day usual use of an LLM spewing out overly verbose walls of text. I still have conflicting feelings about it in general, but can't deny it was fun.

For a while now I've been playing a wide range of instruments badly, but well enough that I love making music. I find it extremely calming sitting down to the piano or plucking away at my guitar, purely for myself and making just the sounds I like to hear.

This was nothing like that. It felt more like playing a one-armed bandit, one that pays out every time. Sometimes you hit the jackpot, sometimes you get something vaguely disappointing, but you always get something. And, as I imagine any gambling addict would tell you, the real thrill isn't in the winnings, it's in pulling the lever just one more time and waiting for the reels to stop.

So I kept pulling the lever, sticking in prompt after prompt trying out different music styles. Classical/Drill-rap fusion, jungle with a hint of K-pop, nursery rhyme sung by a dolphin. I went completely off track for a little while (maybe a long while, I have no idea) and then snapped back with a bit of focus. I wrote some lyrics myself, hummed a little tune and put it all in with a prompt for a little computer game-style jingle.

I downloaded the MP3 and was ready to think about how I would get the full setup configured.

I'd ordered a little DAC/amplifier module. It takes the digital output from the Pico 2, converts it to analogue and amplifies the signal so I can attach a tiny 4 Ohm speaker.

I then pulled out my trusty breadboard and got to wiring it all up with dupont and jumper wires. The final was something like this:

```
                     BUTTON
                    +------+
Pico GP15 ----------|      |
Pico GND -----------|      |
                    +------+


  PICO 2                     MAX98357
  ------                     --------
  GP10  -------------------- BCLK
  GP11  -------------------- LRC
  GP12  -------------------- DIN
  VBUS  -------------------- VIN
  GND   -------------------- GND

                              SPK+ ----+
                                       |  SPEAKER
                              SPK- ----+
```

Simple. And it worked as you would expect. I tested with my mp3 and it played the tune, sounded a bit tinny and quiet but it was a tiny speaker and it was all it needed to be.

Up until this point the 'button' was actually just me tapping two wires together. But I wanted a nice big red thing like you're launching a rocket. I looked up how much it would be to buy a prefab one, and decided against shelling out £20-£40 and instead to find some stuff around my house to fashion one instead. Full on bodge time.

![Finn and Jake high-fiving under the words “Bodge Time”.](bodge_time.jpg)

As any parent will know toys seem to just spawn around small children. And our daughter is no exception. Every visit to a charity shop seems to result in us discovering a toy which either we or she took a liking to, and for 50p it's hard to find justification to _not_ bring it home. Luckily these are fantastic sources of weird shapes of wood, plastic and metal from which one can stand up many a bodge.

It was from this pile of toys I found a red domed top to a stack of rings, perfect for a big red button.

Another source of good bodging materials is my basement. Just like toys spawn around my daughter, bits of broken things spawn in my basement. One such thing was an old warning light button from the dashboard of a car. I have no recollection of where it came from or what I was keeping it for, but it was there and I guess I was keeping it for this.

So I stuck the red dome on the button by way of some double sided tape and a very old and crumbling chuppa-chups eraser to let the button sit a little more proud. This was all set into the base of a plastic box which was once used to hold a set of fairy lights and the electronics popped inside. A little usb power bank provided power and I closed it all up.

This could have been the end, and would have been good enough, but on showing my wife the finished product it was clear the song was too long and the whole thing a bit underwhelming and boring. So it was back to the AI, cutting the song down from a 3 minute odyssey, going into the roles of everyone involved, to a 30 second jingle. 

I also wanted to add some flare, something animated, something flashy. LEDs were the obvious choice. Low power requirements and simple to control direct from the GPIO pins.
I wrote a bit of code in the microcontroller firmware to light up these LEDs in response to the volume of the tune. For this I converted the mp3 to WAV as it contains uncompressed PCM samples, which the I could read in to easily calculate loudness. That level was then mapped to the LEDs brightness across the set.

I printed out a little label, stuck it on and it was done.

True to intent, it lived just long enough to serve its purpose marking the launch event and was promptly torn apart to be used in the aforementioned concertina and other projects. 

Tibetan Buddhist monks spend days painstakingly creating intricate sand mandalas, only to ceremonially destroy them upon completion. It's a profound meditation on the impermanence of existence and the importance of letting go.

Sometimes it's important to have a relic of the time, care and love your pour into something. A thing to look back on and remember the journey.

But sometimes the making is enough. The thing itself has served its purpose, and there's no need to preserve it beyond that. The memories, the lessons learned and the joy of having made something are capable of surviving without the object itself.

I have recently been reminded, by a colleague, of the tale of King Solomon and the ring he possessed inscribed with the words "This too shall pass", a reminder that neither our greatest triumphs nor our deepest troubles are permanent.

I think its important to remember, particularly after years spent working towards a single moment. The frustrations eventually pass, but so too does the excitement of success. Perhaps we should not cling too tightly to either, but to appreciate them while they're here and enjoy the ride.

And if that appreciation happens to involve a chuppa-chups eraser from the 90s, a button from a car probably long been destroyed and a dollop of AI slop, then so be it.