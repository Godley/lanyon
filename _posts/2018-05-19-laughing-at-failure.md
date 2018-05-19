---
layout: post
title: Laughing at failure
slug: laughing-at-failure
date: '2018-05-19 13:21:33+01:00'
---
About 6 months ago I was working on a feature of our pipeline - deleting old resources. Just before I started the ticket, my colleague and I were discussing adding testing to our bash scripts, something that seemed daunting and painful to both of us. We pushed it under the rug and said "nah, let's just try it and then see what happens".

About 3 or so days later when the change had been reviewed, merged and I'd attempted to use it on a cluster, things went wrong. The way we'd agreed to handle deletes of old Kubernetes resources was:

1. Save a timestamp every time you do a build. This goes into a metadata file associated with the deployment artifact.
1. Load that in when deploying. Label all resources which you just applied to the cluster with it.
1. Select all resources that have the same label name but any timestamp other than the current. This should mean they weren't deployed this time around, ergo are not in git, so delete those resources.

Unfortunately, the build script had somehow managed to write an empty string to the file. The kubernetes API server took this to mean "give me everything that has this label with a non-empty value"...so as I watched, it deleted a lot of stuff I didn't intend to get rid of, like the Kubernetes control plane.

I announced it on the appropriate slack channels then my colleague and I sat fixing the problem, laughing to ourselves about the situation. I spent the next week doing test driven bash development which is still something I and others on my team follow. 

I still regard the laughing part as a crucial element of doing things like this, and in situations before and since I've seen my colleagues do similarly dangerous things and laugh to themselves about their mess up moment. People screw up sometimes, but it isn't a problem and you shouldn't feel bad for it. If you can't laugh at the situation it can make the problem worse. It can indicate the environment isn't safe for failure, meaning you won't be telling anyone you failed, just silently freaking out that things went wrong. It can make people tense around you because they're not sure if you need help, if you need consoling or if you just have no idea what you're doing. 

On the other hand, as I've said before, no one should ever be blaming the person who screwed up and every effort should be made after to not only fix it but ensure no one else can get stung by this again. There's limitations you can do here for people with infrastructure and ops power, to be honest - one of the other memorable "whoops" moments was my colleague deleting all the VMs of a cluster being heavily used by developers. We still need to be able to access the view where you can do that in order to administer the cluster in the first place. 

Sometimes there's no way to make the pit of success bigger than the pit of despair, but at least if you can laugh and joke about it, you've shaped the moment into a positive learning experience.



