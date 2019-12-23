---
layout: post
title: 'New years day project - Travel mapping (again)'
slug: travel-mapping
date: '2019-01-01'
---

Around about summer of 2017 I tried to get going on making my side project, a travel planning app, slightly more professional. I learned how to use [now.sh](https://now.sh) which is a great service for doing this sort of thing and started to try shifting from a database sqlite file I would change, upload and download from `now` to a managed or else better deployed service.

I got close and gave up - notably, I ripped out the backend api keys from git and replaced them with environment variables but got stuck around the point of database migration and how to do the same ripping out process on the frontend.

Now that I'm starting on 2019 travel plans - namely, Iceland and India - I wanted to get it going again, so I decided I would:

1. Move to a managed or else properly deployed database instance, not sqlite
1. Pull the frontend api keys into `now secrets`
1. Improve the UI and generally make the models less clunky to work with

## Step 1 - shift the database
I went with [elephantSQL](https://www.elephantsql.com) as my database provider for this project as I was recommended this by my partner who has other side projects of this nature. It took about 10 minutes to set this up and a few hours fiddling on the django end.

Since I needed to configure this database rather than create a file, chuck it on the server then download/upload a new one occasionally, I also had to do any migrations before chucking it into action, so I added `python3 manage.py migrate` to the dockerfile. (I do the `makemigrations` step manually as it may require user input from time to time). This necessitated changing `now.json` to tell `now` I need the `DATABASE_URL` at container build time.

In addition to this I'd need an account for admin. I attempted to make the container do this as well and looked at `djcli` which allows you to create stuff in an automation flow, but it didn't work perfectly/didn't use django's hashing algorithm for passwords so I eventually just did `python3 manage.py createsuperuser` and entered the details manually.

All was going ok, but every so often I was getting `operational error - too many database connections`. I tried a variety of things which led me down some rabbit holes.

### 1.1 Switch off debug mode
The first thing I found was that debug mode generates extra connections according to stack overflow. I disabled this in `settings.py` but this led to needing to set up my own static files server (Django handles this for you in development mode but doesn't recommend it for prod usage, therefore it's disabled).
I went with [`whitenoise`](http://whitenoise.evans.io/en/stable/), did a bit of extra configuration and added a few lines to my `Dockerfile` to have the build process collect the admin static files. (`python manage.py collectstatic --no-input`).

### 1.2 Lower the connection time
The second thing was the number of connections - django will reuse connections where possible to cut down on requests, but obviously if your server is multithreaded there will still be a few of them kicking around. I lowered the `max_conn_time` to 0 so new connections will start with every request. Probably not as performant, but this is a personal project so I don't care.


## Step 2 - get the api key out of git
For the frontend I'm using [mapbox](https://mapbox.com) which is another service I love using. I'd done a bad thing by chucking my API key into git - that key is now refreshed on mapbox so it's of little relevance now. The reason for this is I'm a relative newbie at frontend and simply didn't get how to use environment variables in frontend code - the concept of not having an environment is still a bit weird to me.

Anyway, having probed the internet and my boyfriend for answers, I went with [`dotenv`](https://www.npmjs.com/package/dotenv) and had the docker process spit out the api key environment variable to a file which is loaded by that library, which is again loaded from a `now` secret.

Around this time I also found out how to properly do CD using `now` - I had previously got a hack together using travis-ci, but since I last looked Now.sh has a good integration with GitHub, meaning all you need to do is click "install" and Now.sh will figure out everything from your `now.json` file. I'm well impressed.
![now ui](/images/now-ui.png)

## Step 3 - any other business
Finally, working with the admin side of this app has always been a bit clunky. I went to Japan and did majority trains, so I wanted to know the route names and numbers. I went to the US and did mostly roads but also planes, so I wanted different colours for modes of transport. I wanted to have the concept of places so that I could represent hotels and activities I wanted to do and have some ability to collect up prices for things.

I started by tidying the journey model, so that anything that's only related to a subset of the journeys is in `route_info` - I'm just going to make this a json blob. This will be for things like route names and IDs.
I also ripped out things like "duration" and the geoencoding on the backend - the search function had broken anyway, and MapBox provides an API which you can use to get this info from just a location name.

The main thing I wanted to get to was accurate drawings for roadtrips, instead of this:

![route in iceland]({{ site.url }}/images/iceland-lines.png)

Yet again, mapbox has an api for this, so I now have a thing which takes the locations named in the db, converts them to geocoded locations and then requests matches from mapbox. I'm still working on this but the api docs are really nice to use.

## Summary
I've really enjoyed doing this, and the tooling for deploying small applications seems to get better every year. If you stepped back to when I was at university you'd probably have had to manage your own VM, figure out the ssh connection between github, your CI provider and your deployment location, and somehow keep secret details secret. `now`, GitHub and elephantSQL have really made putting this somewhere other than my laptop a doddle.