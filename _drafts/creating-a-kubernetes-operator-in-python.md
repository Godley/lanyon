---
layout: post
title: Creating a Kubernetes Operator in Python
slug: creating-a-kubernetes-operator-in-python
---


Here are some things I've needed to think about

# Test Driven Development
I am a firm believer that any code you can write TDD style, you should. And any code you think you _can't_ write TDD style, you should still try anyway or write it as if you're intending to test it in some way later.

Reason being that I used to work in embedded systems, and I was never given the time or the support to write proper tests and use continuous integration correctly to avoid ingrained regressions. If I'm in a position to avoid that I will at all costs, not just because it makes 

# Your CRD

Next up, I needed to think about how I was going to manage the actual *C*ustom *R*esource *D*efinition (CRD). CRDs are a way of extending the Kubernetes API to cater for resource types which you manage, they look a bit like this:
```
```

The thing to think about here was that the [Prometheus operator](), which is the one our clusters use pretty heavily for setting up monitoring pipelines, creates that CRD for you. I didn't have much opinion on this either way, but in access control terms that means the operator needs full access to create definitions at cluster-wide level...which isn't so great in security terms.

We decided we were just going to create it separately and the operator would just sit and wait for it to be there before watching custom resources get created. That way, the operator only needs read only access to custom resource definitions.

The other issue was you can pretty much state whatever you like as the extension/group name for the CRD. Elsewhere our labelling follows a specific pattern but I couldn't remember what that was. Here's where merge requests are fab for this kind of thing - I wrote the definition and asked for feedback, which (hopefully) prevents it from being inconsistent since other people on the team are likely to notice stuff like that.

# But...what...do I actually need in that CR
You may notice from the above definition that no where does it specify what fields are valid, and beyond that what content is valid. 

This is a good and bad thing 
- (-) it means people could throw absolutely anything into a custom resource and not know why the operator wasn't working as expected.
- (+) You can throw absolutely any fields into your custom resource so it's highly flexible.

I have a feeling there's some validation coming of Kubernetes CRDs but I'm not sure which version that's coming into.

# How do I lay this out in code?
