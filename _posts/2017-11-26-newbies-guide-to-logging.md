---
layout: post
title: 'A basic guide to Python Logging'
slug: basic-guide-to-logging
date: '2017-11-26 16:02:00+00:00'
---
Recently, I had the following revellation
![](images/2017/logging.png)

Educators, be those tutorial writers, former teachers, or current teachers are using print statements everywhere because they don't know that industry doesn't do that.

Or at least, having 2 former teachers and one current teacher respond in that way leads me to believe this. So I thought I'd write a short guide on how and why to use logging instead of print.

## What's the difference?

Here's some code using print:
```
def adder(num1, num2):
    print("numbers: %i, %i" % (num1, num2))
    return num1 + num2

result = adder(1, 2)
print("Result was: %i" % result)
```

Here's the same code using logging best practice:
```
import logging # which is built into python


LOGGER = logging.getLogger(__name__) 
LOGGER.setLevel(level=logging.DEBUG)

def adder(num1, num2):
    LOGGER.debug("Numbers: %i, %i", num1, num2)
    return num1 + num2

if __name__ == '__main__':
    logging.basicConfig(level=logging.DEBUG) # allows us to tune ALL of the loggers that have been used in this code (i.e if we called library code which used loggers, they would also use DEBUG). 
    result = adder(1, 2)
    LOGGER.info("Result was: %i", result)
```

This is a bit more complex, but please bare with me about the benefits:
```
LOGGER.setLevel(level=logging.DEBUG)
```
1. We can tune our output up and down, according to whether you're debugging particularly deeply or not

```
logging.basicConfig(level=logging.DEBUG)
```
1. We can tune the output of ALL code including library code or just our own code

`LOGGER.info("Result was blah")` vs `LOGGER.debug("Hello, world")`
1. We don't need to put in 200 print statements, take them out then put them back in again whenever we're stuck with that problem again

1. logger objects are singletons (that's why it's `getLogger` not `createLogger`), so you could have a logger which covers your whole program, not just the current file or context. 
    - The standard in industry tends to be to use `__name__` because when your log lines are outputted, it will start with the name of the file or context which gives you a better indication of where in the program this took place.

1. Your debug output now has an indication of how important the output is, for example let's look at the output of this program:
```
python3 myprog.py
DEBUG:__main__:Numbers: 1, 2
INFO:__main__:Result was: 3
```

1. Variables actually have meaning - notice we're not using "%" to format the variables straight into your output, we're passing them alongside your description. This means information about those variables - what type they are, what they contain - isn't cast or lost in a bland string, we can get at that information later.


### What's better about this