---
title: Making for makers
layout: post
---
A friend from the Raspberry Pi community makes it well known that in his company, "documentation is considered too important to leave to developers", and as I've hacked my way through the past year of writing and recording coding tutorials, I can completely see why, and that's shaped a lot of the code I now write.

When I first started cracking my way into code for embedded systems, I stuck with the Pi. Why? My first device, it's comfortable and it does most of the stuff I need. Aside from the community around it, until I veared away to write various tutorials, I thought this was the only reason.
Now I find that the world outside of Rpi - for makers who don't always feel comfortable playing with the really low level bit shifting, register writing embedded C that the professionals swear by - is a scary place.

The problem that I think is most prominent is companies producing boards which "are arduino compatible", but beyond sticking an R3 header, there's little to no examples and little to no API documentation. I've written for a few different boards in the past 12 months, and I found with all of them excluding the ones that I'd already used, the steps to getting from installation to hacking hardware were long and arduous. The pathways assume that you know what you're doing or that you know where to download x y and z, or that if an error pops up you'll know it's because you're missing w which is downloaded from another source.
It's the most frustrating thing when companies do this: I honestly don't understand what's the point in smacking R3 headers onto the board if you're going to continue down the same trajectory towards business customers being your main supported use case. It'll probably bring in a few people to your flock I guess, but they'll quickly turn the thing into a paperweight unless they know what they're doing. Which I freely admit I don't, around 99% of the time.

Open source libraries written by random people frustrate me, too, but in different ways, and I thought I'd list some things that have annoyed me during this experience and ways to avoid them:

 - writing libraries in C for a Raspberry Pi and it being a total mess no one except a pro can properly understand. I initially left this at just "writing libraries in C for a Raspberry Pi", but then realised wiringPi and various other things exist and they're sort of alright to work with. Personally I just find that as a writer and a maker, I would rather work with a Python library because explanation needed is minimal, and a long section of C code can be quite scary to read. I'd advise either giving very clear instructions or else wrapping in Python. In particular it bugs me when people who've made hardware specifically for the Pi and even more specifically, for the beginner user, do this, particularly if all they provide is an undocumented example and just expect people to get it from that. Come. On.
 
 - libraries where I have to read long comments at the beginning of a .h file to find how to use your library. If it's on github, or anywhere else for that matter, make a readme. And make sure you include stuff about installation and any errors people might get when compiling, and how to avoid them.
 
 - libraries where I have to change something to make it work for a specific hardware, but I have no idea what. This should be included in your readme.
 
 - libraries where the installation instructions assume I know what I'm doing. I'M A HACKER (unless you're from the NSA or GCHQ. in that case, nothing to see here). As if I know what I'm doing. I can appreciate to some extent people wanting to avoid assuming another user is an idiot, but at the same time, I would rather you explain everything to me than you assume everything is obvious. Try to see it from someone else's perspective - is that dependency you assume all systems will have pre-installed obvious to everyone, or just you? If you were picking up x language for the very first time, would you have been explained the dependency before reading your library?
