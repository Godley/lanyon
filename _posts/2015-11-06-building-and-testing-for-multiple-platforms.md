---
title: Building and testing for multiple platforms
layout: post
---
![]({{ site.url }}/images/2015/11/logos-continuous-integration-tools.png)
When I graduated from Hull, I set myself a target that I would open source my project by Christmas. At the time I was ready to release the first chunk as a python package and did so [here](http://github.com/godley/museparse). I should note to anyone else who's at university and thinking of open sourcing their projects that in general, **you should ask your university first** because technically, they own the project. 

Since then I've not really touched it until recently because I had various other things on the go, and to be honest I spent so long [arguing with windows and poppler](http://charlottegodley.co.uk/arguing-with-poppler-and-qt-on-windows/) that I needed a break to think about it.

About a week ago I started the process of making sure everything was ready by [setting up travis](http://travis-ci.org). For anyone who's not initiated into continuous integration, the idea is that every push to your repo gets built against your unit tests to confirm nothing's broken. "But I can just do that on my local machine!" I hear you cry. 

Storytime! A couple of years ago my friends and I took part in the infamous [Three Thing Game](https://www.youtube.com/watch?v=1GkvKC8Ls9E). Like a lot of the uninitiated second year newbs, none of us had got into the habit of using source code control, and we decided to just write our separate bits, pass them around on memory sticks and do a big merge in the end.

This did not go well, and whilst the idea was brilliant and we'd seen parts of our code working together, ultimately our separate sections fought with each other.

To me this is something that continuous integration would have been brilliant for fixing, because it would systematically not only prove whose code was the cause, but it would also give a more precise estimate as to which sections of code caused the problems. A few other reasons to think about:

1. Do you really want to sit watching unit tests build for however long? Having a build machines allows you to go off and work on something else, knowing you'll get told when something's broken.
1. What about if you're building for multiple operating systems? Before using CI, to test ready for my final year project demo I had to launch 2 VMs and clone or else copy the same repo into each one and run my tests 3 times. I have around about 2.5k tests, it really wasn't fun.
1. How do you know that when you deploy to a machine that doesn't have your developer set up it's all going to work just the same?

In this particular project, the thing that using continuous integration has improved the most has been the last one. Having ran my tests a lot on OS X, I was pretty confident they would all pass without a hitch on travis which by default runs ubuntu linux (I had also previously ran tests on ubuntu which passed). 

To my surprise, about 5 tests failed because I had used far too many absolute paths to folders that obviously wouldn't exist on another machine, and a couple failed because requirements.txt (the list of python packages used in the project) was incorrect.

I fixed all of these and then tried it out on windows again. Multiple tests failed because windows isn't unix compatible, so handles files and opening and closing them differently.
From that experience, and particularly as I had been keen with this project to provide all the same features on every major platform, I knew I wasn't happy with just travis, so I went in search of a windows based continuous integration platform, and found [appveyor](http://appveyor.com) which feels very similar.

I found both appveyor and travis very easy to use, because you merely give them a yml file which defines your environment. Here's .travis.yml:
```
language: python
python:
- '3.3'
- '3.4'
- '3.5'
- 3.5-dev
- nightly
install: pip3 install -r requirements.txt
script: python3 -m "nose" --logging-level=WARNING

os:
- linux
# - osx

notifications:
  email:
    on_success: change
    on_failure: change
```

aanaard appveyor.yml:
```
# Taken from: https://packaging.python.org/en/latest/appveyor.html
# and from: https://bitbucket.org/pygame/pygame/pull-request/45/create-python-wheel-builds-using-appveyor/diff

environment:
  matrix:
    - PYTHON: "C:\\Python33"
      PYTHON_VERSION: "3.3.5"
      PYTHON_ARCH: "32"
      DISTRIBUTIONS: "bdist_wheel"

    - PYTHON: "C:\\Python34"
      PYTHON_VERSION: "3.4.1"
      PYTHON_ARCH: "32"
      DISTRIBUTIONS: "bdist_wheel"

    - PYTHON: "C:\\Python33-x64"
      PYTHON_VERSION: "3.3.5"
      PYTHON_ARCH: "64"
      WINDOWS_SDK_VERSION: "v7.1"
      DISTRIBUTIONS: "bdist_wheel"

    - PYTHON: "C:\\Python34-x64"
      PYTHON_VERSION: "3.4.1"
      PYTHON_ARCH: "64"
      WINDOWS_SDK_VERSION: "v7.1"
      DISTRIBUTIONS: "bdist_wheel"

init:

  - "ECHO %PYTHON% %PYTHON_VERSION% %PYTHON_ARCH%"

install:
  - "set PATH=%PATH%;%PYTHON%;%PYTHON%\\Scripts"
  - "pip install nose"
  - "pip install -r requirements.txt"
  - "set HOME=%APPVEYOR_BUILD_FOLDER%"

build: off

test_script:
  - "nosetests --logging-level=WARNING"
```


This makes it very easy to define every element, including downloading non-python packages in [museparse](http://github.com/godley/museparse)'s case. 

A perhaps greater thing for open source projects though is that the whole service is free if your project is public on github, and Travis is included in the Github education package so students get it free for private projects too!

I strongly encourage developers, especially students to start using continuous integration. With great services out there for all platforms that require minimal setup, there's really no excuse and it's not just something that large teams should be using.