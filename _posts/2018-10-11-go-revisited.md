---
layout: post
title: 'Go, revisited?'
slug: go-revisited
---

I've had a few people ask if I was ever going to add to my last blog post about my feelings on Go.

It may have moved about 20%, mostly because I recently went back to Python briefly.

## It feels safe
It's been a few years since I've written in a compiled "modern" language. Going back to python after a few months made me notice all the benefits you get of that - mostly, things like variable name changes don't get into your container, and there's no digging for "what type will this be after it returns".

I think this is a good and bad thing - in python I had a strong sense we should be writing unit tests, because otherwise how will you know if your code has errors?
While it's not necessary to forcibly call every line of your code if it's compiled, that can also make it easy to...just forget you still need tests, and your code could still end up in a weird state containing bugs.

## The standard library's lacking
I find myself building up packages containing stuff that exists in the standard lib for other modern languages, such as updating dictionaries/generics/maps, checking whether an array contains a specific element, etc.

## Getting the layout right is important
I think one of the reasons I may have disliked vendoring before was the project layout for dotmesh (where I work now) is a bit...strange. After about a month or so another component was written in it's own repo and suddenly I get a lot about what golang is going for, with packages and vendoring etc.

Easy fix right, just reorganise everything? heheh, the project is huge and lots of components and types are tangled together.

## I'm still not sure I like error handling
Filling your code base with `if err != nil` just seems so repetitive. And it's so, so easy to accidentally surpress error handling.
