---
title: Training on call newbies
layout: post
---
Recently my team established a first and second line support mechanism at work. Essentially, the idea is to address the need to train newer or lesser experienced people on the team for being on call on their own.
During work there will be one person on first line who should take any tickets or requests to the team for support and do their best to answer the need. If there's anything that person cannot handle they should either throw it over the fence to the second line support person, or ask them for help/links to documentation.
Here's some of the things that my week of first line support highlighted, and as work continues hopefully I'll update this post with tips for avoiding it or fixing it.

## 1. Fractured permissions
Our team was formed of 2-3 other teams which were either too small, no longer relevant or more usefully placed elsewhere. Then 3 people were hired from outside the company. As a result, some of the team have a lot of permission, some have bits in one area of the infrastructure and bits in another, and some have none at all (hello!).

## 2. Lack of monitoring
Our team maintains a lot of tools we probably shouldn't. We're working to address this issue, but what doesn't help is that for half of the week I've been pushed towards area of the system purely on the basis that second line support, who is an expert in that area and actively working to stabilise it, has noticed something is wrong.

For someone who knows nothing about that area of infra, I felt a bit daunted. I'm trying to embrace the fact that I shouldn't be expected to be the monitoring and alerting system for that area and push to take that responsibility from second line support and set up an actual system for monitoring it right now.

## 3. Fractured documentation
On day 3 of first line support, second line support turned to me and said "Hey, challenge for you. Find the kibana documentation and get to the kibana UI for x environment".

I went to Gitlab, where we host our Kubernetes documentation, thinking maybe, we'd started to move the rest of our docs there and that the log aggregation portion of documentation would be in the log aggregation team on Gitlab. Nu uh.
Went to the wiki, found the previous team's wiki page. Some stuff under log aggregation but not a lot.

"I'm stuck. Where are they?"

"Try google sites"

Opened up Google, went to Google sites, switched to company wide searching, looked for Kibana/log aggregation.

"Uhm. Still no joy"

"search for platform automation (our team name)".

Nope. The site was actually named platform services and it wasn't especially obvious to me how it was laid out.

If all of our docs had been on Gitlab, or Wiki, or Google sites, I wouldn't have thought to look in 2 other locations and probably would have made it to the log aggregation and Kibana setup documentation without calling in back up.
This annoyed me quite a lot, so I had a long moan and wrote some JIRA tickets. My plan is to:

- cold: give each git repo a link to it's docs in the readme.
- getting warmer: move documentation for projects that I know enough about onto Gitlab
- hot: add links/code/whatever for automatic linking into the new and shiny centralised docs store.

## Lack of support docs
The majority of the issues I mentioned earlier on parts of the system I have no idea how to work with were fixed through second line support telling me exactly what to type and who to speak to. We could really do with some basic troubleshooting docs and common scenarios and how they're solved. I'm not talking nth degree documentation because most of it won't get read, but just something where I can read it and learn for myself how to easily troubleshoot different issues. 

I don't tend to find that listening to commands from the next person along and typing them helps me learn what I'm doing and why, as much as I appreciate the opportunity to learn and do things for myself, so some really general support docs could do me a lot of good.