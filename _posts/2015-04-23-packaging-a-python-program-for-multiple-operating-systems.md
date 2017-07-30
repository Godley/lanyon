---
title: Packaging a Python Program for multiple operating systems
layout: post
---
I'm well aware that I'm probably one of the very few people who even considers installers for dissertations, and I'm proud to be in the position that my next step is either add more features to my dissertation project, or package it for users.
Which do I choose? well given the title of this blog, you can probably guess. My next feature expectations on dissertation are either too big to put in just yet (Optical Music Recognition), or have too many hours expected in order to do them properly. 

I'd much rather have a tried and tested 3 or 4 featured app which runs on Mac/Windows/Linux than have 3 or 4 awesome features and 1 crap one, and only be able to run it from a debugger, and I think that sometimes escapes Computer Scientists because the feature part is the fun part.

Anyhow, I'm writing this blog as an initial "I am going to tell you how I did this" post, which I will add to as I go about making an installer for mac and windows. I might not complete this to be honest (2 weeks till deadline, maybe 3 to the demo), but I'll definitely have a writeup of this at some point. I'm writing this both for anyone who's googled "how do you make an installer", and for myself because I'd like to remember this information for future tasks.



## Packaging options
After some discourse on twitter, there seems to be a lot of different options and packages people have made to make this easy (or easier. I've spent a good hour on this and I don't even have a mac app working). So here's my list of options:

 1. [Py2exe](http://py2exe.org): pretty much does what it says on the tin. Turns all your python code into an EXE file. I'm not using this just yet because whilst I have a windows 8.1 VM up and running on my mac right now, I prefer to work from my mac side and then move over once I know that's working, and this particular package only works from windows. Meh.
 
 1. [py2app](https://pythonhosted.org/py2app/): Same as above but for mac apps. Not using this because I want something that ideally, I don't have to change a lot when I flip onto my VM.
 
 1. [pyinstaller](https://github.com/pyinstaller/pyinstaller/wiki): I've had a lot of recommendations for this, but it seems to be more strongly supported on python 2.7. 
 
 1. [cx_freeze](http://cx-freeze.readthedocs.org/en/latest/): This seems to be the one I'm looking for as the command is the same on Mac and Windows with 1 argument switch, but I can't get it to play nicely with pyqt4 at the minute. For some reason it's complaining about not being able to find Pyqt4.QtXml, a package I'm not even using.
 
 ##Notes on creating setup.py
 Something you may have used before is a file called "setup.py", as in the command "python setup.py install" - this is a file that basically defines what's in your project, and what to do with it, a long with some useful information about the developer. Usually you use these to install a library you've downloaded from source - it's something pip and various other python package helpers will do for you, generally.
 
 Aside from pyinstaller, the packages mentioned above extend this functionality by implementing new commands. I'm going to be using cx_freeze, I forsee, and thus it has some specific syntax you need to use to get the options right.
 
 Another thing to note is that my IDE of choice is PyCharm (because it's brilliant), and one of the features of this IDE is that it can generate the initial phases of setup.py for you, available from the "Tools" menu.
![]({{ site.url }}/images/2015/04/Screen-Shot-2015-04-26-at-21-29-44.png)
I recommend starting from this generated file because PyCharm automatically puts in all the packages (aka folders) inside your project, as well as possibly some useful contact information if you've set that up.

Once you've had it create this, the option switches to running that setup task:


![]({{ site.url }}/images/2015/04/Screen-Shot-2015-04-26-at-21-32-48.png)

If you click this, it will pop up with options for what arguments to give the setup task. In the case of cx_freeze, the one most useful to debugging what's wrong with your package is build:
![]({{ site.url }}/images/2015/04/Screen-Shot-2015-04-26-at-21-34-39.png)

This is useful as it means you don't have to go to a terminal window to complete the operation. Note that when we build our final dmg file, PyCharm doesn't seem to recognise the argument "bdist_dmg" which is what you need to create a mac installer, so at that point I will be flipping back to a terminal window.

# Modifying setup.py
Next on my little adventure was to modify setup.py, based on cx_freeze documentation. I'll explain this modularly, and then give the final file at the end.
```
from cx_Freeze import setup, Executable
import sys
import os

```
first up, we need to change the import statements to use cx_freeze. These are basically the same as what Distutils and setup tools uses, but there's some extra arguments it can process.

```
# GUI applications require a different base on Windows (the default is for a
# console application).
base = None
print(os.path.exists("implementation/main.py"))
if sys.platform == "win32":
    base = "Win32GUI"

includes = ["implementation/primaries/GUI/designer_files",
"implementation/primaries/GUI/themes", "implementation/primaries/GUI/images"]
build_exe_options = {"packages": ["os"], "excludes": ["tkinter"],
"include_files":includes}
```
Next, setup a variable that will hold the base of what our application is - for consoles, that's none and most apps are happy with that, but on windows it's a bit different.

Includes is my list of folders which are resource files - when assign these to different keys they start to reside in different places. These folders will get moved to where your main executable will reside, which is important because I then had to modify part of my code to make sure python could find them.
Build exe options allows you to include or exclude various items, and this is where we bring in these included files.

```
setup(
    name='FYP',
    version='0.1',
    #package_data = files,
    packages=['implementation', 'implementation.primaries', 'implementation.primaries.GUI',
              'implementation.primaries.GUI.pyqt_plugins', 'implementation.primaries.Drawing', 'implementation.primaries.Drawing.classes',
              'implementation.primaries.Drawing.classes.tree_cls', 'implementation.primaries.ExtractMetadata',
              'implementation.primaries.ExtractMetadata.classes', 'implementation.primaries.ImportOnlineDBs','implementation.primaries.ImportOnlineDBs.classes'],
    url='http://github.com/godley/fyp',
    license='',
    author='charlottegodley',
    author_email='me@charlottegodley.co.uk',
    description='',
    options = {"build_exe": build_exe_options},
    executables = [Executable("implementation/main.py", base=base)]
)
```

Finally, we tie all this together with the actual setup class. there's a ot of stuff in here, mostly things that PyCharm generated for me. The important elements are:

 - packages: this is a list of the folders and sub folders you want to be copied and used in your application.
 
 - options: this is where we bring in specific things to highlight to cx_freeze. 
 
 - executables: what your app is actually going to run in order to work.
 
 ## Arguing with Poppler + PyQt4
 My application uses Poppler to render PDF files inside PyQt4. It also uses designer files to load the overall structure of a given window, which are in XML. Both of these functions needed a package called PyQt4.QtXml, which cx_freezer didn't seem to want to copy to the right folder.
 I fixed this by putting an import statement I never use inside my main application window: ```import PyQt4.QtXml```
 
 There's probably a more elegant solution, but this fixed my problem for now.
 
 ## Modifying your python code to use relative paths
 
 I mentioned in the above section that I needed to change certain areas of code to use relative paths, because of where the resource files I need get put.
 I stole a helper method from a user on stack overflow which achieves this:

<script src="https://gist.github.com/Godley/219114caf606baf13546.js"></script>

Essentially, this figures out whether we're running the file from a frozen copy of the python or whether we're running from debug mode.
I then went back to wherever I'd used resource files and changed them to something like this:
```
from helpers import get_base_dir
path_to_file = os.path.join(get_base_dir(), "designer_files", "Startup.ui")
```
and extracted the designer file, or css file, or whatever kind of file I needed, using this path instead of the relative path I was using before.

## Then what happened?

With baited breath I ran setup.py through PyCharm with build. I then headed over to where my dump of files had been put and opened up main. Opened up fine. Selected a folder.
Startup window closed. Main window opened. Fiddled for a bit longer, then shut it down. All worked as planned.

I build a dmg package for Mac through a terminal and opened it. Crashed after closing the main window when it should go back to the startup window. Huh.

Anyway. I moved on, fired my Windows VM back up and installed Python 3.4. I found it much easier to use [WinPython](http://winpython.github.io/) rather than Python from Python.org, partially because it already has a bunch of useful packages installed from the get go, and partially because you don't have to do any changes to environment variables to get it to work from command line. It's also portable, so you could put it anywhere on your system/use it to package up another python app without going through the .exe procedure.
I then ran:

`python3 setup.py build`

A bunch of stuff happened. I went to the relevant folder and cracked open main.exe. It crashed asking for poppler.

Here I think the problem is that I forgot the baseline amount of modules I put on my mac which I work with in the project, and I hadn't yet installed poppler on the windows side. So now I have to figure out how to make poppler work on windows -_-

[I go into explaining the process of that on another blog.](http://charlottegodley.co.uk/arguing-with-poppler-and-qt-on-windows/)

## Getting the DMG/APP files to work
I recently picked up this process again, but this time as I'm having some issues finding a working python poppler wrapper for windows so I'm taking another look at getting the mac edition to work standalone.

After some fiddling with my setup file (very very minor changes) I'm now on this:
<script src="https://gist.github.com/Godley/9d3a74f35aee885ef602.js"></script>

I ran python3 setup.py bdist_dmg and tried to open the app file. Closed immediately.
This time I'm debugging it by going into the contents (right click the app, "view Package contents"), going to MacOS, then clicking the executable (should be named whatever your exe name is, like app.py).
Error message:

```
QWidget: Must construct a QApplication before a QPaintDevice
```

Hmm. Ok. Not really sure why that's an issue, my app code creates an application and then a main window.
After some fiddling, I fixed this by creating the application, creating the window inside it and then calling the show method immediately. After show, I then call a method which does the setup ui stuff I'd initially done in the constructor of that window.
I changed this on all my other windows and tried again. Fixed it, but now when I open a new window or close one and want to open one after closing it, it crashes with the same error. Hmm.

How I fixed this:

 1. Inside my application class which subclasses QtObject (I'm not sure whether you should subclass QApp?), I store all my windows in a dictionary.
 1. When I start my app, I don't do any loading of window items inside the constructor. This makes sure the object is there inside Qt before I try fiddling with it. I don't know if this helped? I then call each window's "load" method when I need it.
 1. Even if a window isn't in use right away, I instantiate it inside the dictionary at the beginning of the program. I will then reuse that window whenever it's needed, and if the window needs info to play with (e.g my error window needs a list of problems it encountered), I provide that as a parameter to my "load" method.
 1. When I start my app, I load _all_ of my windows, show them, and then hide them. I don't know why this fixed the problem, maybe it makes sure QApp is aware of them all the way through, but then when I come to refresh the window when I need it and show it, I don't get an error.

I have more to write on both OSes I'm currently targeting. I've written about mac packaging specifics [here.](http://charlottegodley.co.uk/packaging-a-python-app-mac-problems/)