---
title: Doing my best at documentation
layout: post
---
I'm a firm believer that documentation is probably more important than your code.
Why? Well, mainly this stems from my industrial year combined with blogging for Element14. In my industrial year, I had to spend a long time looking at the docs before I understood any of the stuff I was working with, and much of the code was either written by developers who had left or the current developers had long forgotten how bits of it worked, which is completely natural when you have a huge project to work on. Often I kind of reverted to using the dir() method in python quite a lot and just guessing my way round stuff.

At E14 I was pretty much flying solo - any help I received took days to come back from people who were working with other things, and again, may or may not remember how to use the devices. It's also a different work environment - the code at Airbus was mostly in-house, if not contracted for their purposes. E14 were just the distributors for most of the kit I was working with, so it's natural people would be less fluent.

So why are docs more important? Because if I'm mostly relying on those to figure out how your code works and you don't explain what a method does, I'm probably not going to use that method. So you just wasted a long time on it if your intended user is an OS developer.

Anyhow. I promised myself on my FYP open source plan that I wouldn't just dump my program out there for the world to see with the note "lol I built this for me only have fun figuring out how it works", and today I've been working on automatic documentation.

In python, automatic documenters use introspection to examine the docstrings of each class and method. A docstring in python terms is the first string inside that method or class, where you can indicate what the method does. I started on this by running through each class and most of the methods within that class which weren't obviously titled (Hey, guess what addStaff does!). For each one I tried to give a brief explanation of the purpose and for classes, an indication of the variables within it.

I started off looking at pydoc, the module provided by the foundation with python. This worked pretty well, but if I was going to publish the docs anywhere...well...here's a screenshot:
![]({{ site.url }}/images/2015/07/Screen-Shot-2015-07-27-at-22-37-33.png)
Colourful, yes, but ick. Not really all that readable or pretty, to be honest. 

So I went for a browse, and OSS and python being the fabulous community that they are, there's a lot of other tools that do this. If I remember correctly, epydoc was the one we used to use at work. A lot of other people have used it too, but unfortunately python3 support isn't great, so yeah. Maybe not.

The one I've gone for is [pdoc](http://github.com/BurntSushi/pdoc), which looks like this:
![]({{ site.url }}/images/2015/07/Screen-Shot-2015-07-27-at-22-36-51.png)

Nice, right? It supports some markdown notation, and has an additional feature in which it can figure out docstrings for different variables, which pydoc doesn't do.
For example:
```
majors = {-
          7: "Cflat", -
          6: "Gflat", -
          5: "Dflat", -
          4: "Aflat", -
          3: "Eflat", -
          2: "Bflat", -
          1: "F", 0: "C", 1: "G", 2: "D", 3: "A", 4: "E", 5: "B", 6: "Fsharp", 7: "Csharp"}
'''dictionary of all key signature names in the major mode, indexed by their number of fifths'''
minors = {-7: "Aflat", -6: "Eflat", -5: "Bflat", -4: "F", -3: "C", -2: "G", -1: "D",
          0: "A", 1: "E", 2: "B", 3: "Fsharp", 4: "Csharp", 5: "Gsharp", 6: "Dsharp", 7: "Asharp"}
'''dictionary of all key signature names in the minor mode, indexed by their number of fifths'''
```
pydoc then figures out what those two strings mean, and does this:

![]({{ site.url }}/images/2015/07/Screen-Shot-2015-07-28-at-00-22-25.png)

Pretty!

Annoyingly the navigation isn't all that great - if I headed into a class, I couldn't get back without using the back button. Despite this, the template looks so beautiful that I uploaded it to my own hosting.

You can now view the docs [here](http://docs.charlottegodley.co.uk/MuseParse) at my new fancy subdomain, and view overviews of any upcoming projects on the new [projects page](http://charlottegodley.co.uk/projects).