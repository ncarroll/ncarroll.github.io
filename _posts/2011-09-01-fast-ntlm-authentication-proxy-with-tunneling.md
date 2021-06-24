---
layout:     post
title:      "Fast NTLM authentication proxy with tunneling"
date:       2011-09-01 14:18:00
---

If you are using Linux behind a corporate firewall that only supports Windows, and the Windows proxy authentication is causing you pain, then I suggest installing and using CNTLM.

The problem I was experiencing behind my corporate firewall is that I need to authenticate using the windows domain prepended to my username. It seems that you are not able to have a backslash in your username when setting your http_proxy environment variable using the below format.

https://username:password@host:port/

In other words I was getting strange errors when using the following in my .bash_profile.

export http_proxy=https://domainusername:password@host.com/

You can’t escape the backslash, nor wrap everything in quotes etc. The only solution I came across was to use an NTLM authentication proxy such as CNTLM, which is a fast NTLM authentication proxy written in C. The Ubuntu package is described as follows.

Cntlm is a fast and efficient NTLM proxy, with support for TCP/IP tunneling, authenticated connection caching, ACLs, proper daemon logging and behaviour and much more. It has up to ten times faster responses than similar NTLM proxies, while using by orders or magnitude less RAM and CPU. Manual page contains detailed information.

It can be installed using the command, but you’ll need to do this when you are connected directly to the internet, and thus bypassing your corporate proxy!

sudo apt-get install cntlm

You will then need to configure CNTLM by modifying the config file at /etc/cntlm.conf. You’ll need to specify your windows domain login credentials in the config file.

Once configured, restart CNTLM using the command:

sudo /etc/init.d/cntlm restart

Once CNTLM has been configured and restarted, you can then update your http_proxy settings to use http://localhost:3128, or whatever port number you used in the CNTLM config file. By default CNTLM listens on port 3128. Now you will be able to use apt-get, but this time behind your corporate proxy.

