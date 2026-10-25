+++
title = "Turning a Concertina into a MIDI Instrument"
date = 2026-10-23T11:00:00-07:00
draft = false
description = "Replacing the concertina's leaky musical innards with micro switches, a Raspberry Pi Pico 2 and MIDI."
tags = ["concertina", "MIDI", "electronics"]
categories = ["Projects"]
+++

In the first part, I bought a battered £10 concertina, repaired its missing reeds with pieces of a charity-shop harmonica, and got it playing well enough to sit on a shelf. It stayed there until June 2026, when I bought an electric piano and wandered into the world of MIDI.

I'd wanted an electric piano for many, many years but never been able to justify it. With the arrival of my daughter, I could now justify it under the pretence that we should have one in the house for her to learn on. Apparently becoming a new parent is the perfect time to drop an unreasonable amount of money on something I totally don't need.

While setting it up, I noticed the piano could be used as a MIDI keyboard. I'd heard of MIDI and knew roughly what it was, but this discovery took me into the magical world of LMMS, soundfonts and synthesisers. Another deep rabbit hole I need , but let's stay on track.

After a few weeks of playing around with this I stumbled back on my bodged concertina. Now what if _that_ could be plugged in and become a MIDI concertina? Is that something that already existed, had anyone done it before. An idea for a new bodge was brewing.

The plan: swap out the valve-opening keys for micro switches and add some kind of pressure sensor for the bellows. How hard could *that* be? (Turns out, a lot)

How to get the buttons to send MIDI messages? How to use a sensor to change the volume, expression, something else? Lots of questions and not many answers, but I had a completely unsupported confidence that I could make this work.

I checked online for similar projects, but found only one or two videos demonstrating some home-made electric accordions, with nothing about how they worked or how to begin. 

Now I'd played around with the Raspberry Pi years ago, wiring up my doorbell to play a spooky recording for trick-or-treaters, and this seemed like a reasonable place to start, but I knew it was overkill. I wanted something smaller and lighter, which could simply send a MIDI message for a button press instead of triggering a WAV file.

Enter the microcontroller.

I found the [Raspberry Pi Pico 2 online](https://www.raspberrypi.com/products/raspberry-pi-pico-2/), and for about £5 I thought it was a pretty safe gamble. I ordered one along with 10 microswitches to try one side of the concertina as a proof of concept. In a previous life, I worked with some extremely clever people making routers and access points. They handled the embedded development while I bumbled along with the basic front-end interface in vanilla JS. I remember watching them flashing firmware and writing C, doing absolute magic which went way over my head.

I expected this to be a challenge, flash firmware for the Pico 2 can be done simply over USB, the firmware written in Python using CircuitPython and code can be tested instantly on the Pico 2. Surprisingly easy.

![The Raspberry Pi Pico 2 wired up to the concertina keys.](wiring-up-the-pico-2.jpg)


So what is this [CircuitPython](https://circuitpython.org/)? I'd worked with Python before, and once tried to do a year of [Advent Of Code](https://adventofcode.com/) so I recognised the structure. It turns out CircuitPython is a lightweight version of Python for microcontrollers, originally based on [MicroPython](https://micropython.org/), with a particular focus on making it easy to interact with hardware. Perfect for my little project using the Pico 2.

Once I'd written and flashed some simple firmware, I wired buttons to the GPIO pins and got them sending MIDI messages to my laptop. The proof of concept worked. Now I just needed to fit everything into the concertina and work out how to use the bellows.

The useful bit of the firmware was a small wrapper around the MIDI library. A button press starts a note; releasing it stops the note again:

```python
def play_note(midi, note, velocity=100):
    midi.send(NoteOn(note, velocity))

def stop_note(midi, note):
    midi.send(NoteOff(note, 0))
```

I cut up the valve plates inside the concertina and placed the microswitches under the keys. Getting them secure without restricting the keys took trial and error, plus plastic offcuts for spacers, bent wire and small metal plates. I wired the ground pins together and soldered cut-off Dupont wires to each switch to plug into the Pico 2.

![Microswitches fitted beneath the concertina buttons.](setting-up-the-button-plate.jpg)

Side one done and tested, I ordered the next 10 switches for the bass side and, in the meantime, played around with setting up different configuration options for my concertina.

While looking into what notes should be where I discovered there are a few types of concertina: Anglo, English, Duet, Chemnitzer, all with varying keys and tunings. It turns out I'd bought an Anglo. Each key plays two notes, depending on whether you push or pull the bellows. Obviously, I couldn't replicate that without a bellows sensor.

So I initially configured it instead as a Duet concertina, with each key playing the same note in or out. Without a bellows sensor, it was simply: press button = play a note.

Because everything is controlled by software, I _could_ switch between Duet, Anglo, English, whatever. An Anglo concertina is tuned to a given key, such as C/G, G/D or the older C/F, but again, everything being controlled by software meant I could easily switch to any key I wanted.

In practice, that meant choosing a different root note before building the button-to-note map. The real firmware has a `ROOT_NOTE` setting; in a simplified version, I could select it from a little lookup:

```python
key = "D"
root_notes = {"C": 60, "D": 62, "F": 65}  # MIDI note numbers
root_note = root_notes[key]
```

While I waited for the remaining switches, I wrote a script to find the best-approximated bass chord to go with the melody from my 10 keys. A small side shoot of the rabbit warren I was already miles deep into.

Anyway, my other switches eventually arrived and I wired them up the same way and put it all together.

![The microswitches and wiring laid out during the build.](micro-switches.jpg)

I found I liked adding chords to the melody, so I set the left 10 keys to play chords in the same key as the right hand, a bit more like an accordion. This isn't the right setup for a real concertina, but this isn't a real concertina. It's my own thing and it's whatever I want it to be.

In the simplest version, each left-hand button gives me a root note, and I build a chord from that: the root, a fifth above it, and the root again an octave higher.

```python
def chord_notes(root):
    return (root, root + 7, root + 12)

for note in chord_notes(button_root):
    midi.send(NoteOn(note, 70))
```

I also set the left hand to be on one MIDI channel and the right on another so I could play the bass chords in one soundfont and the melody in another.

The MIDI channel is just another setting on the output:

```python
treble = adafruit_midi.MIDI(midi_out=usb_midi.ports[1], out_channel=0)
bass = adafruit_midi.MIDI(midi_out=usb_midi.ports[1], out_channel=1)
```

Finally, I could play a tune by passing the MIDI messages into a soundfont player on my laptop. I could choose something close to a real concertina or something completely different. I particularly liked playing a giant pipe organ on a tiny little concertina.

By this point I'd accidentally learnt quite a lot. I'd gone from vaguely knowing that microcontrollers existed to programming a Pico 2, wiring GPIO pins, sending MIDI messages, using different MIDI channels, building chords from note numbers and learning far more about the various species of concertina than I had any reason to know. I'd also discovered something I rather liked about putting software inside an instrument: the rules suddenly became optional. I didn't have to build a proper Anglo, English or Duet concertina. I could build the increasingly incorrect concertina that I actually wanted to play.

It worked, but the result still sounded a bit flat: more like a synthesised concertina than a real one, and I still could not replicate the true Anglo behaviour. What was missing was the bellows. [In the final part](../electric-concertina-bellows/), I add a pressure sensor and find out what happens when an instrument can finally breathe.
