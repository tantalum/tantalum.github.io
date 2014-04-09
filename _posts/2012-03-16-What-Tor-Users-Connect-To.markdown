---
layout: post
title: What Tor Users Connect To
---
# What Tor Users Connect To

The other day I was playing with my Tor node's configuration when I found out that with absolutely no trickery you can get Tor to log some interesting addresses. One of the things Tor logs is the address of edge (aka. exit) connections, which tells you what the node is connecting to but not who the connection is for.

Naturally it would be interesting to know what other people are using Tor for, so I kept a couple of days worth of logs and wrote this little perl script to parse them out for me.

```perl
#!/usr/bin/env perl

%address_counter = ();

while($s = <STDIN>){
 if($s =~ m/connection_edge_finished_connecting/){
  @parts = split /\s/, $s;
  ($address, $port) = split /:/, $parts[8];
  $address =~ s/"//g;

  if(exists $address_counter{$address}){
   $address_counter{$address} += 1;
  }else{
   $address_counter{$address} = 1;
  }
 }
}

foreach $adr (sort {$address_counter{$b} cmp $address_counter{$a} } keys %address_counter){
 print $adr.": ".$address_counter{$adr}."\n";
}

```

Here are the results. One interesting thing is that the top 8 addresses were accessed more than 90 times, and then the number drops straight down to 9 connections for sites numbers 8 and 9


- 91.215.184.72: 98
- 123.125.65.93: 97
- 74.6.238.254: 97
- 212.219.56.133: 96
- 66.244.147.240: 94
- 85.17.80.121: 91
- 93.158.121.60: 91
- 2.18.178.110: 90
- 66.220.149.32: 9
- 174.37.33.236: 9
