---
title: Using Github effectively on a self project
layout: post
---
Recently I've spent around 3 or so months solidly working on my Final Year Project. It's quite interesting seeing certain styles of my own development change and develop over time. 
Most obviously, the changes in how I work on this fall into 2 categories, 1 of which is the way I use source code control.

![]({{ site.url }}/images/2015/02/Screen-Shot-2015-02-24-at-22-24-23.png)
<center>*muh github*</center>

One of the many good habits I got into during my industrial placement was actually using source code control, which is incredibly important, but progressively I've actually used it to track my own progress. My workflow started off as simply using TDD - i.e creating unit tests, creating code/red green refactor etc and then committing when I felt I had a big enough section of code to submit.

More recently I found out how useful issues are in this process, and how clever github is as a project management system - in particular, the ability to reference issues from inside commit messages and commits from issue comments impressed me. 
Now my workflow goes something like this:

1. review code
1. find any issues
1. report those as open issues on github
1. assign them to myself one by one
1. any commits working towards the specific issue should reference it from inside the message
1. any larger goals or problems, for me these are the 6 objectives I'm working through, should be milestones and have issues put underneath them
1. any time I'm stuck on something or notice a further thing to add I comment on the issue with the new problem or reflection
 
 It sounds a little odd using all of this on a project where you're running it by yourself, but this is all the more reason to do management properly, even if it means assigning to yourself or talking to yourself. My reasoning comes down to the following:
 - I have a tonne of things to do on this project and on other things, and sometimes I'll come back to it, sit down and go "huh. where was I", so if I assign them one by one, I know that the last thing I was working on related to that issue. 
 - For this project I have to write a report on it at the end, and being able to track what I was thinking and when is useful for talking about it later.
 - I'll be open sourcing this once the project is finished. If an issue resurfaces or new contributors wonder why I did a particular thing, it's all documented without digging through code.
   