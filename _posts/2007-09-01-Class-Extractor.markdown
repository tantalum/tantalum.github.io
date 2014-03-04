---
layout: post
title: Class Extractor
---
# Class Extractor

Ok so I wanted to do a little something in perl because it's been a while since I've written anything in it. Since I still have the code from my myspace profile I decided to write a little script to extract the css classes from it. Well here it is:

```perl
#use strict ;
use HTML::Parser ;

sub handle_start{
  shift ;
  #print $tag."\n" ;  
  my @attrs = @_ ;
  for (@attrs) {
    my %hash = %$_ ;
    #print "\t".%hash."\n" ;
     for(keys %hash){
       if(m/^class$/){
  $class = $hash{$_} ;
  if(defined $class_count{$class}){
    $class_count{$class} += 1;
  }
  else{
    $class_count{$class} = 0;
  }
       }
     }
  }
}

$parser = HTML::Parser->new ;
$parser->handler(start=>\&handle_start,  'tag, attr') ;
while(){
  $parser->parse($_) ;
}

for(keys %class_count){
  print $_." : ".$class_count{$_}."\n" ;
}
```

It seems to work pretty well. I tried to write the same script in python but it'shtml parser it too uptight or myspace so.... I fell kinda guilty because I like python more than perl but sometimes perl's just kinda cool to play with.

Here is what I got when I ran my unedited profile page throught it.

```
nametext : 0
redtext : 1
bodyContent : 0
txt : 0
redlink : 0
redbtext : 0
extendedNetwork : 0
contactTable : 0
tdborder : 2
text : 28
btext : 1
navbar : 14
friendSpace : 0
blacktext12 : 0
text tdborder : 1
whitetext12 : 1
commentlinks : 1
navigationBar : 0
userProfileDetail : 0
friendsComments : 0
blacktext10 : 1
userProfileURL : 0
lightbluetext8 : 8
latestBlogEntry : 0
profileInfo : 0
blurbs : 0
orangetext15 : 4
ImgOnlineNow : 1

```

So I hope that a list of all the classes in myspace is usefull. I think a little more usefull is the a script that extracts classes and count from a web page


