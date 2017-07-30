---
title: Refactor #20000: how to organise muh notes
layout: post
---
I preface this blog with a warning that I am mostly writing this to get my head around or find a data type to fit my latest problem, and thus you will probably find it boring.

There's been several points during the last few days where I've suddenly had a wakeup call and gone "NO, NO I CAN'T DO IT THIS WAY ANYMORE". Some of these have been where I've realised my coding looks messy and anyone else reading it would go "seriously?", but most of them have been my own oversights from not knowing enough about the two formats I'm converting from and to.

![]({{ site.url }}/images/2017/7/Paris_Tuileries_Garden_Facepalm_statue.jpg)

The latest is note organisation - I started this project with a piece class containing a list of part classes. Each part class had a list of measures, and each measure had a list of notes and a list of miscellanious items (anything that's not a note, like text directions and such). A la whiteboard:

![]({{ site.url }}/images/2015/02/IMG_20150217_182329.jpg)

I then found that MusicXML doesn't have indexing beyond measure and part - that is to say, all the stuff inside a measure tag is sequential in terms of where it comes in the bar. In the given class arrangement, as items and notes are both lists, items would not be in the right place because they don't have an awareness of where they should be in the bar after being loaded, so I reorganised and now have:
1 list of notes - these can stay sequential because they represent the movement of the measure through time, and thus are how we index everything else
1 dictionary of lists of directions - these are indexed according to what the last note was before they were loaded.
1 dictionary of lists of expressions - these are again indexed according to what the last note was. Expressions are separated from directions because of the nature of how lilypond handles these items - for example, dynamics have to come after a note, not a direction. I give you more whiteboard drawing:

![]({{ site.url }}/images/2015/02/IMG_20150217_182504.jpg)
Up until today, this was doing fine. Then I discovered forward and backup tags were things I had to pay attention to.
In musicXML, not only is stuff dumped in the bar sequentially, but the developer or software writing the xml can move forward and backward in time using these tags. 

This is kind of fine with backup, because I know what notes have come before this one, so I can just go find how much to move back by, and pop the note in where I need to in time. Forward? Uhm. Yeah, here's my conundrum.

I'm now thinking of organising my notes "list" in a different way: what I want is a time slicing type, so basically we would index all the notes by how long they are, and then leave a gap where a forward comes in or push the previous notes forward a section when the backup comes in. In my fuzzy whiteboard drawing:
![]({{ site.url }}/images/2017/7/24xqaec.jpg)

Things to consider in this:
    
   - data structure to use: I guess a stack could do the trick, though this would take a lot of pushing and popping to move around. I considered subdividing a dictionary (i.e top layer index: 1,2,3,4; then haf etc) but this won't work for all notes because you can have a note that goes over the beat, and on top of that would require including meter into the consideration and it'd probably be pretty slow. Maybe a linked list?
    
   - algorithms: how do we quickly find where to point the time pointer?
   
   - all the other things: how do we organise those if notes is no longer a list and are now organised by time?
   
I need to look into what's the best plan for this and probably bat some ideas off my supervisor in my meeting on thursday, but right now this is both an interesting problem and a pain in the butt.
Oh, and then I need to figure out how Mxml notates percent repeats, because that's what I thought "forward" was.