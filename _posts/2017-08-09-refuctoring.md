---
layout: post
title: How to not piss off other developers with your programming style
---
Around the time I first started my degree, someone showed me [this video](https://www.youtube.com/watch?v=7RJmoCWx4cE) called refuctoring. I thought it was funny but didn't really get the full context of the video.

More recently I've spent a fair amount of time working with other peoples' code and finding some styles irk me, the kind that remind me of that video. So with that in mind I found some choice examples and wrote down my observations. The first draft of this post and the conversations before it were very...sweary. I've calmed down a bit.

# Just because a language has some great features, doesn't mean you should always use them
Let's take this code:
```
class MyClass(object):
    def __init__(self):
        self.variable_that_never_changes_but_could_in_theory = object()

    @property
    def myproperty(self):
        return MyOtherClass(self.variable_that_never_changes_but_could_in_theory)

    @property
    def myotherprop(self):
        for f in self.myproperty.mylist:
            yield f + 1
```

For those who don't write a lot of Python, the property decorator converts that method into a member of the class. Basically.
Python doesn't have the concept of public and private variables, so you can control how variables are set and returned using properties.

Like everything else in Python, they have their place and they can be useful, but this is not the time or the place. Why?

1. `myproperty` is going to constantly be reinstantiating `MyOtherClass`. Since we call it just like we would an object member (i.e `myobject.myproperty` not `myobject.myproperty()`), the average developer has no idea that accessing that member is going to result in a potentially memory expensive call or long process.
1. `myproperty`'s never going to return an object with different information. Sure, our variable indicates it could one day change if someone used your library differently to what you expect, but if you have no documented reason to use that variable differently, why keep wrapping it in another object over and over again?
1. `myotherprop` is not only calling that property, it's doing another potentially long process iterating a for loop. It's also literally just adding one to everything in that list.
1. `myproperty` doesn't seem to be getting used much but for that list, so why the nesting?

# Premature flexibility can flex your code the wrong way
```
def my_method(clients, data):
    """
    clients: dictionary of indexes to clients,
    OR
    callable which calculates which client to use based on a string input

    data: string, data for a client to process
    """
    # logic to convert potential dictionaries into callables
    # exception raised if clients is not a callable or dictionary
    ...

my_method({"blahblah": blah, "blah3": blah})
...
my_method({"blah": blah, "blah3": blah})
...
my_method({"blahblahblah": blah, "blah3": blah})
```

In what universe is it necessary for you to offer people the change to submit a callable when all of your uses have been passing in dictionaries? All you're achieving here is making your method over complicated. Adding another code path and test case for no real reason "just in case" you run into a reason for it doesn't make your codebase better, it makes it harder to maintain.

# You should (probably) only write a library if you're going to use it in 3 or more places
Some time back, while one of my senior colleagues who I would normally go to for merge reviews was on holiday, I decided to take our end to end test code for k8s and turn it into a library, so I could use some of the functions elsewhere on our deployment pipeline.

I named it a very generic `kubeutil`, had it reviewed by a fellow python programmer and pushed that into the end to end test suite. Then I started to write code with it for our deployment pipeline.
My colleague came back the following week and questioned what I'd done. Initially I was slightly offended, not because he was wrong but because I got a bought of imposter syndrome and thought he thought I was really, really stupid for having done it.

At the time, and I don't know if he'd picked this up from somewhere else in the Python community, he told me the rule of thumb "if you're doing the same thing in 3 different projects, write a library". Seems logical enough, for me because if you split out that code too early, it immediately becomes harder to manage the dependency because it comes from a different source.

There are limits and principles that conflict with this, such as avoiding monolithic applications or interfaces which are tightly coupled, but balance that about how heavily you're using the code and whether it's actually of use to other people is I suppose the point here.
