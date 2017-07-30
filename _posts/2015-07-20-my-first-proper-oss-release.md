---
title: My first "proper" OSS release
layout: post
---
![]({{ site.url }}/images/2015/07/Screen-Shot-2015-07-20-at-17-38-54.png)

During the past year I've been working on my Final Year Project as you're probably sick of hearing about. I promised myself and my lecturers that I would open source it for several reasons:

1. I made it because music software is expensive and I wanted something that I could pick out what I wanted. Seems dumb to release it for money or just as shareware or whatever when I'm sure there's plenty of other people who are equally frustrated at music software being crap or expensive.

2. There's a lot of stuff in there that's useful but hasn't been released before, or not documented in a way that you can actually use it for your own projects.

3. I'd really hate for all that code to go to waste.

On Wednesday I graduated, so I guess that means all of that code is definitely one hundred percent never going to get assessed again. Today I cleaned out the last horrible global variables from my parser, moved the folders into a better more manageable structure and released the drawing portion of my project into the wild on [Github](http://github.com/Godley/MuseParse). I call this my first "proper" release because most things I've worked on which are not for the purposes of university are already open source on my personal Github account, but none of them were really documented or intended for other people to work on.

I still have a bunch of issues to move over from their original location in my still-private FYP repo, but all the code and tests are now on there.
After that was done and I'd tested by building, installing and finally changing my original FYP to use only that package rather than the project's local copy, with bated breath, I followed this [tutorial](https://jamie.curle.io/posts/my-first-experience-adding-package-pypi/) and released it onto the Python Package Index. Now all you need to do to get that package is use `pip3 install MuseParse` and you're away.
Note I have yet to test if it works with Python 2.7. I really doubt there's much to change if it doesn't work in 2.7.

There's still some documentation work to do if anyone wants to use the classes for anything other than how the test scripts tell you to use them, which I hope to write over the next few days. For now, enjoy!