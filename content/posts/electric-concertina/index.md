+++
title = "Electric Concertina"
date = 2026-10-16T11:00:00-07:00
draft = false
description = "A post about electric concertina"
tags = ["markdown", "reference"]
categories = ["Reference"]
+++


This journey started back in March 2024, walking past a vintage junk shop and a battered and broken concertina caught my eye.

For the low sum of £10 it could be mine. I had no idea how to play it, no idea if it worked and if not no idea how to fix it.
But like all these projects that was no reason to not give it a go.

It came home with me and within a few minutes I'd forced out a rendition of Twinkle Twinkle with only a few missing notes. It was evident at this point that not only was it badly out of tune but it missing some of the reeds completely, and the bellows were leaking like a sieve.

After a little coaxing and fiddling with some very rusty screws I'd opened up the body of the concertina and saw some snapped reeds, fairly obvious holes and a lot of dust.

Enter bodge #1

What was I to do about these missing reeds? A quick google search and it was clear this was going to spiral from a fun little project into 
a significant investment of time and more importantly money. All I wanted at this point was to bash out a full version of Twinkle Twinkle Little Star or Hot Cross Buns, with all the notes.

If I couldn't feasibly get concertina reeds, what else could I use. I looked around the house and came across my old harmonica. I thought to myself, huh, that uses reeds, and they look pretty similar, how different can they be (turns out a bit).

Anyway, rather than destroying my reasonable quality harmonica and ending up with two broken instruments. I found a harmonica in a charity shop for £1 and quickly got to work dismantling it.

The reed plate was one solid piece with the metal reeds attached by a pin. On the concertina the reeds were small metal plates held in a wooden frame. So far so simple, just chop up the reed plate of the harmonica and replace the missing bits with ones that match the note I'm missing.
I'm sure anyone who knows anything about concertinas is now screaming at the screen with all the reasons this is a bad idea, but this _is_ a bodge. My aim is not to restore this to a quality performance instrument, but as always to see if I could get close at minimal cost and with what I have lying around or easily available. I wasn't going to shell out a few hundred quid, or wait weeks for a shipment of reeds.

So chop up I did. 

After a lot of blowing through small sharp bits of metal to find the right notes, I secured the reeds in place and put the whole concertina back together. I gave the old faithful Twinkle Twinkle another bash and although all the notes were there, the tuning still had something to be desired.

A bit more research and reading of some very helpful reddit posts and I found to raise the pitch I had to remove some of the material from the end of the reed and to lower the pitch I had to add some. Again seems simple enough. For all the flat notes I filed off a little metal at a time and used a tuning app on my phone to check. For all the sharp notes I added a little metal by melting a little solder to the tip of the reed and again tweaked with adding/removing until the note was right.

And hey presto, a semi-working concertina. All the notes there, all in tune. The only problem was that too much pressure in the bellows and they strained. Obviously the harmonica reeds were never intended for the pressure provided by a set of concertina bellows.

A sand down of the horrible red paint, some cardboard and glue to bodge the holes in the bellows, an old belt for some new hand straps and some decorative pyrography; it could almost pass as a usable instrument.

And this is where my interest died. The project was done for now. I could get a tune out of it and enjoyed the process and the bodges along the way and now it was working it did a good job of sitting on my shelf for a few years until it came to life again in another bodge project.


If you've stuck with me this far with the promise of an electric concertina, this is where it starts getting spicy.

We fast forward to June 2026 and I had just bought an electric piano. Something I'd been wanting to buy for many many years but never been able to justify it. Well now with the arrival of my daughter I could justify it under the pretense that we should have one in the house for her to learn on. Apparently this is the perfect time to drop an unreasonable amount of money on something I totally don't need. Anyway, before we get into that rabbit hole...where were we?

Yes, I bought the piano and while setting it up I noticed it could be used as a midi keyboard. Not something I've ever played with, but this took me into the magical world of midi, LMMS, Soundfonts and synthesizers. Another rabbit hole I could go down but for now lets stay on track.

I'd been playing around with this for a few weeks and out of the corner of my eye I spotted my concertina. Now I'd played it a few times and come to realise the harmonica reeds were not good enough to make this an enjoyable instrument to play but what if it could be plugged in and become a midi concertina.

The project was starting to take shape in my mind. Swap out valve opening keys for buttons, some kind of pressure sensor for the bellows. How hard could _that_ be? (turns out a bit).

But how to get the buttons to send midi messages, how to use a sensor to change the volume, expression, something else. Lots of questions and not many answers, but I had a completely unsupported confidence that I could make this work.

First I checked online to see if anyone had done a similar project and published their build to no avail. One of two videos surfaced with someone demoing an electric accordion but nothing about how it worked or how to even begin.

Now I've played around in the past with the raspberry Pi, many years ago, but I knew this could be an option. I'd wired up my doorbell my Raspberry pi around halloween to get it to play a spooky recording for the trick or treaters, but knew it was overkill for that project and for this one. So something smaller and ligther weight, and I'm sure I could do something similar but send a midi message/command for the button press instead of triggering a wav file to play.

Enter the microcontroller.

I found the Raspberry Pi Pico 2 online, and for about £5 it was a safe gamble. I ordered this and a set of 10 micro switches (I thought lets try doing one side of the concertina first as a proof of concept then we can expand). Once they arrived I delved into the world of embedded development, which with the Pico 2 was _WAY_ simpler than I'd imagined. In a previous life I worked with some very clever people in a small company making routers and access points. They handled the embedded development while I dealt with some very basic front-end development for the interface. I remember watching them flashing firmware, writing code in C and doing some absolute magic which at the time went way over my head. 

So coming into this I expected this to be a challenge, but finding I could flash firmware over USB, write in python using Circuit Python and test my code instantly on the Pico 2 it was a surprisingly easy.

Notes: (Circuit Python)[https://circuitpython.org/] - which is based on (MicroPython)[https://micropython.org/] but with some extra bits in for Adafruit's hardware. Perfect for my little project using the pico2.

Once I'd figured out how to write some simple firmware in CircuitPython and flash it, I took to wiring up some buttons to the GPIO pins and getting these to output some midi messages to my laptop. At this point I had the proof of concept, I had most of the bits I just needed it to fit into the concertina form factor, and then think about how I was going to adapt it to use the bellows.

I set about cutting up the valve plates on the inside of the concertina and placing the microswitches under the keys. This proved to be harder than I first envisioned, getting the microswitches in place and solid enough while not preventing full range of motion of the keys took some trial and error, and lots of cut plastic cutoffs as spacers, bent wire and small plates of metal to hold it all rigid. I wired all the ground pins together and soldered some cut off dupont wires onto each switch so I could plug them onto the GPIO pins of the pico2.

Side one done and tested I ordered the next 10 switches for the bass side and in the meantime played around with setting up different configuration options for my concertina.

So there are a few types of concertina. Anglo, English, Duet, Chemnitzer, all with varying numbers of keys, and tunings. What I had bought, with no prior knowlege of what a concertina really was, was an Anglo. With an anglo concertina each key plays two notes, one note on squeezing the bellows in and a different one on pulling them open. Obviously I couldn't replicate this currently without something to sense the bellows.

So my initial configuration was to make this a duet concertina, with the key playing the same note with bellows going in or out, and given that I had no bellows I just had press button = play a note.

The great thing is that given that everything is controlled by software there would be no reason I can't switch between Duet, Anglo, English, whatever. The Anglo also is tuned to a given key C/G, G/D or older C/F, which when you buy an anglo concertina restricts you to playing just songs in those keys, again being software based I can switch to whatever key I want.

I also wrote a script which determined the best approximated bass chord to play which would accompany the melody I was playing on the 10 keys I had. This was fun but I was really just exploring a little side shoot of the rabbit warren I was already miles deep into while I awaited the remaining switches.

Anyway, my other switches eventually arrived and I wired them up the same way and put it all together.

With my playing around with the auto-bass chords I found I really liked being able to easily put chords to the melody I was playing in the right hand, and why not. This isn't the right setup for a real concertina, but this isn't a real concertina. It's my own thing and its whatever i want it to be. So I set it up such that the left 10 keys played chords in the same key as whatever I configure the right hand to be, a bit more like an accordion.

I also set the left hand to be on one midi channel and the right on another so I could play the bass chords in one soundfont and the melody in another.

So finally, I could play a tune, simply passing these midi messages into a soundfont midi player on my laptop. I could choose different soundfonts, something close to a real concertina or something completely different. I particulartly liked the juxtaposition of playing a giant pipe organ on a tiny little concertina.

But it still sounded a bit flat and more like a synthesized concertina and not a real one. What was missing was the bellows. After a bit of research I found a MPXV7002DP MPXV7002 Piezoresistive Transducer Differential Pressure Sensor - the perfect candidate. It basically senses the pressure differential between its two input tubes. I could have one tube going outside the concertina for the atmospheric pressure and one for inside the concertina for the inside pressure. I could check then how much pressure was in the bellows and whether I was pushing or pulling the bellows, and this switch the note (e.g. on the Anglo concertina), and crucially change the expression or volume based on how much pressure was in the bellows.

This took quite a bit of trial and error. First few attempts were wildly out and needed some tuning to get it to do what I wanted but in the end I got there.

Despite getting an accurate replication of an Anglo concertina, with the notes switching based on whether pushing or pulling I found I preferred playing with my unisonoric setup and full chords in the left hand. 

A final addition I made was when the pressure reached the top 10% I wanted it to sound like it was straining, so I decided if the pressure reached this 10% it would open some new midi channels with a new soundfont for the strained version, and the volume/expression increasing as the pressure increased as with the first channels.

I could also use this to play other sound fonts just for fun. For example I played quietly and calmy with a concertina soundfont but if I really go for it with the higher pressure bellows we merge in some pipe organ, or have a guitar  soundfont and turn on overdrive for the higher pressure.

So it was done. At least for now. 

I could play any song I want, in any key, with any soundfont, and it sounded close enough to a real concertina for my use as a fun practice instrument.

I have plans to integreate the actual synthesizer into the body of the concertina, with a little speaker and an integrated battery, maybe using a raspberry pi for the synth. But that's a project for another time. I've bodged this concertina enough, and I've had enough fun with this project to call it a day and commit it to my blog. 









