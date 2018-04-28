---
layout: post
title: Stuff I learned deploying with travis and now
slug: stuff-i-learned-deploying-with-travis-and-now
date: '2018-03-09 22:32:47+00:00'
---

A while back I wrote an app to manage my travel plans using Django and React. I'm pretty happy with it on the whole and getting it on the interwebs using [now]() was a real joy of a developer experience.

But...every time I had to change anything about the app, I had to redeploy it manually, most of the time the package had changed or there was some dumb build error from javascript, I had about 3 different repos I was confused about which ones I'd merged, etc etc...

So I turned to travis and ran through the 