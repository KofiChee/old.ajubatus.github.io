---
excerpt: "I decided to look for a replacement to my self-hosted email solution"
header:
  overlay_color: "#333"
---

Originally, when I registered my own domain, I decided I wanted to use it for a professional looking email to use on my CV and for applying to jobs. When I initially planned this, I had read an [Ars Technica article](http://arstechnica.com/information-technology/2014/02/how-to-run-your-own-e-mail-server-with-your-own-domain-part-1/) about setting up an email server. With this info, I decided to read more into running your own email server, and came across IRedMail. For almost a year now, iRedMail is what I've been using, but between needing to constantly monitor access logs for numerous services, triple checking security settings every time a change is made, spend an eternity updating everything and dealing with mail delivery issues, I started to get frustrated.


I decided to look for a replacement, either an all in one suite that is regularly updated (and libre if I can get it), or a solution hosted elsewhere that allows me to use my own domain for equal or lesser cost to running the server.


I compared two appropriate solutions for this, Zoho Mail and Zimbra Suite. Zoho Mail is a service that provides email hosting for custom domains, they offer a free tier, limited to 1 domain and 25 users. Zimbra Suite is a locally installed collection of mail software, similar to IRedMail, but with a simpler update procedure and less headaches from maintenance.


In the end, I went with Zoho Mail, the limitations of a single domain and 25 users are perfect for me. Another alternative I was looking at was Zimbra suite, however given the problems I had running IRedMail on its VPS due to lack of memory, I decided against it.

The migration was relatively simple, it involved siging up for Zoho, tweaking my DNS records and then running a script to import my old mail. After that, all I had to do was point my email clients to the new IMAP and SMTP addresses and I was finished.
