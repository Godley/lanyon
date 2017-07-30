---
title: Cross-platform python: the ubuntu edition
layout: post
---
![]({{ site.url }}/images/2015/09/Screen-Shot-2015-07-21-at-01-40-49.png)
I'm still plodding on with my final year project given I don't have a lot else to do right now. I'm now up to moving to ubuntu, which I started on because of my latest OSS release which I needed to check anyhow, so I decided to test the rest of my FYP on ubuntu while I was at it.
To begin with, it was going swimmingly. Got the packages I needed from apt-get:
```
sudo apt-get install python3-qt4
sudo apt-get install python3-pip
```
along with some other programs, fetched my code and my module from git and pip respectively.
Ran the program and the library, all functioned as expected...
Until, as usual, poppler fell out with me.

In order to note this for later use, you need the following packages to get poppler to function on ubuntu:
python3-qt4, python-qt4-dev, python3-sip, python3-sip-dev, libpoppler-qt4-4, libpoppler-qt4-dev
Then run ```pip3 install python-poppler-qt4``` and it *should* work just fine.
And finally it functions in full on 2 platforms:
![]({{ site.url }}/images/2015/07/Screen-Shot-2015-07-21-at-01-40-49.png)
