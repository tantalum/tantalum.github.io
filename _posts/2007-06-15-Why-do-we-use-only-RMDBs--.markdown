---
layout: post
title: Why do we use only RMDBs?!
---

Well It's aas far as the former issue of figuring out how to get Mac and FreeBSD to play nice together I haven't done anything. I think I'm just gonna install BootCamp, create a small partition to boot freeBSD off of and get it over with.

I had a little debate this week with the people in the Postgresql Novice mailing list. The thing is there are other formats to store data other than the row/column format (ala trees) but there doesnt seem to exist any ACID capable system to manage such formats and I was asking why the situation was so. The Postgres mailing list was the wrong place to bring that up but I still dont have an answer. There are a couple of LDAP implementation for storing tree-like data but thats about it. I understand that alot of other formats can be emulated using RMDBs but why hammer that data to fit the system? Why hasn't any one made a system that fits the data?


