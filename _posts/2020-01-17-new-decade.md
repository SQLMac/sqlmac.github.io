---
title: DevOps in the new decade
date: 2020-01-17 08:52 -500
categories: [technology,devops]
tags: [devops,engineering]
---

> “The Only Thing That Is Constant Is Change”>
>
>   ~ Heraclitus

# A decade of new challenges

The rate of change is always about the same, as fast as possible, but the directions have shifted back on themselves. Twenty years ago, when I was starting out in IT, the focus was to move off the centralized computing platform with terminal interfaces in favor of multiple servers and client access. Now we're shifting back to a centralized platform. Of course, back then, it was mainframes and terminals versus today being the cloud and devices (phones, tablets, laptops, etc). I suppose with the [promise of high mobile bandwidth](https://techcrunch.com/2015/08/15/the-promise-of-5g/) that makes sense.

## Containerization

I think in the next 10 years, probably more like 5, containers are going to push out Virtual Machines. Less to manage, easier to spin up, and can use Orchestrators like Kubernetes to manage the systems. Why troubleshoot a failed system, if you can just quickly replace it? I also think it'll be an easy way to horizontally scale SQL Availability Groups (this is probably a pipe dream) across containers. Toss a load balancer in the mix....and suddenly you're not as constrained on reads. The only problem to solve is that of licensing. Of course, if it's a write-heavy application then you may still need the power of a traditional node.

## Infrastructure as Code (IaC)

IaC is the next move forward in the infrastructure space. Why? Because why should we be sitting around clicking next when we can define a server or application with declarative code and it takes care of the build? Oh ya, and add in source control, the config file is maintained, documented, and versioned. I don't know why anyone would fight this, except those who feel threatened. Plus, a positive is when Azure has a sale and you have to move your entire organization over from AWS, or vice versa, it'll be a lot less work the second time.

The downside is, I think those coming up in the ranks now and in the future are going to lose some valuable troubleshooting skills. Although I say that, I can't remember the last time I've thought about if a comm port issue that was related to an IRQ or memory address. It is good for trivia when you ask the younger crowd what the difference is between the different comm ports are.

## Source Control

I honestly don't know why it has taken so long for Source Control to catch on outside of development. I mean I get it, there's a bit of a learning curve and branching isn't something I have completely figured out yet. But dammit, it makes working on the same piece of code so much easier for me to work on between different computers. With my team at work finally embracing it, I'm looking forward to seeing how it helps us.

## DevOps

I think the '20s are going to be the decade of DevOps. And by that, I mean on 12/31/29, DevOps will have been the default for years. Spinning up servers by hand will be a rare exception, but more like a novelty. And all of the previous things I listed are components of DevOps anyway. See, it's already happening.

## Change is Change

Change can be either good or bad, but I think it depends on how you see the change. Regardless of how you view it, it's going to happen. Perhaps instead of fighting it, embrace it.....use the new decade as a launch platform to retool your skillset, embrace new technology, geek out and have some fun learning.