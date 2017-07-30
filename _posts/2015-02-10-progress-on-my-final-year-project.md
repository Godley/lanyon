---
title: Progress on my Final Year Project
layout: post
---


![]({{ site.url }}/images/2017/7/nowplaying.png)

It feels like I haven't written anything substantial for quite a while: surprisingly, cutting all my hobbies out of my life makes me a bit of a dull person.
At the minute I'm focussing on my degree, and that means I can't go to events as often or work on electronics projects so much (though my current one is quite fun. See element14 in a month or so's time for updates on that)

Anyway, all my non-exam addled energy has been going into my final year project (and maybe, y'know, watching some netflix...), of which I've achieved creating a testcase in MuseScore, extracting objects from the output and pulling it in to recreate the testcase as a PDF. Yes, that means I've literally achieved what loads of other programs have - drawn sheet music, and not much else.
In all seriousness, I've made a fair amount of progress, and this is essentially the baseline of my project. Without the ability to draw sheet music, my project is really just a program that tells you a bunch of stuff about a piece. Which you could already tell by reading the music...

Now that I'm close to having nicely drawn sheet music from objects I designed myself, I'm looking at phase 2: redoing my work on metadata parsing. I say redoing because the current phase I have done extensively, in so much that I have around about 2.1k unit tests and, aside from my lack of knowledge on lilypond syntax organisation, I'm fairly confident it's a solid bit of code. 

My metadata parser on the other hand is slightly less elegant: it currently uses a dictionary of lists, which are either organised by the value found or the type the value is. 

So for example, if I use the first method, my parser might return:
`{"treble":[["clef","accidentals.xml"],["clef","lines.xml"]],"4/4":[["meter","accidentals.xml"]]}`
for a given folder, which tells me I have 2 files, both of which are in treble clef and one of which is in 4/4 time.
This works ok, but I'm starting to look at what else I could include in the parsing algorithm, and how I could make it more sophisticated. One thing that I'm considering is **difficulty**.

When a performer looks at a piece, s/he will generally consider whether s/he can play it or not as an important factor on whether to bother trying it out. Some factors in this are common to a lot of musicians: chiefly, complex sequences of notes. 


Personally, if I see a lot of semi quavers in a clarinet piece (or god forbid anything shorter than a semi quaver), or a wide range of chords and cross rhythms in a piano piece, I will probably think "hahahaha, no.". 

<center>![]({{ site.url }}/images/2017/7/?format=1000w)</center>
There's other things to consider on clarinet, too, like where a certain sequence of note requires a specific organisation of your fingers, or on piano how far apart notes are spread and whether your hands can physically make that distance.

This particular measurement varies per instrument - for example, brass players control a lot of the sound frequency elements using their lips and mouth, so if a piece jumps around in terms of relative sound "highness" or "lowness", they're probably going to struggle a bit; and per performer - this is of particular relevance to singers, because whilst vocal training can improve your range, for instance, there are natural limits on what your voicebox can achieve. Naturally, other things to contemplate are how much practice the player or singer has had and how much experience.

All things considered, if I have the time I will be attempting some kind of algorithm to attempt this, and have put together a survey to gauge how other performers rank their music: [click](https://www.surveymonkey.com/s/JBSJ9XV). Any musicians reading this, please do submit your own input!