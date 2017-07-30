---
title: Platformio review
layout: post
---
![]({{ site.url }}/images/2015/09/Arduino_Logo-svg_.png)
I've embarked on a new project recently, as part of a crew of other faces you'd probably recognise.
Personally my job at mo is to write wrapper libraries for the hardware. I've done some before, but small ones, whereas this one I had several to write so it meant I wanted to find a good tool for the job.
When I started, I was literally writing the code in atom, opening up arduino, writing a sketch, installing then importing the library, running it, finding a bug and running the whole process again. I quickly got sick of it.
I went on a hunt via twitter for better tools.

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/charwarz">@charwarz</a> platformio is your friend. dump the yukky ide and work from the command line and have a lib manager for free</p>&mdash; Russell Davis (@ukscone) <a href="https://twitter.com/ukscone/status/643521348216713218">September 14, 2015</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/charwarz">@charwarz</a> <a href="https://twitter.com/arduino">@arduino</a> <a href="http://t.co/fjnlPB3eM5">http://t.co/fjnlPB3eM5</a> is your friend</p>&mdash; Louis Taylor (@kragniz) <a href="https://twitter.com/kragniz/status/643523024914935808">September 14, 2015</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/charwarz">@charwarz</a> make? <a href="https://t.co/JX2JAOGz1G">https://t.co/JX2JAOGz1G</a></p>&mdash; D Jones (@drjtwit) <a href="https://twitter.com/drjtwit/status/643685428055420928">September 15, 2015</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

I then googled it, and found the alternate to the obove suggestions, all of which are based heavily in the commandline/terminal, was using an eclipse plugin.
Considering the other day I had a discussion with a co worker, and we agreed saying "it's better than eclipse" really isn't a bench mark for how good an IDE is, I'm not the biggest of fans of eclipse. I worked in it heavily for a year whilst in industry in python 2.7, but most experiences with it have been poor. Still, I gave it a shot.
I couldn't even get to working with it because the settings I gave it were apparently wrong. Googled the errors, several issues, stack overflow posts and I still couldn't figure out the problem.

Next I went to platformio, because ukscone raves about it. Me likey.
Essentially it provides a sort of structure and framework for your projects, whereby you have a folder for any libs which is where it looks first when you do imports, and a folder for your source. Works with a lot of diff boards including many that aren't arduino, you can make it automatically run and upload your code, has a library manager which is usable both through the internet and the command line (altho...I'm still not sure how well it works - I've installed a few libraries and they don't seem to be found when I do an import of any of them). Pretty much everything I want when checking a library is valid code, but if I were on larger projects I'd still quite like to have a full IDE which isn't just a text editor with an upload button.

Still, when I eventually have time where I'm not building stuff but still want something to do, I would like to make my own IDE probably with platformio integrated into it to make it do the arduino-y bit. For now, Atom and Platformio are my tools of choice.

