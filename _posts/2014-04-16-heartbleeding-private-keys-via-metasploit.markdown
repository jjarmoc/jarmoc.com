---
layout: post
title: "Heartbleeding Private Keys via Metasploit"
date: 2014-04-16 21:58
comments: true
categories: Metasploit Heartbleed Crypto
---

I've resisted posting about [Heartbleed](http://www.heartbleed.com), just because I don't think there's much more to be said, so I'll keep this brief.  By now you know that it allows for reading raw server memory including things like credentials, session tokens, and whatever else happens to be in the web server processes memory near where your request is allocated.  This also includes encryption keys used to secure SSL/TLS sessions.  

Thanks, at least in part, to [Cloudflare's challenge](http://blog.cloudflare.com/the-results-of-the-cloudflare-challenge) it's been proven by a [number](https://blog.indutny.com/9.heartbleed) [of](https://news.ycombinator.com/item?id=7577659) [people](http://blog.erratasec.com/2014/04/cloudflare-challenge-writeup.html) that [RSA private keys]("http://en.wikipedia.org/wiki/RSA_(cryptosystem)") can be recovered with enough attempts, and a little math.

So what makes my [successful attack](https://gist.github.com/jjarmoc/10890697) different?  I did it with [a Metasploit module](https://github.com/rapid7/metasploit-framework/pull/3268).
<!-- more -->

Here's what it looked like on an earlier version of the module;
<img src="{{site.url}}/jekyll-test/images/heartbleed/success.png" style="display: block; margin: auto;" /> 

Being able to use Metasploit to dump keys just means one less tool to use, and the benefits of Metasploit's database workflow, storage of keys as loot, etc.

# Okay, maybe not so brief after all

I had a pretty good head start, thanks to [@Firefart](https://github.com/firefart) (Okay, there's words I never expected to say!) and everyone else who worked on earlier versions of the Metasploit module.  It was already dumping memory reliably, so all that was left was to parse it and build a key.

I already had [code](https://github.com/jjarmoc/csaw2012_cert_app/blob/master/lib/openssl-patch.rb) for generating RSA keys from chosen factors written for a [challenge](https://github.com/jjarmoc/csaw2012_cert_app) I put together for [CSAW CTF](https://ctf.isis.poly.edu/) a couple years back, so that part was simple.  All I needed to do was retrieve the server's certificate, parse the memory dump looking for factors of that certificate's public key, and rebuild the key using my existing code.  

It really wasn't all that hard, and my technique is no different than anyone else's, but the end result is an [improved Metasploit module that anyone can use.](https://github.com/rapid7/metasploit-framework/pull/3268)  This, to me, embodies what Open Source, and the Metasploit community are all about; taking the work of others, adding your own improvements, and releasing it back to the community while having a little fun along the way. 

# ## TODO

I've still got a laundry list of things I'd like to do with this specific module.  I'm posting them here mostly to hold myself to them, but also in hopes that maybe someone else with some time and motivation on their hands who may be looking for a place to jump in and contribute to Metasploit can get started.  This list may grow a bit as I think about it more, there's seemingly no end to the impacts of this bug.

* Modify bleed code to function after the SSL handshake, so our requests and leaked data are encrypted.
* Re-use the same connection for successive bleeds.
* Parse server handshake responses and retrieve certs, so we can dump keys from protocols that use TLS_CALLBACKS.
* Add support for [DTLS](http://en.wikipedia.org/wiki/Datagram_Transport_Layer_Security), though this might need a seperate module.
* Maybe support extraction of other key types; DSA and ECDSA.  They aren't anywhere near as common as RSA, but this could be a fun Crypto exercise.

Have fun, and [stay out of trouble!](http://arstechnica.com/tech-policy/2014/04/heartbleed-hacker-arrested-charged-in-connection-to-malicious-bug-exploit/)



