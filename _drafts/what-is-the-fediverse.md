---
title: "What is the Fediverse"
layout: post
date: 2020-10-16
tags: fediverse, social media
---

# Outline

* The distributed nature of the Fediverse
* Implication of decentralization
    - Connectivity across instances
    - Funding, management and survivability
    - Search
    - Moderation & Spam

## What is the Fediverse

By the time you are reading this you've probably heard of "The Fediverse", or at least Mastodon. At the time I am writing this post, the Fediverse is the fastest growing social media "platform". At the same time, most people don't know what the Fediverse is or what makes it different from Facebook, TikTok or X. 

Lets start with what the Fediverse is. Wikipedia defines the [Fediverse](https://en.wikipedia.org/wiki/Fediverse) as:

> The fediverse (a portmanteau of "federation" and "universe") is an ensemble of federated (i.e. interconnected) servers that are used for web publishing [...] and file hosting, but which, while independently hosted, can communicate with each other. 

More commonly the Fediverse is defined as the set of applications (e.g. Mastodon or PeerTube) that can communicate using the [ActivityPub](https://www.w3.org/TR/activitypub/) protocol, the servers and groups that host the applications, and the users of those applications and the content they create. All of that together makes up the Fediverse. The key difference between the Fediverse and traditional social media sites like Facebook or YouTube is the addition of the ActivityPub protocol and servers into the mix. Anyone can setup a Mastodon server, and anyone can write a new application that uses the ActivityPub protocol, and that server or that application can (within reason) communicate with any other server or application in the Fediverse

<!--more-->

## Decentralization

Let's make all that more concrete. The two most popular Fediverse applications are Mastodon form micro blogging and PeerTube for video sharing. [Noc.social](https://noc.social/home) hosts a Mastodon instance and I have an account on that instance. I can follow other Mastodon users either on my home instance (noc.social in this case) or on other instance such as [infosec.exchange](https://infosec.exchange/explore). I can also follow a PeerTube account, like [babbagebool](https://diode.zone/a/babbageboole/video-channels) because PeerTube also uses the ActivityPub protocol. A common, but imperfect, analogy is email. You can have an account on gmail.com and your friend can have an email account at hotmail.com and you can both still send and receive emails with each other.

![Centralized vs. Decentralizes vs. Distributed](/img/centralized-vs-decentralized-vs-distributed.png)

Facebook, X (aka Twitter) and YouTube are all centralized services. If I want to talk to or interact with any of my friends on Facebook, I need to have a Facebook account and log into Facebook. Same thing with YouTube, if I want to follow a channel or comment on a video on YouTube, I have to have a YouTube account. Cross posting content from Facebook to X is not an option. 

Along the same line there is only 1 Facebook. The Facebook.com website is run by Meta. No one else can create another Facebook instance and even if they manage to create something that is _Facebook like_, they will never be allowed to interact with the _real Facebook_. Whereas with The Fediverse anyone can spin up a site that can interact with any other site on The Fediverse. Anyone can spin up a new Mastodon site, but no one will be able to spin up a new X. In fact there are even services out there that will help you spin up your own dedicated Mastodon instance. The official Mastodon documentation has a section on [how to spin up your own instance](https://docs.joinmastodon.org/user/run-your-own/).

Decentralization is the key difference between The Fediverse and traditional social media platforms and brings with it a number of fundamental differences in how you, as a user, might approach this decentralized world.

### Instances, Funding and Accountability

When you sign up for a Facebook account the only option you have to so sign up on Facebook. Facebook lets you create an account and hosts your data for free and does that for billions of users costing Facebook billions of dollars a year. At the same time by signing up with Facebook you agree to have your data mined and shared with advertisers and affiliates, you also agree to have ads and content you never signed up for displayed in your feed. You also agree to abide by Facebook's terms of service, and content moderation policies. If you don't like that arrangement, your only option is to not sign up with Facebook. 

When you sign up for a Mastodon account, you first have to pick the _instance_ you want to sign up with. Once you've picked an instance you can start following users on other instances, and users on other instances can start following you and interacting with your content. So why have instances in the first place?

#### Moderation and Acceptable Use

Each instance's administrators are responsible for defining the instance's acceptable use policy and enforcing that policy through moderation. This means that different instance will have different policies about what content is and isn't allowed. For example some instances allow adult (aka. NSFW) content, while some instances have very strict no NSFW content policies. The instance administrators and moderators are also responsible for enforcing the instance's policy and taking action against offending users. There are a [number of actions that can be taken against offending users](https://docs.joinmastodon.org/admin/moderation/), ranging from marking the account as sensitive alerting other users to potentially-harmful content all the way to suspending the account, effectively deleting the account and the content associated with it.

Because of the Federated nature of the Fediverse, a user X on instance A might follow or interact with another user Y on instance B with a more lax content policy. This would cause the violating posts from Y to show up on instance A's federated timeline. In that case instance A's moderators might take action against user Y, or against the entire B instance. The thing to keep in mind is that whatever enforcement action instance A's admins take, it will only impact users on instance A. A third instance might take completely different moderation actions or no action at all. 

This means users have the choice, and associated responsibility, to choose an instance whose usage guidelines aligns with their views and tastes.

TODO: Example - Gab mass blocking.

#### Funding

The current economic model for mainstream social media platforms is simple: We provide them with data an and attention which they sell to advertisers for money, and in return we get to use their platforms for free. As the old saying goes "If you aren't paying for the product, you are the product". TODO: Negative impact of model and misaligned incentives.

In may ways the Fediverse is a reaction to the impact of the this model on social media users and society at large. However, at the end of the day _someone_ has to pay for the service. Bandwidth, server space, administrator and moderator time all cost real money. Almost all mastodon instance allow free signup and are run on a volunteer basis, however this can only last so long. As instances grow it will be impossible for volunteers to provide the resources needed to maintain the instance without some form of funding. 

The Fediverse's funding model is still in flux. If we as users want to keep the incentives of the Fediverse operators aligned with our interest, we need to fund the platforms. Most instances accept donations and setting up a recurring donation to your instance admin (even if it is as low as $1) is common practice by users. If users don't provide the means to fund instances, the instances will either be limited in their growth and unstable, or instance admins will have to find alternative funding sources. Advertising is one option, but hasn't gain traction yet because most user joined the Fediverse to escape that very economic model. Another model that might evolve is for organizations to setup and manage instances for their members, similar to how most corporate email works today. All this means is that early users and adopters will have an outsized impact on the Fediverse's economic model and future evolution, if any.

### Spam

As the Fediverse grows there will be a strong incentive by spammers to target Fediverse users, especially since the Fediverse is a distributed environment which means different instances will have different approaches and abilities to manage Spam. Centralizes systems have a major advantage in combating Spam. Once a spammer has been identified on a centralized system, the can immediately be banned and their content removed, significantly raising the cost for spammers to reach users. The other side of that is that if a legitimate user is misidentified as a spam, the might not have any options for reaching users. 

In a decentralized system like the Fediverse, and instance by identify and block a spammer, whither it is a single users or a spamming instance. However, non of the other instances in the Fediverse are aware of the spammer and will continue to be able to reach users on those instances. Even if a spammer was know to all (or at least the vast majority) if instances on the Fediverse, it is trivial for a spammer to start a new instance repeating the whole cycle again. This is a very difficult problem to solve. People as old as I am will remember the hay-day of email spam, and in reality we never found a good system for combating spam on email. It wasn't until the vast majority of email was controlled by a few centralized entities (i.e. Gmail, Hotmail and Yahoo) that significant gains against spammers were made. In fact today most email providers will automatically mark any message as Spam unless it originated from one of the major "known good" email providers. 

TODO: Closing thoughts - Like email Spam will be a evolving issue in the Fediverse

# Search and Reach
