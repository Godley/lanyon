---
title: Reflecting on my Final Year Project
layout: post
---
![]({{ site.url }}/images/2015/05/Screen-Shot-2015-05-04-at-02-13-15-1.png)

![]({{ site.url }}/images/2015/05/Screen-Shot-2015-05-04-at-02-13-15-2.png)
A few weeks back during a project meeting, my supervisor, the other student in our meeting and I discussed reflection in our dissertations. This was a section included in the dissertation template last year, but which got removed this year.

My supervisor wondered why, so we emailed the lecturer in charge, [Dr. David Parker](http://www.goparker.com). His response was pretty much "no, don't put it in your template, but maybe you should blog about it?", so he's to blame if this is a terrible, terrible piece of writing.

I'll be writing this in sections based on what I thought then and what I think now. This will probably be one of those things where I add to it as I think about them.

## Picking a Project
I actually picked two projects in two separate years. Before I headed out on placement, I was going to be producing a Life of Pi scene in mixed reality for an immersive environment. What's funny about having changed is that I ended up with the same project supervisor, which did incur a "Why did you drop my project?" conversation. Would have been fun, I guess, but I really did pick this based on my brain going "ehhh, that looks cool!". Another factor in choosing from the list of options, rather than writing my own was time - I barely had any. The release of "you need to pick your project" was about March, and by around April we had to have decided.

Like a lot of things placement was good for, I am very glad that I think a lot more about my decisions, and that I had a long time before the decision came round again to think about what I wanted to do. I am very glad that I came up with an idea that solves a problem, and that I'm fully interested in for reasons other than gimmicks (not that the idea wasn't a good one), because you have to write a LOT of words on this topic and to go with it, a lot of code, and if it isn't something you love and truly want to complete from the offset, then you will hate it.

To anyone else doing a degree at any stage other than that point of decision, start thinking now. Write your ideas down when they come to you and review them when you can, so that when it comes to the decision you're sure of yourself.


##Starting the Project
I'm very curious what the average start date for people writing words and code on their projects is. Technically, you can begin development from summer before uni up until well, your demo at the end. Personally, I started prototyping my code around December time. Why? Because my supervisor insisted on having a demonstration before Christmas in preparation for the big push over Christmas, and the potential _real_ demo after Christmas, and I wanted to have something to show rather than just research. 

I spent a lot of Christmas writing code, which my parents and family found hard to believe. I honestly did get addicted to writing code, particularly unit tests as around that time I started applying Test Driven Development whole to my project, which I think everyone should really do as it's so much better than dumping tests at the end, and it's a real tangible skill which a lot of companies will respect you for. As proof of how sad I am, here's my Github commit tracker.
![]({{ site.url }}/images/2015/05/Screen-Shot-2015-05-04-at-23-51-52.png)

Having started and spent so long around that period writing code has stood me in good stead, because now I know a lot of people who are screwed in the sense that they have to write a report about features which aren't actually in their code yet, but which they know will be there by the demo. To add to that stress, in about 9 hours the mobile dev coursework deadline occurs, and in 3 days (the same day of my dissertation hand in, and oh yes, the General Election) distributed systems coursework is handed in. Me? I have all my primaries done. My secondaries aren't because some of them are huge and they're just that - secondaries. They don't prove whether I've done what I set out to do.

As for the writing portion, here is where I screwed up a little bit. For the Initial report I really underestimated how much GHC would affect me, with regard to jet lag and with regard to time. I wasn't aware of when the initial report deadline was when I got selected as a GHC scholar and decided to book my flights, but then, my university wasn't aware I was even going until I submitted an absence request.

How would I change that if I did it again? Well, I would probably do a bit of work while I was out there, but I forgot to take a US to UK adapter so that wasn't going to be easy and to be quite honest, GHC is a really full conference and particularly with jet lag involved, when we got home each day I just wanted to crash.

Secondly though I would let uni know when I got the acceptance and ask if there was anything going on around that time. I'd still probably go even if the deadline had been during GHC, but just so that I was aware of it upon going to uni again.

What were the consequences of my jaunt to the US? Egh. I was very jet lagged the two weeks after I came back, and that meant that I missed a lot of 9AM lectures, felt crap for much of the time and was stressing that I couldn't hack third year at all and that I was going to fail my degree. I broke down to my project supervisor over the fact I handed in my initial report in late, and she told me to submit a mitigating circumstances request which I'd never done before. I filled in the paperwork quite late, handed them over to the relevant people and spoke briefly about the issue, and prayed I would get it.
When results rolled around next term, I got an email telling me I had got mitigating circumstances and that my report hadn't had a mark reduction. *phew*.
Anyway, it taught me a valuable lesson about jet lag, starting things early and telling people about things early.

## Writing
Something else to point out on writing is I think it's possible I'm the only one who wrote their interim and final reports using LaTeX. I don't have citations for that, but most other people I'd imagine have stuck to the word template.
I think LaTeX is much better if you can commit the time to learning it and converting your template - personally I wrote my CV in LaTeX before starting to write my report using it, and that helped me with the learning process.

The great thing about it is that it's multi platform without changing anything about the language. I started using it because I'd moved to Ubuntu and then to Mac OS X, and I couldn't really find a word equivalent that wouldn't screw up the formatting if I converted from one to the other. With it being a language rather than an editor, it means everything is precise and you know it won't change if you move OS. 

I also find it useful being able to include documents rather than paste things in. For example, I recently wrote my user guide, but after I'd written and included it in my FYP, I wanted to change the default theme so I had to update the screenshots. As they were all includes rather than actual things in the document, I just had to change the images in the folder and typeset it again.

That's also useful because all of the sections of my report are in different files, with my report essentially being the preamble where I include them all. If I want feedback on a precise area and don't fancy giving someone 75 pages of stuff I don't want them to have to read or skim through (as I just did to my supervisor with my intro and conclusion), I can just make a new document which includes only those files.

## Know When to Stop, And When to Keep Going
Around march time I was at an impasse with my drawing objective. That is, the bit of my project which takes an xml file and turns it into what musicians know as sheet music. The impasse was that there were several bits, like drum tab, guitar tab, lyrics, that simply wouldn't fit with my object model in terms of both the fact my model didn't include the right symbols, and that Lilypond (the rendering system I use) was expecting them to be in a different place to where they were going to appear.

There were several other smaller areas which I didn't know whether to include either, and at that point it was tough to not fiddle with the little bugs and say "no. I'm moving to my next objective". Ultimately, it's important to know where that line is, and know that you have to make that decision.


## Get your other tasks out of the way
I mentioned earlier that Mobile dev is collected in a few hours. Whilst that coursework is in my view very simple, the usual all nighter crew were hanging out in the lab this evening and I was included in that. I felt very comfortable in where I was with that because I had it the bulk of the work done around easter time, with gradual changes and improvements as various bugs and problems came to me during term. The reason I was in there was so that I could ensure it worked using eclipse and that it was all in the university's source code control, which was the hand in method.

 Several other people in the room though had not been so fortunate, particularly as the IDE on campus is Eclipse, which at times can be something which slows you down (which is slightly ironic, being that an IDE is a tool which is meant to help, not hinder development).

I'd made that decision to work on it over easter because I knew it was easy, and that if I left it late that I wouldn't do a quality job of it, and I'd also be panicking over my project and my dissertation writing. It's at times hard to think that clearly and decide to work on one thing rather than the thing that's worth more marks, but I feel much better having spent a couple of hours getting it to work with Eclipse than spending those hours and rushing over features I needed to implement.

## Techy Stuff
What else did I learn from this project? Heh. The tech stuff has been fun and I've written a lot of code, but the project is really learning about who you are as a developer, a writer, a researcher and in some ways, a person really. Sounds quite cheesy.

I've also really and truly committed (heheh) to using source control, and I appreciate why people use issue trackers and write proper commit messages, which is important.

## Features aren't the be all and end all, and don't bite off more than you can chew
I recently made a decision that I'd not implement my secondary objectives. I've already mentioned that they're too ambitious, and I now appreciate that fully having written a renderer. The idea of implementing an entire Optical Music Recogniser and connecting it to the rest of my system seems completely ridiculous now (that's one of my secondaries), and even beginning the process seems an over-stress.

I'm very glad that I set measurable objectives and that I'm fully aware from those objectives that the other three are not achievable before the deadline, and that's okay. I would say to other people when writing objectives to fully think about just how much work that will be. If you're at the initial phase and writing the objectives, probably times the amount of dev hours by 10 for the ones you don't think will take long but are something you've never done before.