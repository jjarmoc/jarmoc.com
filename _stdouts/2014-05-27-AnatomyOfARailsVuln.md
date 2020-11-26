---
title: "The Anatomy of a Rails Vulnerability"
subtitle: "CVE-2014-0130: From Directory Traversal to Shell"
date: 2014-05-27
whitepaper: https://www.nccgroup.com/globalassets/our-research/us/whitepapers/AnatomyOfRailsVuln-CVE-2014-0130.pdf
---

On May 6th 2014, the Ruby on Rails team released updates to address a security vulnerability involving the 'implicit render' feature, and identified it as CVE-2014-0130.1 In their advisory, they go on to describe a Directory Traversal vulnerability involving globbing routes including
the string '*action'. An amendment to that advisory3 broadens the warning, stating that there are 'additional attack vectors' and advising that all users upgrade to a fixed version.

In this paper, we explore the attack vectors of this vulnerability, as well additional impacts beyond simple file retrieval. These include remote code execution across a variety of Ruby on Rails deployment environments. If you take away one thing, it should be this: vulnerability impact is not always clear without close review. In this case, the advisories say arbitrary file read, with a highly unusual configuration. We’ll see how to achieve remote code execution with a more common setup.