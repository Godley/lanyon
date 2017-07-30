---
title: Learning React
layout: post
---
A few months ago at work I was put on a project where we were using an in house framework on the device end and on the PC end. I'm not going to go into detail, it's a good platform, but the PC end had some issues so I suggested improvements. 

One of those improvements was using a frontend framework rather than pure javascript. At that point I didn't have enough time or the business case to put in a framework so I improved what I could in pure javascript and went on my merry way.

More recently I've needed to write a better UI for the client and decided to put my money where my mouth was and write it using a framework, so I chose React. I've used Angular 1 before and wanted to learn React and compare the two, so it seemed like a good idea.

Writing the UI was fairly easy - I started using create-react-app and everything was fairly straight forward.
The problem came when I wanted to link it all together so I thought I'd note down what I did to get where I am now, I'll update this as I fix some issues:

- Followed [this tutorial](https://medium.com/@patriciolpezjuri/using-create-react-app-with-react-router-express-js-8fa658bf892d#.k4gbvuyg0) on react routing. The app I'm writing is fairly simple and doesn't *really* need routing, but it mentioned express so I thought I'd give it a go. However, debugging this and getting it so I could pass data from express to react was hard, so I moved on. It helped me to better understand how to go from create-react-app to using express and node, though.
- Followed [this tutorial](https://scotch.io/tutorials/react-on-the-server-for-beginners-build-a-universal-react-and-node-app) on express and react universal apps. Initially I had problems working with babel on a windows PC - I don't really remembered how I fixed it, but my babel command is now "babel-node <fpath> --presets es2015,react" which seems to work OK.
- Read a lot of docs for dropzone. My app needs to allow the user to upload a config file which will set up the UI and allow them to tweak it, so I needed to have it upload the file, open it, read the contents, and use the contents later when the user switched to tweaking the config.

My problem now is trying to get the data I've imported from express into react router and into my route so I can use it in the form. Looking at tutorials on how to do this next.

Let me know if there's any more links to add to this which might be of use. :)
