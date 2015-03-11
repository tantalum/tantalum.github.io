---
layout: post
title: A Quick and Dirty Perl Hack
---

Well a while back I read a book to learn perl. I have seen perl code before and I, like very one else, have heard about it. I did the standard programming practices (e.g: Remove blank lines) and wrote a couple of utilities that I used to clean up some data I had. But after that I dropped it for about 4 month. Today I was looking a my copy of the Perl Cookbook and started reading the web stuff chapter. I read a couple of recipes and came up with this:

<!--more-->

```
#!/usr/bin/perl -w

use strict;
use LWP::UserAgent;
use HTTP::Response;
use URI::Heuristic;
use HTML::LinkExtor;

my $cmd_url = shift ; 
my $url = URI::Heuristic::uf_urlstr($cmd_url) ;
my $ua = LWP::UserAgent->new() ;
$ua->agent("Schmozilla/v9.3 BETA") ;
my $resp = $ua->get($url, Referer=>"http://www.yellowdog.com") ;
die "Invalid response\n" if $resp->is_error() ;
my $lp = HTML::LinkExtor->new(undef, $url) ;
$lp->parse($resp->content()) ;
my @links = $lp->links;
for (@links){
  my($el_type, $attr_name, $attr_val) = @$_ ;
  if($el_type eq 'a' && $attr_name eq 'href'){
    if($attr_val->scheme eq "http"){
      #printf "checking %s\n", $attr_val;
      my $agent = LWP::UserAgent->new(agent=>"Schmozilla/v9.3 BETA");
      my $response = $agent->get($attr_val, Referer=>$url) ;
      if($response->is_error()){
 printf "%s is dead\n", $attr_val ;
 print $response->message, "\n";
      }
    }
  }
}

```

I know. I know. No comment. I didn't even tell you what it does but if you read this blog your probably a smart person and can figure it out by your self...
