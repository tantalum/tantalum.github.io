---
layout: post
title: The "Freedom" of Ubuntu
---

## The Problem

I've been running Ubuntu as my main OS for pretty much a couple of years now and I have to say I LOVE it. The hardware support is great, tons of apps, and its free. Well "free" as in beer for sure, but not really "free" as in freedom.

This past summer I moved back to Syria, where, as you all know, the US has restricted the imports on software products for "National Security" reasons. So I've gotten used to Google Code being blocked, SourceForge not letting me download stuff and all that jazz. Still Ubuntu worked like a champ and I could download anything I wanted from the repos. That was until I decided to update my system to 11.04.

<!--more-->

After spending about 16 hours downloading the updates (Slow internet over here), I was greeted with this little error:

```
Err http://us.archive.ubuntu.com/ubuntu/ maverick/main python-libproxy all 0.3.1-1ubuntu1
  403  Forbidden [IP: 91.189.92.182 80]

```

So according to Canonical Ltd I don't have the right to access Open Source software... FUUUUCCKKK!

## Solution 1

The first obvious solution is to try a mirror that's outside of the US... Somewhere like Ukraine, but that didn't work. Canonical is in control.

## TOR to the Rescue

The second obvious solution is to make it look like I'm coming from an IP that has the "right" to download the software. This is where the mighty [Tor](https://www.torproject.org/) comes into play. Thankfully the Network Proxy Manager lets you apply system wide proxies. First I tried setting the SOCKS proxy to point to Tor but that didn't work. What you need is to point the HTTP proxy to a proxy that routs through Tor, Polipo or Privoxy will both do the trick. So after I set everything up and re-ran the update manager and now I'm working on my shiny new 11.04 setup... In your face Canonical.


