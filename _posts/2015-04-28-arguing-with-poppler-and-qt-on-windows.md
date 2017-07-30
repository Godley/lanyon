---
title: Arguing with Poppler and Qt on windows
layout: post
---
I recently started a blog documenting how I'm packaging my FYP for windows and Mac OSes. As that should only be about the instructions, not specific packages I'm using, I decided to move the portion about Poppler and Qt over here, so here we go.

## Installing PyQt
I had a long list of tips here because I thought you had to do this whole bit the same way as mac - install Qt, then PyQt, then whatever else you need from source.
Turns out I completely missed the binary packages on the [PyQt website](http://www.riverbankcomputing.co.uk/software/pyqt/download):
![]({{ site.url }}/images/2015/05/Screen-Shot-2015-05-31-at-15-33-57.png)

Ran that and tried my PyQt4 GUI. Ran perfectly.
Incidentally, I also found in my travels a script which does a lot of the conversion from [PyQt4 to PyQt5](http://python.6.x6.nabble.com/pyqt4topyqt5-a-helper-for-the-conversion-PyQt4-to-PyQt5-td5025591.html) in a similar way to Python 2 to Python 3. It didn't really work all that well with threads and signals to be honest, and my PyQt5 installation couldn't find `uic` for some reason which I needed in order to load my guis from designer files.

## Working with Poppler
Now that I have proof my gui works fine with windows, I'm back to the Poppler issue.