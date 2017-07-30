---
title: Animating PyQt4 Apps
layout: post
---
I recently did a complete overhaul of my UI for my Final Year Project. I still have yet to confirm with my supervisor whether this is ok, but as my project doesn't even include a GUI as an objective I'm fairly sure it will be ok. As an interim measure I've kept it separate to application logic for now.
ANYWAY, this is the new GUI:
![]({{ site.url }}/images/2015/05/Screen-Shot-2015-05-22-at-12-31-08.png)
And the GUI in candy theme:
![]({{ site.url }}/images/2015/05/Screen-Shot-2015-05-22-at-15-52-23.png)

As opposed to previous pictures you might have seen from past blogs, I'm having each widget slide out from the left when you press the relevant icon. Accidentally this has turned out to look a bit like Ubuntu because of the colours and the icons.

What I'd like to write about today, because it's useful, is property animations in PyQt4.


## What are property animations?
Property Animations allow you to say "I want this to change this particular property starting from here and ending here". Setting up a property animation for geometry, which is what I'm using here, is pretty simple:

```
animation = QtCore.QPropertyAnimation(self.contentFrame, "geometry")
```
This says "I want a new animation that alters this frame on this particular setting"

## What's geometry?
In PyQt4, you set the shape and position of any graphical object using setGeometry:

```
object.setGeometry(x,y,width,height)
```

As you can probably guess, `x` is the position of the object on the horizontal plane - 0 is left. `y` is the position of the object on the vertical plane - 0 is top. `width` is the width of the object, and `height` is the height of the object. If you want any of the properties to stay as they were, you can do `object.pos().x()`, `object.pos().y()`, `object.width()` and `object.height()` respectively.

## How long does it last for?
Firstly, let's set how long the animation should take to complete.

```
animation.setDuration(200)
```

## Where does it start?
Back to our animation: next up we need to define what shape the frame should be at the start of working with the frame. As you'd imagine, this involves setting a start value:

```
animation.setStartValue(QtCore.QRect(position.x(), position.y(), self.contentFrame.width(), self.contentFrame.height()))
```

This basically says "start where I am right now" - I think effectively this line is pointless as I'm fairly sure that if there's no start value given, **the animation assumes this information anyway**. Here I'm providing the code as reference in case you wanted a different start position.

##Where does it stop?
And now we define the stop position.

```
animation.setEndValue(QtCore.QRect(endx, endy, endwidth, self.contentFrame.height()))
```

Personally, my `endx` is set to be next to the button bar to the left, so I grabbed self.buttonFrame.width(). `endy` is it's current y position, and `endwidth` is set to the width of the widget it will contain.

## Aaaaand go!
Finally, all you need to do to tell it to begin is call `animation.start()`

## Memory management
I don't really remember why, but I also had to set `self.animation = animation` to make sure C++ didn't simply throw the object away before it finished. I can't remember what the bug was, but you probably want it in there to be safe.

