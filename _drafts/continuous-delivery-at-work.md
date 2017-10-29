---
layout: post
title: Continuous delivery at work
slug: continuous-delivery-at-work
---
_This is a sort of writeup of my talk given at DevOps Days Edinburgh this week_

For around about 6 or 7 months now deployment pipelines have been a hot topic of conversation at work in the context of Kubernetes. I really enjoy this sort of work because it's an area where you can make a big change in the way developers integrate their workflows with the underlying infrastructure in a good way. When users complain about something or say "it'd be good to have x" you have total control over that and can have some well reasoned and interesting discussions about how to improve their situation.

This is particularly pertinent given I've used our other deployment processes at work, and they were aweful, for the following reasons

# Getting a big picture was hard
If it's hard or time consuming to know

1. What's in prod
1. Who owns said things in prod
1. What version said things are at (whether that be a semver or commit hash)

...it becomes difficult to feel confident when things go badly. 

## What's in prod?
Firstly, it's hard to track and understand the resource requirements of your system without knowing what's there in the first place. 

## Who owns said things in prod?
Secondly, if you can't talk to the owner of x or y microservice, informing that person that I don't know, they're causing problems with the underlying infra by using it incorrectly or they're using up to much space can be tricky. 

## What version is said thing on?
Finally, if someone rolls out a new version of x, but you don't know what version it was on previously, and that microservice deployment fails or else causes interraction problems, tracking down what changes happened and whehter they're related to the problem becomes kind of guess work depending how far from the problem you are as a support engineer. 

In my previous job I spent several hours literally going through the svn logs and jumping to that change, trying it out and marking it broken or not. Git probably has better tools for handling that, but the amount of changes you have to go through if you know the two version numbers/tags etc is much smaller.

# Incomplete or non-existent documentation
If the whole process isn't documented _clearly_, we could end up in several, bad scenarios:

1. You could add a feature to the pipeline which users don't know exists. They're not going to ask you about it because they don't know they can ask about it, so you just spent a whole load of time writing a potentially time saving feature of automation and nobody uses it.

1. A user could misuse the pipeline and end up destroying something. That probably indicates your pipeline is crap and there's problems underneath your docs problem, but if you at least write down all of the pitfalls and highlight them to your users, then they don't end up dealing with incidents which could have been dodged.

1. Users could end up spending weeks or months trying to deploy, not to mention the time they might spend talking to other engineers about the problem which causes multiple people to context switch.

This is all entirely avoidable through 
