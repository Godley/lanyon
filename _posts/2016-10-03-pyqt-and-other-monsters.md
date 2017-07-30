---
title: PyQt and other monsters: a deep dive into GUIs and packaging python apps
layout: post
---
One of the big bad and ugly things about python is that the vast majority of scripts don't have (or in fact, need) a GUI. This means if you dig a little under the surface and try to write a GUI or package an app for people who don't want or need python on their machine, it's easy to get put off by the mess that's under there and the lack of other people there are to ~~moan at~~ get help from.

In the past two years I've written quite a lot about PyQt and the horrors of writing one code base to run on 3 platforms as a *proper* program, so I thought I'd summarise as I still get lots of questions on the matter.
If you don't want to read my in depth reasoning for everything, there's a TL;DR section at the bottom.

## GUI Libraries
The first inevitable problem is finding a GUI library you like. I can argue with people about the merits of each library till the cows come home, but fundamentally it comes down to who you are and what you want out of it. So I wrote you another flowchart:
![]({{ site.url }}/images/2016/10/Untitled-Diagram.png)
That is incredibly simplistic, but it gives you the basic reasoning of this section.

Previously when asked this question, I would have said **use wxPython unless you really hate designing GUIs in pure python** or your GUI is complicated.
This is because most developers don't want to spend weeks setting up a developer environment and then teaching other developers how to set it up the same way. If you'd like evidence of how painful setting this crap up is, go and see my other blog posts.

### What's so great about Qt?
First of all, Qt is the basis for a lot of different dev toolkit's gui stuff. Android, Windows Phone draw heavily from it, I'm not sure about iOS. If you get used to Qt's toolkit, it's very much transferable to other platforms.

My reasoning for saying "if your GUI is complicated, use PyQt" is because you can do all of the designing, and in fact, a lot of the very basic signals and slots, (those are the C++ terms for asynchronous communications between elements), using the designer tool which saves to an XML file. You could theoretically write an app in C++ and Python and they would look exactly the same because you then have Qt load the file dynamically in your app. I used TKInter and WxPython for a while, and there's so much fiddling to be done with frames and stuff if you don't have the ability to visually create it. 

The other advantage to the creator is that you could easily have your designers produce a perfectly legitimate design without them knowing how to code and they'd then give you the XML file and it'd be a smooth workflow.

Finally, if you're a fan of web design and want to be able to theme your windows, **it works with it's own flavour of CSS called QSS**. You won't find all that many differences to regular CSS and like the XML designer files, you can load it on the fly.

#### Qt creator/Qt Designer
During my final year project the tool was called Qt Designer and you didn't need any config crap set up to use it, but since then the Qt people rolled it into Qt Creator. When PyQt5 was put onto pip this actually made it more faff like - I couldn't find where pip had downloaded Qt and wasn't sure what environment variables, if any, were set. So I followed this path:

- download and install QT and Qt creator from the Qt website
- make your design
- Install pyqt5 from PIP

I still don't think that's the right workflow, but it meant Qt creator didn't keep bugging me about "kits" because it had been configured by the download/installer, and that didn't seem to affect the pyqt download.

### Why would you recommend against it?
Qt and PyQt used to be painful to get set up, because PyQt and Pyside are wrappers to Qt, which is a C++ library. For windows this wasn't such a pain, actually. All you needed to do was download the binary installer from the PyQt website and install it like you would a regular program, but other platforms needed you to do various things with system paths and such and that was further compounded, for me, by struggling to get Poppler (a PDF library) working with it. Homebrew and MacPorts made this a little easier, but my use case seemed so specific that recipes didn't always fit what I needed them for.

However, as of a few months ago, **pyqt5 was put onto PIP**. You don't have to install the underlying Qt, it works the same on all platforms and you don't have to fiddle around with SIP or any other crap.

If you're doing something incredibly simple, though, then wxPython might be better purely because you'll probably find more people using it. You might also consider PyGame for some contexts.

**PLEASE don't ever use TKInter.** It's a really horrible library, still not sure why it's part of python by default. If you're wondering what TKInter stands for it's TCL Kit Interface, TCL being a really old language. To the point where TKInter doesn't by default support PNGs. Yah.

You could also consider using a lightweight web framework like **flask** and having your app start using a web page shortcut and a batch file to run the server in the background.

### What's the difference between PySide and PyQt?
I'm not entirely sure. I've always thought PyQt was better documented and a more direct link between the C++ and the python upper layer. I think PySide may have with it a deployment tool (PyInstaller) but I could be wrong about that. I really think this is a personal decision, and by that I mean, I found PyQt, spent weeks getting it working and went "OK I AM NEVER LEAVING YOU OR CHANGING THIS CONFIG" so I don't know about PySide. I don't think there's much in it.

### What else do I need to know?
PyQt doesn't support PDFs. Poppler, which plugs into PyQt and is a PDF reader, is devilishly tricky to get working on windows (to this day I don't know how you do it, could someone please tell me? I can't find a binary anywhere...).
I personally got poppler to work on other platforms using the MacPorts recipe **py34-poppler-qt4**, and then on windows cheated and used the open command to have it open in the default windows pdf viewer.

## Packaging apps
Once your GUI is ready and your app is bug free (loljks) you probably want to start thinking about how people will run your app. It may be ok if your users are technical to have them install python etc, but what if you don't want that?

Once again, python has a lot of options, not all of them good. 

### CX Freeze
At the time, I wanted one package for all 3, so I went with **cx_freeze**. **I'm not sure I can currently recommend it now** because when I upgraded my python to python 3.5 either cx_freeze or my app after being frozen **blew up with a segmentation fault**. I didn't even know you could do that in python and that's really not a good thing.

Some other issues I found with CX freeze were that when I applied a theme right at the beginning of my app, the menu bar wouldn't be there when I froze it on mac. I don't remember if that happened on Windows because I mostly work on mac. I fixed this by having it load the theme when you clicked it which wasn't a perfect solution but hey.

### Other options
At the time, PyInstaller didn't work with python 3 I don't think, py2app and py2exe weren't a single-command-different-parameter set up and I think that's all of our options folks.

### What's the difference?
PyInstaller seems to produce 1 file with all of your python bundled into it. I think this is much better because it makes it harder for other people to dig into your code and find things you don't want them to find, like secrets and API keys which are tough to obfuscate in python.

CX freeze on the other hand makes the file you give it into an executable, but leaves your other code as python. I think all the dependencies get turned into exes as well actually?

### Platform specific stuff
For mac I had to make my own nib file which defines things like menus and config stuff that's specific to a mac app.

### Debugging packaged apps
I found quite regularly that packaging the app threw up errors I hadn't expected or seen when running it in python. On mac, if an error occurs, the app will just close. There's a relatively easy way of finding out what the error is:

- Open your applications folder
- right click, view contents on the app
- go into the source folder
- find the executable which is named the same as the original python file you gave the freezer
- run it and play with it until you get the same crash.

### Beyond the packager
Something else I wanted to do once I had a packaged app was **create an installer**. For Mac, I wanted one which did the whole magical drag and drop thing. For this there's multiple options, I went with dmgCreator, mostly because I liked how easy it was to layout where everything needed to go and I think you can save your config to a json file as well.

On windows, I just wanted a regular binary installer. I don't remember where I got with this one, but at this point we're out of the realms of python so there's a lot available for installing a vanilla exe file.


## TL;DR

- Use PyQt unless you're doing something very simplistic, like making GUI designs using pure python or need more support for what you're doing from the community
- Don't use TKInter. I will laugh in your face. Seriously, you can't justify to me why you'd use TKInter.
- PyInstaller's probably your best bet for a one stop shop packaging solution, but you still can't make an app for macOS from windows or Windows from macOS.
- Installer programs are separate from packagers. There's a lot of them, dmgCreator is my preference for mac. On Windows I don't remember which one I used.