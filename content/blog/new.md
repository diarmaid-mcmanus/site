---
title: "TPotCE"
date: 2022-10-15T18:25:11Z
draft: true
---

Installing TPotCE on a Debian 11 server is very straightforward. Clone tpotce, change the username & password in the config file, and run the installer. I installed early in the evening of October 15th. 

This is my first time using TPot in a year or so, and I just learned of the dps.sh script. It's a nice overview. The dashboard seems updated, or more intuitive. I like it. I set up an a record pointing this to honeypot.diarmaidmcmanus.com, to make it really clear what's here. I also set rdns the same way.

Cowrie is my current focus. By the time I get to look at its dashboard in Kibana, a few minutes in, the honeypot has already been attacked by 4 unique source IPs. Only one command exec so far, `uname -nm`, but this would change within a few hours. By then, I saw many more commands in cowrie logs, and 3 unique hashes of files attempted download. TODO pulling the session id, searching for the session to recreate the stream, allows you to review full session. is interesting. Viewing the files it's very quick to tell I need a sandbox.

I want to take the downloads and uploaded files, find yara rules for them, and start IDing samples. This would be linked back to greynoise domain rep, VT sample rep, etc. Sandbox too. logstash as an event stream makes this pretty simple to do.

I'm also interested in making my own honeypots, so I will look into wordpot2 some, and dionaea to emulate http(s?) servers. 

when will it be tagged honeypot in shodan? As of now, it is not appearing as a honeypot, but I don't have access to tags.

Next, SSL via letsencrypt. 
