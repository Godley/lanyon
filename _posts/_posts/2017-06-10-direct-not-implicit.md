---
title: Explicit not Implicit
layout: post
---
A repeated complaint of mine since joining a new company, one which heavily relies on a task board, is that we have a lot of tasks.
Some of them are old, most of them are ill-defined, and a lot of them are just notes of what people have felt they should be doing and more dangerously, notes people felt they *would* be doing not anyone else in the team.

At task and ticket level this is bad, but it becomes much worse if this is a feature of your development and deployment process. I brought down an entire service at work because I thought having a variable `DOCKER_REPO` in your environment template meant this variable would come from whatever environment sent a new deployment request, not the environment you were deploying into. Having had that explained to me I asked if the "feature" was documented anywhere, knowing what the answer would be.

The problem of being implicit rather than direct is a large part of why my writing style is the way it is when it comes to tutorials and technical instruction. If your tutorial is reliant upon some other thing the user should have done, such as set up their wifi with a static IP or installed x package, you shouldn't have to fully explain how to do that step, but you should provide a *explicit* source of where they can go to get that information. Assuming makes an ass out of you and your reader and is your responsibility to avoid as a writer, maintainer and leader. 

What happens if you leave it up to implicit or assumed knowledge?

1. Your reader blames themselves, or worse, you blame them if they don't know it or thought something different to what you had in mind. 
1. Your reader has to duplicate a lot of the work you put in to pick out that knowledge. Bare in mind your reader could later be you if it's been a long time since you reported the issue, made the process or wrote the project. Second reason why I write my tutorials to assume the person reading them knows absolutely nothing - that person could one day be me, again.
1. Your reader causes serious problems or technical debt for you and your team later which it could take a while to fix or recover from.

Avoiding the trap:

1. Write your processes, code and documentation in line with each other. If something changes or someone reports that your documentation isn't clear, *stop*. **Improve your documentation.**
1. Take a few minutes more than you normally would to create issues, and think of what information lead you to discover the problem and how you were maybe thinking about fixing it later. If your least experienced team member doesn't understand the issue or any element of it could be left up to assumptions and guesswork of what exactly you wanted, you haven't written the issue right. Sure, you could put a step in your process being talking to your team about the issue and I often do that, but in an asynchronous team or one in which people are working in lots of different contexts and tasks, it can be hard to find the time and get people to context switch. Better to get into the habit of writing explicitly
1. Write as you learn. It's hard to get to the point where you know everything and remember exactly where you learned the thing from.