---
layout: post
title: Python Link Extractor/tester
---

Ok so I was going to write a python counter part to yesterdays script. It's kind of funny because the difference between Python and Perl are so big that it's hard to write a good counter part in python.

For starters python doesnt have a HTML::LinkExtor module so I have to write my own link extractor which isn't hard. It's actually kind of easy. But it's also somethig I didn't have to do in perl.

<!--more-->

Another difference is that python has urllib and urllib2 instead of the LWP::* packages. You can get the same results using both packages but the method is a little different.

Also I cant find a package to guess the correct url from a partial url in python (think URI::Heurostic) so I have to write the whole url on the command line in the python version... Some thing I didn't have to do with perl

I started writing the python version at about midnight but I stopped after 10 lines of code because I need to decide if I want to write the python version in a way that it emulates the perl version. Or I could write the python version so that the result is the same but it uses the "pythonic" way of doing things.

If someone is reading this it really sounds like a rant against python. It isn't. I personally like python very much. It's the main language I code in.

 Any way I have to get up early tomorrow so it's time for bed


