---
title: An update on Refactor #2000 (and refactor 2001)
layout: post
---
In a previous blog post I hashed out my thoughts on my current system, why I needed to change it and what I thought I should change it to.
At that point my thoughts were going along the route of what kinds of structures I'd used back in Computer Science first year, and I immediately jumped for a linked list - why? Because I was looking at it from the point of view that I'd do my notes as a completely separate entity to everything else.
As I started creating unit tests for a python implementation of the idea, I quickly realised as I was indexing everything by notes, the idea of keeping things separate was dumb.
I'm not in the lab right now, so I can't draw a new whiteboarded model, but loosely, my structure is now:
Piece
Piece has part children
Part has staff children
staff has measure children
measures have voice children
voice has note children
notes have 2-3 children which can either be notes, expressions or directions.

Each node still has it's "item" which are the classes I made before - this means that where a thing has an attribute that doesn't need to be organised into a tree structure, I put the stuff inside the item to keep it tidy, and also so that I didn't have to make too many changes to the underlying tests or code.

I started this off by implementing the basic structure of a tree, but specifying that a node could only add a child if it had specific rules attached that stipulated the type of child node and they matched the type of child we're trying to add. So for example in my model, if you called AddNode on piece and gave it a measure node, it would go looking for any of it's children and the children of it's children for where this could be added.
Whilst I quite liked this idea, and in principle I used it, I ended up not implementing a lot of the cool stuff I did on that level. It was still interesting to work with, but some of the functionality either wasn't necessary or I just decided to go down another route.

To solve the timing issue of jumping forward and backward, I now have the measure know where it is in time by holding it in a variable: every time we jump forward, increment it and put in a spacer if need be. Every time we move backward, decrement it. Every time we add a new note, increment it.

A change I made quite recently after implementing this was allowing notes to have children that are also notes: I quite like this way of modelling, but there's only 1 case in which this is used, and once I figured out that it would be a good idea it seemed ridiculous I'd not used this before: chords.
Most people probably know what a chord is even without knowing a lot about music (2 notes of the same length played at the same time but with different pitches). When I first started this project I looked at the MusicXML implementation of it, and it seemed a bit weird.
In essence, here is a pair of notes which form a chord:
```
<note>
	<pitch>
    	<step>C</step>
        <octave>1</octave>
    </pitch>
</note>

<note>
	<chord/>
    <pitch>
    	<step>A</step>
        <octave>1</octave>
    </pitch>
</note>
```
What seemed odd to me was that note 1 doesn't know that it's part of a chord, so what I did was cycle through every note checking for a chord, and then rewind a note and tell it that it's the start of the chord.
Now that I have the tree structure in place, when I hit a chord it just gets added at note level rather than measure level, and when I hit the first note of a chord, I just check the first child (which if I've added a note to it, will always be a Note, not an Expression or Direction) for a Note - if there is one and the current note is not marked as a chord, it's now the start of the chord so add the appropriate notation. If the current note is a chord, do nothing, and if the first child of the current note is empty or isn't a note, we're at the end of a chord.
Simples.
I should note that I stumbled upon this idea kind of by accident - Lilypond defines that you can't add directions or markups to individual noteheads of a chord, so I took a step back and looked at my structure. This solves the problem because now when a note is added to the note, the indexer of where we are in time inside the measure isn't updated, so the measure adds any directions attached to it to the top of the chord note chain.
