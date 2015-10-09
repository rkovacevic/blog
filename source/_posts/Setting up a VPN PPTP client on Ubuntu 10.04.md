title: Setting up a VPN PPTP client on Ubuntu 10.04
date: 2011-10-21 15:03:00
tags: [linux,ubuntu,vpn]
---

Today I needed an external IP for a server deep inside corporate network, so the simplest solution for me was to purchase a cheap VPN service. I used [StrongVPN](http://strongvpn.com/), since it was the first I could find. This is a quick and dirty guide how to get it up and running on Ubuntu 10.04 server:  

Install the PPTP client:  

> sudo apt-get install pptp-linux

Add a line like this to <span style="font-family: &quot;Courier New&quot;,Courier,monospace;">/etc/ppp/chap-secrets</span> file:  

> **YOUR_USERNAME** PPTP **YOUR_PASSWORD** *

Create a file <span style="font-family: &quot;Courier New&quot;,Courier,monospace;">/etc/ppp/peers/NAME_OF_YOUR_CONNECTION</span>:  

> <span style="font-family: &quot;Courier New&quot;,Courier,monospace;">pty "pptp **VPN_SERVER_URL** --nolaunchpppd"</span>  
> <span style="font-family: &quot;Courier New&quot;,Courier,monospace;">name **YOUR_USERNAME**</span>  
> <span style="font-family: &quot;Courier New&quot;,Courier,monospace;">remotename PPTP</span>  
> <span style="font-family: &quot;Courier New&quot;,Courier,monospace;">require-mppe-128</span>  
> <span style="font-family: &quot;Courier New&quot;,Courier,monospace;">file /etc/ppp/options.pptp</span>  
> <span style="font-family: &quot;Courier New&quot;,Courier,monospace;">ipparam **NAME_OF_YOUR_CONNECTION**</span>

Create a script <span style="font-family: &quot;Courier New&quot;,Courier,monospace;">/etc/ppp/ip-up.d/route-traffic</span>:  

> <span style="font-family: &quot;Courier New&quot;,Courier,monospace;">#!/bin/bash</span>  
> <span style="font-family: &quot;Courier New&quot;,Courier,monospace;">route add -net 0.0.0.0 dev ppp0</span>

 Make the script executable:  

> <span style="font-family: &quot;Courier New&quot;,Courier,monospace;">sudo chmod +x /etc/ppp/ip-up.d/route-traffic</span>

 Establish the connection:  

> <span style="font-family: &quot;Courier New&quot;,Courier,monospace;">sudo /usr/sbin/pppd call **NAME_OF_YOUR_CONNECTION**</span>

 That should be it. Check <span style="font-family: &quot;Courier New&quot;,Courier,monospace;">/var/log/messages</span> to see if everything went OK, and if you have <span style="font-family: &quot;Courier New&quot;,Courier,monospace;">curl</span> installed, run this to see if you got your new external IP:  

> <span style="font-family: &quot;Courier New&quot;,Courier,monospace;">curl ifconfig.me</span>