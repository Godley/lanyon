---
layout: post
title: Creating a Kubernetes Operator in Python
slug: creating-a-kubernetes-operator-in-python
date: '2017-09-09 12:42:05+01:00'
---
An integral part of our Kubernetes infrastructure at work is container image registry mirroring. It ensures if anything happens networking wise to our clusters then the services in that data center can carry on running, and locks down access in and out of the cluster to that mirror, rather than haphazard pulling from registries in the cloud.

We mirror the 3 standard locations images come from in open source - the Docker Hub, Google Container Registry and Quay. In addition to that we have our core internal registry (just named internal), and we have some additional registries for specific streams of work which we need to mirror based on what that cluster is for. 

All the mirrors are made up of the same Kubernetes resource types with slightly different variable input. Up until now we've maintained this issue using a template which we pump variables into in order to generate the new yaml, then bundle that together with everything else being deployed. 

The problem is that other teams maintain the "streamed" registries, and they need to be added to only specific clusters, so that means while they should be templated in the same way they can't form part of our baseline yaml that goes onto the cluster...I guess this part of the blog might be confusing without a picture of our deployment model, but hopefully you get the point.

# What the hell is an Operator?
Operators are a class of entities in Kubernetes which extend the Kubernetes API in a customised way - so, instead of having this duplicated template all over the place, you tell the cluster "I'm defining a new resource type", create an object of that resource type and the operator manages the creation and management of that resource type, generally through telling the Kubernetes API what entities to create and what fields those entities should have. The intention is to have operations as code to avoid duplication, and it works well in cases like this where you have the same set of resources being created over and over again. Other examples include monitoring and alerting pipelines such as Prometheus and database installations like Postgres and Cassandra.

Aaaaaanyway, I've been itching to write some more on what I do at work, so here are my thoughts on getting started.

# Test Driven Development
I am a firm believer that any code you can write TDD style, you should. And any code you think you _can't_ write TDD style, you should still try anyway or write it as if you're intending to test it in some way later. For example our bash scripts for deployments didn't have any testing and I wasn't really sure how we could fix this - we ended up using `basht` in conjunction with [GitlabCI services](https://docs.gitlab.com/ce/ci/services/), and while it's a little clunky and getting the local developer instructions to mirror the setup was challenging, it meant my development process had a faster feedback loop and later, other people who were also low on bash skills could contribute and have bugs which would have taken longer to find highlighted, avoiding lots of failed deployments and poor developer experience.

If I'm in a position to avoid testing code through deploying it then poking at it I will at all costs, not just because it gives you confidence your change hasn't broken anything but because it makes it easier for new contributors to work with your code. The first place I look to find out what your code *actually* does, not how to use the output, is tests.

With that in mind I've tried doing TDD on Kubernetes tools before when I wrote our end-to-end healthcheck tester application (which one day maybe we'll open source if people think it's useful). I failed because I didn't quite get what Kubernetes was, or how to effectively mock stuff. I've done a lot more mocking recently in the process of testing other tools, so I figured I could try mocking out parts of the kubernetes API.

This turned out to be
a) boring: I really love the idea of writing code to create stuff on a cluster. It's fun, but when you're weighed down in thinking "how do I exactly imitate how that cluster behaves", it takes all the fun out having orchestrated an actual container.

b) hard: the main logic of this operator is it's going to watch events about custom objects. For some reason the kubernetes python api uses `urllib3` rather than `requests` so I can't use httmock which makes things a lot more logical, and the `urllib3-mock` library doesn't appear to have a way to mock a streaming api. I settled with mocking out the stream method but eh. I really prefer having mocked the data from an api, rather than the methods inside the client library.

c) slow: I don't 100% know how to do everything on this task and while TDD is good for taking a complex task and breaking it into small, testable pieces, the mocking problem means I spent several hours trying to mock rather than code for a problem which isn't _overly_ complicated.

So I've temporarily hung my "is this tested" hat on the shelf and continued on my merry way. 

# Your CRD

Next up, I needed to think about how I was going to manage the actual *C*ustom *R*esource *D*efinition (CRD). CRDs are a way of extending the Kubernetes API to cater for resource types which you manage, they look a bit like this:
```
apiVersion: apiextensions.k8s.io/v1beta1
kind: CustomResourceDefinition
metadata:
  name: myresources.area.company.postfix
spec:
  group: area.company.postfix
  version: v1
  names:
    kind: MyResource
    plural: myresources
  scope: Namespaced # Can also be cluster level using "Cluster"
```

The thing to think about here was that the [Prometheus operator](https://github.com/coreos/prometheus-operator), which is the one our clusters use pretty heavily for setting up monitoring pipelines, creates that CRD for you. I didn't have much opinion on this either way, but in access control terms that means the operator needs full access to create definitions at cluster-wide level...which isn't so great in security terms.

We decided we were just going to create it separately and the operator would just sit and wait for it to be there before watching custom resources get created. That way, the operator only needs read only access to custom resource definitions.

The other issue was you can pretty much state whatever you like as the extension/group name for the CRD. Elsewhere our labelling follows a specific pattern but I couldn't remember what that was. Here's where merge requests are fab for this kind of thing - I wrote the definition and asked for feedback, which (hopefully) prevents it from being inconsistent since other people on the team are likely to notice stuff like that.

# But...what...do I actually need in that CR
You may notice from the above definition that no where does it specify what fields are valid, and beyond that what content is valid. 

This is a good and bad thing 
- (-) it means people could throw absolutely anything into a custom resource and not know why the operator wasn't working as expected. (I suppose you can combat this with good docs, which is what I'm planning on doing as I go along)
- (+) You can throw absolutely any fields into your custom resource so it's highly flexible.

I have a feeling there's some validation coming of Kubernetes CRDs but I'm not sure which version that's coming into. 

I started to think about this in code terms but I diverted back to writing yaml and showing it to my team. We decided we actually just need 2 fields.

# How do I lay this out in code?
The final issue, and really the reason we're making this operator, is that our current method of creating registry mirrors involves creating 2 services, 1 statefulset, 1 daemonset and 1 pvc...per mirror.
Naturally to avoid duplication, we have all of this in a template and then use some hacky bash script to `sed` different variables and values into the template on creation.

Question is, do we carry on with this but use the python yaml library to load it in/some templating engine, or do you represent that yaml template as objects and update them with the data coming in from the custom resource?

I've uhmmed and ahhed over this a lot. I think I'm headed towards creating an object which represents both the custom resource coming in and the outgoing Kubernetes resources. Seems the most pythonic method + means I can click around to get to the attributes in my IDE, rather than looking at yaml.

I do like working with yaml, but I think if you're writing code (unless it's some test fixture in which case probably easier to work in yaml) you should be thinking in code, not manifest. That's part because the code I'm working with right now is object oriented, if I went with functional I'd maybe think differently

# Summary
I hope this blog post has given an insight into the things you need to think about when writing your own operators, but also shown they're easier to write/get started with if the problem is well defined.
If it's not well defined you should probably step back and consider whether you should actually be writing one?
