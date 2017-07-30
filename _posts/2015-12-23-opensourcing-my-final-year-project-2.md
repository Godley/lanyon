---
title: Opensourcing my Final Year Project - a lesson in python versions
layout: post
---
**TLDR: here's the binaries/installers for my final year project. I'd appreciate people having a click through:**

- [Mac](https://github.com/Godley/Music-Library/releases/download/v1.0-alpha/MuseLib.dmg)
- [Windows]()

Over the past few months I've been trying to create installers for Windows, Mac and Ubuntu for my final year project, [Music Library](http://github.com/godley/music-library). Despite some issues with Windows DLLs, I've succeeded in producing a windows installer. I'll find somewhere to put it so I can link to it later.

Despite the OS X edition being the original, I'm actually having trouble getting my laptop back to the exact state it was in when cx_freeze built and froze my python into an app file. The main reason is for some reason I updated my python to 3.5 one day, and then my build seg faulted, then I rewound to 3.4.3 then...I just don't really know what happened. I started to mess with my python config and yeah. It gets confusing when you have installers for python and home-brew installations.

What's the lesson here? I'm not actually that sure. In general my belief is we should stick to latest and greatest no matter what the language is, but in python's case something almost always breaks and it's hard to predict in exactly what way.

Ultimately I will probably just flush out the config on this laptop and start over. It's a good thing to do anyway so I know how to write developer install instructions properly.

For now I've gone ahead and open sourced the repo. A couple of the tests still break on linux as shown by my travis CI, but I think this is something to do with the musescore API so I'm not entirely sure it's *actually* broken.

Happy sheet music organisering?