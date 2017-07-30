---
title: The start of my dissertation
layout: post
---
The past few weeks I've had the following things to do:

*  **Virtual environments coursework:** this entailed taking a template of a scene with a pair of tracked glasses floating in it, a cube and a floor plane, and transforming it using equations to make it so that you saw through the tracked glasses, staring at the cube which had 3D stereoscopic vision in place to make it look 3D. Lots of fun once I understood the brief, but like nothing I've ever done before.

*  **Languages and Compilers coursework:** this one is still ongoing, but for this module we have to write a Simple Programming Language compiler program in C. Some people moan about this one because it's hard, but I love a challenge and the brief and teaching for this module is top notch.

* **Multiple interview processes:** as I'm graduating next year an external pressure has been finding jobs and companies to interview with. So far I think this is going well, and having GHC to start the ball rolling with handing people CVs, doing interviews and waiting for feedback helped to make this feel much more normal. Hopefully I'll have found the right job by the end of all this and can forget about it until graduation. 

* **Element14 project work:** I've been working with the Raspberry Pi to upgrade my Pi Passport project from what it was to having a competition element, ready for the **Element14 Electronica '14** stand this Friday in **Munich, Germany**. This is my first trade show and it should be pretty exciting, so if you fancy a shot at winning some swag, come visit!

Finally, now that Virtual Environments is out of the way and my projects for Element14 are ready, I've started doing some work on my dissertation project, **a Sheet Music Organisation system**. In this I have 3 main objectives:

* Rendering sheet music from MusicXML 

* Searching sheet music using various filters

* integration of online score libraries such as the [International Music Score Library Project](http://imslp.org)

I'm still in the early stages of research, but this last week I started to implement something called **SAX** for XML (very fitting, considering the topic area).

Essentially, instead of using the standard XML processing libraries which take in the entire document, and let you search that memory according to it's tag, or add certain attributes which can be fiddly, SAX allows you to read in the document line by line, and fires off events each time:

* a tag starts

* a tag's characters begin

* a tag ends

I think there's some other methods we can overload, but those are the most important ones. This is much easier to use than for example, DOM and MINIDOM which are two built in libraries for XML processing in Python, as I need maybe 3 or 4 of the tags in the musicXML - at the moment, I'm handling composer, title and instruments - when there could be 40 or 50 tags in the document itself. 

If I can tell it to only track and handle the tags I need, rather than tracking everything and searching through it, then this algorithmically makes the program easier to understand, particularly as in MusicXML these 3 simple things can be nested which would take a bit of digging around in DOM, whereas in SAX this can be done by a simple if statement in the tag-start method to check it against the filters.

Another library I've used in this part of the project is **pickle**. Previously in C# I found out about class serialization and deserialisation - essentially, this allows you to dump your objects to a file, in a format that's almost completely unreadable to a human, but the program can unserlialise to recreate your object in your program exactly as it was. This took several lines in C#, but in Python is as easy as:

`import pickle
my_list = ["hello","world"]
file_to_write = open("my_obj",'w')
pickle.dumps(my_list,file_to_write)`

For serialising and:

`import pickle
file_to_read = open("my_obj",'r')
my_list = pickle.load(file_to_read)`

for unserialising. I did this so that when it comes to searching, I can just pick it out of the unserialised list if it's in there: whether this is memory efficent, *shrug*. Other option of course would be to just write it to a text file, but this would then take list recreation and iteration over the lines until they're all loaded. 

Nonetheless, figured I'd mention some new libraries I've been using.

My next topic is to look more indepth into rendering sheet music: so far I've done the stave lines calculations, aka: how many will fit on a page, calculating how many bars I'll need based on time sigs and note durations, calculating how wide the bars are based on note spacing, calculating how many staves I'll need based on how...yeah. that took an entire sheet of paper to write the entire algorithm just for those calculations out, so I may have to take a different tact to avoid reinventing the wheel. 

The trouble is, I don't want to lose this objective's marks because of taking someone else's ideas, but is there really a point in writing an entire rendering algorithm for sheet music when MuseScore and LilyPond have done all of this 100% x how-many-contributors-they-have better than I could ever imagine?
