---
layout: post
title: "BSJTF CTF Writeup -  On the Rogue Agents Tail"
date: 2013-05-24 17:33
comments: true
categories: CTF
author: DrBearsec
---

This is a guest post, mostly, from [@drbearsec](http://www.twitter.com/drbearsec).  I say mostly, because I'm formatting his words somewhat.  I did not take part in this challenge, so the solve is totally the work of he and his crack CTF team (currently in 2nd place overall, and gaining).  The team is compromised of @drbearsec, [@essobi](http://www.twitter.com/essobi), [@Babs0matic](http://www.twitter.com/babs0matic), and [@j0hnnyxm4s](http://www.twitter.com/j0hnnyxm4s).

For this challenge, teams were given a series of password hashes.  Cracking them would yield the key.
<!-- more -->
```
5C53884888CBC6263A4B8CECC81EBCFB
DECC8CF50CDD236FED6590F8AE87B93C
fc583783938252ea40514ce8ea6364ef
CE37DC094B14829D9C8EF10F617B591C
8c1d108d5f8b4f6854351edaa2340afb
1AF5F8D3305E0077ACC8EC27779BE3D4
E187AAFDF7AB986BAAD3B435B51404EE
```

Here's what the good Doctor says:
***
Its not about brute force but creative thinking.  In this case we had a great [clue](https://twitter.com/bsjtf/status/337357050122215424) and worked from there.

So we took the initial hashes and ran them through [ophcrack's online cracker](http://www.objectif-securite.ch/en/ophcrack.php).  That gave us THERE IS NO, HARD I WILL, and KEY=...

After trying the others in multiple online crackers we started to look at the clue and what we had.  Admins told us that the challenge did not need to be brute forced and was solvable by thinking outside the box.

So we were thinking maybe the hashes were a fortune cookie message.  So we googled the phrases "THERE IS NO" and "HARD I WILL" at the same time... 

INTERESTING RESULT... A quote from the Bourne Identity... a movie about a rogue spy... and here we are playing in a CTF about spies.  HMM

So we try the quote... no luck.

After verifying with the Admins that the flag was working right, we then went about turning the quote into hashes using a hash generator.  Hashes were a mix of NTLM and MD5.  By trying different combinations of the words, we were able to get all of the hashes...  Unfortunately, the last one we cracked held the answer to the puzzle.  Once they were all cracked the message read:

THERE IS NO measure to how fast and how HARD I WILL bring this fight to your KEY=...

The last word of the quote is "doorstep".  

*And X gets the Flag!*

### Other thoughts

You could also have created a word list with the quote words.  We would have originally done this but we were not near computers and started doing it on our cells with the hash generator.  Once we got a few just finished off that way.

***

Jeff again.

I thought this was a pretty interesting way to go about solving this particular challenge.  They leveraged what they knew, to guess at what they didn't know.  While it's an odd scenario to have password hashes that're linked in this way, it definitely demonstrates the value of knowing your target.  This is definitely the sort of thing you see in cryptographic attacks often; knowing partial plaintext may let you infer, or at least guess more reasonably, at the rest.  It also shows the value of knowing your target and building appropriate wordlists.

Cracking isn't solely about brute force.

Congrats to [@drbearsec](http://www.twitter.com/drbearsec), [@essobi](http://www.twitter.com/essobi), [@Babs0matic](http://www.twitter.com/babs0matic), and [@j0hnnyxm4s](http://www.twitter.com/j0hnnyxm4s) for the solve!
