---
title: Red green refactor
layout: post
---
During my talk at CamPUG, someone asked me a question about my unit tests.
"When you tested this did you do it the right way, i.e TDD style of writing the test, getting it to pass then modifying the code, or...?"
And I guess I kind of lied.

I've spent the past few weeks since EMF Camp looking over my codebase and trying to make it so that the next time someone asks "do you think it'll be easy for me to get going with working on it" I can say confidently "yes. My code is beautiful" without a hint of sarcasm.
The reality of working for a year on a codebase on your own is that unless you're a complete stickler 100% of the time, it will get messy. SO I thought I'd write a blog on doing the actual refactor part which I'd neglected to do.

### Where to begin
To begin with, I thought I'd go through all of my code and write docstrings for every method. Reasonably standard practice - I disagree with this if your method is completely simple. E.g "is_int" which returns "type(value) == int" seems fairly self explanatory, but there's a lot of my methods that do a lot more than that.

At this point I got side tracked because I remembered 

1. My continuous integration had been bugging out for a while
1. I didn't have anything checking whether my tests had good coverage on my code.

I went off to find something that could do 2. and fiddled for a while with 1. That's how I found [codeclimate](https://codeclimate.com) which is a useful website for checking:

1. Cyclometric complexity, i.e how many paths there are through your code represented as a number. For every branching condition (e.g if, for, while, boolean operators, etc) the number is incremented by 1 in each method, and then a final 1 is added (probably because your method will have been called?).

1. Code duplication, which should be avoided so you don't end up with lots of copying and pasting and edits when stuff changes. Similar code can also be found meaning code that does mostly the same thing but maybe affects different classes or tables or something.

1. Style

1. Todos/fixmes which could indicate bugs.

Looked like a pretty decent tool so I got cracking. Boy was my score low.
I branched my repo and got working on point 1.
### Cyclometric complexity
This probably took a lot of my time up. I had huge methods which weren't very atomic or functional and had far too many side effects. Some big things I picked up doing this:

1. List comprehensions and dictionary comprehensions may seem cool when you first pick up python, but do you always actually need them? (for those not versed in python or other functional/lisp type languages, basically think for loop in one line to some extent)
1.1 Further addendum - if a lot of your list comps are filtering out dead weight or you're doing checks like "if value > 0 add it to my list", you can just add it to the list and filter it out later using filter(None, myiterable). Note I don't know if filter is particularly fast (stack overflow did tell me filter(None) is faster than filter(len), though).

1. Python doesn't have a switch/case, so where possible, avoid doing "if x in x" and make use of methods being first class objects (i.e replace your switches with ```mydictofhandlers[indexer]()```)

1. Look for things you're doing frequently and extract them to helper methods. This helps for both complexity and code duplication, for example I had a lot of:
```
if key not in dict:
    dict[key] = []
dict[key].append("blabla")
```
Whilst that's easy to read, I made a helper that would initialise a key if it wasn't already there to a default value. There's probably a better pythonic way to do this but meh.

### Code duplication
The majority of my code duplication was either to do with the meta parser or my database access layer. 
Stuff I went after:

1. XML parsing may handle different tags, but a lot of the time they're handling it in similar ways.
 For example, code climate brought up that my key handler and my clef handler were mostly the same - key has an int value (fifths), so does clef (line). Key has a string value (mode) so does clef (sign). I did my best to merge them and pulled out anything I could make more atomic into a separate method. This helped later because I did a lot  the same in other handlers so I could cut out the duplications and complexities just using those helpers.

2. Duplications in my data layer were similar, but it was here I realised how often I'd done a connect, ran a query with or without data, got the data, disconnected and then fiddled with the result. Sounds like a lot of connecting and disconnecting and well, code copying, so I abstracted again and made a class to handle just queries and connections, subclassed it and called those methods. Much neater!

### Style
The usual style guide for python is PEP8 and its pretty picky. For the most part I'm lazy and I usually run autopep8 to clean up issues (guess I should probably add that as a build step...). This week for some reason autopep8 just wasn't working, and it cant solve all your issues anyway, so I dug in.

My main problem here is line lengths - quite rightly, pep8 demands you have lines of less than x characters (I believe it's 79).
My problem is coming from a childhood of writing lots of SQl and relishing the fiddltness of writing queries. I have SQL strings everywhere and some of them are pretty huge.
I could either:

1. Deal with it and put the queries in a file and load them in. While valid, I feel like loading a file to load your data from another file is obfuscated.

1. Make my own atomic methods to generate queries. Again seems obfuscated and tbh, someone else has probably done it better.
1. Refactoring and find a better less hacky way of doing things.

Naturally given I haven't put forward an argument for not refactoring, I went with option 3, took to Twitter and found SQL alchemy. For anyone familiar with node packages like sequelize, its like that to an extent, but in python.
For the most part it's cleaner to read and doesn't really need you to know SQL, just understand how relational databases work.

### summary + good things
The best thing about red green refactoring for me is you've already done the red green part. On any major change you can hack your way through the code then run the test suite to confirm all is ok, or identify exactly where stuff went wrong.

Having focussed on this for a while also helped take my mind off why continuous integration wasn't working. After a few weeks of work, I came back to it and fixed some issues with my Travis and took to Twitter to get help with appveyor, so they're both running nicely and finally, I got test coverage stats (89% which isn't amazing but the scores only pulled down by a few files).

I should note having done all this doesn't necessarily mean I'm close to having a nice codebase. Reducing cyclometric complexity using methods is a good thing in a lot of cases, but if you make helper methods that only really split that method into random if statements in different methods I'm not sure whether that really serves the purpose as it becomes more obfuscated.

In summary code climate and other services are a good thing, but use them in combination with your own idea of what makes clean, readable code.