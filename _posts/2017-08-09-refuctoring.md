---
layout: post
title: Refuctoring
---
Around the time I first started my degree, someone showed me [this video]() called refuctoring. I thought it was funny but didn't really get the full context of the video.

Now, oh boy, do I know the type who does that.
Let's show you some choice examples of stuff I'm really amazed by:
```
def Repository(client, *args, **kwargs):
    if client.version == 1:
        return RepositoryV1(client, *args, **kwargs)
    else:
        assert client.version == 2
        return RepositoryV2(client, *args, **kwargs)
```
WHY NOT
```
def Repository(client, *args, **kwargs):
    if client.version == 1:
        return RepositoryV1(client, *args, **kwargs)
    elif client.version == 2:
        return RepositoryV2(client, *args, **kwargs)
    else:
        Raise("SOME ERROR OH NO YOUR CLIENT VERSION IS INVALID")
```

What's next, let's see...
```
def mydumbmethod(mydict_or_callable_apparently):
    """
    mydictorcallableapparently can be a dictionary or callable containing
    logic on discovering what a given key maps onto
    """
    if mydict is a dict:
        convert to callable
    if mydict is not a callable:
       throw an error
```
Elsewhere we call the method
```
mydumbmethod(mycrapdictionary)
mydumbmethod(mycrapdictionary2)
mydumbmethod(mycrapdictionary3)
```
OHKAY so we're calling the method with dictionaries constantly, THEN WHY IS IT NECESSARY TO FLEXIBLY ALLOW A CALLABLE. 
