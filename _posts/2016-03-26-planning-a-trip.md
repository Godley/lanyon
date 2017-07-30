---
title: Planning a trip
layout: post
---
![]({{ site.url }}/images/2016/03/Screen-Shot-2016-03-25-at-23-16-10-1.png)
Over the last few months, the plan for what I'm doing for my summer holiday has changed quite a bit.
I started out planning to go to Yellowstone with my parents. Then I expanded that trip to nip and see a friend in LA, and a friend in San Francisco.
Decided I didn't fancy seeing Yoghi bear quite yet for various reasons, so backed out.

Around the same time, Danny started to think about planning a road trip to see his friends in Denver and our mutual friend Rob in San Francisco, so I decided to tag along, and added a point to go see my friend in LA and to nip up and see Ellen in Milwaukee on my way home.

Given that there's 6 people involved (7 including Ellen, but she's not road tripping) we wanted to find an app to plan and share the plan with each other that would show the route, allow us to store the points and if poss, add things like flights into the mix. I found a couple, namely [Furkot](http://furkot.com) and [Roadtrippers](), but neither seemed great and the sharing and storing functionality is a bit rubbish, in particular the dates get very muddled on Furkot and Roadtrippers doesn't seem to like having a centralised plan with multiple contributors.

So we did what nerds do, and decided (well, me and Danny did) to put the data on Github manually.
I decided, after accidentally making a relational database from json files, to map it all together and now we have this:
![]({{ site.url }}/images/2016/03/Screen-Shot-2016-03-25-at-23-16-10.png)

## How it works
The map is formed of about 4 files:

- locations.js: contains every point we're stopping in or visiting on route written in geojson.
- people.js: contains every person on the route, with a list of points they are visiting and a line colour.
- transport.js: contains every flight and train on the route, with indication of whether the flight is unique on the map or not (e.g the flight from Denver to Milwaukee) - this was so that these lines don't need to be offset from others in order to be seen.
- hotels.js: err...this is empty, but will contain hotel bookings.

They're all pretty cludgy, as I just needed somewhere to put the data. If I were to reuse what I have, I'd probably make a proper backend, but as is there's not really enough data to justify it.

The data is tied together using leaflet and mapbox, which is a nice api for creating maps that look nice on mobiles and is used by several well known map apps such as foursquare. 

To pull the data onto the map, I did this:

- loop through people, creating a line for each person with each point's coordinates pulled from locations using a foreign key. Add an offset each time so that they don't all pile on top of each other.
- loop through flights, creating a dotted line between the points and attaching an on hover method which shows the box giving flight information.
- loop through trains, creating a smaller dotted line.

There's various improvements I could make and clever things I could have done to avoid having properties like "unique" on flights, but for now I'm fairly happy with it. You can check out how it's going [here](http://charlottegodley.co.uk/usa-2016/), I'll be updating it as we get more info.