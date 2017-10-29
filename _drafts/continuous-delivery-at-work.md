---
layout: post
title: Continuous delivery at work
slug: continuous-delivery-at-work
---
_This is a sort of writeup of my talk given at DevOps Days Edinburgh this week. I've reordered some of what I said in the talk since this is a blog post. Slides are [here](https://speakerdeck.com/godley/engineering-a-continuous-delivery-pipeline)_

For around about 6 or 7 months now deployment pipelines have been a hot topic of conversation at work in the context of Kubernetes. I really enjoy this sort of work because it's an area where you can make a big change in the way developers integrate their workflows with the underlying infrastructure in a good way. When users complain about something or say "it'd be good to have x" my team gets to be the people who shape that idea and can have some well reasoned and interesting discussions about how to improve their situation.

This is particularly pertinent given I've used our other deployment processes at work, and they were aweful, for the following reasons

# Getting a big picture was hard
## The problems

If it's hard or time consuming to know

1. What's in prod
1. Who owns said things in prod
1. What version said things are at (whether that be a semver or commit hash)

...it becomes difficult to feel confident when things go badly. 

### What's in prod?
Firstly, it's hard to track and understand the resource requirements of your system without knowing what's there in the first place. 

### Who owns said things in prod?
Secondly, if you can't talk to the owner of x or y microservice, informing that person that I don't know, they're causing problems with the underlying infra by using it incorrectly or they're using up to much space can be tricky. 

### What version is said thing on?
Finally, if someone rolls out a new version of x, but you don't know what version it was on previously, and that microservice deployment fails or else causes interraction problems, tracking down what changes happened and whehter they're related to the problem becomes kind of guess work depending how far from the problem you are as a support engineer. 

In my previous job I spent several hours literally going through the svn logs and jumping to that change, trying it out and marking it broken or not. Git probably has better tools for handling that, but the amount of changes you have to go through if you know the two version numbers/tags etc is much smaller.

## What we did about this
The main "concept" or tool we're using to avoid this is GitOps. Essentially, it means your infrastructure, whether that's a k8s cluster, a cloudformation deployment, whatever, is represented as a git repo. CI/CD pipelines are used to deploy everything, so an addition/update/rollback is represented as a merge request. [Weaveworks write quite a lot on this topic](https://www.weave.works/blog/gitops-operations-by-pull-request)

### Version 0.1
Initally we had something like this:
1. Microservices in whatever language are built/tested/deployed through CI to their appropriate package manager from their repo (TMI: we have Nexus instances as the central source for those package managers)
1. A second repo builds the docker image and deploys it to our private container registry (again, Nexus based)
1. A third repo deploys it (and only it) to an appropriate environment

#### Why is this only version 0.1?
Mainly, the problem here is we have a lot of microservices, and a lot of environments. If every microservice demands it's own repo to deploy it, we haven't really improved the first problem (knowing what's in prod) because we need to go through all of the repos for that environment. As a consequence, the same goes for the third point (knowing what versions are in prod).

This layout is also going to lead to a lot of duplication - most of what's in the Kubernetes yaml files for each app won't change from one environment to another, but yet we have it duplicated in every repo.

It's also not particularly secure or maintainable. Lots of different repos have full access to change the state of the cluster, and lots of engineers will need to add and remove stuff from those repositories so will probably have access to your deploy keys etc by virtue of having that right. From a maintainability perspective if I need to deploy an updated version of the pipeline scripts, I have to go through all of those repos updating the version tag for those scripts.


### Version 1.0
As above, but:
1. We add a repo which builds the yaml manifests and bundles them into a tar, which is put in a raw Nexus repository
1. The deployment repo deploys not only the application, but all applications. This is done through some raw yaml files, but mostly, urls to those tar files we deployed in the previous repo.

#### What's better about this?
We're not duplicating everywhere, and we have one repo per environment, so you know by looking at that repo what's on the cluster, rather than going through all the env repos. 

# Incomplete or non-existent documentation
## The problem
If the whole process isn't documented _clearly_, we could end up in several, bad scenarios:

1. You could add a feature to the pipeline which users don't know exists. They're not going to ask you about it because they don't know they can ask about it, so you just spent a whole load of time writing a potentially time saving feature of automation and nobody uses it.

1. A user could misuse the pipeline and end up destroying something. That probably indicates your pipeline is crap and there's problems underneath your docs problem, but if you at least write down all of the pitfalls and highlight them to your users, then they don't end up dealing with incidents which could have been dodged.

1. Users could end up spending weeks or months trying to deploy, not to mention the time they might spend talking to other engineers about the problem which causes multiple people to context switch.

## What we did about it
