---
layout: post
title: "Open Redirect in benefits.gov for Eight Years"
date: 2013-10-14 08:10
comments: true
categories: 
---

While digging into some old phishing campaigns, I came across something interesting.  It's nothing earth-shattering, but the sheer length of time this vulnerability has remained viable, even after being identified in the press as actively exploited in the wild, is noteworthy.

You see, nearly eight years after being abused in a phishing campaign, the vulnerability is still present.

<!--more-->

It starts with a [News.com piece from November 2005](http://news.cnet.com/Phishers-use-IRS-tax-refund-as-bait/2100-7349_3-5977588.html)  The article discusses a [phishing](http://en.wikipedia.org/wiki/Phishing) campaign, which purports to be an email from the IRS, but instead takes the victim to a fraudulent site which asks for their Social Security Number, Credit Card Details, and IRS Filing information.

 "With GovBenefits.gov there is a great opportunity for criminals by posing as the IRS to get a great deal of information, including your credit card details and Social Security number."
										--	Graham Cluley, Sophos

There's more details available in [Sophos' original report.](http://www.sophos.com/en-us/press-office/press-releases/2005/11/irsphish.aspx)

For our purpose, the interesting part is that the [News.com](http://news.cnet.com/Phishers-use-IRS-tax-refund-as-bait/2100-7349_3-5977588.html) article gives an example of the vulnerable [Open-Redirect](http://cwe.mitre.org/data/definitions/601.html)

```
http://www.govbenefits.gov/govbenefits/externalLink.jhtml?url=http://www.news.com
```
-- Example Redirect URL

News.com apparently contacted the Department of Labor about the issue back in 2005, and cites an unnamed representative as stating The government is aware of the issue and is working to fix it."

Surely the government once made aware of an active campaign abusing one of their sites, and promising to fix it, can manage that fix in eight years.  Right?  Well, let's see...

Current State
{% highlight plain %}
$ curl -v http://www.govbenefits.gov/govbenefits/externalLink.jhtml?url=http://www.news.com
* About to connect() to www.govbenefits.gov port 80 (#0)
*   Trying 23.72.82.40...
* connected
* Connected to www.govbenefits.gov (23.72.82.40) port 80 (#0)
> GET /govbenefits/externalLink.jhtml?url=http://www.news.com HTTP/1.1
> User-Agent: curl/7.24.0 (x86_64-apple-darwin12.0) libcurl/7.24.0 OpenSSL/0.9.8y zlib/1.2.5
> Host: www.govbenefits.gov
> Accept: */*
> 
< HTTP/1.1 301 Moved Permanently
< Server: AkamaiGHost
< Content-Length: 0
< Location: http://www.benefits.govgovbenefits/externalLink.jhtml?url=http://www.news.com
< Date: Mon, 14 Oct 2013 13:35:45 GMT
< 
{% endhighlight %}

Some time has passed since all this was written, and it seems govbenefits.gov is no more.  All visits to the site now redirect with a 301 Moved Permanently response to benefits.gov.  So to, the vulnerable URL cited by News.com redirects to the newer benefits.gov branding.

Interestingly, the Location header it gives is malformed.  Their redirect seems to strip the first / that trails the hostname, so it probably breaks a good number of legitimate links, bookmarks, etc.  as well.  This effectively means that the redirect will only work for bare requests to govbenefits.gov without any path or file name following.

If we correct it, by adding the missing slash, we see a much more interesting result.  

html Corrected for bad site-wide 301

{% highlight bash %}
$ curl http://www.benefits.gov/govbenefits/externalLink.jhtml?url=http://www.news.com
<html>
<head>
	<script>
			window.location.href = "http://www.news.com";
	</script>
</head>
<body>
</body>
</html>
{% endhighlight %}

So here we have a Javascript redirect to www.news.com.  Remember, this was publicly reported and associated with a Phishing campaign masquerading as the IRS almost eight years ago!

Open-Redirects aren't the most severe of vulnerabilities, and abuse doesn't really damage the vulnerable organization, so it's not uncommon for them to linger.  They are, however, of much more significance when the vulnerable site carries additional trustworthiness in the minds of target victims.  The IRS Phishing campaign noted by Sophos in 2005 shows a good example of the impact.  It's a shame that even after being notified of the vulnerability, it appears those responsible for securing government web properties, and the trust the general public places in them, are unwilling or unable to respond.  