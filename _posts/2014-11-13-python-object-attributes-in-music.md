---
title: Python Object Attributes in music
layout: post
---
Before embarking on this project, like a lot of students, I thought my research area was easy and simple.
Haha. Now I'm wondering how I ever learned to read music, to be quite honest.
Nonetheless, I noticed an interesting implementation in a [music object library](http://web.mit.edu/music21/) I was perusing today using a pecular feature of python - it's implementation of object attributes.

Something that several of my fellow developers, myself included, scoff at, is that python allows people to set an attribute from outside of the class, so we could have:

`	
class Charlotte(object):
        def __init__(self):    
                self.surname='godley'
`
and then instantiate it:
`me = charlotte()
me.firstname='charlotte'
c = charlotte()`
Me now has the firstname "charlotte", but c doesn't have this attribute, so if we then messed up and forgot this later, and tried to ask c for the attribute "firstname", this could cause problems.

However, musically this is actually a useful thing: a note, or a stave, or a measure, can have many attributes which are optional. Actually, almost everything about music is optional, except perhaps a clef and the stavelines themselves.

For example, a note could have a dynamic of "f", (forte, or loud), an articulation dot indicating it be played staccato/very short, and it could be slurred to the next note.
Or...it could have none of those things and still be a valid note, and there would be no notation indicating they weren't there. 
In most other object oriented languages, you'd need to indicate this by setting up a variable holding all that stuff, or a list, and set it to empty. In python? Not necessary. 

Still, the problem above could exist - I have yet to get to the part about reading the attributes for rendering or saving out to musicXML, I just figured I'd point out something interesting.