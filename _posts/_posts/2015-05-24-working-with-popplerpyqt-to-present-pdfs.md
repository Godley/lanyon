---
title: Working with Poppler/PyQT to present PDFs
layout: post
---

<center>![]({{ site.url }}/images/2015/05/Screen-Shot-2015-05-24-at-18-57-25.png)</center>
Today I have another tutorial because I've been fiddling again. This time I was working on changing my application so that it will show each pair of pages horizontally, and then subsequent pairs vertically as if it were a book. Important for musicians because it requires less page turning. 

What this requires is producing 2 layouts - the main one which holds sub layouts and is vertical, and each sub layout which is horizontal. The libraries I'm using here are PyQt4, Poppler for PDF rendering on Python 3.4. Both of these can be fetched from PyPi.

## The code/algorithm
### TLDR
The full code is here in case you're feeling lazy:
<script src="https://gist.github.com/Godley/a95675c11d9e37166a9c.js"></script>
### Vertical layout
The implementation of this form of layout is relatively simple. First, we need to produce the vertical layout
<script src="https://gist.github.com/Godley/d0d6a06e8238806c844e.js"></script>
This has loaded no pages, but the point here is to show how poppler and piquet work together. What we basically do is create a layout and a scene, put the pages into the scene, the scene into the layout and then set the graphics widget's layout to be that layout.

### Horizontal layout and page loading
Next up we need to pop in the code that handles the pages. This needs to go before the line `graphicsWidget = QtGui.QGraphicsWidget()`

<script src="https://gist.github.com/Godley/149d7ca1456c863bfa83.js"></script>

Essentially what we do here is loop through the pages but increment every 2 pages instead of every page. 
Note I've used a list for every variable. This is because of memory management. When an item gets added to a scene or a layout, the object *itself* gets added, not a copy or the data that it holds, because C++. In normal circumstances to avoid messing up the layout by creating a new object and overwriting the old one, we'd use `copy.deepcopy` which copies the data of that object into a new object - the objects held in the layouts here are "shallow copied", meaning their memory space is exactly the same as the one we created.
In PyQt, Poppler and other libraries which python is *wrapping*, not managing itself, we can't use deep copy because python isn't able to manage the object itself - that's handled by C++. 

