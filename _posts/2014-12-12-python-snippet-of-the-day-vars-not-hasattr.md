---
title: "Python snippet of the day: vars() not hasattr()"
layout: post
---
At the minute, now that I'm largely coursework free, I've been working through music XML, converting things to objects.

One of the benefits of using python, particularly when sax parsing, is you don't need to know all your variable members for a class when you write the class. This makes rapid dev much easier, as I can say, make a text class, and then when I find a bit of text in musicXML, create that class and give it certain attributes as I'm going along discovering everything. As SAX doesn't load everything into memory at once, I can add to that class as I'm going along.

In C sharp (where the hell is the hash key on mac?!), I would more likely need to slowly scroll through my musicXML finding all the attributes and modeling them before developing, so I guess this is a bit like the difference between CS and SE I was talking about the other day to some extent.

However, I currently have a string method which sends back to the top class all of the attributes and things the class has. Previously, I was doing this in a very. icky way, something like this:
`def __str__(self):
	st = ""
	if hasattr(self, "whatevs"):
    	st += "whatevs : " + self.whatevs
 	return st`
for each class. hasattr is a useful method, as it allows you to check the variable is in the given object, `self` in this case, before calling it and causing an error.
However, my way of doing it now is to use `vars(self)` - this returns a dictionary of all the variables set up in your class, or, if you don't give it an object to review, it will return all the local variables initialised.
So now we can do this:
`def __str__(self):
	st = ""
    for key, var in vars(self).iteritems():
    	st += key + ":" + str(var)
    return st
`
This isn't quite right, as in the case of lists and dictionaries, we won't get what we want which is a string of each thing in the list, so we can do:

`if type(var) is list:
	#do list iteration here
 if type(var) is dict:
 	#do dict iteration here
`
Aaand now it looks much cleaner than a long list of if statements :) happy Pythoning!