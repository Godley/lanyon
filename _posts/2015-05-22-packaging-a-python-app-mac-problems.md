---
title: "Packaging a python app: Mac problems"
layout: post
---
I started on a post about a month ago about packaging for multiple operating systems. The process involves taking a python application and producing an app file for Mac, an exe file for Windows, and probably a shell script or something for Linux. Here I'm going to talk about problems I specifically found producing an app file.

## Setting the menu bar title
The first problem I encountered was setting the menu bar name - i.e in mac, that little word that goes next to the Apple logo in the top left corner.
![]({{ site.url }}/images/2015/05/Screen-Shot-2015-05-12-at-20-48-05-2.png)
This info is stored inside the app contents package in the file "info.plist". I did a manual edit to fix this, adding the lines:

```
<key>CFBundleName</key>
<string>MuseLib</string>
```

inside the `<dict>` tag. I did this manually, but note this will get overwritten every time you run setup.py so I suspect there's an option I need to set inside setup.py. To fix this I copied the info.plist with that key in it to the folder where setup.py is, and then added a field to "options":

```
options = {
    'build_exe': {
        'includes': 'atexit',
        "include_files":zips
    },
    'bdist_mac' : {
        'custom_info_plist' : "info.plist"
    }
}
```

## Retina screens
Another annoyance I found was that the outputted app file wasn't properly utilising my retina screen and came out in what's called "magnified mode". This turned out to supposedly be something else I needed to change in info.plist, so I added this field:
```
<key>NSHighResolutionCapable</key>
	<true/>
```

When you look at the app file standalone, this won't appear to have affected anything, but once I'd created a DMG and copied it to my Applications folder, the blur went away thankfully.

## Menubars and app packaging
After a rethink of my whole UI, I looked at an issue I came across which was that for some reason when I packaged to an App file, input via keyboard stopped functioning and the menu bar evaporated. 
I went back step by step, removed all callbacks and connections to my main window that I thought could have caused it, still nothing. Then I stopped applying a stylesheet as I was doing when it loaded so that I don't have to edit the style from inside QtDesigner and it worked...
That was until I had another smaller startup window load before I open a main window. I removed the loading/hiding of that window and only did it when necessary and it fixed it.

## Creating a DMG
Once I had a working App file I looked at making a better DMG. cx_freeze has the command `bdist_dmg` which functions well enough, but it doesn't really allow much room for customisation. Instead I used dmgCreator, which both enabled me to put a link to the Applications folder so that the user doesn't have to physically copy and paste it, and allowed me to add multiple app files which I needed to do in order to have the user install Lilypond at the same time as the application.
