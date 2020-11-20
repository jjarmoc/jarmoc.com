---
layout: post
title: "Follow Up: BSJTF Packet Analysis Challenge"
date: 2013-05-24 21:42
comments: true
categories: 
---

Recently, I posted a [blog](http://jarmoc.com/blog/2013/05/22/bsjtf-ctf-writeup-what-in-the-name-of-zeus/) about a CTF challenge I participated in that involved some packet analysis.  Since then, I've had a chance to chat with [@dth0m](http://www.twitter.com/dth0m) who wrote that challenge.

He had some *very* interesting things to say, which I thought warranted another entry.  

If you haven't read the [previous post](http://jarmoc.com/blog/2013/05/22/bsjtf-ctf-writeup-what-in-the-name-of-zeus/) about this challenge, you might want to read that first.

<!-- more -->

###Updates from behind the scenes...

When I talked to dth0m, he expressed some suprise about how I solved the challenge.  Apparently, it wasn't his intended path.  His intention, as the name of that challenge implied, was that we would discover that only one IP from the packet capture was listed on the [abuse.ch ZeuS Tracker blocklist](https://zeustracker.abuse.ch/blocklist.php).  Obviously, I missed that hint, but talking to him about this ended up raising some really interesting points.

Neither my finding that the flag IP was the only one with a 0 in the second octet, nor the fact that it's TTL differed from the others was apparently intended.

He noted that the code he used to generate the pcap was somewhat cobbled together, using bits and pieces of other scripts he had around, which probably explains how these anomalies crept in.  For a one off task like this, especially considering this is just one of many challenges, it's easy to see how minor errors can creep in.  What's amazing is that they both pointed to the suspect packet.  

This may have ended up being a more realistic example of detecting malware than it could possibly have been designed.  Often when reviewing suspicious activity, what we're really looking for are unintended signs the attacker left behind unintentionally.  When looking at worm or DDoS related traffic, for example, it's not uncommon for random IP generators to have unique patterns in timing, IPs they do/don't probe, etc.  Often these sorts of indicators become valuable to the defensive analyst or incident responder.

I asked dth0m if he'd share the code he used to generate it, and he sent it to me.  I'm posting it here with his permission.  

``` python Code for Generating Challenge PCAP
from scapy.all import *

from random import randrange

i=1
f=1
while i < 600:
        not_valid = [10,127,169,172,192]
        first = randrange(1,256)
        while first in not_valid:
                first = randrange(1,256)
        ip = ".".join([str(first),str(randrange(1,256)),
        str(randrange(1,256)),str(randrange(1,256))])

        b = IP(dst=ip, ttl=60)/UDP(dport=(666), sport=(1337))/"...bacon...I mean beacon"
        send(b)
        i=i+1

a = IP(dst="37.0.122.152", ttl=20)/UDP(dport=(666), sport=(1337))/"...bacon...I mean beacon"
send(a)

while f < 600:
        not_valid = [10,127,169,172,192]
        first = randrange(1,256)
        while first in not_valid:
                first = randrange(1,256)
        ip = ".".join([str(first),str(randrange(1,256)),
        str(randrange(1,256)),str(randrange(1,256))])

        c = IP(dst=ip, ttl=60)/UDP(dport=(666), sport=(1337))/"...bacon...I mean beacon"
        send(c)
        f=f+1
```

There's a few portions of the code I want to highlight.

- Lines 7 and 22 start the loops that generate the surrounding packets.  In this code, there's 600 each so we'd expect to see our flag packet in the very center of a 1201 packet capture.  Apparently this was changed last minute to make things a little less symmetric and to give us more packets to sift through.
- Lines 8 and 23 set the values that will be skipped in the first octet, as we found in our earlier analysis.
- All the [randrange()](http://stackoverflow.com/questions/3540431/difference-between-random-randint-vs-randrange) calls look for a number between 1 and 256.  This function has an inclusive start value and an exclusiver stop, so the minimum is 1 and the max is 255.  0 is absent from his ranges, which explains why it doesn't appear in the randomly generated traffic.
- Line 19 generates the flag packet.  Notice it's TTL is 20, while the other generators use a TTL of 60.  This explains the variance, and is likely attributed to code reuse introducing a small difference.

Realizing these two artifacts weren't intended is something I found really interesting.  It makes what was already an interesting challenge even more so.

Thanks again to [@dth0m](http://www.twitter.com/dth0m) for putting together the challenge.  And *especially* for his willingness to let me share his code publicly, and point out some of his unintended fingerprints in this challenge.

