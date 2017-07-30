---
title: Do I use an Arduino or a Raspberry Pi?
layout: post
---
I commonly get asked the above question by friends who want to get into hardware prototyping for the first time and have a project idea. I thought I'd share my input and show the questions I normally ask in the form of a flow chart. Some notes based on feedback:

1. Multithreading *can* be done on an arduino, but akin to the internet question, it will take more effort and time than on a Raspberry Pi.
1. Some paths could lead to you using both - for example, you might need a complex algorithm to parse some data which then displays it repeatedly on an LED matrix (see Amy Mather's Game of Life project). I left this out because I decided it would complicate the simplicity of the flow chart.
1. Analogue IO could probably also go on there which would lead to arduino. Meh.
1. Some paths could point to either I guess. Again, meh.

Please feel free to spread this flowchart around if it's useful to you, but if you can credit this to me then please do :)
![]({{ site.url }}/images/2015/12/flow-pi-1.png)