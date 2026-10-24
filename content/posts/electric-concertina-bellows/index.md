+++
title = "Putting the Bellows Back into the Concertina"
date = 2026-10-23T11:00:00-07:00
draft = false
description = "Adding pressure sensing, expression and a little bit of pipe organ to my homemade MIDI concertina."
tags = ["concertina", "MIDI", "electronics"]
categories = ["Projects"]
+++

In the previous part, I replaced the concertina's keys with microswitches and used a Raspberry Pi Pico 2 to send MIDI notes to my laptop. It played melodies and chords, but something important was missing: the bellows. Without them, there was no pushing or pulling to change the notes, and no way to control how hard the instrument was being played.

After a bit of research I found an MPXV7002DP piezoresistive transducer differential pressure sensor: the perfect candidate. It basically senses the pressure difference between its two input tubes. I could have one tube going outside the concertina for atmospheric pressure and one inside for the pressure in the bellows. I could then check how much pressure was in the bellows and whether I was pushing or pulling, switch the note (as on the Anglo concertina), and, crucially, change the expression or volume based on how much pressure was in the bellows.

![The concertina opened up, with the Pico, pressure sensor and a rather enthusiastic amount of wiring inside.](full-wiring-mess.jpg)

This took quite a bit of trial and error. The first few attempts were wildly out and needed some tuning to get it to do what I wanted, but in the end I got there.

I also found that the holes I'd cut for the switches were now severley leaking air from the bellows. I needed something to plug the gap, and settled on using some silicone. This wasn't enough so applied a heaftly layer of cling film and tape around the buttons, and finally it was air tight.

Despite getting an accurate replication of an Anglo concertina, with the notes switching based on whether I was pushing or pulling, I found I preferred playing with my unisonoric setup and full chords in the left hand.

A final addition I made was to have it sound like it was straining when the pressure reached the top 10%. I decided that once the pressure reached this 10%, it would open some new MIDI channels with a new soundfont for the strained version, with the volume and expression increasing as the pressure increased, as with the first channels.

I could also use this to play other soundfonts just for fun. For example, I played quietly and calmly with a concertina soundfont, but if I really go for it with the higher-pressure bellows, we merge in some pipe organ. Or I can have a guitar soundfont and turn on overdrive for the higher pressure.

So it was done. At least for now.

I could play any song I wanted, in any key, with any soundfont, and it sounded close enough to a real concertina for use as a fun practice instrument. I have plans to integrate the actual synthesiser into the body of the concertina, with a little speaker and an integrated battery, maybe using a Raspberry Pi for the synth. But that's a project for another time. I've bodged this concertina enough, and I've had enough fun with this project to call it a day and commit it to my blog.

The main lesson is that I didn't need to restore the instrument into something it used to be. Making the bellows and buttons talk to a computer let me turn it into something else entirely, and the software meant I could change my mind about how it should work along the way. [The first part](../electric-concertina/) is where the whole thing started; [the middle part](../electric-concertina-midi/) covers the electronics and MIDI setup.
