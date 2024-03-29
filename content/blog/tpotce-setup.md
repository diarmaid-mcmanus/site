---
title: "TPotCE Setup"
date: 2022-10-16T15:40:20Z
draft: false
---

### What is the TPot Honeypot?
The [TPot Community Edition](https://github.com/telekom-security/tpotce) Honeypot is a collection of simulated services to entice hackers, plus the entire management system needed to run your honeypots and investigate the findings. TPot gives you the tools needed to start capturing threats quickly, and build out your own data processing pipeline to further analyze these threats. Already fantastic at [release](https://github.security.telekom.com/2015/03/honeypot-tpot-concept.html) in 2015, it has come a long way since then.

### TPotCE Setup & First Look
Installing TPot on a Debian 11 server is very straightforward. Clone TPot, change the username & password in the config file, and run the installer. I installed early in the evening of October 15th. 

This is my first time using TPot in a year or so, and I just learned of the dps.sh script. It's a nice overview. The Kibana dashboard seems updated, or more intuitive. I like it. I set up DNS for my honeypot of honeypot.diarmaidmcmanus.com, to make it really clear what's here. I also set rDNS the same way.

Cowrie is my main focus right now. By the time I get to look at its dashboard in Kibana, a few minutes in, the honeypot has already been attacked by 4 unique source IPs. Only one command exec so far, `uname -nm`, but this would change within a few hours. By then, I saw many more commands in cowrie logs, and 3 unique hashes of files attempted download. By pulling the session identifier, searching for the session to recreate the stream, lets you to review full session, everything the attacker did, in one view. I noticed some [behaviour matching Mirai (Slide 5)](https://www.virusbulletin.com/uploads/pdf/conference_slides/2018/LiuWang-VB2018-TrackingMiraiVaraints.pdf). This search is a little cumbersome to perform in Kibana, though. 

### Future Work
Viewing the files, it's very quick to tell I need a sandbox. Most often a small loader is written, which attempts to download a compiled bot. Manually downloading & submitting to [VirusTotal](https://www.virustotal.com/gui/home/upload) identified some samples as Mirai, matching the behaviour I had seen earlier. On my laptop, I got yara rules running against these samples. 

After looking through some dashboards other than Cowrie, I've decided I want to correlate all the activity my honeypot sees from each IP address. Especially with the traditional Mirai loader, a multiple server architecture is needed to facilitate fast spreading. I'm curious to see what other bad activity these IPs participate in, and how this activity is spread over time. Then, enrich this with data from [GreyNoise](https://www.greynoise.io/), Shodan, and other platforms.

I also want to take the downloads and uploaded files and run yara rules against them to start IDing samples, upload them to VirusTotal, and detonate them in a sandbox. This could be used to enrich the IP data, including possibly clustering by c2, or at least extracting the c2. Logstash has plenty of [output plugins](https://www.elastic.co/guide/en/logstash/current/output-plugins.html) to make this pretty simple. However, I'm not sure how to integrate this into the TPot ecosystem yet.

Once I've done with that, I'm also interested in making my own honeypots. I must look into wordpot2 some, and dionaea honeypot to emulate web servers. I'd like to proxy any request that comes into certain paths on my other sites, such as /wp-admin/, to my honeypot. Ideally, this would happen with nginx configuration. I also need to set up proper TLS on my honeypot admin interface via letsencrypt.

Within about 20 hours, I had gained the honeypot tag in shodan. 
