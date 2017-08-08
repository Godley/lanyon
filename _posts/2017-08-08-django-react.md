---
layout: post
title: Writing Django-React apps
---
A while back I wrote about updating my mapping application to use Django for the backend and React for the frontend, having previously used pure JS on the front and well..having no real backend.

That's great except that:
- every time I wanted to use it, I had to have the database sat on my computer
- I had to be running it on my computer. That's not particularly shareable or collaborative with other people planning it.
- There's 2 components to it (react server + django server). Fine, well, perfect if you have a high availability app where you're likely to need to scale different components according to usage, but it's likely to just be used by me and Danny, and maybe other people I want to share my plans with.

With all that in mind I looked into how to deploy it and found out about [now](https://now.sh) which seems pretty awesome. I also found there's quite a lot of people who've decided django-react is a good idea and found [this template](https://github.com/scottwoodall/django-react-template) which seems to provide you with everything you need to get started.

Having deployed my app somewhere I then thought...err...I'm kind of telling the whole of the internet *exactly* what date and how long I'm going on holiday for which is uhm. sub optimal. So I turned on django authentication by using the authenticated decorator. The downside to that is that then all users need to login which isn't especially social media friendly? I wanted a more user friendly way to show my travel plans, so I turned off auth on the view but left it on for the API.

Instead I had my javascript try to get the data, and if it failed, get the guest api instead:
```
var options = {
    method: 'GET',
    credentials: 'include',
    headers: {
      Accept: 'application/json',
      'Content-Type': 'application/json'
      }
    };
    try {
      let result = await fetch("/api/v1/journey/?format=json", options);
      let journeys = await result.json();
      result = await fetch("/api/v1/hotel/?format=json", options);
      let hotels = await result.json();
      result = await fetch("/api/v1/activity/?format=json", options);
      let activities = await result.json();
      this.setState({
        "journeys": journeys.objects,
        "hotels": hotels.objects,
        "activities": activities.objects
      });
    }
    catch (exception) {
      let result = await fetch("/api/v1/guestjourney/?format=json", options);
      let journeys = await result.json();
      this.setState({
        "journeys": journeys.objects
      });
    }
```
This way the guest API just gives me start and endpoints and what method of travel we took, and there's no information about specifics, but enough information to show off + document where we're going.

Now seems to be taking a while to upload my stuff, but once it's done I'll update my nav :)
