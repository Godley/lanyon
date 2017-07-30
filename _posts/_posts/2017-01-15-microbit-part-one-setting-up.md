---
title: Microbit part one - setting up
layout: post
---
![]({{ site.url }}/images/2017/7/fc11b345-8a63-466d-9301-ab99f66a5aa8.jpg)
[Project github repo](https://github.com/godley/microbitwerewolf)

Today I started to set up to write some code for my microbit. After initially doing some googling - see [here](http://microbit-micropython.readthedocs.io/en/latest/radio.html) - I found the library for radio is pretty straight forward.

My next step was thinking about how exactly to go about writing for the microbit. Having worked with embedded systems in my day job, I know constrained environments make it difficult to test your implementation, and the setup cost of doing it can take a toll on how long the task takes, so I decided to start by building it for a mac/pytest before porting it to microbit.

## Step 1: game fundamentals + testing
At the bottom of this game realistically is just a finite state machine. In real life, there's 8 states:

1. Setup the game
1. Have the wolves vote who to kill
1. Have the doctor vote who to heal
1. Have the seer pick who to ask about
1. Transition to day, make announcements
1. Have the people vote who to lynch
1. Kill whoever has been lynched
1. End the game

My first thoughts were "I know, I'll implement my own state machine".
I then thought of how much that was a dumb and daunting task, and went to find other better implementations and settled on [transitions](https://github.com/tyarkoni/transitions) which seemed like what I wanted.

I set up my controller and started with state 1 - setup the game. In this instance the controller's going to randomly assign everyone a role, so all I needed to do was register how many microbits were playing. For the most part I wanted the radio comms to be completely decoupled from the actual gameplay, so I settled on 3 reasonably testable methods:

1. Add a player using a method which puts it onto a queue
1. Get the player using a function which gets it from the queue
1. Have a blocking method which gets the player, checks it's not a duplicate and puts it onto the player list.

I went with this approach because:

1. It's reasonably easy to test. I can check add player and get player work by well...adding then getting. The blocking method I parameterised the length for which it blocks before going to the next state, so the test can just set the block to a shorter amount of time than in game.
1. I can add the radio bit later and just have the handler call add
1. If it's not possible to do asynchronous stuff with microbit (I strongly suspect it's not capable) I can replace or else make a child class of the controller and change the get method to a blocking call to check the radio.

A lot of the rest of the game will probably need a similar approach to this as it's a lot about voting, so I probably won't go into this much detail with later phases of development.

As far as testing goes my test library of choice right now is pytest because it's so easy to pick up and keep everything neat and tidy. I highly recommend it.