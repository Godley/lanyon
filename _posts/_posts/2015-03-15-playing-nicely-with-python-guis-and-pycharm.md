---
title: Playing nicely with Python GUIs and Pycharm
layout: post
---
My project is progressing nicely, to the point where I'm ready to start connecting it to an interface that isn't black and white - that is, a graphical user interface.

One of my perhaps optimistic goals of this project is to give all of the functionality of it to every platform without changing a lot of code to make it work. This works just fine on my actual objectives - those are rendering sheet music from xml files, extracting meta info, downloading files from online sources, creating sound output from MIDI, calculating a piece difficulty rating and finally, importing files from images to mxml (MusicOCR). All of these can be done exclusively in python with some external links to relevant file types and systems, all of which are cross platform.

Something I hadn't thought of was that whilst all of this is brilliant, I needed a GUI. This part's the hardest to test automatically and requires using the GUI on all platforms you intend to dev for to test whether it looks right.
I've used a few different widget collections during my year in industry - WX, TK and QT seem to be the main ones. 

I started down the path of installing QT on my macbook: this was slightly annoying because it didn't seem to be available on pip, meaning a manual or brew based install (brew is homebrew, a package manager for Mac sort of similar to apt-get on linux systems). I started by following the instructions on the Riverbank Computing website, which meant downloading SIP, running python on it and then downloading the PyQt binary. I got halted at the first hurdle because my python3 command on sip didn't seem to do anything. I tried moving on but it then couldnt find sip. I stopped.

I had a google to see what other options there were, and found TKinter which is the default built in python GUI library. This seemed pretty nice, and I got a couple of frames working and created my own widgets off of what was there already, but I stopped usign this due to:
a) the lack of support for pngs: I found a few libraries that would allow you to put them in, but even after getting the code working it wouldn't function as proper PNGs. (For anyone looking to use python3 pillow or ImageTK and using pycharm, I recommend that you install them by going to Pycharm>Preferences>Project Interpreter and installing them from that menu rather than installing them using terminal. This ensures that they're linked properly to whatever pycharm interpreter you're using).
b) People recommended QT as a nicer toolbox to use and in general I'd heard of that a lot more.

So I went back to look at QT. I found a lot of people had posted the same queries as me about installing it so that PyCharm would find all the intellisense info and it would work with the Python3 interpreter. Finally, I landed on this method:

 1. I did a lot of attempts with homebrew (brew install [mod]) - think this resulted in all of my python stuff being put in the brew cellar (/usr/local/cellar).

 1. [Download SIP](http://www.riverbankcomputing.com/software/sip/download) like I was doing before. Extract it, go to the folder and put into terminal:
    ```
	python3 configure.py --arch=x86_64
    make && make install
    ```

 1. Download [PyQt (I'm using version 4)](http://www.riverbankcomputing.com/software/pyqt/download), extract it and run:
   ```
	python3 configure.py
    make && make install
    ```
 1. Switch your python interpreter in PyCharm to be the one in the brew cella