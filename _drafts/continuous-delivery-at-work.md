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
![]()

Initally we had something like this:
1. Microservices in whatever language are built/tested/deployed through CI to their appropriate package manager from their repo (TMI: we have Nexus instances as the central source for those package managers)
1. A second repo builds the docker image and deploys it to our private container registry (again, Nexus based)
1. A third repo deploys it (and only it) to an appropriate environment

#### Why is this only version 0.1?
Mainly, the problem here is we have a lot of microservices, and a lot of environments. If every microservice demands it's own repo to deploy it, we haven't really improved the first problem (knowing what's in prod) because we need to go through all of the repos for that environment. As a consequence, the same goes for the third point (knowing what versions are in prod).

This layout is also going to lead to a lot of duplication - most of what's in the Kubernetes yaml files for each app won't change from one environment to another, but yet we have it duplicated in every repo.

It's also not particularly secure or maintainable. Lots of different repos have full access to change the state of the cluster, and lots of engineers will need to add and remove stuff from those repositories so will probably have access to your deploy keys etc by virtue of having that right. From a maintainability perspective if I need to deploy an updated version of the pipeline scripts, I have to go through all of those repos updating the version tag for those scripts.


### Version 1.0
![]()
As above, but:
1. We add a repo which builds the yaml manifests and bundles them into a tar, which is put in a raw Nexus repository
1. The deployment repo deploys not only the application, but all applications. This is done through some raw yaml files, but mostly, urls to those tar files we deployed in the previous repo.

#### What's better about this?
We're not duplicating everywhere, and we have one repo per environment, so you know by looking at that repo what's on the cluster, rather than going through all the env repos. 

#### What's bad about this?
In order to update an app, (assuming it requires a code change) you have to submit, at minimum, 4 merge requests. Add one for every environment you want to deploy the new version into and this becomes a pain to manage.

To get around this we wrote a bot which runs once an hour, talks to Gitlab and gets a list of projects to submit suggestions to, then talks to Nexus/Private container registries and checks whether referenced projects have been updated. It then submits a MR with a full changelog generated from that repo.

We had some other stuff to deal with, like we added validation of Kubernetes yaml against a "minimal" K8s apiserver (i.e an etcd container and a bootkube container running pointed at that container) to the pipeline to make sure half completed deployments didn't happen, deleting stuff required some thought (we went with applying labels to everything with a timestamp as the value, then deleting anything that had the label set to anything else), and I had a fun week of doing Test Driven Bash Development so that we could avoid testing pipeline changes on clusters people were using.

# Incomplete or non-existent documentation
## The problem
If the whole process isn't documented _clearly_, we could end up in several, bad scenarios:

1. You could add a feature to the pipeline which users don't know exists. They're not going to ask you about it because they don't know they can ask about it, so you just spent a whole load of time writing a potentially time saving feature of automation and nobody uses it.

1. A user could misuse the pipeline and end up destroying something. That probably indicates your pipeline is crap and there's problems underneath your docs problem, but if you at least write down all of the pitfalls and highlight them to your users, then they don't end up dealing with incidents which could have been dodged.

1. Users could end up spending weeks or months trying to deploy, not to mention the time they might spend talking to other engineers about the problem which causes multiple people to context switch.

## What we did about it
Basically, write down all of the things, but specifically:
1. Make your docs be in one place. We had issues where our docs were spread over gitlab, wiki, Google sites, Google docs due to organisational change, so this time around we made sure everything is in Gitlab and users of our pipelines are made aware where that is.

1. Peer review them. Specifically what works well for us is making the full team aware of it and hopefully, waiting for feedback from someone experienced in that area (i.e the k8s concept you're addressing, not what we're doing with it) and someone who knows little to none about that area (maybe someone who's actually using and deploying via your pipelines). That's probably just good advice for anything you do, but in this case it helps us to pick up bits that seem clear but really aren't.

1. Make them concise but with lots of examples. I hate reading really really over wordy docs, I want to know how to get running and to do it fast. However with k8s it's a good idea to give a lot of examples of yaml you're using and that you explain that yaml.

1. Keep them up to date. Generally write down stuff at the time you had the problem or added the feature, but later on try to keep on top of any holes and remind people when they've forgotten to document the area.

1. Avoid duplicating upstream and follow best practice. Particularly we have a few guides for doing different things. We write these on our local docs usually because there's something about the way _we're_ doing k8s which is different to other people or there's some quirk of the pipeline we need to make a note of, but we reference upstream a lot. 

Thing with k8s is the docs are so extensive that it can be hard to navigate them sometimes unless you know what you're looking for, so there probably _is_ a doc for what you need you just need to know how to sew it together, so providing links to relevant details is a good idea.

In addition to these "guidelines" we also do template repositories, with the intention being they show how we suggest you use the pipeline. Users then fork them or copy out all the files into their own repo, and we give clear instructions on what to do with the output etc. This helps to communicate when we change or add to the pipeline, but also allows us to easily set up new clusters since we do templates for stuff we have to manage too.

# One size doesn't fit all
Developers don't work well when you put all of them in the same box. Any processes we create or manage should aim to be consistent, but not limiting in what developers can and can't do.

For example, previous processes used templating languages and internally developed tools, and both of those were requirements of the pipelines which helped people maintaining the infrastructure to work faster, but slowed down and caused irritation for development teams.

## What we did about it
Avoid rules, unless you really, really need them. This involves talking to current users but taking what they need with a pinch of salt or a stepped back view on the process as a whole. 

Any time feature requests or pain points come up, ask yourself - is there another way they could do this? How is this going to harm or hinder other people's needs and workflows? 