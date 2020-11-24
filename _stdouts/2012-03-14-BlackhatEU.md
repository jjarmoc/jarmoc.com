---
title: Blackhat Europe 2012
subtitle: SSL Interception Proxies and Transitive Trust
date: 2012-03-14
slides: http://media.blackhat.com/bh-eu-12/Jarmoc/bh-eu-12-Jarmoc-SSL_TLS_Interception-Slides.pdf
video: https://media.blackhat.com/bh-eu-12/Jarmoc/bh-eu-12-Jarmoc-SSL-TLS.mp4
whitepaper: https://media.blackhat.com/bh-eu-12/Jarmoc/bh-eu-12-Jarmoc-SSL_TLS_Interception-WP.pdf
---

SSL/TLS is entrusted with securing many of the communications services we take for granted in our connected world. Threat actors are also aware of the advantages offered by encrypted communication channels, and increasingly utilize encryption for exploit delivery, malware command-and-control and data exfiltration.

To counter these tactics, organizations are increasingly deploying security controls that intercept end-to-end SSL/TLS channels. Web proxies, DLP systems, specialized threat detection solutions, and network IPSs now offer functionality to intercept, inspect and filter encrypted traffic. Similar functionality is also present in lawful intercept systems and solutions enabling the broad surveillance of encrypted communications by governments. Broadly classified as "SSL/TLS Interception Proxies," these solutions act as man-in-the-middle, violating the end-to-end security guarantees promised by SSL/TLS.

In this presentation we'll explore a phenomenon known as "transitive trust," and explain how deployment of SSL/TLS interception solutions can introduce new vulnerabilities. We detail a collection of new vulnerabilities in widely used interception proxies first discovered by the Dell SecureWorks CTU and responsibly disclosed to the impacted vendors. These vulnerabilities enable attackers to more easily intercept and modify secure communications. In addition, we will introduce a public web site that organizations can use to quickly and easily test for these flaws.