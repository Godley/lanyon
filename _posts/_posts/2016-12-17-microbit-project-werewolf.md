---
title: Microbit Project: werewolf
layout: post
---
![]({{ site.url }}/images/2016/12/p02wfdqd.jpg)
Since I'm moving on jobs I have about 2 weeks of time to chill out and transition at the end of January. For part of that time I'm jetting off with Danny, but part of it I'm intending on writing and making videos for the Microbit project after some discussions with [David](http://blog.whaleygeek.co.uk).

Given the microbit has a radio on it, I was particularly interested in working with microbit to microbit communications. I felt a good application of this was team games and in particular, the game [Wolverine](http://www.playwerewolf.co/rules/) struck me as an interesting one. Loosely there would be 5 different UI expectations:

- The moderator interface, which should allow the elected moderator to progress the game from day to night etc.
- The werewolf interface, which identifies to the werewolves which players are werewolves, and allows them to vote for a player to kill
- the healer/doctor interface which again allows them to save a player
- the seer interface which allows them to vote for a person to check their status
- the villager interface which allows the villagers to vote on who to lynch

Each microbit would potentially hold all the classes for this, the moderator microbit would communicate to the other microbits which roles they have.

Given a lot of this is based on voting, I would expect I might need to make an expansion board with maybe a potentiometer to let users scroll through the other players. I could also envisage just using the buttons as up and down arrows, allowing the user to go through the player numbers. I'm expecting numbers would be better than player names because the display is just an LED matrix so long names would be a pain to read and scroll through.

It might also be possible to have an automated moderator which is a computer using the same radio. A user would configure it to give it how many players there are and it would then assign who's who. All user feedback would then be through the microbit rather than having any symbolic feedback from a human, meaning more people could play the game.

What I'd like most though is to build this up bit by bit - given the rules of the game are set, it gives me a pretty clear picture of the requirements and therefore it should be easy to systematically work through the roles and communication requirements. I need to confirm the automated moderator part with David, though. Not entirely sure what radio is on the device and therefore whether I could put a raspberry pi onto the task of being the moderator. I guess there's no reason why you couldn't just have a microbit on auto mode doing the job for you if not.

Looking forward to getting going with it but I might have to beg/borrow/steal more microbits than I've been offered if I'm going to test this...I also need to work on how I'm going to film it as previous videos were done by pros of which I'm certainly not when it comes to youtube.

Expect more on this in the new year!