---
title: Rewriting my road trip app
layout: post
---
![]({{ site.url }}/images/2017/04/Screen-Shot-2017-04-30-at-22.18.41.png)
This is another rehash post where I mention an app I wrote poorly to try to improve my life and we discuss why it was bad and how I've totally rewritten it.
I guess this is what I do in my free time so that I don't do it professionally. Rewriting an entire app in the latest greatest framework at work is generally a bad thing.

ANYWAY, Done with bashing my own blog and writing style. Last year I went to the USA on a road trip starting in Los Angeles and ending in Denver, so I [wrote this blog](http://charlottegodley.co.uk/planning-a-trip/) and a crappy Javascript frontend with a really cludgy sqlite->python->JSON backend. Don't do that. 
The trip however was awesome and I do like planning stuff like that. Tripwise here's some things I would have done differently:

- print off all your hotel details beforehand. I meant to do this and forgot and we got lost in Los Angeles as a result because my phone died and we were relying on 3's feel at home awesomeness to get us there. Bad. Idea.
- Don't do route 1 in one day. And definitely don't set off at 10am, then drive back 3 miles taking 20 minutes in LA traffic because you left your passport in the top drawer of your AirBnB bedside table.
- Give yourself a bit more time to look into sections you don't know so well/talk to people who do know it. Between Las Vegas and Denver we only gave ourselves a day or 2 to make the journey (which is a very long way driving through the whole of Utah) and for similar reasons to the route 1 point, I regret it. I should have spoken to our American buddies who went with us about what good National Parks there are there because there's lots and the scenery is so vastly different to over here (deserts, rocks, canyons, it's amazing). Basically Zion national park is awesome and I want more of that.
- Do more national parks. Just about anywhere. If I went again I'd spend the majority of my time in national parks.

Anyway on the planning front, the UI I made to plan it was sludgy because:

- the backend was bad. I started off with JSON, then I accidentally made a relational collection of JSON objects, then I started using SQLite but didn't want to put in an API so ended up making a python to JSON converter and yuck.
- the frontend wasn't quite so bad, but it was just pure javascript with the mapbox API (which I still love. It's super easy to work with) and not easily extendible.

So yeah maybe the whole app was bad.
Anyway, given my new found love for Django and how easy it is to write a REST api in it, I wrote an API in Django and the frontend in React. Turns out there's a very nice React component for mapbox.
It's currently fairly simple - it just does a request to journeys, activities and hotels and draws each group of elements on its own layer and assigns a popup to each one containing information about that element. In future it'd be cool to:

- have it group trips together: if I added all my data for another trip like going to Canada or India in a few years time then they'd all display on the map which I like the idea of, but I would also like to be able to give it a parameter saying that I just wanted the current trip I'm planning
- an itinerary window: before I had like a todo list and an itinerary list saying when I'd be going where/what I still needed to book in different cities

[Here is the API repo](https://github.com/Godley/mapAPI) and [here is the UI repo](https://github.com/Godley/mapUI)